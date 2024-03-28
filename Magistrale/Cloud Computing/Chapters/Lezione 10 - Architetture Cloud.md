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

I nodi del cluster non devono occuparsi di molte operazioni di I/O, eccetto la comunicazione tra i vari nodi.
Le VM dello stesso cluster generalmente sfruttano un middleware che gestisce la comunicazione sia tra macchine dello stesso cluster sia con quelle all'esterno appartenenti a tier diversi.



