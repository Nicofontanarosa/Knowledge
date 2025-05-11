# SNMP 📡

#SNMP  #ASN #DER 

> [!IMPORTANT]
> 
> ***Simple Network Management Protocol***
>
>  <p align="center"><img src="img/Screenshot 2025-03-07 201616.png" /></p>
>  
>  C'era la necessità di gestire dispositivi di rete come switch, router, ma anche sensori, apparecchiature mediche, ecc., per ottenere informazioni sul loro stato o per ricevere dati da essi. Il protocollo è rimasto costante nel tempo, mentre i dati gestiti si sono evoluti ( *ASN tree* ). Gli obiettivi di SNMP sono =>
>  
>  - Minimizzare la complessità delle funzioni di gestione implementate dagli agent
>  - Utilizzare le stesse funzioni su tutti i dispositivi di rete
>  - Permettere l’aggiunta di nuove funzionalità senza dover riscrivere tutto l’agent
>
> Tutto questo è stato reso possibile attraverso la definizione di una **MIB** ( *Management Information Base* ): **una collezione di oggetti** che possono essere interrogati per ottenere risposte ( _Ad esempio, non ha senso chiedere a una stampante quanti caffè ha fatto poiché quel dato non esiste nel suo MIB_ )
> 
> <mark>**Robustness by a simple, connectionless transport service ( #UDP )**</mark>. *Perché UDP?*
> 
> <p align="center"><img src="img/Screenshot 2025-03-08 090315.png" /></p>
> 
> L'interazione tra **Agent** e **Manager** avviene nel seguente modo -> il **Manager** invia richieste all'Agent quando necessario, ma soprattutto in modo periodico ( **Polling** ), e l’**Agent** risponde con i dati richiesti. Tuttavia, l'Agent può anche inviare messaggi spontanei al Manager quando rileva un cambiamento di stato significativo -> questi messaggi sono chiamati **Traps**
> 
> Questa comunicazione avviene tramite **UDP**, perché è veloce e adatto a scambi frequenti. Utilizzare **TCP** sarebbe meno efficiente a causa del tempo richiesto per l'**handshake**, e inoltre, se un Agent dovesse andare offline, TCP manterrebbe la connessione "viva" per ore tramite il meccanismo di **Keep-Alive**, rendendo il sistema poco reattivo e più pesante
> 
> <mark>**SNMP is a strictly centralized model, where the manager implements the whole functionality and responsibility**</mark>
> 
> Per ridurre la complessità dell'ASN.1 ( *Abstract Syntax Notation One* ), nella versione utilizzata da SNMP è stata **semplificata la sintassi**, limitandola a pochi **tipi di dato predefiniti**
> 
> <p align="center"><img src="img/Screenshot 2025-03-08 163857.png" /></p>
>

> [!TIP]
> 
> ***Structure of Management Information SMI v.2***
> 
> #SMIv2 o #SMI è una versione ridotta e semplificata di **ASN.1**. ( _SMIv2 is based on an extended subset of ASN.1 – 1998_ )
> 
> In SMIv2 esistono solo **tipi di variabili primitivi**, e le variabili possono essere **Scalari** ( *valori singoli* ) o **Colonne** ( *parte di tabelle, dette anche* **tabular objects** )
> 
> Le uniche operazioni consentite sono **lettura** e **scrittura**, rendendo il modello semplice, ma sufficiente per la maggior parte delle esigenze di gestione della rete
> 
> <p align="center"><img src="img/Screenshot 2025-03-08 101425.png" /></p>
> 
>  Dove è fondamentale distinguere tra **Gauge** e **Counter** =>
>  
>  - Il **Gauge** è un intero che rappresenta valori che **possono salire o scendere**, come ad esempio la temperatura, l’utilizzo della CPU o della memoria. Può avere picchi e fluttuazioni
>  - Il **Counter**, invece, è un valore che **aumenta solo** nel tempo. Viene utilizzato per misurare eventi cumulativi, come il numero di pacchetti trasmessi su un’interfaccia. Il problema è che, essendo un intero con dimensione fissa, **quando raggiunge il valore massimo, si resetta** a zero: questo fenomeno si chiama <mark>**wrap-around**</mark>
>
> Per gestire questo comportamento ci sono due soluzioni comuni =>
> 
> 1. **Utilizzare contatori a 64 bit**, che richiedono molto più tempo per arrivare al massimo e quindi riducono la frequenza del wrap
> 2. **Monitorare intervalli di byte** anziché il valore assoluto, in modo da comprendere l’andamento nel tempo
>  
>  <p align="center"><img src="img/Screenshot 2025-03-08 102201.png" /></p>
>  
>  Ogni oggetto è identificato dall' **Object identifier** nell'albero di registrazione
>  
>  <p align="center"><img src="img/Screenshot 2025-03-08 114132.png" /></p>
>  
>  > *I nomi dei nodi sono rilevanti solo per l'uomo*
>
> <p align="center"><img src="img/Screenshot 2025-03-08 114320.png" /></p>
> 
> Gli **oggetti scalari** hanno **una sola istanza**, identificata aggiungendo **`.0`** alla fine dell’**Object Identifier ( OID )**. Per gli **oggetti tabellari**, invece, l’istanza viene determinata leggendo la **tabella** tramite **indici** =>
> 
> > si specifica prima l’**OID della colonna** e poi l’**indice ( o gli indici ) della riga**
> > Esempio: l’elemento al centro della tabella ha OID `1.3.1.2.9`.
> 
> Questo approccio, però, **non è molto efficiente**, poiché l’indice viene **ripetuto all’interno dell’OID** per ogni elemento, aumentando la lunghezza e la ridondanza dell’identificativo. Inoltre, **gli indici non sono necessariamente interi**: ad esempio, nei router possono essere utilizzati **indirizzi IP** come indice
> 
> <p align="center"><img src="img/Screenshot 2025-03-08 120427.png" /></p>
> 
> È come se la tabella, che normalmente leggeremmo in modalità **riga/colonna**, venisse letta **invertita**: prima la **colonna**, poi la **riga** ( *cioè l’indice* ). Questo è **l’unico caso** in cui, in **SNMP SMIv2**, viene utilizzato il tipo **`SEQUENCE OF`**. La **tabella SNMP** è vista come un oggetto composto da =>
> 
> 3. Un oggetto "container" che rappresenta **la tabella**
> 4. La **riga** definita da una struttura (`SEQUENCE`) di colonne
> 5. La tabella nel suo insieme è definita come una **`SEQUENCE OF` righe** gli indici
> 
> ```
> IfEntry ::= SEQUENCE {
>    ifIndex     INTEGER,
>    ifDescr     OCTET STRING,
>    ifType      INTEGER
> }   
> IfTable ::= SEQUENCE OF IfEntry
> 
> ```
>  
> <p align="center"><img src="img/Screenshot 2025-03-08 115428.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-03-08 115957.png" /></p>
> 
> **L'approccio a colonne** utilizzato nei MIB è simile a quello adottato oggi nei **database colonnari**, perché consente di **comprimere più facilmente i dati** dato che tutti i valori di una stessa colonna ( *che hanno lo stesso tipo* ) possono essere gestiti e compressi in modo efficiente rispetto ad una struttura a righe dove i dati sono spesso eterogenei
> 
> Tutti questi oggetti sono scritti e descritti in un <mark>**modulo MIB**</mark>. Questo modulo MIB è formato da un nome, una definizione eventuale di altri moduli MIB, un **Module Identifier** e un **Object Identifier**
> 
> <p align="center"><img src="img/Screenshot 2025-03-08 122455.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-03-08 122610.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-03-08 122723.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-03-08 122637.png" /></p>
> 
> > *Dove il contatto è colui che ha scritto l'RFC, non l'inventore*
> 
> <p align="center"><img src="img/Screenshot 2025-03-08 122846.png" /></p>
> 
> ...
> 
> Un altro esempio sono le **notifiche ( trap )** inviate quando la **porta di un dispositivo cambia stato**. Se accendo un dispositivo con molte porte attive, riceverò **molti pacchetti trap in poco tempo**: questa situazione è nota come <mark>**_Trap Storm_**</mark>
> 
> In SNMP esiste anche il concetto di **_Textual Conventions_**, usato per **definire il formato e la rappresentazione dei dati**, in modo coerente tra i vari MIB
> 

> [!TIP]
> 
> ***MIB Compiler***
> 
>  <p align="center"><img src="img/Screenshot 2025-03-08 161940.png" /></p>
>  
>  Il **Backend Compiler** produce =>
>  
>  - la **documentazione**
>  - una **parte del codice sorgente** per l’**Agent**
>  - dei **test-case** per testare sia il **Manager** che l’**Agent**
>  - l’**input per l’applicazione di management**
>
> >( _Il MIB deve essere definito e disponibile **a run-time**_ )
> 
> Questo tipo di compilatore **non è semplicemente** uno strumento che traduce codice in linguaggio macchina, ma un componente fondamentale per **generare strutture, controlli e supporto operativo** legati alla gestione SNMP
>  
>  **MIB-II** definisce una serie di oggetti standard ( circa **170** ) relativi ai protocolli **IP, ICMP, UDP, TCP** e **SNMP**. L’implementazione di questo MIB **non dovrebbe interferire** con il normale funzionamento delle attività di rete
>  
>  <p align="center"><img src="img/Screenshot 2025-03-08 163224.png" /></p>
>  <p align="center"><img src="img/Screenshot 2025-03-08 163348.png" /></p>
>  
>  Un **Agent** può rendere disponibili gli stessi oggetti visti da **prospettive diverse**, perché esistono **diversi MIB** che osservano le stesse informazioni a **livelli differenti** del modello di rete. Ad esempio, le **statistiche di rete** possono essere presenti in **tre MIB diversi** =>
>  
>  - A livello **repeater**, trovi statistiche su quante volte è stato amplificato il segnale
>  - A livello **bridge**, i dati riguardano il **livello 2 del modello ISO/OSI**, come il numero di **MAC address** osservati ...
>  - ...
>  

> [!IMPORTANT]
> 
> ***SNMPv1***
> 
> In SNMPv1 le istanze del MIB sono ordinate in ordine lessicografico
> 
> <p align="center"><img src="img/Screenshot 2025-03-08 170745.png" /></p>
> 
> Il formato del messaggio SNMP è il seguente =>
> 
> <p align="center"><img src="img/Screenshot 2025-03-08 181014.png" /></p>
> 
> - **Community** -> E' un modo di autenticazione. Deve combaciare tra l'Agent e il Manager. Se ci si prova ad autenticare ad una certa comunità ma si sbaglia password arriva una notifica di **authentication fail**. Le comunity di default sono le seguenti ->
> 
> <p align="center"><img src="img/Screenshot 2025-03-14 175914.png" /></p>
> 
> - La risposta ai vari comandi ( **getrequest, next e set** ) , ad eccezione della trap, è sempre una **GetResponse** che specifica le chiavi e i valori. Questa risposta unica per + messaggi è una scelta per pura semplicità implementativa
> 
> La porta in cui l'**Agent** lavora è la <mark>**161**</mark> mentre quella del **Manager** la <mark>**162**</mark>. Le operazioni possibili in SNMPv1 sono 4 =>
> 
> <p align="center"><img src="img/Screenshot 2025-03-08 174201.png" /></p>
> 
> 1. **Get** significa leggo esattamente una variabile specificata. Posso ottenere errori del tipo **noSuchName** ovvero l'istanza richiesta non esiste o non appartiene all'albero, **ToBig** significa che non entra nel pacchetto UDP e **genErr** errore generale. In caso di + errori viene segnalato solo uno come **error-index**
> 
> <p align="center"><img src="img/Screenshot 2025-03-08 174847.png" /></p>
> 
> 2. **GetNext** significa dammi l'object identifier successivo a quello richiesto in ordine lessicografico, infatti questo comando serve per scorrere l'albero. Gli errori possibili sono gli stessi però noSuchName occorre quando l'albero finisce ( **end of MIB** ) 
> 
> <p align="center"><img src="img/Screenshot 2025-03-08 175718.png" /></p>
> 
> 3. **Set** scrive valori in 1 o + istanze del MIB, è un operazione atomica. Gli errori sono gli stessi e in più c'è **badValue** ovvero ho sbagliato il tipo del valore inserito ( *esiste anche l'errore **readOnly** ma non è usato spesso* )
> 
> <p align="center"><img src="img/Screenshot 2025-03-08 180022.png" /></p>
> 
> 4. **Trap** manda le notifiche sul cambio di stato. Non viene richiesta dal manager ma viene mandata dall'agent. Essendo inviata solo dall'agent, se viene persa, non viene richiesta dal manager ( *quella particolare trap* ) però il manager fa il pulling ( *siamo salvi* ). Per non mischiare il traffico il manager è in ascolto sulla <mark>***port 162***</mark>
> 
> <p align="center"><img src="img/Screenshot 2025-03-08 180446.png" /></p> 
> 
> Le **trap definite** sono 5 =>
> 
> 5. **coldStart** -> Il dispositivo si è avviato
> 6. **warmStart** -> Il dispositivo si è riavviato
> 7. **linkDown** -> Un oggetto sta entrando in Down state ( *1 per porta* )
> 8. **linkUp** -> Un oggetto sta andando in Up stsate
> 9. **authenticationFailure** -> Un soggetto prova ad accedere ma fallisce ( **Bad Comunity** )
> 

