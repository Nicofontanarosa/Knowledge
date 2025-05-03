#packet #analysis #time_series #RRD

> [!IMPORTANT]
> 
> ***Definition of Time Series***
>
>  - **Serie** -> Sequenza ordinata di numeri
>  - **Ordine** -> L'indice dei numeri della serie ( *serie ordinata per tempo -> indice è il tempo* )
>  - **Time Series** -> Serie ordinata per tempo
>  - **Osservazione** -> Il valore numerico osservato reale in uno specifico momento
>  - **Forecast** -> La previsione è la stima del valore atteso in un specifico tempo che noi però ancora non conosciamo
>  - **Forecast Error** -> L'errore di previsione, ovvero un valore che si discosta dall'effetivo misurato. Solitamente questo errore è mostrato al quadrato per non avere un errore negativo ma quindi un ***discostamento*** che per sua natura è positivo
>  - #SSE -> <mark>**Sum of Square Error**</mark> sarebbe la differenza al quadrato del valore misurato e valore predetto -> <mark>***Confrontare serie temporali***</mark>
>
> <p align="center"><img src="img/Screenshot 2025-03-27 130447.png" /></p>
> 
> La differenza tra le **serie univariate** e le **serie multivariate** è che nella prima riesco a prendere tutte le informazioni che mi servono mentre le seconde devo prima unirle in qualche modo e poi prendere i dati. Quindi nel primo ho un valore **Scalare**, nel secondo ho vari valori che devono essere messi assieme
> 
> > *Each variable depends not only on its past values but also has some dependency on other variables*
> 
> <p align="center"><img src="img/Screenshot 2025-03-28 114710.png" /></p>
> 
> Le **serie temporali** possono essere ***stazionarie e non***. Le prime sono serie dove è stata fatta la differenza tra i valori e quindi sono state normalizzate è hanno un andamento che possiamo studiare. Quelle non stazionarie hanno il valore gouge o counter grezzo
> 
> <p align="center"><img src="img/Screenshot 2025-03-28 165645.png" /></p>
> 
> Guardando una serie stazionaria possiamo distinguere vari campi ovvero **la stagionalità, la tendenza e l'errore** ( *traffico dell'università di pisa -> season = 5/7g è forte mentre il weekend no, la tendency = i mesi di lezioni + forte delle vacanze, error = ci sono dei giorni di festa dove mi sbaglio nel predire un valore* )
> 
> Ci sono due metodi per predirre i valori nelle serie temporali -> **Additivi e Moltiplicativi**
> 
> <p align="center"><img src="img/Screenshot 2025-03-28 170046.png" /></p>
> 
> Gli additivi sono buoni se la stagione e la tendenza sono + o - costanti mentre i moltiplicativi tengono conto di un cambio di questi 2 parametri. I metodi principali per predire i valori futuri sono =>
> 
> - #ETS **Error, Trend and Seasonality** -> Algoritmi del tipo *Single Exponential Smoothing, Double Exponential Smoothing & Triple Exponential Smoothing*
> - #ARIMA **AutoRegressive Integrated Moving Average** -> Algoritmo che tiene presente alla regressione della variabile ( *tiene conto del recente passato quindi* ) e tiene conto anche degli **errori** per **adattarsi**
> - #Prophet ( #Meta ), #TimeGTP ( *AI-based* )
> 
> Notazione -> Y^x+1 è il valore Y predetto ^ nell'istante x+1
> 
> La previsione più facile è la **media** oppure la **media mobile** ovvero una media che tiene in considerazione i valori + recenti per calcolare la media e quindi considera l'andamento, oppure la **media pesata**
> 
> <p align="center"><img src="img/Screenshot 2025-03-28 174541.png" /></p>
>  
> - <mark>**Single Exponential Smoothing**</mark> -> Non è una vera e propria predizione ma una funzione di verifica con "a" un valore compreso tra 0 e 1 in base a se voglio dare più importanza al valore precedente o no
> 
> <p align="center"><img src="img/Screenshot 2025-03-28 174809.png" /></p>
> 
> > *α: smoothing factor or “memory decay rate”: the higher α, the faster the method forgets*
> 
> <p align="center"><img src="img/Screenshot 2025-03-28 175646.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-03-28 175812.png" /></p>
> 
> Tramite questo appen è possibile usare questa formula per predirre il valore successivo
> 
> - <mark>**Double Exponential Smoothing**</mark> -> Introduco B ovvero il trend ovvero quanto sarà inclinata la mia funzione di previsione per il prossimo punto
> 
> <p align="center"><img src="img/Screenshot 2025-03-28 181821.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-03-28 182122.png" /></p>
> 
> - <mark>**Triple Exponential Smoothing**</mark> -> Introduco la stagionalità ovvero la ripetizione del segnale ad intervalli stabiliti quindi posso prevedere n punti a piacere. Ovviamente più vado lontano più aumenta l'errore perché mi baso solo sulla stagionalità
> 
> <p align="center"><img src="img/Screenshot 2025-03-28 182729.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-03-29 082334.png" /></p>
> 
> Interpretazione grafica life [qui](ExponentialSmoothing.xlsx)
> 
> Un <mark>***Anomalia***</mark> è un valore che esce dalla mia previsione. Nella maggior parte del tempo però, picchi nei valori non sono anomalie ( *tutta la classe visita un web detto dal prof* ) -> Un sintomo solo non è sufficiente per alzare la bandierina di allarme. Definiamo quindi un **upper bound e lower bound**
> 
> - Un **problema** è che questi allarmi non possono essere fatti sui **byte grezzi** perché non avrebbe senso, se iniziassi a scaricare un file avrei un allarme scattato
> - Un altro **problema** è che questi algoritmi **imparano dalla storia** quindi se l'**anomalia** diventa la **normalità** possono dare non pochi problemi 
> 
> <p align="center"><img src="img/Screenshot 2025-03-29 090544.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-03-29 090715.png" /></p>
> 

