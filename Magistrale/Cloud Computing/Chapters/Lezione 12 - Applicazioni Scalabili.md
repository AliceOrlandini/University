## Architettura delle applicazioni Cloud

Le applicazioni cloud sono sviluppate come sistemi distribuiti, dove la loro implementazione è una composizione di diverse funzionalità ospitate su macchine virtuali diverse. Queste VM sono organizzate in cluster, denominati tiers, all'interno dei quali ogni VM può replicare le funzionalità per garantire disponibilità e scalabilità. 

Le richieste provenienti dai clienti sono inizialmente ricevute dal primo tier, noto come frontend tier. Questo tier funge da punto di ingresso per le richieste esterne e si occupa di instradarle agli altri tiers, chiamati backend che contengono la logica applicativa e i dati necessari per elaborare le richieste. 
Questa architettura a tier consente una gestione efficiente del carico di lavoro e una migliore scalabilità, poiché le risorse possono essere allocate e scalate in modo indipendente in base alle esigenze del sistema.

La metodologia di design utilizzata è la SOA (Service-Oriented Architecture), in cui ogni VM ospita servizi modulari e indipendenti che interagiscono tramite message passing, utilizzando protocolli come SOAP o REST.

## Request-Reply communication

Entrambe SOAP e REST adottano un protocollo di comunicazione Request-Reply, il che implica una comunicazione sincrona in cui il client rimane in attesa fino a ricevere una risposta dal server (*time-coupled*). Questo tipo di comunicazione è affidabile in quanto la risposta funge da conferma di ricezione della richiesta.
Inoltre, questa comunicazione è diretta, consentendo al client e al server di interagire senza intermediari (*space-coupling*). Tuttavia, questa caratteristica può presentare delle sfide in alcuni contesti, poiché può essere necessario un intermediario per garantire la scalabilità, la sicurezza o altre esigenze di gestione della comunicazione.

Nei sistemi backend, dove la scalabilità e l'affidabilità sono prioritari, l'approccio basato su time e space coupling può compromettere le prestazioni. Una comunicazione sincrona diretta tra client e backend può sovraccaricare il sistema e limitare la flessibilità nel ridimensionamento delle risorse. È preferibile adottare approcci asincroni e a basso accoppiamento per distribuire le richieste in modo efficace su più istanze del backend, garantendo una migliore gestione del carico e una maggiore affidabilità.

## Comunicazione indiretta

Il paradigma di comunicazione indiretta è fondamentale per garantire due aspetti chiave:
1. **Space uncoupling**: qui, mittente e destinatario non devono conoscere l'identità reciproca. Questo permette la sostituzione dinamica dei partecipanti alla comunicazione (come la creazione o la rimozione di una VM). Per ottenere ciò, si introduce un intermediario che non ha un accoppiamento diretto tra mittente e destinatario.
2. **Time uncoupling**: in questo contesto, mittente e destinatario hanno "vite" indipendenti. Ciò significa che possono esistere in tempi diversi durante la comunicazione, anche in un ambiente volatile. Questo è reso possibile dall'utilizzo di una comunicazione asincrona, in cui il mittente invia il messaggio all'intermediario e prosegue senza attendere, consentendo all'intermediario di consegnare il messaggio in modo tempestivo al destinatario. In questo modo, non è necessario che mittente e destinatario si trovino contemporaneamente nello stesso momento.

## Comunicazione orientata ai messaggi

Per le applicazioni cloud sono stati introdotti nuovi paradigmi di comunicazione di tipo *indiretto* e *message-oriented*. In questi modelli, la sorgente e la destinazione non stabiliscono un canale di comunicazione diretto (come avviene in TCP/UDP), ma il trasmettitore inietta il messaggio in un *bus condiviso*.

Questo bus condiviso è gestito da un intermediario: il trasmettitore invia il messaggio all'intermediario, il quale si occupa di recapitarlo al destinatario. Lo stesso processo avviene in caso di una risposta.
Nel tempo, sono emersi due principali tipi di intermediari: il **broker** e la **message queue**.

