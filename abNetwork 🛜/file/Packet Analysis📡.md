# Packet Analysis📡

#packet #analysis #SPAN #RSPAN #ERSPAN #VALN #full_duplex #half_duplex

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
> 1. **Eliminazione del rischio di perdita pacchetti**
> 2. **Monitoring dei pacchetti**
> 
> Gli <mark>**svantaggi**</mark> dei tap sono =>
> 3. **Ho bisogno di due interfacce per catturare il traffico**
> 4. **Costi aggiuntivi per i tappi**
> 5. **Non si può leggere il traffico prima dello switch** ( *host to host* )
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
> L’architettura su cui si basa sFlow è composta da un 
probe che 
cattura 
il traffico, da un 
collector che riceve i pacchetti esportati 
ed 
un’applicazione che riceverà i report. Il 
probe 
sFlow opera secondo principi diversi da quelli del probe NetFlow, poiché prende un pacchetto da 
ciascuna porta dello switch su cui si trova in base al sampling rate che si sta applicando. Una 
volta presi i pacchetti, 
li incapsula e li invia al collector, quindi il probe non analizza il 
pacchetto ma si limita ad inviarlo al collector
> 

---