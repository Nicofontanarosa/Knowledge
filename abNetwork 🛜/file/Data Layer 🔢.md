# Data Layer 🔢

#data #TCP/IP #ISO/OSI #ISO #physical

> [!IMPORTANT]
> 
> ***Definition of Data Layer***
>
> Il livello di collegamento muove i datagrammi da un nodo al nodo adiacente su un singolo link di comunicazione. Ogni host e router diventa un **nodo** e i link sono collegamenti fisici **cablati** oppure **collegamenti wireless**. In questo livello non si parla più di **datagrammi**, ma di  <mark>**Frame**</mark> o <mark>**Trama**</mark> che incapsulano il datagramma ricevuto a livello di rete
> 
> <p align="center"><img src="img/Screenshot 2024-08-29 093715.png" /></p>
> 
> Un datagramma può essere gestito da diversi protocolli su collegamenti differenti ( *ethernet, wireless, fibra etc ...* ) e anche **i servizi erogati dai protocolli livello link possono non essere del tutto affidabili ( wired o ethernet )**
> 
> I collegamenti possono essere =>
> 
> 1. ***Point-to-Point*** -> Collegamento dedicato a due soli dispositivi
> 2. ***Broadcast*** -> Collegamento condiviso tra + dispositivi, infatti il canale diffonde il frame a tutti gli host collegati. Nei collegamenti **broadcast**, i dispositivi dovrebbero **filtrare** i frame destinati a loro tramite **indirizzi MAC**
> 
> Un frame è formato dal campo dati ( *che contiene il datagramma IP* ), intestazione o **header** ed eventuale trailer. Il <mark>***Framing***</mark> separa i vari messaggi durante la trasmissione da una sorgete ad una destinazione. Per identificare i nodi viene usato un indirizzo diverso da quello IP chiamato <mark>***Indirizzo MAC***</mark> che identifica la NIC, 6 byte espresso in 12 cifre esadecimali ( *1A-63-F9-BD-06-9B* ) dove i primi 24 bit sono assegnati dalla ***OUI Organization Unique Identifier*** mentre i rimanenti 24 sono gestite dalle aziende a livello locale
> 
> <p align="center"><img src="img/Screenshot 2024-08-30 112643.png" /></p>
> 
> Questo livello si occupa di =>
> 
> - ***Controllare il flusso*** -> Evita che un mittente troppo veloce travolga un destinatario più lento
> - ***Rilevazione degli errori*** -> Errori causati dall'attenuazione del segnale o da un rumore elettromagnetico
> - ***Correzione errori***
> 
> Proprio per questo il livello di collegamento è diviso in <mark>**due sottolivelli**</mark> =>
> 
> 1. **Data-Link Control** o **Logical Link Control** ( <mark>***LLC***</mark> )-> Regole per correggere gli errori
> 2. **Medium Access Control** -> Regole per accedere al mezzo trasmissivo
> 
> Questo layer è implementato da una **NIC Network Interface Card** e usa lo standard **IEEE 802.x** ( ad esempio **802.3 per Ethernet** o **802.11 per Wi-Fi** )
> 
> <p align="center"><img src="img/Screenshot 2024-08-29 095402.png" /></p>
> 
> - Quando si riceve un frame il destinatario controlla gli errori, edt, flow control etc ... e successivamente estrae il datagramma e lo passa al livello superiore
> - Quando si invia un frame il mittente lo incapsula in questo frame aggiungendo la checksum, rdt, flow control etc ...
> 
> <p align="center"><img src="img/Screenshot 2024-08-29 100502.png" /></p>
>

