# Introduzione alla Queueing Theory

> [!note] Queueing Theory
La teoria delle code è una tecnica analitica usata per modellare sistemi e ottenerne misurazioni in termini di performance. 

Tutta la teoria si basa sull'osservazione che generalmente abbiamo un'entità che è in grado di svolgere un job alla volta, e i restanti vengono memorizzati in una coda d'attesa, da cui il nome queueing theory. 
La coda è fondamentale per aumentare l'utilizzazione del sistema, infatti, se quest'ultima non ci fosse allora il sistema scarterebbe i jobs quando il server è impegnato e poi, quando ha finito di servire il job, dovrebbe aspettare per l'arrivo di uno nuovo. Se invece utilizziamo una coda, il server inizierà ad elaborare il nuovo job appena finisce di elaborare il precedente. Il prezzo da pagare per questo è il *delay* per il singolo job che deve aspettare in coda. 

Un tipico esempio è quello del router con velocità di elaborazione pari a $C$ bits/s.
Assumiamo che il pacchetto $0$ arrivi nella coda, essendo il primo verrà subito trasmesso, supponiamo che questo istante sia $t$. Sappiamo che il pacchetto lascerà il router all'istante $t + \frac{L_{0}}{C}$ quindi $\frac{L_{0}}{C}$ sarà il **service time**. 
Il pacchetto $4$ invece:
- inizierà la trasmissione a: $t_{4}^{S}=t+\frac{\sum\limits_{i=0}^{3}L_{i}}{C}$
- finirà la trasmissione a: $t_{4}^{D}=t_{4}^{S}+\frac{\sum\limits_{i=0}^{4}L_{i}}{C}$
Assumendo che questo pacchetto sia arrivato al tempo $t_{4}^{A}$ (con $t_{4}^{A}<t_{4}^{S}<t_{4}^{D}$) allora $t_{4}^{S}-t_{4}^{A}$ sarà il **waiting time** ed infine, $t_{4}^{D}-t_{4}^{A}$ sarà il **response time**.

La risposta alla maggior parte delle domande che possiamo porci su questo sistema si basa sulla definizione del *workload*, questo è dato da:
- **interarrival time** dei pacchetti, che sarà una  [[Teoria della probabilità#Variabili Aleatorie |variabile aleatoria]].
- **service demand**: il tempo in cui il server è occupato, nell'esempio precedente è dato dalla lunghezza dei pacchetti diviso per la velocità di trasmissione $\frac{L_{i}}{C}$ (service time).

Se un sistema è composto da più server allora si definisce *bottleneck* il server con l'utilizzazione più alta. **L'utilizzazione** è definita come la percentuale di tempo in cui il server è occupato. In questa tipologia di sistemi, il server con utilizzazione più alta è colui da sostituire per primo. 

La potenza delle queueing theory risiede nel fatto che si possono ottenere **Average Performance Metrics**, come ad esempio il valor medio dei response times oppure il valor medio dei numero di jobs nella coda, e molto spesso in *forma chiusa*, risolvendo semplici calcoli algebrici.

Questa teoria ha anche delle limitazioni, una di queste è il rischio di *over-semplificazione*. Tuttavia, se il modello è corretto, si ottengono delle predizioni piuttosto accurate in termini di throughput mentre per il response times molto di meno perché è un valore dipendente dal carico di lavoro.

# Analisi dei nodi singolarmente

## Caratterizzazione dello stato della coda

Iniziamo con un esempio: singola coda + server. Dobbiamo caratterizzare lo stato del sistema ad un certo istante $t$, il modo di fare ciò dipende da ciò che vogliamo osservare. Ad esempio potremmo voler osservare il numero di jobs nel sistema all'istante $t$ (il cosiddetto **backlog**). 
![[single queue.webp|center|600]]
![[single_queue_time.webp|center|600]]

$N(t)$ è una quantità discreta che è funzione di un parametro continuo, il tempo.
Dall'immagine si nota che le traiettorie di questa funzione dipendono dall'*interarrival time* dei jobs nella coda. Queste traiettorie saranno **random (or stochastic) processes**.
Iniziamo a studiarne uno che ha:
- L'interarrival times tra i vari jobs [[Teoria della probabilità#Distribuzione esponenziale|esponenziali]] IID con un rate $\lambda$.
- Il service demands anch'esse esponenziali IID con rate $\lambda$.
- La coda infinita con politica FCFS.
Siamo interessati a calcolare la distribuzione del numero di job nel sistema.
Notiamo che se tutti gli interarrival e service times sono esponenzialmente distribuiti allora $N(t)$ descrive completamente lo stato del processo, per cui, non c'è alcun bisogno di 