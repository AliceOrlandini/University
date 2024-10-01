### Modelli per sistemi distribuiti

#### Shared Memory e Message Passing

I principali modelli di **scambio di messaggi** nei sistemi distribuiti sono:
- **Shared Memory**: in questo modello, ogni CPU ha la propria *cache locale*, ma tutte le unità condividono una memoria centrale comune. La comunicazione tra le unità avviene indirettamente attraverso questa memoria condivisa. Le CPU accedono alla memoria condivisa tramite un **bus** comune, permettendo lo scambio di messaggi e dati tra i processi. Tuttavia, è necessario gestire la sincronizzazione per evitare problemi come i data races.
- **Message Passing**: in questo modello, ogni nodo è indipendente e ha la propria *memoria locale*. La comunicazione tra le unità avviene inviando e ricevendo messaggi attraverso un **network di interconnessione**. Questo modello non richiede memoria condivisa e ogni unità comunica esplicitamente con le altre attraverso messaggi, il che riduce i problemi di sincronizzazione e offre maggiore scalabilità, soprattutto in sistemi distribuiti su larga scala.

![[Shared Memory to Msg Passing.webp|center|400]]

#### Canali di interconnessione

Concentrandoci sul secondo tipo di **passaggio di messaggi**, possiamo realizzarlo attraverso due tipi di reti di comunicazione:
- **Grafo completamente connesso**: in questo modello, ogni unità è direttamente collegata a tutte le altre tramite archi. Ciò significa che ogni nodo può comunicare direttamente con ogni altro nodo, senza passare attraverso intermediari.
- **Grafo parzialmente connesso**: in questo caso, non tutte le unità sono collegate direttamente tra loro. Alcune comunicazioni devono passare attraverso altri nodi intermedi, il che può ridurre il numero di connessioni fisiche necessarie, ma potrebbe introdurre latenze e richiedere meccanismi più complessi per instradare i messaggi.

![[Channels.png|center|400]]

In entrambi i modelli, ogni arco può essere **unidirezionale** o **bidirezionale**, a seconda che i messaggi possano viaggiare in una sola direzione o in entrambe.

Un altro aspetto da considerare è l'ordine in cui vengono recapitati i messaggi. In alcuni sistemi, i messaggi seguono un ordine **FIFO**, cioè i messaggi inviati per primi vengono ricevuti per primi. In altri sistemi, questo ordine potrebbe non essere garantito, e i messaggi potrebbero arrivare fuori sequenza, richiedendo ulteriori meccanismi per garantire la coerenza della comunicazione.

#### Tipi di Modelli

Possiamo categorizzare i modelli di sistemi distribuiti in base al **livello di astrazione** che vogliamo adottare:
- **Modelli Fisici**: questi modelli si concentrano sugli aspetti hardware del sistema distribuito, come la configurazione delle reti fisiche, la disposizione delle CPU, le memorie, e le interconnessioni.
- **Modelli Architetturali**: a un livello di astrazione più alto, questi modelli descrivono la struttura del sistema in termini di **componenti** e delle loro **relazioni**. Esempi tipici sono architetture **client-server** o **peer-to-peer**, dove tutte le unità hanno ruoli simili. In questo contesto, è comune l'uso di un **middleware** per gestire la comunicazione e coordinazione tra i componenti, nascondendo i dettagli di implementazione e rendendo più semplice lo sviluppo di applicazioni distribuite.
- **Modelli Fondamentali (abstract)**: questi modelli si concentrano sulle proprietà essenziali e sui **principi base** che governano i sistemi distribuiti, come la comunicazione, la sincronizzazione, la coerenza dei dati e i possibili fallimenti. Non descrivono dettagli specifici dell'architettura o dell'hardware, ma evidenziano le caratteristiche teoriche del sistema, come la tolleranza ai guasti, i limiti di consistenza e la gestione dei ritardi nella comunicazione.

#### Messaggi

Il **passaggio di informazioni** tra i nodi di un sistema distribuito avviene tramite lo scambio di **messaggi**. Il contenuto principale del messaggio è noto come **payload**, e rappresenta i dati effettivi che devono essere trasferiti dalla memoria locale di un processo a quella di un altro.

Oltre al payload, il messaggio può includere **informazioni aggiuntive**, come: tipo di messaggio, metadati, mittente, ecc.
Le primitive fondamentali utilizzate per lo scambio di messaggi in un sistema distribuito sono: *send* e *receive*.

#### Resource Management

In un sistema che utilizza il **message passing**, le risorse possono essere associate a un processo dedicato, incaricato di interagire con esse. Se una risorsa deve essere condivisa tra più processi, il processo che ne ha la proprietà agisce come **manager** della risorsa. Questo manager è responsabile di coordinare e controllare l'accesso alla risorsa, garantendo che tutte le interazioni avvengano in modo sicuro e ordinato.

![[Resource Management.webp|center|300]]

### Indirizzamento dei Processi

Un processo in un'applicazione distribuita è identificato da un **PID** (Process Identifier), un identificatore univoco che lo rende *"trovabile"* dagli altri nodi, in modo che i messaggi possano essergli recapitati correttamente. Per questo, l'infrastruttura di un sistema distribuito ha bisogno di un sistema di **indirizzamento** che permetta ai processi di localizzarsi a vicenda.

