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

Il paradigma di comunicazione indiretta è richiesta per assicurare:
- space uncoupling: il sender non sa o non ha bisogno di sapere l'identità del ricevitore e viceversa, quindi questi p
- time uncoupling: 

## Comunicazione orientata ai messaggi

## Coda dei messaggi

## Messaggi

## Scalabilità

## Replicazione dati

## Gestione delle richieste

## 
