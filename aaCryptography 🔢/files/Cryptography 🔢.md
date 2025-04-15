
#cryptography

Il desiderio di mantenere i messaggi nascosti tra due interlocutori agli occhi di terzi non è una necessità nata solo negli ultimi anni ma da molti millenni

> [!brain]- res
> 
> [[SSL_HTTPS.pdf]]
> 
> [[New Direction in Cryptography.pdf]]
> 
> [[Integrita_Autenticazione_Sicurezza.pdf]]
> 
> [[IEEESpectrum.pdf]]
> 
> [[Cryptography Book.pdf]]
> 
> [[AEv2rt5_Cryptography 🔢/res/Crittografia SPbox.pdf|Crittografia SPbox]]
> 
> [[AEv2rt5_Cryptography 🔢/res/Crittografia asimmetrica e RSA.pdf|Crittografia asimmetrica e RSA]]
> 
> [[Appunti_di_Crittografia.pdf]]
>  

> [!key]- Definition of Cryptography
> 
> E' l'insieme delle tecniche che consentono di realizzare la cifratura di un messaggio e la decifratura disponendo di determinati permessi
> 
> Per fare questo viene usato un **algoritmo** che utilizza i simboli che formano il messaggio in chiaro -> ***plain text*** per creare il messaggio occultato -> ***cipher text***  
> 
> > Queste regole di cifratura devono essere note sia al mittente che al destinatario poiché quest'ultimo dovrà poter tradurre il plain text in tempo ***polinomiale***
> 

> [!terminal]- Example
> 
> Se vogliamo trasmettere la parola AIUTO allora l'algoritmo sarà la sostituzione del messaggio con delle lettere e la chiave sarà quella di sommare due posizioni ottenendo così da AIUTO = CKWVQ
> 

> [!radar]- Note
> 
> La differenza tra **codifica** e **cifratura** è che nella prima si intende " ***una traduzione di parole con altre*** ", con la seconda si intende la " ***sostituzione delle lettere e caratteri applicando una funzione ( iniettiva )*** " 	
> 
 
---
#erodoto #spartani #cesare #augusto #alberti #vigenère
## History

> [!quote]- Erodoto V sec. a.c
> 
> *Narra lo storico Erodoto di Alicarnasso che nell’anno 499 a.C., mentre le città ioniche preparavano una grande ribellione contro il dominio persiano, **Istieo di Mileto** si trovava alla corte del re Dario I e non aveva modo di mettersi in contatto con il suo compatriota e tiranno della città Aristagora per comunicargli che era il momento di dare il via alla sollevazione. Alla fine ebbe un’idea: fece rasare la testa al suo schiavo più fedele **e gli tatuò sul cuoio capelluto il messaggio che desiderava trasmettere,** poi aspettò che i capelli ricrescessero, in modo da nascondere il messaggio. Dopo di che, inviò lo schiavo a Mileto, dove gli rasarono nuovamente la testa e poterono leggere il messaggio*
> 

> [!quote]- Enea IV sec. a.c
> 
> *Enea Tattico, autore greco del IV secolo a.C., dedicò un capitolo completo del suo trattato di tecniche militari d’assedio, i **Poliorketikà**. Nel testo proponeva diversi metodi steganografici: **scrivere il messaggio su foglie legate come rimedio medicinale a una ferita;** gonfiare una vescica e scrivervi sopra, in modo che, sgonfiandola, il messaggio non si vedesse **e rigonfiandola si potesse recuperare l’informazione** *
> 
> *Scrivere i messaggi su sottili lamine di piombo che poi venivano arrotolate **e indossate dalle donne come se fossero orecchini** *
> 

> [!quote]- Scitale
> 
> *Il metodo della scitala consisteva nell’arrotolare una striscia di materiale per scrittura, per esempio di pergamena, attorno a un bastone, o “scitala”. Sulla striscia **si scriveva il messaggio e successivamente la si svolgeva,** in modo da ottenere una striscia sulla quale comparivano lettere che non avevano alcun senso. Per poter leggere il messaggio, il destinatario doveva avere una scitala esattamente **dello stesso spessore e della stessa lunghezza,** in modo che, arrotolandovi sopra la striscia di pergamena, le lettere tornassero a occupare la posizione giusta e il messaggio risultasse leggibile*
> 

