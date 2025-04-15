
#quantum #cryptography 

> [!atom]- Definition of Quantum Cryptography
> 
> La crittografia applicata al nuovo mondo della fisica quantistica -> Lo studio di piccoli elementi studiati dall' uomo. E' definita da alcune leggi quali =>
> 
> 1. ***Legge di Sovrapposizione*** -> La proprietà per un sistema quantistico di trovarsi in diversi stati contemporaneamente -> ***superposizione***, tuttavia
> 2. ***Legge della Decoerenza*** -> Il tentativo di osservare o misurare lo stato del sistema ne determina il collasso verso uno stato singolo, ovvero il sistema, ormai disturbato, perde la sovrapposizione di stati e i dati che esso conteneva. Questo collasso è determinato anche dal più piccolo dei disturbi
> 3. ***Legge della Anti - Clonazione*** -> Impossibilità di clonare lo stesso stato quantico 
> 

> [!atom]- Entanglement
> 
> Un fenomeno abbastanza particolare =>
> 
> Riguarda la possibilità che due o + elementi si trovino in stati quantici completamente correlati tra loro in modo che, pure essendo trasportati a grande distanza uno dall'altro, mantengono le correlazioni 
> 
> 	Spooky action at a distance by Einstein 
> 

> [!note_qua]- Note
>
> Mentre i sistemi classici basano la loro computazione sui bit, i computer quantistici basano la propria esistenza sul ***qbit***, che non assume valore 0 o 1 ma si trova in sovrapposizione dei due stati. Dunque la rappresentazione dell'informazione è binaria ma ogni qbit non conterrà n bit di informazioni ma 2^n bit di informazioni anche se non posso osservarle tutte e 2^n poiché potrei portare ad un disturbo e perdere tutte quelle informazioni
> 

> [!atom]- Polarizzazione
> 
> Si assegna ad ogni fotone utilizzato un' informazione binaria e si usano per creare sequenze temporali ( di fotoni polarizzati )
> 

---
#BB84 #key #distribution #Charles_Bennet #Gilles_Brassard
## BB84 Protocol

> [!atom]- BB84 Protocol
> 
> Protocollo nato negli anni '80 per sostituire il protocollo DH e gli altri venuti per lo stesso scopo, scambiarsi chiavi. Il meccanismo dello scambio di chiavi tra due utenti mediante l'invio di fotoni polarizzati è noto come ***QKD*** ( **Q**uantum **K**ey **D**istribution ) nato nel 1984 da **Charles Bennet** e **Gilles Brassard** nominato BB84. La prima realizzazione pratica è avvenuta nel 2001 presso il CERN dove è stato effettuato uno scambio di chiavi lungo un cavo di fibra ottica lungo 67km da Ginevra a Losanna
> 
> ![[Screenshot 2023-12-20 230551.png]]
> 

> [!brain]- Impressions
> 
> #mind
> 
> *L'unica sicurezza che ha il protocollo BB84 è quella della generazione dei bit casuali per stabilire le basi, sia di Alice che di Bob. Se un crittoanalista riuscisse a capire i bit generati casualmente da Alice o da Bob capirebbe le basi da usare per passare inosservato e captare una buona parte dei bit di chiave*
> 
> Questo protocollo infatti viene usato per scambiarsi sequenze lunghe di bit usate per ==**creare chiavi**== per i #one_time_pad
> 

---