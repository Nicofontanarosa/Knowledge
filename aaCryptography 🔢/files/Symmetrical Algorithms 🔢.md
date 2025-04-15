
#symmetrical_algorithms

> [!key]- Definition of Symmetrical Algorithms
> 
> In questi algoritmi il mittente e il destinatario devono scambiarsi la chiave prima di poter effettuare la cifratura del messaggio
> 
> ![[AEv2rt5_Cryptography 🔢/img/Immagine 2022-02-03 165645.jpg]]
> 
> Sono molto **più veloci** degli algoritmi asimmetrici ma come fare per ***scambiarsi la chiave?*** si utilizza la **crittografia asimmetrica**
> 
> A critta il messaggio con la sua chiave privata e critta la sua chiave con la chiave pubblica di B, una volta arrivato, B decripta la chiave di A con la sua chiave privata e successivamente decripta il messaggio con la chiave che ha appena decriptato
> 

> [!quote]-
> 
> #Shannon 
> 
> Alla base dei cifrari a chiave simmetrica, Shannon propose due criteri ->
> 
> 1. ***Diffusione*** -> Il testo in chiaro si deve distribuire su tutto il crittogramma -> ogni carattere  del crittogramma deve dipendere da tutti i caratteri del blocco di messaggio in modo da disperdere ogni tipo di regola sintattica che legava il messaggio
> 
> 2. ***Confusione*** -> Messaggio e chiave sono combinati tra loro in modo complesso per non permettere al crittoanalista di separare le due sequenze tramite l'analisi statistica del crittogramma
> 
> 	- La chiave deve essere ben distribuita sul testo cifrato
> 
> 	- Ogni bit del crittogramma deve dipendere da tutti i bit della chiave
> 
> *Più tardi si parlerà anche di sostituzione, scorrimento e trasposizione*
> 

---
#IBM #NIST #LUCIFER #DES #3DES #AES 
## History of DES & 3DES

> [!quote]- History
> 
> *Nel 1972 il National Bureau of Standards (NBS), oggi National Institute for Security and Technology (NIST), ufficio americano per la definizione degli standard, iniziò un programma per proteggere le comunicazioni non classificate, cioè in genere le comunicazioni commerciali o personali fra privati. Il progetto doveva dirigersi verso un sistema di cifratura che presentasse facilità di secretazione della chiave, sicurezza certificata ed economicità nella sostituzione della chiave in presenza di forzature*
> 
> *La necessità di sviluppare un tale sistema era in quel tempo fortemente sentita poiché molte compagnie comunicavano in modo “sicuro” impiegando prodotti non certificati ed esponendosi così a possibili attacchi, in particolare da parte dei realizzatori del software stesso. Inoltre i prodotti disponibili per la sicurezza erano diversi tra loro rendendo incompatibili le comunicazioni tra sistemi differenti. Il programma della NBS si proponeva di definire un prodotto a chiave segreta noto a tutti (non oscuro), che fosse unico (compatibile), e che potesse essere studiato a fondo per dimostrarne la sicurezza (ufficialmente certificato). Nel 1973 l’NBS pubblicò un bando che fra i vari punti richiedeva* ->
> 
> • che la sicurezza dell’algoritmo risedesse nella segretezza della chiave e non nel processo di cifratura e decifrazione
> • che l’algoritmo potesse essere realizzato efficientemente in hardware
> 
> *Sfortunatamente nessuno avanzò proposte*
> 

> [!quote]- Born of LUCIFER & DES
> 
> *Un bando successivo venne accolto dalla IBM che propose un sistema, il DES ( Horst_Feistel ), derivato da un software già noto chiamato Lucifer. La National Security Agency (NSA), ente che a quel tempo possedeva tutto lo scibile sulle comunicazioni segrete, certificò il DES, fece commenti sulla sua struttura e propose delle variazioni tra cui la riduzione della lunghezza della chiave da 128 a 56 bit e la modifica delle funzioni contenute nella * ***S-box***. *Tra le motivazioni della NSA vi era la necessità di dissipare ogni possibile dubbio degli utenti sulla presenza di una “back-door” nel cifrario*
> 
> *L’IBM accettò le modifiche solo dopo una severa serie di test i cui criteri sono rimasti segreti*
> 
> ***Il DES fu accettato e reso pubblicamente disponibile nel 1977***
> 
> *Il DES doveva essere certificato ogni cinque anni e così avvenne regolarmente fino al 1987, quando ci si chiese se la sua sicurezza fosse ormai messa in pericolo dalle nuove tecniche crittoanalitiche sviluppate nei dieci anni precedenti, nonché dall’aumento di potenza dei calcolatori che rendeva più probabili futuri attacchi esaurienti sull’insieme delle chiavi*
> 
> *Così il cifrario fu certificato di nuovo a più riprese fino al 1999, anno in cui fu dichiarato accettabile solo per scopi limitati ammettendone un uso generale nella versione estesa 3DES*
> 

