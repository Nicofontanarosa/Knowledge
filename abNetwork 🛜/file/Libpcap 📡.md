#LibPcap #network #networks #sniffing #wireshark #tcpdump #WinPcap #snort #tshark 

> [!TIP]
> 
> ***Introduction of Packet Capture***
> 
> Da quando le workstation sono diventate interconnesse, gli amministratori di rete hanno sentito l’esigenza di “vedere” cosa scorre sui cavi. Abbiamo già visto, nel file _Packet Analysis📡_, come sia possibile catturare i pacchetti, ma _come funziona davvero questo meccanismo ?_
> 
> I sistemi operativi hanno sviluppato delle **API** per lo sniffing dei pacchetti. Tuttavia, poiché non esisteva un vero e proprio standard, ogni sistema operativo dovette inventare una propria API: [Ultrix Packet Filter di DEC](AA-PBM2A-TE_Ultrix_4.0_The_Packet_Filter_-_An_Efficient_Mechanism_for_User-Level_Network_Code_Jun1990.pdf), [Snoop di Solaris](AB_Snoop) e altre ancora
> 
> Questo ha portato a numerose complicazioni: le API più semplici copiavano tutti i pacchetti nello sniffer nello spazio utente, causando un'enorme quantità di lavoro inutile su sistemi molto attivi. Le API più complesse, invece, permettevano di filtrare i pacchetti prima di trasferirli allo spazio utente, ma spesso erano macchinose e lente
>  
> Tutto cambiò nel 1993, quando Steven McCanne e Van Jacobson pubblicarono un articolo che introduceva un metodo più efficiente per filtrare i pacchetti direttamente nel kernel. Lo chiamarono ["The BSD Packet Filter" (BPF)](BPF.pdf).
> 
> <p align="center"><img src="img/Screenshot 2025-02-13 180844.png" /></p>
>

