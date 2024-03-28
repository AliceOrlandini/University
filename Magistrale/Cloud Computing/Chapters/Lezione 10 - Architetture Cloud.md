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

Gli High-Availability Cluster (HA) sono cluster progettati per essere fault-tolerant e supportare l'esecuzione di srevizi che richiedono di essere sempre disponibili. 
I cluster HA per operare utilizzano la ridondanza sui vari nodi per sostenere i fault o failures e avoid single point of failure.
Il cluster HA più semplice è un clouster con solo 2 nodi ridondanti da utilizzare in sostituzione del principale nel caso si guastasse. 
Quindi, grande ridondanza garantisce grande availability, e per fare ciò ogni tier deve implementare diverse VM che implementano lo stesso servizio. 
L'architettura è quindi una master slave e quando un master si guasta si elegge un nuovo server o a priori o tramite una procedura di elezioni.
Bisogna anche prevedere un meccanismo di replicazione delle informazioni per fare in modo che tutti gli slave siano aggiornati.

Ci sono degli svantaggi considerevoli in questo meccanismo come: il fatto che per la maggior parte del tempo gli slave stanno a fare nulla, e che non è un'architettura scalabile.

### Load Balancing Clusters

Per garantire un migliore utilizzo delle risorse che non trascuri l'availability, un alternativa sono i Load Balancing Clusters.
Sono progettati per distribuire il carico di lavoro in modo uniforme sui nodi che permette di sfruttare al massimo la proprietà del cloud di essere scalabile in modo orizzontale.

Un secondo va
