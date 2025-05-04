# RADIUS, PNAC & EAP 📡

#RADIUS #PNAC #EAP #_802_ #_802_1X 

> [!IMPORTANT]
> 
> ***Definition of RADIUS***
>
>  **RADIUS ( Remote Authentication Dial-In User Service )** → Il server che gestisce l'autenticazione. E' un protocollo di autenticazione centralizzato ed è il + utilizzato per i device di rete
>  
> 🔹 **Obiettivo** -> Gestire l’accesso sicuro alle reti ( *Ethernet, Wi-Fi, VPN, ecc* )
> 
> 🔹 **Funziona su UDP** ( *porta **1812** per autenticazione, **1813** per accounting* ) 
> 
> 1️⃣ **Client RADIUS** → Lo switch, router o access point che vuole autenticare un utente
> 
> 2️⃣ **Server RADIUS** → Il server che verifica le credenziali
> 
> 3️⃣ **Database utenti** → Può essere un **Active Directory, LDAP, MySQL, ecc**
> 
> <mark>**Come Funziona un'Autenticazione con RADIUS?**</mark>
> 
> 1️⃣ Un utente tenta di connettersi alla rete
> 
> 2️⃣ Lo switch/AP chiede l'autenticazione e inoltra la richiesta al **Server RADIUS**
> 
> 3️⃣ Il server RADIUS verifica le credenziali (password o certificato)
> 
> 4️⃣ Se l’utente è autorizzato → Il server RADIUS risponde con **Access-Accept**
> 
> 5️⃣ Se l’utente non è autorizzato → Il server RADIUS invia **Access-Reject**
> 
> <p align="center"><img src="img/Screenshot 2025-04-15 183139.png" /></p>
> <p align="center"><img src="img/Screenshot 2025-04-15 183701.png" /></p>
> 
> Oltre alla sua implementazione tramite credenziali è utilizzato anche tramite cavo ( *used for billing activities on wired lines* ) cioè gli operatori di rete sanno che quel cavo arriva a casa di Pippo e quindi tassano il traffico a pippo
> 
> La versione per mobile è il protocollo DIAMETER
> 

> [!IMPORTANT]
> 
> ***Definition of EAP***
>
>  **EAP ( Extensible Authentication Protocol )** → Il protocollo utilizzato per scambiare le credenziali tra client e server
 > 
> 🔹 **Obiettivo** -> Scambiare credenziali tra il dispositivo e il server RADIUS
> 
> <mark>**Flusso di una richiesta EAP in una rete 802.1X**</mark>
> 
> 1️⃣ Il client si connette alla rete, lo switch/AP invia una richiesta **EAP-Request/Identity**
> 
> 2️⃣ Il client risponde con il proprio nome utente **EAP-Response/Identity**
> 
> 3️⃣ Lo switch/AP inoltra il tutto al **server RADIUS**
> 
> 4️⃣ Il server chiede ulteriori informazioni (password, certificati, token, ecc.)
>   
> 5️⃣ Se l'autenticazione va a buon fine, il client riceve l’**EAP-Success** e può navigare
> 
> <mark>**Tipologie di EAP più usate**</mark>
>   
> - **EAP-TLS** → Usa certificati digitali ( *molto sicuro* )
> - **EAP-PEAP** → Incapsula le credenziali in una connessione sicura TLS
> - **EAP-MSCHAPv2** → Usato spesso con Windows ( *meno sicuro* )
> - **EAP-TTLS** → Simile a PEAP, ma più flessibile

> [!IMPORTANT]
> 
> ***Definition of PNAC***
>
>  **PNAC ( Port-Based Network Access Control ) – Controllo di Accesso Basato sulle Porte** è un meccanismo che controlla l'accesso alla rete **a livello di porta Ethernet o Wi-Fi**, assicurandosi che solo dispositivi autorizzati possano connettersi
>   
> - Quando colleghi un dispositivo a una porta di rete o a un Wi-Fi aziendale, la rete **non ti permette subito di comunicare**.  
> - **802.1X + RADIUS** verificano le tue credenziali prima di concederti l'accesso.  
> 
> **Come Funziona PNAC?**
> PNAC si basa su **802.1X** e usa **EAP e RADIUS** per autenticare i dispositivi connessi a una rete cablata o wireless.  

---