> [!quote]- Born of AES
> 
> *Nel 1993 il NIST decise di valutare nuove proposte, e nel novembre 2001 ha scelto il successore denominato AES per Advanced Encryption Standard che rimosse dagli standard il 3DES nel 2005*
> 

---
#DES #Horst_Feistel
## DES Cipher

> [!key]- Definition of DES Cipher
> 
> **DES** ( **D**ata **E**ncryption **S**tandard ) è un algoritmo **simmetrico** a chiave segreta di **64 bit** dei quali **8** di **controllo** ( *8 byte totali di cui 7 bit scelti arbitrariamente e l'ottavo aggiunto per il controllo di parità* ), **56 utili** che vengono impiegati in **16** trasformazioni successive applicate al messaggio ( anche chiamati **round** )
> 
> Il **messaggio** è suddiviso in **blocchi** da **64 bit** cifrati e decifrati indipendentemente dagli altri blocchi
> 
> Dalla **chiave k** vengono create le **sottochiavi di fase** ( **usate** nei vari **round** )
> 
> ![[AEv2rt5_Cryptography 🔢/img/Immagine 2022-02-09 173340.jpg]]
> 
> **La decifrazione consiste nel ripetere il processo invertendo l’ordine delle chiavi, caratteristica per nulla ovvia vista la struttura del cifrario**
> 

> [!radar]- Note
> 
> *Ricordiamo che nei computer la crittografia deve lavorare non più su di un alfabeto di 26 lettere ma su file binari con byte codificati in ASCII, ( 128 caratteri di cui solo 96 stampabili )*
> 

> [!danger]- Attack DES Ciphers
> 
> 1. **Progettare apposite architetture** -> Il 17 luglio 1998 la **Electronic Frontier Foundation** diffuse un comunicato stampa con il quale annunciò la definitiva sconfitta del DES effettuata grazie ad un **calcolatore di 250.000 $** che in meno di ***60h*** aveva forzato un messaggio cifrato con DES
> > Nel 2008 con la macchina RIVYERA un calcolatore altrettanto costoso, venne costruito appositamente per rompere il cifrario in meno di ventiquattr’ore
> 
> 2. **Distribuire il calcolo** -> Nel 1997 per 5 mesi di esplorazione, vinse un informatica statunitense la bellezza di $10000 ( andati anche alle entità che prestavano la macchina ) distribuendo il calcolo su più macchine e uso il 25% dello spazio delle chiavi totali. Il messaggio risultante fu "strong cryptography makes the world a safer place"
> > Nel 1998, con l'aumento della potenza tecnologica, si impiegarono 39 giorni, esplorando l'85% dello spazio delle chiavi. Il messaggio fu "many hands make light work"
> 
> Lo **spazio delle chiavi** era **{0, 1}^56** ma in realtà sono **55** ( leviamo il doppio delle chiavi ). Perché ?
> 
> **C(m, k) = C(not(m), not(k)) = c**
> 
> > Questo spazio si riduce di altre ***64 chiavi deboli*** che sono quella composta da tutti 0 e tutti 1, quella composta da 28 1 e 28 0 e viceversa poichè producono la stessa sottochiave in ogni fase del cifrario
> 
> 3. **Crittoanalisi Differenziale** -> Si parla di attacco **chosen plain text**. Nata nel 1990 costa come un brute force sulle chiavi con 16 fasi ma richiede 2^47 coppie <m, c> ( >> spazio ). Con 8 fasi il numero di coppie richieste scende a 2^14
> 
> 4. **Crittoanalisi Lineare** -> Si parla di attacco **know plain-text attack**. Nata nel 1993 meno costoso di un brute force ed il numero di coppie <m, c> scende a 2^43. Attacca alcuni bit della chiave con un brute force mentre altri gli infierisce mediante un'**approssimazione lineare della S - BOX**
>  

> [!key]- How DES Cipher work
> 
> - La **S - BOX** garantisce la **non linearità** del DES
> - Ogni bit della chiave partecipa a 14 delle 16 fasi del DES
> 
> ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-10-21 092703.png]]
> 
> 1. Il **messaggio** viene **permutato** inizialmente e anche alla fine -> Tranquillamente **omissibile**
> 
> ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-10-21 093108.png]]
> > Permutazione iniziale e finale
>
> 1. La **chiave** viene **depurata** togliendo i **bit di parità** e **mescolando** i **56 bit** rimanente
> 
> ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-10-21 093226.png]]
> > Bit mescolati e rimossi i multipli di 8
> 
> 3. Il **messaggio** viene **diviso** in **blocco sx** e **blocco dx**
> 
> 4. **k[0]** è la prima **sottochiave di round**
> 
> 5. in ogni **round** cambio la **parte sinistra con quella destra** e alla **parte destra applico una funzione non lineare** ( **Feistel Network** )
> 
> ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-10-20 233127.png]]
> 
> ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-10-21 093513.png]]
> > Il trapezio significa espansione ( EP ) mentre quello rovesciato significa riduzione ( CT )
> 
> **Estrazione della sottochiave di fase** -> 
> 
> 1. Si **dividono in due** metà e **ruotate** in modo **ciclico** di un numero di posizione che **dipende** dalla **fase** ( nella fase 1, 2, 9, 16 si esegue un **shift** ciclico di 1 sola posizione mentre 2 nelle altre fasi )
> 
> **S - BOX** -> **Riduce** il numero di **bit** da 48 a 32
> 
> ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-10-21 103026.png]]
> > Dove i 4 bit centrali corrispondo alla colonna di una tabella mentre i due bit estremi corrispondono alla riga
> 
> ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-10-21 103259.png]]
> > Dove tutte le altre sottofunzioni S2, S2, ..., S8 sono definite similmente
> 
> - Il ***vantaggio*** di questa S - BOX è quello di essere **super veloce** nel fornire il risultato dato che ce l'ho già scritto in forma tabellare
> 
> - Lo ***svantaggio*** è che non si conosce la funzione che ha portato a quella tabella 
>
> **Permutazione dopo S - BOX** ->
> 
> ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-10-21 121809.png]]
> 

