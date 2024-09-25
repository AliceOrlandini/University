## Architettura delle applicazioni Cloud

Le applicazioni cloud sono sviluppate come sistemi distribuiti, dove la loro implementazione è una composizione di diverse funzionalità ospitate su macchine virtuali diverse. Queste VM sono organizzate in cluster, denominati tiers, all'interno dei quali ogni VM può replicare le funzionalità per garantire disponibilità e scalabilità. 

Le richieste provenienti dai clienti sono inizialmente ricevute dal primo tier, noto come frontend tier. Questo tier funge da punto di ingresso per le richieste esterne e si occupa di instradarle agli altri tiers, chiamati backend che contengono la logica applicativa e i dati necessari per elaborare le richieste. 
Questa architettura a tier consente una gestione efficiente del carico di lavoro e una migliore scalabilità, poiché le risorse possono essere allocate e scalate in modo indipendente in base alle esigenze del sistema.

La metodologia di design utilizzata è la SOA (Service-Oriented Architecture), in cui ogni VM ospita servizi modulari e indipendenti che interagiscono tramite message passing, utilizzando protocolli come SOAP o REST.

## Request-Reply communication

Entrambe SOAP e REST adottano un protocollo di comunicazione Request-Reply, il che implica una comunicazione sincrona in cui il client rimane in attesa fino a ricevere una risposta dal server (*time-coupled*). Questo tipo di comunicazione è affidabile in quanto la risposta funge da conferma di ricezione della richiesta.
Inoltre, questa comunicazione è diretta, consentendo al client e al server di interagire senza intermediari (*space-coupling*). Tuttavia, questa caratteristica può presentare delle sfide in alcuni contesti, poiché può essere necessario un intermediario per garantire la scalabilità, la sicurezza o altre esigenze di gestione della comunicazione.

Nei sistemi backend, dove la scalabilità e l'affidabilità sono prioritari, l'approccio basato su time e space coupling può compromettere le prestazioni. Una comunicazione sincrona diretta tra client e backend può sovraccaricare il sistema e limitare la flessibilità nel ridimensionamento delle risorse. È preferibile adottare approcci asincroni e a basso accoppiamento per distribuire le richieste in modo efficace su più istanze del backend, garantendo una migliore gestione del carico e una maggiore affidabilità.

## Comunicazione indiretta

Il paradigma di comunicazione indiretta è fondamentale per garantire due aspetti chiave:
1. **Space uncoupling**: qui, mittente e destinatario non devono conoscere l'identità reciproca. Questo permette la sostituzione dinamica dei partecipanti alla comunicazione (come la creazione o la rimozione di una VM). Per ottenere ciò, si introduce un intermediario che non ha un accoppiamento diretto tra mittente e destinatario.
2. **Time uncoupling**: in questo contesto, mittente e destinatario hanno "vite" indipendenti. Ciò significa che possono esistere in tempi diversi durante la comunicazione, anche in un ambiente volatile. Questo è reso possibile dall'utilizzo di una comunicazione asincrona, in cui il mittente invia il messaggio all'intermediario e prosegue senza attendere, consentendo all'intermediario di consegnare il messaggio in modo tempestivo al destinatario. In questo modo, non è necessario che mittente e destinatario si trovino contemporaneamente nello stesso momento.

## Comunicazione orientata ai messaggi

Per le applicazioni cloud sono stati introdotti nuovi paradigmi di comunicazione di tipo *indiretto* e *message-oriented*. In questi modelli, la sorgente e la destinazione non stabiliscono un canale di comunicazione diretto (come avviene in TCP/UDP), ma il trasmettitore inietta il messaggio in un *bus condiviso*.

Questo bus condiviso è gestito da un intermediario: il trasmettitore invia il messaggio all'intermediario, il quale si occupa di recapitarlo al destinatario. Lo stesso processo avviene in caso di una risposta.
Nel tempo, sono emersi due principali tipi di intermediari: il **broker** e la **message queue**.

### Broker

La prima implementazione che possiamo considerare è il meccanismo di *publish-subscribe*. In questo modello, i trasmettitori, chiamati *publishers*, inviano strutture dati, solitamente eventi, mentre i ricevitori, detti *subscribers*, si iscrivono per ricevere i messaggi di loro interesse.

Al centro del sistema troviamo il *message broker*, che si occupa di inoltrare i messaggi dai *publishers* ai *subscribers*. Questo modello permette una comunicazione di tipo uno-a-molti (1 → many).

Esistono tre principali modalità di sottoscrizione:
- **Channel Based**: i subscribers si iscrivono a canali specifici, ricevendo tutti i messaggi che vi transitano, indipendentemente dal contenuto.
- **Topic Based**: i subscribers si iscrivono a uno o più *topic*, ricevendo solo i messaggi associati a quei *topic*, offrendo una maggiore granularità rispetto al modello basato sui canali.
- **Type Based**: i subscribers si iscrivono in base al *tipo di dato* o messaggio, ricevendo solo quelli che corrispondono a un certo tipo predefinito.
### Message Queue

Il secondo approccio implementativo è il *message queue*, che fornisce uno scambio di messaggi di tipo point-to-point tra produttore e consumatore. In questo modello, il trasmettitore inserisce un messaggio in una coda, che verrà poi rimosso e processato dal ricevitore.

Lo scambio avviene all'interno del *Message Queueing System*, dove possono esistere più code. Sia il trasmettitore che il ricevitore possono decidere in quale coda inserire o prelevare i messaggi, garantendo così flessibilità nel flusso di comunicazione.

Il set di operazioni offerte dal *Message Queueing System* è semplice e comprende:
- **Send**: utilizzato dal produttore per inserire messaggi nella coda.
- **Blocking Receive**: utilizzato dal consumatore per attendere in modo sincrono fino a quando un messaggio di suo interesse diventa disponibile nella coda.
- **Non-blocking Receive**: chiamato anche polling, permette al consumatore di verificare lo stato della coda in modo asincrono. Se un messaggio è disponibile, viene restituito, altrimenti non accade nulla.
- **Notify**: utilizzato dal system message queue per notificare al consumatore la disponibilità di un messaggio nella coda a cui è iscritto.

Ogni messaggio è composto da un identificatore della coda di destinazione e metadata come ad esempio la priorità o la modalità di delivery.
Il corpo del messaggio è solitamente ignorato dal sistema.
La politica delle code è generalmente FIFO ma alcune supportano il concetto di priorità.
Un consumatore può anche selezionare mess

## Messaggi

## Scalabilità

## Replicazione dati

## Gestione delle richieste

