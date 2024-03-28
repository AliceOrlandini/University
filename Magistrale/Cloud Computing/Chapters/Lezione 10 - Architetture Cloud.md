## Recap del Cloud

Basandoci su ciò che abbiamo osservato finora, possiamo definire il cloud come un'infrastruttura che consente la creazione di macchine virtuali su cui eseguire i propri servizi. 
Ogni macchina virtuale è dotata di una o più VCPU, RAM dedicata e storage locale su disco rigido, su cui è installato il sistema operativo. 
Le VM sono inoltre equipaggiate con un adattatore di rete virtuale, che facilita la comunicazione sia con altre VM tramite connessioni virtuali o fisiche, sia con host esterni attraverso Internet. 
Inoltre, è comune trovare uno storage condiviso accessibile a tutte le VM tramite rete locale virtuale o fisica.

## Applicazioni Tradizionali vs Cloudification

Nel modello tradizionale di sviluppo delle applicazioni, si adotta un approccio lineare e chiaro. 
Il cuore dell'applicazione risiede nel server, il quale non solo implementa la logica principale, ma fornisce anche REST API che consentono ai client di accedere ai servizi offerti. Qualora sia necessario, il server può interagire direttamente con il database.

Come anticipato, l'approccio tradizionale si rivela svantaggioso per svariati motivi, pertanto numerose società hanno abbracciato la cloudification, trasferendo le proprie applicazioni su un'infrastruttura cloud senza introdurre modifiche sostanziali. 
In pratica, l'implementazione rimane immutata, ma tutto viene eseguito in un ambiente virtuale.

Questo approccio, tuttavia, non sfrutta appieno le potenzialità del cloud in quanto le applicazioni vengono semplicemente trasferite su di esso senza essere progettate specificamente per sfruttarne le caratteristiche.
Il cloud, oltre a fornire risorse hardware a basso costo, offre vantaggi significativi come la scalabilità, l'affidabilità elevata e la capacità di gestire grandi quantità di dati. 
Per sfruttare appieno questi vantaggi, è necessario ripensare le applicazioni in ottica cloud, adottando un modello differente in base ai requisiti specifici e agli scenari d'uso previsti.

<div style="page-break-after: always;"></div>

## Multi-tier Architecture

Le applicazioni cloud sono comunemente implementate seguendo un'architettura **multi-tier**: le diverse funzionalità vengono suddivise tra varie macchine virtuali, ciascuna responsabile di una specifica parte del sistema. Non ci sono regole generali per effettuare la divisione delle funzionalità.

Tra le architetture più semplici vi è la **two-tier**, che divide le funzionalità di gestione dei dati da quelle di accoglienza delle richieste (il server). 
Il tier del server comunica con il tier del database per recuperare o memorizzare i dati necessari.

Se necessario, è possibile aggiungere ulteriori tier, ad esempio utilizzando un'architettura **three-tier** composta da: un tier dell'applicazione, un tier del database e un tier dello storage. 
Un'applicazione distribuita in questa configurazione consente di separare in modo più granulare le funzionalità e di scalare facilmente aggiungendo macchine virtuali su richiesta. Questo approccio offre vantaggi significativi in termini di scalabilità e gestione delle risorse nell'ambiente cloud.

Il risultato generale è un'architettura multi-tier in cui il servizio viene erogato ai clienti attraverso una **catena di interazioni** che coinvolge i diversi tier. Questi tier sono collegati tramite una rete LAN, la quale può essere implementata con l'ausilio di un **middleware** per semplificare lo sviluppo e la gestione del sistema.

## Compute Cluster

Per garantire l'affidabilità, è comune duplicare i task. 
Un insieme di macchine virtuali che appartengono allo stesso tier e che quindi implementano la stessa funzionalità dell'applicazione globale vengono definite **compute cluster**.

> [!note] Compute Cluster
> Un **Compute Cluster** è un set di macchine virtuali organizzate per svolgere una computezione collettiva su un singolo grande job o set di singole richieste. 

