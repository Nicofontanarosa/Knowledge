
#curves  #elliptical #cryptography #ECC #encryption #asymmetric_algorithms #RSA #NIST #DH

> [!key]- Definition of ECC
> 
> Il **cifrario RSA** richiede** tempi** di cifratura e decifratura **notevolmente maggiori** di quelli dei **cifrari simmetrici**; inoltre il carico computazionale si è ulteriormente aggravato col progressivo **aumento** della lunghezza della **chiave**
> 
> Oggi si utilizzano chiavi di **1024 bit** ma il NIST raccomanda chiavi di **2048** per **proteggere** messaggi fino al **2030**
> 
> La crittografia a curve ellittiche offre tempi e prestazioni migliori dei cifrari a chiave pubblica quali RSA e protocollo DH per lo scambio di chiavi
> 
> A **parità di sicurezza** questo tipo di crittografia richiede **chiavi** di dimensioni di grand lunga **inferiori**
> 

> [!radar]- AES vs RSA vs ECC
> 
> ![[Screenshot 2023-11-11 094259.png]]
> 

---
#elliptical #curves #XIX #IBM #Victor_Miller #Neil_Koblitz
## Elliptical Curves

> [!key]- Definition of Elliptical Curves
> 
> Sono ***curve algebriche descritte da equazioni cubiche*** simili a quelle utilizzate per il calcolo della lunghezza degli archi delle ellissi
> 
> *La loro applicazione nel mondo crittografico è molto recente anche se introdotte nella prima metà del XIX secolo*
> 
> I **primi** a proporre **algoritmi** di cifratura basati su curve ellittiche furono **Victor S. Miller** ( IBM ) e **Neil Koblitz** ( University of Washington ) nel **1985** e proposero di sostituirli agli algoritmi basati sull'algebra modulare
> 

> [!terminal]- Example
> 
> ![[Screenshot 2023-10-31 220312.png]]
> 

> [!brain]- Impressions
> 
> #mind
> 
> Come tutti i problemi in crittografia, occorre usare una funzione **one-way trap-door** che ne garantisce la sicurezza. Nelle curve ellittiche usiamo il problema del **logaritmo discreto** che è più difficile del suo analogo nell' algebra modulare poiché attacchi quali #GNFS e #index-calulus non hanno effetto 
> Proprio perché più difficile, la **lunghezza** della **chiave** viene **ridotta** notevolmente quasi a livello degli algoritmo simmetrici. Questa difficoltà è dipesa anche dal fatto che i campi su cui sono definite le curve ellittiche hanno 2 operazioni principali ( *prodotto tra punti e valore e somma di punti* ) che non permettono di giocarci algebricamente
> 

---