> [!IMPORTANT]
> 
> ***SNMPWalk and Others for SNMPv1***
> 
>  SNMP walk is <mark>**an application that runs multiple GETNEXT requests automatically**</mark>. The SNMP walk command allows users to extract useful information without entering the unique commands for each OID or node. SNMP walk simplifies the extraction of information from MIB as it is issued to the root-node of the sub-tree
>  
>  Le opzioni di SNMPwalk sono le seguenti =>
>  
>  <p align="center"><img src="img/Screenshot 2025-03-14 200050.png" /></p>
>  
> > *SNMPv3 per adesso censurato perché non ci interessa*
> 
> <p align="center"><img src="img/Screenshot 2025-03-14 200352.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-03-14 200541.png" /></p>
> 
> Per semplicità digitiamo il seguente comando =>
> 
> ***snmpwalk -v 1 -c public 46.37.227.66*** che ci genererà il seguente traffico ( *stoppato* )
> 
> <p align="center"><img src="img/Screenshot 2025-03-14 200711.png" /></p>
> 
> Che analizzato su #whireshark esce in questo modo
> 
> <p align="center"><img src="img/Screenshot 2025-03-14 200906.png" /></p>
> 
> Analizziamo ora i pacchetti di **Getnext** e **GetResponse** =>
> 
> <p align="center"><img src="img/Screenshot 2025-03-14 201134.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-03-14 201218.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-03-14 201708.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-03-14 201741.png" /></p>
> 
> I campi che vediamo sono **version, comunity, id, ErrorStatus e ErrorIndex** e i più importanti sono l'<mark>**object name e l'object value**</mark>. Tutto il processo di SNMPWalk si basa sull'agent che richiede il next object id di uno di default dato alla partenza, e il server risponde. SNPMWalk visualizza la risposta e genera la getNext dell'oggetto che ha ricevuto
> 
> Dopo l'<mark>***indirizzo IP dell'agent***</mark> posso mettere gli object identifier che voglio chiedere ( 1 ... n se parliamo di **get** ) opppure l'object identifier da cui partire con la snmpwalk ( parliamo di una serie di **getnext** )
> 
> <p align="center"><img src="img/Screenshot 2025-03-14 202523.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-03-14 202621.png" /></p> 
> 
> Nella ***set*** devo specificare invece il tipo ed il valore del dato =>
> 
> <p align="center"><img src="img/Screenshot 2025-03-14 202930.png" /></p>
>  