> [!NOTE]
> 
> ***Multiple Access***
> 
> In un **canale broadcast condiviso**, come avviene nelle reti Wi-Fi o Ethernet, **più nodi potrebbero tentare di trasmettere simultaneamente**, causando **collisioni** o **interferenze**
> 
> Per questo si definiscono dei ***protocolli di accesso multiplo*** che determinano il comportamento dei nodi che devono trasmettere sullo stesso canale. Idealmente vorremmo che se un solo nodo deve trasmettere usi tutto il canale, altrimenti + nodi usino equalmente il canale. Inoltre vogliamo che il canale non invii messaggi per la sincronizzazione poiché questo genererebbe del traffico inutile in + ( **decentralizzato** )
> 
> I protocolli principali sono =>
> 
> 1. ***A suddivisione del canale*** ovvero divido il canale in slot + piccoli ( **TDMA, FDMA, CDMA** )
> 2. ***Ad accesso random*** ovvero il canale rimane condiviso e si può trasmettere assieme generando però delle collisioni che poi andranno gestite ( **ALOHA, CSMA/CD, CSMA/CA** )
> 3. ***A rotazione*** ovvero i nodi si alternano per trasmettere --> <mark>**polling**</mark> Ovvero un nodo master invita i nodi slave a trasmettere a rotazione. Gli svantaggi di questo metodo sono **single point of failure**, **latenza** e **polling overhead**
> 
> <p align="center"><img src="img/Screenshot 2024-08-30 111909.png" /></p>
> 
> Un altro protocollo a rotazione è il **token passing** ovvero ogni nodo si passa un **token** che garantisce il permesso di inviare dati
> 
> <p align="center"><img src="img/Screenshot 2024-08-30 112052.png" /></p>
>
> Quando parliamo dei metodi a suddivisione del canale, questo può essere diviso in 2 modi differenti =>
>
> <mark>***TDMA Time Division Multiple Access***</mark> => Accesso al canale in "*intervalli di tempo*" ovvero **time slot**, ogni stazione ha a disposizione un time slot di lunghezza fissa ( *vecchie radio* ). Lo svantaggio di questo metodo e che gli slot non utilizzati sono sprecati
> 
> <p align="center"><img src="img/Screenshot 2024-08-30 104756.png" /></p>
> 
> <mark>***FDMA Frequency Division Multiple Access***</mark> => Spettro del canale diviso in bande di frequenza e ad ogni stazione viene assegnata una banda di frequenza ( *radio* ). Se una stazione **non sta trasmettendo**, la sua **banda rimane inutilizzata**, causando **spreco di risorse**, proprio come avviene in TDMA
> 
> <p align="center"><img src="img/Screenshot 2024-08-30 105101.png" /></p>
>

> [!TIP]
> 
> ***Slotted ALOHA & ALOHA Puro ( unslotted )***
> 
> **Slotted ALOHA** è uno dei primi protocolli per la comunicazione da parte di più nodi sullo stesso canale ed è un protocollo ad **accesso random**. Assumiamo che =>
> 
> - Dispongo di time slot
> - I nodi iniziano a trasmettere all'inizio dello slot temporale
> - I nodi sono sincronizzati
> - Se 2 o + nodi trasmettono ogni nodo rileverà la collisione
> 
> Questo protocollo funziona nel seguente modo => **Quando il nodo deve trasmettere un frame comincia a trasmetterlo ad uno slot temporale, se non ci sono collisioni avanza di slot per trasmettere altrimenti, se rileva collisioni, il nodo ritrasmette in base ad una probabilità, allo slot successivo**
>
> <p align="center"><img src="img/Screenshot 2024-08-30 105616.png" /></p>
> 
> *In questo esempio 3 nodi hanno trasmesso nello stesso slot allora rilevano tutti la collisione, lanciano i dadi, nessuno trasmette, rilanciano i dadi, è uscito che 1 e 2 possono ritrasmettere, rilevata collisione, rilanciano i dadi, solo 2 trasmette e quindi finisce il suo ciclo di vita, 1 e 3 continuano a lanciare i dadi ma niente, rilanciano i dadi, trasmettono assieme etc...*
> 
> Il <mark>**vantaggio**</mark> è che se il nodo riesce a trasmettere **usa tutto il canale** ma lo <mark>**svantaggio**</mark> è che se ci sono tante stazioni i nodi hanno una **bassa probabilità di trasmettere** con successo
> 
> In **ALOHA Unslotted** invece non c'è una sincronizzazione e quindi i nodi usano sempre tutto il canale e la probabilità di collisione coincide con la probabilità che un nodo trasmetta quando anche l'altro nodo sta trasmettendo
> 
> <p align="center"><img src="img/Screenshot 2024-08-30 110512.png" /></p>
> 

