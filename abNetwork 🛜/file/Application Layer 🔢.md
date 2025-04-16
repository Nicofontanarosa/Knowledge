# Application Layer 🔢

#application #ISO #ISO/OSI #TCP #TCP/IP #networks #HTTP #DNS #socket #serversocket #client_server #client 

> [!NOTE] 
> 
> ***What are network applications ?***
>
> <p align="center"><img src="img/Immagine 2022-03-01 174017.jpg" /></p>
>
> Le applicazioni di rete sono applicazioni **formate da processi distribuiti comunicanti**, ovvero **programmi** eseguiti dai vari **host** di una rete che comunicano tra di loro
> 
> All'interno dello stesso host ci sono processi che possono comunicare tra di loro tramite una **comunicazione inter - processo** e alla base della comunicazione a livello applicativo abbiamo il **messaggio**
> 
> <p align="center"><img src="img/Screenshot 2024-06-23 191351.png" /></p>
> 
> Durante la comunicazione il livello applicativo si interfaccia ai vari sottolivelli tramite interfacce. Le **API Application Programming Interface** sono un insieme di regole che consentono l'utilizzo di risorse
>

> [!IMPORTANT]
> 
> ***Application Protocol***
> 
> Definisce i **tipi di messaggio** scambiati a livello applicativo ( *es request o post* ) , la loro **sintassi e semantica** e le **regole** per determinare quando e come un processo invia e riceve messaggi
> 
> In questo livello quindi, ho programmi applicativi che girano su host differenti scambiandosi messaggi. Questi 2 programmi possono non essere entrambi capaci di offrire servizi. A seconda del modello di interazione, distinguiamo diversi **paradigmi di comunicazione** =>
> 
> 1. **Client - Server** => Un numero limitato di processi server che offrono un servizio ad un numero limitato di processi client e il server è in attesa di ricevere delle richieste di connessione
> 2. **Peer - to - peer** => Si parla di peer che possono sia offrire che richiedere servizi
> 3. **Hybrid**
> 4. **Publish - Subscribe** => - I **produttori** di messaggi ( _publisher_ ) non inviano direttamente ai consumatori (_subscriber_), ma pubblicano su un **canale o topic**. I **consumatori** si iscrivono ai topic di interesse e un **broker centrale** gestisce la distribuzione
> 
> etc ...
> 

> [!TIP]
> 
> ***Client - Server model & Socket***
> 
> 1. Si inizia con il **contattare il server** per richiedere un servizio 
> 2. Il server **fornisce il servizio richiesto** pertanto, *dovrebbe* essere sempre attivo
> 
> - *Per il Web abbiamo il client che possiede un browser, Il server Web in ascolto, un formato standard per la comunicazione online di risorse e un protocollo per scambiarsi messaggi quale HTTP o HTTPS*
> - *Per la posta elettronica abbiamo uno standard per i messaggi, un programma di scrittura e lettura sul client, un server che immagazzina la posta e i protocolli SMTP, POP3 etc...*
> 
> quindi **Il protocollo è solo una parte di tutta l'applicazione di rete** che comprende standard per i messaggi, dispositivi specifici etc...
> 
> Il **Socket** è l'API utilizzata dal livello applicativo per comunicare su internet e livelli sottostanti
> 
> <p align="center"><img src="img/Screenshot 2024-06-23 212809.png" /></p>
> 
> L'applicazione invia e riceve messaggi tramite il **socket** che funge da **file descriptor** -> Esso rappresenta un **punto di accesso** a una **struttura dati** più complessa gestita dal sistema operativo, contenente informazioni sul tipo di *connessione, indirizzi, porte, stato, etc ...*
> 
> <p align="center"><img src="img/Screenshot 2024-06-24 110909.png" /></p>
> 
> Il socket è composto da 2 parti => **Indirizzo IP :: Porta** con l'indirizzo IP formato da 32 bit =>
> 
> - 10100100.11110110.10100111.01011010 => 4 ottetti => 164.246.167.90
> - :: 5673 => 16 bit => 164.246.167.90::5673
>
> <p align="center"><img src="img/Screenshot 2024-06-24 112010.png" /></p>
> 
> ---
> 
> Quindi, usando ad esempio il protocollo di trasporto TCP, il client e server si comportano rispettivamente in questo modo =>
> 
> <p align="center"><img src="img/Screenshot 2024-06-24 112618.png" /></p>
> <p align="center"><img src="img/Screenshot 2024-06-24 112643.png" /></p>
> 
> Il **client** si connette al **server** ed invia dati, mentre il **server** deve ->
> - **creare il socket**
> - **associare** il socket a un indirizzo IP e una porta con la **`bind()`**
> - mettersi in **ascolto** con la **`listen()`**
> - **accettare** una connessione con **`accept()`**, che **blocca** il server in attesa di richieste da parte dei client
> 
> Ricordiamo che la *connection è una quadrupla ip_server::porta_server|ip_client::porta_client*
> 
> La coppia di processi client - server utilizza i servizi offerti a livello di trasporto per la comunicazione usando i protocolli **TCP** ( *connection - oriented basato su stream* ) o **UDP** ( *connection - less basato su messaggio* ) 
> 
> Nel modello client-server la modalità di comunicazione può essere di due tipi =>
>
>- **Unicast** -> Il server comunica con un solo client per volta, accettando una sola richiesta di connessione per volta
>- **Multiclient** -> Al server possono essere connessi e comunicare più client contemporaneamente
>

> [!WARNING]
> 
> ***Protocollo di trasporto TCP vs UDP***
> 
> Servizio ***TCP*** => - veloce + sicuro
> - Connection - oriented cioè setup richiesto tra client e server
> - Trasporto affidabile tra mittente e destinatario
> - Controllo del flusso e controllo della congestione
> - Non offre garanzie di *timing* e di *ampiezza di banda*
> 
> ---
> 
> Servizio ***UDP*** => + veloce - sicuro 
> - Connection - less cioè non richiede una fase di setup
> - Trasporto non affidabile
> - Non c'è controllo di flusso
> - Non c'è controllo di congestione
> - Non offre garanzie di *timing* e di *ampiezza di banda*
> 
> ---
>
> <p align="center"><img src="img/Screenshot 2024-06-24 132502.png" /></p>
> <p align="center"><img src="img/Screenshot 2024-06-24 132841.png" /></p>
> 

---