> [!NOTE]
> 
> ***MIB Implementation***
> 
>  <p align="center"><img src="img/Screenshot 2025-03-11 132741.png" /></p>
>  
>  È stato definito il **MIB-II**, che introduce oggetti per la gestione dei protocolli Internet come **IP, ICMP, UDP, TCP**, ecc., aggiungendo circa **170 nuovi oggetti**. Molti di questi oggetti sono stati inclusi per comodità, ad esempio la **routing table** e la **interface table**.
>  
>  Il **MIB-II** utilizza indirizzi a **4 byte** per rappresentare gli indirizzi **IPv4**; per **IPv6**, invece, si utilizza **SNMPv2** o versioni successive
>  
>  Gli obiettivi principali del MIB-II erano =>
>  
>  1. Definire la **gestione di base degli errori** e della **configurazione** per i protocolli Internet
>  2. Introdurre **pochissimi oggetti di controllo**, mantenendo la semplicità
>  3. **Evitare ridondanze** nelle informazioni
>  4. Non includere **oggetti dipendenti dall'implementazione** specifica di un dispositivo
>
> ---
> 
> L’accesso ai dati tramite SNMP deve **avvenire senza impattare il normale funzionamento del nodo**, quindi ad esempio le richieste alle tabelle del router **non devono rallentare il traffico** o influire sulle sue prestazioni
> 
> In particolare, l’introduzione della **interface table** ha permesso di recuperare facilmente informazioni come =>
> 
> > “Quanti pacchetti stanno passando su una certa interfaccia?”
> > Prima del MIB-II, questo tipo di informazione veniva fornita solo tramite **trap periodiche**, che rischiavano di **saturare il canale**
> 
> Un insieme di oggetti correlati all'interno dell’albero SNMP è chiamato **gruppo**. Esempi di questi gruppi sono =>
> 
> <p align="center"><img src="img/Screenshot 2025-03-14 203310.png" /></p>
> 
> Sono le informazioni relative all'agent tra cui ***sysUpTime*** ci dice da quanto è attivo l'agent ( *software* ) su quella macchina ( *hardware* )
> 
> If sysUpTime.0 t1 > sysUpTime.0 t2 where t2 > t1 then l'Agent è stato riavviato
> 
> Questo valore è molto utile per capire quando i campi dei dati misurati sono validi. Esempio ->
> 
> > Misuro il sysUpTime 1 ed è 4, dopo 1 secondo lo rimisuro ed è 5 => l'agent non è stato riavviato quindi se leggo anche il numero di pacchetti che sono entrati nell'interfaccia allora i 2 valori presi dopo 1 secondo sono coerenti. Se l'agent viene riavviato invece come faccio a capire se il numero di pacchetti è giusto, perchè tipo ha subito un wrap, o meno? tramite il **sysUpTime** 2 che sarà < del sysUpTime 1 => <mark>***Counter wrapping check***</mark>
> 
> - ***sysObjectId.0*** has the format enterprises.< manufacturer >.< id > and it is used to identify manufacturer and model. For instance enterprises.9.1.208 identifies a Cisco (.9) 2600 router (.1.208)
> 
> <p align="center"><img src="img/Screenshot 2025-03-14 224640.png" /></p>
> 
> Altro esempio di un gruppo è =>
>
> <p align="center"><img src="img/Screenshot 2025-03-21 181516.png" /></p>
> 
> Mi da informazioni sulle interfacce di rete ( *schede hardware non virtuali* ) 
> 
> <p align="center"><img src="img/Screenshot 2025-03-21 182039.png" /></p>
> 
> `ifNumber` è un Object Identifier ( *OID* ) definito all'interno della **MIB-II ( Management Information Base version 2 )**. Il suo scopo è quello di indicare il numero totale di interfacce presenti su un dispositivo di rete
> 
> <p align="center"><img src="img/Screenshot 2025-03-21 183453.png" /></p>
> 
> #MTU **Maximum Trasmission Unit** e **speed** ovvero velocità scheda di rete
> 
> **If Admin Status** può assumere 3 valori => **Up(1), Down(2), Testing(3)** è ci indica lo stato di un interfaccia fisicamente. Se l'interfaccia è down significa che non è presente fisicamente li. **ifOperStatus** può assumere valori simili  ad admin status ma riguarda lo stato operativo dell'interfaccia
> 
> > *A value different from up means that the interface is not physically present on the system or that it’s present but unavailable to the operating system* 
> 
> **ifLastChange** contiene il **sysUpTime** in cui quell'interfaccia ha cambiato stato per l'ultima volta
> 
> <mark>***ifInOctets / out***</mark> indica quanti pacchetti entrano ed escono dall'interfaccia. ***ifInUcastPkts / out*** sono i pacchetti ad un indirizzo specifico in entrata mentre ***ifInNUcastPkts / out*** sono i pacchetti ad un indirizzo o multicast o broadcast
> 
> **ifOutQLen** indica la lunghezza della coda dei pacchetti in output
> > *It is useful for knowing more about transmission speeds and throughput*
> 
> 1) **The number of packets delivered by a network interface to the next higher protocol layer: ifInUcastPkts + ifInNUcastPkts**
> 2) **The number of packets received by the network: (ifInUcastPkts + ifInNUcastPkts) + ifInDiscards + ifInUnknownProtos +ifInErrors**
> 3) **The number of actually transmitted packets: (ifOutUcastPkts + ifOutNUcastPkts) - ifOutErrors - ifOutDiscards**
> 
> Questi sono puri e semplici contatori che non fanno distinzione per il protocollo destinazione. Questi dati quindi possono essere usati per verificare che la rete funzioni a dovere, ma non chi ci passa dentro
> > *Interface errors can be used for detecting communication problems, especially on WAN links*
> 
> ---
> 
> Altra interfaccia è quella ***ARP*** ( *deprecata* ) o `ipNetToMediaTable` o `ipNetToPhysicalTable` che mostra la tabella ARP del dispositivo remoto ( *ARP and switch Forwarding tables are DIFFERENT concepts* )
> 
> <p align="center"><img src="img/Screenshot 2025-03-21 194036.png" /></p>
> 
> *Quando si dovrebbe popolare la tabella ARP?* un router genera un pacchetto ARP quando non sa dove mandare un pacchetto per poi generare un associazione #IP = #MAC . Esempio tipico è quello di un host che vuole comunicare fuori dalla rete. *In uno switch che funziona a livello IP, quale sarà l'ARP table?* dato che lo switch non genera nulla di ARP, non ha bisogno di cambiare indirizzo MAC, la sua tabella sarà vuota. *E allora perché questo switch ha la tabella ARP con un valore?* Si può facilmente verificare che il MAC di quella tabella non è di un interfaccia SNMP di quello switch => Probabilmente l'amministratore di rete avrà collegato la porta di management ad internet per configurare il dispositivo da remoto e quindi l'unico valore che vediamo è il **MAC = IP** del router precedente collegato a questo 
> 
> Infatti anche noi possiamo accedere al pannello di configurazione dello switch ip =>
> 
> <p align="center"><img src="img/Screenshot 2025-03-21 211901.png" /></p>
>

