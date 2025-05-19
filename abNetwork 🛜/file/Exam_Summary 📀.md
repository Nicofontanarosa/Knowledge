# Internet paradigms 📡

> [!NOTE]
> 
> Oggi si parla di #MOS => Il The Mean Opinion Score ( **MOS** ) è la misura della qualità della voce di una comunicazione. Viene assegnato un punteggio da 1 a 5 da varie fonti e successivamente viene fatta una media
> 
> ---
> 
> **Protocollo Agent - Manager** ->
> 
> Tutte le funzioni di **controllo, coordinamento e gestione** vengono fatte su un *managed objects* che **è la rappresentazione astratta di una risorsa che ne presenta le sue proprietà**. Questi oggetti sono una rappresentazione astratta di una risorsa reale
>  
> <p align="center"><img src="img/Screenshot 2025-03-05 112419.png" /></p>
> 
> Questi oggetti presentano **attributi, operazioni, un comportamento ovvero l'interazione con il mondo reale e le notifiche ovvero messaggi generati in determinate situazioni dal managed object**. Il **Manager o Management System** non deve dipendere dall'implementazione fisica del managed object ( *macchinetta caffè* )
> 
> Il <mark>**MIB**</mark> ( **Management Information Base**  #MIB ) ovvero l'unione di tutti i managed object contenuti in un sistema. Questo MIB dovrebbe essere conosciuto sia dal manager che dal responsabile ovvero dall' **agent** è un software che gira all'interno della risorsa fisica e implementa i managed object del MIB accedendo a questa e attende un comando del **manager**, comunica delle notifiche ad un cambio di stato nel MIB e protegge i moduli da accessi esterni al manager tramite **Regole di accesso**
> 
> <p align="center"><img src="img/Screenshot 2025-03-05 132503.png" /></p>
> 
> > *spesso il manager e agent girano sullo stesso host*
>  
>  Il tutto è basato su un **protocollo di gestione** implementa l'accesso a managed object remoti codificando i dati di gestione, che vengono poi protetti durante il trasferimento. Questo protocollo di management si basa su =>
>  
>  - **Fault management** -> Identificazione degli errori e riparazione
>  - **Configuration management** -> Configurazione secondo standard amministrativi
>  - **Account management** -> Inserimento dei dati di consumo, monitoraggio e fatturazione ai clienti
>  - **Performance management** -> Bisogna capire se si sta andando in peggio oppure in meglio e quindi investire 
>  - **Security management** -> Autenticazione e segnalazione di attività illecite
>
> <mark>***FCAPS***</mark> ( #FCAPS ) è il termine con il quale si riassumono queste 5 proprietà o **aree funzionali** che non sono indipendenti tra di loro ma si influenzano a vicenda
> 
> *Ma come fanno effettivamente a comunicarsi i dati ?* Cioè 2 router comunicano tra di loro mediante indirizzo ip, oppure una scheda wifi comunica con un'altra scheda wifi, non con una ethernet poiché i dati non sono compatibili
> 
> Diciamo che le interfacce attraverso le quali si può accedere alle primitive di servizio ( *request, indication, response e confirm* ) sono chiamate **punti di accesso al servizio** ( **Service Access Points** #SAP )
> 
> <p align="center"><img src="img/Screenshot 2025-03-05 154232.png" /></p>
> 
> Mentre i dati che noi comunichiamo tramite queste interfacce usano una sintassi chiamata <mark>**ASN Abstract Syntax Notation**</mark> ( #ASN )

> [!NOTE]
> 
> ***Apache Avron vs Protobuf vs Json vs ASN***
> 
> - Apache Avro™, Google Protocol Buffers ( **Protocol Buffers**, o **Protobuf** ) e JSON è un **formato di serializzazione dei dati**. Serve quindi per rappresentare e scambiare dati strutturati in modo efficiente
> 
> - ASN.1 ( Abstract Syntax Notation One ) è un **linguaggio di descrizione dei dati** standardizzato usato in telecomunicazioni, crittografia e nelle reti
>

> [!TIP]
> 
> ***Abstract Syntax and Transfer Syntax***
> 
> **ASN.1** consente diverse regole di codifica che trasformano la sintassi astratta in un flusso di byte adatto al trasferimento. Definiamo quindi la <mark>**BER**</mark> ( _Basic Encoding Rules_ #BER ) definisce la mappatura tra la sintassi astratta e quella di trasferimento quindi codifica i tipi di dato ASN in stream di byte ( *nei browser è* #DER )
> 
> - BER definisce anche la direzione di trasmissione del flusso di bit oltre alla codifica dei tipi di dato ASN.1
> 
> <p align="center"><img src="img/Screenshot 2025-03-14 181844.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-03-07 183242.png" /></p>
> 
> *l'object identifier da un significato a degli oggetti: se voglio dire che una stringa è un nome di persona userò l'oi che dice che quella stringa rappresenta un nome*
> 
> <p align="center"><img src="img/Screenshot 2025-03-07 184203.png" /></p>
> 
> - Come possiamo vedere, qui definiamo una sintassi astratta per i tipi, non andiamo a specificare quanti bit occorrono poiché quello è il compito del compilatore ASN che trasformare la struttura dati astratta in una struttura dati del mio linguaggio
> 
> <p align="center"><img src="img/Screenshot 2025-03-07 195012.png" /></p>
> 
> L'<mark>**ISO Registration Tree**</mark> è un albero per identificare univocamente oggetti, tipi di dato, protocolli, documenti, ... insomma è un modo per standardizzare tutto
> 
> Il percorso dal nodo radice al nodo foglia è l'**Object identifier**. La struttura è un albero proprio perché con il tempo aggiungo livelli ai protocolli, certificati, standard etc .... che non devono cambiare tutta la sintassi ( *le curve ellittiche nei certificati prima non cerano* )
> 
> <p align="center"><img src="img/Screenshot 2025-03-07 183915.png" /></p>
> 
> Quindi possiamo dire che l'object identifier è un **riferimento assoluto** che la macchina deve essere in grado di risolvere. Un object identifier tipo 1.6.3.1.1.2.3 = WWW-HIR ( *non è vero* ) viene tradotto in una stringa comprensibile dall'uomo
> 

---
# SNMP 📡

> [!IMPORTANT]
> 
> ***Simple Network Management Protocol*** -> Protocollo centralizzato dove il manager implementa tutte le funzionalità e responsabilità
>
>  <p align="center"><img src="img/Screenshot 2025-03-07 201616.png" /></p>
>  
>  C'era la necessità di gestire dispositivi di rete come switch, router, ma anche sensori, apparecchiature mediche, etc... , per ottenere informazioni sul loro stato o per ricevere dati da essi
> 
> <mark>**Robustness by a simple, connectionless transport service ( #UDP )**</mark>. *Perché UDP?*
> 
> <p align="center"><img src="img/Screenshot 2025-03-08 090315.png" /></p>
> 
> L'interazione tra **Agent** e **Manager** avviene nel seguente modo -> il **Manager** invia richieste all'Agent quando necessario, ma soprattutto in modo periodico ( **Polling** ), e l’**Agent** risponde con i dati richiesti. Tuttavia, l'Agent può anche inviare messaggi spontanei al Manager quando rileva un cambiamento di stato significativo -> questi messaggi sono chiamati **Traps**
> 
> Questa comunicazione avviene tramite **UDP**, perché è veloce e adatto a scambi frequenti. Utilizzare **TCP** sarebbe meno efficiente a causa del tempo richiesto per l'**handshake**, e inoltre, se un Agent dovesse andare offline, TCP manterrebbe la connessione "viva" per ore tramite il meccanismo di **Keep-Alive**, rendendo il sistema poco reattivo e più pesante
> 
> Per ridurre la complessità dell'ASN.1 ( *Abstract Syntax Notation One* ), nella versione utilizzata da SNMP è stata **semplificata la sintassi**, limitandola a pochi **tipi di dato predefiniti**
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
> E' fondamentale distinguere tra tipo Gauge e Counter =>
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
> ```
> 
> **L'approccio a colonne** utilizzato nei MIB è simile a quello adottato oggi nei **database colonnari**, perché consente di **comprimere più facilmente i dati** dato che tutti i valori di una stessa colonna ( *che hanno lo stesso tipo* ) possono essere gestiti e compressi in modo efficiente rispetto ad una struttura a righe dove i dati sono spesso eterogenei
> 
> Tutti questi oggetti sono scritti e descritti in un <mark>**modulo MIB**</mark>. Questo modulo MIB è formato da un nome, una definizione eventuale di altri moduli MIB, un **Module Identifier** etc ...
> 
> <p align="center"><img src="img/Screenshot 2025-05-11 215953.png" /></p>
> 
> ...
> 
> Un altro esempio sono le **notifiche ( trap )** inviate quando la **porta di un dispositivo cambia stato**. Se accendo un dispositivo con molte porte attive, riceverò **molti pacchetti trap in poco tempo**: questa situazione è nota come <mark>**_Trap Storm_**</mark>
> 

> [!IMPORTANT]
> 
> ***SNMPv1***
> 
> In SNMPv1 le istanze del MIB sono ordinate in ordine lessicografico. Il formato del messaggio SNMP è il seguente =>
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
> 2. **GetNext** significa dammi l'object identifier successivo a quello richiesto in ordine lessicografico, infatti questo comando serve per scorrere l'albero. Gli errori possibili sono gli stessi però noSuchName occorre quando l'albero finisce ( **end of MIB** ) 
> 
> 3. **Set** scrive valori in 1 o + istanze del MIB, è un operazione atomica. Gli errori sono gli stessi e in più c'è **badValue** ovvero ho sbagliato il tipo del valore inserito ( *esiste anche l'errore **readOnly** ma non è usato spesso* )
>  
> 4. **Trap** manda le notifiche sul cambio di stato. Non viene richiesta dal manager ma viene mandata dall'agent. Essendo inviata solo dall'agent, se viene persa, non viene richiesta dal manager ( *quella particolare trap* ) però il manager fa il pulling ( *siamo salvi* ). Per non mischiare il traffico il manager è in ascolto sulla <mark>***port 161***</mark>
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
> ***snmpwalk -v 1 -c public 46.37.227.66*** che ci genererà il seguente traffico ( *stoppato* )
> 
> <p align="center"><img src="img/Screenshot 2025-03-14 200711.png" /></p>
> 
> Tutto il processo di SNMPWalk si basa sull'agent che richiede il next object id di uno di default dato alla partenza, e il server risponde. SNPMWalk visualizza la risposta e genera la getNext dell'oggetto che ha ricevuto
> 
> Dopo l'<mark>***indirizzo IP dell'agent***</mark> posso mettere gli object identifier che voglio chiedere ( 1 ... n se parliamo di **get** ) opppure l'object identifier da cui partire con la snmpwalk ( parliamo di una serie di **getnext** )
> 
> <p align="center"><img src="img/Screenshot 2025-03-14 202523.png" /></p>
> 
> Nella ***set*** devo specificare invece il tipo ed il valore del dato =>
> 
> <p align="center"><img src="img/Screenshot 2025-03-14 202930.png" /></p>
>  

> [!NOTE]
> 
> ***MIB Implementation***
> 
>  È stato definito il **MIB-II**, che introduce oggetti per la gestione dei protocolli Internet come **IP, ICMP, UDP, TCP**, ecc., aggiungendo circa **170 nuovi oggetti**. Molti di questi oggetti sono stati inclusi per comodità, ad esempio la **routing table** e la **interface table**
>  
>  <p align="center"><img src="img/Screenshot 2025-03-08 163224.png" /></p>
>  <p align="center"><img src="img/Screenshot 2025-03-08 163348.png" /></p>
>  
>  Un **Agent** può rendere disponibili gli stessi oggetti visti da **prospettive diverse**, perché esistono **diversi MIB** che osservano le stesse informazioni a **livelli differenti** del modello di rete. Ad esempio, le **statistiche di rete** possono essere presenti in **tre MIB diversi** =>
>  
>  - A livello **repeater**, trovi statistiche su quante volte è stato amplificato il segnale
>  - A livello **2 del modello ISO/OSI**, come il numero di **MAC address** osservati ...
>  - ...
>
> ---
>  
>  Il **MIB-II** utilizza indirizzi a **4 byte** per rappresentare gli indirizzi **IPv4**; per **IPv6**, invece, si utilizza **SNMPv2** o versioni successive
> 
> ---
> 
> L’accesso ai dati tramite SNMP deve **avvenire senza impattare il normale funzionamento del nodo**, quindi ad esempio le richieste alle tabelle del router **non devono rallentare il traffico** o influire sulle sue prestazioni
> 
> ---
> 
> Un insieme di oggetti correlati all'interno dell’albero SNMP ( _ISO Registration Tree_ ) è chiamato **gruppo**. Esempi di questi gruppi sono =>
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
> **If Admin Status** può assumere 3 valori => **Up(1), Down(2), Testing(3)** è ci indica lo stato di un interfaccia fisicamente. Se l'interfaccia è down significa che non è presente fisicamente li. **ifOperStatus** può assumere valori simili  ad admin status ma riguarda lo stato operativo dell'interfaccia
>  
> **ifLastChange** contiene il **sysUpTime** in cui quell'interfaccia ha cambiato stato per l'ultima volta
> 
> <mark>***ifInOctets / out***</mark> indica quanti pacchetti entrano ed escono dall'interfaccia. ***ifInUcastPkts / out*** sono i pacchetti ad un indirizzo specifico in entrata mentre ***ifInNUcastPkts / out*** sono i pacchetti ad un indirizzo o multicast o broadcast
> 
> **ifOutQLen** indica la lunghezza della coda dei pacchetti in output
> 
> 1) **The number of packets delivered by a network interface to the next higher protocol layer: ifInUcastPkts + ifInNUcastPkts**
> 2) **The number of packets received by the network: (ifInUcastPkts + ifInNUcastPkts) + ifInDiscards + ifInUnknownProtos +ifInErrors**
> 3) **The number of actually transmitted packets: (ifOutUcastPkts + ifOutNUcastPkts) - ifOutErrors - ifOutDiscards**
> 
> Questi sono puri e semplici contatori che non fanno distinzione per il protocollo destinazione. Questi dati quindi possono essere usati per verificare che la rete funzioni a dovere, ma non chi ci passa dentro
> 
> ---
> 
> Altra interfaccia è quella ***ARP*** ( *deprecata* ) o `ipNetToMediaTable` o `ipNetToPhysicalTable` che mostra la tabella ARP del dispositivo remoto ( *ARP and switch Forwarding tables are DIFFERENT concepts* )
> 
> <p align="center"><img src="img/Screenshot 2025-03-21 194036.png" /></p>
> 
> *Quando si dovrebbe popolare la tabella ARP?* un router genera un pacchetto ARP quando non sa dove mandare un pacchetto per poi generare un associazione #IP = #MAC . Esempio tipico è quello di un host che vuole comunicare fuori dalla rete. *In uno switch che funziona a livello IP, quale sarà l'ARP table?* dato che lo switch non genera nulla di ARP, non ha bisogno di cambiare indirizzo MAC, la sua tabella sarà vuota. *E allora perché questo switch ha la tabella ARP con un valore?* Si può facilmente verificare che il MAC di quella tabella non è di un interfaccia SNMP di quello switch => Probabilmente l'amministratore di rete avrà collegato la porta di management ad internet per configurare il dispositivo da remoto e quindi l'unico valore che vediamo è il **MAC = IP** del router precedente collegato a questo 
> 

> [!NOTE]
> 
> ***Bridge MIB***
> 
> Utile per controllare lo stato degli **switch**. È in qualche modo complementare alla **MIB-II**, poiché fornisce informazioni sugli **host collegati** alle porte dello switch
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
>

> [!NOTE]
> 
> ***Definition of NAC***
> 
>  Un #NAC o <mark>***Network Access Controll***</mark> è il processo che impedisce a utenti e dispositivi non autorizzati di accedere a una rete aziendale o privata. NAC garantisce che solo gli utenti autenticati e i dispositivi autorizzati e conformi ai criteri di sicurezza possano accedere alla rete
>  

> [!IMPORTANT]
> 
> ***SNMPv2c***
> 
> Ci sono + versioni di SNMP versione 2 ma quela che venneaccettata fu la ==**SNMPv2c**== -> Implementato con la comunity
> 
> <p align="center"><img src="img/Screenshot 2025-03-14 093719.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-03-14 093838.png" /></p>
> 
> Le novità di SNMPv2 sono l'aggiunta di 2 nuove primitive e il formato di messaggio della trap è cambiato =>
> 
> - **GetBulk** -> Migliora la velocità della get e getnext di SNMPv1. Con queste 2 primitive infatti bisogna richiedere ogni volta un elemento, aspettare la risposta e successivamente richiedere la get della risposta ( *velendo* ) -> Funzionamento della **snmpwalk**. Con la GetBulk vado a richiedere più elementi dell'albero consecutivi senza aspettare una risposta tra un elemento e l'altro
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
> Sono stati aggiunti i <mark>***Counter a 64 bit***</mark>
> 
> Altra novità è che SNMPv2 è stato modellato per funzionare su ogni protocollo di trasporto ma oggi viene usato ancora UDP
> 

> [!IMPORTANT]
> 
> ***SNMPv3***
> 
> Molto **più complesso** ma anche molto **più sicuro**. La novità principale di SNMPv3 è l'aggiunta di un contesto ovvero una quantità di informazioni di gestione a cui un'**entità SNMP** può accedere. Non ci basiamo + sulla community ma sull'user. Si usa il #MAC per autenticare il messaggio ovvero l'unione tra **Chiave utente e dati**. Oltre all'autenticazione del messaggio, se una attaccante conserva un pacchetto con permessi che ha intercettato nel passato, quando vuole può rinviare questi pacchetti => Il Manger e l'Agent hanno il <mark>**Timestamp Sincronizzato**</mark>
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
# Packet Analysis 📡

> [!NOTE]
> 
> ***What can i mesure?***
>
>  **Analisi quantitativa del traffico di rete** => Si concentra sulla **misurazione** e **classificazione** del traffico di rete sulla base di parametri rilevanti tipo Identificazione dei protocolli applicativi utilizzati o Top Talkers etc ...
> 
> **Analisi qualitativa del traffico di rete** => Si concentra sugli aspetti legati alla **sicurezza** e alla **funzionalità** del traffico di rete, con l'obiettivo di individuare anomalie o potenziali minacce tipo rilevamento dell'uso di tecnologie come Tor o VPN etc ...
> 

> [!TIP]
> 
> ***Traffic Mirror***
> 
> Per osservare il traffico di rete di un dispositivo posso =>
> 
> 1. **Collegarmi fisicamente alla macchina** ( *non sempre possibile* )
> 2. A livello hardware, utilizzare:
> 	- Un **hub** che inoltra tutto il traffico verso tutte le porte
> 	- Un **TAP** (Test Access Point) o un **optical splitter** che duplicano il traffico in modo passivo, permettendomi di osservarlo senza alterarlo
> 3. Utilizzare uno **switch con funzionalità di port mirroring**, configurando una porta di mirroraggio tramite tecnologie come **SPAN**, **RSPAN** o **ERSPAN**
> 
> Ovviamente, o la porta che esegue il mirror supporta una velocità maggiore, oppure devo stare attento a non superare la sua capacità. Infatti, normalmente una porta genera traffico in modalità **full duplex** (es. 1 Gbps in upload + 1 Gbps in download), mentre sulla porta mirror tutto il traffico viene **inviato** verso il sistema di analisi. Quindi, se non ho abbastanza banda disponibile sulla porta di destinazione (es. 2 Gbps in totale), rischio di perdere pacchetti. In pratica, il traffico viene visto **in modalità half duplex** (solo una direzione per volta nella pratica del mirroring).
> 
> <p align="center"><img src="img/Screenshot 2025-02-04 191831.png" /></p>
> 
> *In questo modo però rischio di perdere pacchetti durante il tragitto*
> 
> **ERSPAN** (_Encapsulated Remote SPAN_) invece estende il concetto di RSPAN, permettendo di inviare i pacchetti mirrorati anche attraverso dispositivi intermedi **non configurati per SPAN o RSPAN**, grazie all'incapsulamento tramite **GRE** ( _Generic Routing Encapsulation_ ) #GRE
>  
> Altre tecniche includono =>
> 
> 4. A livello software, usare una **VLAN di mirroring** oppure funzioni come **Switch Traffic Filter / Mirroring** ( *es. su dispositivi Juniper* )
> 5. Eseguire un attacco di **MAC Spoofing**, fingendosi un altro host. In questo modo posso intercettare il traffico **da fuori verso dentro** ( *non il traffico interno all' host* ), poiché lo switch inoltrerà i pacchetti destinati sia all' host legittimo sia a me
>   
> I <mark>**vantaggi**</mark> dei **tap** sono =>
> 
> - **Eliminazione del rischio di perdita pacchetti**
> - **Monitoring dei pacchetti**
> 
> Gli <mark>**svantaggi**</mark> dei tap sono =>
> 
> - **Ho bisogno di due interfacce per catturare il traffico**
> - **Costi aggiuntivi per i tappi**
> - **Non si può leggere il traffico prima dello switch** ( *host to host* )
> 

> [!IMPORTANT]
> 
> ***Flow Measurement***
> 
> Nella rete occorre analizzare il traffico ma non è possibile fare il **mirror** di tutto il traffico che si genera perché sarebbe troppo e dal punto di vista della rete, la riempie e la rallenta. Un altro problema è che il traffico è **Criptato** è quindi **difficilmente comprimibile**, diverso per file di testo, simile per le immagini che già per loro definizione / creazione, sono compresse e di qualità ridotta
> 
> Sono così nate delle **sonde** posizionate in rete chiamate <mark>**meter**</mark> che non inoltra il traffico ma fa un analisi che poi invia al manager ( **delega l'elaborazione dell'analisi** )
> 
> <p align="center"><img src="img/Screenshot 2025-03-29 154557.png" /></p>
> 
> SNMP implementa questo paradigma **manager-agent** e i contatori SNMP possono essere usati per il **monitoraggio del traffico**. Il problema è che devo specificare quali contatori vedere e analizzare e l'Agent deve avere delle soglie per informare il manager dell'avvenimento di un certo evento, che però per i dispositivi su internet sono inutili perché sarebbero una diversa dall'altra
> 
> Questo meccanismo di pooling è anche sbagliato quando ad esempio stabilisco una connessione per risolvere un nome DNS poiché la connessione dura pochi secondi e quindi il manager non fa in tempo a chiedere cosa è successo che la connessione è stata già chiusa e dimenticata
> 
> Quindi si è optato per non usare SNMP ma un **flusso di rete** ovvero un sistema di misura x per il traffico di rete ( *pensato per il router* ) e il singolo flusso di rete contiene informazioni ( **proto - ip_s - prt_s - ip_d - prt_d -> packets, bytes, time, AS, network mask, interfaces** ) analizzate dai meter che utilizzano **UDP** come protocollo di trasporto e le informazioni che misuro sono =>
> 
> - Con chi il tuo **operatore** scambia traffico, in base a **indirizzo IP**, **prefisso IP** o **ASN**
> - Il **tipo** e la **quantità** di **traffico** ( *SMTP, WEB, File Sharing, etc ...* )
> - Quali **servizi** girano su una rete
> - **Riepiloghi del traffico** a livello di ASN
> - Tracciare i **virus di rete** fino ai dispositivi host
> 
> Questi flussi possono essere <mark>**unidirezionali o bidirezionali**</mark>. 2 flussi unidirezionali corrispondono ad un flusso bidirezionale e i flussi bidirezionali possono contenere informazioni quali ***RTT***. Questo perché il flusso unidirezionale è composto da informazioni da sorgente a destinatario e un altro flusso unidirezionale sarà composto da informazioni da destinatario a sorgente. Il bidirezionale ha tutte le informazioni insieme
>  
> Il **limite** di questo approccio è che tutto il **traffico non ip** non riesco a vederlo poiché non passa dal router. Con SNMP invece riesco a capire a livello fisico da quale porta sta passando il traffico ( **congestion, delay, packet loss** ), e non posso misurare neanche **transaction latency, # positive/negative replies & protocol errors**
> 
> Un altro problema è che il meter può decidere di mandare i dati alla fine del flusso oppure durante il flusso. Mandarli alla fine è peggio ( *il meter si può interrompere e i dati non arriveranno mai all'operatore telefonico* ). Tutto questo determina un overhead maggiore poiché andrò ad inviare + dati
> 
> L'**architettura** usata **oggi** è la seguente =>
> 
> <p align="center"><img src="img/Screenshot 2025-03-29 191235.png" /></p>
> 
> Dove il probe funge da meter è vive all'interno di un router. Il protocollo di trasporto del flusso non è più SNMP ma <mark>***NetFlow***</mark> #netflow ( *standard sviluppato da #CISCO* )
> 
> <p align="center"><img src="img/Screenshot 2025-03-29 190841.png" /></p>
> 
> In questo grafico possiamo vedere come il flusso però attraversa più router quindi devo stare attento a non considerare + di una volta lo stesso flusso. Questo viene evitato tramite un blocco di analisi del traffico da parte del probe di una direzione ( *conta solo il traffico in entrata / uscita* )

> [!IMPORTANT]
> 
> ***Cisco NetFlow & IPFIX***
>
>  Protocollo standard sviluppato da CISCO per il **trasporto dei flussi** unidirezionali fino alla versione 8 e bidirezionali dalla 9. La versione NetFlow 9 trasporta i flussi **in maniera flessibile** poiché al contrario delle versioni precedenti, dove un pacchetto aveva un formato standard e fixed, qui nasce il concetto di **template**. Prima viene inviato questo template e poi arrivano i dati => Posso aggiungere tutti i campi che voglio aggiungendoli nel template che invio
>
> Dalla v.9 c'è un **supporto di IPv6**
> 
> Come per il concetto di **meter**, questo protocollo misura solo il traffico che attraversa il router e quindi dal livello 3 in su
> 
> nelle versioni precedenti alla 9 c'è un **flow sequence number** per tenere traccia del flusso e di quelli persi. Dalla 9 però, è stato rimosso questo numero e introdotto il **packet sequence number** che tiene traccia dei pacchetti persi e non dei flussi
> 
> Iniziamo con il dire che un flusso ha una **chiave** -> **quintupletta proto ip porta ip porta** e una parte di **valore** -> 
> 
> <p align="center"><img src="img/Screenshot 2025-04-13 130351.png" /></p>
> 
> > ( *le interfacce sono molto importanti soprattutto per i provider* )
> 
> I **contatori** sono gli stessi di **SNMP**, e le **interfacce** sono importanti poiché io malintenzionato potrei spacciarmi per un client interno alla rete e ad esempio fingermi il server DHCP e cambiare gli indirizzi agli host. Quindi sapere da quale interfaccia è arrivato un indirizzo IP è importante. Banalmente la rete interna avrà un indirizzo IP e dalle porte che escono non potranno entrare pacchetti con la stessa famiglia di indirizzi ( *pacchetto kamikaze* )
> 
> I **flussi attivi** vengono memorizzati in RAM nella cosiddetta **cache NetFlow**. Una volta comunicato il flusso, svuoto la cache. Il flusso termina quando dura troppo a lungo, oppure se contiene il flag FIN oppure è rimasto inattivo per troppo tempo. Una **Conseguenza importante** è che quindi un flusso può essere inviato al collector in maniera segmentata che quindi ha anche un compito di **riassemblatore**
> 
> <p align="center"><img src="img/Screenshot 2025-04-13 174619.png" /></p>
> 
> Per capire se il flusso è inattivo la **cache mantiene** un **contatore di active** ovvero da quanti secondi quel flusso è attivo e di **idle** quel flusso non ha fatto traffico. A livello hardware questa cosa è semplice ma a software sarebbe uno spreco di risorse poiché ogni secondo devo accedere alla tabella hash e aggiornare i valori del tempo:
> 
> 1) **Prima osservazione** -> Basterebbe segnarsi quando inizia il flusso e in questo modo il contatore active non serve più poiché quando invio il flusso faccio il timestamp attuale meno la variabile che ha segnato quando è iniziato il flusso
> 2) **Seconda osservazione** -> Posso fare tutto in questo modo. Mantengo il timestamp di quando inizia il flusso e il timestamp dell'ultimo update ( *poiché tanto devo accedere all'entry della tabella hash* ) e per calcolare l'**IDLE** all'estrazione faccio **now - last update** mentre l'**ACTIVE** = **now - start**
> 
> Per capire quali flussi stanno per scadere dovrei scansionare tutta l'hash table per calcolare l'idle e vedere se è molto grande. Per ottimizzare questo posso tenermi una lista dedicata all'idle e una dedicata all'active. Per vedere il flusso più vecchio vedo il primo che sta in lista poiché avrà l'active che calcolerò maggiore. Mentre per capire i flussi inattivi la lista deve aggiornarsi aggiungendo gli elementi uno sotto l'altro però un elemento che viene modificato presente in lista diventa l'ultimo
> 
> ---
> 
> <mark>***IPFIX***</mark> è un protocollo che implementa una tecnologia per l'IP Trafﬁc Flow measurement che fa le stesse cose di NetFlow v9 però non dipende da CISCO. Si basa sul protocollo di trasporto SCTP ( *non funziona su windows* ) che però non usa nessuno e infatti si può usare anche sopra il protocollo TPC/UDP
> 
> La differenze + grande è che non si manda un template ma un PEN prima di inviare i dati, ovvero un ***identificativo univoco assegnato da IANA*** ad un'organizzazione. Serve a definire **template personalizzati** nei protocolli come IPFIX
> 
> *Immagina che **Palo Alto Networks** voglia esportare un campo custom come `"app_id"` ( che rappresenta un'applicazione identificata da loro ). Con **IPFIX**, Palo Alto può usare il proprio PEN (es. `25461`) e definire quel campo in modo univoco, senza conflitti con altri vendor. NetFlow v9 invece non ha un meccanismo formale per includere questi campi proprietari in modo _portabile_ tra strumenti diversi*
> 

> [!NOTE]
> 
> ***NetFlow Packet Format***
> 
> <p align="center"><img src="img/Screenshot 2025-04-15 105519.png" /></p>
> 
> Per ogni record potevo inserire massimo 30 campi in v5
> 
> <p align="center"><img src="img/Screenshot 2025-04-15 105705.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-04-15 105804.png" /></p>
> 
> Questo modo di definire i flussi è **statico e non estendibile, anche se molto veloce** poiché occupa **poca banda**. Per questi motivi è stata definita la v9 che propone il concetto di template. Ogni X tempo, si invia il template e subito dopo i dati che quindi il collector riesce ad interpretare. Nel template posso decidere se mettere dei parametri o meno
>  
> <p align="center"><img src="img/Screenshot 2025-04-15 110856.png" /></p>
>

> [!CAUTION]
> 
> I **flussi grezzi** sono utili, ma a volte è necessario rispondere a varie domande, come ad esempio ->
> 
> - Quanta parte del nostro traffico è web, news, email, Quake (gioco) ?
> - Quanto traffico proviene o va verso i vari reparti ?
> - Quanto traffico va verso altri reparti, il provider X, Google, ecc. ?
> - Qual è la quantità di traffico che attraversa l’interfaccia X ?
> 
> Quindi quello che si fa è unire i flussi per risparmiare spazio e poter rispondere ad informazioni di questo tipo. Questa aggregazione può essere fatta dal probe, dal collector o da entrambi ( *il collector è + potente ma anche + costoso* )
> 
> Questi **flussi** possono essere anche **filtrati** tramite parametri quali **Flow duration, Flow src / dst, ports etc ...**. Il filtraggio è diverso dall'aggregazione anche se possono coesistere
>
> Combinando aggregazione e filtraggio è possibile identificare ->
> 
> - Portscan / portmap detection
> - Detect activities on suspicious ports
> - Identify sources of spam, unauthorised servers
> - Flow with to many packets in it ( *count* )
> - 1 ip contact more destination -> Host Scanning
> 

> [!IMPORTANT]
> 
> ***sFlow***
>
>  Netflow e IPFIX presentano dei problemi =>
>  
>  - **Costo** -> Il monitoraggio delle reti richiedeva l'uso di più sonde, le soluzioni di monitoraggio integrate richiedevano hardware e/o software aggiuntivo e i costi amministrativi sono elevati per la gestione di apparecchiature aggiuntive
>  - **Impatto sulle prestazioni della rete** -> Le prestazioni degli switch venivano compromesse dalla misurazione dei flussi di traffico e la comunicazione dei dati di flusso consumava una quantità eccessiva di banda di rete
>  - **Scarsa scalabilità del sistema di monitoraggio** -> Incapacità di tenere il passo con velocità dell’ordine del Gigabit e l'impossibilità di costruire una visione globale del traffico di rete in ambienti grandi e molto utilizzati
>
> Proprio per questo è nato sFlow che implementa come Netflow e IPFIX una tecnologia per monitorare i flussi di rete ma non pretende di essere veloce come loro perché tanto questi dati vengono persi. **Analizza un singolo pacchetto ogni X pacchetti** -> <mark>**packet sample**</mark>  ( *questi pacchetti vengono mandati dal probe al collezionatore* )
> 
> Si utilizza UDP e il pacchetto codificato in sFlow conterrà un <mark>**Counter Sample**</mark> ovvero i counter delle interfacce SNMP MIB-II => **SNMP versione push** anche perché il pacchetto non deve essere obbligatoriamente di livello 3 ma anche 2, cosa che Netflow non permette
> 
> <p align="center"><img src="img/Screenshot 2025-04-15 165222.png" /></p> 
> 
> L’architettura su cui si basa sFlow è composta da un probe che cattura il traffico, da un collector che riceve i pacchetti esportati ed un’applicazione che riceverà i report. Il probe sFlow opera secondo principi diversi da quelli del probe NetFlow, poiché prende un pacchetto da ciascuna porta dello switch su cui si trova in base al sampling rate che si sta applicando. Una volta presi i pacchetti, li incapsula e li invia al collector, quindi il probe non analizza il pacchetto ma si limita ad inviarlo al collector
> 

---
# Time Series 📡

> [!IMPORTANT]
> 
> ***Definition of Time Series***
>
>  - **Serie** -> Sequenza ordinata di numeri
>  - **Ordine** -> L'indice dei numeri della serie ( *serie ordinata per tempo -> indice è il tempo* )
>  - **Time Series** -> Serie ordinata per tempo
>  - **Osservazione** -> Il valore numerico osservato reale in uno specifico momento
>  - **Forecast** -> La previsione è la stima del valore atteso in un specifico tempo che noi però ancora non conosciamo
>  - **Forecast Error** -> L'errore di previsione, ovvero un valore che si discosta dall'effetivo misurato. Solitamente questo errore è mostrato al quadrato per non avere un errore negativo ma quindi un ***discostamento*** che per sua natura è positivo
>  - #SSE -> <mark>**Sum of Square Error**</mark> sarebbe la differenza al quadrato del valore misurato e valore predetto -> <mark>***Confrontare serie temporali***</mark>
>
> <p align="center"><img src="img/Screenshot 2025-03-27 130447.png" /></p>
> 
> La differenza tra le **serie univariate** e le **serie multivariate** è che nella prima riesco a prendere tutte le informazioni che mi servono mentre le seconde devo prima unirle in qualche modo e poi prendere i dati. Quindi nel primo ho un valore **Scalare**, nel secondo ho vari valori che devono essere messi assieme
> 
> <p align="center"><img src="img/Screenshot 2025-03-28 114710.png" /></p>
> 
> Le **serie temporali** possono essere ***stazionarie e non***. Le prime sono serie dove è stata fatta la differenza tra i valori e quindi sono state normalizzate è hanno un andamento che possiamo studiare. Quelle non stazionarie hanno il valore gouge o counter grezzo
> 
> <p align="center"><img src="img/Screenshot 2025-03-28 165645.png" /></p>
> 
> Guardando una serie stazionaria possiamo distinguere vari campi ovvero **la stagionalità, la tendenza e l'errore** ( *traffico dell'università di pisa -> season = 5/7g è forte mentre il weekend no, la tendency = i mesi di lezioni + forte delle vacanze, error = ci sono dei giorni di festa dove mi sbaglio nel predire un valore* )
> 
> Ci sono due metodi per predirre i valori nelle serie temporali -> **Additivi e Moltiplicativi**
> 
> <p align="center"><img src="img/Screenshot 2025-03-28 170046.png" /></p>
> 
> Gli additivi sono buoni se la stagione e la tendenza sono + o - costanti mentre i moltiplicativi tengono conto di un cambio di questi 2 parametri. I metodi principali per predire i valori futuri sono =>
> 
> - #ETS **Error, Trend and Seasonality** -> Algoritmi del tipo *Single Exponential Smoothing, Double Exponential Smoothing & Triple Exponential Smoothing*
> - #ARIMA **AutoRegressive Integrated Moving Average** -> Algoritmo che tiene presente alla regressione della variabile ( *tiene conto del recente passato quindi* ) e tiene conto anche degli **errori** per **adattarsi**
> - #Prophet ( #Meta ), #TimeGTP ( *AI-based* )
> 
> La previsione più facile è la **media** oppure la **media mobile** ovvero una media che tiene in considerazione i valori + recenti per calcolare la media e quindi considera l'andamento, oppure la **media pesata**
> 
> <p align="center"><img src="img/Screenshot 2025-03-28 174541.png" /></p>
>  
> - <mark>**Single Exponential Smoothing**</mark> -> Non è una vera e propria predizione ma una funzione di verifica con "a" un valore compreso tra 0 e 1 in base a se voglio dare più importanza al valore precedente o no
> 
> <p align="center"><img src="img/Screenshot 2025-03-28 174809.png" /></p>
> 
> > *α: smoothing factor or “memory decay rate”: the higher α, the faster the method forgets*
> 
> - <mark>**Double Exponential Smoothing**</mark> -> Introduco B ovvero il trend ovvero quanto sarà inclinata la mia funzione di previsione per il prossimo punto
> 
> - <mark>**Triple Exponential Smoothing**</mark> -> Introduco la stagionalità ovvero la ripetizione del segnale ad intervalli stabiliti quindi posso prevedere n punti a piacere. Ovviamente più vado lontano più aumenta l'errore perché mi baso solo sulla stagionalità
> 
> Un <mark>***Anomalia***</mark> è un valore che esce dalla mia previsione. Nella maggior parte del tempo però, picchi nei valori non sono anomalie ( *tutta la classe visita un web detto dal prof* ) -> Un sintomo solo non è sufficiente per alzare la bandierina di allarme. Definiamo quindi un **upper bound e lower bound**
> 
> - Un **problema** è che questi allarmi non possono essere fatti sui **byte grezzi** perché non avrebbe senso, se iniziassi a scaricare un file avrei un allarme scattato
> - Un altro **problema** è che questi algoritmi **imparano dalla storia** quindi se l'**anomalia** diventa la **normalità** possono dare non pochi problemi 
> 

> [!TIP]
> 
> ***How to RRD tool Database work***
> 
> Per lavorare sulle serie si usano dei database per immagazzinare i valori. Il problema dei database classici è che se volessi query del tipo: *Questi valori somigliano a quali valori?* dovrei prendere ogni singolo campo alla volta e confrontarlo con altri e vedere se ce una correlazione quindi nxm confronti. Quindi calcolare l'SSE per ogni campo
> 
> Un altro problema è la cardinalità che nel tempo cresce e le dimensioni diventano improponibili. Per riassumere e unire più valori si esegue un operazione chiamata di ***Roll-up*** dove si comprimono i dati tramite un qualche criterio ( *sappiamo il meteo nell'anno 1971 ma non dei singoli giorni o mesi* )
> 
> ***Round Robin Database*** è un database circolare per serie temporali che arrivato alla fine si sovrascrive. Questo database è su File, quindi gira in locale ( *tipo* #sqlite ). Il file RDD è composto dai seguenti campi
> 
> <p align="center"><img src="img/Screenshot 2025-03-27 162459.png" /></p>
> 
> dove negli archivi memorizzo una delle metriche che sto misurando
> 
> <p align="center"><img src="img/Screenshot 2025-03-27 163008.png" /></p>
> 
> Una volta creato il database, la grandezza è fixed, quindi alloco già lo spazio necessario per andare a riempire gli slot che si è calcolato con i dati misurati. *Cosa succede se non misuro in un certo istante di tempo ?* Definisco un tipo che è **UNKNOW** che ***NON E' 0***. Quando misuro i valori da scrivere potrei non tener conto del tempo di scrittura di una macchina, oppure se ne va la corrente etc ... Quindi non riesco a scrivere il tempo in tempo => Non scrivo il valore oppure scrivo una stima del valore ( *dipende* )
>
> <p align="center"><img src="img/Screenshot 2025-03-27 163451.png" /></p>
> 
> Dove il valore che posso scrivere notiamo che può scendere sia perché ha <mark>**wrappato**</mark> sia perché il dispositivo è stato **spento** oppure è stato resettato
> 
> I valori che vado a scrivere all'interno di questi database sono valori **normalizzati**, ovvero come prima cosa scrivo valori che sono la differenza del numero misurato con il precedente e poi per comprimerli posso fare una media oppure il massimo o il minimo ( *dipende da cosa voglio ottenere* )
> 
> <p align="center"><img src="img/Screenshot 2025-03-27 182713.png" /></p>
> 

> [!IMPORTANT]
> 
> ***How RRD Works***
>
>  <p align="center"><img src="img/Screenshot 2025-03-27 184559.png" /></p>
>  
> L'esempio in questione significa => Ogni 5 minuti ( *300 secondi* ) catturi un valore di tipo GAUGE. Attendi massimo 500 secondi altrimenti scrivi UNKONW. I valori massimi e minimi scrivibili sono 300 e 0. Di tutti questi valori che misuri memorizzali in un archivio dove fai la **media di un valore** cioè sostanzialmente copia se stesso, infatti era indifferente usare MAX o MIN, e memorizza 120 di questi valori => l'archivio diventa grande 120 x 5m = 10 ore. Da questo archivio creane un altro dove fai la media di 12 slot alla volta e mantieni 96 di queste medie => 12 x 5m ( *5 minuti perchè i valori memorizzati nell'archivio di sopra sono 1 ogni 5 minuti* ) = 1h e di questi nuovi valori memorizzane 96 => 96h complessive. 0.5 è la soglia dalla quale non considerare più i valori per i calcoli ( *se ho soglia 0.5 => 1 2 3 unknow = mi scrive 2, se ho 1 unknow unknow unknow = mi scrive unknow* )
> 
> <p align="center"><img src="img/Screenshot 2025-03-28 123620.png" /></p>
> 
> Questo comando ( _update_ ) serve per inserire i dati all'interno dove **N** è il tempo di quel momento preso però lo prende da locale. perché è dispendioso prendere il tempo dal dispositivo destinatario, occupa banda, scarica il dispositivo ... Nella **realtà** non sono io che ogni 5 minuti vado a prendere la misura ma vado a prendere ogni 1 ora le misure fatte ogni 5 minuti dall' host locale x quell'ora
> 
> L'inserimento in questo database funziona in questo modo =>
> 
> Quando inserisco i dati, l'ordine di inserimento è importante perché le differenze che mi portano ad eventuali buchi me le calcola in ordine e se faccio una differenza dove non ho un valore e dopo 1 ora mi arriva quel valore => non posso tornare indietro per ricalcolare la differenza. Esistono database che consentono di inserire i valori nell'ordine che voglio e poi quando estraggo i valori me li raggruppa per tempo e mi esegue le differenze
> 
> Il valore che si scrive poi, è un valore che viene scritto non intero come viene misurato ma spalmato nell'intervallo ( *banalmente se ho 5 secondi di intervallo => il valore non lo scrivo in 1 intero ma in più interi diventando a virgola mobile* ). Negli anni futuri si è dimostrato che se i numeri interi vengono convertiti in virgola mobile => riesco a comprimerli molto + efficientemente
> 

---
# Data Clustering 📡

> [!NOTE]
> 
> ***Note of Artificial Intelligence***
> 
> Il mondo dell’intelligenza artificiale è suddiviso in due principali famiglie =>
> 
> - **Supervisionato** -> In questo approccio, l’AI viene "addestrata" fornendo esempi con etichette corrette. Possiamo immaginarlo come un array di pesi in cui ad ogni input corrisponde un output atteso. Le aree più esplorate nell’ambito supervisionato sono **testo** e **immagini**
> - **Non supervisionato** -> Qui si utilizzano algoritmi di **clustering**, che raggruppano elementi simili tra loro per poi confrontare nuovi dati con questi gruppi ( *cluster* ). Questo approccio è spesso usato, ad esempio, per analizzare **immagini del traffico di rete** ( *non immagini grafiche, ma rappresentazioni di pacchetti o pattern di comunicazione* )
> 
> Esistono diversi tipi di algoritmi di clustering. Il + semplice è il <mark>**K-means**</mark> che suddivide gli elementi in _K_ cluster _( nDPI adatta questo algoritmo per produrre al massimo K cluster e non esattamente K )_ 
> 
> <p align="center"><img src="img/Screenshot 2025-04-16 151758.png" /></p>
> 
> Il <mark>**DBSCAN**</mark> ( **Density-Based Spatial Clustering of Applications with Noise** ) invece trova automaticamente **quanti cluster** ci sono, basandosi sulla **densità** dei punti. Più intelligente, ma **più complicato** da usare e regolare
> 
> **Catena di Markov**
> 
> È un **modello probabilistico** in cui la **prossima azione** ( *stato futuro* ) dipende **solo** dallo stato attuale, **non dalla storia completa**. In pratica -> Cosa succede dopo dipende solo da dove sono ora, non da come ci sono arrivato
>

---
# RADIUS 📡

> [!IMPORTANT]
> 
> ***Definition of RADIUS***
>
>  **RADIUS ( Remote Authentication Dial-In User Service )** → E' un protocollo di autenticazione centralizzato ed è il + utilizzato per i device di rete
>  
> 🔹 **Obiettivo** -> Gestire l’accesso sicuro alle reti ( *Ethernet, Wi-Fi, VPN, ecc* )
> 
> 🔹 **Funziona su UDP** ( *porta **1812** per autenticazione, **1813** per accounting* ) 
> 
> Gli attori principali sono -> **Client RADIUS**, **Server RADIUS** e **Database utenti**
> 
> <mark>**Come Funziona un'Autenticazione con RADIUS?**</mark>
> 
> 1️⃣ Un utente tenta di connettersi alla rete
> 
> 2️⃣ Lo switch/AP chiede l'autenticazione e inoltra la richiesta al **Server RADIUS**
> 
> 3️⃣ Il server RADIUS verifica le credenziali (password o certificato)
> 
> 4️⃣ Se l’utente è autorizzato → Il server RADIUS risponde con **Access-Accept**
> 
> 5️⃣ Se l’utente non è autorizzato → Il server RADIUS invia **Access-Reject**
> 
> Oltre alla sua implementazione tramite credenziali è utilizzato anche tramite cavo ( *used for billing activities on wired lines* ) cioè gli operatori di rete sanno che quel cavo arriva a casa di Pippo e quindi tassano il traffico a pippo
> 
> La versione per mobile è il protocollo DIAMETER
> 

> [!IMPORTANT]
> 
> ***Definition of EAP***
>
>  **EAP ( Extensible Authentication Protocol )** → Il protocollo utilizzato per scambiare le credenziali tra client e server
 > 
> 1️⃣ Il client si connette alla rete, lo switch/AP invia una richiesta **EAP-Request/Identity**
> 
> 2️⃣ Il client risponde con il proprio nome utente **EAP-Response/Identity**
> 
> 3️⃣ Lo switch/AP inoltra il tutto al **server RADIUS**
> 
> 4️⃣ Il server chiede ulteriori informazioni ( _password, certificati, token, etc..._ )
>   
> 5️⃣ Se l'autenticazione va a buon fine, il client riceve l’**EAP-Success** e può navigare
> 

> [!IMPORTANT]
> 
> ***Definition of PNAC***
>
>  **PNAC ( Port-Based Network Access Control ) – Controllo di Accesso Basato sulle Porte** è un meccanismo che controlla l'accesso alla rete **a livello di porta Ethernet o Wi-Fi**, assicurandosi che solo dispositivi autorizzati possano connettersi
>   
> - Quando colleghi un dispositivo a una porta di rete o a un Wi-Fi aziendale, la rete **non ti permette subito di comunicare**
> - **802.1X + RADIUS** verificano le tue credenziali prima di concederti l'accesso
> 

---
# Data Layer 🔢

> [!TIP]
> 
> ***Ethernet Flow Control***
> 
> Se provassi a fare un attacco #dos ad una macchina, proverei a mandare dei pacchetti UDP poiché **non essendo connection-oriented**, non prevedono un meccanismo di risposta diretto come avviene con il protocollo TCP. La **Scheda di rete** però, a livello ethernet, genera dei pacchetti di <mark>***PAUSE***</mark> che permettono di arrestare la ricezione dei pacchetti da parte della scheda di rete
> 
> La NIC genera questi pacchetti di #PAUSE che valgono solo nel tratto da una #NIC ad un altra, per arrestare la ricezione dei messaggi per quella NIC. L'**indirizzo destinazione MAC è fisso a 01:80:C2:00:00:01** ovvero un indirizzo multicast riservato per questo tipo di pacchetto e che non viene inoltrato oltre il dominio locale
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

---
# nDPI 📡

> [!NOTE]
> 
> ***What is Deep Packet Inspection***
> 
> Sappiamo che un #IDS **Intrusion Detection System** è un sistema per identificare le minacce pre - post attack. Un problema dei IDS tradizionali è quando i dati sono **cifrati**
> 
> Qui entra in gioco il **Machine Learning** che riesce a capire se un dato cifrato è effettivamente una minaccia, ma per fare questo ha bisogno di essere allenato, ovvero ha bisogno di **dati certi** ( *se non ho questi dati rischio di produrre delle allucinazioni* )
>

> [!IMPORTANT]
> 
> ***nDPI***
>
>  E' un tool che permette di identificare il traffico di rete =>
> 
> Permettendo di andare a capire se qualcuno sta generando del traffico indesiderato in un determinato contesto. Quindi riesco a capire se un determinato **protocollo** è consentito o meno in un contesto. In nDPI un protocollo non è uno standard dell'RFC ... ma è creato partendo da un protocollo precedentemente creato e rappresentato tramite =>
> 
> - **< major >.< minor >** tipo DNS.Facebook o QUIC.YouTube and QUIC.YouTubeUpload
> 
> Molti applicativi infatti usano + protocolli in base alla funzione richiesta ( *Skype or Facebook are application protocols in the nDPI world but not for IETF* ) e nDPI riconosce + di 300 protocolli
> 
> Oggi quasi tutti i protocolli applicativi si basano su HTTPS / TLS e quindi bisogna ricavare le informazioni per riconoscere il protocollo da quei pochi dati non cifrati in TLS. In base al tipo di traffico i <mark>***dissector***</mark> ( *file che analizza il payload grezzo di un protocollo. Viene chiamato durante la fase di dissezione, ovvero appena viene identificato il protocollo* ) vengono applicati in sequenza a partire da quello che con maggiore probabilità corrisponde al flusso ( *per TCP/80 si prova prima il dissector HTTP* )
> 
> Queste parti che nDPI prende dai <mark>**primi pacchetti di un flusso**</mark> per ricavare informazioni sono parti **non cifrate** ma **offuscate** ( *sulla parte cifrata non si può fare nulla* ). nDPI classifica il traffico basandosi sull'analisi dei primi 8 pacchetti di un fusso
>   
> nDPI è utile anche in sicurezza poiché posso capire, anche se il traffico è criptato, delle informazioni ovvero dei <mark>***metadati***</mark> e per ogni flusso si associa un **flow risk**
> 
> <p align="center"><img src="img/Screenshot 2025-04-11 173720.png" /></p>
> 
> Inoltre nDPI non riconosce solo i pacchetti ma anche il tipo di traffico ( *VPN, HTTPS etc ...* ), se i pacchetti sono dei malware e tutto questo tramite <mark>**Fingerprinting**</mark> 
> 

> [!TIP]
> 
> ***JA3***
> 
> E' stato disegnato per fare **malware detection** infatti crea un'impronta digitale del modo in cui un'applicazione client comunica tramite TLS e così facendo è possibile ricavare informazioni tipo il sistema operativo usato, oppure se un host sta usando una macchina virtuale
> 
> <p align="center"><img src="img/Screenshot 2025-04-13 120307.png" /></p>
> 

> [!CAUTION]
> 
> ***Advanced Data Structured used by nDPI and not***
> 
> - <mark>**Bitmap**</mark> -> Vettore di bit di lunghezza variabile. Molto spesso però queste bitmap sono sparse e se dovessi tenermi un bitmap di milioni di elementi *cosa dovrei fare?* tipo avrei solo gli elementi [ 0, 12, 23, 60, 140, 1556, 50000 ] però questa bitmap mi richiede 2^16 bit di spazio almeno
> 
> - <mark>**Compressed Bitmap**</mark> -> Una bitmap compressa dove **conto le ripetizioni** con algoritmi del tipo **WAH, EWAH, COMPAX etc ...**
> 
> <p align="center"><img src="img/Screenshot 2025-04-15 210746.png" /></p>
> 
> La cosa importante di queste bitmap compresse è che si possono fare operazioni del tipo **AND, OR e NOT** senza decomprimerle. In questo modo è possibile confrontare le colonne tramite AND
> 
> - <mark>**Bloom Filter**</mark> -> Struttura probabilistica che risponde alla domanda: un elemento fa parte di un set? L'idea è quella di fare un confronto veloce per capire se un dato input matcha o meno un filtro. Nel caso matcha allora possiamo fare un operazione + costosa a livello di prestazioni per recuperare il dato interessato, altrimenti nulla
> 
> <p align="center"><img src="img/Screenshot 2025-04-16 113003.png" /></p>
> 
> Un filtro non è altro che un vettore di bit di lunghezza m che ha gli 1 corrispondenti all'hash di un elemento che vogliamo che sia filtrato % m. Per ogni elemento faccio + volte l'hash ( *nell'esempio 3* ) che appunto funzioni hash indipendenti e le inserisco nel filtro
> 
> Il problema è che una volta creato il filtro non posso eliminare elementi inseriti e quindi lo devo ricrearlo e sostituirlo
>  
> - <mark>**Tries**</mark> -> E' una struttura ad albero che ha ogni nodo associato ad una lettera. Se la lettera è una finale di una parola viene marcata in un modo, altrimenti in un altro se è una lettera in mezzo alla parola
> 
> <p align="center"><img src="img/Screenshot 2025-04-16 115830.png" /></p>
> 
> Se ci sono stringhe quindi con un prefisso comune si riesci a risparmiare spazio accumunando delle stringhe
> 
> - <mark>**Radix**</mark> -> Una triade dove i nodi però possono contenere un set di caratteri. Molto utile per i domini scritti al contrario, perché molti finiscono con .com o .it etc ...
> 
> <p align="center"><img src="img/Screenshot 2025-04-16 120036.png" /></p>
> 
> Il **Patricia Tree** è un Radix molto efficiente per fare il **subnet matching per IPv4 / IPv6**
> 
> Supponendo di avere una lista di domini, *come faccio a capire a quale categoria appartiene quel dominio?* Si usano i Radix ma l'algoritmo è l'<mark>**Aho-Corasick**</mark> è un algoritmo che ci consente di confrontare le stringhe
> 
> - <mark>**Binary Fuse Filter**</mark> -> Un alternativa meno costosa al Radix e Aho-Corasick algorithm che usano delle bitmap compresse che migliorano i filtri di bloom e riducono del 13% lo spazio rispetto agli alberi
> 
> - <mark>**Probabilistic Counting**</mark> -> *Come faccio a capire con quanti indirizzi IP diversi ho parlato?* Devo avere un contatore che conta ogni volta che parlo con un indirizzo IP ma l'indirizzo IP, per vedere se ci ho già parlato, lo devo confrontare con qualche altro tenuto in memoria -> Dispendioso
>
> Una possibile soluzione è usare un bloom filter che controlla se un indirizzo è stato contattato "probabilmente"; se non corrisponde con il filtro lo conto e lo aggiungo al filtro
> 
> Partiamo con il concetto che se ho una bitmap con m bit 0 e 1 non definibili tramite una regola, la prob che il primo bit sia 0 è 0.5, la prob che il secondo bit sia 0 è 0.25 etc ...
> 
> Invece di contare ogni elemento si **applica una funzione di hash** ad ogni elemento. Ogni valore hashato viene convertito in binario, e si guarda **quanti zeri consecutivi ci sono all’inizio**  ( *000010101 ... → 4 zeri iniziali* ). Più zeri ci sono = più probabile che il valore sia "raro" → il dataset è più grande. Il massimo numero di zeri trovato tra tutti gli hash dà una **stima della cardinalità**. Se per **sfortuna** trovi un valore hashato con **tantissimi zeri iniziali**, può **gonfiare** la stima ( *outlier* ). Per evitare che un solo valore anomalo rovini tutto si usano **più funzioni di hash**, si calcolano più stime, si prende la **media** etc ...
> 
> I **metodi di conteggio probabilistico** ( *HyperLogLog o Flajolet-Martin* ) sono usati per stimare **quanti elementi distinti** ci sono in un dataset **senza memorizzare tutto**
> 
> L'algoritmo di <mark>**HyperLogLog**</mark> prende un elemento, ci calcola l'hash, lo divide in 2, la prima parte serve per indirizzare l'elemento dell'hash table, la seconda serve per contare quanti 0 si sono visti nella seconda parte => Ogni <mark>**bucket**</mark> ,ovvero entry dell'hash table, tiene traccia del **massimo numero di zeri consecutivi** visti finora per quel prefisso preso in considerazione. **Più zeri = più "raro" = più grande dovrebbe essere l’insieme**
> 
> **Alla fine** → si combinano i valori di tutti i bucket per **stimare la cardinalità complessiva**
> 
> <p align="center"><img src="img/Screenshot 2025-04-16 125324.png" /></p>
> 
> Con 1KB posso stimare la grandezza di un qualsiasi dataset con errore del 3%
> 
> - <mark>**Leaky Bucket**</mark> -> Un modo per identificare eventi problematici. *How do I detect a port or network scan ? How can I detect trafﬁc bursts?*
> 
> - Hai un **secchio** con una **capacità massima** (es: 100 eventi)
> - Ogni volta che arriva un evento (es: una richiesta di connessione), **togli una goccia dal secchio**
> - Ogni tot tempo (es: ogni secondo), **il secchio viene ricaricato (riempito)**
> - Se **arrivano troppi eventi troppo in fretta**, il secchio si svuota velocemente
> - Quando il secchio è **vuoto** e arriva un altro evento → **allarme**: è un comportamento eccessivo
> 
> <p align="center"><img src="img/Screenshot 2025-04-16 125941.png" /></p>
> 
> - <mark>**Entropia**</mark> -> L'entropia, nel contesto delle reti, è una misura di **casualità o disordine** nei dati. L’entropia dei dati grezzi prima e dopo la cifratura **cambia**, ma **rimane entro certi limiti** se i dati sono **omogenei** e grazie a questi limiti è possibile classificare il traffico anche se cifrato
> 
> <p align="center"><img src="img/Screenshot 2025-04-16 130400.png" /></p>
> 
> - <mark>**Data Binning**</mark> -> I bin permettono di classificare i dati usando un **piccolo insieme di intervalli**, invece di considerare **ogni valore singolarmente**. Questo non mi da i dati precisi però mi permette di rispondere a delle domande quali: *Due host stanno usando protocolli simili?* 
> 
> I **bin** sono un modo efficiente per memorizzare osservazioni, ma abbiamo bisogno di un modo per **confrontarli** e trovare **somiglianze**. Due **valori singoli** possono essere confrontati con gli operatori `<`, `>`, `=`,  ma *come si confronta un vettore di dati ?* => bisogna ->
>
> 1. **Normalizzare i dati** -> Per evitare che bin con più osservazioni “pesino” di più
> 2. **Confrontare** ogni elemento del bin con un **algoritmo di similarità**
>
> Ci sono vari metodi per confrontare gli elementi ( *distanza del coseno, distanza euclidea etc ...* ). Un criterio oggi usato è il <mark>**Coefficiente di Similarità di Jaccard**</mark> ->Jaccard(A, B) = ( numero di elementi comuni ) / ( numero totale di elementi unici ) e produce un valore tra 0 e 1 dove 0 = completamente diversi mentre 1 = identici
> 

---
# Physical Layer 🔢

> [!IMPORTANT]
> 
> ***Physical layer***
>
> La NIC incapsula i datagrammi IP in un **pacchetto ethernet** di questo tipo =>
> 
> <p align="center"><img src="img/Screenshot 2024-08-30 132247.png" /></p>
>
> - **Preambolo** -> Serve ad allineare i pacchetti ethernet sul mezzo trasmissivo, quindi per sincronizzarli
> - **Dati** -> L'unità massima di trasferimento è **MTU** da 46 byte fino a 1500 byte. Se il datagramma è + grande di questa unità viene frammentato altrimenti se è più piccolo di 46 byte viene riempito di 0
> 	-  **Indirizzo di destinazione** -> Il MAC di destinazione da 6 byte oppure di broadcast. Questo indirizzo è dell'**interfaccia che deve ricevere il frame**
> - **Indirizzo sorgente**
> - **Type** -> Supporto di protocolli
> - **Controllo CRC** -> Controllo errori bit
> 
> Il preambolo e il CRC sono gestiti a livello della scheda di rete quindi non vengono visualizzati dal programmatore
> 
> <p align="center"><img src="img/Screenshot 2024-08-30 175544.png" /></p>
> 
> Tra un frame ethernet e un altro bisogna aspettare un <mark>l'**Interpacket Gap** ( #IPG )</mark> ovvero il **tempo minimo di attesa tra due frame Ethernet consecutivi** per permettere ai dispositivi di rete di elaborare il pacchetto ricevuto prima di trasmetterne un altro
>
> - **Valore tipico**: **96 bit-time** ( *che equivale a 12 byte di "vuoto" sulla linea* )
> - Questo intervallo è **obbligatorio** per garantire che il sistema possa sincronizzarsi e prevenire la congestione nei dispositivi di rete
> - Viene inserito **dal trasmettitore**, non fa parte del frame Ethernet e non viene trasmesso come dato reale
> 
> <p align="center"><img src="img/Screenshot 2025-02-01 124853.png" /></p>
> 
> Questo livello è **connectionless e non affidabile** poiché che non abbiamo ACK e i pacchetti danneggiati che si perdono o non arrivano a destinazione, vengono eliminati. Per rilevare le collisioni si usa **CSMA/CD** con **binary backoff**. Un protocollo per aggiungere sicurezza a questo livello è l' <mark>**802.1X**</mark>
> 

> [!TIP]
> 
> ***Ethernet***
> 
> <p align="center"><img src="img/Screenshot 2025-01-28 101147.png" /></p>
> 
> Inizialmente ethernet era un progetto creato in questo modo -> 
> 
> *Un cavo coassiale ( giallo ) collegava i vari nodi e alle estremità vi erano 2 tappi che permettevano al segnale che si propagava nel filo di non rimbalzare, quindi portandolo a massa. I computer si attaccavano fisicamente al filo tramite **vampire tap** ovvero si bucava il cavo e si attaccava un altro filo. Il transceiver era invece un adattatore che trasformava il segnale elettrico in bit che poi andavano nell'interfaccia di rete, gestita poi da un controller*
> 
> **Problemi** => 
> 1. Frequenti problemi di **collisioni** e scomodità per portare il filo
> 2. Una persona che si collegava al filo poteva sentire tutte le conversazioni
> 
> <p align="center"><img src="img/Screenshot 2025-01-28 101220.png" /></p>
> 
> Questa tipologia a bus, ulteriormente migliorata nel 1988, si è trasformata nella **tipologia a stella** nel 1990. Questa tipologia operava originariamente in **half duplex** quindi aveva gli stessi problemi delle altre tipologie, ovvero che tutti spedivano a tutti e gli altri computer non potevano rispondere fin quando il canale non era libero. Successivamente è nata la tipologia **full duplex**
> 
> <p align="center"><img src="img/Screenshot 2025-01-28 102513.png" /></p>
> 
> In questo modo si può **ricevere e trasmettere contemporaneamente**
> 
> <p align="center"><img src="img/Screenshot 2025-01-28 102630.png" /></p>
> 
> Per unire alimentazione e flusso di dati è nato il protocollo <mark>***Power over Ethernet PoE***</mark>
>
> <p align="center"><img src="img/Screenshot 2025-01-28 103234.png" /></p>
> 
> La **40 Gb Ethernet** è implementata unendo 4 lane, ognuna con capacità di 10Gb/s. Queste implementazioni dello standard moltiplicato per 4 sono dei modi per non aspettare troppo per l'uscita del prossimo standard
> 
> <p align="center"><img src="img/Screenshot 2025-02-01 100514.png" /></p>
> 
> Notiamo che la velocità del flusso dei dati cresce molto + velocemente rispetto alla velocità del processore che ha per catturare ed elaborare i pacchetti 
> 

> [!TIP]
> 
> ***Connection Device***
>
> - **Patch Panel** -> Punto di raccolta dei cavi
> - **Repeater** => Opera solo a livello fisico, rigenera il segnale che riceve
> - **Hub** => Reapeater multi porta 
> - ***Switch*** => Operano sia a livello fisico rigenerando il segnale che a livello link verificando gli indirizzi MAC. Lo switch distrugge e rigenera il pacchetto dato che controlla il CRC e genera il preambolo. Una sua proprietà è chiamata di <mark>**Store and Forward**</mark>. Questi dispositivi hanno una tabella che usano per filtrare i frame in maniera selettiva
> 
> <p align="center"><img src="img/Screenshot 2024-08-30 182437.png" /></p>
> <p align="center"><img src="img/Screenshot 2024-08-30 182459.png" /></p>
> 
> Gli host non si rendono conto della presenza degli switch e non devono essere configurati ma vale la regola **plug & play, self learning** ( *sono invisibili* )
> 
> Gli switch **bufferizzano i pacchetti** per gestire temporaneamente le trasmissioni in attesa di inoltro
> 
> <p align="center"><img src="img/Screenshot 2024-08-30 183815.png" /></p>
> 
> Questa tabella viene costruita in maniera dinamica quando gli host comunicano tra di loro. *E se collego più switch tra di loro?*
> 
> <p align="center"><img src="img/Screenshot 2024-08-30 183953.png" /></p>
> 
> In una rete del genere, ad una porta switch corrisponderanno + indirizzi MAC poiché avrò più host raggiungibili nella stessa sottorete da una singola porta
> 
> **Ad una porta posso associare anche + indirizzi MAC di un' unico computer**  ( *macchine virtuali installate* )
> 
> Per configurare un dispositivo di livello 2, che quindi normalmente non ha un indirizzo IP, si utilizza una porta **seriale**. Oggi gli switch hanno, oltre la parte di livello 2 che comprende le porte ethernet, un control panel con un proprio indirizzo IP che serve a configurare lo switch tramite browser
> 
> Le porte di uno switch possono essere anche logiche, accomunate da un unico MAC. Questo perché le interfacce logiche, i MAC virtuali etc ... esistono per semplificare le comunicazioni. Un esempio è l'interfaccia logica di <mark>**loopback**</mark>
> 
> - ***Router*** => Opera sia a livello fisico rigenerando il segnale che a livello link verificando il MAC che a livello network verificando l'indirizzo IP. I router però hanno un proprio indirizzo MAC e un indirizzo IP. Operano a livello link e fisico solo per l'interfaccia in cui arriva il frame e poi cambiano l'indirizzo MAC dei frame che inoltrano 
> 

---
# Metrics 📡

> [!IMPORTANT]
> 
> ***Definition of Metrics***
> 
> Per **misurare** le **prestazioni** della **rete** usiamo delle metriche quali =>
> 
> - **Availability** -> La **Disponibilità** indica la **percentuale di tempo** in cui un sistema di rete, un componente o un'applicazione, è **disponibile per l'utente**. Dipende dall'affidabilità dei singoli componenti della rete. La disponibilità si calcola con la formula ->
> 
> *Disponibilita* = *MTBF / ( MTBF + MTTR )​*
> 
> > **MTBF** = tempo medio tra i guasti ( _Mean Time Between Failures_ )
> 
> > **MTTR** = tempo medio per la riparazione ( _Mean Time To Repair_ )
> 
> - **Bandwidth** -> Larghezza dell'intervallo di frequenze utilizzato dal mezzo trasmissivo ( misurato in *Hertz* oppure cicli per secondo -> *Hz / secondo* ) -> **Velocità teorica**
> - **Throughput** -> Quantità di dati che vengono effettivamente trasmessi con successo. Non dipende solo dalla velocità ma anche dalla quantità di dati, dai protocolli, dal mezzo trasmissivo etc... -> **Velocità effettiva**
> -  **Goodput** -> Quantità di dati utili che vengono effettivamente trasmessi con successo -> **Velocità utile**. Viene usato per scoprire connessioni TCP sospette. Viene considerato solo il payload del pacchetto di livello 7
> - **Bit-rate** -> Quantità di bit trasmessi nell'unità di tempo ( *bits / secondo oppure bps* ) 
> - **Latenza** -> Tempo richiesto affinché un messaggio arrivi a destinazione dal momento in cui il primo bit parte dalla sorgente. Utilizzando la commutazione di pacchetto, questo tempo è dovuto ai ritardi causati da ->
> 1. **Ritardi di elaborazione del nodo** -> Controllo errori sui bit e determinazione del canale di uscita
> 2. **Ritardi di accodamento** -> Attese di trasmissione e intensità del traffico
> 3. **Ritardo di trasmissione** -> Tempo impiegato per trasmettere il pacchetto sul mezzo trasmissivo ( *L/R* ovvero lunghezza del pacchetto / rate del collegamento => bit / bps )
> 4. **Ritardo di propagazione** -> Tempo impiegato da 1 bit per raggiungere l'altro nodo ( d/s ovvero lunghezza del collegamento / velocità di propagazione del mezzo ( 3 \* 10^8 m/sec ) )
> 
> <p align="center"><img src="img/Screenshot 2025-05-04 100140.png" /></p>
> 
> - **RTT ( Round-Trip Time )** -> L’RTT è il tempo che un pacchetto impiega per viaggiare dalla sorgente alla destinazione e poi ritornare alla sorgente. È una misura comune utilizzata per valutare la latenza di una rete e può essere influenzato da vari fattori, come la distanza fisica tra i nodi, la congestione della rete, e la qualità della connessione. Sappiamo inoltre che in base alla distanza e all' RTT, viene regolata la windows possibile per trasmettere i dati
> 
> <p align="center"><img src="img/Screenshot 2025-05-04 100554.png" /></p>
> 
> - **Utilization** -> **L'utilizzo** è una misura più dettagliata rispetto alla **throughput**. Indica la **percentuale di tempo in cui una risorsa è effettivamente in uso** in un dato intervallo di tempo. Un valore molto basso di utilizzo può essere un segnale che qualcosa non sta funzionando correttamente ( _poco traffico perché il file server è andato in crash_ )
> 
> - **Jitter** -> E' la **variazione nel ritardo tra pacchetti consecutivi** su un collegamento in una sola direzione. E' un parametro molto importante per le applicazioni multimediali, come le **telefonate via Internet** o le **trasmissioni video**. In sostanza, il **jitter misura quanto è irregolare il flusso di dati** -> Nella telefonia il Jitter deve essere molto basso
> 
> <p align="center"><img src="img/Screenshot 2025-05-04 101629.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-05-04 101731.png" /></p>
>

> [!IMPORTANT]
> 
> ***Other Metrics***
> 
> <p align="center"><img src="img/Screenshot 2025-05-04 102746.png" /></p>
> 
> - **Mean** -> La **media** è una misura di tendenza centrale che rappresenta il valore medio di un insieme di dati. Nel caso dell'RTT, la media rappresenta il tempo medio che un pacchetto impiega per viaggiare dalla sorgente alla destinazione e ritorno
> 
> - **Variance** -> Media dei quadrati degli scarti dalla media. Ovvero la media delle somme delle differenze al quadrato tra valore misurato e media 
> 
> - **Standard Deviation** -> La radice della varianza. La **deviazione standard** è una misura che indica quanto i valori di un insieme di dati si discostano dalla media. Una bassa deviazione standard indica che i valori sono vicini alla media, mentre una deviazione standard elevata implica che i valori sono sparsi su un ampio intervallo. Nel caso dell'RTT, una deviazione standard bassa indica che la latenza è consistente e stabile, mentre una deviazione standard alta potrebbe suggerire variabilità nella connessione o la presenza di problemi di rete
> 
> - **Outlier** -> Un **outlier** è un dato che si discosta significativamente dal resto dei dati in un insieme. In un contesto di RTT, un outlier rappresenta un valore di latenza che è notevolmente più alto o più basso rispetto alla maggior parte degli altri valori raccolti. Gli outlier possono essere causati da vari fattori, come errori di rete, congestione o malfunzionamenti di qualche nodo della rete. È importante identificare e trattare questi valori, in quanto possono influire negativamente sulle analisi statistiche, portando a conclusioni errate
> 
> Per trovare gli outlier, una formula tipica è la seguente ( **IQR** = Q3 - Q1 ) ->
> 
> - **Lower-bound** = Q1 - 1.5 \* IQR
> - **Upper-bound** = Q3 + 1.5 \* IQR
> 
> ---
> 
> - **Percentile** -> **Percentile**: è il valore sotto il quale cade una certa percentuale di osservazioni in un insieme di dati ( _il **95° percentile** indica il valore sotto cui ricadono il 95% delle osservazioni_ )
> 
> Nelle reti Il 95° percentile è importante perché rappresenta il consumo massimo "tipico" ->
> 
> > Per il 95% del tempo, l'utilizzo rimane al di sotto di quel valore
> 
> Gli operatori lo usano per limitare i costi. Se si supera solo raramente quel valore, forniscono meno banda in media, risparmiando risorse
> 
> - **Quartiles** -> Divido la mia serie in percentili noti
> 
> <p align="center"><img src="img/Screenshot 2025-05-04 104245.png" /></p>
> 
> - **Z-Score** -> Lo **z-score**, noto anche come **punteggio standardizzato**, è una misura statistica che indica di quante deviazioni standard un dato valore si discosta dalla media di un set di dati. È definito come ->
> 
> z = *( x - media ) / deviazione*
> 
> dove x è il valore osservato. Lo z-score è utile per identificare valori anomali in una distribuzione normale, poiché valori di z che si discostano significativamente dalla media ( _|z| > 2_ ) possono essere considerati fuori dal normale intervallo di variabilità. Questo è particolarmente utile nell'analisi dei dati per rilevare outliers
>

---
# Libcap 📡

> [!TIP]
> 
> ***Introduction of Packet Capture***
> 
> Da quando le workstation sono diventate interconnesse, gli amministratori di rete hanno sentito l’esigenza di “vedere” cosa scorre sui cavi. I sistemi operativi hanno sviluppato delle **API** per lo sniffing dei pacchetti. Tuttavia, poiché non esisteva un vero e proprio standard, ogni sistema operativo dovette inventare una propria API: [Ultrix Packet Filter di DEC](AA-PBM2A-TE_Ultrix_4.0_The_Packet_Filter_-_An_Efficient_Mechanism_for_User-Level_Network_Code_Jun1990.pdf), [Snoop di Solaris](AB_Snoop) etc ...
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
> **Ma come può un processo in spazio utente leggere i dati che viaggiano sulla scheda di rete, senza accedere direttamente allo spazio kernel ?**
> 
> In passato si usavano **chiamate di sistema**, che però, pur essendo sicure, risultavano lente. Una soluzione più efficiente è usare applicazioni che accedono direttamente alla memoria tramite #DMA ( _Direct Memory Access_ ), permettendo di leggere/scrivere pacchetti bypassando la #CPU e riducendo la latenza
> 
> Questa **memoria circolare** viene creata nel **kernel** e contiene la copia dei pacchetti catturati. Essa è **condivisa** ( o mappata ) con lo **spazio utente**, permettendo così all'applicazione di monitoraggio di accedervi direttamente
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
> Per ogni filtro che accetta il pacchetto, **BPF copia nel buffer associato** solo la quantità di dati richiesta. Prima dell’applicazione del filtro, viene utilizzato un **puntatore**: il pacchetto viene **filtrato "sul posto"** ( **direttamente nella memoria dove il motore DMA della scheda di rete lo ha scritto** ), evitando di copiarlo in un altro buffer prima di applicare il filtro
> 
> Successivamente, lo **sniffer** legge i pacchetti dal **buffer condiviso** e li trasferisce nel proprio **buffer locale** per l’elaborazione
> 
> Un **filtro di pacchetti** è semplicemente una **funzione booleana** applicata a ogni pacchetto
> 
> Più il pacchetto è grande, **maggiore sarà la dimensione della cella** nel buffer circolare
> 
> <p align="center"><img src="img/Screenshot 2025-02-13 185937.png" /></p>
> 
> Il **BPF** ( e la sua evoluzione **eBPF** ) è una vera e propria **macchina virtuale** che accetta **programmi ( filtri )** scritti in uno specifico **bytecode**, provenienti dallo **spazio utente**. Questi programmi vengono caricati nel kernel tramite una **chiamata di sistema** – che avviene **una sola volta** – e da quel momento in poi vengono eseguiti direttamente nel kernel, senza ulteriori interventi dallo spazio utente.
> 
> <mark>_**eXpress Data Path ( XDP )**_</mark> è un framework che consente l'elaborazione dei pacchetti **ad altissima velocità**, integrando programmi BPF direttamente **all'ingresso dell'interfaccia di rete**. In pratica, un programma BPF può essere eseguito **appena il pacchetto viene ricevuto**, _prima ancora che venga processato dallo stack TCP/IP del kernel_
> 

> [!TIP]
> 
> ***What is Libpcap ?***
>
>  La **libreria** libpcap <mark>**Packet capture library**</mark> è stata scritta come **parte di un programma** più ampio chiamato <mark>***TCPDump***</mark>. La libreria libpcap ha permesso agli sviluppatori di scrivere codice per ricevere pacchetti di livello di collegamento su diversi tipi di **sistemi operativi UNIX** senza doversi preoccupare delle differenze sulle varie schede di rete e dei driver dei diversi sistemi operativi. In sostanza, la libreria libpcap **cattura i pacchetti direttamente dalle schede di rete**, il che ha permesso agli sviluppatori di scrivere programmi per decodificare, visualizzare o registrare i pacchetti
>  
>  <mark>**The biggest user of this construct ( BPF ) might be libpcap**</mark>
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
> TcpDump quando viene eseguito passa attraverso il compilatore interno di questa libreria che genera una struttura che viene passata al kernel e usata come filtro. Con il flag  `-ddd` mostriamo cosa viene caricato all'interno di questa struttura
>
>  1. Il bytecode BPF è collegato all'interfaccia di rete #TAP
>  2. TcpDump stampa i pacchetti per cui il filtro è soddisfatto
>
> In sostanza, Tcpdump chiede al kernel di eseguire un programma BPF all'interno del contesto del kernel. Potrebbe sembrare rischioso, ma in realtà non lo è. Prima di eseguire il bytecode BPF, il kernel si assicura che sia sicuro ->
> 
> 3. Tutti i salti sono solo in avanti, il che garantisce che non ci siano loop nel programma BPF. Pertanto deve terminare
> 4. Tutte le istruzioni, in particolare le letture in memoria, sono valide e nel raggio d'azione
> 5. Il singolo programma BPF ha meno di 4096 istruzioni
>

> [!IMPORTANT]
> 
> ***How Wireshark Work?***
> 
> Sulla maggior parte delle moderne piattaforme UNIX è disponibile libpcap. E' installato di default su BSD e macOS, e potrebbe essere installato di default anche sulle distribuzioni Linux
> 
> Sono disponibili due versioni Windows di libpcap. Il più vecchio si chiama [WinPcap](https://wiki.wireshark.org/WinPcap); Non viene più mantenuto attivamente ed è basato su una versione precedente di libpcap. Il più recente si chiama [Npcap](https://nmap.org/npcap/); è attivamente mantenuto ed è basato su una versione relativamente recente di libpcap ed è disponibile solo per Windows 7 e versioni successive di Windows
> 
> In Wireshark, utilizzando il linguaggio Lua o C, esistono tre meccanismi principali per ispezionare ed elaborare i pacchetti =>
> 
> - **Dissector** -> Utilizzato quando si vuole modificare il modo in cui Wireshark mostra il pacchetto, analizzando il payload grezzo di un protocollo. Viene chiamato durante la fase di dissezione, ovvero appena Wireshark identifica il protocollo
> - **Post-dissector** -> Usato quando si vogliono analizzare i pacchetti dopo che tutti i dissector standard hanno terminato la loro analisi, per aggiungere informazioni extra o confrontare dati tra più protocolli. Non analizza direttamente il payload, ma legge i campi già estratti da altri dissector
> - **Tap** -> Utile per raccogliere statistiche, dati temporali o esportare flussi di pacchetti
> 
> <p align="center"><img src="img/Screenshot 2025-05-04 190230.png" /></p>
> 

---
# CDN 📡

> [!CAUTION]
> 
> ***Problem TLS & CDN***
>
> *Perché, se inserisco l'IP di una pagina web invece del suo URL, il browser mi segnala che la connessione non è sicura, mentre utilizzando l'URL non ricevo alcun avviso? Ad esempio, accedendo a `https://131.114.21.42` ottengo un errore di certificato non valido, mentre con `https://www.unipi.it` il certificato risulta valido?*
> 
> **I certificati TLS sono emessi per i domini, non per gli IP** =>
> 
> - Quando un sito web usa TLS ( _HTTPS_ ), il browser controlla che il certificato fornito dal server corrisponda esattamente al nome del dominio richiesto (es. `www.unipi.it`)
> - Il certificato di `www.unipi.it` è registrato per quel dominio specifico e **non per l'IP** (`131.114.21.42`)
> - Se accedi direttamente con l’IP, il certificato non può essere verificato e il browser mostra un errore di sicurezza
> 
> *Ma perché non creo certificati per indirizzi IP?*
> 
> Semplicemente perché in internet ho molti indirizzi IP uguali / questi indirizzi IP cambiano facilmente
> 
> *Come faccio a scegliere a quale macchina andare?*
> 
> Semplicemente non la scegli tu ma la sceglie il "cash" che dice ai protocolli di routing dove instradare chi
> 
> Parlando di soldi abbiamo anche gli #ISP che comprano le cache dei grandi fornitori di contenuti online per avvantaggiarsi. I grandi fornitori di contenuti come Youtube e Netflix installano i propri sistemi proprietari di cache dei contenuti presso gli ISP. È a loro vantaggio reciproco perché migliora le prestazioni per i clienti, riduce notevolmente il traffico che lascia l'ISP in peering con altri ISP e riduce notevolmente il traffico in arrivo dalle connessioni Internet primarie ai data center dei provider di contenuti. Non è necessario che l'ISP rompa HTTPS perché la richiesta di contenuto sta andando in una scatola appartenente a quel provider. L'ISP ha una scatola nera nei suoi rack di server. Non possono toccarli e non hanno idea di come funzionino, né se ne preoccupano. I fornitori di contenuti più piccoli collaborano con servizi di caching come #Akamai per ottenere lo stesso effetto
> 
> Se così non fosse allora gli ISP dovrebbero inviare le richieste del client fino al server del fornitore ( *o generare traffico inutile* ) che poi è in grado di risolvere
> 
> Le grandi aziende tipo Amazon, Google etc ... hanno i propri **Content Delivery Network** ovvero una rete di distribuzione di contenuti ( #CDN ) cioè una **rete di server interconnessi che velocizza il caricamento di pagine Web** per applicazioni ad alto contenuto di dati -> ***==Cache==***
> 
> ***IMPORTANTE: L'indirizzo della cache è dell'operatore locale e non dell'azienda per cui abbiamo acquistato le informazioni in cache***
> 

---