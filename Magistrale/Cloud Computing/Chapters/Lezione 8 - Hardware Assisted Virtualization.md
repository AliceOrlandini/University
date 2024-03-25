
## Performance Penalty

Come discusso in precedenza, l'esecuzione del sistema operativo ospite esclusivamente in modalità utente comportava un notevole overhead, specialmente quando il sistema ospite doveva eseguire operazioni privilegiate, richiedendo che l'hypervisor emulasse tutte le istruzioni coinvolte.

Per mitigare questo problema e migliorare le prestazioni complessive, i principali produttori di processori come Intel e AMD dal 2006 hanno intrapreso l'iniziativa di estendere il set di istruzioni per supportare direttamente la virtualizzazione. 
Questa estensione non rappresenta una modalità completamente nuova di virtualizzazione, ma piuttosto un supporto aggiuntivo che viene fornito anche quando si utilizza la virtualizzazione completa.

Ci concentreremo sulle estensioni fornite da Intel, note come VMX (Virtual Machine eXtension), progettate appositamente per ridurre al minimo l'overhead dell'hypervisor.

## Root e Non-Root Modes

Finora abbiamo sempre considerato due modalità operative: user e system. 
Tuttavia, con l'introduzione della tecnologia VMX, si aggiungono due nuove modalità operative: root e non-root, il che porta alla creazione di quattro possibili combinazioni (in ordine di privilegio decrescente):
- root/system
- root/user
- non-root/system
- non-root/user

Il mode root viene utilizzato dall'hypervisor, mentre il mode non-root viene utilizzato dal sistema operativo ospite.
Inoltre, un vantaggio significativo è che queste estensioni consentono la retrocompatibilità. Ciò significa che un sistema che non sfrutta la virtualizzazione può semplicemente ignorare i mode non-root.

Il principale obiettivo di questi nuovi modes è introdurre controlli a livello hardware per concedere al guest OS un maggiore controllo, mentre allo stesso tempo limita le azioni che può eseguire. Se il guest OS tenta di eseguire un'istruzione non autorizzata, verrà generata un'eccezione trap, che verrà gestita dall'hypervisor attraverso l'emulazione.

In particolare, Intel ha introdotto due nuove istruzioni assembly eseguibili nel root/system mode:
- VMLAUNCH: consente di avviare una nuova macchina virtuale
- VMRESUME: consente di riprendere l'esecuzione di una macchina virtuale precedentemente sospesa

Le due operazioni che implementano il cambio di contesto diventano:
- VM Entry: è il passaggio dall'esecuzione dell'hypervisor al sistema operativo ospite (VM). Durante questo processo, l'hardware carica lo stato della VM e avvia l'esecuzione della VM.
- VM Exit: è il duale della precedente cioè il passaggio dall'esecuzione della VM all'hypervisor. Si verifica quando l'hypervisor deve intervenire per gestire eventi o istruzioni che richiedono l'accesso all'hardware fisico o altre operazioni specifiche. Durante il "VM exit", l'hardware salva lo stato della VM e passa il controllo all'hypervisor per la gestione dell'evento.

## Gestione delle interruzioni

Con l'introduzione dei nuovi modes, i sistemi operativi guest acquisiscono la capacità di gestire direttamente alcune interruzioni, come quelle software (non quelle di I/O perchè in alcuni casi è richiesta l'emulazione).

Affinché questa gestione delle interruzioni sia efficiente, è necessario introdurre una nuova struttura dati chiamata Interrup Remapping Table (IRT). Questa tabella consente il reindirizzamento dinamico di ciascuna interruzione durante l'esecuzione del sistema.
La struttura dati IRT contiene un'entrata per ogni possibile richiesta di interruzione. Ciascuna entrata mappa la richiesta alla corrispondente riga dell'IDT (Interrupt Descriptor Table) della macchina virtuale Guest, o verso altre strutture pertinenti che esamineremo in seguito. 
Pertanto, quando viene sollevata un'interruzione, la IRT determina quale IDT deve effettivamente gestire l'interruzione e redirige la richiesta verso di essa.

### Esempio - Interruzione Software tramite int

Le interruzioni software tramite l'istruzione INT e la conseguente IRET richiederebbero diversi cambi di contesto tra il sistema operativo guest e l'hypervisor in assenza di supporto hardware. 
Tuttavia, con il supporto hardware appropriato, possiamo eseguire l'istruzione senza la necessità di intervento da parte dell'hypervisor, migliorando l'efficienza complessiva del processo.