> [!NOTE]
> 
> ***Bridge MIB***
> 
> Utile per controllare lo stato degli **switch**. È in qualche modo complementare alla **MIB-II**, poiché fornisce informazioni sugli **host collegati** alle porte dello switch
> 
> <p align="center"><img src="img/Screenshot 2025-03-21 232550.png" /></p>
> 
> Quando scriviamo che il MAC fa parte dell'OID significa che il numero che leggiamo dopo il ..0.2. non è un indirizzo IP ma il MAC in decimale ( *brutta implementazione* )
> 
> <p align="center"><img src="img/Screenshot 2025-03-21 233053.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-03-21 233128.png" /></p>
> 
> - Permette di conoscere l’**indirizzo MAC** di un host collegato alla porta **X/unità Y** dello switch tramite la tabella **dot1dTpFdbTable.dot1dTpFdbAddress**
> - L’associazione **MAC/porta** è fondamentale per individuare la posizione fisica di un host ( <mark>***Forwarding Table***</mark> )
> - Tiene traccia del **MAC address precedente** ( e del tempo di connessione ) associato a una porta, permettendo di **tracciare gli utenti mentre si spostano da una stanza all’altra**
> - Può essere utilizzato per **rilevare porte con più MAC address associati** ( trunk ), identificando ->
> 	- **Utenti con più MAC address** ( *es. un utente usa VMware sul suo PC* )
> 	- **Host infetti da virus/worm** che generano traffico anomalo
> 	- **Porte con switch collegati**, violando la policy di rete
>

