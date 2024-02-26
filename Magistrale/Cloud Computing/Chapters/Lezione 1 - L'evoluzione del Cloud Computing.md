Il cloud computing non è stata una rivoluzione brusca ma è il risultato di decadi di anni di ricerca. 
Il primo ricercatore a parlare di quello che sarebbe diventato il cloud computing è stato John McCarthy nel 1960s che scrisse che prima o poi la potenza di calcolo sarebbe diventata un servizio di pubblica utilità, proprio come il servizio di elettricità, acqua o gas, ovvero poco costosa e quindi accessibile a tutti. 
Ci sono voluti 40 anni ma il risultato è stato effettivamente raggiunto da alcune compagnie che forniscono servizi accessibili agli utenti con politica pay-as-use. 
Vediamo l'evoluzione dei calcolatori negli anni.
## Architettura Mainframe

Nell'architettura Mainframe (1970) si aveva un unico grande calcolatore con performance elevate che veniva utilizzato dai dipendenti dell'azienda nel quale era installato. Oltre ad essere molto grande era anche costoso quindi pochissime aziende potevano permettersi di acquistarlo, si era quindi lontani dall'idea di servizio accessibile a tutti. 
Aveva altri difetti quali il fatto che solo un numero definito di utenti alla volta potevano usarlo (time-sharing), nacque quindi la necessità di avere più mainframe a disposizione dell'azienda, cosa difficile a causa del costo elevato.
Inoltre, per interfacciarsi con il mainframe si utilizzavano dei terminali molto semplici (dumb-terminal) che permettevano solo di inserire il proprio input e di vedere il proprio output.
Il risultato erano lunghe code di attesa poiché il mainframe stesso rappresentava un collo di bottiglia. 
## La rivoluzione dei PC

Con lo sviluppo dei microprocessori, si iniziarono a produrre i personal computer (PC) caratterizzati da dimensioni ridotte rispetto ai mainframe ed un costo molto basso, tanto che le aziende iniziarono a comprarne uno per piano e successivamente uno per dipendente. 
Inoltre, non c'era più il bisogno di utilizzare un terminale semplice ma si iniziavano a vedere le prime applicazioni software che i dipendenti potevano facilmente utilizzare. 
I mainframe vennero velocemente dismessi in favore dei PC ma emerse il problema di connetterli tra loro in modo da scambiarsi informazioni da computer a computer. 
## Network di PC

La network communication venne introdotta per trasferire informazioni tra computer, nacquero quindi le reti LAN (Local Area Network) o le WAN (Wide Area Network) che permettevano di connettere i computer delle organizzazioni. 
Col tempo è aumentata sempre di più la banda utilizzabile per scambiare tali informazioni.
All'inizio le comunicazioni tra computer dello stesso network passavano tutte da un singolo nodo in modo da formare un'architettura di tipo client server. In seguito, si capì che quest'ultimo rappresentava il collo di bottiglia della rete e fu così che vennero introdotte le reti peer-to-peer in cui tutti i computer possono comunicare direttamente tra loro senza un'entità che faccia da mediatore. 
## Parallel Processing and Fast Network Communication

Negli anni 80 si iniziò a capire che le performance di un computer non potevano essere aumentate solo dalla produzione di processori più veloci e potenti ma anche tramite la parallelizzazione della processazione. 
In particolare, si iniziò a dividere il task da svolgere in sotto-task per assegnarle ad ogni processore. Infatti, il task completo non poteva essere risolto dal singolo processore, in quanto troppo poco potente, ma dividendolo e utilizzando due o più processori è possibile risolverlo utilizzando un approccio parallelo. 
La tecnologia dell'esecuzione parallela fu seguita dal concetto di esecuzione distribuita, definita come un insieme di processori connessi tra loro tramite un network che possono eseguire un task complesso che sarebbe impossibile da risolvere in un singolo PC. 
Quindi il task complesso viene diviso in sotto-task e assegnate al singolo nodo che avrà la sua memoria e la sua potenza, alla fine della computazione condividerà il risultati tramite il network con gli altri nodi del sistema distribuito. 
I due grandi problemi di questo approccio riguardavano l'inefficienza e l'unico point of failure poiché se ad esempio si guastava un nodo, il task veniva perso e non poteva essere completato. 
## Cluster Computing

Per superare i limiti del distributed processing si decise di dividere i computer del sistema in cluster, ognuno dei quali responsabile di risolvere uno specifico task e connessi tra loro tramite LAN. 
In questo modo, anche se un nodo falliva, gli altri potevano sostituirlo tramite l'introduzione della ridondanza.
Lo svantaggio principale riguardava il fatto che per assegnare i task ai nodi vi era un nodo "speciale" chiamato cluster head. 
Il problema dell'unico point of failure rimaneva a causa della presenza del cluster head ed inoltre quest'ultimo rappresentava il collo di bottiglia del sistema quindi si passò all'approccio grid. 
## Grid Computing

Nell'approccio grid non vi era un unico nodo responsabile dell'assegnazione dei cluster ma bensì erano proprio i nodi del cluster a coordinarsi tra loro per decidere chi dovesse risolvere quel task. 
L'approccio era quindi totalmente decentralizzato e permise di introdurre una funzionalità importante: l'eterogeneità dei sistemi, ovvero sistemi con diverse configurazioni hardware. 
Tuttavia rimanevano degli svantaggi come:
- lo scaling in tempo reale non era possibile 
- il sistema non era tollerante ai guasti perché il fallimento di un nodo rappresentava un fallimento dell'intero task 
- hardware eterogeneo richiedeva del codice diverso per ogni configurazione
## Virtualizzazione

Per il terzo svantaggio si trovò una soluzione negli anni 90 con l'introduzione della virtualizzazione hardware. In particolare, si andava a porre un layer di astrazione tra il software e l'hardware in modo che il codice potesse essere scritto ed eseguito indipendentemente dalla configurazione hardware sottostante in quanto si può simulare un hardware specifico. 
Questo permise anche di superare lo svantaggio dello scaling in tempo reale e della tolleranza ai guasti, vedremo nelle prossime lezioni come. 
## Web Services

Oltre alla virtualizzazione, si sviluppò anche il servizio web con nuovi protocolli di comunicazione come il World Wide Web che permise di scambiarsi informazioni in luoghi geograficamente distanti. Prima infatti si parlava solo di reti LAN o WAN, mentre ora gli utenti in parti diverse del globo potevano connettersi.  

# Service Oriented Architecture 

# Utility Computing

# Autonomic Computing

# Cloud Computing
