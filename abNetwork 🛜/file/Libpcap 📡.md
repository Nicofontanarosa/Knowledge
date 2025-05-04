# Libpcap 📡

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
> Tutto cambiò nel 1993, quando Steven #McCanne e Van #Jacobson pubblicarono un articolo che introduceva un metodo più efficiente per filtrare i pacchetti direttamente nel kernel. Lo chiamarono ["The BSD Packet Filter" (BPF)](BPF.pdf).
> 
> <p align="center"><img src="img/Screenshot 2025-02-13 180844.png" /></p>
>

> [!IMPORTANT]
> 
> ***What is BPF ?***
> 
> <mark>**BPF - Berkeley Packet Filter**</mark> è un paradigma originariamente sviluppato per filtrare i pacchetti di rete a livello kernel
> 
> <p align="center"><img src="img/Screenshot 2025-02-11 165733.png" /></p>
> 
> Normalmente, il traffico Internet segue lo stack a destra, passando attraverso il **driver della scheda di rete ( Ethernet device driver )**, che permette al sistema operativo di interagire con l'hardware di rete.
> 
> Sappiamo che ogni processo sulla macchina vede solo ciò che il sistema operativo gli consente di vedere. Possiamo quindi affermare che ogni processo è "chiuso" nel proprio ambiente. **Ma come può un processo in spazio utente leggere i dati che viaggiano sulla scheda di rete, senza accedere direttamente allo spazio kernel ?**
> 
> In passato si usavano **chiamate di sistema**, che però, pur essendo sicure, risultavano lente. Una soluzione più efficiente è usare applicazioni che accedono direttamente alla memoria tramite #DMA ( _Direct Memory Access_ ), permettendo di leggere/scrivere pacchetti bypassando la #CPU e riducendo la latenza
>
> <p align="center"><img src="img/Screenshot 2025-02-11 171841.png" /></p>   
> <p align="center"><img src="img/Screenshot 2025-02-11 174853.png" /></p>
> 
> Questa **memoria circolare** viene creata nel **kernel** e contiene la copia dei pacchetti catturati. Essa è **condivisa** ( o mappata ) con lo **spazio utente**, permettendo così all'applicazione di monitoraggio di accedervi direttamente
> 
> Per migliorare la velocità di accesso, **non viene usata sincronizzazione con variabili di controllo**, poiché esiste **un solo produttore** ( kernel ) e **un solo consumatore** ( sniffer ). Il buffer è gestito con un **indice di testa** ( _write index_ ) e un **indice di coda** ( *read index* ) che **non devono mai coincidere**: devono sempre essere distanziati almeno di una cella. Questo consente allo sniffer di leggere in sicurezza tutti i pacchetti **fino al penultimo inserito**, lasciando sempre una cella vuota per distinguere il buffer pieno da quello vuoto
> 
> Questo tipo di **buffer circolare** consente un accesso molto efficiente ai pacchetti, ma in situazioni di traffico intenso può comunque verificarsi la perdita di pacchetti ( _kernel packet drops_ )
> 
> Se l'applicazione sniffer è **multi-thread**, si presentano nuove sfide -> **più thread** proveranno ad accedere a **un unico buffer** condiviso, causando **contenzione** ( *nic -> buffer condiviso -> buffer applicazione* ). Una prima ottimizzazione consiste nell'introdurre **n buffer separati**, ognuno gestito da un #thread diverso. Un **thread smistatore** si occupa di leggere dal buffer condiviso ( kernel-user ) e distribuire i pacchetti nei buffer locali dei thread sniffer
> 
> Per migliorare ulteriormente, è possibile **eliminare il thread smistatore** spostando la logica di smistamento **nel kernel stesso**. In questo caso, il kernel **mappa direttamente n buffer**, uno per ogni thread dell'applicazione utente. Così, il kernel smista i pacchetti ricevuti dalla NIC **direttamente nei buffer corrispondenti** usando una politica ( #round_robin )
> 
> Queste ottimizzazioni funzionano bene in scenari **1:1 ( un produttore e un consumatore )**. In configurazioni **1:n** diventano più complesse da gestire
> 
> Questa tecnica prende il nome di <mark>**Receive Side Scaling ( RSS )**</mark> ->
> 
> #RSS distribuisce la ricezione dei pacchetti **tra i vari core** della CPU per evitare il **collo di bottiglia** derivante dall'accesso simultaneo a un'unica struttura dati. Di default, il kernel può assegnare **un buffer per ogni core**, ma si può anche configurare un numero maggiore. Infatti, avere **più buffer rispetto ai core** può essere vantaggioso ( *mettere - buffer significa che + core lavoreranno su un unico buffer avendo problemi di sincronizzazione, metterne di + non è una soluzione così sbagliata invece poiché se un core è abbastanza veloce potrebbe lavorare su + buffer* )
> 
> <p align="center"><img src="img/Screenshot 2025-02-11 164123.png" /></p>
> 
> ---
> 
> A livello concettuale, troviamo il **driver BPF** mantiene l’elenco delle **applicazioni registrate** per catturare pacchetti e, per ognuna, gestisce una **coda dedicata** contenente i pacchetti filtrati. Questo significa che sniffare i pacchetti **consuma memoria**, poiché ogni applicazione ha il proprio spazio di buffering
> 
> Il filtraggio viene eseguito **a livello kernel**, anziché a livello utente, per **evitare spostamenti inutili** di pacchetti nel buffer circolare e **ridurre il numero di pacchetti processati inutilmente**
> 
> Il **filtro definito dall’utente** determina ->
> 
> - **Se un pacchetto deve essere accettato**
> - **Quanti byte del pacchetto devono essere salvati**
>
> Per ogni filtro che accetta il pacchetto, **BPF copia nel buffer associato** solo la quantità di dati richiesta. Prima dell’applicazione del filtro, viene utilizzato un **puntatore**: il pacchetto viene **filtrato "sul posto"** ( **direttamente nella memoria dove il motore DMA della scheda di rete lo ha scritto** ), evitando di copiarlo in un altro buffer prima di applicare il filtro. Il numero di **reference** del pacchetto viene aggiornato per gestire l’accesso ai dati
> 
> Successivamente, lo **sniffer** legge i pacchetti dal **buffer condiviso** e li trasferisce nel proprio **buffer locale** per l’elaborazione
> 
> Un **filtro di pacchetti** è semplicemente una **funzione booleana** applicata a ogni pacchetto
> 
> Più il pacchetto è grande, **maggiore sarà la dimensione della cella** nel buffer circolare.
> 
> <p align="center"><img src="img/Screenshot 2025-02-13 185937.png" /></p>
> 
> Il **BPF** ( e la sua evoluzione **eBPF** ) è una vera e propria **macchina virtuale** che accetta **programmi ( filtri )** scritti in uno specifico **bytecode**, provenienti dallo **spazio utente**. Questi programmi vengono caricati nel kernel tramite una **chiamata di sistema** – che avviene **una sola volta** – e da quel momento in poi vengono eseguiti direttamente nel kernel, senza ulteriori interventi dallo spazio utente.
> 
> <mark>_**eXpress Data Path ( XDP )**_</mark> è un framework che consente l'elaborazione dei pacchetti **ad altissima velocità**, integrando programmi BPF direttamente **all'ingresso dell'interfaccia di rete**. In pratica, un programma BPF può essere eseguito **appena il pacchetto viene ricevuto**, _prima ancora che venga processato dallo stack TCP/IP del kernel_, permettendo così azioni immediate come il **drop**, il **redirect** o la **modifica del pacchetto**.
> 
> Un interessante approfondimento sull'ottimizzazione della macchina virtuale BPF per architetture **x86** è disponibile in [questo articolo](https://lwn.net/Articles/437981/)
> 

