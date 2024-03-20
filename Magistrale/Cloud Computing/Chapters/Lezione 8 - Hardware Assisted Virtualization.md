
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


## Gestione della memoria

