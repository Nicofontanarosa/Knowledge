
#Zero_Knowledge #protocol

> [!brain]- Impressions
> 
> #mind
> 
> Questo protocollo è **molto sicuro** poiché **V non scopre nulla** di più da P, **non può mentire** e P può bleffare solamente se riesce a prevedere i bit generati casualmente da V => La **sicurezza** si basa sulla **generazione** del **bit** in maniera **casuale** ( così come nel #BB84 )
> 
> - Uno svantaggio dell' algoritmo ( #fiat_shamir) è quello che V sceglie un k molto basso => P potrebbe essere disonesto ma avere fortuna e passare le varie fasi. Qui parliamo però di #static_cryptanalysis 
> - L' algoritmo basa la sua segretezza sulla difficoltà di ricavare la **radice in modulo** di t per cui occorre tempo esponenziale
> - Errore che riguarda l' uso scorretto dell' algoritmo è il non generare ogni volta un nuovo r ( **crittoanalisi statica** ) poiché se e = 0, z = r mod n = r ( r < n ) e quindi il crittoanalista in ascolto prende r e, nell' iterazione successiva se r non viene generato e abbiamo che e = 1 => z = r\*s che se < n => posso trovare s
> 

> [!radar]- Zero Knowledge vs RSA
> 
> Le differenze tra questo protocollo e quelli a chiave pubblica e che non è soggetto ad attacchi di tipo #man-in-the-middle e ad attacchi di tipo #chosen-plain-text 
> Basano la loro sicurezza su problemi esponenziali da calcolare ovvero radice in modulo e fattorizzazione 
> 

---