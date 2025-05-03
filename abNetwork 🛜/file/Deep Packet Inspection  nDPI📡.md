#nDPI #packet #monitor #monitoring 

> [!NOTE]
> 
> ***What is Deep Packet Inspection***
> 
> Sappiamo che un #IDS **Intrusion Detection System** è un sistema per identificare le minacce pre - post attack. Un problema dei IDS tradizionali è quando i dati sono **cifrati**
> 
> <p align="center"><img src="img/Screenshot 2025-04-11 120804.png" /></p>
> 
> Questo Signature IDS è facile da implementare ma difficile da leggere e la seconda regola che dice la stessa cosa della prima, è facilmente passabile poiché non guarda il flusso TLS ma dei byte di un pacchetto, che posso modificare nei pacchetti che genero
> 
> Qui entra in gioco il **Machine Learning** che riesce a capire se un dato cifrato è effettivamente una minaccia, ma per fare questo ha bisogno di essere allenato, ovvero ha bisogno di **dati certi** ( *se non ho questi dati rischio di produrre delle allucinazioni* )
> 
> Il problema di oggi è andare a proteggere l'**edge-network** ovvero la periferia della rete che non devono propagare i problemi nel **Core** poiché nodi lenti e infetti possono rallentare a cascata tutti i vicini. Oggi giorno non basta vedere quanto traffico stai facendo poiché il traffico che creiamo è parecchio, quindi dobbiamo **monitorare i pacchetti in una maniera non abusiva, leggera, passiva e scalabile** tenendo traccia del cambiamento di comportamento di un dispositivo
>

> [!IMPORTANT]
> 
> ***nDPI***
>
>  E' un tool che permette di identificare il traffico di rete =>
>
> <p align="center"><img src="img/Screenshot 2025-04-11 123237.png" /></p>
> 
> Permettendo di andare a capire se qualcuno sta generando del traffico indesiderato in un determinato contesto. Quindi riesco a capire se un determinato **protocollo** è consentito o meno in un contesto. In nDPI un protocollo non è uno standard dell'RFC ... ma è creato partendo da un protocollo precedentemente creato e rappresentato tramite =>
> 
> - **< major >.< minor >** tipo DNS.Facebook o QUIC.YouTube and QUIC.YouTubeUpload
> 
> Molti applicativi infatti usano + protocolli in base alla funzione richiesta ( *Skype or Facebook are application protocols in the nDPI world but not for IETF* ) e nDPI riconosce + di 300 protocolli
> 
> Oggi quasi tutti i protocolli applicativi si basano su HTTPS / TLS e quindi bisogna ricavare le informazioni per riconoscere il protocollo da quei pochi dati non cifrati in TLS. In base al tipo di traffico i <mark>***dissector***</mark> vengono applicati in sequenza a partire da quello che con maggiore probabilità corrisponde al flusso ( *per TCP/80 si prova prima il dissector HTTP* )
> 
> - Ogni flusso mantiene lo stato dei dissector che non hanno corrisposto, in modo da saltarli nelle iterazioni successive
> - L'analisi continua fino a quando non viene trovata una corrispondenza o dopo troppi tentativi
> 
> Queste parti che nDPI prende dai <mark>**primi pacchetti di un flusso**</mark> per ricavare informazioni sono parti **non cifrate** ma **offuscate** ( *sulla parte cifrata non si può fare nulla* )
> 
> <p align="center"><img src="img/Screenshot 2025-04-11 161449.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-04-11 161554.png" /></p>
> *prestazioni nDPI*
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
> - <mark>**Compressed Bitmap**</mark> -> Una bitmap compressa dove **conto le ripetizioni** con algoritmi del tipo **WAH, EWAH, COMPAX etc ...** Una delle + usate è la [[https://roaringbitmap.org/]]
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
> - <mark>**Probabilistic Counting**</mark> -> *Come faccio a capire con quianti indirizzi IP diversi ho parlato?* Devo avere un contatore che conta ogni volta che parlo con un indirizzo IP ma l'indirizzo IP, per vedere se ci ho già parlato, lo devo confrontare con qualche altro tenuto in memoria -> Dispendioso
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
> - **==Leaky Bucket==** -> Un modo per identificare eventi problematici. *How do I detect a port or network scan ? How can I detect trafﬁc bursts?*
> 
> - Hai un **secchio** con una **capacità massima** (es: 100 eventi)
> - Ogni volta che arriva un evento (es: una richiesta di connessione), **togli una goccia dal secchio**
> - Ogni tot tempo (es: ogni secondo), **il secchio viene ricaricato (riempito)**
> - Se **arrivano troppi eventi troppo in fretta**, il secchio si svuota velocemente
> - Quando il secchio è **vuoto** e arriva un altro evento → **allarme**: è un comportamento eccessivo
> 
> <p align="center"><img src="img/Screenshot 2025-04-16 125941.png" /></p>
> 
> - **==Entropia==** -> L'entropia, nel contesto delle reti, è una misura di **casualità o disordine** nei dati. L’entropia dei dati grezzi prima e dopo la cifratura **cambia**, ma **rimane entro certi limiti** se i dati sono **omogenei** e grazie a questi limiti è possibile classificare il traffico anche se cifrato
> 
> <p align="center"><img src="img/Screenshot 2025-04-16 130400.png" /></p>
> 
> - **==Data Binning==** -> I bin permettono di classificare i dati usando un **piccolo insieme di intervalli**, invece di considerare **ogni valore singolarmente**. Questo non mi da i dati precisi però mi permette di rispondere a delle domande quali: *Due host stanno usando protocolli simili?* 
> 
> I **bin** sono un modo efficiente per memorizzare osservazioni, ma abbiamo bisogno di un modo per **confrontarli** e trovare **somiglianze**. Due **valori singoli** possono essere confrontati con gli operatori `<`, `>`, `=`,  ma *come si confronta un vettore di dati ?* => bisogna ->
>
> 1. **Normalizzare i dati** -> Per evitare che bin con più osservazioni “pesino” di più
> 2. **Confrontare** ogni elemento del bin con un **algoritmo di similarità**
>
> Ci sono vari metodi per confrontare gli elementi ( *distanza del coseno, distanza euclidea etc ...* ). Un criterio oggi usato è il **==Coefficiente di Similarità di Jaccard==** ->Jaccard(A, B) = ( numero di elementi comuni ) / ( numero totale di elementi unici ) e produce un valore tra 0 e 1 dove 0 = completamente diversi mentre 1 = identici
> 

---