> [!quote]-  Giulio Cesare 50 a.c
> 
> *Ovidio ( Poeta Latino ) diede consigli nel terzo libro dell'Ars Amatoria alle matrone su come inviare messaggi segreti ai loro amanti. Giulio Cesare usava un metodo più sofisticato degli altri ( De bello gallico di Giulio Cesare e nell'opera di Svetonio ) . Secondo Cassio Dione, «era solito, se voleva comunicare a taluno per via di carteggio qualche segreto, **di metter sempre la lettera dell’alfabeto, che secondo l’ordine era la quarta,** invece di quella che vi si doveva porre, affinché i suoi scritti da nessuno potessero intendersi». Platone, per esempio, si scriverebbe "toesq" *
> 
> Invece  di rotare l'alfabeto di 3 posizioni possiamo rotarlo di una quantità arbitraria **1 <= k <= 25** dove k è la **chiave**
>
> ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-09-26 221511.png]] 
> 

> [!quote]-  Augusto 50 a.c
> 
> *Durante il suo governo Augusto tenne un archivio speciale cifrato di cui solo lui e la moglie Livia conoscevano la struttura e la chiave. Alla morte di Augusto, Tiberio entrò in possesso dell'archivio ma essendo cifrato non riuscì a ricavarne nulla. Successivamente l'imperatore Claudio giunse alla soluzione. I documenti dell'archivio erano scritti in numeri. Augusto scriveva la parola in greco e mettava le sequenze di lettere tradotte in corrispondenza della sequenza di lettere del primo libro dell'Iliade e sostituiva ogni lettera del documento con il numero che indicava la distanza, nell'alfabeto greco, di tale lettera da quella in pari nell'Iliade*
> 
> > [!terminal] Example
> > 
> > ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-10-02 185751.png]]
> > 
> 
> *Tale cifrario è stato svelato poiché Claudio rinvenne casualmente un'edizione dell'iliade usata da Augusto che riportava annotazioni*  
> 
> > [!radar] Note
> > 
> > Sarebbe rimasto inviolato anche se fosse stato reso pubblico mantenendo la chiave segreta ( lunghissima )
> > 
> > *Questo metodo fu utilizzato anche nella seconda guerra mondiale prendendo come chiave una pagina di un libro e cambiandola di giorno in giorno*
> > 
> > Lo svantaggio è di dovere **registrare la chiave per iscritto**
> > 
>  

> [!radar]- Note
> 
> La ***segretezza*** dipendeva dal ***metodo*** poiché scoprire il metodo equivaleva a compromettere l'intero messaggio
> 

> [!quote]-  Leon Battista Alberti XV 
> 
> Sviluppò un cifrario ritenuto inattaccabile fino alla metà dell'Ottocento
> 
> Era basato su un **disco cifrante** che mittente e destinatario dovevano possedere uguale, il che ne limitava un uso generale
> 
> ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-10-10 160021.png]]
> 
> Questo attrezzo era costituito da due dischi rotanti che affiancavano due alfabeti. L'alfabeto esterno era usato per scrivere il messaggio. Il disco interno conteneva un alfabeto più ricco, usato per costruire il crittogramma
> 
> > [!terminal] Example
> > 
> > **ABCDEFGHILMNOPQRSTUVZ12345**
> > **SDTKBJOHRZCUNYEPXVFWAGQILM** 
> > 
> > Chiave -> **A - S**
> > Messaggio -> Non fidarti di Eve
> > 
> > m = NONFIDA2RTIDIEVE
> > 
> > *Scelgo dei numeri casuali da mettere all'interno del messaggio ( 2 )*
> > 
> > c = UNUJRKQ
> > 
> > *Quando incontro un numero la mia chiave viene aggiornata alla lettera corrispondente al numero -> Chiave -> A - Q*
> > 
> > **ABCDEFGHILMNOPQRSTUVZ12345**
> > **QILMSDTKBJOHRZCUNYEPXVFWAG**
> > 
> > c = **UNUJRKQ**==UYBMBSPS==
> >
>
> Il cifrario di Alberti rende **inutili** gli attacchi basati sulla **Frequenza di caratteri**
>  

