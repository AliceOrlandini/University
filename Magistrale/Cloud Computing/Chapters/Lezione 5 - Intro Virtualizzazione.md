
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
- Para Virtualization
- Hardware-assisted

In aggiunta, è possibile implementare tecniche di virtualizzazione più leggere per specifiche esigenze. Le vedremo nel dettaglio in seguito.

L'implementazione dell'hypervisor per la virtualizzazione avviene principalmente attraverso due tecniche:
1. **Approccio Hosted**: Un sistema operativo è installato inizialmente sulla macchina fisica. Su questo sistema opera l'hypervisor (come software) per eseguire le macchine virtuali. Questa è la tecnica utilizzata ad esempio in VirtualBox.
2. **Approccio Bare Metal**: L'hypervisor è installato direttamente sulla macchina fisica. Questo tipo di hypervisor, chiamato di tipo 1, può accedere direttamente all'hardware dell'host.