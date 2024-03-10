
I requisiti chiave per la virtualizzazione sono:
1. **Equivalenza**: il sistema operativo in esecuzione su una macchina virtuale sopra un hypervisor deve comportarsi in modo equivalente a come lo farebbe se eseguito direttamente sulla macchina fisica. Questo assicura la coerenza delle operazioni tra le due modalità di esecuzione.
2. **Controllo delle Risorse**: l'hypervisor deve avere il completo controllo delle risorse fisiche del sistema, mentre il sistema operativo guest deve gestire le risorse virtuali fornite dall'hypervisor. Questo divisione di controllo è essenziale per garantire la separazione e l'isolamento tra le macchine virtuali.
3. **Efficienza**: affinché la virtualizzazione sia efficiente, una parte delle istruzioni deve essere eseguita direttamente sull'hardware senza passare attraverso l'hypervisor. Ciò assicura una maggiore performance ed evita un eccessivo overhead dovuto alla traduzione delle istruzioni.

Ricordare: il set di istruzioni usato nel sistema virtuale è lo stesso utilizzato nell'hardware effettivo.

## Multiprogrammazione

I requisiti che abbiamo delineato sono analoghi a quelli richiesti per la multiprogrammazione, ossia la capacità di un sistema di eseguire *"contemporaneamente"* più processi. 

Attraverso la multiprogrammazione, è possibile simulare una macchina con più di un processore, anche se tale caratteristica non è presente nell'hardware effettivo. Ogni processo viene eseguito su un processore virtuale (VCPU). 
I processori virtuali diventano effettivi a intervalli di tempo definiti, seguendo un approccio *"round robin"* molto rapido, garantendo così che il processo abbia l'illusione di avere il controllo completo del processore durante la sua esecuzione.

## VCPU e Cambio di Contesto

La realizzazione della multiprogrammazione si basa sull'utilizzo di rappresentazioni virtuali del processore (VCPU), le quali sono materializzate attraverso una struttura dati che conserva lo stato attuale del processore, comprensivo del contenuto dei suoi registri.

Il **cambio di contesto** è un'operazione che implica la sospensione di un processo attivo. Tale procedura comporta il salvataggio dello stato corrente del processo e la selezione di un altro processo. 
Quest'ultimo viene quindi caricato nel processore fisico, permettendogli di proseguire l'esecuzione. In pratica, il cambio di contesto assicura una transizione fluida tra processi, consentendo al sistema di sfruttare al massimo le risorse disponibili e di garantire un'efficace gestione della concorrenza tra processi.
L'operazione di cambio di contesto è implementata sia in hardware che in software.

<div style="page-break-after: always;"></div>


## Supporto alla multiprogrammazione

La multiprogrammazione necessita di supporti hardware fondamentali, tra cui:
- **Interruzioni**: ne esistono di varie tipologie, un esempio è un timer che, al suo scadere, attiva il cambio di contesto. 
- **Copia stato**: si tratta di un meccanismo automatico che consente la copia dello stato dei registri dalla struttura dati ai registri fisici o viceversa. Quest'operazione viene eseguita a livello hardware per garantire maggiore efficienza, dato che deve essere ripetuta frequentemente.
- **Protezione**: questo meccanismo è progettato per prevenire accessi non autorizzati alle porzioni di memoria non pertinenti al processo in esecuzione, oltre a scoraggiare l'uso improprio di funzioni riservate a livello di sistema.

I processori moderni offrono il supporto a diversi **livelli di privilegio**, con un minimo di due: il livello utente e il livello sistema (o kernel), che si distinguono per l'accesso completo alla memoria e a tutte le primitive del sistema. 
Un processo in esecuzione a livello utente non può autonomamente passare al livello sistema. Tuttavia, in seguito a un'interruzione, è possibile effettuare una transizione da un livello all'altro per eseguire una specifica routine di sistema, nota come handler, la quale viene eseguita a livello sistema. 
Questo meccanismo permette l'implementazione efficace del cambio di contesto senza mettere a rischio la sicurezza dei singoli processi.

Le caratteristiche della protezione includono:
- Un *flag usr/sys*, il quale indica la modalità attuale della CPU.
- Un *registro sys-mem* che conserva l'indirizzo di inizio della memoria riservata al sistema, dove sono archiviate le strutture dati della VCPU.
- Un *registro ret* contenente l'indirizzo di memoria che sarà utilizzato quando il controllo tornerà all'utente, consentendo la ripresa del proprio programma.