> [!quote]-  Indice Mobile
> 
> Identico al cifrario di Alberti solo che quando si incontra un numero questo rappresenta il numero di lettere mancanti alla nuova associazione della chiave
> 
> > [!terminal] Example
> > 
> > **ABCDEFGHILMNOPQRSTUVZ12345**
> > **SDTKBJOHRZCUNYEPXVFWAGQILM** 
> > 
> > Chiave -> **A - S**
> > Messaggio -> Il Delfino
> > 
> > m = ILD**2**EL==P==FINO
> > 
> > *Scelgo dei numeri casuali da mettere all'interno del messaggio ( 2 ) e un carattere dopo 2 posizioni*
> > 
> > c = PDC**S**WD==O==
> > 
> > *Quando incontro la lettera che non fa parte del messaggio la mia chiave viene aggiornata alla lettera corrispondente al numero -> Chiave -> A - P*
> > 
> > **ABCDEFGHILMNOPQRSTUVZ12345**
> > **PDNXAOGYIBZRJTSKUFEQHCWLMV**
> > 
> > c = PDC**S**WD==O==OIRJ
> >
> 
>  

> [!quote]-  Cifrario di Vigenère 1586
> 
> E' un cifrario meno sofisticato di quello di Alberti, ma è più pratico poiché basato su una tabella T di dominio pubblico. Questa **tabella** di dimensioni **26 \* 26** contiene l'alfabeto sia per le righe che per le colonne
> 
> ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-10-10 165006.png]]
> 
> La **chiave** del cifrario è costituita da una parola segreta k
> 
> > [!terminal] Example
> > 
> > Chiave -> **C H I A V E** ( caratteri -> 2 7 8 0 24 4 )
> > Messaggio -> NONFIDARTIDIEVE
> > Chiave ripetuta -> CHIAVECHIAVECHI
> > Crittogramma -> PVVFGHCYBIBMGCM
> > 
> 
> La **decifrazione** di un crittogramma si esegue in maniera del tutto analoga alla crittazione
> 
> ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-10-10 170243.png]]
> 
> Anche se questo cifrario è polialfabetico la sicurezza dipende dalla **lunghezza h** della **chiave** poiché se la chiave è ripetuta avrò sempre la stessa cifratura ogni multiplo di h 
>  

> [!quote]-  Principi di Bacone XIII
> 
> 1. Le funzioni **C** e **D** devono essere **facili da calcolare**
> 2. E' **impossibile** ricavare la D se la C non è nota
> 3. Il crittogramma c = C (m) deve apparire **innocente**
>  

---
#ciphers #restricted_ciphers #general_ciphers
## Restricted & General use Ciphers

> [!key]- Definition of Restricted Ciphers
> 
> Le ***funzioni*** di cifratura e decifratura sono tenute ***segrete*** e per questo non sono adatte alla comunicazione di massa ( Quasi tutti i cifrari storici )
> 

> [!key]- Definition of General Ciphers
> 
> Le ***funzioni*** di cifratura e decifratura sono ***pubbliche*** e la ***chiave*** è ***segreta*** ( Cifrari moderni )
> 

---
#ciphers #replacement #transposition
## Replacement & Transposition Ciphers

Il cifrario di Cesare ci ha permesso di distinguere due proprietà che caratterizzano i cifrari ->

> [!key]- Definition of Replacement Ciphers
> 
> I **Cifrari a Sostituzione** sostituiscono ogni lettera del plain text tramite una regola prefissata. A loro volta questi cifrari si dividono in ->
> 
> 1. **Sostituzione Monoalfabetica** -> ad una lettera del messaggio corrisponde una stessa lettera nel crittogramma ( *cifrario di Cesare* )
> 2. **Sostituzione Polialfabetica** -> ad una lettera del messaggio corrisponde una lettera scelta in un insieme di lettere possibili ( *cifrario di Augusto - Alberti* )
> 

> [!key]- Definition of Transposition Ciphers
> 
> I **Cifrari a Trasposizione** permutano le lettere del messaggio in chiaro secondo una regola prefissata eliminando qualsiasi struttura linguistica presente ( *eventualmente inserendo nuove lettere che poi verranno ignorate nella decifrazione* )
> 

> [!radar]- Note
> 
> Se la ***segretezza*** dipende dalla **chiave** allora il numero di chiavi deve essere così grande da essere immune ad ogni attacco a forza bruta. Allora la chiave deve essere **scelta** in modo **CASUALE**
> 

---
#complete_ciphers
## Complete Ciphers

> [!key]- Definition of Complete Ciphers
> 
> I **Cifrari Completi** permutano le lettere del messaggio in chiaro secondo una permutazione dell'alfabeto -> La **chiave** diventa la **permutazione dell'alfabeto** portando un attacco di forza bruta a provare **26! - 1** -> Impraticabile
> 
> ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-10-02 183411.png]]
> 