> [!TIP]
> 
> ***LLDP***
> 
> Un MIB particolare è chiamato LLDP. *Come faccio a sapere come sono realmente collegati i dispositivi allo switch?* Con il Bridge MIB scopriamo che una porta ha visto un determinato MAC. X conoscere la topologia della rete uso LLDP che mi consente di scoprire le adiacenze dello switch che supporta SNMP e questo MIB inviando **periodicamente** pacchetti #LLDP in multicast  
> 
> <p align="center"><img src="img/Screenshot 2025-03-22 084039.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-03-22 085026.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-03-22 085050.png" /></p>
> 
> Il time sarebbe il timestamp in cui si è ricevuto il pacchetto LLDP quindi potrei avere più righe per porta con tempo diverso
> 
> <p align="center"><img src="img/Screenshot 2025-03-22 085709.png" /></p>
> 
> **Ricordiamo che tutti questi dati sono presenti nei CLI Counters** ( *if config* )
> 
> <p align="center"><img src="img/Screenshot 2025-03-22 092008.png" /></p>
>

> [!NOTE]
> 
> ***Definition of NAC***
> 
>  Un #NAC o <mark>***Network Access Controll***</mark> è il processo che impedisce a utenti e dispositivi non autorizzati di accedere a una rete aziendale o privata. NAC garantisce che solo gli utenti autenticati e i dispositivi autorizzati e conformi ai criteri di sicurezza possano accedere alla rete
>  
>  <p align="center"><img src="img/Screenshot 2025-03-22 092908.png" /></p>
>  <p align="center"><img src="img/Screenshot 2025-03-22 092946.png" /></p>
>  

