### [[Intro#2. Qual è il protocollo alla base del core di Internet? Qual è il suo limite maggiore e come lo si può risolvere?|1. Cosa si intente per scalabilità di una rete?]]

### [[Intro#3. Come si raggiunge flessibilità nelle reti? Qual è la differenza con la scalabilità?|2. Cosa si intende per flessibilità di una rete?]]

### 3. Cosa si intende per network performance?

Le performance di una rete possono essere ottimizzate attraverso il **traffic engineering**, un approccio che non si limita a far arrivare i pacchetti a destinazione, ma si preoccupa di farlo in modo efficiente per tutta la rete. L'obiettivo del traffic engineering è migliorare l'utilizzo delle risorse di rete, ridurre la congestione e bilanciare il carico tra vari percorsi disponibili.

Quando si inviano pacchetti tra due **edge routers**, spesso esistono più path disponibili attraverso la rete. I protocolli tradizionali di **IP routing** (come OSPF o BGP) scelgono generalmente un solo percorso ottimale basato su metriche come il numero di salti (hop count) o la capacità di trasmissione attuale di quel singolo percorso.
Il routing tradizionale tuttavia non sfrutta appieno tutti i possibili **path** che collegano i due router. Questo approccio può portare a un utilizzo inefficiente della rete: alcuni percorsi potrebbero essere congestionati, mentre altri rimangono sottoutilizzati. In questo scenario, la rete non è utilizzata al massimo della sua capacità.

Il **traffic engineering** mira a risolvere questo problema sfruttando tutti i percorsi disponibili tra due punti della rete, distribuendo i pacchetti su diversi percorsi attraverso il **load balancing**. Questo significa che invece di instradare tutto il traffico su un solo percorso, il traffico viene distribuito tra più percorsi, migliorando l'efficienza complessiva e riducendo il rischio di congestionare una singola path.

In pratica, il traffic engineering cerca di:
- **Bilanciare il carico**: distribuire il traffico su più percorsi per evitare il sovraccarico di uno solo.
- **Aumentare la resilienza**: distribuendo il traffico, la rete è meno vulnerabile a guasti su un singolo percorso.

### 4. Come funziona il Label Switching?

Il **label switching** si basa su due azioni fondamentali:
1. **Classificazione (Forwarding Equivalent Classes - FEC)**: In questa fase, i pacchetti vengono suddivisi in **Forwarding Equivalent Classes (FEC)**, cioè *insiemi di pacchetti* che verranno trattati in modo identico durante il loro instradamento. Il set di destinazioni possibili è finito ma ampio, quindi, attraverso una funzione, i pacchetti vengono raggruppati in classi equivalenti. Ad esempio, pacchetti con lo stesso prefisso IP, la stessa destinazione o che richiedono un certo livello di servizio possono appartenere alla stessa FEC. Pacchetti appartenenti alla stessa FEC vengono considerati "uguali" per il processo di forwarding.
2. **Mapping (Associazione FEC-Path)**: Dopo aver suddiviso i pacchetti in classi (FEC), si effettua il mapping tra queste *classi* e il *path* che i pacchetti di quella classe seguiranno attraverso la rete. Inoltre, è possibile che più classi condividano lo stesso percorso, poiché il trattamento che devono ricevere è identico.

![[Label Switching.png|center|600]]

#### Protocollo IP

Il protocollo IP può implementare il modello di **label switching** basandosi *sull'indirizzo di destinazione* del pacchetto, utilizzando tecniche che coinvolgono la **routing table** di ciascun router. Ecco come funziona il processo.

La **Forwarding Equivalent Class (FEC)** può essere vista come l'insieme delle **entries** presenti nella **routing table**. In altre parole, ogni entry della tabella di routing rappresenta una possibile classe di forwarding per un pacchetto con una determinata destinazione. Formalmente, possiamo esprimere questa relazione come:
$$\{FEC_{i} = \text{le entries della routing table}\}$$

Una caratteristica fondamentale del protocollo IP è che *tutti i router nella rete condividono lo stesso insieme di entries*, cioè hanno una visione coerente delle destinazioni di rete, il che garantisce la consistenza delle FEC tra tutti i router.

