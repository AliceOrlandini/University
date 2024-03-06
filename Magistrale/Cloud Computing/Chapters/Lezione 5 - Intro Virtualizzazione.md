
> [!note] Virtualizzazione
> Per **virtualizzazione** si intende la rappresentazione di una risorsa fisica in forma *simulata*, tramite un layer software. 

Il software installato sulla macchina fisica (host) viene anche chiamato “virtualization layer” ed è composto da varie tecniche di virtualizzazione. Il meccanismo della virtualizzazione permette di separare le risorse computazionali fisiche dall’accesso diretto degli utenti, per una migliore gestione delle risorse. 

Ogni risorsa IT è suscettibile di virtualizzazione e solitamente si classificano in due categorie:
- Essenziali: quali CPU, RAM e hard disk, indispensabili per il funzionamento di un server.
- Facoltative: come le periferiche, non strettamente necessarie per il funzionamento del server.
Relativamente alle risorse essenziali, è cruciale sottolineare che la virtualizzazione è possibile solo se esiste almeno una corrispondenza fisica che supporti la rappresentazione virtuale.

Ad esempio, un processore virtuale richiede un processore fisico corrispondente per essere virtualizzato mentre, per le periferiche, non è essenziale avere una corrispondenza fisica in hardware. 

In linea generale, il livello di virtualizzazione converte le risorse fisiche in una versione virtuale, che può differire dalla controparte reale. Questa flessibilità consente, ad esempio, la creazione di un processore virtuale a 32 bit da un processore fisico a 64 bit.

## Server Virtualization

La virtualizzazione del server è il concetto di creare di una macchina virtuale su una macchina fisica: la macchina fisica è denominata host system, mentre la macchina virtuale è la guest o virtual system. 

In contrasto con l'approccio tradizionale senza virtualizzazione, dove la corrispondenza tra un computer fisico e un sistema operativo era 1:1, la virtualizzazione consente ora una relazione 1:N, permettendo l’esecuzione parallela di più sistemi operativi sulla stessa macchina fisica.

Ogni guest system è autonomo dagli altri e, attraverso il virtualization layer, può accedere alle risorse fisiche dell'host system. Ciò consente l'esecuzione di multiple applicazioni, ognuna all'interno del proprio sistema operativo senza che interferiscano l’un l’altra.

## Hypervisor

L'hypervisor o Virtual Machine Monitor rappresenta il cuore della virtualizzazione, essendo il software responsabile della creazione e gestione delle risorse virtuali. 
È l'unico che può accedere alle risorse fisiche, controllando e monitorando le macchine virtuali presenti. Il suo ruolo fondamentalmente è fornire risorse virtuali ai guest systems ed eseguire in modo effettivo le istruzioni sulle risorse fisiche.

Le tecniche di virtualizzazione basate sull'hypervisor si suddividono generalmente in tre categorie:
- Full Virtualization
- Para-Virtualization
- Hardware-assisted Virtualization

In aggiunta, è possibile implementare tecniche di virtualizzazione più leggere per specifiche esigenze. Le vedremo nel dettaglio in seguito.

L'implementazione dell'hypervisor per la virtualizzazione avviene principalmente attraverso due tecniche:
1. **Approccio Hosted**: un sistema operativo è installato inizialmente sulla macchina fisica. Su questo sistema opera l'hypervisor (come software, chiamato di tipo 2) per eseguire le macchine virtuali. Questa è la tecnica utilizzata ad esempio in VirtualBox.
2. **Approccio Bare Metal**: l'hypervisor è installato direttamente sulla macchina fisica. Questo tipo di hypervisor, chiamato di tipo 1, può accedere direttamente all'hardware dell'host.

Quindi, nel secondo scenario, l'hypervisor assume un ruolo simile ad un sistema operativo a tutti gli effetti, poiché ha accesso diretto all'hardware. Nel primo caso invece, l'hypervisor funge da software eseguito su un sistema operativo installato, come ad esempio Linux.

