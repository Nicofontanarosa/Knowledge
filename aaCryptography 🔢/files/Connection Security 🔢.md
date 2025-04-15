
 #identification #authentication #digital_signature #MAC #CBC #Zero_Knowledge

> [!key]- Identification, Authentication & Digital Signature
> 
> In origine i metodi crittografici sono stati sviluppato per garantire la confidenzialità delle comunicazioni ma con la diffusione delle reti sono emerse tre **funzionalità importanti** che sono oggi **richieste ai protocolli crittografici** e sono -> 
> 
> 1. ***Identificazione*** -> Un sistema deve essere in grado di accertare l'identità di un utente che richiede di accedere ai suoi servizi ( login con password verificata con la cifratura hash )
> 
> 2. ***Autenticazione*** -> Richiede di rispettare il punto primo e garantire l'***integrità*** del crittogramma ricevuto -> non deve essere stato modificato o sostituito durante la trasmissione
> 
> 3. ***Firma Digitale*** -> Funzionalità più complessa poiché deve rispettare tre requisiti :
> 
>	1. Il mittente non deve poter negare di aver inviato un messaggio m
>	2. Il destinatario deve poter accertare l'identità del mittente e l'integrità del crittogramma ricevuto ( punti 1 e 2 )
>	3. Il destinatario non deve poter sostenere che un messaggio m' != m sia quello inviatogli dal mittente
>
> 	Questi tre **requisiti devono** inoltre **essere verificabili** da una terza parte ovvero dal **giudice** ovvero certificare la comunicazione
> 
> ![[Screenshot 2023-12-30 095344.png]]
> 
> ![[Screenshot 2023-12-30 095415.png]] 
> 
> ![[Screenshot 2023-12-30 095441.png]]
> 
> ![[Screenshot 2023-12-30 095504.png]]
> 
 
> [!radar]- Note
> 
> Queste **tre funzionalità** sono utili per **contrastare** possibili **attacchi attivi di tipo man in-the-middle**
>

> [!radar]- Login & Password
> 
> Quando un utente U fornisce per la prima volta una password P =>
> 
> - Il sistema **associa ad U un S ( seme )** prodotto da un generatore pseudocasuale
> - **Q = H ( PS )** l' hash della password associata al seme
> 
> Conoscere solo il seme non va a discapito della sicurezza poiché bisognerebbe ricavare la password dall' immagine hash memorizzata
> 
> Si usa il **seme** perché =>
> 
> - La scelta di **S** **impedisce** al **crittoanalista** di capire se + utenti hanno scelto la **stessa password** guardando le immagini hash
> - Se le password sono scelte da un dominio non sufficientemente ampio, il crittoanalista potrebbe **enumerarle tutte**, ecco perché l' aggiunta di S
> - Rende **inutile** il **preprocessing** ( *enumerazione delle password* )
> - Fa richiedere un **brute force** per scoprire la password
>
 
