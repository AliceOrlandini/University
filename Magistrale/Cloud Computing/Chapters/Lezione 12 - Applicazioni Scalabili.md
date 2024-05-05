## Architettura delle applicazioni Cloud

Le applicazioni cloud sono sviluppate come sistemi distribuiti, dove la loro implementazione è una composizione di diverse funzionalità ospitate su macchine virtuali diverse. Queste VM sono organizzate in cluster, denominati tiers, all'interno dei quali ogni VM può replicare le funzionalità per garantire disponibilità e scalabilità. 

Le richieste provenienti dai clienti sono inizialmente ricevute dal primo tier, noto come frontend tier. Questo tier funge da punto di ingresso per le richieste esterne e si occupa di instradarle agli altri tiers, chiamati backend che contengono la logica applicativa e i dati necessari per elaborare le richieste. 
Questa architettura a tier consente una gestione efficiente del carico di lavoro e una migliore scalabilità, poiché le risorse possono essere allocate e scalate in modo indipendente in base alle esigenze del sistema.

La metodologia di design utilizzata è la SOA (Service-Oriented Architecture), in cui ogni VM ospita servizi modulari e indipendenti che interagiscono tramite message passing, utilizzando protocolli come SOAP o REST.

## Request-Reply communication

Entrambe SOAP e REST adottano un protocollo di comunicazione Request-Reply, il che implica una comunicazione asincrona in cui il client rimane in attesa fino a ricevere una risposta dal server (*time-coupled*). Questo tipo di comunicazione è affidabile in quanto la risposta funge da conferma di ricezione della richiesta.
Inoltre, questa comunicazione è diretta, consentendo al client e al server di interagire senza intermediari (*space-coupling*). Tuttavia, questa caratteristica può presentare delle sfide in alcuni contesti, poiché può essere necessario un intermediario per garantire la scalabilità, la sicurezza o altre esigenze di gestione della comunicazione.

Nei sistemi backend, dove la scalabilità e l'affidabilità sono prioritari, l'approccio basato su time e space coupling può compromettere le prestazioni. Una comunicazione sincrona diretta tra client e backend può sovraccaricare il sistema e limitare la flessibilità nel ridimensionamento delle risorse. È preferibile adottare approcci asincroni e a basso accoppiamento per distribuire le richieste in modo efficace su più istanze del backend, garantendo una migliore gestione del carico e una maggiore affidabilità.

## Comunicazione indiretta

Il paradigma di comunicazione indiretta è fondamentale per garantire due aspetti chiave:
1. **Space uncoupling**: qui, mittente e destinatario non devono conoscere l'identità reciproca. Questo permette la sostituzione dinamica dei partecipanti alla comunicazione (come la creazione o la rimozione di una VM). Per ottenere ciò, si introduce un intermediario che non ha un accoppiamento diretto tra mittente e destinatario.
2. **Time uncoupling**: in questo contesto, mittente e destinatario hanno "vite" indipendenti. Ciò significa che possono esistere in tempi diversi durante la comunicazione, anche in un ambiente volatile. Questo è reso possibile dall'utilizzo di una comunicazione asincrona, in cui il mittente invia il messaggio all'intermediario e prosegue senza attendere, consentendo all'intermediario di consegnare il messaggio in modo tempestivo al destinatario. In questo modo, non è necessario che mittente e destinatario si trovino contemporaneamente nello stesso momento.

## Comunicazione orientata ai messaggi



## Coda dei messaggi

## Messaggi

## Scalabilità

## Replicazione dati

## Gestione delle richieste

## 
