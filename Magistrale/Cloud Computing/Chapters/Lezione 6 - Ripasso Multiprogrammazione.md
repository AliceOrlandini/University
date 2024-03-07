
I requisiti chiave per la virtualizzazione sono:
1. **Equivalenza**: il sistema operativo in esecuzione su una macchina virtuale sopra un hypervisor deve comportarsi in modo equivalente a come lo farebbe se eseguito direttamente sulla macchina fisica. Questo assicura la coerenza delle operazioni tra le due modalità di esecuzione.
2. **Controllo delle Risorse**: l'hypervisor deve avere il completo controllo delle risorse fisiche del sistema, mentre il sistema operativo guest deve gestire le risorse virtuali fornite dall'hypervisor. Questo divisione di controllo è essenziale per garantire la separazione e l'isolamento tra le macchine virtuali.
3. **Efficienza**: affinché la virtualizzazione sia efficiente, una parte delle istruzioni deve essere eseguita direttamente sull'hardware senza passare attraverso l'hypervisor. Ciò assicura una maggiore performance ed evita un eccessivo overhead dovuto alla traduzione delle istruzioni.

Ricordare: il set di istruzioni usato nel sistema virtuale è lo stesso utilizzato nell'hardware effettivo.

## Multiprogrammazione

I requisiti che abbiamo delineato sono analoghi a quelli richiesti per la multiprogrammazione, ossia la capacità di un sistema di eseguire *"contemporaneamente"* più processi. 

Attraverso la multiprogrammazione, è possibile simulare una macchina con più di un processore, anche se tale caratteristica non è presente nell'hardware effettivo. Ogni processo viene eseguito su un processore virtuale (VCPU). 
I processori virtuali diventano effettivi a intervalli di tempo definiti, seguendo un approccio "round robin" molto rapido, garantendo così che il processo abbia l'illusione di avere il controllo completo del processore durante la sua esecuzione.

## VCPU e Cambio di Contesto

La realizzazione della multiprogrammazione si basa sull'utilizzo di rappresentazioni virtuali del processore, le quali sono materializzate attraverso una struttura dati che conserva lo stato attuale del processore, comprensivo del contenuto dei suoi registri.

Il **cambio di contesto** è un'operazione che implica la sospensione di un processo attivo. Tale procedura comporta il salvataggio dello stato corrente del processo e la selezione di un altro processo. 
Quest'ultimo viene quindi caricato nel processore fisico, permettendogli di proseguire l'esecuzione. In pratica, il cambio di contesto assicura una transizione fluida tra processi, consentendo al sistema di sfruttare al massimo le risorse disponibili e di garantire un'efficace gestione della concorrenza tra processi.
L'operazione di cambio di contesto è implementata sia in hardware che in software.

## Supporto alla multiprogrammazione

La multiprogrammazione necessita di supporti hardware fondamentali, tra cui:
- **Interruzioni**: ne esistono di varie tipologie, un esempio è un timer che, al suo scadere, attiva il cambio di contesto. 
- **Copia stato**: si tratta di un meccanismo automatico che consente la copia dello stato dei registri dalla struttura dati ai registri fisici o viceversa. Quest'operazione viene eseguita a livello hardware per garantire maggiore efficienza, dato che deve essere ripetuta frequentemente.
- **Protezione**: questo meccanismo è progettato per prevenire accessi non autorizzati alle porzioni di memoria non pertinenti al processo in esecuzione, oltre a scoraggiare l'uso improprio di funzioni riservate a livello di sistema.

I processori moderni offrono il supporto a diversi livelli di privilegio, con un minimo di due: il livello utente e il livello sistema (o kernel), che si distinguono per l'accesso completo alla memoria e a tutte le primitive del sistema. 
Un processo in esecuzione a livello utente non può autonomamente trasferirsi al livello sistema. Tuttavia, in seguito a un'interruzione, è possibile effettuare una transizione da un livello all'altro per eseguire una specifica routine di sistema, nota come handler, la quale viene eseguita a livello sistema. 
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