Vediamo i pro e i contro dei due approcci:

|        | Hosted Approach                                                                                                                                                                                                                                 | Bare Metal Approach                                                                                                                                                                                     |
| ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Pro    | 1. Il sistema operativo host provvede alla gestione dei driver per le risorse fisiche sottostanti, ciò semplifica l’implementazione dell’hypervisor. 2. Grande compatibilità dell’hypervisor con tutte le configurazioni di hardware possibili. | 1. L’hypervisor ha diretto accesso alle risorse, questo garantisce performance migliori.                                                                                                                |
| Contro | 1. L’hypervisor non ha accesso diretto all’hardware ma deve passare dall’OS host, questo porta ad una degradazione delle performance.                                                                                                           | 1. Generalmente l’hypervisor ha un set limitato di driver che porta ad un limitato supporto hardware, infatti sono implementati i driver per l’hardware specifico utilizzato nel server del datacenter. |

## Full Virtualization

Nella Full Virtualization (o virtualizzazione nativa), l'hypervisor implementa tutte le tecniche necessarie per virtualizzare ogni risorsa fisica. Di conseguenza, il sistema operativo guest ha l'impressione di essere eseguito su risorse fisiche reali e rimane inconsapevole che le risorse utilizzate sono, in realtà, virtuali. 
Questa modalità consente l'utilizzo di sistemi operativi guest non modificati, poiché per loro non fa differenza se operano su hardware virtuale o fisico.

Questo è reso possibile dall'hypervisor, il quale gestisce tutte le richieste da parte del sistema operativo guest all'hardware durante la loro esecuzione. 
Un ulteriore vantaggio è che il sistema operativo guest rimane completamente isolato dalle risorse fisiche, garantendo un elevato livello di sicurezza. Tuttavia, l'inconveniente è rappresentato da un aumento del carico di lavoro, che si traduce in un maggiore delay.
## Para-Virtualization

In questo tipo di virtualizzazione, si spezza la barriera che separava i sistemi operativi guest dalle risorse fisiche, con l'hypervisor come intermediario. 

Nella para-virtualizzazione, parte delle operazioni passa dalla responsabilità dell'hypervisor a quella dei sistemi operativi guest. Pertanto, non è possibile utilizzare versioni standard di sistemi operativi come nella modalità di virtualizzazione completa. 
È necessario adoperare sistemi operativi con un kernel modificato per eseguire queste operazioni (un processo noto come porting). 
In questo contesto, il guest OS è consapevole di operare su un sistema virtualizzato e deve sapere quale hypervisor sarà utilizzato nel livello sottostante.

I vantaggi di questo approccio sono:
- Maggiore efficienza, con la possibilità per i guest OS di comunicare direttamente con le risorse fisiche, riducendo l'overhead dell'hypervisor.
- Maggiore compatibilità con le periferiche, in quanto il sistema non è vincolato ai driver forniti dall'hypervisor.

Gli svantaggi invece:
- Modifica necessaria dei sistemi operativi guest, operazione non sempre consentita per molti sistemi operativi come Windows.
- Minore sicurezza, poiché si concede a una singola macchina virtuale guest l'accesso diretto alle risorse.
## Hardware-assisted Virtualization

Dal 2006, i principali produttori di hardware come Intel e AMD hanno introdotto dispositivi appositamente progettati per supportare la virtualizzazione. 
Nuove primitive sono state integrate, invocabili direttamente dai guest OS e gestite dalla CPU per migliorare significativamente le prestazioni. 
Queste chiamate non richiedono traduzione o emulazione da parte dell'hypervisor, eliminando così la necessità di para-virtualizzazione. 
Tuttavia, questo approccio è vincolato a configurazioni specifiche di componenti hardware.
## Operating System Level Virtualization


## Emulazione

## Vantaggi e Svantaggi della Virtualizzazione