> [!TIP]
> 
> ***CSMA Carrier Sense Multiple Access / CD Collision Detection / CA Collission Avoidance***
> 
> Si basa sul concetto di <mark>***Ascoltare prima di parlare o Carrier Sensing***</mark>, ovvero se il canale è libero trasmetto altrimenti ritardo la trasmissione ( *non interrompo qualcuno che sta parlando* ). Ovviamente le collisioni possono ancora avvenire poiché a causa del **ritardo di propagazione** i due nodi non rilevano che entrambi vogliono spedire e quindi spediranno entrambi i frame --> La **distanza e il ritardo di propagazione** influiscono sulla probabilità di collisione
> 
> <mark>**CSMA / CD**</mark> è **facile** da implementare in **LAN cablate** ma **difficile** in **LAN wireless**
> 
> L'algoritmo è il seguente =>
> 
> - NIC riceve il datagramma e crea il frame
> - NIC inizia a trasmettere se non sente nulla, altrimenti aspetta fin quando il canale non si libera
> - Se la NIC riesce a trasmettere tutto il frame senza collisioni allora ha finito
> - Altrimenti abortisce inviando uno **jam signal**
> - Dopo aver abortito la NIC entra in uno stato di ***==binary exponential backoff==*** cioè dopo tot collisioni NIC sceglie un numero random K da 0 a 2^tot - 1 e aspetta K x 512 bit times e ritorna allo step 2. Questo significa che più collido più aspetto 
> 
> <p align="center"><img src="img/IEEE 802.11.jpg" /></p>
> 

> [!IMPORTANT]
> 
> ***Definition of ARP Address Resolution Protocol***
>
> Quando un adattatore ( *NIC* ) spedisce un frame, vi inserisce un indirizzo MAC della scheda di destinazione oppure del caso della LAN Broadcast l'adattatore lo spedisce a tutta la rete ( *fino al router* ). <mark>*Come faccio a capire quali sono gli altri indirizzi MAC della mia rete ?*</mark>
> 
> Per questo nasce il protocollo ***ARP***, per scoprire gli indirizzi MAC della rete. Le richieste ARP verranno fatte in broadcast. *Come funziona questo protocollo?*
>
> <p align="center"><img src="img/Screenshot 2024-08-30 113302.png" /></p>
> 
> Ogni nodo mantiene una tabella ARP ovvero la corrispondenza indirizzi IP - indirizzi MAC - TTL dove il tempo di vita indica fra quanto bisognerà eliminare la voce della tabella ( *tipico 20 minuti* )
> 
> <p align="center"><img src="img/Screenshot 2024-08-30 113758.png" /></p>
>
> ARP risolve gli indirizzi IP solo per i nodi della stessa LAN e quindi ho corrispondenza fra indirizzi IP e indirizzi MAC della mia stessa sottorete
> 
> <p align="center"><img src="img/Screenshot 2024-08-30 113908.png" /></p>
>  
>  ( *questo schema è fatto male perché mettendo 32 bit di indirizzi e vanno a capo ma è lo stesso campo* )
>  
>  <p align="center"><img src="img/Screenshot 2025-02-01 170650.png" /></p>
>   
> 1. **Hardware Type** -> Protocollo a livello collegamento ( *ethernet 1* )
> 2. **Protocol Type** -> Protocollo a livello superiori ( *IP* )
> 3. **Destination address** -> Vuoto nelle richieste
>
> ***Forwarding diretto*** ->
> <p align="center"><img src="img/Screenshot 2024-08-30 114059.png" /></p>
> <p align="center"><img src="img/Screenshot 2024-08-30 114130.png" /></p>
> 
> Questo comportamento di ARP è chiamato **plug & play** ovvero la tabella ARP di un nodo si crea automaticamente e non viene configurata dall'amministratore di sistema ( *a costo di traffico aggiunto* )
> 
> ***Forwarding indiretto*** ->
> Si passa da un router intermedio:
> <p align="center"><img src="img/Screenshot 2024-08-30 115907.png" /></p>
> 
> - A crea un datagramma IP con IP sorgente a e destinazione B
> - A crea un frame con indirizzo MAC destinazione quello di R
> 
> <p align="center"><img src="img/Screenshot 2024-08-30 120110.png" /></p>
> 
> - R riceve il datagramma, lo deincapsula e ricava l'indirizzo IP destinatario ovvero B
> - R inoltra il datagramma con IP sorgente A e destinazione B e usa ARP per ricavare il MAC di B
> 
> <p align="center"><img src="img/Screenshot 2024-08-30 120305.png" /></p>
> 
> - R inoltra il datagramma di risposta
> 
> Questo perché l'indirizzo **MAC è sempre quello del prossimo dispositivo che attraverso**
> 