> [!radar]- Note
> 
> Questo cifrario non sarà più attaccabile da forza bruta ma da altri 2 metodi ->
> 
> 1. **Struttura logica del messaggio in cifrato**
> 2. **Occorrenza statica delle lettere** -> Il messaggio in chiaro conserva le informazioni di frequenza dell'alfabeto originale -> La struttura del testo in chiaro permane nel testo cifrato
> 
> >*Frequenze dei caratteri in italiano*
> ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-10-02 184059.png]]
> 

---
#simple_permutation
## Simple Permutation

> [!key]- Definition of Simple Permutation
> 
> **Chiave** -> Intero **h** e **pi** = permutazione dei primi h interi ( { 1, 2, ..., h } )
> **Cifratura** -> Si suddivide il messaggio **m** in **blocchi** di **h lettere**
> 
> *Se la lunghezza di m non è divisibile per h, si aggiungono alla fine del messaggio delle lettere causali* -> **Padding** *che sono aggiunte alla trasposizione ma sono ignorate dal destinatario perché situate alla fine del messaggio*
> 
> ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-10-10 172614.png]]
> 
> Il numero delle chiavi totali è **h! - 1** ( la permutazione identica ) -> Tanto più h è grande tanto più l'algoritmo sarà efficiente
> 

---
#columns_permutation
## Columns Permutation

> [!key]- Definition of Columns Permutation
> 
> **Chiave** -> k = **< c, r, pi >** dove **c** e **r** denotano il numero di **righe e colonne** di una tabella e pi rimane invariato  
> **Cifratura** -> Si suddivide il messaggio **m** in **blocchi** di **c x r caratteri** disponendo il messaggio dall'alto verso il basso seguendo le righe
> 
> *Se la lunghezza di m non è divisibile per h, si aggiungono alla fine del messaggio delle lettere causali* -> **Padding** *che sono aggiunte alla trasposizione ma sono ignorate dal destinatario perché situate alla fine del messaggio*
> 
> ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-10-10 173334.png]]
> 
> Dopo aver disposto il blocco nella tabella si permutano le colonne
> 
> ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-10-10 173500.png]]
> 

---
#grid_cipher #richelieu
## Grid Cipher

> [!key]- Definition of Grid Cipher
> 
> ***Cifrario di  Richelieu*** -> L'idea è quella di prendere una pagina tutta scritta e poggiarci sopra una scheda forata che copre tutte le lettere tranne quelle che formeranno il messaggio
> 
> **Chiave** -> **griglia q x q** con q **pari** con 1/4 delle celle trasparenti ( q^2 / 4 ) e le altre opache
> 
> Si scrivono i primi s caratteri del messaggio nelle posizioni corrispondenti alle celle trasparenti. Successivamente la griglia viene **ruotata** tre volte di 90 gradi in senso orario e, per ogni rotazione, si ripete l'operazione di scrittura di tre successivi gruppi di s caratteri
> 
> ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-10-10 202040.png]]
> 
> *La griglia deve essere scelta in modo che le posizioni corrispondenti alle celle trasparenti non si sovrappongano mai nelle rotazioni*
>  
> ** \# Chiavi** -> G = 4^s --> se q = 6 -> G = 4^9 ≈ 260000 
> 
> Possiamo dire che questo cifrario è **immune** dagli **attacchi** di **forza bruta**
> 

---
#cryptanalysis #static_cryptanalysis
## Cryptanalysis

Generalmente questo algoritmo di cifratura è noto, pertanto diviene soggetto a crittoanalisi da parte di malintenzionati

> [!key]- Definition of Cryptanalysis
> 
> Studio dei metodi per ottenere il significato delle informazioni cifrate: tipicamente si tratta delle operazioni effettuate per la ricerca della chiave segreta
> 

> [!key]- Definition of Cryptology
> 
> La ***Crittologia*** è l'unione della **crittografia** e della **crittoanalisi**  	
> 