> [!IMPORTANT]
> 
> ***SNMPv2c***
> 
> Ci sono + versioni di SNMP versione 2 =>
> 
> 1. **SNMPv2p** -> Implementato con il party-based security model ( *storico* )
> 2. ==**SNMPv2c**== -> Implementato con la comunity
> 3. **SNMPv2u** -> Implementato con il user-based security model ( *storico* )
> 4. **SNMPv2\*** -> Implementato con il security and administration model ( *storico* )
> 
> ( *varie proposte ma venne accettata quella della comunity* )
> 
> <p align="center"><img src="img/Screenshot 2025-03-14 093719.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-03-14 093838.png" /></p>
> 
> Le novità di SNMPv2 sono l'aggiunta di 2 nuove primitive e il formato di messaggio della trap è cambiato =>
> 
> - **GetBulk** -> Migliora la velocità della get e getnext di SNMPv1. Con queste 2 primitive infatti bisogna richiedere ogni volta un elemento, aspettare la risposta e successivamente richiedere la get della risposta ( *velendo* ) -> Funzionamento della **snmpwalk**. Con la GetBulk vado a richiedere più elementi dell'albero consecutivi senza aspettare una risposta tra un elemento e l'altro
> 
> <p align="center"><img src="img/Screenshot 2025-03-22 123024.png" /></p>
> 
> Dove **nonrepeaters** sono quanti OID prendo con la get mentre **maxrepetition** sono quanti OID prendo con la get next
> 
> <p align="center"><img src="img/Screenshot 2025-03-22 123456.png" /></p>
> 
> Devo stare attento perché può essere che la risposta non entra nel pacchetto se aumento troppo il maxrepetition. Il problema è che con tante stringhe non riesco a stimare la lunghezza la lunghezza del pacchetto ( *compromesso* )
> 
> La GetBulk è simile alla GetNext quindi in ordine lessicografico
> 
> - **Inform** -> E' una specie di trap confermata dal manager. Nella pratica si è rivelata un disastro poiché un dispositivo che invia la inform e non riceve risposta si deve tenere una tabella per capire se sta ancora ricevendo risposta o meno ( *idea giusta ma dettagli difficili da implemetare e gestire* )
> 
> Sono cambiate anche alcuni errori / eccezioni =>
> 
> <p align="center"><img src="img/Screenshot 2025-03-22 121721.png" /></p>
> 
> Queste eccezioni permettono di restituire un errore specifico per la singola istanza che ha dato errore e non per l'intera operazone ( *SNMPv1* )
> 
> <p align="center"><img src="img/Screenshot 2025-03-22 122414.png" /></p>
> 
> Sono stati aggiunti i <mark>***Counter a 64 bit***</mark>
> 
> Altra novità è che SNMPv2 è stato modellato per funzionare su ogni protocollo di trasporto ma oggi viene usato ancora UDP
> 