> [!key]- Property of S - BOX
> 
> 1) **Dimensioni tabella** -> 4 x 16 bit era la dimensione massima su un chip nel 1974
> 2) **Output** -> I bit in uscita non devono avere nessuna correlazione con una funzione lineare
> 3) **One - to - one** -> I 4 bit in uscita sono una permutazione dei 4 bit in entrata
> 4) **Effetto valanga** -> Cambiando un bit in entrata cambiano almeno due bit di output
> 
> *Affinché la S - BOX non operi sempre sugli stessi 6 bit -> i 4 bit di ogni S - BOX influenzano l'input di 6 della S - BOX del round successivo*
> 

> [!terminal]- Example
> 
> **Se due input del DES differiscono di un solo bit ?** Se la differenza è nella metà sinistra ->
> 
> 1. Dopo il primo round gli output differiscono di un solo bit nella metà destra
> 2. Dopo il secondo round gli output differiscono di almeno 3 bit, uno nella metà di sinistra e due nella metà di destra
> 3. La **permutazione a destra diffonde i due bit in modo che ciascuno vada in input ad una S - BOX diversa**
>
> Dopo **7 round** tutti i 32 bit nella **metà di destra** saranno **modificati**
> Dopo **8 round** tutti i 32 bit nella **metà di sinistra** saranno **modificati**
> 

---
#DES #Horst_Feistel #3DES
## 3-DES Cipher