> [!warning]- Definition of Static Cryptanalysis 
> 
> La sicurezza di un cifrario è legata dalla dimensione dello spazio delle chiavi ma ...
> 
> I possibili attacchi riguardano non l'algoritmo ma ->
> 
> 1. **Chiavi usate male** -> troppo corte, generate male, prevedibili, riutilizzate
> 2. **Si conosce il formato del messaggio**
> 3. ...
> 
> Allora con la ***crittoanalisi statica*** il cifrario è forzato non da un punto di vista algoritmico bensì da un ***analisi***. I cifrari storici infatti sono stati violati con un attacco statico di tipo ***chiper text***
> 
> *Questo dalla metà del XIX secolo quando si scoprì come violare il cifrario di Vigenére, considerato assolutamente sicuro da 300 anni*
> 
> Disponendo del **metodo** per la cifratura / decifrazione e il **linguaggio naturale** in cui è scritto il messaggio ( abbastanza lungo ) si **studia la frequenza** con cui appaiono in media le varie lettere dell'alfabeto e si studiano anche ->
> 
> 1. ***Diagrammi*** -> gruppi di due lettere consecutive
> 2. ***Trigrammi*** -> gruppi di tre lettere consecutive
> 3. ***q - grammi***
> 

> [!danger]- Attack Old Ciphers
> 
> - Se un crittogramma è stato generato per ***sostituzione monoalfabetica*** ->
> 
> 1. ***frequenza ( x ) ≈ frequenza ( y )*** ( Nel cifrario di Cesare basta scoprire la coppia <x, y> )
> 2. ***Cifrari affini*** -> Individuare due coppie di  lettere corrispondenti con cui impostare un sistema di due equazioni nel die incognite **a** e **b** che formano la **chiave segreta** ( con a **invertibile** )
> 3. ***Cifrari completi*** -> Come il primo metodo ma si tiene conto contemporaneamente della **sequenza** di **più caratteri** e dei **diagrammi più frequenti** e si provano le varie associazioni
> 
> - ***Cifrari a sostituzione polialfabetica*** -> Ovviamente la decifrazione è più difficile ma non impossibile; la **debolezza** del cifrario è che la **chiave** viene **ripetuta** più volte quindi se la chiave contiene h caratteri, le apparizioni della stessa lettera distanti un multiplo di h ne messaggio si sovrappongono alla stessa lettera della chiave ( stessa lettera cifrata ). Molto più facile sarebbe se si conoscesse la dimensione della chiave h
> 
> > Per questi ultimi cifrari notiamo che il cifrario conterrà quasi sicuramente **gruppi di lettere adiacenti ripetuti** -> ci cercano nel crittogramma coppie di posizione p1, p2 in cui iniziano le sottosequenze identiche -> d = p2 - p1 è probabilmente la lunghezza h della chiave o un suo multiplo
> 
> - ***Cifrari a trasposizione*** -> Le lettere del crittogramma sono le stesse del messaggio in chiaro quindi non ha senso condurre un attacco statico sulle frequenze ma sui **q-grammi** presenti nel messaggio. Anche qui molto più semplice se si conosce la lunghezza h della chiave poiché basta dividere il messaggio in h sottosequenze e cercare i gruppi di q lettere che formano i q-grammi ( Es: Nella lingua italiana **Q** è **seguita** sempre dalla **U** )
>  

> [!radar]- Note
> 
> *Queste tecniche immuni contro il cifrario di Alberti e Indice mobile ma sappiamo che mantenere a lungo una chiave mette a rischio il cifrario*
> 
> *Se non si conosce il metodo di cifratura usato, la frequenza delle lettere del crittogramma è un potente indizio per comprendere la natura del cifrario*
> 

---
#attacks #brute-force-attack
## Cryptographic Attacks

> [!danger]- Definition of Cryptographic Attacks
> 
> 1. ***Cipher Text Attack*** -> Il Crittoanalista rileva solo il messaggio cifrato
> 
> 2. ***Know Plain-Text Attack*** -> Il Crittoanalista conosce il messaggio in chiaro
> 
> 3. ***Chosen Plain-Text Attack*** -> Il Crittoanalista sceglie il messaggio in chiaro
> 
> 4. ***Chosen Cipher-Text Attack*** -> Il Crittoanalista sceglie il messaggio cifrato
> 
> 5. ***Brute Force*** -> Il Crittoanalista prova tutte le possibile chiavi
> 
> 6. ***Man in-the-Middle*** -> Il Crittoanalista si installa sul canale di comunicazione 
> 
> 7. ***Letters Frequency*** -> Quando la forza bruta non funziona possiamo usare la tecnica delle frequenze dei caratteri per ridurre il numero di opzioni all'attacco
> 
> ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-09-20 114126.png]]
> 