I nodi del cluster sono ottimizzati per gestire principalmente operazioni di comunicazione tra di loro, limitando le operazioni di I/O. Le macchine virtuali all'interno dello stesso cluster di solito utilizzano un middleware per gestire la comunicazione sia tra i nodi interni al cluster che con quelli esterni appartenenti a tier diversi.

L'architettura complessiva dei cluster varia in base ai requisiti specifici dell'applicazione, che a loro volta dipendono dai diversi casi d'uso.

I design di cluster possibili includono:
1. **High-Availability Clusters**: questi cluster sono progettati per garantire la massima disponibilità del servizio, riducendo al minimo i tempi di inattività. Utilizzano tecniche come la duplicazione dei servizi e il failover automatico per assicurare che il servizio rimanga accessibile anche in caso di guasti hardware o software.
2. **Load Balancing Clusters**: questi cluster distribuiscono il carico di lavoro tra i nodi in modo equo, consentendo di gestire un elevato numero di richieste senza sovraccaricare alcun nodo specifico. Questo design è particolarmente utile per applicazioni web ad alta traffico o servizi che richiedono una distribuzione uniforme delle risorse.
3. **Compute Intensive Clusters**: questi cluster sono ottimizzati per eseguire operazioni computazionalmente intensive, come analisi dati complesse o simulazioni scientifiche. Sfruttano risorse hardware potenti e tecniche di parallelizzazione per eseguire rapidamente operazioni complesse.

### High-Availability Clusters

Gli High-Availability Cluster (HA) sono progettati per garantire la tolleranza ai guasti e supportare l'esecuzione di servizi che richiedono un'elevata disponibilità continua. Utilizzano la ridondanza su diversi nodi per gestire i guasti e evitare punti singoli di fallimento.

Il cluster HA più semplice è costituito da un nodo ridondante che può sostituire il nodo principale in caso di guasto. In questo modo, la ridondanza dei nodi assicura un'alta disponibilità del servizio, consentendo al cluster di continuare a funzionare anche se uno dei nodi dovesse smettere di operare correttamente.
Per garantire un'elevata disponibilità, ogni tier dell'applicazione deve implementare diverse macchine virtuali che forniscono lo stesso servizio. In questo modo, se una macchina virtuale dovesse fallire, le altre possono continuare a erogare il servizio senza interruzioni.

L'architettura adottata è quella master-slave, dove in caso di guasto del server principale, viene eletto un nuovo server per assumere il ruolo di master, sia mediante una designazione preventiva che tramite una procedura di elezione.

È fondamentale prevedere un meccanismo di replicazione delle informazioni per mantenere aggiornati tutti gli slave con le ultime modifiche effettuate sul master.

Tuttavia, questa configurazione presenta svantaggi significativi. Ad esempio, gli slave possono rimanere inattivi per la maggior parte del tempo, comportando un utilizzo inefficiente delle risorse. 
Inoltre, questa architettura potrebbe non essere facilmente scalabile per gestire carichi di lavoro in continua crescita.

### Load Balancing Clusters

Per garantire un'ottimale utilizzazione delle risorse senza compromettere l'affidabilità, un'alternativa sono i Load Balancing Clusters (LB). Questi sono progettati per distribuire equamente il carico di lavoro tra i nodi, sfruttando appieno la capacità del cloud di scalare in modo orizzontale.

Tra i vantaggi di questo approccio vi sono non solo una maggiore efficienza nell'utilizzo delle risorse, ma anche prestazioni migliori, come tempi di risposta ridotti. Ciò è possibile grazie alla distribuzione uniforme delle richieste tra i nodi, evitando sovraccarichi su singoli server.

Per implementare i Load Balancing Clusters, si introduce un tier aggiuntivo chiamato **balancer tier** tra l'applicazione e i client. Questo tier ha il compito di ricevere le richieste dai client e indirizzarle a una delle macchine virtuali dell'application tier utilizzando una specifica policy di bilanciamento del carico.

Successivamente, si ha generalmente un tier del database in cui ogni istanza è associata a una specifica istanza dell'application tier. Ciò significa che ogni istanza del database è dedicata a servire le richieste provenienti da una particolare istanza dell'applicazione, consentendo una migliore gestione dei dati e delle risorse.