> [!radar]- Note
> 
> Sono stati quindi proposti diversi metodi per incrementare la sicurezza del DES e diversi cifrari in alternativa ->
> 
> 1. ***Scelta indipendente delle sottochiavi*** -> Le sedici sottochiavi di 48 bit utilizzate nelle successive fasi vengono scelte indipendentemente l’una dall’altra, per un totale di 768 bit anziché 56, il che rende impraticabile un attacco esauriente sulle chiavi stesse
> 
> 2. ***Cifratura multipla*** -> Concatenazione di più coppie del DES che utilizzano chiavi diverse -> **cifrario composto**. Questa cosa funziona poiché ->
> > ***C(C(m, k1), k2) != C(m, k3)***
> 
> In questo modo ottengo due chiavi da 56 bit -> **112 bit di chiave**. No in realtà si anno **57 bit di chiave** perché ->
> 
> - **Meet in the middle** -> c = C(C(m, k1), k2) --> D(c, k2) = C(m, k1) 
> 
> data una coppia <m, c>, per ogni k1 calcolo e salvo C(m, k1) in una tabella e per ogni k2 calcolo D(c, k2) e cerco nella tabella di C(m, k1) --> il costo di questo algoritmo è O(2^56 + 2^56) = **O(2^57) + uno spazio di O(2^56)** 
> 

> [!key]- Definition of 3T-DES & 2T-DES Ciphers
> 
> Nel 1999 è stato introdotto Triple DES. Nella cifratura e decifratura alterniamo un passo di cifratura ed un passo di decifratura. Nel 2T-DES k3 = k1 -> chiave complessiva = 112 bit ( 128 - 16 di parità )
> 
> ![[AEv2rt5_Cryptography 🔢/img/Immagine 2022-02-09 175256.jpg]]
> 
> Nel 3T-DES k1 != k3 -> chiave complessiva = 112 bit ? perché ->
> 
> - **Meet in the middle** -> D(c, k3) = D(C(m, k1), k2)  e C(D(c, k3), k2) = C(m, k1)
>
> Data una coppia <m, c>, per ogni k1 calcolo e salvo C(m, k1) in una tabella e per ogni coppia k2, k3 calcolo C(D(c, k3), k2) e cerco nella tabella --> il costo dell'algoritmo è O(2^56 + 2^56 \* 2^56) = **O(2^112) + uno spazio di O(2^56)**
> 
> 

---
#IDEA
## IDEA Algorithm

> [!quote]- Quote
> 
> #Xuejja_Lai #James_Massey
> 
> Nel 1991 fu proposto il cifraio **IDEA** ( Internetional Data Encryption Algorithm ). Il metodo è stato sviluppato in Svizzera da i due ricercatori Xuejja Lai e James L. Massey
> 

> [!key]- Definition of IDEA Algorithm
> 
> Si basa su concetti simili al DES con chiave a **128 bit** da cui si estraggono **52** sottochiavi da **16** bit, mediante shift ciclici, dove i blocchi da **64** bit del messaggio vengono divisi in **4** blocchi da **16** usando le operazioni di XOR, somma e moltiplicazione modulo 216. La **cifratura** di sviluppa in **otto fasi uguali** tra loro
>
> Basandosi su forti basi teoriche questo cifrario **è rimasto inviolato** negli anni
> 

---
#RC5
## RC5 Algorithm

> [!quote]- Quote
> 
> #Ron_Rivest
> 
> RC5 è il successore di altri 4 cifrari di cui RC2 e RC4 ampiamente utilizzati ideato da il crittografo statunitense Ron Rivest
> 

> [!key]- Definition of RC5 Algorithm
> 
> Si basa su concetti simili al DES a **blocchi di 64 bit**, con **chiave** di **c x 32 bit** organizzato in **r fasi** con **c & r scelti a piacere**
> 
> Le operazioni usate nel cifrario sono lo **shift ciclico, XOR e addizione**. Con **r = 16** ( come per il DES ) e c scelto opportunamente questo cifrario diventa sicuro quanto il 3DES
> 

---
#AES #side-channel #padding
## AES Algorithm