### Broker

La prima implementazione che possiamo considerare è il meccanismo di *publish-subscribe*. In questo modello, i trasmettitori, chiamati *publishers*, inviano strutture dati, solitamente eventi, mentre i ricevitori, detti *subscribers*, si iscrivono per ricevere i messaggi di loro interesse.

Al centro del sistema troviamo il *message broker*, che si occupa di inoltrare i messaggi dai *publishers* ai *subscribers*. Questo modello permette una comunicazione di tipo uno-a-molti (1 → many).

Esistono tre principali modalità di sottoscrizione:
- **Channel Based**: i subscribers si iscrivono a canali specifici, ricevendo tutti i messaggi che vi transitano, indipendentemente dal contenuto.
- **Topic Based**: i subscribers si iscrivono a uno o più *topic*, ricevendo solo i messaggi associati a quei *topic*, offrendo una maggiore granularità rispetto al modello basato sui canali.
- **Type Based**: i subscribers si iscrivono in base al *tipo di dato* o messaggio, ricevendo solo quelli che corrispondono a un certo tipo predefinito.
### Message Queue

Il secondo approccio implementativo è il *message queue*, che fornisce uno scambio di messaggi di tipo point-to-point tra produttore e consumatore. In questo modello, il trasmettitore inserisce un messaggio in una coda, che verrà poi rimosso e processato dal ricevitore.

Lo scambio avviene all'interno del *Message Queueing System*, dove possono esistere più code. Sia il trasmettitore che il ricevitore possono decidere in quale coda inserire o prelevare i messaggi, garantendo così flessibilità nel flusso di comunicazione.

Il set di operazioni offerte dal *Message Queueing System* è semplice e comprende:
- **Send**: utilizzato dal produttore per inserire messaggi nella coda.
- **Blocking Receive**: utilizzato dal consumatore per attendere in modo sincrono fino a quando un messaggio di suo interesse diventa disponibile nella coda.
- **Non-blocking Receive**: chiamato anche polling, permette al consumatore di verificare lo stato della coda in modo asincrono. Se un messaggio è disponibile, viene restituito, altrimenti non accade nulla.
- **Notify**: utilizzato dal system message queue per notificare al consumatore la disponibilità di un messaggio nella coda a cui è iscritto.

#### Messaggi

Ogni messaggio è composto da un identificatore della coda di destinazione e da metadati, come la priorità o la modalità di consegna. Il corpo del messaggio, invece, è solitamente ignorato dal sistema stesso.
Le code seguono generalmente una politica FIFO (First In, First Out), anche se alcune supportano priorità per gestire l'ordine di consegna dei messaggi. Inoltre, un consumatore può selezionare i messaggi dalla coda in base alle loro proprietà specifiche.

Una caratteristica fondamentale del *message queue system* è la **persistenza** dei messaggi: la coda memorizza i messaggi indefinitamente (finché non vengono consumati) e li salva su disco per garantire una consegna affidabile.

È possibile applicare una trasformazione ai messaggi all'arrivo, una pratica utile soprattutto per facilitare la comunicazione tra sistemi eterogenei. Un esempio comune è la conversione tra formati di notazione come *big-endian* e *little-endian*, permettendo così l'interoperabilità tra dispositivi che utilizzano rappresentazioni dati differenti.

#### Scalabilità

Le code di messaggi sono ideali per i servizi di comunicazione nel backend di un'applicazione cloud, poiché gestiscono efficacemente la dinamicità dell'ambiente. Ad esempio, possono supportare sistemi in cui il numero di VM in un cluster varia per gestire il *load balancing*. 
In questo scenario, i produttori non devono attendere la creazione o l'attivazione di una VM per elaborare il messaggio; possono semplicemente inserirlo nella coda, consentendo una gestione più flessibile e scalabile dei carichi di lavoro.

