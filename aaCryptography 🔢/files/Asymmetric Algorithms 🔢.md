# Asymmetric Algorithms 🔢

#TTP 

> [!NOTE]
> 
> ***Definition of Trusted 3rd Party***
> 
> Per scambiarsi le chiavi prima di ovviare all'utilizzo di una chiave pubblica e una chiave privata, la gente si affidava a delle organizzazioni che comunicavano le chiavi da utilizzare ( *simmetriche* ) da utilizzare per il DES o AES ...
> 
> Ogni utente doveva memorizzare solamente la chiave che serviva per comunicare in sicurezza con la TTP
> 

> [!TIP]
> 
> Alice vuole comunicare con Bob che usando una TTP che conosce la chiave Ka e Kb =>
> 
> 1. Alice avvisa al TTP che ha intenzione di avviare la comunicazione con Bob
> 2. TTP genera casualmente la chiave Kab
> 3. TTP manda ad alice c(Kab, Ka) e a bob c(Kab, Kb)
> 4. Alice e Bob iniziano la comunicazione con la chiave Kab
> 

> [!CAUTION]
> 
> **TTP dovrà essere sempre online e dovrà conoscere tutte le chiavi** 
>  

---
#asymmetric_algorithms #DH

> [!IMPORTANT]
> 
> **Definition of Asymmetric Algorithms**
> 
> #Diffie #Hellman #Merkle Introdussero il concetto di cifrari a chiave pubblica, ovvero un modo per scambiare una chiave segreta su un canale insicuro
>
> In questi algoritmi il mittente non deve comunicare con il destinatario per accordarsi poiché utilizza la chiave pubblica del destinatario
> 
> Le due chiavi devono essere tra loro indipendenti, in modo che dalla prima non si possa in nessun modo ricavare la seconda => D( C( m, Kpub ), Kpriv ) = m dove ->
> 
> 1. < Kpub, Kpriv > è facile da generare e deve essere impossibile ai due utenti generare la stessa chiave
> 2. D e C sono facili da calcolare conoscendo Kpriv e Kpub ma difficili se non si conosce la chiave => C = **One - Way Trap - Door**
> 
> <p align="center">
 <img src="img/Immagine 2022-02-03 170205.jpg" />
</p>

> [!IMPORTANT] 
> 
> *Per n utenti avrò bisogno di 2n chiavi ( coppia <Kpub, Kpriv> )*
> 
> Con la crittografia asimmetrica -->
> 
> - Si risolve il problema della **riservatezza** del messaggio dato che solo il possessore della chiave primaria è in grado di decifrare il messaggio e garantisce l' **autenticità del mittente**
>  

<p align="center">
 <img src="img/Immagine 2022-02-09 185310.jpg" />
</p>

> [!IMPORTANT] 
> 
> Distinguiamo due modalità di funzionamento -->
> 
> - **Modalità confidenziale** --> sono garantite la riservatezza e l'inte![[AEv2rt5_Cryptography 🔢/img/Immagine 2022-02-09 185333.jpg]]e 2022-02-09 185310.jpg]]
> 
> - **Modalità autenticazione** --> garantisce l'autenticità del messaggio ma non viene garantita la riservatezza ( firma elettronica sul messaggio ) 
> 
> ![[Immagine 2022-02-09 185333.jpg]]
> 
> E' anche possibile garantire sia la riservatezza che l'autenticazione del messaggio combinando le due modalità
> 
 
> [!danger]+ 
> 
> Per loro natura però sono esposti ad attacchi di tipo ***chosen plain-text*** poiché il crittoanalista può decidere quale messaggio inviare al destinatario e confrontare tutti i messaggi cifrati con la chiave pubblica per trovare la chiave privata
> 
> Può succedere anche che un crittoanalista finga di essere Bob agli occhi di Alice e spaccia una chiave pubblica e privata che solo lui ha oppure si intromette nella comunicazione e quando Alice richiede la chiave pubblica di Bob, la sostituisce alla sua  
>  

---
#RSA #GNFS #padding
# RSA Algorithm

> [!quote]+ Quote
> 
> #Rivest #Shamir #Adleman
> 
> Le iniziali di RSA sono quelle dei nomi dei suoi creatori Rives, Shamir e Adleman al MIT, brevettato nel 1983
> 

> [!key]+ Definition of RSA Algorithm
> 
> L'algoritmo RSA lavora sfruttando i numeri primi e **n** ottenuto proprio dal prodotto di due numeri **p x q**. Il funzionamento è il seguente -->
> 
> - **A** deve spedire un messaggio criptato a **B**
> - **B** sceglie due numeri primi molto grandi e li moltiplica tra di loro ( chiavi )
> - **B** invia in chiaro ad **A** il numero che ha ottenuto
> - **A** usa questo numero per crittografare il messaggio
> - **A** manda il messaggio a **B**, che chiunque può vedere ma non leggere
> - **B** riceve il messaggio e utilizzando i due fattori primi che solo lui conosce, riesce a decifrare il messaggio
> 
> *Risalire a p e q conoscendo n è computazionalmente difficile* => **Fattorizzazione**
> 