| prefix     | next hop |
| ---------- | -------- |
| a.b.c.d/24 | C        |

- **$f_{1}$ Longest Prefix Match (LPM)**: Quando un pacchetto arriva a un router, il router esamina la destinazione del pacchetto e cerca il prefisso più lungo nella sua routing table che corrisponde all'indirizzo di destinazione. Questa operazione permette di selezionare la entry di routing più specifica.
- **$f_{2}$: OSPF**: Garantisce che tutti i router abbiano una visione completa e aggiornata della topologia di rete. Questo significa che tutti i router possono *autonomamente* calcolare il percorso più breve verso una destinazione utilizzando l'algoritmo di **Dijkstra**. Inoltre, anche se ci sono diversi next hop corrispondenti allo stesso indirizzo di destinazione, tutti i router calcolano la stessa shortest path grazie alla consistenza della topologia.

Questo approccio, in cui ogni router, sia **edge** che **core**, esegue le operazioni di **LPM** e **OSPF** per ogni pacchetto, rappresenta una soluzione completamente distribuita. Un tempo, l'idea era che questa modalità distribuita fosse la migliore opzione.
Tuttavia, con il tempo, è diventato chiaro che questo approccio **non è scalabile né efficiente** in reti di grandi dimensioni. Il motivo è che richiede che ogni router, indipendentemente dalla sua posizione nella rete, debba eseguire tutte le operazioni di routing in modo indipendente per ogni pacchetto, il che aumenta significativamente la complessità e i tempi di calcolo. Questo è uno dei motivi per cui modelli più centralizzati e ottimizzati, come **MPLS (Multiprotocol Label Switching)**, sono diventati più popolari per ottimizzare il routing e gestire meglio il traffico.

#### MPLS

Con **MPLS (Multiprotocol Label Switching)**, vogliamo basarci sullo stesso modello di instradamento ma in modo più efficiente e scalabile rispetto a IProuting. La differenza chiave sta nel fatto che, in MPLS, gran parte delle decisioni di instradamento e classificazione dei pacchetti viene spostata agli **edge routers**, mentre i **core routers** si limitano a inoltrare i pacchetti basandosi su etichette già assegnate.

- **$f_{1}$ Classificazione**: Avviene tramite una *policy* configurata solo negli edge routers. Questa policy determina quante e quali classi di pacchetti verranno generate. A differenza del routing IP tradizionale, in cui la classificazione è fissa e basata esclusivamente sugli indirizzi di destinazione, MPLS permette una maggiore flessibilità, poiché la policy può essere configurata in modo più ampio. La classificazione può, ad esempio, basarsi non solo sulla destinazione IP, ma anche su parametri come il tipo di servizio o altre caratteristiche del traffico.
- **$f_{2}$: Mapping e Labeling**: Anche il processo di mapping avviene solo sugli edge routers. Gli edge routers calcolano il percorso del pacchetto (come farebbe un router IP usando OSPF o altri algoritmi), e aggiungono un'etichetta al pacchetto. Questa label rappresenta la classe del pacchetto e il percorso che dovrà seguire attraverso la rete. In questo modo i **core routers** non devono ricalcolare la classificazione o il percorso; si limitano a leggere la label associata al pacchetto e a consultare una tabella di forwarding basata sulle label.

Quindi per ogni core router avremo una tabella fatta in questo modo: 

| label | next hop |
| ----- | -------- |
| Lj    | C        |

E vediamo che il numero di entries della tabella sarà pari al numero di path e non al numero di prefissi. In questo modo garantiamo: 
1. Flessibilità: Garantita da $f_{1}$ infatti gli edge routers possono utilizzare politiche specifiche per assegnare le classi in base a una varietà di criteri, come QoS, tipi di traffico o criteri di sicurezza.
2. Scalabilità: I core routers non devono più scalare in base al numero di **prefissi IP**, che può essere estremamente grande in reti complesse. Invece, devono solo scalare in base al numero di **path**, che è significativamente inferiore rispetto ai prefissi IP. Questo riduce la complessità della tabella di forwarding nei core routers e rende la rete molto più scalabile.