# Metrics 📡

#metrics #router #switch #switching #throughput #bit-rate #bandwidth #goodput #latenza

> [!IMPORTANT]
> 
> ***Definition of Metrics***
> 
> Per **misurare** le **prestazioni** della **rete** usiamo delle metriche quali =>
> 
> - **Availability** -> La **Disponibilità** indica la **percentuale di tempo** in cui un sistema di rete, un componente o un'applicazione è **disponibile per l'utente**. Dipende dall'affidabilità dei singoli componenti della rete. La disponibilità si calcola con la formula ->
> 
> *Disponibilita* = *MTBF / ( MTBF + MTTR )​*
> 
> > **MTBF** = tempo medio tra i guasti ( _Mean Time Between Failures_ )
> 
> > **MTTR** = tempo medio per la riparazione ( _Mean Time To Repair_ )
> 
> - **Bandwidth** -> Larghezza dell'intervallo di frequenze utilizzato dal mezzo trasmissivo ( misurato in *Hertz* oppure cicli per secondo -> *Hz / secondo* ) -> **Velocità teorica**
> - **Throughput** -> Quantità di dati che vengono effettivamente trasmessi con successo. Non dipende solo dalla velocità ma anche dalla quantità di dati, dai protocolli, dal mezzo trasmissivo etc... -> **Velocità effettiva**
> -  **Goodput** -> Quantità di dati utili che vengono effettivamente trasmessi con successo -> **Velocità utile**. Viene usato per scoprire connessioni TCP sospette. Viene considerato solo il payload del pacchetto di livello 7
> - **Bit-rate** -> Quantità di bit trasmessi nell'unità di tempo ( *bits / secondo oppure bps* ) 
>
> <p align="center"><img src="img/Screenshot 2024-01-21 190400.png" /></p>
> <p align="center"><img src="img/Screenshot 2024-01-21 191029.png" /></p>
>
> Minimo per l'effetto ***Bottleneck Link*** => *link sul path end-to-end che limita il throughput end-to-end*
> 
> - **Latenza** -> Tempo richiesto affinché un messaggio arrivi a destinazione dal momento in cui il primo bit parte dalla sorgente. Utilizzando la commutazione di pacchetto, questo tempo è dovuto ai ritardi causati da ->
> 1. **Ritardi di elaborazione del nodo** -> Controllo errori sui bit e determinazione del canale di uscita
> 2. **Ritardi di accodamento** -> Attese di trasmissione e intensità del traffico
> 3. **Ritardo di trasmissione** -> Tempo impiegato per trasmettere il pacchetto sul mezzo trasmissivo ( *L/R* ovvero lunghezza del pacchetto / rate del collegamento => bit / bps )
> 4. **Ritardo di propagazione** -> Tempo impiegato da 1 bit per raggiungere l'altro nodo ( d/s ovvero lunghezza del collegamento / velocità di propagazione del mezzo ( 3 \* 10^8 m/sec ) )
> 
> <p align="center"><img src="img/Screenshot 2025-05-04 100140.png" /></p>
> <p align="center"><img src="img/Screenshot 2024-01-21 192647.png" /></p> 
> 
> Trascurando il ritardo di elaborazione e di accodamento, un **Ritardo end-to-end** =>
> 
> <p align="center"><img src="img/Screenshot 2024-01-21 194748.png" /></p>
>
> - Prodotto **Rate \* Ritardo** -> Numero massimo di bit che il link può contenere ad un certo istante
> 
> <p align="center"><img src="img/Screenshot 2024-01-22 115159.png" /></p>
> 
> *Se ho un link con un rate di 1bps e un ritardo di 5 secondi* => volume = 5 => 5 è il massimo numero di bit che possono riempire il collegamento e non possono esserci + di 5 contemporaneamente
> 
> <p align="center"><img src="img/Screenshot 2024-01-22 115443.png" /></p>
> 
> - **RTT ( Round-Trip Time )** -> L’RTT è il tempo che un pacchetto impiega per viaggiare dalla sorgente alla destinazione e poi ritornare alla sorgente. È una misura comune utilizzata per valutare la latenza di una rete e può essere influenzato da vari fattori, come la distanza fisica tra i nodi, la congestione della rete, e la qualità della connessione. Sappiamo inoltre che in base alla distanza e all' RTT, viene regolata la windows possibile per trasmettere i dati
> 
> <p align="center"><img src="img/Screenshot 2025-05-04 100554.png" /></p>
> 
> - **Utilisation** -> **L'utilizzo** è una misura più dettagliata rispetto alla **throughput**. Indica la **percentuale di tempo in cui una risorsa è effettivamente in uso** in un dato intervallo di tempo. Un valore molto basso di utilizzo può essere un segnale che qualcosa non sta funzionando correttamente ( _poco traffico perché il file server è andato in crash_ )
> 
> - **Jitter** -> E' la **variazione nel ritardo tra pacchetti consecutivi** su un collegamento in una sola direzione. E' un parametro molto importante per le applicazioni multimediali, come le **telefonate via Internet** o le **trasmissioni video**. In sostanza, il **jitter misura quanto è irregolare il flusso di dati** -> Nella telefonia il Jitter deve essere molto basso
> 
> <p align="center"><img src="img/Screenshot 2025-05-04 101629.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-05-04 101731.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-05-04 101839.png" /></p>
>