> [!IMPORTANT]
> 
> ***SNMPv3***
> 
> Molto **più complesso** ma anche molto **più sicuro**
> 
> <p align="center"><img src="img/Screenshot 2025-03-22 133437.png" /></p>
> 
> La novità principale di SNMPv3 è l'aggiunta di un contesto ovvero una quantità di informazioni di gestione a cui un'**entità SNMP** può accedere
> 
> <p align="center"><img src="img/Screenshot 2025-03-22 160524.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-03-22 160607.png" /></p>
> 
> Come si evince dall'architettura, non ci basiamo + sulla community ma sull'user. Questo modo di autenticazione risponde alle domande =>
> 
> <p align="center"><img src="img/Screenshot 2025-03-22 163258.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-03-22 163417.png" /></p>
> 
> Si usa il #MAC per autenticare il messaggio ovvero l'unione tra **Chiave utente e dati**. La funzione di hash e di cifratura è cambiata nel tempo =>
> 
> <p align="center"><img src="img/Screenshot 2025-03-22 163610.png" /></p>
> 
> Il problema adesso, oltre all'autenticazione del messaggio, è che se una attaccante conserva un pacchetto con permessi che ha intercettato nel passato, quando vuole può rinviare questi pacchetti
> 
> <p align="center"><img src="img/Screenshot 2025-03-22 164748.png" /></p>
> 
> Dove il Manger e l'Agent hanno il <mark>**Timestamp Sincronizzato**</mark>
> 
> <p align="center"><img src="img/Screenshot 2025-03-22 165827.png" /></p>
> 
> semplicemente cifro ogni blocco di dati
> 
> <p align="center"><img src="img/Screenshot 2025-03-22 185149.png" /></p>
> 
> Dove si definiscono anche i permessi di **read view, write view and notify view**
> 

