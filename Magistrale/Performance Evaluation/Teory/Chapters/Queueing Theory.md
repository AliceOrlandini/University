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
Notiamo che se tutti gli interarrival e service times sono esponenzialmente distribuiti allora $N(t)$ descrive completamente lo stato del processo, per cui, non c'è alcun bisogno di sapere l'ultimo istante di arrivo/partenza nel passato visto che questa distribuzione è **memoryless**. Questo significa che la futura evoluzione del sistema può essere predetta conoscendo solo la $N(t)$.

Facciamo un contro esempio, consideriamo un sistema in cui:
- Gli arrivi sono esponenziali.
- I service times sono costanti.
In questo caso $N(t)$ non fornisce una caratterizzazione completa perché il futuro dipende dal passato. In questo caso è utile fare un diagramma di stato chiamato *transition-rate diagram* oppure **continuous time Markov chain (CTMC)**:

![[state_diagram.webp|center|700]]

Il cerchi rappresentano lo stato del sistema al tempo $t$ e gli archi le transizioni da uno stato all'altro. 
Assumeremo sempre che $\lambda$ e $\mu$ siano *time-indipendent* ovvero che non cambino col tempo, potranno invece essere *state-dipendent* ovvero dipendenti dallo stato del sistema. Per questo motivo è utile usare la notazione $\lambda_{n}$ per indicare l'arrival rate quando il sistema contiene $n$ jobs e $\mu_{n}$ per lo stesso motivo.
LA CTMC diventa così:

![[ctmc.webp|center|700]]

La probabilità di due eventi simultanei è trascurabile, per questo abbiamo un diagramma in cui gli archi raggiungono solo i propri vicini.

Concentriamoci su uno stato $n > 0$ e fissiamo il tempo $t$. 
>[!note] Probabilità di trovare $n$ jobs nel sistema
>Chiamiamo $p_{n}(t)$ la probabilità che ci siano $n$ jobs nel sistema al tempo $t$, ovvero $$p_{n}(t) = P\{N(t) = n\}$$

Possiamo scrivere quindi la **Probability Flow-Balance Equation** (funzione di come la probabilità di trovarsi in un certo stato cambia nel tempo) semplicemente guardando il CTCM:
![[ctmc_equation.webp|center|600]]

$$
\begin{equation}
\begin{cases} \frac{d}{dt}p_{n}(t) = -(\lambda_{n}+\mu_{n})\cdot p_{n}(t) + \mu_{n+1}\cdot p_{n+1}(t) + \lambda_{n-1}\cdot p_{n-1}(t) & n > 0 \\[4pt]
\frac{d}{dt}p_{0}(t) = -\lambda_{0}p_{0}(t)+\mu_{1}\cdot p_{1}(t) & n = 0\end{cases}
\end{equation}
$$
Per ricavarle bisogna fare il seguente ragionamento: probabilità di entrare nello stato $n$meno la probabilità di uscire (quindi flusso entrante meno quello uscente).
In questo modo $\lambda_{n}$ può essere visto come il *transition rate* dallo stato $n$ a quello $n+1$ e $\mu_{n}$ il *transition rate* dallo stato $n$ all'$n-1$.
Queste equazioni si chiamano **Chapman-Kolmogorov's equations**.
Questa tipologia di sistema di equazioni differenziali può essere risolto solo se si specificano le *condizioni iniziali* date da una PMF per lo stato al tempo zero, ovvero $p_{n}(0)$ $\forall n$ tali che $\sum\limits_{n=0}^{+\infty}p_{n}(0) = 1$.
Generalmente le condizioni iniziali sono quelle del sistema inizialmente vuoto: 
- $p_{0}(0) = 1$
- $p_{n}(0) = 0$ per $n > 0$

### Birth-only process

In questo caso particolare si ha $\mu_{n} = 0$ $\forall n$, e nel caso più semplice in cui $\lambda_{n}= \lambda$ le equazioni CK diventano:
$$
\begin{equation}
\begin{cases} \frac{d}{dt}p_{n}(t) = -\lambda\cdot p_{n}(t)+\lambda\cdot p_{n-1}(t) & n > 0 \\[4pt]
\frac{d}{dt}p_{0}(t) = -\lambda\cdot p_{0}(t) & n = 0\end{cases}
\end{equation}
$$
Assumendo le condizioni iniziali del sistema inizialmente vuoto possiamo risolvere l'equazione facilmente, infatti:
- $n=0$: 
	$\frac{d}{dt}p_{0}(t) = -\lambda\cdot p_{0}(t)$ ammette come soluzione $p_{0}(t) = k\cdot e^{-\lambda t}$ in cui $k=1$ per la condizione iniziale, quindi $p_{0}(t) = e^{-\lambda t}$.
- $n=1$: 
	$\frac{d}{dt}p_{1}(t) = -\lambda\cdot p_{1}(t)+\lambda\cdot p_{0}(t) = -\lambda \cdot p_{1}(t) + \lambda\cdot e^{-\lambda t}$ la cui soluzione è $p_{1}(t) = \lambda t \cdot e^{-\lambda t}$.
