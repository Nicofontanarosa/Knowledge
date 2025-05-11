
> [!IMPORTANT]
> 
> ***Simple Network Management Protocol***
>
>  <p align="center"><img src="img/Screenshot 2025-03-07 201616.png" /></p>
>  
>  C'era la necessità di gestire dispositivi di rete come switch, router, ma anche sensori, apparecchiature mediche, ecc., per ottenere informazioni sul loro stato o per ricevere dati da essi. Il protocollo è rimasto costante nel tempo, mentre i dati gestiti si sono evoluti ( *ASN tree* ). Gli obiettivi di SNMP sono =>
>  
>  - Minimizzare la complessità delle funzioni di gestione implementate dagli agent
>  - Utilizzare le stesse funzioni
>  - su tutti i dispositivi di rete
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
> Tutti questi oggetti sono scritti e descritti in un <mark>**modulo MIB**</mark>. Questo modulo MIB è formato da un nome, una definizione eventuale di altri moduli MIB, un **Module Identifier** e un **Object Identifier**
> 
> <p align="center"><img src="img/Screenshot 2025-05-11 215953.png" /></p>
> 
> ...
> 
> Un altro esempio sono le **notifiche ( trap )** inviate quando la **porta di un dispositivo cambia stato**. Se accendo un dispositivo con molte porte attive, riceverò **molti pacchetti trap in poco tempo**: questa situazione è nota come <mark>**_Trap Storm_**</mark>
> 

> [!TIP]
> 
> **MIB-II** definisce una serie di oggetti standard ( circa **170** ) relativi ai protocolli **IP, ICMP, UDP, TCP** e **SNMP**. L’implementazione di questo MIB **non dovrebbe interferire** con il normale funzionamento delle attività di rete
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
> 4. **Trap** manda le notifiche sul cambio di stato. Non viene richiesta dal manager ma viene mandata dall'agent. Essendo inviata solo dall'agent, se viene persa, non viene richiesta dal manager ( *quella particolare trap* ) però il manager fa il pulling ( *siamo salvi* ). Per non mischiare il traffico il manager è in ascolto sulla <mark>***port 162***</mark>
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
> Che analizzato su #whireshark esce in questo modo
> 
> <p align="center"><img src="img/Screenshot 2025-03-14 200906.png" /></p>
> 
> Tutto il processo di SNMPWalk si basa sull'agent che richiede il next object id di uno di default dato alla partenza, e il server risponde. SNPMWalk visualizza la risposta e genera la getNext dell'oggetto che ha ricevuto
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
> > **Ricordiamo che tutti questi dati sono presenti nei CLI Counters** ( *if config* )
> 
> <p align="center"><img src="img/Screenshot 2025-03-22 092008.png" /></p>
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
> La novità principale di SNMPv3 è l'aggiunta di un contesto ovvero una quantità di informazioni di gestione a cui un'**entità SNMP** può accedere. Non ci basiamo + sulla community ma sull'user
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