Sfruttando i modes root/non-root, è possibile eseguire queste istruzioni senza coinvolgere direttamente l'hypervisor. In particolare, il cambio di contesto avviene da non-root/user a non-root/system per l'istruzione INT e viceversa per l'istruzione IRET, senza la necessità di emulare l'istruzione stessa. Questo approccio permette di ottimizzare l'efficienza del sistema, riducendo al minimo l'overhead e migliorando le prestazioni complessive.

Quindi, per riassumere, i modes non-root vengono utilizzati per mantenere la distinzione tra le applicazioni in user mode e kernel mode all'interno della macchina virtuale. 
Invece, i modes root/system e root/user consentono di implementare entrambi i [[Lezione 5 - Intro Virtualizzazione#Hypervisor|tipi di hypervisor]], vale a dire l'approccio hosted e bare metal.

## Virtual Machine Control Structure

L'Intel VMX ha introdotto una nuova struttura dati chiamata Virtual Machine Control Structure (VMCS), che racchiude tutte le informazioni relative allo stato di ogni macchina virtuale in esecuzione. 
Questa estensione comprende anche un set di istruzioni per manipolare tale struttura dati, che vengono utilizzate durante il cambio di contesto dalla macchina virtuale all'hypervisor oppure durante il ripristino dello stato della VM.

I principali campi della VMCS sono:
1. **Guest State**: contiene lo stato dei registri del processore virtuale associato al sistema operativo guest. Durante la fase di lancio della VM, questo stato viene caricato nella CPU effettiva. Al contrario, durante la fase di uscita della VM, il contenuto viene memorizzato per essere utilizzato in seguito.
2. **Host State**: Questo campo contiene lo stato memorizzato nel processore fisico prima della fase di lancio della VM. Pertanto, può includere la VCPU dell'hypervisor o lo stato di una macchina virtuale che era in esecuzione. Quando viene invocata la VM exit, questo stato può essere ricaricato per consentire alla VM o all'hypervisor di riprendere la normale esecuzione.
3. **VM Execution Control**: specifica le operazioni e le istruzioni che sono consentite con i privilegi di tipo non-root. Qualsiasi istruzione non consentita provoca una VM exit, ossia un'interruzione che richiede l'intervento dell'hypervisor per gestire l'evento.
4. **VM Enter Control**: Questo campo contiene dei flag che determinano i comportamenti da assumere durante l'ingresso nella VM, ovvero durante il cambio di contesto da root a non-root. Essi definiscono le condizioni e le operazioni da eseguire per garantire un corretto passaggio dal controllo dell'hypervisor al sistema operativo ospite.
5. **VM Exit Control**: è il duale del precedente, contiene dei flag che permettono di specificare le operazioni da eseguire durante l'uscita dalla VM, ovvero la transizione da non-root a root.
6. **VM Exit Reason**: contiene la ragione per cui la VM è uscita dal suo stato in esecuzione, fornendo informazioni utili all'hypervisor per gestire correttamente il cambio di contesto. Tale informazione permette all'hypervisor di comprendere immediatamente il motivo dell'uscita senza dover esaminare lo stato specifico della singola macchina virtuale.

Per quanto riguarda il guest e l'host states, essi contengono l'Instruction Pointer che permette di eseguire la prima istruzione del programma non appena la macchina virtuale o l'hypervisor torneranno in esecuzione. Questo punta all'indirizzo di memoria della prossima istruzione da eseguire dopo il cambio di contesto.

La sezione VM execution control contiene numerosi flag, che includono gestione delle interruzioni e dell'I/O, utilizzati principalmente per il **peripheral passthrough** (lo vedremo meglio in seguito).
Uno di questi flag determina se la CPU deve gestire direttamente un'interruzione esterna o se deve causare una VM exit affinché l'hypervisor prenda il controllo.
Un altro flag determina se determinate istruzioni critiche devono provocare un cambio di contesto.
Infine, un insieme di flag specifica quali operazioni di I/O sono permesse direttamente dalla VM e quali richiedono una VM exit.

Nel campo VM enter control, ci sono dei campi che possono essere utilizzati dall'hypervisor per simulare una falsa interruzione. In particolare, le azioni intraprese includono:
1. **Salvataggio dello stato nel guest stack**: l'hypervisor salva lo stato corrente della CPU all'interno dello stack del sistema operativo guest per garantire un ripristino sicuro dopo la gestione dell'interruzione.
2. **Controllo della guest IDT**: per determinare l'indirizzo dell'handler dell'interruzione associato alla falsa interruzione da simulare.
3. **Modifica dei registri della CPU per eseguire l'handler**: l'hypervisor imposta i registri della CPU in modo appropriato per dirigere l'esecuzione verso l'handler dell'interruzione simulata.

## Gestione della memoria

Dal punto di vista delle singole macchine virtuali, la memoria fisica appare come un'unica entità continua, e ognuna di esse dispone della propria Memory Management Unit (MMU) per tradurre gli indirizzi virtuali in indirizzi fisici. 
È compito dell'hypervisor fornire questa astrazione in modo da garantire che ciascuna VM possa accedere alla memoria in modo isolato e sicuro, senza interferenze da parte delle altre VM.

Precedentemente, abbiamo esaminato il meccanismo delle Shadow Page Table, il quale, tuttavia, comportava un notevole overhead. Vediamo ora come tale problema sia stato risolto attraverso il supporto hardware.

Sia AMD che Intel hanno apportato modifiche all'hardware per agevolare la virtualizzazione della memoria virtuale dei guest. In particolare, hanno introdotto la **Extended Page Table** (**EPT**).

Con l'EPT, la MMU dell'host è stata estesa con due registri PTBR: uno per la page table della macchina virtuale guest (per implementare la funzione $G$ che mappa gli indirizzi virtuali del guest agli indirizzi fisici del guest) e un altro che punta alla page table creata dall'hypervisor (implementando la funzione $H$ che mappa gli indirizzi fisici del guest agli indirizzi fisici dell'host).

L'hardware stesso gestirà queste due tabelle in cascata utilizzando i puntatori appropriati, semplificando così il processo di traduzione degli indirizzi e riducendo l'overhead complessivo della virtualizzazione della memoria.

Con questo meccanismo non è necessario eseguire una VM exit ogni volta che il sistema operativo guest modifica la propria page table, poiché può farlo tranquillamente nel mode non-root. 

Tuttavia, uno svantaggio di questo approccio è l'aumento del costo della traduzione degli indirizzi, poiché sono necessari due accessi in memoria per ogni traduzione (per un totale di 24 accessi in memoria aggiuntivi). Per mitigare questo overhead, le MMU più recenti sono dotate di una cache TBL, che riduce il numero di accessi in memoria necessari per le traduzioni degli indirizzi.

## Hardware passthrough

La completa virtualizzazione si impegna a emulare ogni singolo dispositivo di input/output, un processo che inevitabilmente compromette le prestazioni delle macchine virtuali. 
L'hypervisor era costretto a intervenire in ogni interazione tra la macchina virtuale e il dispositivo per emularlo, generando così numerosi VM exits.

Per superare tali ostacoli, si è adottata la soluzione del **passthrough**, che consiste nell'assegnare un accesso diretto ed esclusivo alle periferiche. Questo approccio permette di mappare direttamente un dispositivo con la relativa macchina virtuale, evitando la necessità di compiere alcuna traduzione tramite emulazione. Questa strategia riduce notevolmente la latenza e migliora le performance complessive del sistema virtuale perché elimina la necessità di VM exits.
Tuttavia, va notato che questo approccio presenta uno svantaggio significativo: poiché l'hardware non è virtualizzato, il dispositivo può essere utilizzato solo dalla singola VM a cui è stato assegnato e non può essere condiviso con altre istanze virtuali o questo causerebbe inconsistenza. 

Per implementare questo meccanismo, è necessario un hardware specifico che gestisca due aspetti:
1. **Direct mapping**: una mappatura diretta tra lo spazio di memoria del dispositivo fisico e la macchina virtuale in modo che quest'ultima possa leggere direttamente i registri delle periferiche.
2. **Direct assignment**: assegnazione diretta di un dispositivo fisico a una singola macchina virtuale senza condivisione. In questo modo, la VM gestisce direttamente le interruzioni provenienti dal dispositivo pass-through, mentre l'hypervisor gestisce tutte le altre operazioni.

### I/O Mapping

L'obiettivo di questo meccanismo è di mappare una porzione dello spazio di I/O, che è reale, verso una porzione virtuale senza l'intervento dell'hypervisor.

Nell'architettura Intel VMX, la struttura dati VMCS include un bit chiamato **I/O bitmap** per ogni possibile dispositivo I/O. 
Questo bit specifica se il dispositivo è abilitato al pass-through o meno. 
Se il bit è pari a zero, si rende necessario un cambio di contesto e il controllo viene passato all'hypervisor che emulerà l'operazione. Al contrario, se il bit è pari a uno, lo spazio è accessibile direttamente dalla VM, anche in modalità non-root.

### Interruzioni

La seconda parte del meccanismo riguarda la gestione delle interruzioni, 