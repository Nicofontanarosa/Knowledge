#data #clustering #network #networks #AI #markov

> [!NOTE]
> 
> ***Note of Artificial Intelligence***
> 
> Il mondo dell’intelligenza artificiale è suddiviso in due principali famiglie =>
> 
> - **Supervisionato** -> In questo approccio, l’AI viene "addestrata" fornendo esempi con etichette corrette. Possiamo immaginarlo come un array di pesi in cui ad ogni input corrisponde un output atteso. Le aree più esplorate nell’ambito supervisionato sono **testo** e **immagini**
> - **Non supervisionato** -> Qui si utilizzano algoritmi di **clustering**, che raggruppano elementi simili tra loro per poi confrontare nuovi dati con questi gruppi ( *cluster* ). Questo approccio è spesso usato, ad esempio, per analizzare **immagini del traffico di rete** ( *non immagini grafiche, ma rappresentazioni di pacchetti o pattern di comunicazione* )
> 
> Esistono diversi tipi di algoritmi di clustering. Il + semplice è il <mark>**K-means**</mark> che suddivide gli elementi in _K_ cluster _( nDPI adatta questo algoritmo per produrre al massimo K cluster e non esattamente K ) 
> 
> <p align="center"><img src="img/Screenshot 2025-04-16 151758.png" /></p>
> 
> Il <mark>**DBSCAN**</mark> ( **Density-Based Spatial Clustering of Applications with Noise** ) invece trova automaticamente **quanti cluster** ci sono, basandosi sulla **densità** dei punti. Più intelligente, ma **più complicato** da usare e regolare
> 

> Catena di Markov
> 
> È un **modello probabilistico** in cui la **prossima azione** ( *stato futuro* ) dipende **solo** dallo stato attuale, **non dalla storia completa**. In pratica -> Cosa succede dopo dipende solo da dove sono ora, non da come ci sono arrivato
>

---