---
#one_time_pad
## Perfect Ciphers

> [!quote]- Quote
> 
> #Kereckhoffs
> 
> Il principio di Kerckhoffs ( 1835 - 1903 ) stabilisce che è la chiave l'elemento fondamentale per la sicurezza di un sistema informatico. La sicurezza dipende dalla segretezza della chiave
> 
> *come corollario al principio*
> #Shannon
> 
> Shannon aggiunse la frase << ***il nemico conosce il sistema*** >>
> 

> [!quote]- Quote
> 
> #Shannon 
> 
> *Claude Shannon sostiene che Il messaggio in chiaro è il crittogramma devono essere del tutto scorrelati tra loro*
> 
> Nel 1949 Claude Shannon formalizzò il processo crittografico mediante un modello matematico. Prima di allora aveva già ottenuto dei risultati ma per motivi bellici dovette rimandare la pubblicazione di tali scoperte
> 

> [!key]- Definition of One Time Pad
> 
> Grazie a questi principi si è arrivati alla definizione di un cifrario **assolutamente sicuro** chiamato ***one-time pad*** -->
> 
> - Le due parti coinvolte nella comunicazione condividono un pad ( blocco ) di chiavi generate casualmente e per ogni lettere cambiano la chiave
> - Se il messaggio viene intercettato, l'intruso vede solamente una sequenza di caratteri casuali

> [!quote]- Quote
> 
> #Vernam
> 
> Questo meccanismo trova implementazione nel cifrario di **Vernam** la cui realizzazione richiede una chiave lunga quanto il messaggio, essendo molto complesso. Da qui prendono spunto anche i sistemi ***one-time key*** che generano ogni volta una chiave valida per un tempo modesto ( 10-20 secondi )

---
#avalanche_effect
## Avalanche Effect

> [!key]- Definition of Avalanche Effect
> 
> Una caratteristica per gli algoritmi di cifratura è quello che prende il nome di effetto valanga -> ***un cambiamento di pochi bit nel plain text deve provocare un cambiamento di quanti più bit nel cipher text***
> 

---
#encryption
## Encryption

> [!key]- Type of Encryption
> 
> Se la chiave di cifratura coincide con quella di decifratura si parla di **schema crittografico simmetrico** e la chiave prende il nome di **chiave comune**
> 
> ![[AEv2rt5_Cryptography 🔢/img/Immagine 2022-02-03 161949.jpg]]
> 
> Quando la chiave è diversa invece, si parla di **schema crittografico asimmetrico** e le due chiavi sono una **pubblica** ed una **privata** ( alla base delle comunicazione odierne )
> 
> ![[AEv2rt5_Cryptography 🔢/img/Immagine 2022-02-03 162132.jpg]]
> 
> Il numero delle ***chiavi*** deve essere così grande da essere praticamente ***immune*** dagli attacchi di ***Brute Force*** e la chiave deve essere scelta in maniera ***casuale*** in modo tale da non permettere al crittoanalista di decifrare il messaggio se non dispone della chiave in tempo **polinomiale**
> 
> La funzione di cifratura deve essere una funzione ***one-way trap-door***, cioè ***criptare*** il messaggio diviene un'operazione computazionalmente ***facile*** mentre **decriptare** è computazionalmente ***difficile a meno che*** non si **conosca** un meccanismo segreto ovvero la ***chiave privata***
> 
> Generalmente ogni algoritmo di cifratura si basa su 4 operazioni principali -->
> 
> - ***Sostituzione***
> - ***Scorrimento***
> - ***Trasposizione***
> - ***Confusione*** 
> 

---
#hybrid_cryptographic
# Hybrid Cryptographic

> [!key]- Definition of Hybrid Cryptographic
> 
> I sistemi di crittografia ibridi uniscono le due tecniche allo scopo di unirne i vantaggi
> 
> Si usa un cifrario simmetrico ma la chiave viene scambiata con un cifrario asimmetrico ->
> 
> 1. Si usa l'AES per la comunicazione di massa
> 2. Un cifrario a chiave pubblica per scambiare la chiave dell'AES senza mettere in contatto precedentemente gli interlecutori
> 
> In questo modo la comunicazione di messaggi avviene in maniera rapida mentre quelle delle chiavi in maniera lenta
> 

---