### Implementazione

Sia i *Brokers* che i *Message Queuing Systems* possono essere implementati in modo centralizzato o distribuito. Nella versione centralizzata, sono rappresentati da una singola VM, con gli svantaggi di mancanza di resilienza e scalabilità, poiché costituiscono un unico punto di fallimento e possono diventare un collo di bottiglia per le prestazioni.

L'approccio distribuito, sebbene più complesso, offre coordinazione e replica dei dati, garantendo maggiore scalabilità e resilienza. 

L'*Advanced Message Queueing Protocol* (AMQP) è uno dei protocolli più adottati per implementare i *Message Queueing Systems*, grazie alle sue capacità di interoperabilità e affidabilità.

## Replicazione dati

Nelle applicazioni cloud, ogni *tier* è composto da un cluster di VM che implementano la stessa funzionalità per garantire performance, scalabilità, disponibilità e tolleranza ai guasti. La replicazione dei dati tra le VM all'interno dello stesso *tier* è essenziale per assicurare che tutte possano operare sullo stesso insieme di dati aggiornati.

La replicazione dei dati è solitamente realizzata attraverso lo scambio di messaggi tra le VM del cluster. Quando si gestiscono grandi dataset, si ricorre spesso a uno storage condiviso; tuttavia, anche in questo scenario, lo scambio di messaggi tra VM rimane cruciale per il coordinamento delle operazioni.

I requisiti fondamentali per la replicazione dei dati sono:
- **Replication Transparency**: la replicazione deve essere trasparente agli utenti, permettendo loro di accedere a copie diverse dei dati senza notare alcuna differenza.
- **Consistency**: tutte le repliche devono essere consistenti tra loro, in modo che un'operazione eseguita su una replica produca lo stesso risultato su tutte le altre.
- **Resiliency**: il sistema deve essere resiliente ai fallimenti delle VM, garantendo continuità del servizio. Si assume che le partizioni di rete non si verifichino, un'ipotesi ragionevole dato che le VM sono generalmente connesse alla stessa LAN.

### System Model 

In un sistema, i dati sono rappresentati come una collezione di oggetti, ad esempio file, classi o set di record. Ogni oggetto logico è implementato come una collezione di copie fisiche, dette repliche. 

Ciascuna replica è gestita da un *replica manager*, un componente responsabile della memorizzazione di un'istanza della replica. Per garantire la consistenza, le repliche sono trattate come *state machines*, il che significa che non solo i dati, ma anche lo stato della macchina, vengono replicati.
Ogni replica accetta operazioni di aggiornamento sui dati in modo recuperabile, consentendo il ripristino delle operazioni in caso di fallimento, garantendo così l'affidabilità del sistema.

## Gestione delle richieste

Una richiesta viene classificata come *query* se non altera lo stato dell'oggetto, mentre viene considerata un *update* se modifica lo stato.