> [!IMPORTANT]
> 
> ***What is TcpDump ?***
>
> **Tcpdump** è uno strumento di analisi del traffico di rete basato su riga di comando, composto da tre parti logiche principali =>
>  
> 1. **Parser di espressioni** -> Tcpdump analizza prima un'espressione di filtro leggibile come `ip e udp e la porta 53`. Il risultato è un breve programma in uno speciale bytecode minimo, il bytecode BPF. Il modo più semplice per vedere il parser in azione è passare un flag `-d`, che produrrà un programma leggibile simile a un assembly ->
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
> In Wireshark, utilizzando il linguaggio Lua, esistono tre meccanismi principali per ispezionare ed elaborare i pacchetti =>
> 
> - **Dissector** -> Utilizzato quando si vuole modificare il modo in cui Wireshark mostra il pacchetto, analizzando il payload grezzo di un protocollo. Viene chiamato durante la fase di dissezione, ovvero appena Wireshark identifica il protocollo
> - **Post-dissector** -> Usato quando si vogliono analizzare i pacchetti dopo che tutti i dissector standard hanno terminato la loro analisi, per aggiungere informazioni extra o confrontare dati tra più protocolli. Non analizza direttamente il payload, ma legge i campi già estratti da altri dissector
> - **Tap** -> Utile per raccogliere statistiche, dati temporali o esportare flussi di pacchetti
> 
> Wireshark mostra dei **metadati** racchiusi tra parentesi **quadre \[\]**
> 

---