> [!IMPORTANT]
> 
> ***What is BPF ?***
> 
> <mark>**BPF Berkeley Packet Filter**</mark> è un paradigma che veniva usato originariamente per filtrare i pacchetti
> 
> <p align="center"><img src="img/Screenshot 2025-02-11 165733.png" /></p>
> 
> Normalmente il traffico internet viaggia sulla pila a destra, passando per l'**ethernet device driver** che consente di far riconoscere la scheda di rete al sistema operativo
> 
> Sappiamo che ogni processo sulla macchina, vede quello che il SO vuole fargli vedere. Possiamo dire che quindi è chiuso nel suo ambiente. *Come faccio adesso a leggere dati che viaggiano sulla scheda di rete da un processo software in spazio utente e non in spazio kernel?* 
> 
> Prima si usavano **chiamate di sistema** però questo è un procedimento **sicuro ma lento**. Si utilizza un'applicazione che legge e scrive direttamente i pacchetti tramite #DMA quindi accedendo direttamente alla memoria senza passare dalla #CPU
>
> <p align="center"><img src="img/Screenshot 2025-02-11 171841.png" /></p>   
> <p align="center"><img src="img/Screenshot 2025-02-11 174853.png" /></p>
> 
> Questa memoria circolare viene creata nel kernel e al suo interno c'è la copia del pacchetto. Questa memoria è condivisa o **mappata** con lo spazio utente e in questo modo l'applicazione di monitoraggio potrà accedere a questi pacchetti. Per una maggiore velocità **non si usa sincronizzazione** con variabili poiché ho un solo produttore ( *kernel* ) e un solo consumatore ( *sniffer* ). In particolare questo buffer usa un indice di testa e un indice di coda che non devono mai essere allineati ma sempre differenti di una cella. In questo mod lo sniffer potrà leggere i pacchetti fino al penultimo inserito dal kernel ( *in realtà è l'ultimo e poi c'è una cella vuota* ) 
> 
> Questo buffer circolare è una possibilità di guadagnare tempo nel prendere i pacchetti. Infatti c'è la voce pacchetti kernel persi
> 
> Un'ottimizzazione possibile del buffer è la seguente: Se ho più thread / core che si dedicano all'applicazione sniffer, ci saranno + entità che proveranno ad accedere ad un unico buffer di destinazione ( *nic -> buffer condiviso -> buffer applicazione* ). Per ovviare a ciò, l'applicazione non utilizza più un buffer di destinazione ma n, ognuno gestito da un thread, e prima di questi c'è un #thread che li smista, ovvero li prende dal buffer condiviso e li sistema dei buffer gestiti dai vari core
> 
> Se volessimo ottimizzare ancora di più questa procedura, il thread che funziona da "smistatore" possiamo tranquillamente rimuoverlo se spostiamo i buffer gestiti dai vari thread a livello kernel. Quindi non mappo un buffer condiviso con lo spazio utente ma n buffer condivisi con i vari thread dello spazio utente e il kernel, ricevuti i pacchetti dalla NIC, li smista direttamente nei buffer condivisi con una politica del caso ( #round_robin  )
> 
> Tutte queste ottimizzazioni vanno bene poiché io ho un produttore e un consumatore ( *1:1* ). Cn 1:n queste ottimizzazioni già non funzionano più
> 
> Questa ottimizzazione è chiamata<mark>***Receive Side Scaling***</mark> ( #RSS ) ovvero distribuisce la ricezione dei pacchetti di rete tra i vari core della CPUs. Viene usato per risolvere il **bottleneck** che si forma andando a lavorare con + core su un unica struttura dati, riducendo la latenza della rete. Di default il kernel assegna un numero di buffer per sniffer quanti sono i core ( *mettere - buffer significa che + core lavoreranno su un unico buffer avendo problemi di sincronizzazione, metterne di + non è una soluzione così sbagliata invece poiché se un core è abbastanza veloce potrebbe lavorare su + buffer* )
> 
> <p align="center"><img src="img/Screenshot 2025-02-11 164123.png" /></p>
> 
> Il BPF driver contiene le applicazioni registrate per catturare pacchetti e per ogni applicazione mantiene una coda contenente i pacchetti filtrati per quell'applicazione. Quindi per sniffare i pacchetti consumo una parte di memoria 
> 
> Il filtraggio viene fatto a livello kernel e non a livello utente perché così non devo spostare i pacchetti nel buffer circolare inutilemente e non leggo pacchetti inutili
> 
 > Questo filtro definito dall'utente decide se un pacchetto deve essere accettato e quanti byte di ciascun pacchetto devono essere salvati. Per ogni filtro che accetta il pacchetto, BPF copia la quantità di dati richiesta nel buffer associato a tale filtro. Prima del filtro viene usato un puntatore ovvero il pacchetto dovrebbe essere filtrato "*sul posto*" ( *ad esempio, dove il motore DMA dell'interfaccia di rete lo ha inserito* ) piuttosto che copiato in un altro buffer del kernel prima di filtrarlo aggiornando un numero di reference per quel pacchetto. Dopo di che lo sniffer prende i pacchetti dal buffer condiviso e li mette nel suo personale
>
> Un filtro di pacchetti è semplicemente una funzione a valori booleani su un pacchetto
>
> Più il pacchetto è grande più la size di una cella del buffer circolare è grande
> 
> <p align="center"><img src="img/Screenshot 2025-02-13 185937.png" /></p>
> 
> Questo **BPF o eBPF** è una vera e propria macchina virtuale che accetta dei programmi ( *filtri* ) scritti in una sintassi appropiata ( *bytecode corretto* ) provenienti dallo spazio utente ( *tramite chiamate di sistema ma questo avviene solo una volta* )
> 
> <mark>***eXpress Data Path***</mark> ( #XDP ) è un framework che consente di eseguire l'elaborazione di pacchetti ad alta velocità all'interno di applicazioni BPF. Per consentire una risposta più rapida alle operazioni di rete, XDP esegue un programma BPF il prima possibile, di solito non appena un pacchetto viene ricevuto dall'interfaccia di rete
> 
> In questo [articolo](https://lwn.net/Articles/437981/) vi è un ottimizzazione della macchina virtuale che implementa eBPF però che funziona per CPU x86
> 

> [!IMPORTANT]
> 
> ***What is TcpDump ?***
>
>  Tcpdump è composto da tre parti logiche =>
>  
>  1. Parser di espressioni -> Tcpdump analizza prima un'espressione di filtro leggibile come `ip e udp e la porta 53`. Il risultato è un breve programma in uno speciale bytecode minimo, il bytecode BPF. Il modo più semplice per vedere il parser in azione è passare un flag `-d`, che produrrà un programma leggibile simile a un assembly ->
>
> <p align="center"><img src="img/Screenshot 2025-02-13 183429.png" /></p>
> 
> Questo programma si legge così ->
> - Caricare una mezza parola (2 byte) dal pacchetto all'offset 12
> - Controlla se il valore è 0x0800, altrimenti fallisci. In questo modo viene verificata la presenza del pacchetto IP su un frame Ethernet
> - ...
> 
> Qui è possibile trovare [la documentazione completa della sintassi dei filtri](https://www.kernel.org/doc/Documentation/networking/filter.txt)
> ed alcuni [esempi](https://unix.stackexchange.com/questions/699500/understanding-of-bpf)
> 
> <mark>**The biggest user of this construct ( BPF ) might be libpcap**</mark>. TcpDump quando viene eseguito passa attraverso il compilatore interno di questa libreria che genera una struttura che viene passata al kernel e usata come filtro. Con il flag  `-ddd` mostriamo cosa viene caricato all'interno di questa struttura
>
>  1. Il bytecode BPF è collegato all'interfaccia di rete #TAP
>  2. TcpDumpo stampa i pacchetti per cui il filtro è soddisfatto
>
> In sostanza, Tcpdump chiede al kernel di eseguire un programma BPF all'interno del contesto del kernel. Potrebbe sembrare rischioso, ma in realtà non lo è. Prima di eseguire il bytecode BPF, il kernel si assicura che sia sicuro ->
> 
> 1. Tutti i salti sono solo in avanti, il che garantisce che non ci siano loop nel programma BPF. Pertanto deve terminare
> 2. Tutte le istruzioni, in particolare le letture in memoria, sono valide e nel raggio d'azione
> 3. Il singolo programma BPF ha meno di 4096 istruzioni
>

> [!NOTE]
> 
> ***Usage of TcpDump***
> 
> ***Promisque*** ci consente di vedere effettivamente il traffico internet che non viene dalla nostra scheda di rete, poiché le schede di rete fanno il filtraggio del MAC address
> 
> `sudo tcpdump -i any`
> 
> <p align="center"><img src="img/Screenshot 2025-02-13 191834.png" /></p>
> 
> Qui è possibile trovare un ottima [guida](https://andreaskaris.github.io/blog/networking/bpf-and-tcpdump/)
> 

> [!TIP]
> 
> ***What is Libpcap ?***
>
>  La **libreria** libpcap <mark>**Packet capture library**</mark> è stata scritta come **parte di un programma** più ampio chiamato <mark>***TCPDump***</mark>. La libreria libpcap ha permesso agli sviluppatori di scrivere codice per ricevere pacchetti di livello di collegamento ( *livello 2 nel modello OSI* ) su diversi tipi di **sistemi operativi UNIX** senza doversi preoccupare delle differenze sulle varie schede di rete e dei driver dei diversi sistemi operativi. In sostanza, la libreria libpcap **cattura i pacchetti direttamente dalle schede di rete**, il che ha permesso agli sviluppatori di scrivere programmi per decodificare, visualizzare o registrare i pacchetti
>  
>  Per maggiori informazioni attenersi all'API [libpcap di alto livello](http://www.tcpdump.org/manpages/pcap.3pcap.html)
>
> Questa libreria è usata frequentemente negli strumenti di sicurezza di rete per una varietà di scopi, inclusi scanner di rete e software di monitoraggio di rete. Mentre molte piattaforme UNIX sono fornite di default con _libpcap_ , **la piattaforma Windows** non lo è ma è fornita di un driver di pacchetti <mark>**WinPcap**</mark>  
>  
>  Il programma **TCPDump** ha fatto proprio questo. **Uno sniffer multipiattaforma**, originariamente scritto da #Van_Jacobson, #Craig_Leres e #Steven_McCanne presso i #Lawrence #Berkeley Labs per **analizzare i problemi di prestazioni TCP**, TCPDump ha permesso di catturare pacchetti e quindi decodificarli e visualizzarli. Un giorno, frustrato dalle limitazioni e dai formati di output di TCPDump, #Marty_Roesch ha scritto <mark>**Snort**</mark> come sostituto di TCPDump ( *era semplicemente un TCPDump migliore* )
>

> [!IMPORTANT]
> 
> ***How Wireshark Work?***
> 
> Sulla maggior parte delle moderne piattaforme UNIX è disponibile libpcap. E' installato di default su BSD e macOS, e potrebbe essere installato di default anche sulle distribuzioni Linux
> 
> Sono disponibili due versioni Windows di libpcap. Il più vecchio si chiama [WinPcap](https://wiki.wireshark.org/WinPcap); Non viene più mantenuto attivamente ed è basato su una versione precedente di libpcap. Il più recente si chiama [Npcap](https://nmap.org/npcap/); è attivamente mantenuto ed è basato su una versione relativamente recente di libpcap ed è disponibile solo per Windows 7 e versioni successive di Windows
> 
> La versione di Wireshark su terminal è chiamata <mark>**tshark**</mark>
> 

---