
## Full Virtualization

La **full virtualization** è un approccio tecnicamente obsoleto, ma lo esaminiamo attentamente per acquisire una comprensione più approfondita delle tecniche attuali. 
Precedentemente, abbiamo esaminato la multi-programmazione poiché le logiche coinvolte sono in gran parte simili a quelle impiegate nella virtualizzazione.

Similmente alla multi-programmazione, dove ogni processo percepisce di avere il completo controllo delle risorse fisiche grazie all'intermediazione del sistema operativo, **l'hypervisor** (noto anche come **Virtual Machine Monitor**) conferisce a ciascuna macchina virtuale l'illusione di avere il totale controllo delle risorse. 
Pertanto, il ruolo dell'hypervisor può essere paragonato a quello del sistema operativo, ma le sue funzionalità sono estese in modo da consentire l'esecuzione simultanea di diverse macchine virtuali, ognuna dotata del proprio sistema operativo.

## Hypervisor

> [!note] Hypervisor
> Un **hypervisor**, o Virtual Machine Monitor (VMM), è un software che *consente l'esecuzione simultanea di più sistemi operativi su una singola macchina fisica*, gestendo l'allocazione delle risorse e l'isolamento tra le macchine virtuali.

Se le architetture del sistema target e dell'host coincidono, è possibile implementare l'hypervisor riducendo al minimo l'utilizzo dell'emulazione. Come noto, l'emulazione aumenta l'overhead e di conseguenza il ritardo dovuto alla necessità di tradurre il codice in binario.

Nella maggior parte dei casi, il codice del sistema operativo o del software in esecuzione in una macchina virtuale può essere eseguito *direttamente sull'hardware* senza richiedere l'intervento dell'hypervisor.

## Virtualizzazione della CPU

Il primo passo coinvolge la creazione di una *rappresentazione virtuale della CPU* mediante tecniche simili a quelle impiegate nella multi-programmazione. 
In questo contesto, si utilizzano CPU virtuali realizzate attraverso strutture dati e scambiate mediante il meccanismo di cambio di contesto. Fondamentalmente, trattiamo il sistema operativo ospite (guest OS) come se fosse un'applicazione ordinaria. 
La differenza chiave risiede nel fatto che *il guest OS viene eseguito interamente in modalità utente*. Pertanto, sorge la domanda: come riesce a eseguire istruzioni privilegiate? 
In pratica, l'hypervisor funge da *mediatore* tra le richieste privilegiate del guest OS e l'hardware sottostante.

La procedura si articola nel seguente modo:
1. L'hypervisor carica lo stato della VCPU nel processore host, concedendo alla macchina virtuale il controllo della CPU per eseguire il proprio codice finché non si trova di fronte a un'istruzione con privilegi superiori che quindi non può eseguire.
2. Si verifica un cambio di contesto e l'hypervisor *emula l'istruzione target*, non eseguibile direttamente dal guest OS, in software. Una volta completata l'emulazione, ricarica la VCPU nell'host CPU per consentire alla macchina virtuale di riprendere la sua esecuzione normale.
3. Quando più macchine virtuali sono attive, si adotta un approccio *Round Robin* con un timer. Ogni volta che scade, si effettua un cambio di contesto tra le VM per garantire fairness nell'esecuzione.

Nell'approccio multi-programmato, quando un processo in esecuzione in modalità utente tentava di eseguire un'istruzione privilegiata, veniva generata un'eccezione che terminava immediatamente il processo. 
Nella virtualizzazione, l'obiettivo è evitare questo comportamento, poiché causerebbe la chiusura dell'intera macchina virtuale. Di conseguenza, si cerca un modo per generare un'eccezione che consenta all'hypervisor di gestire la situazione attraverso l'emulazione.

Questo metodo è denominato **Trap and Emulate**. Alcuni esempi di istruzioni gestite tramite eccezioni trap includono quelle che richiedono l'accesso allo spazio di I/O, le istruzioni relative alle interruzioni software e quelle coinvolte nella manipolazione della Memory Management Unit (MMU).

