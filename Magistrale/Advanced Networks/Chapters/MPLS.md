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

Il protocollo IP può implementare questo modello