> [!CAUTION]
> 
> ***ARP Poisoning / Spoofing***
> 
> ARP è stato progettato per essere il + possibile efficiente. Proprio per questo presenta delle vulnerabilità che possono essere sfruttate ad esempio **==Man in the middle==** o **==Denial of Service Dos==** che inviano pacchetti ARP malevoli che reindirizzano i pacchetti ad un altro host malevolo
> 

> [!TIP]
> 
> ***Ethernet Flow Control***
> 
> Se provassi a fare un attacco #dos ad una macchina, proverei a mandare dei pacchetti UDP poiché **non essendo connection-oriented**, non prevedono un meccanismo di risposta diretto come avviene con il protocollo TCP. La **Scheda di rete** però, a livello ethernet, genera dei pacchetti di <mark>***PAUSE***</mark> che permettono di arrestare la ricezione dei pacchetti da parte della scheda di rete. Sappiamo che =>
> 
> *Quando uno switch o un router riceve un pacchetto, generalmente opera in modalità store-and-forward, ciò significa che il dispositivo riceve l’intero pacchetto, lo memorizza temporaneamente e ne verifica l’integrità. Una volta verificato, il dispositivo non “mantiene” fisicamente il segnale ricevuto così com’è, ma lo riconverte e lo ritrasmette attraverso la sua interfaccia di uscita anche per adattarsi al segmento di rete che può utilizzare tecnologie o standard fisici differenti, oppure cambia il TTL etc...*
> 
> Proprio per questo, la NIC genera questi pacchetti di #PAUSE che valgono solo nel tratto da una #NIC ad un altra, per arrestare la ricezione dei messaggi per quella NIC. L'**indirizzo destinazione MAC è fisso a 01:80:C2:00:00:01** ovvero un indirizzo multicast riservato per questo tipo di pacchetto e che non viene inoltrato oltre il dominio locale
> 
> <p align="center"><img src="img/Screenshot 2025-02-01 113325.png" /></p>
>
> **Pause_time** => 16 bit che indicano gli slot di tempo per cui la NIC non deve più inoltrare pacchetti ma scartarli solamente ( *ad esempio se si riempiono i buffer di ricezione questo permette di farli svuotare* )
>
> **EtherType** => **0x8808** ( #MAC_Control_Frame )
>
> **Opcode** => 
> - 1 ( *0x0001* ) è per PAUSE
> - 2 ( *0x0002* ) **Priority-based Flow Control** ( #PFC ) è un' estensione del frame PAUSE per supportare il controllo di flusso **per classi di traffico**. Invece di bloccare completamente la trasmissione, permette di fermare solo alcuni tipi di traffico specifici
> - 3 ( *0x0003* ) - **Congestion Notification Message** ( #CNM ) serve per gestire la congestione della rete. Se un dispositivo rileva congestione su un link, può inviare un **CNM frame** a monte per segnalare il problema e adattare la trasmissione
> - 4 ( *0x0004* ) - **Ethernet Bandwidth Allocation** ( #EBA ) è un frame di controllo utilizzato per **allocare dinamicamente la larghezza di banda** tra più dispositivi
> - **Opcodes per Link Aggregation ( #LACP )** **Link Aggregation Control Protocol** utilizza pacchetti speciali per negoziare la formazione di link aggregati. Questi non usano direttamente MAC Control Frames ma sono importanti nel controllo della rete
> 
> Questi pacchetti quindi non arrivano al software dell' host e quindi non vengono mostrati in tool quali Wireshark. Per visualizzare dei dati che includo questi pacchetti si può usare il comando <mark>**ethtool -S "interfaccia"**</mark>
> 
> Nelle reti seriali vengono usati XON e XOFF, ovvero segnali di controllo utilizzati per **fermare** ( #XOFF ) e **riprendere** ( #XON ) la trasmissione dei dati tra due dispositivi
> 

> [!TIP]
> 
> ***Ethernet HandShake***
> 
> - <mark>**Fast Link Pulses ( #FLP )**</mark> => Invece di scambiarsi veri e propri “pacchetti” Ethernet nel senso tradizionale, l’auto-negotiation utilizza dei **brevi impulsi** chiamati Fast Link Pulses
> 
> FLP Non sono frame standard ma impulsi inviati in burst e non hanno la struttura tipica di un frame Ethernet; sono implementati a livello hardware e operano in modo trasparente per il livello di rete
> 
> Per visualizzare questi segnali usare il comando <mark>**ethtool "interfaccia"**</mark>
> 

---