Le fasi principali per gestire le richieste dei client sono:
1. **Request**: la richiesta viene ricevuta da un modulo di frontend, che la inoltra a uno o più *replica managers* (un'istanza del primo tier del backend).
2. **Coordinazione**: i *replica managers* si coordinano per preparare l'esecuzione della richiesta. Questo può includere la decisione se la richiesta può essere eseguita immediatamente o deve essere posticipata per garantire la consistenza.
3. **Esecuzione**: la richiesta viene eseguita dai *replica managers*, in base al tipo di richiesta (query o update).
4. **Agreement**: i *replica managers* si assicurano di aver raggiunto un consenso sul risultato della richiesta, specialmente per gli *update*, per garantire la consistenza tra le repliche.
5. **Response**: il risultato della richiesta viene inviato al client tramite il modulo di frontend.

Per la fase di *coordinazione* e *agreement*, viene utilizzato il **Group/Multicast Communication**, che permette di inviare un messaggio simultaneamente a tutti i membri di un gruppo. Questo meccanismo semplifica la comunicazione collettiva tra i *replica managers*, aumentando l'efficienza nel raggiungere un consenso. Tuttavia, l'adozione di questo approccio introduce maggiore complessità nel sistema, poiché richiede una gestione accurata della sincronizzazione e della consistenza tra tutte le repliche.

Esistono diverse proprietà fondamentali dei sistemi Multicast:
- **Reliable/Atomic Multicast**: garantisce che tutti i membri di un gruppo ricevano o non ricevano un messaggio, assicurando che nessuno resti in uno stato incoerente. Questo approccio è cruciale per evitare situazioni in cui alcune repliche applicano un aggiornamento e altre no.
- **Total/Causal Order Multicast**: 
  - Nel *Total Order Multicast*, tutti i messaggi vengono ricevuti dai membri del gruppo nello stesso ordine, indipendentemente dall'ordine di invio.
  - Nel *Causal Order Multicast*, l'ordine dei messaggi riflette le dipendenze causali, cioè i messaggi vengono ricevuti in un ordine che rispetta le relazioni causa-effetto.
- **View-Synchronous Communication**: in questo modello, ogni replica ha una visione condivisa del gruppo, e qualsiasi cambiamento nella composizione del gruppo (ad esempio, l'ingresso o l'uscita di un membro) è noto a tutti i partecipanti prima che possano essere inviati nuovi messaggi, garantendo una comunicazione coerente.

### Primary Backup Replication

La **Passive** o **Primary Backup Replication** è una delle strategie di replicazione più semplici, utilizzata per garantire alta disponibilità e tolleranza ai guasti. In questo schema, una singola replica viene selezionata come primaria (*master*), mentre le altre fungono da backup (*slave*).

I frontends comunicano esclusivamente con il *primary replica manager*, che gestisce le richieste e si occupa di inviare le copie degli aggiornamenti ai backup. In caso di fallimento del primario, uno dei backup viene eletto come nuovo primario, assicurando la continuità del servizio.

Vediamo le cinque fasi della **Primary Backup Replication**:
1. **Request**: Il frontend invia una richiesta al primario, includendo un identificatore univoco per evitare duplicati.
2. **Coordinazione**: Il *primary replica manager* gestisce le richieste nell'ordine di arrivo. Controlla l'identificatore della richiesta e, se è già stata eseguita, restituisce la risposta precedente. Non è necessaria la coordinazione tra i *replica managers* in questa fase.
3. **Esecuzione**: Il primario esegue la richiesta, memorizzando sia lo stato aggiornato che la risposta.
4. **Agreement**: Se la richiesta comporta un aggiornamento, il primario invia lo stato aggiornato, la risposta e l'identificatore univoco a tutti i backup. I backup confermano la ricezione inviando un ACK. Per questa fase si può utilizzare l'*atomic multicast* per garantire la consistenza.
5. **Response**: Il primario risponde al frontend, che inoltra la risposta al client.
### Active Replication

**Active Replication** è un approccio per garantire bilanciamento del carico e disponibilità. In questo modello, i replica managers sono organizzati in gruppi e il frontend invia le richieste in multicast direttamente a tutto il gruppo. Ogni replica esegue la richiesta in modo indipendente e restituisce una risposta identica. Se una delle repliche va in crash, non ci sono impatti significativi sulle prestazioni, poiché le repliche rimanenti possono continuare a rispondere.

Le cinque fasi dell'**Active Replication** sono:
1. **Richiesta**: Il frontend aggiunge un identificatore univoco alla richiesta e la invia in multicast a tutti i replica managers.
2. **Coordinazione**: Non è necessaria coordinazione esplicita, poiché il multicast si occupa di distribuire la richiesta a tutte le repliche. È essenziale utilizzare un _total order multicast_ per garantire che le richieste vengano ricevute nello stesso ordine da tutti i replica managers.
3. **Esecuzione**: Ogni replica manager esegue la richiesta in modo autonomo.
4. **Accordo**: Grazie al _total order multicast_, non è richiesto alcun accordo aggiuntivo tra le repliche.
5. **Risposta**: Ogni replica manager invia la propria risposta al frontend. Poiché il frontend riceve le risposte in ordine determinato dal _total order multicast_, può utilizzare la prima risposta ricevuta.

### Approccio alternativo

Gli approcci passivi e attivi si concentrano principalmente nel garantire **alta disponibilità (high availability)**. Tuttavia, esiste un approccio alternativo che mira a garantire sia **alta disponibilità** che **scalabilità** contemporaneamente.

In questo modello, diversi replica managers gestiscono le richieste provenienti dal frontend in parallelo (non solo uno che riceve le richieste o tutti che le elaborano), ottimizzando così l’utilizzo delle risorse. Gli aggiornamenti tra i replica managers non vengono propagati immediatamente, ma sono differiti quando possibile, mentre il controllo viene restituito al client il più rapidamente possibile.
## Gossip Architecture

Nell'architettura **gossip**, i replica managers si scambiano periodicamente messaggi di gossip per trasmettere gli aggiornamenti ricevuti dai client. I client vengono assegnati a istanze specifiche del frontend per bilanciare il carico (_load balancing_), ma possono essere riassegnati se l'istanza a cui sono collegati subisce un crash.

I **service provider** eseguono due operazioni di base:
- **Query**: operazioni di sola lettura.
- **Update**: modificano lo stato dell'oggetto, ma non leggono lo stato corrente prima di eseguire l'aggiornamento.

Il sistema gossip garantisce due aspetti fondamentali:
1. **Consistenza nel tempo per il client**: ogni client otterrà una visione consistente del sistema. La risposta a una query verrà fornita da un replica manager i cui dati riflettono l'ultimo aggiornamento osservato dal client (o il suo stesso aggiornamento).
2. **Consistenza tra le repliche**: le repliche possono raggiungere la consistenza nel tempo grazie allo scambio frequente di messaggi di gossip. Ciò significa che un client potrebbe ricevere una versione leggermente più vecchia ma comunque consistente delle informazioni quando esegue una query. È garantita la **sequential consistency**.

Le cinque fasi del **Gossip** sono:
1. **Richiesta**: Il frontend invia solitamente le richieste a un singolo replica manager alla volta.
2. **Coordinazione**: Il replica manager che riceve la richiesta non la processa immediatamente, ma attende che vengano soddisfatti i vincoli di ordine. Questo potrebbe richiedere di ricevere aggiornamenti da altri replica manager attraverso i messaggi gossip. Non è necessario alcun coordinamento aggiuntivo tra i replica managers. 
3. **Esecuzione**: Il replica manager esegue la richiesta non appena i vincoli sono rispettati.
4. **Agreement**: Dopo l'esecuzione, il replica manager aggiorna i dati inviando e ricevendo messaggi gossip contenenti l'aggiornamento più recente. Questi aggiornamenti vengono scambiati in modo ritardato (_lazy fashion_).
5. **Risposta**: Se la richiesta è una query, il replica manager invia una risposta al frontend subito dopo l'esecuzione. Se invece è un'operazione di aggiornamento, risponde una volta che l'aggiornamento è stato ricevuto.

#### Timestamps

Per mantenere l'ordine delle richieste, vengono utilizzati i **timestamp**, il che richiede la sincronizzazione degli orologi di tutti i frontend. Ogni frontend mantiene un vettore di timestamp che rappresenta la versione dell'ultimo valore a cui ha avuto accesso. Questo timestamp viene inviato ai replica manager insieme alla query. Il replica manager fornisce un nuovo timestamp insieme alla risposta, e i timestamp ritornati vengono combinati con quelli precedenti.

#### Processazione di una query

Il timestamp associato ad una query riflette l'ultima versione del valore che il frontend ha letto o 

#### Processazione di un Update
## ZooKepeer