Al fine di ridurre l'overhead complessivo, si cerca di minimizzare le operazioni che richiedono l'intervento dell'hypervisor. L'overhead, infatti, è direttamente proporzionale al numero di system call effettuate.

Per esempio, l'esecuzione di un'interruzione software con il comando *int* comporta le seguenti operazioni:
1. L'istruzione provoca un cambio di contesto verso l'hypervisor, il quale viene effettuato a livello hardware.
2. L'hypervisor avvia l'esecuzione dell'handler e successivamente restituisce il controllo alla macchina virtuale.
3. L'handler è ora in esecuzione in modalità utente, ma per natura contiene molte istruzioni privilegiate che, a loro volta, causano un ulteriore cambio di contesto per emulare l'istruzione e successivamente ritornare all'handler.

In definitiva, l'Interrupt Descriptor Table (IDT) del guest OS viene utilizzata per richiamare vari handler, generando di conseguenza numerosi cambi di contesto.

## Virtualizzazione della memoria

Il secondo aspetto da considerare riguarda la gestione della memoria. 
In questo contesto, la memoria fisica è suddivisa, ma anziché assegnare i segmenti ai singoli processi, vengono assegnati alle macchine virtuali. 
In questo modo, la macchina virtuale percepisce di avere il controllo totale, anche se in realtà non è così. È essenziale anche implementare un meccanismo che impedisca gli accessi non autorizzati.

Per mappare la memoria virtuale in quella fisica, si fa nuovamente affidamento sulla Memory Management Unit (MMU) dell'host CPU. 
Questa viene configurata con i range di indirizzi in memoria fisica assegnati a ciascuna macchina virtuale, e prima di eseguire le istruzioni della VM, i suoi indirizzi vengono tradotti.

Se si verifica un page fault, l'hypervisor assume il controllo e si occupa di recuperare la pagina dalla memoria di massa.

Alcune operazioni del guest OS potrebbero generare eccezioni trap, come ad esempio quelle operazioni che modificano la Page Table. In questi casi, l'hypervisor emula queste operazioni per garantire un corretto funzionamento del sistema.

Per implementare la Virtual Memory Management Unit (Virtual MMU), vengono definite due funzioni eseguite in **cascata**:
- $G : \text{guest} \rightarrow \text{guest}$, che mappa gli indirizzi *virtuali del guest OS* negli indirizzi *fisici guest* (che, tecnicamente, non corrispondono ancora agli indirizzi reali, ma il guest OS ne è ignaro).
- $H : \text{guest} \rightarrow \text{host}$, per mappare l'indirizzo *fisico guest* nell'indirizzo *fisico host*, che rappresenta l'indirizzo reale.

Un metodo per implementare le due funzioni è chiamato **Brute Force** e prevede la modifica della Page Table per aggiungere un livello addizionale (denominato **shadow table**). 
Questo strato implementa la funzione $H$, ovvero la traduzione dall'indirizzo *fisico guest* a quello *fisico host*. Questo approccio assicura anche la sicurezza, poiché ogni accesso non autorizzato da parte del guest OS a una porzione di memoria solleverà un'eccezione.

Tuttavia, il problema di questo metodo è rappresentato da un significativo overhead, poiché richiede numerosi cambi di contesto per una continua traduzione di indirizzi da parte dell'hypervisor.

## Virtualizzazione dei device I/O

Le **istruzioni di I/O** sono tipicamente istruzioni privilegiate, non consentite nello spazio utente. 
In un sistema multi-programmato, i processi possono accedere allo spazio di I/O attraverso le system calls. Tuttavia, in un ambiente virtuale, è necessario fornire *all'apparenza* alla macchina virtuale ospite il controllo completo dell'hardware e la capacità di accedervi direttamente. 
Pertanto, l'hypervisor deve *emulare* l'hardware reale creando la sua rappresentazione mediante un insieme di strutture dati caricate in memoria.