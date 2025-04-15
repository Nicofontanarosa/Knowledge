#enigma #alberti #plugboard #rotors #colossus

> [!quote]- Born of Enigma
> 
> Enigma è stata la **prima evoluzione** verso i sistemi **automatizzati**. Ha avuto un ruolo essenziale nella **II WW**. Nasce in **Germania** nel **1918** come un' **estensione elettromeccanica del cifrario di Alberti**
> 

> [!key]- How Enigma is made
> 
> ![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-10-11 105018.png]]
>
>![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-10-11 105047.png]]
>
>![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-10-11 105117.png]]
>
>![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-10-11 105143.png]]
>
>![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-10-11 105209.png]]
>
>![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-10-11 105233.png]]
>
>![[AEv2rt5_Cryptography 🔢/img/Screenshot 2023-10-11 114002.png]]
> 

> [!radar]- External View
> 
> Presenta una **tastiera** con **26 lettere** e 26 **lampadine** corrispondenti alle lettere. Sulla tastiera si batteva il **messaggio** e le lampade indicavano il **crittogramma** che andava creandosi carattere per carattere. Dopo aver spedito il messaggio tramite radio o telegrafo, il destinatario riscriveva il crittogramma e si illuminavano le lampadine del messaggio originale ( **reversibile** )
> 

> [!radar]- Internal Rotors
> 
> **Tre dischi** di gomma rigida ( **rotori** ) montati sullo stesso asse ma liberi di **ruotare** in maniera **indipendente** e un **disco fisso** ( **riflettore** ). Ogni disco era collegato attraverso dei contatti elettrici. Le due facce di ogni rotore erano dette **pad e pin**, con 26 contatti elettrici disposti a cerchio che venivano **permutati in maniera fissa** ( *alla produzione di una coppia di macchine enigma* ) cioè dopo 1 giro completo del primo rotore girava il secondo con uno shift del primo etc ...
> 
> ***Chiavi*** = 26 x 26 x 26 = ***17576 chiavi diverse***
> 
> Possiamo intuire che questo modo di usare la macchina Enigma è **molto debole** poiché uso sempre la **stessa permutazione** ( *Alberti aveva previsto un cambio di chiave* )
> 
> Con la possibilità di ruotare i tre rotori in maniera casuale ->
> 
> ***Chiavi*** = 3! x 26^3 > ***10^5***
> 

> [!radar]- Plugboard
> 
> Con questa aggiunta è possibile **scambiare** tra loro i **caratteri** di 6 coppie scelte arbitrariamente in ogni trasmissione. Ogni cablaggio è descritto da una sequenza di 12 caratteri ( 6 coppie )
> 
> ***Chiavi ≈ 10^7*** ma dato che ogni gruppo di caratteri su può presentare in 6! permutazioni diverse ma non tutte producono effetti diversi ( AB e BA oppure CD e DC ) quindi 64 permutazioni se ne vanno ->
> 
> ***Chiavi ≈ 10^7 x 6!/64 > 10^15***
> 

> [!quote]- Events of Enigma
> 
> *Durante la IIWW i tedeschi dotarono le macchine Enigma di 8 rotori diversi di cui ne venivano montati 3 alla volta; le posizioni iniziali dei rotori furono scelte arbitrariamente; nel Plugboard venne incrementato a 10 il numero di coppie scambiabili*
> 
> *Si aveva un elenco di chiavi giornaliere ( in dotazione ai reparti militari ) crittate con la chiave del giorno prima ( con cui si scambiavano la configurazione di quel giorno )*
> 

> [!quote]- Ends of Enigma
> 
> *Alcuni eccellenti matematici, prima polacchi e successivamente inglese, studiarono come rompere Enigma. In Inghilterra fu costruito il centro di Bletchley Park per la decrittazione delle comunicazioni radio tedesche*
>
> *Si costruì un simulatore di Enigma chiamato* ***Colossus*** *nel 1944 con l'aiuto principale di Alan Turing, usato come base per i successivi calcolatori elettronici*
> 

---