> [!attention]+ Security of RSA Algorithm
> 
> La difficoltà nel forzare l' RSA sta nella fattorizzare un numero arbitrariamente molto grande, problema difficile, ma non come un tempo ->
> 
> - La potenza di calcolo aumenta ogni giorno
> - Esiste un algoritmo chiamato **GNFS** ( *General Number Field Sieve* ) che richiede O( 2^rad( b log b ) ) rispetto ad un forza bruta che ne impiega O ( 2^b ) =>![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-12-23 092334.png]]iprimi fino a 829 bit ( / 3 = 250 digit )
> - La fattorizzazione e il logaritmo discreto non sono problemi NP - Hard e si risolvono in tempo polinomiale su macchine quantistiche
> 
> ![[Screenshot 2023-12-23 092334.png]]
> 
> Per usare al meglio l' RSA vanno scelti *p* e *q* appuratamente ->
> 
>- p e q devono essere di almeno 1024 bit per resistere ad attacchi bruteforce
>- p - 1 che q - 1 devono contenere un fattore primo grande ( altrimenti n si fattorizza velocemente )
>- MCD( p - 1, q - 1 ) deve essere piccolo => (p - 1)/2 e (q - 1)/2 sono co - primi
>- Non riciclare gli stessi primi poiché se per 2 chiavi pubbliche è stato utilizzato lo stesso primo => eseguo il MCD (Kpub1, Kpub2) in tempo polinomiale e mi trovo uno dei 2 fattori primi
>- p e q non devono essere vicini altrimenti n ~ p^2 ~ q^2 => Brute Force su rad(n) -> **Wiener Attack**
>
>Anche *e* va scelta appuratamente ->
>
>- e != ( φ(n) + k ) / k con (p - 1)/k e (q - 1)/k non dia resto => k divide p e q - 1 poiché altrimenti il messaggio rimane invariato dopo la cifratura
>- *e* e *d* piccoli accellera la cifratura o decifratura ma se uno è piccolo => l'altro deve essere grande altrimenti basterebbero gli attacchi bruteforce
>- Se m ed e sono piccoli | m^e < n => si può ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-12-31 102055.png]] c = m^e ( *non interviene la riduzione in modulo* ) => **padding**
>- Se e utenti hanno scelto lo stesso valore di e, e ricevono lo stesso messaggio m attraverso i crittogrammi: 
>
>![[Screenshot 2023-12-31 102055.png]]
>
>per il **Teorema Cinese del Resto** si può calcolare in tempo polinomiale m' < n = n1 \* n2 \* n3 ... \* ne che soddisfi l'equazione m' = m^e mod n e successivamente si può intervenire con la radice e-esima che viene eseguita facilmente perchè non interviene l'operazione in modulo
> 

> [!Danger]+ Common modules attack
> 
> Cosa succede se due utenti usano lo stesso modulo?
>
> **Identità di Bezout** -> a e b due interi, d = MCD (a, b) => E x e y interi | ax + by = d e questo posso calcolarlo in tempo polinomiale
> 
> Supponiamo di avere due chiavi <n, e1> <n, e2> con MCD (e1, e2) = 1 => xe1 + ye2 = 1 =>
> Supponiamo che il crittoanalista intercetti il crittogramma dello stesso messaggio ma relativo a e1 e e2 =>
> c1 = m^e1 mod n e c2 = m^e2 mod n =>
> m = m^(e1x + e2y) = (c1^x \* c2^y) mod n = ((c1^-1)^-x \* c2^y) mod n con x < 0
> 

> [!Danger]+ Time attack
> 
> Si basa sul tempo di esecuzione dell'algoritmo di decifrazione => determino d analizzando il tempo impiegato per decifrare => Aggiungere un ritardo casuale per confondere l'attaccante
> 

---
#ELGamal
## ELGamal Cipher

> [!key]+ Definition of ELGamal Cipher
> 
> Sistema a chiave pubblica pubblicato nel 1985 basato sul problema del **logaritmo discreto** ->
> 
> 1. Si sceglie un numero primo p e un generatore g di Z*p
> 2. Si sceglie a caso un intero x in [2, p - 2]
> 3. Si calcola y = g^x mod p
> 4. Adesso la Kpub = <p, g, y> e Kpriv = \<x>
> 

> [!brain]+ Impressions
> 
> #mind
> 
> Gli algoritmi asimmetrici sono ottimi dal punto di vista algebrico e utili per lo scambio di chiavi ma =>
> 
> - #DH -> Non buono per attacchi di tipo #man-in-the-middle altrimenti bisognerebbe risolvere il logaritmo discreto che non è neanche troppo difficile però vi è la versione su #ecc + sicura
> - #ELGamal -> Basa la sua sicurezza sul logaritmo discreto  quindi non possiamo affidarci troppo a questo algoritmo ma vi è la versione su #ecc + sicura
> - #RSA -> Non c'è la versione su curve ellittiche poiché vi è già quella di ELGamal e basa la sua difficoltà sulla fattorizzazione di numeri e ben presto non potrà più essere usato. Recentemente è stato attaccato tramite #static_cryptanalysis e #ssh 
> 

---<p align="center">
<img src="./app/src/main/res/mipmap-xxxhdpi/grass_background.png" alt="Sheep" width="100" style=''>
<img src="./app/src/main/res/drawable/my_sheep.png" alt="Sheep" width="100" style=''>
<img src="./app/src/main/res/drawable/my_dark_sheep.png" alt="Sheep" width="100" style=''>
</p>