> [!TIP]
> 
> ***How to RRD tool Database work***
> 
> Per lavorare sulle serie si usano dei database per immagazzinare i valori. Il problema dei database classici è che se volessi query del tipo: *Questi valori somigliano a quali valori?* dovrei prendere ogni singolo campo alla volta e confrontarlo con altri e vedere se ce una correlazione quindi nxm confronti. Quindi calcolare l'SSE per ogni campo
> 
> Un altro problema è la cardinalità che nel tempo cresce e le dimensioni diventano improponibili. Per riassumere e unire più valori si esegue un operazione chiamata di ***Roll-up*** dove si comprimono i dati tramite un qualche criterio ( *sappiamo il meteo nell'anno 1971 ma non dei singoli giorni o mesi* )
> 
> ***Round Robin Database*** è un database circolare per serie temporali che arrivato alla fine si sovrascrive. Questo database è su File, quindi gira in locale ( *tipo* #sqlite ). Il file RDD è composto dai seguenti campi
> 
> <p align="center"><img src="img/Screenshot 2025-03-27 162459.png" /></p>
> 
> dove negli archivi memorizzo una delle metriche che sto misurando
> 
> <p align="center"><img src="img/Screenshot 2025-03-27 163008.png" /></p>
> 
> Una volta creato il database, la grandezza è fixed, quindi alloco già lo spazio necessario per andare a riempire gli slot che si è calcolato con i dati misurati. *Cosa succede se non misuro in un certo istante di tempo ?* Definisco un tipo che è **UNKNOW** che ***NON E' 0***. Quando misuro i valori da scrivere potrei non tener conto del tempo di scrittura di una macchina, oppure se ne va la corrente etc ... Quindi non riesco a scrivere il tempo in tempo => Non scrivo il valore oppure scrivo una stima del valore ( *dipende* )
>
> <p align="center"><img src="img/Screenshot 2025-03-27 163451.png" /></p>
> 
> Dove il valore che posso scrivere notiamo che può scendere sia perché ha <mark>**wrappato**</mark> sia perché il dispositivo è stato **spento** oppure è stato resettato
> 
> I valori che vado a scrivere all'interno di questi database sono valori **normalizzati**, ovvero come prima cosa scrivo valori che sono la differenza del numero misurato con il precedente e poi per comprimerli posso fare una media oppure il massimo o il minimo ( *dipende da cosa voglio ottenere* )
> 
> <p align="center"><img src="img/Screenshot 2025-03-27 182713.png" /></p>
> 

> [!IMPORTANT]
> 
> ***How RRD Works***
>
>  <p align="center"><img src="img/Screenshot 2025-03-27 184559.png" /></p>
>  
>  - **Nome_file.rrd** 
>  - **Step** -> Grandezza in tempo di un singolo slot di un array
>
> **DS:ds-name:GAUGE | COUNTER | DERIVE | ABSOLUTE:heartbeat:min:max**
> 
> - **Tipo di dato** -> GAUGE si aspetta un valore pronto mentre COUNTER si aspetta di fare una sottrazione con il precedente misurato. Nel caso il valore non arriva viene messo **Unknow**
> - **Heartbeat** -> Tempo massimo per aspettare i valori da ricevere
> - **Min - Max** -> Minimo e massimo valore scrivibile nel file
> 
> **RRA:AVERAGE | MIN | MAX | LAST:xff:steps:rows**
> 
> - **RRA** -> Indica come voglio usare i miei dati, quindi facendo il massimo, minimo etc ... Memorizzandoli in archivi
> - **Xff** -> Significa che se + del Xff dei valori è unknow, non esegui calcoli per quello slot
> - **Steps** -> Quanti valori dell'archivio soprastante sono usati per calcolare l'archivio tramite l'operazione definita in RRA
> - **Rows** -> La grandezza del mio archivio moltiplicata per lo **step**
> 
> L'esempio in questione significa => Ogni 5 minuti ( *300 secondi* ) catturi un valore di tipo GAUGE. Attendi massimo 500 secondi altrimenti scrivi UNKONW. I valori massimi e minimi scrivibili sono 300 e 0. Di tutti questi valori che misuri memorizzali in un archivio dove fai la **media di un valore** cioè sostanzialmente copia se stesso, infatti era indifferente usare MAX o MIN, e memorizza 120 di questi valori => l'archivio diventa grande 120 x 5m = 10 ore. Da questo archivio creane un altro dove fai la media di 12 slot alla volta e mantieni 96 di queste medie => 12 x 5m ( *5 minuti perchè i valori memorizzati nell'archivio di sopra sono 1 ogni 5 minuti* ) = 1h e di questi nuovi valori memorizzane 96 => 96h complessive. 0.5 è la soglia dalla quale non considerare più i valori per i calcoli ( *se ho soglia 0.5 => 1 2 3 unknow = mi scrive 2, se ho 1 unknow unknow unknow = mi scrive unknow* )
> 
> <p align="center"><img src="img/Screenshot 2025-03-27 190830.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-03-27 191255.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-03-27 192342.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-03-28 123620.png" /></p>
> 
> L'ultimo comando serve per inserire i dati all'interno dove **N** è il tempo di quel momento preso però lo prende da locale. perché è dispendioso prendere il tempo dal dispositivo destinatario, occupa banda, scarica il dispositivo ... Nella **realtà** non sono io che ogni 5 minuti vado a prendere la misura ma vado a prendere ogni 1 ora le misure fatte ogni 5 minuti dall' host locale x quell'ora
> 
> L'inserimento in questo database funziona in questo modo =>
> 
> Quando inserisco i dati, l'ordine di inserimento è importante perché le differenze che mi portano ad eventuali buchi me le calcola in ordine e se faccio una differenza dove non ho un valore e dopo 1 ora mi arriva quel valore => non posso tornare indietro per ricalcolare la differenza. Esistono database che consentono di inserire i valori nell'ordine che voglio e poi quando estraggo i valori me li raggruppa per tempo e mi esegue le differenze
> 
> Il valore che si scrive poi, è un valore che viene scritto non intero come viene misurato ma spalmato nell'intervallo ( *banalmente se ho 5 secondi di intervallo => il valore non lo scrivo in 1 intero ma in più interi diventando a virgola mobile* ). Negli anni futuri si è dimostrato che se i numeri interi vengono convertiti in virgola mobile => riesco a comprimerli molto + efficientemente
> 

---