> [!quote]- 
> 
> #rijndael #vincent_rijmen #joan_daelmen
> 
> Nel 1997 il ***NIST*** ( National Institute of Standard & Technology ) USA organizzò un concorso per sostituire l'algoritmo di cifratura DES. Vennero proposti 21 algoritmi, scremati nel 1998 a 15 e nel 1999 a i 5 finalisti sottoposti a verifiche e opinioni di tutti gli altri team. Questi algoritmi erano ->
> 
> 1. ***MARS*** -> Proposto da *IBM*
> 2. ***RC6*** -> Proposto da *RSA Laboratories*
> 3. ***Rijndael*** -> Proposto dalla *Proton World International & Leuven Univeristy*
> 4. ***Serpent*** -> Proposto dalle *Cambridge, Technion & San Diego University*
> 5. ***Twofish*** -> Proposto dalle *Berkeley & Princeton University*
> 
> Nell'aprile del 2000 è stata indetta una conferenza per discutere dei pro e contro di ogni algoritmo proposto e nell'ottobre del 2000 il NIST definì l' ***Advanced Encryption Standard*** ideato da due crittologi belgi **Joan Daelmen & Vincent Rijmen** anche chiamato algoritmo **Rijndael** come sostituto del DES ( ufficialmente nel 2001 dopo 4 anni )
> 
> L'algoritmo AES è stato valutato ed approvato sulla verifica di requisiti fondamentali -->
> 
> - **sicurezza** --> La chiave ha dimensioni minime di 128 bit, gli attacchi di forza bruta non sono molto efficienti
> - **costo computazionale ottimale**
> - **semplicità, flessibilità, adattabilità**
>   

> [!key]- Definition of AES Algorithm
> 
> Prevede che i **blocchi** in cui è diviso il **messaggio** e la **chiave** segreta siano formati da **128, 192 o 256 bit**. L'AES prevede solo queste dimensioni per i blocchi
> 
> ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-11-05 192511.png]]
> 
> **Ogni fase** impiega una **propria chiave locale** creata a partire dalla chiave segreta e opera su una **matrice** di 128bit, ovvero **16 byte** ( 4 x 4 )
> 
> ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-11-05 192843.png]] 
>
> La **chiave iniziale** è caricata in una **matrice W** successivamente ampliata **aggiungendo 40** colonne generate dalla prime 4 ->
>
>- W[i] = W[i-4] XOR W[i-1] con i non multiplo di 4
>- W[i] = W[i-4] XOR T(W[i-1]) con i multiplo di 4
> Dove **T** è la **S - BOX non lineare**
> 
>![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-11-05 194153.png]]
>
> questo poiché la chiave k(i) per la i-esima fase è data dalle 4 colonne W[4i], W[4i+1], W[4i+2] ...
>
>![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-11-05 194616.png]]
> 

> [!key]- Definition of T
> 
> ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-11-05 195039.png]]
> 
> - ***Shift ciclico a sinistra***
> - ***Applico la S - Box che lascia invariato il numero dei bit***
> - ***Eseguo lo XOR bit a bit per il primo elemento con una costante dinamica***
> 
> ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-11-05 195113.png]]
> 
> Dove **RC(i) = 1 se i = 1 e 2RC(i-1) con 2 <= i <= 10**
>  

> [!radar]- Note
> 
> Tutte queste operazioni vengono eseguite nel **campo** ***GF(2^8)*** --> ***Campi Binari***
> 
> Questo significa che le **somme** si fanno **mod 2** e le **moltiplicazioni** tra byte significa moltiplicare per un **polinomio** di **grado 7** e trarne il **resto**
> 

> [!key]- Message
> 
> Il **messaggio** viene **cifrato** tramite **4 operazioni** principali tranne nell'ultima fase ( *la decifrazione non è neanche del tutto identica a questa cifratura* )-->
> 
>![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-11-05 195659.png]]
>
> Dove la fase ***Add Round Key*** significa porre ogni byte del **messaggio** posto nella matrice B in **XOR** bit a bit con il corrispondente byte della **chiave**
>
> ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-11-06 164309.png]]
> 
> 1. ***Substitute Bytes*** -> Ad ogni byte del blocco applico la **S - Box**
> 
> ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-11-06 182833.png]]
> 
> 2. ***Shift Rows*** -> I byte di **ogni riga** i vengono **shiftati** a sinistra di i posizioni 
> 3. ***Mix Columns*** -> **Ogni colonna** del messaggio viene **moltiplicata** per una **matrice 4 x 4 byte** ( *eseguita in mod 2^8* ). In questo modo l'elemento ai,j si trasforma in bi,j che dipende da ciascun byte della colonna con a1,1, a1,2 ...
>
> Con **Shift & Mix** si garantisce la diffusione dopo solo 2 fasi -> **ogni bit di output dipende da tutti i bit di input**
> 
> 4. ***Add Round Key*** -> **Xor con la chiave locale**
> 