- $n>1$:
	generalizzando le precedenti computazioni si ottiene $p_{n}(t) = \frac{(\lambda t)^{n}}{n!}\cdot e^{-\lambda t}$.

Questa è una [[Teoria della probabilità#Distribuzione di Poisson|distribuzione di Poisson]] con media $\lambda t$. Questo è il motivo per cui normalmente chiamiamo "Poisson Processes" quei processi che hanno interarrival times esponenziali IID. 
Inoltre, all'aumentare del tempo: $\lim_{t\rightarrow \infty}p_{n}(t) = 0$ $\forall n$ ovvero la probabilità di trovare un job nel sistema sarà zero. (da rivedere il perché)

### Two-state Birth-Death process

Questo modello utilizza una coda con un solo slot quindi se il sistema è nello stato 1 e arriva un nuovo job, quest'ultimo verrà scartato.
Quindi si ha $\lambda_{0}=\lambda$, $\lambda_{1}= 0$ e $\mu_{0}= 0$, $\mu_{1}= \mu$.

![[two-state.webp|center|300]]

Le equazioni di CK sono:
$$
\begin{equation}
\begin{cases} \frac{d}{dt}p_{1}(t) = -\mu_{1}\cdot p_{1}(t) + \lambda_{0}\cdot p_{0}(t) \\[4pt]
\frac{d}{dt}p_{0}(t) = -\lambda_{0}\cdot p_{0}(t) + \mu_{1}\cdot p_{1}(t) \end{cases}
\end{equation}
$$
Sommando le equazioni si ottiene: $$\frac{d}{dt}p_{1}(t) + \frac{d}{dt}p_{2}(t) = 0$$che significa che $p_{0}(t) + p_{1}(t) = \text{costante}$. con $\text{costante} = 1$ $\forall t$ per la *normalizzazione*.
Per risolvere queste equazioni possiamo usare i metodi tradizionali ottenendo:
$$
\begin{equation}
\begin{cases} p_{0}(t) = \frac{\mu}{\mu+\lambda}+\left[p_{0}(0)-\frac{\mu}{\mu+\lambda}\right]\cdot e^{-(\lambda + \mu)t} \\[4pt]
p_{1}(t) = \frac{\lambda}{\mu+\lambda}+\left[p_{1}(0)-\frac{\lambda}{\mu+\lambda}\right]\cdot e^{-(\lambda + \mu)t} \end{cases}
\end{equation}
$$Queste espressioni dipendono dalle condizioni iniziali.
Sappiamo che è sempre vero che $p_{0}(t) + p_{1}(t) = 1$ e se poniamo $t\rightarrow +\infty$ trovaimo:
$$
\begin{equation}
\begin{cases} p_{0} \triangleq \lim_{t\rightarrow +\infty} p_{0}(t) = \frac{\mu}{\lambda + \mu} \\[4pt]
p_{1} \triangleq \lim_{t\rightarrow +\infty} p_{1}(t) = \frac{\lambda}{\lambda + \mu} \end{cases}
\end{equation}
$$e ancora $p_{0}+p_{1}= 1$. 

> [!danger] Stady State Probability
> Chiamiamo $p_{0}$ e $p_{1}$ **Stady State Probabilities** poiché un sistema in questo stato *non dipende più dal tempo*.

Invece, $p_{i}(t)$ viene chiamata **transient probability** che dipende dalle condizioni iniziali.
Le stady state probabilities invece sono indipendenti dalle condizioni iniziali.
Se per esempio, $p_{0}(0) = \frac{\mu}{\mu+\lambda}$ allora il sistema sarà stady state $\forall t$.

Se siamo interessati solo al calcolo delle stady state probabilities allora c'è un modo più veloce di ottenerle che non include la risoluzione di equazioni differenziali. Infatti, nella definizione di stady state si ha che: $$\forall n, \frac{d}{dt}p_{n}(t) = 0$$poiché la probabilità non cambia nel tempo, la sua derivata sarà una costante. Quindi possiamo riscrivere le equazioni di CK in questo modo:
$$
\begin{equation}
\begin{cases} 0 = -\mu\cdot p_{1}+ \lambda\cdot p_{0} \\[4pt]
0 = -\lambda\cdot p_{0}+\mu\cdot p_{1} \end{cases}
\end{equation}
$$che è solo algebrico. Notiamo però che le due equazioni non sono indipendenti quindi possiamo cancellarne una ed applicare la normalizzazione, il sistema diventa quindi:
$$
\begin{equation}
\begin{cases} 0 = -\mu\cdot p_{1}+ \lambda\cdot p_{0} \\[4pt]
p_{0}+p_{1}= 1 \end{cases}
\end{equation}
$$
Le cui soluzioni sono: 
$$
\begin{equation}
\begin{cases} p_{0} = \frac{\mu}{\lambda + \mu} \\[4pt]
p_{1}  = \frac{\lambda}{\lambda + \mu} \end{cases}
\end{equation}
$$come avevamo trovato precedentemente. 

## Steady State analisi per sistemi Birth-Death