> [!IMPORTANT]
> 
> ***Other Metrics***
> 
> <p align="center"><img src="img/Screenshot 2025-05-04 102746.png" /></p>
> 
> - **Mean** -> La **media** è una misura di tendenza centrale che rappresenta il valore medio di un insieme di dati. Nel caso dell'RTT, la media rappresenta il tempo medio che un pacchetto impiega per viaggiare dalla sorgente alla destinazione e ritorno
> 
> - **Variance** -> Media dei quadrati degli scarti dalla media. Ovvero la media delle somme delle differenze al quadrato tra valore misurato e media 
> 
> - **Standard Deviation** -> La radice della varianza. La **deviazione standard** è una misura che indica quanto i valori di un insieme di dati si discostano dalla media. Una bassa deviazione standard indica che i valori sono vicini alla media, mentre una deviazione standard elevata implica che i valori sono sparsi su un ampio intervallo. Nel caso dell'RTT, una deviazione standard bassa indica che la latenza è consistente e stabile, mentre una deviazione standard alta potrebbe suggerire variabilità nella connessione o la presenza di problemi di rete
> 
> - **Outlier** -> Un **outlier** è un dato che si discosta significativamente dal resto dei dati in un insieme. In un contesto di RTT, un outlier rappresenta un valore di latenza che è notevolmente più alto o più basso rispetto alla maggior parte degli altri valori raccolti. Gli outlier possono essere causati da vari fattori, come errori di rete, congestione o malfunzionamenti di qualche nodo della rete. È importante identificare e trattare questi valori, in quanto possono influire negativamente sulle analisi statistiche, portando a conclusioni errate
> 
> Per trovare gli outlier, una formula tipica è la seguente ( **IQR** = Q3 - Q1 ) ->
> 
> - **Lower-bound** = Q1 - 1.5 \* IQR
> - **Upper-bound** = Q3 + 1.5 \* IQR
> 
> - **Percentile** -> **Percentile**: è il valore sotto il quale cade una certa percentuale di osservazioni in un insieme di dati ( _il **95° percentile** indica il valore sotto cui ricadono il 95% delle osservazioni_ )
> 
> Nelle reti Il 95° percentile è importante perché rappresenta il consumo massimo "tipico" ->
> 
> > Per il 95% del tempo, l'utilizzo rimane al di sotto di quel valore
> 
> Gli operatori lo usano per limitare i costi. Se si supera solo raramente quel valore, forniscono meno banda in media, risparmiando risorse
> 
> - **Quartiles** -> Divido la mia serie in percentili noti
> 
> <p align="center"><img src="img/Screenshot 2025-05-04 104245.png" /></p>
> 
> - **Z-Score** -> Lo **z-score**, noto anche come **punteggio standardizzato**, è una misura statistica che indica di quante deviazioni standard un dato valore si discosta dalla media di un set di dati. È definito come ->
> 
> z = *( x - media ) / deviazione*
> 
> dove x è il valore osservato. Lo z-score è utile per identificare valori anomali in una distribuzione normale, poiché valori di z che si discostano significativamente dalla media ( _|z| > 2_ ) possono essere considerati fuori dal normale intervallo di variabilità. Questo è particolarmente utile nell'analisi dei dati per rilevare outliers
> 
> <p align="center"><img src="img/Screenshot 2025-05-04 101018.png" /></p> 
>

> [!TIP]
> 
> #mind
> 
> Praticamente per capire la **latenza** possiamo usare il comando ==**tracert**== che manda dei pacchetti verso la destinazione e ci mostra quanto tempo serve per raggiungere tutti i nodi
> 
> <p align="center"><img src="img/Screenshot 2024-01-21 195854.png" /></p>
> 

---