## Modello NIST

Il National Institute of Standards and Technology (NIST) è un'agenzia governativa statunitense responsabile dello sviluppo di standard industriali e tecnologici. Per quanto riguarda il cloud computing, il NIST ha svolto un ruolo chiave nello sviluppo di definizioni e linee guida che hanno contribuito a stabilire un quadro comune per comprendere e implementare questa tecnologia.

Nel 2011, il NIST ha pubblicato il documento intitolato "The NIST Definition of Cloud Computing". Questo documento fornisce una definizione formale del cloud computing e identifica i principali modelli di servizio (come Infrastructure as a Service, Platform as a Service e Software as a Service), i modelli di distribuzione (come cloud pubblico, cloud privato, cloud ibrido e community cloud) e gli attributi principali che caratterizzano l'infrastruttura.

## Attributi del Cloud

I principali attributi del cloud computing, secondo il NIST, sono:
1. **Self-service on-demand:** gli utenti possono automaticamente approvvigionarsi di risorse senza l'intervento umano da parte del fornitore di servizi.
2. **Broad network access:** i servizi sono disponibili su reti e dispositivi eterogenei (ad esempio, computer, telefoni, tablet).
3. **Resource pooling:** le risorse di elaborazione sono consolidate per servire più utenti, con diverse risorse fisiche e virtuali assegnate e riassegnate dinamicamente in base alla domanda. In pratica l'utilizzatore ha l'impressione che le risorse siano infinite e a sua totale disposizione. 
4. **Rapid elasticity:** le risorse possono essere scalate in modo rapido e automatico per soddisfare le esigenze di crescita o calo della domanda.
5. **Measured service:** le risorse cloud sono monitorate e misurate, consentendo un controllo trasparente e l'addebito preciso in base all'utilizzo.

Gli attori coinvolti quando si tratta il modello sono: Cloud Consumer, Cloud Provider, Cloud Auditor, Cloud Carrier e Cloud Broker. 
Li abbiamo già visti tutti nella lezione 2 tranne il Cloud Broker che è un un intermediario che facilita l'utilizzo dei servizi cloud. Questo ruolo è nato a causa della crescente complessità del panorama del cloud computing, con una vasta gamma di servizi offerti da diversi fornitori.

## Modelli di Deployment

Le tipologie di deployment cloud individuate dal NIST sono 4:
1. **Cloud Pubblico:** è il deployment model più pololare. In questa tipologia l'infrastruttura è gestita da un organizzazione esterna che vende servizi e risorse ad un pubblico generale. È praticamente la configurazione di cui abbiamo parlato finora, in cui è implementata la multi-tenancy tramite virtualizzazione e gli utenti condividono le risorse tra loro. I vantaggi sono scalabilità rapida, flessibilità, pagamento basato sull'utilizzo.
2. **Cloud Privato**: è un deployment model chiuso al pubblico ma utilizzato internamente dalla compagnia, non è infatti creato per poter vendere le risorse ad aziende terze ma per utilizzarlo per ospitare i propri servizi. Un esempio può essere Facebook, che ha un'infrastruttura accessibile solo agli utenti autorizzati all'interno dell'organizzazione. Il vantaggio principale riguarda il maggiore controllo sulla sicurezza e sulla privacy, adatto a organizzazioni con requisiti specifici di conformità e sicurezza.
3. **Community Cloud**: è un deployment model che permette l'utilizzo da parte di più organizzazioni che fanno parte della stessa community poiché hanno interessi comuni come requisiti di sicurezza o conformità. Quindi ogni organizzazione mette un capitale per la sua realizzazione e poi si potrà utilizzare l'approccio pay-per-use. 
4. **Cloud Ibrido**: è un deployment model che combina il cloud privato con quello pubblico per avere i vantaggi di entrambi. Ad esempio un cloud ibrido può essere composto da una parte privata che contiene dati sensibili e che verrà utilizzata internamente e una parte pubblica che verrà gestita dal cloud provider. Essa è quindi una combinazione di risorse cloud pubbliche e private collegate in modo che i dati e le applicazioni possano essere spostati tra di esse.

Per la maggior parte delle applicazioni il cloud pubblico è la soluzione ottimale. 

## Modelli di servizio

I modelli di servizio nel contesto del cloud computing definiscono la tipologia di servizi offerti attraverso la piattaforma cloud. 
Ogni modello offre un livello diverso di gestione delle risorse e delle responsabilità da parte dell'utente. 
Ecco una panoramica di ciascun modello:
1. **Infrastructure as a Service (IaaS)**: è il modello più popolare e consiste nel fornire agli utenti macchine virtuali sul quale installare il proprio servizio. In questo modello gli utenti hanno il controllo completo sull'infrastruttura virtuale, ma sono responsabili della gestione del sistema operativo, delle applicazioni e dei dati. Non si dovrà occupare dell'hardware e della scalabilità dinamica.
2. **Platform as a Service (PaaS)**: offre un ambiente di sviluppo e di esecuzione completo, inclusi strumenti e servizi per sviluppare, testare e distribuire applicazioni senza la gestione diretta dell'infrastruttura sottostante. Non ci sono quindi macchine virtuali sulle quali installare il sistema operativo ma una vera e propria piattaforma tramite il quale lo sviluppatore può programmare e immagazzinare i dati. Lo svantaggio è la ridotta flessibilità rispetto all'approccio precedente. 
3. **Software as a Service (SaaS)**: offre applicazioni basate su cloud attraverso Internet. Gli utenti possono accedere alle applicazioni direttamente, senza preoccuparsi di installare, gestire o mantenere il software. L'utente non deve quindi preoccuparsi di niente, deve solo usare il servizio. 

I servizi PaaS e SaaS possono essere integrati su un IaaS. È possibile sfruttare l'infrastruttura fornita dalle macchine virtuali per ospitare le API, utilizzandole successivamente per creare un servizio, seguendo un modello simile a una struttura piramidale. 
In altre parole, l'IaaS costituisce la base fondamentale, mentre i livelli successivi di PaaS e SaaS vengono costruiti sopra, creando così una gerarchia di servizi cloud interconnessi.

Altri servizi possibili sono:
- **Storage as a Service**: si utilizza l'infrastruttura per memorizzare i dati sfruttando la resistenza ai guasti del cloud. 
- **Database as a Service**.
- **Backup as a Service**.
- **Desktop as a Service**: offrono ambienti desktop personalizzati per gli utenti di un servizio, è di fatto un desktop virtuale. 