Quando un timer genera un'interruzione, provocando il cambio di contesto, il sistema operativo esegue le seguenti operazioni:
1. Conserva lo stato attuale della CPU nella rappresentazione virtuale (VCPU).
2. Seleziona un nuovo processo dalla coda e carica la sua VCPU nei registri effettivi della CPU fisica.
3. Effettua un'istruzione speciale per passare all'indirizzo specificato nel registro ret e ritorna al livello utente.

Nel contesto dei processori moderni, le fasi 1 e 2 di queste operazioni vengono eseguite a livello hardware.

<div style="page-break-after: always;"></div>


## Memoria Virtuale

Per quanto riguarda la memoria, il nostro obiettivo è raggiungere il medesimo obiettivo proposto per la CPU, mirando a fornire al processo la percezione di avere accesso illimitato alla memoria, benché tale risorsa sia limitata nella realtà. 
Questo viene realizzato attraverso la creazione di uno spazio di indirizzi "virtuale" dedicato ad ogni processo, il quale crea l'illusione di memorizzare i dati in modo continuo, anche se ciò potrebbe non verificarsi fisicamente. 

La responsabilità di convertire tutti gli spazi virtuali nei rispettivi indirizzi della RAM fisica è attribuita alla **MMU** (Memory Management Unit), implementata in hardware per garantire maggiore velocità. 
D'altra parte, l'assegnazione e la gestione degli indirizzi virtuali sono compiti del sistema operativo. Questo approccio consente agli indirizzi fisici di essere condivisi, in un certo senso, da più processi, ciascuno con il proprio segmento.

La traduzione degli indirizzi avviene prima di raggiungere la CPU, poiché quest'ultima può operare solo con indirizzi fisici anziché virtuali. Per accelerare il processo di traduzione, è possibile introdurre una cache nota come **TLB** (Translation Lookaside Buffer).

La MMU si occupa della traduzione degli indirizzi utilizzando una struttura dati denominata **Page Table**, caratterizzata da una disposizione gerarchica. 
La CPU dispone di un registro denominato PTBR (Page Table Base Register), il quale indica l'indirizzo fisico in cui è archiviata la Page Table del processo attualmente in esecuzione.

La memoria virtuale è organizzata in porzioni di dimensioni uniformi, chiamate pagine. 
La Page Table si struttura in *quattro livelli*, ciascuno apportando informazioni per la ricostruzione dell'indirizzo fisico corrispondente. 
Questa configurazione è adottata per ottimizzare la gestione delle Page Table, le quali tendono ad assumere dimensioni considerevoli, e per migliorare il controllo della sicurezza nell'accesso alle pagine.

Il volume dello spazio di memoria virtuale può superare quello della memoria fisica grazie all'impiego del meccanismo di segmentazione e di una memoria secondaria, come ad esempio un hard disk. 
Questa configurazione consente di allocare temporaneamente le pagine meno utilizzate nella memoria secondaria. Quando necessario, queste pagine possono essere caricare dinamicamente in memoria, generando un'eccezione di *page fault*. 
L'intero processo è implementato a livello hardware per assicurare un'elevata efficienza.

<div style="page-break-after: always;"></div>


## Interruzioni ed Eccezioni

Le **interruzioni** e le **eccezioni** sono meccanismi che richiedono l'intervento del sistema operativo per eseguire *operazioni privilegiate* (nel caso delle interruzioni) e *risolvere situazioni di errore* per ripristinare il sistema a uno stato coerente (nel caso delle eccezioni). 
Quando vengono innescate, il sistema operativo assume il controllo tramite un cambio di contesto, e va a eseguire in modo *atomico* una serie di operazioni prima di restituire il controllo ai processi (handler).

Una caratteristica chiave che distingue le due è che le interruzioni sono *asincrone*, il che significa che possono essere sollevate e gestite in un momento successivo, mentre le eccezioni sono *sincrone*, ciò implica che, nel momento in cui vengono sollevate, vengono gestite immediatamente dal sistema operativo.

Una categoria particolare di eccezioni è la **trap**, la quale si distingue per la sua capacità di essere chiamata tramite software mediante il comando:

```c
$int codice_eccezione
```

Attraverso il codice eccezione, rappresentato da un numero specifico, è possibile interrogare una tabella denominata **IDT** (Interrupt Descriptor Table) utilizzata dal processore per collegare interruzioni ed eccezioni col relativo handler. Questa tabella è popolata dal sistema operativo durante la fase di bootstrap. 