Un aspetto cruciale nell'implementazione dei Load Balancing Clusters è la **sincronizzazione dei dati,** poiché le macchine virtuali possono ricevere e gestire le richieste in parallelo e i dati devono essere sincronizzati tra le istanze dello stesso tier. Ci sono due approcci principali per affrontare questo problema:
1. **Replicazione dei dati**: questo approccio prevede di replicare i dati su più istanze all'interno dello stesso tier. Tuttavia, questa replicazione può comportare un overhead significativo, in quanto richiede la sincronizzazione costante dei dati tra le varie istanze.
2. **Utilizzo di un unico Storage tier**: in questo approccio, viene utilizzato un singolo tier di storage al quale accedono tutte le istanze del database tier. Questo assicura che tutti i dati siano memorizzati nello stesso punto, semplificando la sincronizzazione. Tuttavia, questo costituisce anche un unico punto di fallimento, poiché la perdita del tier di storage potrebbe causare la perdita di tutti i dati.

La scelta tra i due approcci dipende dalle specifiche dell'applicazione e dalla quantità di dati da memorizzare. È importante valutare attentamente i requisiti di prestazioni, affidabilità e scalabilità dell'applicazione prima di prendere una decisione.

### Compute Intensive Clusters

I Compute Intensive Clusters (CI) sono quelli di cui parla Puliafito a lezione e vengono utilizzati per gestire grandi volumi di dati attraverso l'approccio divide et impera, che comprende i seguenti passaggi:
1. Il grande task da eseguire viene suddiviso in piccoli sub-task, noti anche come Jobs.
2. I dati vengono suddivisi in piccoli chunk, ognuno dei quali viene assegnato a un job.
3. Un job e il relativo chunk di dati vengono assegnati a un singolo server (worker) per l'elaborazione.
4. Il worker elabora il task nei dati assegnati e restituisce il risultato al collector.
5. Il collector combina tutti i risultati provenienti dai worker.

Per garantire la scalabilità verso una grande quantità di richieste, anche nei Compute Intensive Clusters, si impiega un Load Balancer Tier. Successivamente, nell'application tier, i dati vengono suddivisi in chunks e viene creata una lista di jobs da eseguire.

Dato che la dimensione del dataset iniziale può essere troppo grande per essere memorizzata nel local storage di diverse macchine virtuali e sincronizzata efficacemente, si conserva il dataset iniziale su un cloud storage. 

I job vengono quindi distribuiti a un insieme di workers appartenenti a un'infrastruttura per l'elaborazione distribuita. Questo approccio consente di processare grandi volumi di dati in parallelo, sfruttando al massimo le risorse disponibili e garantendo una maggiore efficienza e scalabilità.

Per ridurre l'overhead causato dalla comunicazione, gli worker possono accedere direttamente allo storage tier per recuperare i dati necessari per eseguire i compiti assegnati. 

## Dynamic Adaptation

Uno dei vantaggi più significativi delle tecnologie cloud e della virtualizzazione è la scalabilità. Quando il carico di lavoro aumenta, è possibile scalare verticalmente aggiungendo risorse alle macchine virtuali esistenti, oppure scalare orizzontalmente aumentando il numero di macchine virtuali disponibili per gestire le richieste.

Le architetture Load Balancing (LB) e Compute Intensive (CI), sfruttando il tier di bilanciamento del carico, sono in grado di scalare orizzontalmente in risposta all'incremento o alla diminuzione del traffico. Ciò significa che possono aumentare dinamicamente il numero di macchine virtuali per soddisfare la domanda crescente o ridurre le risorse quando il carico diminuisce.

Nelle architetture High Availability (HA), la scalabilità orizzontale viene spesso utilizzata per migliorare la ridondanza e quindi l'affidabilità del sistema, anziché per gestire picchi di traffico. Aggiungendo più nodi, è possibile garantire una maggiore resilienza del sistema, consentendo il failover automatico in caso di guasto di un nodo.