Un approccio comune è l'uso delle **Overlay Networks**, un livello applicativo che si sovrappone a una rete di comunicazione reale. In altre parole, è una rete virtuale che consente la comunicazione tra processi o nodi su un'infrastruttura sottostante esistente. Un esempio pratico è l'architettura peer-to-peer utilizzata da applicazioni come Skype.

Esistono due principali tipologie di **indirizzamento** nei sistemi distribuiti:
- **Indirizzamento esplicito**: in questo caso, i messaggi vengono inviati direttamente a un PID o a un identificatore noto. Il processo destinatario è esplicitamente specificato e identificabile da chi invia il messaggio.
- **Indirizzamento implicito**: qui il mittente non ha bisogno di conoscere il PID del processo destinatario. Un esempio comune è l'uso delle **REST API**, dove si specifica un endpoint, e il mittente non sa né ha bisogno di sapere quale processo gestirà la richiesta. L'infrastruttura si occupa di instradare il messaggio al processo corretto.

#### Middleware

L'implementazione della comunicazione in un sistema distribuito tende a nascondere i dettagli legati alla rete, rendendo l'interazione tra processi o nodi più semplice e astratta. Questo viene realizzato utilizzando un **Middleware**, ovvero un software che opera sopra il sistema operativo e fornisce le funzionalità necessarie per abilitare la comunicazione tra le applicazioni.

Il **Middleware** agisce come un livello intermedio che si occupa di gestire le complessità della rete, come il routing, il trasporto e la sincronizzazione dei messaggi. Grazie a questo, le applicazioni possono comunicare tra loro senza preoccuparsi dei dettagli tecnici del network sottostante.
Un aspetto cruciale del middleware è la sua capacità di fornire **interoperabilità** tra applicazioni eseguite su diversi sistemi operativi o piattaforme hardware.

![[Middleware.webp|center|400]]

### Semantica della Send e Receive

Esistono due principali tipi di **modelli astratti** per descrivere la comunicazione e il comportamento nei sistemi distribuiti:
- **Modello Synchronous**: in questo modello, tutti i processi compiono i loro passi in maniera simultanea, seguendo lo stesso ritmo temporale. Le comunicazioni e le operazioni sono sincronizzate, il che rende il modello molto semplice da capire e da analizzare. Tuttavia, questo approccio è poco realistico per la maggior parte dei sistemi distribuiti, poiché nella realtà i processi raramente operano esattamente nello stesso tempo, a causa di variabili come la latenza della rete o la diversa velocità di elaborazione.
- **Modello Asynchronous**: in questo modello, *non ci sono vincoli di tempo* precisi per l'esecuzione dei processi. Le comunicazioni possono subire **unbounded delays** (ritardi indefiniti), e non esiste un **orologio globale** che sincronizzi le operazioni tra i processi. Ciò rende il modello più realistico, poiché riflette meglio la natura dei sistemi distribuiti reali, dove la latenza della rete e i tempi di elaborazione possono variare notevolmente. Tuttavia, questa mancanza di sincronizzazione introduce complessità nella gestione della coerenza e della sincronizzazione dei dati.

Le primitive di comunicazione **send** e **receive** in un sistema distribuito possono comportarsi in due modi:
- **Blocking**: in questo caso, durante l'esecuzione di una delle due primitive, il processo si blocca fino a quando l'operazione non viene completata.
- **Non-blocking**: qui, l'operazione viene avviata ("lanciata") e il processo continua la sua esecuzione senza aspettare che l'invio o la ricezione siano completati. Spesso, in questo modello, i messaggi vengono bufferizzati in coda fino a quando non possono essere processati.

L'implementazione di queste primitive richiede generalmente l'uso di **mailboxes**, ovvero delle **code di invio e ricezione**, esse permettono ai processi di continuare a eseguire altre operazioni mentre i messaggi vengono gestiti in background.

Nel caso specifico della primitiva **send**, se la modalità è **bloccante**, il processo invia il messaggio alla destinazione e continua la sua esecuzione solo dopo aver verificato che il messaggio è stato effettivamente ricevuto dal destinatario. Questo comportamento implica l'uso di un **handshake**. 
Nella modalità **non bloccante**, invece, il messaggio viene inviato senza attendere conferme, un approccio chiamato *"inviato e dimenticato"*.

Per quanto riguarda la primitiva **receive**, in modalità **bloccante** il processo resta in attesa finché non arriva un messaggio nella incoming queue. In modalità **non bloccante**, il processo controlla se c'è un messaggio disponibile nella coda. Se c'è un messaggio, lo preleva; se non c'è, il processo riceve un messaggio di errore o un'indicazione di coda vuota e continua la sua esecuzione senza bloccarsi.

Quando si utilizzano coppie di primitive **bloccanti** e **non bloccanti**, bisogna prestare molta attenzione, perché una gestione errata di queste operazioni può portare a **deadlock**, ossia situazioni in cui i processi si bloccano aspettando indefinitamente risposte o risorse che non arriveranno mai.

Per evitare tali problemi, esiste un costrutto chiamato **guarded command**, in cui una condizione viene verificata prima di eseguire un blocco di codice bloccante. Se la condizione è soddisfatta, il codice bloccante viene eseguito; altrimenti, l'operazione viene saltata, evitando così blocchi non necessari.

Ecco un esempio di utilizzo delle primitive:

![[Bounded Buffer.webp|center|500]]