> [!TIP]
> 
> ***Monolithic, Proxy & Extensible Agent***
> 
>  In un ***agente monolitico*** =>
>  - L'intero codice SNMP, incluse le **MIB supportate**, viene compilato **in un unico blocco**
>  - Non è possibile **aggiungere o rimuovere** moduli MIB senza **ricompilare** il software
>  - È più semplice ma **meno flessibile** rispetto a un **agente modulare**, che permette di caricare dinamicamente nuove MIB
>
> <p align="center"><img src="img/Screenshot 2025-03-22 200007.png" /></p>
> 
> Un Agent riceve varie richieste dai manager e le smista nei vari "**sotto-agent**"
>
> <p align="center"><img src="img/Screenshot 2025-03-22 200159.png" /></p>
> 
> SNMP Proxy agents permette di raggiungere agent snmp non raggiungibili direttamente ( *dietro firewall* ). Esiste un entità che non è un agent che però prende le richieste e le inoltra ai giusti agent. Il problema è che il proxy non genera errori perché non è effettivamente un agent
> 
> <p align="center"><img src="img/Screenshot 2025-03-22 200753.png" /></p>
> 
> I "**sotto-agent**" vengono avviati dopo che l'Agent principale è stato attivato. Questi comunicano i MIB che supportano e il "**master-agent**" si scrive al suo interno cosa supporta cosa e manda ai vari sotto agent le porzioni di richiesta che riescono a gestire i vari sotto-agent
> 
> <mark>***Agent-X***</mark> è un protocollo <mark>***TCP porta 705***</mark> e non è basato su ASN.1 coding. E' un nuovo standard per implementare un espansione degli agenti SNMP
>  

---