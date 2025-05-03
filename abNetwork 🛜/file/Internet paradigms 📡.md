#internet #cyber_security #TCP #TCP/IP #ISO/OSI  #Avron #Protobuf

> [!NOTE]
> 
> ***Paradigmi e Protocolli per la Gestione di Rete***
>
>  Sappiamo che con il passare del tempo il numero di dispositivi in internet cresce sempre di + e la diversità e complessità continua a crescere. Quello che si cerca allora è una **disponibilità permanente dei servizi di rete con qualità ottimale** e una **riduzione dei costi per l'infrastruttura di rete**. Questo può avvenire tramite la <mark>**Gestione di rete**</mark>
>  
>  ![[Screenshot 2025-03-05 090852.png]]
>  
>  Ci sono tante prospettive per affrontare i problemi che risolve la gestione di reti =>
>  
>  - Dal punto di vista dell'**operatore**, cercherà in tutti i modi di far entrare + clienti possibili su un filo anche se non c'è spazio
>  - Dal punto di vista della **sicurezza** bisogna garantire la sicurezza della rete
>  - Dal punto di vista delle **performance** bisogna garantire delle prestazioni che non vanno a degradarsi e queste performance devo andarle a **dedurre** dai dati che raccolgo ( *prima si origliava la conversazione per capire la qualità della trasmissione* ) 
>
> Oggi si parla di #MOS =>
> > The Mean Opinion Score (MOS) is **a measurement of the voice quality of an interaction**. The calculation of MOS uses an industry standard measurement methodology to rank audio quality from 1 (unacceptable) to 5 (excellent)
> 
> Tutte le funzioni quindi di **controllo, coordinamento e gestione** vengono fatte su questi *managed objects* che **è la rappresentazione astratta di una risorsa che ne presenta le sue proprietà**. Questi oggetti sono una rappresentazione astratta di una risorsa reale
> 
> > *Gli oggetti gestiti non corrispondono necessariamente agli oggetti della programmazione orientata agli oggetti. Nel contesto della gestione delle reti Internet, anche semplici variabili possono corrispondere agli oggetti gestiti*
> 
> ![[Screenshot 2025-03-05 112419.png]]
> 
> Questi oggetti presentano **attributi, operazioni, un comportamento ovvero l'interazione con il mondo reale e le notifiche ovvero messaggi generati in determinate situazioni dal managed object**. Il **Manager o Management System** non deve dipendere dall'implementazione fisica del managed object ( *macchinetta caffè* )
> 
> Il <mark>**MIB**</mark> ( **Management Information Base**  #MIB ) ovvero l'unione di tutti i managed object contenuti in un sistema ( *in sostanza un db con tanti pezzettini* ) ed è composto da moduli intercambiali e modificabili da sistema a sistema. Questo MIB dovrebbe essere conosciuto sia dal manager che dal responsabile ( *macchinetta* )
> 
> l' **agent** ( *responsabile* ) è un software che gira all'interno della risorsa fisica e implementa i managed object del MIB accedendo a questa e attende un comando del **manager**, comunica delle notifiche ad un cambio di stato nel MIB e protegge i moduli da accessi esterni al manager tramite **Regole di accesso**
> 
> ![[Screenshot 2025-03-05 132503.png]]
>  *spesso il manager e agent girano sullo stesso host*
>  
>  Un **protocollo di gestione** implementa l'accesso a managed object remoti codificando i dati di gestione, che vengono poi protetti durante il trasferimento. Questo protocollo di management si basa su =>
>  
>  - **Fault management** -> Identificazione degli errori e riparazione. Bisogna far in modo di non intaccare il funzionamento del sistema anche in caso di errore però bisogna comunicarlo ( *if you fail, fail hard* )
>  - **Configuration management** -> Configurazione secondo standard amministrativi
>  - **Account management** -> - Inserimento dei dati di consumo ( *utilizzo* ), distribuzione e monitoraggio e fatturazione ai clienti per il consumo delle risorse. L'utilizzo degli account ci permette di andare a capire dove fare più manutenzione poiché ci sono utenti che usano di più un pezzo
>  - **Performance management** -> Bisogna capire se si sta andano in peggio oppure in meglio e quindi investire 
>  - **Security management** -> Autenticazione e segnalazione di attività illecite
>
> <mark>***FCAPS***</mark> ( #FCAPS ) è il termine con il quale si riassumono queste 5 proprietà o **aree funzionali** che non sono indipendenti tra di loro ma si influenzano a vicenda
> 
> Per comunicare manager e agent a livello internet si utilizza lo stack TCP / IP che è l'evoluzione dello stack ISO/OSI poiché molto più semplice da implementare ( #TCP/IP #ISO/OSI  )->
> 
> ![[Screenshot 2025-03-05 151234.png]]
> ![[Screenshot 2025-03-05 154929.png]]
> 
> E' importante notare lo stacco tra le 2 implementazioni soprattutto il livello di **presentation** che trasforma e adatta la presentazione dei dati
> 
> *Ma come fanno effettivamente a comunicarsi i dati?* cioè 2 router comunicano tra di loro mediante indirizzo ip, oppure una scheda wifi comunica con un'altra scheda wifi, non con una ethernet poiché i dati non sono compatibili ( *WhatsApp comunica con WhatsApp* ). Diciamo che le interfacce attraverso le quali si può accedere alle primitive di servizio ( *request, indication, response e confirm* ) sono chiamate **punti di accesso al servizio** ( **Service Access Points** #SAP )
> 
> ![[Screenshot 2025-03-05 154232.png]]
> 
> Mentre i dati che noi comunichiamo tramite queste interfacce usano una sintassi chiamata <mark>**ASN Abstract Syntax Notation**</mark> ( #ASN ). Questa sintassi unica è nata come soluzione al problema citato prima, *Come faccio a far capire macchine con SO diversi e anche per tutti i dispositivi che continuano a svilupparsi?* Prima si inviava semplice testo che però bisognava interpretare se si cambiava macchina e quindi è nata questa sintassi che aveva come obiettivo quello di cambiare l'informazione su macchine diverse ( *8/16/32/64 bit che interpretavano i bit o tramite #little-endian o #big-endian* ) e che fosse anche indipendente dai linguaggi di programmazione e trattabile tramite handshake 
> 
> > *Big endian sono processori tipo Motorola 68k e PowerPC mentre Little endian Intel x64 e Apple Silicon*
> 
> Quindi **presentazione dei dati != dati rappresentati da una applicazione** e questa sintassi definisce i dati durante il trasferimento e stabiliscono come poi dovranno essere transformati
>

> [!NOTE]
> 
> ***Apache Avron vs Protobuf vs Json vs ASN***
> 
> - Apache Avro™ è un **formato di serializzazione dei dati**. Serve quindi per rappresentare e scambiare dati strutturati in modo efficiente, specialmente in contesti distribuiti come **Big Data** e **streaming**
> 
> ```json
> {
>   "type": "record",
>   "name": "Persona",
>   "fields": [
>     {"name": "nome", "type": "string"},
>     {"name": "eta", "type": "int"}
>    ]
> }
> 
> ```
> 
> - Google Protocol Buffers ( **Protocol Buffers**, o **Protobuf** ) è un **formato di serializzazione binario** sviluppato da Google. È progettato per essere **più efficiente, veloce e compatto di JSON e XML**, ed è spesso usato per comunicazioni tra servizi in sistemi distribuiti
> 
> ```json
> syntax = "proto3";
> message Persona {
>   string nome = 1;
>   int32 eta = 2;
> }
> ```
> 
> - JSON è un formato di **scambio dati leggero** e **testuale**. JSON è principalmente utilizzato per scambiare dati tra client e server, specialmente nelle **API web**, ed è **indipendente dal linguaggio di programmazione**. È un formato molto popolare per la comunicazione su **HTTP/HTTPS**, in particolare nelle **API REST**
> 
> ```json
> {
>   "nome": "Mario",
>   "eta": 25
> }
> ```
> 
> - ASN.1 ( Abstract Syntax Notation One ) è un **linguaggio di descrizione dei dati** standardizzato usato in telecomunicazioni, crittografia e nelle reti
> 
> ```json
> Persona ::= SEQUENCE {
>    nome UTF8String,
>    eta INTEGER
> }
> ```
> 
> I vari protocolli **trasmettono i dati serializzati**, ma **non impongono** un formato specifico. Ad esempio =>
> 1. Un'API REST può usare **JSON su HTTP**
> 2. Un servizio Kafka può usare **Avro su TCP**
> 3. Un sistema di autenticazione può usare **ASN.1 nei certificati X.509 su TLS**
>

> [!TIP]
> 
> ***Abstract Syntax and Transfer Syntax***
> 
> ![[Screenshot 2025-03-06 164815.png]]
> 
> **ASN.1** consente diverse regole di codifica che trasformano la sintassi astratta in un flusso di byte adatto al trasferimento. Definiamo quindi la <mark>**BER**</mark> ( _Basic Encoding Rules_ #BER ) definisce la mappatura tra la sintassi astratta e quella di trasferimento quindi codifica i tipi di dato ASN in stream di byte ( *nei browser è* #DER )
> > - Le applicazioni normalmente utilizzano una sintassi locale a seconda del linguaggio di programmazione utilizzato
> 
> ![[Screenshot 2025-03-07 183139.png]]
> ![[Screenshot 2025-03-07 183242.png]]
> 
> > *l'object identifier da un significato a degli oggetti: se voglio dire che una stringa è un nome di persona userò l'oi che dice che quella stringa rappresenta un nome*
> 
> ![[Screenshot 2025-03-07 184203.png]]
> 
> - Come possiamo vedere, qui definiamo una sintassi astratta per i tipi, non andiamo a specificare quanti bit occorrono poiché quello è il compito della sintassi di trasferimento che verrà usata
> 
> L'<mark>**ISO Registration Tree**</mark> è un albero per identificare univocamente oggetti, tipi di dato, protocolli, documenti, ... insomma è un modo per standardizzare tutto
> 
> Il percorso dal nodo radice al nodo foglia è l'**Object identifier**. La struttura è un albero proprio perché con il tempo aggiungo livelli ai protocolli, certificati, standard etc .... che non devono cambiare tutta la sintassi ( *le curve ellittiche nei certificati prima non cerano* )
> 
> ![[Screenshot 2025-03-07 183915.png]]
> > *1.3.6.1 = internet*
> 
> ![[Screenshot 2025-03-07 184417.png]]
> ![[Screenshot 2025-03-07 184514.png]]
> 
> Il protocollo #SNMP utilizza proprio questa sintassi per definire i suoi dati =>
> 
> ![[Screenshot 2025-03-07 184558.png]]
> ![[Screenshot 2025-03-07 184628.png]]
> 
> Quindi possiamo dire che l'object identifier è un **riferimento assoluto** che la macchina deve essere in grado di risolvere
> 
> ![[Screenshot 2025-03-07 190102.png]]
> 
> ogni stringa è la traduzione di un object identifier tipo 1.6.3.1.1.2.3 = WWW-HIR ( *non è vero* )
> 
> Per rispondere alla domanda fatta all'inizio di questo file =>
> - BER definisce anche la direzione di trasmissione del flusso di bit oltre alla codifica dei tipi di dato ASN.1
> 
> ![[Screenshot 2025-03-07 194742.png]]
> ![[Screenshot 2025-03-14 181844.png]]
> 
> E per trasformare la struttura dati scritta in ASN per comunicare tra programmi in una struttura dati del mio linguaggio ci sarà un compilatore che 
> 
> ![[Screenshot 2025-03-07 195012.png]]
> 

---