> [!radar]- User Identification
> 
> Ma se il canale è insicuro, la password può essere intercettata durante la sua trasmissione in chiaro => Si utilizza chiave pubblica e chiave privata <e, n>, \<d> ->
> 
> - Il sistema genera un numero casuale r < n e lo invia a U
> - U calcola f = r^d mod n ( *firma di U su r* ) e lo spedisce a S
> - S verifica la correttezza del valore calcolando r = f^e mod n ( S identifica l'utente U ) -> **Modalità Autenticazione**
> 
> **Grosso problema** è che U deve applicare la sua chiave privata ad una sequenza generata da un esterno => potrebbe essere stata scelta di proposito per calcolare d => **Zero Knowledge**
>

> [!radar]- Authentication
> 
> MITT e DEST concordano una chiave segreta k condivisa e una funzione A. Adesso il MITT ->
> 
> - Allega al messaggio un **MAC** ( **Message Authentication Code** ) allo scopo di garantire la provenienza e l' integrità del messaggio A(m, k) oppure H(mk) ( il MAC e ottenuto basandosi su cifrari asimmetrici o simmetrici o Hash )
> - Spedisce la coppia <m, A(m, k) > in chiaro oppure cifra m e manda < C(m, k'), A(m, k) >
> 
> Adesso il DEST ->
> 
> - Entra in possesso di m ( eventualmente dopo averlo decifrato )
> - Conoscendo A e k calcola A(m, k) oppure H(mk) e ne garantisce l'integrità
> 
> Come **MAC** si può usare anche il **blocco finale del crittogramma** se si usa la modalità **CBC**
> 

---
#hash #one-way #fingerprint #SHA #MD5 #Rivest #Ron_Rivest #RIPEMD-160 #keccak
## Hash Functions

> [!key]- Definition of Hash Functions One Way
> 
> Una funzione hash f: X --> Y vede codominio e dominio finiti . n = |X| >> m = |Y|. Queste funzioni sono usate per calcolare la ***fingerprint*** y = f(x) ∈ Y
> 
> Per rappresentare y servono **log(2) m** bit << log(2) n e stessi elementi ∈ in X formeranno la stessa y ∈ Y --> 
> 
> 1. Due elementi estratti a caso da X hanno probabilità 1/m ( m numero di sottoinsiemi X1, X2, ..., Xm da dove estraggo i miei elementi a caso ) di avere la stessa immagine ∈ Y
> 2. Se X è un insieme di interi, **due elementi con valori simili devono avere immagini diverse** -> La **funzione hash** usata dovrà implementare un modo per **gestire** le possibili **collisioni**> 
> 

> [!key]- Hash Functions One Way used in Criptography
> 
> 1. E' computazionalmente facile calcolare f(x) per ogni x ∈ X
> 2. ***One-Way*** -> E' computazionalmente difficile trovare x ∈ X conoscendo y ∈ Y . f(x) = y
> 3. ***Claw-Free*** -> E' computazionalmente difficile determinare una coppia x, x' ∈ X . f(x) = f(x') = y ∈ Y 
> 
> ![[Screenshot 2023-11-11 111117.png]]
> 

> [!key]- Hash Functions SHA
> 
> **SHA** ( **S**ecure **H**ash **A**lgorithm ), nota come **SHA0**, fu proposta dal NIST nel **1993** per competere con un altra funzione hash chiamata **MD5** che fu presto **ritirata** a causa di un **punto di debolezza** scoperto al suo interno. La **NSA** la sostituì con **SHA1** nel **1995** che opera con **sequenze** lunghe fino a **2^64 - 1 bit** producendo **immagini** di **160 bit**. Tali sequenze in input sono state allungate nella versione **SHA-2** pubblicata nel **2001** A causa di **attacchi** puramente **teorici non è più lo standard dal 2010**
> 
> Oggi quindi usiamo le funzioni ***SHA-3*** per l'alta sicurezza ( **Algoritmo Keccak** rilasciato nel **2015** sviluppato da un team di italiani e belgi ) ma nella vita di tutti i giorni continuiamo ad usare SHA1
> 
> La struttura dello SHA-1 è molti simile alle altre strutture e comprende ->
> 
> 1. Un blocco di 160 bit in input contenuto in un **buffer di 5 registri da 32 bit** ( ricordiamo che SHA-1 opera però su 2^64 - 1 bit quindi esegue questa operazione più volte )
> 
> 2. Il messaggio m viene concatenato con una sequenza di di **padding** per renderne la lunghezza un multiplo di **512 bit**
> 
> ![[Screenshot 2023-11-11 114739.png]]
> > Struttura di massima del ciclo i-esimo di SHA1
> 
> 1. **F** è una **funzione non lineare** 
> 2. **<<< n** denota uno **shift ciclico verso sinistra di n posti** con n che varia per ogni iterazione che faccio
> 3. Il quadrato è **l'addizione modulo 2^32** 
> 4. **Kt** è una **costante**
> 5. **Wt** è un** blocco di 32 bit** ottenuto tagliando e rimescolando i blocchi del **messaggio**
> 

> [!quote]- Hash Functions MD5
> 
> Prima di MD5 vennero pubblicati **MD2, MD3, MD4** ma **MD1** **non** fu mai **pubblicato**
> 
> **MD5** ( **M**essage **D**igest version **5** ), costruisce un immagine di 128 bit per un qualunque messaggio, proposta da **Rivest nel 1992** e usata per un decennio prima di scoprire degli attacchi legati alla terza proprietà ( *claw-free* ). Oggi viene **usata** per la usa semplicità **fuori dalla crittografia** 
> 
> Evoluzione del MD5 sviluppata nel 1995 ed esente dai difetti di MD5 è la funzione ***RIPEMD - 160*** ( **R**ace **I**ntegrity **P**rimitive **E**valuation **M**essage **D**igest ) che produce **immagini di 160 bit**
> 

---
#connection_security #authentication_systems
# Authentication Systems

Spesso non serve la segretezza ma basta l'autenticazione e la certezza che il messaggio non venga modificato 

> [!key]- Definition of Authentication System
> 
> Per autenticare un messaggio basta "imbustarlo" all'interno di un "contenitore digitale" che prende il nome di ***firma digitale*** ( smart-card, token USB o Business Key, dotate di PIN )
> 
> La firma digitale ci permette di -->
> 
> - Riconoscere se il documento è stato modificato o meno
> - Attestarne la validità
> - Risalire all'identità del firmatatio
> 
> Il formato dei file firmati digitalmente è il **.p7m / pkcs#7** ( Dal 1999. Oggi usate anche le firme in formato pdf e xml )
> 
> ![[Immagine 2022-02-16 160605.jpg]]
> 

---
#smartcard #rom #promt 
## Smart Card

> [!key]- Definition of Smart Card
> 
> La smartcard è una carta plastificata, su cui è integrato un microchip programmabile, con una ***ROM*** che contiene il sistema operativo e i programmi " fissi ", una ***PROM*** che contiene il numero seriale della smartcard, un'altra ROM che contiene i dati del proprietario e i meccanismi di protezione che ne evitano la clonazione
> 
> ![[Immagine 2022-02-20 111703.jpg]]
> 

---
#digital_signature #man-in-the-middle
## Digital Signature

> [!radar]- Note
> 
> *Deve avere una forma che dipenda dal documento su cui viene apposta
> Per progettare firme digitali si possono usare sia i cifrari simmetrici che asimmetrici*
>

> [!key]- Definition of Digital Signature
> 
> Si utilizza una ***funzione hash*** attraverso la quale si calcola una stringa identificativa del messaggio, detta ***fingerprint*** o ***message digest***. Il calcolo di questa fingerprint risulta essere vantaggioso poichè nella fase di crittazione andremo ad operare su pochi byte invece che su tutto il messaggio ( è ***irreversibile*** )
> 
> ![[Immagine 2022-02-20 112328.jpg]]
> 
> Quando A vuole mandare a B un mesaggi -->
> 
> - Si estrae il **fingerprint**
> - Si cifra l'impronta digitale con esempio la smartcard
> - L'impronta crittografata viene accodata al messaggio in chiaro
> - Al messaggio vine anche accodato il certificato del firmataio
> 
> ![[Immagine 2022-06-28 091452.jpg]]
> 

> [!key]- Create a Digital Signature with DH
> 
> U: utente | Kpriv e Kpub chiavi di U | C e D funzioni di cifratura e decifratura di un cifrario asimmetrico
> 
> 1. U genera la firma f = D(m, Kpriv)
> 2. U spedisce all' utente V la tripla <U, m, f>
> 
> 1. V riceve <U, m, f> e verifica l' autenticità della firma f facendo C(f, Kpub) = m
> Questo funziona poiché => ***C (D (M)) = D (C (M)) = m*** => C e D commutative
> 
> Questo procedimento garantisce l' **autenticità** di U, l' **integrità** del messaggio m e la **non riutilizzabilità** della firma per un' altro messaggio.***Il protocollo è definito per un particolare utente ma non per un particolare destinatario***
> 

> [!key]- Create a Digital Signature with Sign & Encrypt
> 
> L' idea è quella di cifrare la firma, ovvero firmare il documento e metterlo all' interno di una lettera per un particolare destinatario
> 
> 1. U genera la firma f = D (m, Kupriv)
> 2. Calcola c = C (f, Kvpub)
> 3. U spedisce la coppia < U, c > a V
> 
> 1. V riceve < U, c > e decifra il crittogramma D (c, Kvpriv) = f
> 2. Cifra f facendo C(f, Kupub) = m
> 
> Garantisce l' autenticità di U e di V
> 

> [!key]- Sign & Encrypt Algorithm
> 
> Utilizziamo l' RSA con ->
> 
> - \<du>, <eu, nu> chiavi private e pubbliche di U
> - \<dv>, <ev, nv> chiavi private e pubbliche di V
>
> 1. U genera la firma del messaggio => f = m^du mod nu
> 2. Cifra f con la chiave pubblica di V => c = f^ev mod nv
> 3. Spedisce la coppia <U, c>
> 4. V riceve la coppia e decifra c => f = c^dv mod nv
> 5. Decifra f => m = f^eu mod nu
> 
> E' necessario che nu <= nv perché risulti f < nv => si stabilisce un H pubblico grande ad esempio 2^1024 e le chiavi di firma siano < H mentre le chiavi di cifratura siano > H
> 

> [!danger]- Attack 1
>
> Si può **cifrare la firma** con un **cifrario simmetrico** poiché così facendo chi riceve sa chi ha scritto e firmato il messaggio e chi lo ha cifrato ed inviato. Con una cifratura asimmetrica, chi riceve sa solo chi ha scritto e firmato il messaggio
> 

> [!terminal]- Example
> 
> **Bob, destinatario del messaggio originale, potrebbe decifrare il crittogramma, ottenere la firma del messaggio spedito da Alice, cifrarla con la chiave pubblica di Charlie e inoltrare il messaggio a Charlie firmato da Alice**
> 
> **Se invece uso un cifrario simmetrico, Bob, destinatario del messaggio originale, potrebbe decifrare il crittogramma, ottenere la firma del messaggio spedito da alice, ma poi non può cifrarla poichè non ha una chiave condivisa con charlie**
> 

> [!danger]- Attack 2
>
> *Supponiamo che U invii una risposta automatica ACK a MITT ogni volta che riceve un messaggio m. Il segnale di ACK è il crittogramma della firma di U su m. Un crittoanalista X può intercettare il crittogramma c e lo inviandolo lui, facendo credere di essere lui l' autore. Questo comporta* =>
> 
> - MITT ( V ) invia c a U -> f = D(m, Kvpriv), c = C(f, Kupub)
> - Il crittoanalista X intercetta c e lo invia al posto di V
> - U decifra c -> f = D(c, Kupriv)
> - U verifica la firma -> m' = C(f, Kxpub) poiché U riceve il crittogramma da X
> - U ottiene quindi un messaggio m' != m privo di senso ma manda lo stesso l' ACK a X composto da ->
> 	- f' = D(m', Kupriv)
> 	- c' = C(f, Kxpub)
> - Adesso X può risalire al messaggio originario con semplici calcoli =>
> 
> **D(c', Kxpriv) = f' => C(f', Kupub) = m' = C(f, Kxpub) => D(m', Kxpriv) = f = D(m, Kvpriv) => C(f, Kvpub) = m**
> 
> => Bisogna bloccare gli ACK automatici
> 

> [!key]- Definitive Protocol v. 1
> 
> Non si firma il messaggio ma il suo Hash ->
> 
> - U calcola H(m) e genera f = D(H(m), Kupriv) e c = C(m, Kvpub) e spedisce a V <U, c, f>
> - V decifra c: m = D(c, Kvpriv) e calcolo H(m)'
> - V controlla la f: H(m) = C(f, Kupub) 
> - Se H(m)' = H(m) => Tutto ok
> 

> [!danger]- Attack 3
>
> Usando chiavi pubbliche e private un crittoanalista può intromettersi prima che avvenga lo scambio dei crittogrammi => **Man in-the-Middle**
> 

---
#digital_certificates 
## Digital Certificate

> [!radar]- Note
> 
> *Se A ( mittente ) cifra il messaggio con la sua chiave privata e B ( destinatario ) lo decritta con la sua chiave pubblica, come fa B ad essere sicuro che la chiave gli sia pervenuta effettivamente da A ? 	*
> 

> [!key]- Definition of Certification Authority
> 
> La soluzione per il problema descritto in precedenza consiste nel certificare l'identità del mittente, attraverso un ***certificato digitale***. Sostanzialmente un certificato digitale è un file che **lega** la **chiave pubblica** con le **informazioni** dell'**ente** e del certificato stesso ( ha validità di circa 2 anni )
> 
> Questo certificato a sua volta deve essere validato da un ***CA Certification Authority*** che garantisce l'identità del proprietario del certificato firmandone le chiavi pubbliche e private. Anch' essa possiede una Kpub nota utilizzata per verificare la firma sui vari certificati e distribuita tramite i sistemi operativi ( l'utente mantiene localmente Kpub di CA e altre CA )
> 
> Esistono **CA** **organizzate ad albero** e la verifica dei certificati si svolge attraverso una catena di CA
> 
> L'unico controllo On-line è quello di vedere se ci sono CA che sono stati revocati
> 
> Esistono due tipi di Certificate Authority -->
> 
> - ***Pubbliche ( Trusted )*** --> Enti riconosciuti pubblicamente su internet
> - ***Private ( Untrusted )*** --> Enti privati che emettono i certificati a serivizi circoscritti
> 
> Le pratiche relative alla identificazione dell'utente prima della emmissione del certificato vengono fatte dalla ***Registration Authority*** . La ***Certification Authority*** invece si occupa del ciclo di vita del certificato
> 
> I formati per il certificato digitale sono -->
> 
> - **PGP / GPG** --> Creazione veloce ed autonoma
> - **X.509** --> Creazione da un ente addetto allo scopo
> 
> L'insieme costituito da utenti e Authority è chiamato --> ***PKI ( Public Key Infrastructure )*** infatti la CA mantiene un archivio di chiavi pubbliche sicuro, accessibile a tutti e protetto da attacchi in scrittura non autorizzati
> 
> Ogni certificato deve contenere almeno -->
> 
> - **Il formato**
> - **Numero seriale**
> - **Specifiche algoritmo utilizzato dalla CA**
> - **Periodo di validità**
> - **Nome dell'utente**
> - ***Il nome / indirizzo del server*** --> Affinchè il client possa confrontarlo con il nome della macchina a cui è collegato
> - ***La chiave pubblica del server***
> - ***Il nome dell'autorità certificante***
> 

> [!terminal]- Example
> 
> **Alice richiede a CA la chiave pubblica di Bob e la CA risponde inviando ad Alice il Certificato Digitale di Bob che poi controlla e ne verifica la correttezza**
> 

> [!key]- Definitive Protocol v. 2
> 
> - U si procura il certificato di V
> - Calcola H(m) e genera f = D(H(m), Kupriv)
> - Calcola c = C(m, Kvpub) ricavata dalla CA
> - Spedisce a V <certU, c, f> 
> - V riceve la tripla e verifica certU utilizzando la propria Kcapub
> - Decifra il crittogramma m = D(c, Kvpriv) 
> - Verifica l'integrità e l'autenticità del messaggio H(m) = C(f, Kupub) e confronta H(m)
> 

---
#e-invoicing
# E-Invoicing

La fattura commerciale in formato digitale prende il nome di **fatturazione elettronica**. Questa fattura contiene un ***riferimento temporale*** e la ***firma digitale*** emessa da un'***ente certificatore***. Questo garantisce l'integrità dell'informazione ma non l'effettiva lettura ( stesso concetto della ***PEC*** )

---
#security #connection_security 
## Connection Security

> [!warning]- Problem
> 
> I protocolli, essendo insicuri, necessitano di sistemi di protezione applicabili nei livelli di sessione o di rete =>
> 
> ![[Immagine 2022-03-01 173637.jpg]]
> 

---
#SSL #cipher #hybrid_cryptographic #netscape #handshake #reply_attack
## SSL Secure Socket Layer

> [!key]- Definition of SSL Protocol
> 
> Il protocollo ***SSL*** ( *prima versione rilasciata nel 1994 da Netscape* ) garantisce la sicurezza del collegamento Internet ( mediante protocollo HTTP ) mediante tre funzionalità -->
> 
> - ***Privatezza del collegamento o Confidenzialità*** --> Riservatezza del collegamento mediante algoritmi a chiave simmetrica e non -> cifrari ibridi
> - ***Autenticazione*** --> Introduzione di certificati digitali e algoritmi a chiave asimmetrica
> - ***Affidabilità*** --> Nel livello di trasporto viene introdotto il **MAC** ( Message Authentication Code ) che utilizza funzioni hash sicure come SHA e MD5
> 
> Molto spesso viene associato ad altri servizi quali ***HTTPS*** ( PORT --> 443 ), ***SMTPS / POPS / IMAPS*** ( PORT --> 465, 995, 993 ) cioè l'unione dei protocolli applicativi con il protocollo SSL
> 

> [!key]- Structure of SSL Protocol
> 
> SSL è organizzato su **due livelli** ->
> 
> 1. ***SSL Record*** -> Livello + basso -> E' connesso direttamente al **livello** di **trasporto** è ha l'obiettivo di **incapsulare** i **dati provenienti dai livelli superiori** assicurando confidenzialità e integrità
> 
> *Realizza fisicamente il canale*
> 
> 2. ***SSL Handshake*** -> Livello + alto -> E' connesso al **livello di sessione** e permette all'utente e al sistema di **autenticarsi**, della fase di **handshaking** dei vari **algoritmi usati, chiavi, MAC e parametri** garantendo l'**affidabilità e l'autenticità**
> 
> *L'SSL Record fa viaggiare i pacchetti incapsulati a blocchi all'interno dell'SSL Handshake e viceversa utilizza i sistemi crittografici forniti dal livello SSL Handshake per criptare i messaggi e inviarli ai livelli sottostanti*
> 

> [!key]- Comunication in SSL Protocol
> 
> La comunicazione avviene tra **S** ( server ) e **U** ( utente o client ) ->
> 
> 1. Si **Identificano** a vicenda
> 2. Si **accordano**  sugli **algoritmi** da usare
> 3. Si scambiano un **master - secret**
> 
> ![[Screenshot 2023-11-21 092909.png]]
> 
> 1. U manda un messaggio chiamato **client hello** con cui richiede di voler creare una comunicazione cifrata con SSL specificando le "prestazioni" di sicurezza che vorrebbe instaurare + una sequenza di byte casuali. In particolare ->
> 
> - U specifica la versione del protocollo SSL 
> - Un elenco di algoritmi di compressione
> - Una **Cipher Suite** che comprende i meccanismi di cifratura quali ->
> 
> ![[Screenshot 2023-11-21 101317.png]]
> 
> Questo è un esempio che prevede **RSA** per lo scambio di chiavi di sessione, **2TDES_EDE_CBC** per la cifratura simmetrica dei blocchi del messaggio e **SHA1** come funzione hash one - way per il calcolo del MAC
> 
> 2. S riceve il client hello e selezione un algoritmo di compressione e una Cipher Suite che anche lui può supportare e la comunica al client con tramite un **Server Hello** che comprende anche una sequenza di Byte casuali 
> 
> *Se U non riceve il Server Hello interrompe la comunicazione*
> 
> 3. **S invia** il proprio **certificato digitale** a U e S, se presenta dei servizi che vuole proteggere, chiede anche ad U di presentare il proprio certificato digitale ( la maggior parte degli utenti non presenta un certificato digitale ) quindi S dovrà accertarsi dell' identità di U in un secondo momento ( *Se S non presenta un certificato si scambiano la chiave tramite DH ma vulnerabile ad attacchi di tipo Man-in-the-Middle* )
> 
>4. **S invia** il pacchetto **Server Hello done**
>
>5. **U** esegue le **verifiche** sul **certificato digitale** e sulla CA
>
>6. **U costruisce** il **pre - master secret** costituito da una nuova sequenza di byte casuali che cifra con il cifrario a chiave pubblica della cipher suite, combinato con alcune stringhe note ad U e i byte casuali presenti nel Client Hello e Server Hello ( *in chiaro* ), applica a tutte queste stringhe delle funzioni hash e si ottiene il **master - secret**
>
>7. **S riceve** il **pre - master secret** da decifrare e **calcola** il **master - secret** mediante le stesse operazioni di U
>
>8. Se **U** non possiede il **certificato** e gli viene richiesto => il sistema interrompe l'esecuzione ( *a meno che non accetti altri dati per l'autenticazione tipo carta di credito* ) altrimenti invia il certificato e il master secret che egli stesso ha generato con anche la **SSL-history**. In presenza di anomalie la comunicazione con U viene interrotta 
>
>9. U invia un messaggio concatenato con il master - secret e i messaggi di handshake scambiati fino a quel momento + l' identità del mittente. La stringa ottenuta viene trasformata tramite funzioni hash SHA-3 o MD5. Questo messagsgio è chiamato **finished message** ed è un' ulteriore controllo per evitare **attacchi attivi**
>
>10. S riceve questo messaggio e lo invia a U concatenandolo a se stesso e usando funzioni hash ( *U può controllare il messaggio ricevuto con lo stesso calcolato da lui* )
>
>

> [!radar]- Master - Secret
> 
> Il master - secret viene usato da U e da S per costruire una propria **tripla** contenente la chiave segreta da adottare nel cifrario simmetrico, la chiave da adottare in una funzione hash per la costruzione del MAC e la sequenza di inizializzazione per cifrare in modo aperiodico messaggi molto lunghi ( *valore iniziale del CBC* )
> 
> **proprietà** di queste triple è che sono **diverse** tra U e S
>

> [!key]- Data Transport in SSL record
> 
> Il canale realizzato dall' SSL Record può essere utilizzato per avviare la comunicazione una volta finita la fase di ***Handshake***
> 
> I dati sono **frammentati** in blocchi *numerati, compressi, autenticati con l'aggiunta di un MAC, cifrati simmetricamente e trasmessi* 
>

> [!terminal]- Security
> 
> Un crittoanalista non può riutilizzare i **byte casuali** ( **reply attack** ). I byte generati dovranno essere davvero casuali altrimenti il sistema crolla. La sequenza più importante è quella generata da U nella costruzione del pre - master - secret poiché è quella che non comunica se non con un cifrario asimmetrico
> 
> Per **disturbare** un **blocco** cifrato del messaggio ha bisogno di costruire il **MAC** ma questo viene calcolato applicando una funzione hash alla concatenazione di ->
> 
> - Contenuto del blocco
> - Numero del blocco
> - la k del MAC poiché ricordiamo il MAC ha la forma H(mk) oppure A(m,k) con A funzione di cifratura asimmetrica
> - Stringhe note e fissate a priori
> 
> Data che il MAC viene cifrato insieme al blocco del messaggio, un **crittoanalista dovrebbe forzare** prima la **chiave simmetrica** di cifratura => A invia a B un blocco cifrato con una chiave simmetrica e questo blocco contiene il blocco del messaggio con il MAC relativo
> 
> Il destinatario è **autentico** poiché si utilizzano i **certificati digitali** => A comunica il **pre-master-secret** a B tramite la chiave pubblica del certificato
> 

---
#TLS #transport #webservices #client_server #client 
## TLS Transport Layer Security

> [!quote]- Born of TLS
>
> Basato su SSL, creato da Netscape nel 1995, ha subito un continuo aggiornamento ->
> 
> 1. versione 1.0, 1999
> 2. versione 1.1, 2006
> 3. versione 1.2, 2008
> 4. versione 1.3, 2018
> 
> Permette ad un **client** ( ==*web browser*== ) e ad un **server** ( ==*web site*== ) di stabilire un insieme di chiavi simmetriche condivise da utilizzare per cifrare e autenticare la sessione di comunicazione
> 
> **Permette al client di autenticarsi**
> 

> [!key]- Definition of TLS Protocol
> 
> Il protocollo ***TSL*** è composto da due livelli -->
> 
>- ***TLS Record Protocol*** --> Il livello subito sopra il livello di trasporto per trasferire i dati dell'applicazione. Consiste nel ->
>
>1. C usa **Kc** per cifrare i messaggi che invia a S
>2. S una **Ks** per cifrare i messaggi per C
>
>- ***TLS Handshake Protocol*** --> Si occupa della fase di negoziazione in cui si autentica l'interlocutore e si stabiliscono le chiavi segrete condivise e consiste nel ->
>
>1. **C** possiede un insieme di **chiavi pubbliche di CA** e **S** possiede una coppia **chiave pub / priv** per la **firma digitale** e un **certificato digitale cert** per ogni chiave rilasciata da una CA 
>2. C e S usano **DH** per lo scambio di chiavi
>3. Il primo messaggio che C invia contiene ->
>
>	- Il gruppo G usato che può essere Z\*p oppure una curva ellittica
>	- Il generatore g oppure un punto di base B ed il suo ordine
>	- g^x oppure xB con x casuale
>	- Una sequenza di bit casuali Nc ( **nonce** )
>	- Informazioni sulla **chiper suite**
>
>4. S completa DH inviando m a C che contiene g^y oppure yB con y scelto casualmente e Nc
>5. S calcola K = g^xy oppure K = yxB ed applica una **Key Derivation Function** per estrarre da K le chiavi **Ks, Kc, Ks', Kc'**
>6. S invia la propria chiave pubblica, il certificato e la firma calcolata usando la chiave privata su tutti i messaggi di handshake inviati
>7. **Tutti i dati** inviati sono **cifrati** con **Ks'**
>8. C calcola K nello stesso modo e deriva anche lui le stesse chiavi. Usa Ks' per recuperare chiave pubblica, il certificato e la firma, verificandone autenticità e veridicità 
>9. C calcola il **MAC** dei messaggi handshake scambiati usando **Kc'** e lo invia a S
>10. **Fine** fase di **Handshake** ( *fine utilizzo Kc' e Ks'* )
>
>---
>
>1. ***Handshake Protocol***
>2. ***Change Cipher Protocol***
>3. ***Alert Protocol***
>
>![[Immagine 2022-03-01 194112.jpg]]
>
>Prende i dati dal livello superiore, lisuddivide in blocchi, ne calcola il ***MAC***, cifra il tutto e trasmette il risultato
>
>![[Immagine 2022-03-01 194526.jpg]]
>

> [!radar]- Forward Secrecy
> 
> **Non** si usa **più** un cifrario a **chiave pubblica** per scambiarsi K, **ma DH**. Questo perché bisogna garantire la Forward Secrecy, cioè la **segretezza** delle **chiavi di sessione precedenti** nel caso di un server compromesso, cioè la proprietà dei protocolli di negoziazione delle chiavi che assicura che ==se una chiave di cifratura a lungo termine viene compromessa, le chiavi di sessione generate a partire da essa rimangono riservate== => in DH il valore y viene buttato, con RSA la Kpriv non può essere generata ogni volta
>

> [!terminal]- Difference between TLS & SSL
> 
> - Un **handshake** SSL era una connessione esplicita, mentre un handshake TLS è implicito. Il processo di handshake SSL prevedeva più passaggi rispetto al processo TLS. Rimuovendo passaggi aggiuntivi e riducendo il numero totale di suite di crittografia, TLS ha **velocizzato** il processo
> - TLS ha un tipo di **messaggio di avviso aggiuntivo** chiamato _notifica di chiusura_
> - Sia SSL che TLS utilizzano codici di autenticazione dei messaggi (MAC), una tecnica di crittografia per verificare l'autenticità e l'integrità dei messaggi. SSL utilizza l'algoritmo MD5 mentre TLS utilizza il codice di autenticazione dei messaggi basato su hash **HMAC**
> 
> 

> [!danger]- Problem TLS & CDN
>
> *Perché, se inserisco l'IP di una pagina web invece del suo URL, il browser mi segnala che la connessione non è sicura, mentre utilizzando l'URL non ricevo alcun avviso? Ad esempio, accedendo a `https://131.114.21.42` ottengo un errore di certificato non valido, mentre con `https://www.unipi.it` il certificato risulta valido?*
> 
> **I certificati TLS sono emessi per i domini, non per gli IP** =>
> 
> - Quando un sito web usa TLS (HTTPS), il browser controlla che il certificato fornito dal server corrisponda esattamente al nome del dominio richiesto (es. `www.unipi.it`)
> - Il certificato di `www.unipi.it` è registrato per quel dominio specifico e **non per l'IP** (`131.114.21.42`)
> - Se accedi direttamente con l’IP, il certificato non può essere verificato e il browser mostra un errore di sicurezza
> 
> *Ma perché non creo certificati per indirizzi IP?*
> 
> Semplicemente perché in internet ho molti indirizzi IP uguali / questi indirizzi IP cambiano facilmente
> 
> *Come faccio a scegliere a quale macchina andare?*
> 
> Semplicemente non la scegli tu ma la sceglie il "cash" che dice ai protocolli di routing dove instradare chi ( [[Routing 🎛️]] )
> 
> Per maggiori chiarimenti [qui](https://www.quora.com/Why-does-Google-give-us-one-IP-while-it-has-4-servers)
> 
> Parlando di soldi abbiamo anche gli #ISP che comprano le cache dei grandi fornitori di contenuti online per avvantaggiarsi. I grandi fornitori di contenuti come Youtube e Netflix installano i propri sistemi proprietari di cache dei contenuti presso gli ISP. È a loro vantaggio reciproco perché migliora le prestazioni per i clienti, riduce notevolmente il traffico che lascia l'ISP in peering con altri ISP e riduce notevolmente il traffico in arrivo dalle connessioni Internet primarie ai data center dei provider di contenuti. Non è necessario che l'ISP rompa HTTPS perché la richiesta di contenuto sta andando in una scatola appartenente a quel provider. L'ISP ha una scatola nera nei suoi rack di server. Non possono toccarli e non hanno idea di come funzionino, né se ne preoccupano. I fornitori di contenuti più piccoli collaborano con servizi di caching come #Akamai per ottenere lo stesso effetto
> 
> Se così non fosse allora gli ISP dovrebbero inviare le richieste del client fino al server del fornitore ( *o generare traffico inutile* ) che poi è in grado di risolvere
> 
> Per maggiori chiarimenti cliccare [qui](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwjStMzBsfCLAxWqh_0HHSjtJ4EQFnoECCYQAQ&url=https%3A%2F%2Fwww.reddit.com%2Fr%2Fexplainlikeimfive%2Fcomments%2Fw1on1c%2Feli5_how_do_isps_know_what_youtube_videos_to%2F&usg=AOvVaw39ZoI1Q0hpncBXd0LvHla2&opi=89978449), [qui](https://www.quora.com/How-does-YouTube-distribute-uploaded-videos-across-data-centers-and-cache-locations-Are-videos-pushed-to-regional-data-centers-prior-to-users-explicitly-requesting-them-or-when-the-first-user-requests-content-in-a-region) o [qui](https://aws.amazon.com/what-is/cdn/#:~:text=A%20content%20delivery%20network%20(CDN,network%20or%20content%20distribution%20network)
> 
> Le grandi aziende tipo Amazon, Google etc ... hanno i propri **Content Delivery Network** ovvero una rete di distribuzione di contenuti ( #CDN ) cioè una **rete di server interconnessi che velocizza il caricamento di pagine Web** per applicazioni ad alto contenuto di dati -> ***==Cache==***
> 
> ***IMPORTANTE: L'indirizzo della cache è dell'operatore locale e non dell'azienda per cui abbiamo acquistato le informazioni in cache***
> 

---