> [!key]- Definition of S - Box
> 
> La S - Box è una **matrice 16 x 16 byte** ( 256 byte ) che contiene una **permutazione** di tutti i **256 interi a 8 bit**
> 
> ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-11-06 183122.png]]
> 
> Applicare la S - Box al messaggio significa **sostituirlo** con il suo **inverso moltiplicativo in GF(2^8)** ( operazione **non lineare** poiché **(b1 XOR b2)^-1 != (b1^-1 XOR b2^-1)** ). Questo equivale a **sostituire bi** nella tabella di sopra in questo modo -->
> 
> ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-11-06 184021.png]]
> 
> e poi **moltiplicarlo** per una **matrice 8 x 8 bit** ( fissata ) **sommato** con un **vettore colonna** ( fissato )
> 
> 
>  

> [!danger]- Attack AES Ciphers
> 
> **Ad oggi non ci sono stati attacchi di compromettere l'AES anche con 128 bit di chiave** poichè ***tutti i bit di chiave sono bit di sicurezza***
> 
> Con 6 fasi l'AES è attaccabile ma già con 7 non è più efficace 
> 
> Questo cifrario è soggetto ad ***attacchi side - channel*** che non sfruttano le caratteristiche del cifrario ma le possibili debolezze della piattaforma su cui esso è implementato
>  

> [!warning]- Danger
> 
> **Se il messaggio non ha lunghezza che è multiplo della dimensione del blocco ?** -> ***PADDING***
> 
> Il **problema** è che **blocchi uguali** nello stesso messaggio producono **blocchi cifrati uguali** --> servono metodi per aumentare la **diffusione** e limitare la **periodicità** nel crittogramma
> 

---
#CBC #Shannon 
## Cypher Block Chaning

> [!key]- Definition of Cypher Block Chaning
> 
> **Si crea una dipendenza con i blocchi del messaggio** seguendo sempre il principio della diffusione di Shannon
> 
> ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-11-09 161853.png]]
> 
> - ***Cifratura*** -> c(i) = C(m(i) XOR c(i - 1), k)
> - ***Decifratura*** -> m(i) = c(i - 1) XOR D(c(i), k)
> 
> Da queste formule possiamo dire che il processo di **cifratura** è **sequenziale** mentre il processo di **decifratura** può essere eseguito in **parallelo** se tutti i blocchi del cifrario sono disponibili
>
> L'**introduzione** per produrre la sequenza c(0) è essenziale altrimenti messaggi uguali avranno la stessa cifratura -> questa può essere costruita tramite una stringa generata **pseudocasualmente** ( marca temporale )
> 
> ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-11-09 161945.png]]
> 

> [!radar]- Note
> 
> I **vantaggi** del CBC sono ->
> 
> 1. **processi di cifratura e decifratura sono veloci come l' AES normale**
> 2. **+ messaggi possono essere cifrati con la stessa chiave**
> 3. **resistenza agli errori nel crittogramma** ( *dovuti per esempio alla trasmissione o a manomissioni controllate da un crittoanalista* )
> 

> [!key]- Definition of CFB & OFB 
> 
> Il **C**ipher **F**eedback e l' **O**utput **F**eedback il messaggio in chiaro è posto in XOR con l' uscita di un generatore di bit pseudo - casuali ( *invece che con il crittogramma precedente* )
> 

> [!brain]- Impressions
> 
> #mind
> 
> DES - TDES & AES sono dei cifrari simmetrici per criptare i messaggi quindi gli unici attacchi possibili sono quelli che puntano a ricavare il messaggio dal crittogramma andando a forzare la chiave. Detto ciò, i possibili attacchi sono =>
> 
> 1. #brute-force-attack
> 2. #meet-in-the-middle
> 3. #side-channel 
> 
> Solo per il DES abbiamo anche altri attacchi del tipo =>
> 
> 1. **Crittoanalisi Differenziale** -> Si parla di attacco **chosen plain text** ma richiede 2^47 coppie <m, c>
> 2. **Crittoanalisi Lineare** -> Si parla di attacco **know plain-text** dove il numero di coppie <m, c> scende a 2^43. Attacca alcuni bit della chiave con un brute force mentre altri gli infierisce mediante un'**approssimazione lineare della S - BOX**
> 

---