> [!NOTE]
> 
> ***What can i mesure?***
>
>  **Analisi quantitativa del traffico di rete**
>  
> L'**analisi quantitativa** si concentra sulla **misurazione** e **classificazione** del traffico di rete sulla base di parametri rilevanti. Gli aspetti principali includono =>
> 
> - **Identificazione dei protocolli applicativi utilizzati** → Rilevazione e classificazione del traffico generato da applicazioni come Microsoft Teams, Skype, servizi email, etc ...
> - **Top Talkers** → Analisi degli host che generano o ricevono il maggior volume di traffico
> - **Origine e destinazione del traffico** → Monitoraggio delle principali origini e destinazioni per individuare gli endpoint di comunicazione più rilevanti
> - **Rendicontazione del traffico per host** → Creazione di report dettagliati che suddividono il volume di traffico per singolo dispositivo o utente
> 
> **Analisi qualitativa del traffico di rete**
> 
> L'**analisi qualitativa** si concentra sugli aspetti legati alla **sicurezza** e alla **funzionalità** del traffico di rete, con l'obiettivo di individuare anomalie o potenziali minacce. Gli aspetti principali includono =>
> 
> - **Individuazione di strumenti per l'anonimato** → Rilevamento dell'uso di tecnologie come Tor o VPN anonime, che possono nascondere l'identità degli utenti o mascherare il traffico
> - **Identificazione di errori** → Analisi di problemi e anomalie nella comunicazione di rete, come perdita di pacchetti, configurazioni errate o malfunzionamenti applicativi
>
> *Tutto cioè tradotto graficamente come segue* =>
>
> <p align="center"><img src="img/Obiettivo finale Visibilità e sicurezza.png" /></p>
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
> 
> <p align="center"><img src="img/Screenshot 2025-02-04 193444.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-02-04 193535.png" /></p>
> 
> 3. Utilizzare uno **switch con funzionalità di port mirroring**, configurando una porta di mirroraggio tramite tecnologie come **SPAN**, **RSPAN** o **ERSPAN**
> 
> <p align="center"><img src="img/Screenshot 2025-02-04 190528.png" /></p>
> 
> Ovviamente, o la porta che esegue il mirror supporta una velocità maggiore, oppure devo stare attento a non superare la sua capacità. Infatti, normalmente una porta genera traffico in modalità **full duplex** (es. 1 Gbps in upload + 1 Gbps in download), mentre sulla porta mirror tutto il traffico viene **inviato** verso il sistema di analisi. Quindi, se non ho abbastanza banda disponibile sulla porta di destinazione (es. 2 Gbps in totale), rischio di perdere pacchetti. In pratica, il traffico viene visto **in modalità half duplex** (solo una direzione per volta nella pratica del mirroring).
> 
> <p align="center"><img src="img/Screenshot 2025-02-04 191831.png" /></p>
> *In questo modo però rischio di perdere pacchetti durante il tragitto*
> 
> **ERSPAN** (_Encapsulated Remote SPAN_) invece estende il concetto di RSPAN, permettendo di inviare i pacchetti mirrorati anche attraverso dispositivi intermedi **non configurati per SPAN o RSPAN**, grazie all'incapsulamento tramite **GRE** ( _Generic Routing Encapsulation_ ) #GRE
>  
> Altre tecniche includono =>
> 
> 4. A livello software, usare una **VLAN di mirroring** oppure funzioni come **Switch Traffic Filter / Mirroring** ( *es. su dispositivi Juniper* )
> 5. Eseguire un attacco di **MAC Spoofing**, fingendosi un altro host. In questo modo posso intercettare il traffico **da fuori verso dentro** ( *non il traffico interno all' host* ), poiché lo switch inoltrerà i pacchetti destinati sia all' host legittimo sia a me
> 
> ---
> 
> <p align="center"><img src="img/Screenshot 2025-02-04 193107.png" /></p>
> 
> In questo modo vedo tutti i pacchetti che devono "*uscire*" dalla rete però non vedo le comunicazioni locali da host a host dato che quelle si fermano allo switch / acces point precedenti
> 

> [!TIP]
> 
> **Tap Modes**
> 
> <p align="center"><img src="img/Screenshot 2025-02-11 101744.png" /></p>
> 
> Questa modalità ci consente di monitorare il traffico distinguendo le due direzioni garantendo che nessun pacchetto venga perso
> 
> <p align="center"><img src="img/Screenshot 2025-02-11 101910.png" /></p>
> 
> La modalità di filtraggio normale
> 
> <p align="center"><img src="img/Screenshot 2025-02-11 111210.png" /></p>
> 
> Consente di non far bloccare tutto il sistema se va già un apparecchio interposto tra il router e lo switch ( #firewall  ). Questa modalità fa prevalere la disponibilità piuttosto che la sicurezza
> 
> <p align="center"><img src="img/Screenshot 2025-02-11 111445.png" /></p>
> 
> Crea multiple copie del flusso dati che arriva ad uno switch ( #hub )
> 
> <p align="center"><img src="img/Screenshot 2025-02-11 111540.png" /></p>
> 
> Consente di filtrare il traffico che passa e viene inviato a strumenti di monitoraggio
> 
> I <mark>**vantaggi**</mark> dei tap quindi sono =>
> 6. **Eliminazione del rischio di perdita pacchetti**
> 7. **Monitoring dei pacchetti**
> 
> Gli <mark>**svantaggi**</mark> dei tap sono =>
> 8. **Ho bisogno di due interfacce per catturare il traffico**
> 9. **Costi aggiuntivi per i tappi**
> 10. **Non si può leggere il traffico prima dello switch** ( *host to host* )
> 

> [!CAUTION]
> 
> ***Packet capture***
> 
> Normalmente il traffico rivolto ad un host è protetto da vari algoritmi di encryption che lo proteggono da analizzatori esterni. Se però catturo il traffico a livello ethernet, sorvolo in qualche modo tutti questi meccanismi di protezione  
> 

> [!IMPORTANT]
> 
> ***Flow Measurement***
> 
> Nella rete occorre analizzare il traffico ma non è possibile fare il **mirror** di tutto il traffico che si genera perché sarebbe troppo e dal punto di vista della rete, la riempie e la rallenta. Un altro problema è che il traffico è **Criptato** è quindi **difficilmente comprimibile**, diverso per file di testo, simile per le immagini che già per loro definizione / creazione, sono compresse e di qualità ridotta
> 
> Sono così nate delle **sonde** posizionate in rete chiamate <mark>**meter**</mark> che non inoltra il traffico ma fa un analisi che poi invia al manager ( **delega l'elaborazione dell'analisi** ). Spacchettare il pacchetto "criptato" è una pura curiosità 
> 
> <p align="center"><img src="img/Screenshot 2025-03-29 154557.png" /></p>
> 
> SNMP implementa questo paradigma **manager-agent** ( [[SNMP 📡]] ) e i contatori SNMP possono essere usati per il **monitoraggio del traffico**. Il problema è che devo specificare quali contatori vedere e analizzare e l'Agent deve avere delle soglie per informare il manager dell'avvenimento di un certo evento, che però per i dispositivi su internet sono inutili perché sarebbero una diversa dall'altra
> 
> Questo meccanismo di pooling è anche sbagliato quando ad esempio stabilisco una connessione per risolvere un nome DNS ma la connessione dira pochi secondi e quindi il manager non fa in tempo a chiedere cosa è successo che la connessione è stata già chiusa e dimenticata
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
> Il problema di questi flussi è che di quintuplette proto - ip_s - prt_s - ip_d - prt_d c'è ne sono tantissime e quindi normalmente si guarda solo il traffico in uscita o in entrata prendendo come costanti proto - ip_s - ip_d
>  
> Il **limite** di questo approccio è che tutto il **traffico non ip** non riesco a vederlo poiché non passa dal router. Con SNMP invece riesco a capire a livello fisico da quale porta sta passando il traffico ( **congestion, delay, packet loss** ) e non posso misurare neanche **transaction latency, # positive/negative replies & protocol errors**
> 
> Un altro problema è che il meter può decidere di mandare i dati alla fine del flusso oppure durante il flusso. Mandarli alla fine è peggio ( *il meter si può interrompere e i dati non arriveranno mai all'operatore telefonico* ). Tutto questo determina =>
> 
> - ***Overhead & Accuratezza*** -> Maggiore misurazione comporta una maggiore quantità di dati raccolti ma + Overhead ( *carico della CPU* ) su **router, switch** e **host finali**
> 
> - ***Sicurezza*** ->I **flussi emessi** devono raggiungere i **collector** attraverso percorsi protetti e anche questi meter devono essere protetti
> 
> L'**architettura** usata **oggi** è la seguente =>
> 
> <p align="center"><img src="img/Screenshot 2025-03-29 191235.png" /></p>
> 
> Dove il probe funge da meter è vive all'interno di un router. Il protocollo di trasporto del flusso non è più SNMP ma <mark>***NetFlow***</mark> #netflow ( *standard sviluppato da #CISCO* )
> 
> <p align="center"><img src="img/Screenshot 2025-03-29 190841.png" /></p>
> 
> In questo grafico possiamo vedere come il flusso però attraversa più router quindi devo stare attento a non considerare + di una volta lo stesso flusso. Questo viene evitato tramite un blocco di analisi del traffico da parte del probe di una direzione ( *conta solo il traffico in entrata / uscita* ). Un esempio di flussi è il seguente =>
>  
> <p align="center"><img src="img/Screenshot 2025-03-29 192115.png" /></p>
> 

> [!IMPORTANT]
> 
> ***Cisco NetFlow & IPFIX***
>
>  Protocollo standard sviluppato da CISCO per il **trasporto dei flussi** unidirezionali fino alla versione 8 e bidirezionali dalla 9. La versione NetFlow 9 trasporta i flussi **in maniera flessibile** poiché al contrario delle versioni precedenti, dove un pacchetto aveva un formato standard e fixed, qui nasce il concetto di **template**. Prima viene inviato questo template e poi arrivano i dati => Posso aggiungere tutti i campi che voglio aggiungendoli nel template che invio
>
> Dalla v.9 c'è un **supporto di IPv6**
> 
> Come per il conceetto di **meter**, questo protocollo misura solo il traffico che attraversa il router e quindi dal livello 3 in su
> 
> nelle versioni precedenti alla 9 c'è un **flow sequence number** per tenere traccia del flusso e di quelli persi. Dalla 9 però, è stato rimosso questo numero e introdotto il **packet sequence number** che tiene traccia dei pacchetti persi ma non dei flussi
> 
> Iniziamo con il dire che un flusso ha una **chiave** -> **quintupletta proto ip porta ip porta** e una parte di **valore** -> 
> 
> <p align="center"><img src="img/Screenshot 2025-04-13 130351.png" /></p>
> 
> > ( *le interfacce sono molto importanti soprattutto per i provider* )
> 
> I **contatori** sono gli stessi di **SNMP**, e le **interfacce** sono importanti poiché io malintenzionato potrei spacciarmi per un client interno alla rete e ad esempio fingermi il server DHCP e cambiare gli indirizzi agli host. Quindi sapere da quale interfaccia è arrivato un indirizzo IP è importante. Banalmente la rete interna avrà un indirizzo IP e dalle porte che escono non potranno entrare pacchetti con la stessa famiglia di indirizzi ( *pacchetto kamikaze* )
> 
> I **flussi attivi** vengono memorizzati in RAM nella cosiddetta **cache NetFlow**. Una volta comunicato il flusso svuoto la cache. Il flusso termina quando dura troppo a lungo, oppure se contiene il flag FIN oppure è rimasto inattivo per troppo tempo. Una **Conseguenza importante** è che quindi un flusso può essere inviato al collector in maniera segmentata che quindi ha anche un compito di **riassemblatore**
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
> <mark>***IPFIX***</mark> è un protocollo che implementa una tecnologia per l'IP Trafﬁc Flow measurement che fa le stesse cose di NetFlow v9 però non dipende da CISCO. Si basa sul protocollo di trasporto SCTP ( *non funziona su windows* ) che però non usa nessuno e infatti si può usare anche sopra il protocollo TPC/UDP
> 
> <p align="center"><img src="img/Screenshot 2025-04-15 124423.png" /></p>
> 
> > *La differenze è che il probe l'hanno chiamato meter*
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
> I numeri di record possibili sono N che in v5 era max 30
> 
> <p align="center"><img src="img/Screenshot 2025-04-15 105705.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-04-15 105804.png" /></p>
> 
> Questo modo di definire i flussi è **statico e non estendibile, anche se molto veloce** poiché occupa **poca banda**. Per questi motivi è stata definita la v9 che propone il concetto di template. Ogni X tempo, si invia il template e subito dopo i date che quindi il collector riesce ad interpretare. Nel template posso decidere se mettere dei parametri o meno
> 
> <p align="center"><img src="img/Screenshot 2025-04-15 110659.png" /></p>
> 
> > da 1 a + di 100
> 
> <p align="center"><img src="img/Screenshot 2025-04-15 110807.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-04-15 110856.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-04-15 122542.png" /></p>
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