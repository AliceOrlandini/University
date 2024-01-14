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

![[single queue.webp|center|400]]

![[single_queue_time.webp|center|400]]

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

![[state_diagram.webp|center|500]]

Il cerchi rappresentano lo stato del sistema al tempo $t$ e gli archi le transizioni da uno stato all'altro. 
Assumeremo sempre che $\lambda$ e $\mu$ siano *time-indipendent* ovvero che non cambino col tempo, potranno invece essere *state-dipendent* ovvero dipendenti dallo stato del sistema. Per questo motivo è utile usare la notazione $\lambda_{n}$ per indicare l'arrival rate quando il sistema contiene $n$ jobs e $\mu_{n}$ per lo stesso motivo.
LA CTMC diventa così:

![[ctmc.webp|center|500]]

La probabilità di due eventi simultanei è trascurabile, per questo abbiamo un diagramma in cui gli archi raggiungono solo i propri vicini.

Concentriamoci su uno stato $n > 0$ e fissiamo il tempo $t$. 
>[!note] Probabilità di trovare $n$ jobs nel sistema
>Chiamiamo $p_{n}(t)$ la probabilità che ci siano $n$ jobs nel sistema al tempo $t$, ovvero $$p_{n}(t) = P\{N(t) = n\}$$

Possiamo scrivere quindi la **Probability Flow-Balance Equation** (funzione di come la probabilità di trovarsi in un certo stato cambia nel tempo) semplicemente guardando il CTCM:
![[ctmc_equation.webp|center|500]]

$$
\begin{equation}
\begin{cases} \frac{d}{dt}p_{n}(t) = -(\lambda_{n}+\mu_{n})\cdot p_{n}(t) + \mu_{n+1}\cdot p_{n+1}(t) + \lambda_{n-1}\cdot p_{n-1}(t) & n > 0 \\[4pt]
\frac{d}{dt}p_{0}(t) = -\lambda_{0}p_{0}(t)+\mu_{1}\cdot p_{1}(t) & n = 0\end{cases}
\end{equation}
$$
Per ricavarle bisogna fare il seguente ragionamento: probabilità di entrare nello stato $n$ meno la probabilità di uscire (quindi flusso entrante meno quello uscente). Questo metodo di ricavare le equazioni è chiamato **Global Equilibrium Equations**.
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

![[two-state.webp|center|200]]

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
$$
Queste espressioni dipendono dalle condizioni iniziali.
Sappiamo che è sempre vero che $p_{0}(t) + p_{1}(t) = 1$ e se poniamo $t\rightarrow +\infty$ trovaimo:
$$
\begin{equation}
\begin{cases} p_{0} \triangleq \lim_{t\rightarrow +\infty} p_{0}(t) = \frac{\mu}{\lambda + \mu} \\[4pt]
p_{1} \triangleq \lim_{t\rightarrow +\infty} p_{1}(t) = \frac{\lambda}{\lambda + \mu} \end{cases}
\end{equation}
$$
e ancora $p_{0}+p_{1}= 1$. 

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
$$
che è solo algebrico. Notiamo però che le due equazioni non sono indipendenti quindi possiamo cancellarne una ed applicare la normalizzazione, il sistema diventa quindi:
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
$$
come avevamo trovato precedentemente. 

## Steady State analisi per sistemi Birth-Death

Dal precedente esempio evinciamo che possiamo calcolare le steady-state probabilities nei sistemi birth-death in due modi:
1. Il metodo complesso, che consiste nel formulare le equazioni differenzali, risolverle e infine calcolare il limite.
2. Il metodo semplice, che consiste nel porre $\frac{d}{dt}p_{n}(t) = 0$ $\forall n$ nelle equazioni CK e risolvere un sistema algebrico per ottenere $p_{n}$.
Notiamo che il sistema del [[#Birth-only process|primo esempio]] *non ammette uno steady state*. Infatti, nel suo caso $p_{n}=\lim_{t\rightarrow \infty}p_{n}(t) = 0$ $\forall n$ che ha senso perché la probabilità di raggiungere lo stato $n-esimo$ in questo tipo di sistema è zero visto che il server non invia nulla.
Dobbiamo ricordare che il metodo semplice può essere applicato **solo a sistemi che raggiungono lo steady state** per cui questa è una condizione che deve essere *testata a posteriori*. 
L'interpretazione fisica di "raggiungere lo steady state" è la seguente: 

![[ctmc_equation.webp|center|500]]

il flusso di probabilità attraverso una superficie tratteggiata (che corrisponde alla derivata presente nel lato destro delle equazioni CK) è *nulla*. Quindi, **il flusso entrante e il flusso uscente devono bilanciarsi a vicenda**:
$$
\begin{equation}
\begin{cases} (\lambda_{n}+\mu_{n})\cdot p_{n} = \lambda_{n-1}\cdot p_{n-1}+\mu_{n+1}\cdot p_{n+1} & n > 0\\[4pt]
\lambda_{0}\cdot p_{0}=\mu_{1}\cdot p_{1} \end{cases}
\end{equation}
$$
che significa che calcolare le SS probabilities in un sistema birth-death è semplice:
1. disegnare il CTMC (che è la parte difficile)
2. formulare le equazioni allo SS
3. aggiungere la normalizzazione: $\sum\limits_{n=0}^{+\infty}p_{n}= 1$
Il sistema ottenuto ammette una singola soluzione che può essere calcolata partendo da $n = 0$ e calcolando le probabilità in funzione di $p_{0}$:
- $n=0$: dall'equazione otteniamo $p_{1}= \frac{\lambda_{0}}{\mu_{1}}\cdot p_{0}$.
- $n=1$: nell'equazione scritta sopra sostituiamo il precedente risultato e troviamo $p_{2}=\frac{\lambda_{0}\cdot \lambda_{1}}{\mu_{1}\cdot \mu_{2}}\cdot p_{0}$
- $n>1$: generalizzando il risultato sarà: 
$$p_{n}= \prod_{i=0}^{n-1} \frac{\lambda_{i}}{\mu_{i+1}}\cdot p_{0}$$
e la normalizzazione può essere scritta nel seguente modo (utile per semplicemente sostituire): 
$$p_{0}\left[1+\sum\limits_{n=1}^{+\infty}\left(\prod_{i=1}^{n-1}\frac{\lambda_{i}}{\mu_{i+1}}\right)\right] = 1$$

> [!note] Stability Condition
> Condizione **necessaria** e **sufficiente** per un sistema di ammettere uno **steady state** (e quindi di essere stabile) è che sia *finita* la somma: $$\sum\limits_{n=1}^{+\infty}\left(\prod_{i=1}^{n-1}\frac{\lambda_{i}}{\mu_{i+1}}\right)$$

Se la somma non è finita otterremo $p_{n}=0$ $\forall n$ perché $p_{0}\cdot \infty = 1 \Rightarrow p_{n}= 0$ $\forall n$.
Si noti che il problema della stabilità esiste sono nei sistemi con un numero infinito di stati, infatti nei sistemi con un numero finito di stati avrà un numero finito di termini della sommatoria e quindi non potrà mai divergere. 

Abbiamo visto che un modo per trovare le equazioni è il [[#Caratterizzazione dello stato della coda|Global Equilibrium Equations]], esiste un secondo modo che consiste nello scegliere un perimetro arbitrario attraverso il quale imporre l'equilibrio di flusso e spesso porta a calcoli più semplici.
Uno di questi metodi è chiamato **Local Equilibrium Equations** e consiste nel definire il perimetro in modo che includa tutti gli stati da zero a $n$ incluso, ad esempio in questo caso avremo:

![[local_equilibrium.webp|center|500]]

$$
\begin{equation}
\begin{cases}
\lambda_{0}\cdot p_{0}= \mu_{1}\cdot p_{1}\\
\lambda_{1}\cdot p_{1}= \mu_{2}\cdot p_{2}\\
...\\
\lambda_{n}\cdot p_{n}= \mu_{n+1}\cdot p_{n+1}
\end{cases}
\end{equation}
$$
Da cui si ottiene $p_{n}= \prod_{i=0}^{n-1} \frac{\lambda_{i}}{\mu_{i+1}}\cdot p_{0}$ in modo molto più veloce rispetto a prima.
Vedremo più avanti che le global equations sono sempre facile da scrivere, mentre le local sono facili solo quando la CTMC è semplice.

## Sistemi M/M/1 

Studiamo nel dettaglio il sistema birth-death che viene chiamato M/M/1.
Quest'ultima notazione si chiama di **Kendall** e consiste in almeno 3 indicatori:
1. La **distribuzione dei tempi** di arrivo: 
	- M per memoryless
	- D per deterministico (costante)
	- E per Erlang
	- G per generico
2. La **distribuzione dei service times**, possono apparire le stesse lettere di prima.
3. Il **numero di server**, in questo caso uno.
Ci possono essere più indicatori oltre a questi come la **capacità del sistema** o la **popolazione** da cui provengono gli arrivi. Se non sono indicati significa che sono pari a $\infty$.

Assumiamo di avere $\lambda_{n}= \lambda$ e $\mu_{n}= \mu$, ovvero che i rate di arrivo e di partenza sono costanti, e che la coda sia infinita. In questo caso, la relazione si semplifica: 
$$p_{n}= \prod_{i=0}^{n-1} \frac{\lambda_{i}}{\mu_{i+1}}\cdot p_{0}= \left(\frac{\lambda}{\mu}\right)^{n}\cdot p_{0}$$
Chiamiamo $\rho = \frac{\lambda}{\mu}$ **l'utilizzazione** del sistema, avremo:
- $\lambda > \mu \Rightarrow \rho > 1 \Rightarrow$ sistema instabile, questo sistema si chiama **transient**.
- $\lambda < \mu \Rightarrow \rho < 1 \Rightarrow$ sistema forse stabile, questo sistema si chiama **positive recurrent**.
- $\lambda = \mu \Rightarrow \rho = 1 \Rightarrow$ sistema instabile, questo sistema si chiama **null recurrent**.

La normalizzazione utilizzando l'utilizzazione diventa: 
$$p_{0}\left[\sum\limits_{n=0}^{+\infty}\rho^{n}\right]= 1$$
da cui si evince la condizione di *stabilità* che è $\rho < 1$ poiché in questo modo siamo certi che la sommatoria converge a: 
$$\frac{1}{1-\rho}$$
e quindi $p_{0}=(1-\rho)$ e $p_{n}= (1-\rho)\cdot \rho^{n}$.
Quest'ultima formula giustifica il motivo per cui $\rho$ viene chiamato utilizzazione, infatti la probabilità di trovarsi nello stato zero è la probabilità che il server non sia pieno ovvero $(1-\rho)$, quindi $\rho$ è il **valor medio di jobs nel server** oppure la frazione di tempo in cui il server è occupato.

Nei sistemi positive recurrent la distribuzione nel numero di jobs nel sistema allo *steady state* è una [[Teoria della probabilità#Distribuzione Geometrica|distribuzione geometrica]]: $p_{n}=(1-\rho)\cdot \rho^{n}$ con una probabilità di successo di $p = 1-\rho$. Possiamo quindi ricavare immediatamente il valor medio e la varianza del numero di job nel sistema, che è una variabile aleatoria che verrà indicata con $N$:
- $E[N] = \sum\limits_{n=0}^{+\infty}n\cdot p_{n}= \frac{\rho}{1-\rho}$
- $Var(N) = \frac{\rho}{(1-\rho)^{2}}$

È particolarmente interessante sapere come varia il comportamento di $E[N]$ in funzione di $\rho$. Questa funzione è chiamata **Funzione di Kleinrock** e la sua forma è rappresentata in figura:

![[kleinrock_function.webp|center|300]]

Essa ha un comportamento *piatto* fino a $\rho = 0.5$ e poi mostra un *ginocchio* come un asintoto verticale per $\rho \rightarrow 1$. Quindi questo sistema:
- in condizioni di carico basso, il valor medio è sotto l'1, e quindi il sistema è o vuoto o c'è un singolo lavoro che viene immediatamente servito.
- se $\rho$ cresce sopra $0.5$ allora i jobs iniziano a sperimentare un po' di coda fino a *saturare*. 
Quindi nel dimensionare il nostro sistema dobbiamo fare in modo di rimanere a sinistra del ginocchio.

### Esercizio

Dobbiamo dimensionare la dimensione di un network. 
Il carico di lavoro consiste in pacchetti la cui lunghezza è *esponenzialmente distribuita* con valor medio $\frac{1}{\gamma}$. Gli interarrival times sono anch'essi esponenziali con valor medio $\frac{1}{\lambda}$. 
Calcolare la velocità di invio dei pacchetti $C$ tale che:
- la media dei pacchetti arretrati è pari a $B$ pacchetti.
- il $95^{th}$ percentile dei pacchetti arretrati è di $\Pi$ pacchetti.

Osserviamo che questo è un sistema M/M/1 con arrival rate $\lambda$ perché nel testo è scritto che gli interarrival times hanno media $\frac{1}{\lambda}$.
Ci ricaviamo il valor medio del service time, indicato con la variabile aleatoria $t_{s}$, che sarà pari al valor medio della lunghezza dei pacchetti fratto la velocità di invio (tempo = lunghezza/velocità): 
$$E[t_{s}] = \frac{E[lenght]}{C} = \frac{1}{\gamma \cdot C} = \frac{1}{\mu}$$
quindi otteniamo $\mu = \gamma \cdot C$ da cui $\rho = \frac{\lambda}{\mu}= \frac{\lambda}{\gamma \cdot C}$.
Alla prima domanda possiamo rispondere osservando che la media dei pacchetti arretrati è pari a: 
$$B = E[N] = \frac{\rho}{1-\rho} = \frac{\frac{\lambda}{\gamma\cdot C}}{1-\frac{\lambda}{\gamma\cdot C}}$$
e risolvendo in funzione di $C$ si ottiene: 
$$C = \frac{\lambda\cdot (1+B)}{\gamma\cdot B}$$

#Attenzione questo vale solo se il sistema ammette lo steady state! Ovvero se $\rho = \frac{\lambda}{\gamma \cdot C} < 1$.
Alla seconda domanda possiamo rispondere risolvendo la seguente equazione: 
$$P\{N \le \Pi\} = 0.95$$
Sappiamo che:
$$
\begin{align*}
P\{N \le x\} &= \sum\limits_{n=0}^{x}p_{n} =\\[4pt]
&= \sum\limits_{n=0}^{x}(1-\rho)\cdot \rho^{n} =\\[4pt]
&= (1-\rho)\cdot \frac{1-\rho^{x+1}}{1-\rho} =\\[4pt]
&= 1- \rho^{x+1}
\end{align*}
$$
e quindi l'equazione che dobbiamo risolvere è: 
$$1-\rho^{\Pi+1}=0.95$$
e svolgendo i calcoli otteniamo: 
$$C^{\Pi+1}=20\cdot\left(\frac{\lambda}{\gamma}\right)^{\Pi+1} \Rightarrow C = \sqrt[(\Pi+1)]{20} \cdot \frac{\lambda}{\gamma}$$
### Mean Performance Indexes

I più importanti indici di prestazione nella queueing theory sono:
1. Il **valor medio nei jobs nel sistema** $E[N]$ che abbiamo già trovato essere pari a $\frac{\rho}{1-\rho}$.
2. Il **valor medio dei jobs nella coda** $E[N_{q}]$ (quindi non includendo il job servito).
3. Il **valor medio del response time** $E[R]$, ovvero il tempo che intercorre tra il momento di arrivo di un job ed il relativo momento di partenza.
4. Il **valor medio del waiting time** $E[W]$, chiamato anche queueing time, che è il tempo che intercorre tra il momento di arrivo del job nella coda e quello in cui il server inizia a servire quel pacchetto (questo è utile perché la queueing theory è fatta anche per le persone a cui interessa più questo indice piuttosto che il response time).
5. Il **throughput** $\gamma$, ovvero il numero di jobs serviti per unità di tempo.

Visto che il primo lo abbiamo già calcolato, passiamo subito al **valor medio di jobs nella coda**. Esso assume i seguenti valori:
- $0$ con probabilità $p_{0}+p_{1}$
- $1$ con probabilità $p_{2}$
- $k \ge 1$ con probabilità $p_{k+1}$

Avendo chiarito questo è abbastanza facile calcolare il valor medio: 
$$
\begin{align*}
E[N_{q}] &= \sum\limits_{k=1}^{+\infty}k\cdot p_{k+1} =\\[4pt]
&= \sum\limits_{k=1}^{+\infty}(k-1)\cdot p_{k}=\\[4pt]
&= \sum\limits_{k=1}^{+\infty}k\cdot p_{k}-\sum\limits_{k=1}^{+\infty}p_{k} =\\[4pt]
&= E[N]-(1-p_{0}) = \\[4pt]
&= E[N] - \rho
\end{align*}
$$
**Questo risultato è sempre vero nei sistemi con un unico server**.
La formula poteva essere ottenuta più facilmente sfruttando la proprietà additiva del valor medio, quindi togliendo al valor medio di job nel sistema il valor medio di job nel server che è pari all'utilizzazione.

Per quanto concerne il **response time**, esso può essere calcolato utilizzando la legge di Little:
> [!note] Little's Law
> Consideriamo un sistema in uno *steady state*, tale che *nessun lavoro venga creato o distrutto* all'interno del sistema e sia $\lambda$  il suo tasso medio di arrivo dei job, allora la **Little's Law** sostiene che il tempo medio di risposta è: 
> $$E[R] = \frac{E[N]}{\overline{\lambda}}$$

Le uniche ipotesi da considerare sono che il sistema sia FCFS e che il sistema non crei né distrugga jobs. In questo caso, il tasso di arrivo $\overline{\lambda}$ è anche il **tasso medio di partenza** (*average departure rate*).
Notare che la Little's Law si applica ai valor medi e non alle distribuzioni.

Nel sistema M/M/1 avrò: 
$$\overline{\lambda} = \sum\limits_{n=0}^{+\infty}\lambda_{n}\cdot p_{n} = \lambda \sum\limits_{n=0}^{+\infty}p_{n} = \lambda$$
visto che $\sum\limits_{n=0}^{+\infty}p_{n} = 1$ essendo la condizione di *normalizzazione*.
Di conseguenza sarà: 
$$E[R] = \frac{E[N]}{\lambda}= \frac{\rho}{1-\rho}\cdot \frac{1}{\lambda}= \frac{1}{\mu-\lambda}$$

![[response_time.webp|center|300]]

In questo caso, quando il carico è piccolo $p \ll 1$ allora il response time tende a $\frac{1}{\mu}$ che è di fatto il service time medio $E[t_{s}]$, il che ha perfettamente senso perché se un job non incontra coda allora l'unica attesa che sperimenterà sarà quella dovuta al tempo di elaborazione nel server. 
Quando $\rho$ aumenta si inizia a formare la coda fino alla saturazione del sistema.

Passiamo ora al **waiting time**, anch'esso può essere calcolato sfruttando la Little's Law applicata ad una coda in equilibrio. In questo caso, l'arrival e il departure rate sono pari a $\lambda$ per cui: 
$$E[W] = \frac{E[N_{q}]}{\lambda}= \frac{E[N]-\rho}{\lambda} = E[R] - \frac{1}{\mu}$$
che poteva essere calcolato anche sfruttando l'additività del valor medio: 
$$E[W] = E[R]-E[t_{s}] = E[E] - \frac{1}{\mu}$$
Infine, il **throughput** $\gamma$. Per questo indicatore operiamo in modo intuitivo osservando che:
- Se $\gamma > \overline{\lambda}$ allora significa che ci sono jobs che escono dal sistema senza che siano entrati, ciò non è possibile perché il sistema non crea jobs internamente.
- Se $\gamma < \overline{\lambda}$ allora ci saranno jobs che rimarranno nella coda per un tempo indefinito, anche questo caso non è possibile essendo il sistema FCFS e stabile. 
Quindi l'unica possibilità è che sia: 
$$\gamma = \overline{\lambda}$$
L'unica possibilità in cui $\gamma < \lambda$ esiste quando il sistema ha *memoria finita*.
In ogni caso, la definizione formale di throughtput è la seguente: 
$$\gamma = \sum\limits_{n=1}^{+\infty}\mu_{n}\cdot p_{n}$$
Nel caso del sistema M/M/1 avremo $\gamma = \lambda$.

### Metodo alternativo per calcolare i mean performance indexes

Esiste un metodo per il calcolo dei performance indexes che non richiede il calcolo delle SS probabilities. Questo metodo è *generale*, ovvero non legato ad un sistema in particolare, ma verrà spiegato per un M/M/1 per semplicità.
Consideriamo le equazioni globali allo steady state:
$$
\begin{equation}
\begin{cases}
\lambda\cdot p_{0}=\mu\cdot p_{1}  \\
(\lambda+\mu)\cdot p_{n} = \lambda\cdot p_{n-1}+\mu\cdot p_{n+1} & n > 1
\end{cases}
\end{equation}
$$
La tecnica consiste nel moltiplicare entrambe le equazioni per $z^{n}$, con $z \in \mathcal{C}, |z|< 1$, e poi sommando tutto: 
$$
\begin{equation}
\begin{cases}
\lambda\cdot p_{0}\cdot z^{0}=\mu\cdot p_{1}\cdot z^{0}  \\
(\lambda+\mu)\cdot p_{n}\cdot z^{n} = \lambda\cdot p_{n-1}\cdot z^{n}+\mu\cdot p_{n+1}\cdot z^{n} & n > 1
\end{cases}
\end{equation}
$$
sommando otteniamo: 
$$\lambda\cdot p_{0}\cdot z^{0} + \sum\limits_{n=1}^{+\infty}(\lambda+\mu)\cdot p_{n}\cdot z^{n} = \mu\cdot p_{1}\cdot z^{0}+ \sum\limits_{n=1}^{+\infty}\lambda\cdot p_{n-1}\cdot z^{n}+ \sum\limits_{n=1}^{+\infty} \mu\cdot p_{n+1}\cdot z^{n}$$
Ricordiamo che la definizione di [[Teoria della probabilità#Probability Generating Functions PGF|PGF]] di una variabile aleatoria discreta non-negativa è $P(z) \triangleq E[z^{N}] = \sum\limits_{k=0}^{+\infty}z^{k}\cdot p_{k}$ con $p_{k}$ la [[Teoria della probabilità#Probability Mass Function|PMF]].
Lo stato del sistema $N$ è  anch'essa una variabile aleatoria discreta non negativa. 
Manipolando la precedente espressione otteniamo (passaggi sulle dispense):
$$
P(z) = \frac{p_{0}}{1-\rho\cdot z}
$$
che dipende da $p_{0}$ che può essere ricavato applicando la normalizzazione. Dalla definizione di PGF otteniamo che $P(1) \triangleq E[1^{N}] = \sum\limits_{k=0}^{+\infty}1^{k}\cdot p_{k}= 1$ da cui ricaviamo che $p_{0}= 1-\rho$ e quindi, sostituendo nella formula: 
$$
P(z) = \frac{1-\rho}{1-\rho\cdot z}
$$
Questa espressione può essere anti-trasformata ottenendo: 
$$p_{k}= (1-\rho)\cdot p^{k}$$
che avevamo già trovato. 
Comunque, una volta ottenuta la $P(z)$ possiamo trovare i performance indexes senza anti-trasformare ma semplicemente applicando le proprietà della PGF:
- **Valor Medio di jobs nel sistema**: 
$$E[N] = \frac{d}{dz}P(z)|_{z=1} = \frac{\rho\cdot (1-\rho)}{(1-\rho\cdot z)^{2}}|_{z=1}= \frac{\rho}{1-\rho}$$
- **Valor Medio di jobs nella coda**: 
$$E[N^{2}] = \left[\frac{d^{2}}{dz^{2}}P(z) + \frac{d}{dz}P(z)\right]_{z=1}$$
Inoltre, se necessario è possibile calcolare le SS probabilities a partire dalla PGF:
- $p_{0}= \lim_{z \rightarrow 0}P(z)$
- $p_{1}=P^{'}(0)$
- $p_{k}= \frac{P^{(k)}(0)}{k!}$

### Arrival time and Random-observer probabilities

Le SS probabilities che abbiamo calcolato sono quelle che avrebbe calcolato un **random observer**. Quindi, se un osservatore guarda il sistema in un **random time** (allo steady state) allora $p_{n}$ sarebbe la probabilità di trovare $n$ jobs nel sistema.
Esiste però un'altra probabilità, che è quella vista da un job in arrivo, chiamata **arrival-time probability** che viene indicata con $r_{n}$.
In generale queste due probabilità sono diverse. 
Per capire questo concetto consideriamo un sistema con interarrival times costanti e pari a 2 secondi ed un service time costante pari a 1 secondo (D/D/1). Questo sistema è allo steady state essendo deterministico.
La traiettoria $N(t)$ ha la seguente forma:

![[arrival_time.webp|center|400]]

Un osservatore casuale vedrebbe:
- 1 job nel sistema per metà del tempo
- 0 job nel sistema l'altra metà di tempo
Di conseguenza, per lui $p_{0}= p_{1}= \frac{1}{2}$.
Tuttavia, un job in arrivo troverà *sempre* il sistema vuoto per cui sarà $r_{0}= 1$ ed $r_{j}= 0$ $\forall j > 0$.
Quindi in generale si ha $r_{n} \not= p_{n}$.

In un sistema birth-death con interarrival times distribuiti esponenzialmente, le arrival-time probabilities possono essere calcolate utilizzando il [[Teoria della probabilità#Teorema di Bayes|teorema di Bayes]], come segue. Definiamo:
$$r_{n}(t) = \lim_{\Delta t\rightarrow 0}P\{N(t) = n | A(t,t+\Delta t)\}$$
dove $A(t,t+\Delta t)$ significa che è arrivato un job nell'intervallo $[t,t+\Delta t)$.
Svolgendo il limite otteniamo: (calcoli sulle dispense)
$$r_{n}(t) = \frac{\lambda_{n}\cdot p_{n}(t)}{\sum\limits_{k=0}^{+\infty}\lambda_{k}\cdot p_{k}(t)}$$
che vale $\forall t$ e quindi anche allo steady state.
Allo steady state avremo:
$$r_{n}= \lim_{t\rightarrow +\infty} r_{n}(t) = \frac{\lambda_{n}\cdot p_{n}(t)}{\sum\limits_{k=0}^{+\infty}\lambda_{k}\cdot p_{k}(t)} = \frac{\lambda_{n}}{\overline{\lambda}}\cdot p_{n}$$
dove $\overline{\lambda}$ è il *mean arrival rate*.
Quando $\lambda_{n}= \lambda$, e solo in questo caso, abbiamo $r_{n}= p_{n}$.
> [!note] PASTA
> Un sistema in cui $r_{n}= p_{n}$ ha la proprietà **PASTA** (Poisson Arrivals See Time Average).

### Distribuzione del response e del waiting time

Abbiamo già calcolato il *valor medio* del response e del waiting time $E[R]$ e $E[W]$ tramite la *Little's Law*. Ora vediamo come calcolare la **distribuzione** del response e del waiting time $F_{R}(x)$ e $F_{W}(x)$.

Iniziamo dal response time. Supponiamo che un job arrivi nel sistema al tempo $t$. Siano $n$ il numero di job *già* nel sistema al tempo $t^{-}$. Sappiamo che allo steady state la probabilità di avere $n$ job nel sistema nel momento in cui arriva un nuovo job è $r_{n}$. Il tempo che il nuovo job spenderà nel sistema è composto da:
- il **service time residuo** di quel job che sta venendo servito.
- la **somma dei service time** di tutti gli altri $n$ job, incluso il nuovo arrivato.

![[response_distribution.webp|center|300]]

Visto che i service time sono *esponenziali* e quindi *memoryless*, il residual service time ha la stessa distribuzione del service time:
$$P\{t_{s}>x+y|t_{s}> x\} = P\{t_{s}>y\}$$
Perciò, il response time è la *somma dei service time di $n+1$ jobs* **se** ci sono $n$ jobs nel sistema al momento dell'arrivo.
Questa distribuzione ottenuta tramite la somma di esponenziali IID è chiamata **Distribuzione di Erlang**, la cui CDF allo stato $n$ è:
$$F_{n}(t) = 1-\sum\limits_{k=0}^{n-1}e^{-\mu t} \frac{(\mu t)^{k}}{k!}$$
e la PDF è: 
$$f_{n}(t) = e^{-\mu t}\cdot \mu \cdot \frac{(\mu t)^{n-1}}{(n-1)!}$$

![Erlang|center|300](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d3/Erlang_dist_pdf.svg/1200px-Erlang_dist_pdf.svg.png)


Per $n = 1$ è un'esponenziale, mentre quando $n>1$ inizia a mostrare un *picco*. Per $n$ grande si comporta come una distribuzione normale.

Possiamo calcolare $E[S_{n}]$ e $Var(S_{n})$ sfruttando la proprietà di additività e l'indipendenza. 
Perciò, otteniamo:
- $E[S_{n}] = \frac{n}{\mu}$
- $Var(S_{n}) = \frac{n}{\mu}^{2}$
- $CoV(S_{n}) = \frac{\sqrt{Var(S_{n})}}{E[S_{n}]} = \frac{1}{\sqrt{n}}$ utile per l'operazione di *fitting*.

Per riassumere, abbiamo trovato che la somma di $n+1$ esponenziali IID con rate $\mu$ è una Distribuzione di Erlag $(n+1)$-stage con PDF:
$$
f_{n+1}(t) = e^{-\mu x}\cdot \mu \cdot \frac{(\mu x)^{n}}{(n)!}
$$
Questa PDF di una variabile aleatoria $R$ in cui abbiamo $n$ job nella coda.
Possiamo quindi calcolare $f_{R}(x)$ utilizzando [[Teoria della probabilità#Legge della probabilità totale|legge della probabilità totale]]:
$$
\begin{align*}
f_{R}(x) &= \sum\limits_{n=0}^{+\infty}f_{n+1}(x) \cdot r_{n} =\\[4pt]
&= \sum\limits_{n=0}^{+\infty} \mu \cdot e^{-\mu x}\cdot \frac{(\mu x)^{n}}{n!}\cdot (1-\rho)\cdot \rho^{n} =\\[4pt]
&= \mu \cdot (1-\rho)\cdot e^{-\mu x} \cdot \sum\limits_{n=0}^{+\infty} \frac{(\mu \cdot x \cdot \rho)^{n}}{n!} =\\[4pt]
&= \mu \cdot (1-\rho) \cdot e^{-\mu x} \cdot e^{\mu \rho x} = \\[4pt]
&= \mu \cdot (1-\rho) \cdot e^{-\mu(1-\rho)x} =\\[4pt]
&= (\mu -\lambda)\cdot e^{-(\mu-\lambda)x} =\\[4pt]
&= \frac{1}{E[R]} \cdot e^{-x/E[R]}
\end{align*}
$$
Che è una distribuzione esponenziale, quindi:
$$F_{R}(x) = 1-e^{-x/E[R]} = 1-e^{-(\mu-\lambda)x}$$

Per quanto riguarda il **waiting time** $W$ possiamo ripetere gli stessi passaggi del response time ma con una cura in più: c'è la possibilità che il sistema sia *vuoto* al momento dell'arrivo del job e quindi che il waiting time sia null, cosa che avviene con probabilità $r_{0}=1-\rho$. Quindi, la PDF del waiting time sarà:
- **Zero** con una probabilità non nulla $r_{0}= 1-\rho$.
- Pari alla distribuzione di Erlang $S_{n}$ con probabilità $r_{n}$ con $n \ge 1$.

Si noti che $F_{W}(0) = P\{W = 0\} = r_{0}>0$ che implica che c'è *un punto di discontinuità* in corrispondenza di $F_{W}(0)$. Di conseguenza, la PDF avrà una **Delta di Dirac** in zero. Possiamo usare ancora la Probabilità Totale per calcolare la $f_{W}(x)$:
$$
\begin{align*}
f_{W}(x) &= (1-\rho)\cdot \delta(x) + \sum\limits_{n=1}^{+\infty}f_{n}(x)\cdot r_{n} =\\[4pt]
&= (1-\rho)\cdot \delta(x) + \sum\limits_{n=1}^{+\infty} \mu \cdot e^{-\mu x} \cdot \frac{(\mu x)^{n-1}}{(n-1)!} \cdot (1-\rho)\cdot \rho^{n} =\\[4pt]
&= (1-\rho)\cdot \delta(x) + \mu e^{-\mu x}\cdot (1-\rho)\cdot \rho \cdot \sum\limits_{n=1}^{+\infty} \frac{(\mu x)^{n-1}}{(n-1)!} \cdot \rho^{n-1} =\\[4pt]
&= (1-\rho)\cdot \delta(x) + \rho \cdot \mu \cdot (1-\rho) \cdot e^{-\mu(1-\rho)x} = \\[4pt]
&= (1-\rho)\cdot \delta(x) + \rho \cdot \frac{1}{E[R]} \cdot e^{-x/E[R]}
\end{align*}
$$

![[waiting_distribution.webp|center|400]]

Dall'espressione precedente possiamo ricavarci la PDF:
$$
\begin{equation}
F_{W}(x) = \begin{cases}
1-\rho & x = 0 \\
(1-\rho)+\rho\cdot (1-e^{-x/E[R]}) & x > 0
\end{cases}
\end{equation}
$$
che si riduce a $F_{W}(x) = 1-\rho\cdot e^{-x/E[R]}$ per $x \ge 0$.

### Esercizio

Il tuo capo ti comunica che il rate di richieste al sito web della compagnia si duplicherà il prossimo mese. Vuole quindi aumentare la capacità del sistema in modo tale che:
- il response time medio rimanga lo stesso.
- il $99^{th}$ percentile del response time rimanga lo stesso.

Assumendo che il server possa essere modellato come un sistema M/M/1, ci basta incrementare il service rate al fine di accontentare i requisiti. Chiamiamo $\lambda$, $\mu$ gli arrival e service raet *correnti* e $\lambda'=2\lambda$ e $\mu'$ i *nuovi*. 
Per il primo requisito abbiamo:
$$
E[R'] = E[R] \Rightarrow \frac{1}{\mu'-2\lambda} = \frac{1}{\mu-\lambda} \Rightarrow \mu' = \mu + \lambda
$$
È quindi sufficiente incrementare il service rate di un fattore $\lambda$ e non raddoppiarlo come saremmo portati a pensare con l'intuito (anzi se raddoppiassi il service rate, il response time diminuirebbe).
Per il secondo requisito dobbiamo considerare la PDF:
$$F_{R}(x) = 1-e^{-x/E[R]}= 1-e^{-(\mu-\lambda)x}$$
e che il $99^{th}$ percentile del response time *corrente* è la soluzione $x_{.01}$ di:
$$F_{R}(x_{.01}) = 1-e^{-(\mu-\lambda)x_{.01}} = 0.99$$
che risolvendo è:
$$x_{.01}= \frac{log(100)}{\mu-\lambda}$$
Nella nuova configurazione il $99^{th}$ percentile del response time sarà:
$$x_{.01}'= \frac{log(100)}{\mu'-2\lambda}$$
che ancora una volta significa che $\mu' = \mu + \lambda$.

## Sistemi M/M/C 

Finora abbiamo discusso dei sistemi con un singolo server. Un caso frequente è quello in cui una coda venga **servita da più di un server**. 
Se i $C$ server sono equivalenti, cioè hanno lo stesso service rate $\mu$, e tuto il resto rimane uguale agli esempi precedenti (arrivi esponenziali, service times esponenziali, memoria infinita) allora il sistema si chiama **M/M/C one**.
In questo caso, un nuovo job viene gestito in uno dei server liberi, se non c'è nessun server libero, viene messo in attesa nella coda. 

Vogliamo ricavarci le SS probabilities, per fare ciò iniziamo col caso $C = 2$ e poi generalizziamo.
Assumiamo che al tempo $t$, la traiettoria assuma valore $N(t) = n$ con $n \ge 2$ ovvero che entrambi i server siano occupati. La traiettoria tornerà giù quando il più piccolo dei due residual service time si esaurisce. 

![[mmc_trajectory.png|center|400]]

Sappiamo che entrambi i server sono esponenziali IID con rate $\mu$ per cui:
- i service time *residui* sono anch'essi esponenziali IID con rate $\mu$.
- il *minimo* dei due esponenziali IID è un'esponenziale con rate $2\mu$.
Questo significa che gli archi che vanno verso sinistra dopo lo stato $n \ge 2$ avranno transizioni con rate pari a $2\mu$. Per quanto riguarda l'arco allo stato 1, esso avrà rate $\mu$ perché c'è un solo server attivo (possiamo vederlo come un M/M/1 in questo caso).
Per cui, lo schema CTMC sarà:

![[mmc_ctmc.webp|center|400]]

con $\lambda_{n}=\lambda$ ovvero il sistema è PASTA e 
$$
\begin{equation}
\mu_{n} = \begin{cases}
\mu & n = 1\\
2\mu & n > 1
\end{cases}
\end{equation}
$$
In questo caso i service rates si dicono **load-dependent**.
Dato il precedente schema, scriviamo le equazioni globali e locali allo steady state:
- Globali:
$$
\begin{equation}
\begin{cases}
\lambda \cdot p_{0}= \mu \cdot p_{1} \\[4pt]
(\lambda + \mu)\cdot p_{1}= \lambda \cdot p_{0}+2\mu \cdot p_{2} \\[4pt]
(\lambda + 2\mu)\cdot p_{n} = \lambda \cdot p_{n-1}+2\lambda\cdot p_{n+1} & n \ge 2
\end{cases}
\end{equation}
$$
- Locali:
$$
\begin{equation}
\begin{cases}
\lambda \cdot p_{0}=\mu \cdot p_{1} \\[4pt]
\lambda \cdot p_{n}=2\mu \cdot p_{n+1} & n \ge 2
\end{cases}
\end{equation}
$$
Prima di risolvere questi sistemi generalizziamo le espressioni per un numero arbitrario di server $C$, tenendo conto del fatto che:
1. Gli arrival rates sono costanti
2. I service rates sono:
$$
\begin{equation}
\mu_{n} = \begin{cases}
n\cdot \mu & n \le C \\[4pt]
C \cdot \mu & n \ge C
\end{cases}
= min(C,n)\cdot \mu
\end{equation}
$$

![[mmc_ctmc2.webp|center|500]]

Le formule generalizzate per un sistema birth-death con transazioni tra vicini sono:
$$
\begin{equation}
\begin{cases}
p_{n}=\prod_{i=0}^{n-1} \frac{\lambda_{i}}{\mu_{i+1}}\cdot p_{0}& n\ge 1 \\[4pt]
\sum\limits_{n=0}^{+\infty}p_{n}= 1
\end{cases}
\end{equation}
$$
Definiamo:
$$u = \frac{\lambda}{\mu}$$
che *non* è l'utilizzazione e distinguiamo due casi:
- $n \le C$:
$$p_{n}= \frac{u^{n}}{n!}\cdot p_{0}$$
- $n \ge C$:
$$p_{n}= \frac{u^{n}}{C^{n-C}\cdot C!}\cdot p_{0}$$
Per verificare la stabilità del sistema bisogna applicare la normalizzazione che sostituendo diventa:
$$
p_{0}\cdot \left[\sum\limits_{n=C}^{C-1} \frac{u^{n}}{n!}+\sum\limits_{n=C}^{+\infty} \frac{u^{n}}{C^{n-C}\cdot C!}\right] = 1
$$
La prima sommatoria include un numero finito di termini quindi non può divergere, la seconda invece deve essere verificata:
$$
\sum\limits_{n=C}^{+\infty} \frac{u^{n}}{C^{n-C}\cdot C!} = \frac{u^{C}}{C!}\sum\limits_{n=C}^{+\infty} \frac{u^{n-C}}{C^{n-C}}= \sum\limits_{j=0}^{+\infty}\left(\frac{u}{C}\right)^{j} 
$$
per cui:
- se $u < C \Rightarrow \lambda < C\cdot \mu$ il sistema è stabile, che potevamo aspettarci considerando che $\lambda$ è il rate di arrivo e $C\cdot \mu$ è il service rate quando il carico è pesante.
- se $u \ge C$ il sistema è instabile.
La **condizione di stabilità** sarà: $$\rho = \frac{\lambda}{C\cdot\mu} < 1$$
Da questa condizione si evince che cosa avviene vicino allo stato zero non influenza la *stabilità*, ma solo le *performance*. 
Sotto la precedente condizione, la sommatoria converge a $\frac{1}{1-\rho}$ quindi:
$$
p_{0}= \frac{1}{\sum\limits_{k=0}^{C-1} \frac{u^{k}}{k!}+ \frac{u^{C}}{C!}\cdot \frac{1}{1-\rho}}
$$
e:
$$
\begin{equation}
p_{n} = \begin{cases}
p_{0}\cdot \frac{u^{n}}{n!} & n \le C\\[4pt]
p_{0}\cdot \frac{u^{n}}{C^{n-C}\cdot C!} & n \ge C
\end{cases}
\end{equation}
$$
Ora che sappiamo il valore delle SS probabilities possiamo calcolare i performance indexes:
- **Valor medio di jobs presenti in coda**:
$$E[N_{q}] = \frac{u^{C}}{C!}\cdot p_{0}\cdot \frac{\rho}{(1-\rho)^{2}}$$
- **Valor medio del waiting time**:
$$
E[W] = \frac{E[N_{q}]}{\overline{\lambda}} = \frac{u^{C}}{C!}\cdot p_{0}\cdot \frac{\frac{1}{C\cdot \mu}}{(1-\rho)^{2}}
$$
- **Valor medio del response time**:
$$E[R] = E[W]+E[t_{s}] = \frac{u^{C}}{C!}\cdot p_{0}\cdot \frac{\frac{1}{C\cdot \mu}}{(1-\rho)^{2}} + \frac{1}{\mu}$$
- **Valor medio di jobs nel sistema**:
$$E[N] = E[N_{q}]+C\cdot \rho$$
- **Valor medio di jobs serviti**:
$$E[S] = C\cdot \rho$$
- **Throughput**: considerando che l'M/M/C è PASTA abbiamo che $r_{n}= p_{n}$ e quindi:
$$
\gamma \triangleq \sum\limits_{n=1}^{+\infty}\mu_{n}\cdot p_{n}= \sum\limits_{n=1}^{C}n\cdot \mu \cdot p_{n}+ \sum\limits_{n=C+1}^{+\infty}C\cdot \mu \cdot p_{n} = \lambda
$$
i calcoli non sono semplici, ma da un'osservazione fisica si evince che l'unica possibilità è che sia $\gamma = \lambda$ visto che il sistema è allo steady state e ciò che entra deve uscire.
- **Valor medio di server occupati**:
$$
E[c] = \sum\limits_{n=1}^{+\infty}min(n,C)\cdot p_{n}= \sum\limits_{n=1}^{C}n\cdot p_{n}+ \sum\limits_{n=C+1}^{+\infty}C\cdot p_{n}
$$
ancora una volta i calcoli non sono semplici, ma possiamo trovare una strada più veloce sfruttando la Little's Law applicata a un sosttosistema compost da $C$ server e osservando che il response time medio del sistema è $E[t_{s}] = \frac{1}{\mu}$ e il throughput è $\gamma = \lambda$. Quindi:
$$E[c] = \lambda \cdot E[t_{s}] = \frac{\lambda}{\mu}= u$$
	Questo giustifica il fatto che $\rho = \frac{\lambda}{C\cdot\mu} = \frac{E[c]}{C}$ rappresenta *l'utilizzazione* che il valor medio di server occupati fratto il numero totale di server.
	Questa definizione vale anche per sistemi M/M/1.

Le SS probabilities possono essere riscritte in funzione di $\rho$ invece che $\mu$ in questo modo:
$$
\begin{equation}
p_{n} = \begin{cases}
p_{0}\cdot \frac{(C\cdot \rho)^{n}}{n!} & n \le C\\[4pt]
p_{0}\cdot \frac{C^{C}\cdot \rho^{n}}{C!} & n \ge C
\end{cases}
\end{equation}
$$
### Delay centers: sistemi M/M/$\infty$ 

Se consideriamo il kimite $C \rightarrow +\infty$ osserviamo un comportamento particolare: visto che il numero di server è infinito, ogni job che arriva avrà un server disponibile, quindi non ci sarà coda. Lo schema CTMC è il seguente:

![[mminf.webp|center|500]]

Le equazioni locali all'equilibrio sono:
$$
p_{n+1}= \frac{\lambda}{(n+1)\cdot\mu}\cdot p_{n}
$$
Le SS probabilities possono essere scritte facilmente derivandole dalla formula generale:
$$
\begin{equation}
\begin{cases}
p_{n}= \prod_{i=0}^{n-1} \frac{\lambda_{i}}{\mu_{i+1}}\cdot p_{0} & n \ge 1 \\[4pt]
\sum\limits_{n=0}^{+\infty}p_{n} = 1
\end{cases}
\end{equation}
$$
con $\lambda_{n}= \lambda$ e $\mu_{n} = n \cdot \mu$, ottenendo:
$$
\begin{equation}
\begin{cases}
p_{n}= \left(\frac{\lambda}{\mu}\right)^{n}\cdot \frac{1}{n!}\cdot p_{0} & n \ge 0 \\[4pt]
p_{0} \cdot \sum\limits_{n=0}^{+\infty} \left(\frac{\lambda}{\mu}\right)^{n}\cdot \frac{1}{n!} = 1
\end{cases}
\end{equation}
$$
Tuttavia, visto che per Taylor $\sum\limits_{n=0}^{+\infty} \left(\frac{\lambda}{\mu}\right)^{n}\cdot \frac{1}{n!} = e^\frac{\lambda}{\mu}$, allora *il sistema è sempre stabile* perché $p_{0}\cdot e^\frac{\lambda}{\mu}= 1$ indipendentemente dai valori di $\lambda$ e $\mu$.
Questo significa che: 
$$p_{n}= e^{-\frac{\lambda}{\mu}}\cdot \frac{(\frac{\lambda}{\mu})^{n}}{n!}$$
per ogni $n \ge 0$, ovvero **le SS probabilities sono una distribuzione di [[Teoria della probabilità#Distribuzione di Poisson|Poisson]]**.
I sistemi M/M/$\infty$ sono detti **delay centers** perché quando arriva un job viene *ritardato* di un tempo casuale esponenziale prima di venir inoltrato.
Da questo tipo di sistema è possibile calcolare solo alcune performance metric sfruttando la distribuzione di Poisson tra cui:
$$E[N] = E[C] = \frac{\lambda}{\mu}$$
$$E[R] = \frac{1}{\mu}$$
### Models, CTMCs e performance indexes

Modelli molto diversi tra loro potrebbero ammettere le stesse SS probabilities ma è importante osservare che queste similitudini non si estendono ai performance indexes quindi bisogna prestare particolare attenzione quando andiamo a svolgere gli esercizi.
Esermpio: da finire

## Discouraged arrivals

In questo caso, consideriamo un sistema tale che, nel caso in cui nella coda si trovi più di un job, molti meno job arriveranno. (stile Lucca Comics che col ca..o che mi faccio 4 ore di coda per vedere lo stand di Netflix).
Un modello usato in questi casi è quello in cui:
$$
\begin{equation}
\begin{cases}
\lambda_{n}= \frac{\lambda}{n+1} \\[4pt]
\mu_{n}= \mu
\end{cases}
\end{equation}
$$
Lo schema CTMC sarà:

![[discouraged_arrivals.webp|center|500]]

Le cui equazioni locali saranno:
$$
p_{n+1}= \frac{\lambda}{(n+1)\mu}\cdot p_{n}
$$
che sono le stesse nel cado del modello [[Queueing Theory#Delay centers sistemi M/M/$ infty$|delay center]].
Quindi possiamo velocemente dedurre che:
1. il sistema è *sempre stabile*, indipendentemente dai valori di $\lambda$ e $\mu$.
2. $p_{n}= e^{-\frac{\lambda}{\mu}}\cdot \frac{(\lambda/\mu)^{n}}{n!}$ per ogni $n \ge 0$.
3. $E[N] = \frac{\lambda}{\mu}$
Alcune differenze tra i due sistemi sono:
- Si verifica coda e infatti avremo: 
$$E[N_{q}] = E[N]-\rho = E[N]-(1-p_{0})=\frac{\lambda}{\mu}-(1-e^{- \frac{\lambda}{\mu}})$$
- L'arrival rate medio è diverso, quindi tutto ciò che si è calcolato con Little sarà diverso.
- Questo sistema non è PASTA perché gli arrival rate non sono costanti, quindi avremo $r_{n}\not = p_{n}$.
Questo significa che dovremo calcolare l'arrival rate medio definito come :
$$\overline{\lambda} = \sum\limits_{n=0}^{+\infty}\lambda_{n}\cdot p_{n}$$
Tuttavia, sappiamo che dovrà sicuramente essere:
$$\overline{\lambda} = \gamma \triangleq \sum\limits_{n=1}^{+\infty}\mu_{n}\cdot p_{n}$$
ma visto che $\mu_{n}= \mu$ otteniamo:
$$\overline{\lambda} = \mu \cdot (1-p_{0}) = \mu \cdot (1-e^{\frac{-\lambda}{\mu}})$$
con un numero notevolmente inferiore di calcoli.  Per cui otteniamo:
1. **Valor medio del response time**:
$$
E[R] = \frac{E[N]}{\overline{\lambda}} = \frac{\lambda}{\mu^{2}\cdot \left(1-e^{\frac{-\lambda}{\mu}}\right)}
$$
2. **Valor medio del waiting time**:
$$
E[W] = \frac{E[N_{q}]}{\overline{\lambda}}=E[R]-E[t_{s}] = \frac{\lambda}{\mu^{2}\cdot \left(1-e^{\frac{-\lambda}{\mu}}\right)} - \frac{1}{\mu}
$$
3. **Arrival-Time probability**: 
$$
r_{n}= \frac{\lambda_{n}}{\overline{\lambda}}\cdot p_{n}= \frac{p_{n+1}}{1-p_{0}}
$$

## Sistemi con memoria finita: M/M/1/K

Nei sistemi reali abbiamo code di lunghezza finita quindi si hanno perdite dovuto all'overflow. Questo significa che, nel caso in cui $\gamma < \lambda$, cioè il throughput è minore dell'interarrival rate, alcuni pacchetti non entrino nemmeno nel sistema.
Indichiamo con $K$ la memoria del sistema, cioè il massimo numero di jobs che sono ammessi nel sistema in ogni istante. Quando il sistema è nello stato $K$ significa che è *pieno* quindi ogni nuovo arrivo verrà rigettato. 
Se l'arrial e il service rate sono costanti, lo schema CTMC sarà:

![[mmk.png|center|600]]

Da cui possiamo ricavare le equazioni locali all'equilibrio:
$$
\lambda \cdot p_{n}=\mu \cdot p_{n+1}
$$
con $0 \le n < K$. Da cui si ricava:
$$
p_{n}= \left(\frac{\lambda}{\mu}\right)^{n}\cdot p_{0}
$$
La condizione di normalizzazione ora coinvolge una somma finita quindi il sistema è sempre **positive recurrent** che sia $\lambda < \mu$ o meno.
Chiamiamo $u = \frac{\lambda}{\mu}$, sostituendo otteniamo:
$$
\begin{equation}
\begin{cases}
p_{n}= u^{n}\cdot p_{0} & 0 \le n \le K \\[4pt]
p_{0}\cdot \sum\limits_{n=0}^{K} u^{n}= 1
\end{cases}
\end{equation}
$$
Tuttavia:
$$
\begin{equation}
\sum\limits_{n=0}^{K}u^{n} = \begin{cases}
\frac{1-u^{K+1}}{1-u} & u \not = 1 \\[4pt]
K+1 & u = 1
\end{cases}
\end{equation}
$$
per cui:
$$
\begin{equation}
p_{0}= \frac{1}{\sum\limits_{n=0}^{K}u^{n}} = \begin{cases}
\frac{1-u}{1-u^{K+1}} & u \not = 1 \\[4pt]
\frac{1}{K+1} & u = 1
\end{cases}
\end{equation}
$$
e:
$$
\begin{equation}
p_{n}= \begin{cases}
\frac{1-u}{1-u^{K+1}}\cdot u^{n} & u \not = 1 \\[4pt]
\frac{1}{K+1} & u = 1
\end{cases}
\end{equation}
$$
Mi raccomando all'esame non dimenticare la seconda condizione, si perde punti inutilmente e già lo scritto basta per perdere punti...

Se $u < 1$ ci aspetteremo che gli stati "bassi" si verifichino con più probabilità di quelli "alti" e questo è vero perché $p_{n}$ è una serie decrescente. Se invece $u > 1$ allora $p_{n}$ è una sequenza crescente.
Calcoliamo i performance indexes:
1. **Valor medio dei jobs presenti nel sistema**:
$$E[N] = \frac{u}{1-u}- \frac{(K+1)\cdot u^{K+1}}{1-u^{K+1}}$$
	se assumiamo che $u < 1$, questa formula è simile a quella per il sistema M/M/1 ma con un effetto negativo dovuto alla limitatezza della coda che gli impedisce di avere infiniti stati come avviene per la M/M/1. Se invece avessimo $u > 1$ allora la formula diventa:
$$E[N] \approx K - \frac{1}{1-u}$$
	quindi il numero di job nel sistema sarà vicino a $K$ in qualsiasi caso perché le transizioni negli archi che vanno verso destra si verificheranno molto più rapidamente delle transizioni opposte.
2. **Valor medio di jobs nella coda**:
$$E[N_{q}] = E[N]-(1-p_{0}) = \frac{u}{1-u}- \frac{K \cdot u^{K+1}+u}{1-u^{K+1}}$$
3. Un'importante performance metric dei sistemi con coda finita è la **Blocking Probability** detta anche **Loss Probability** cioè la probabilità che un job in arriva venga rigettato. Questa è la probabilità che il job trovi il sistema nello stato $K$. Questo sistema è per metà PASTA: 

![[mmk_pasta.webp|center|300]]
	Quindi avremo: 
$$p_{L} =p_{K}= \frac{1-u}{1-u^{K+1}}\cdot u^{K}$$
	Questa relazione è importante perché conoscendo la relazione tra $p_{L}$, $K$ e $u$ è possibile dimensionare opportunamente la coda badandoci sull'aspettativa sulla los probability dati gli arrival e service rates.

  4. Per calcolare il response e waiting time tramite la Little's Law bisogna stare attenti a considerare solo quei job che effettivamente entrano nel sistema. Per questi ultimi $\lambda_{n}$ non è costante ma è:
$$
\begin{equation}
\lambda_{n}= \begin{cases}
\lambda & 0 \le n < K \\[4pt]
0 & n = K
\end{cases}
\end{equation}
$$
Quindi alla destra del bidone, il sistema non è PASTA ma ha:
$$
\overline{\lambda} = \sum\limits_{n=0}^{K}\lambda_{n}\cdot p_{n}=\lambda \cdot (1-p_{K}) < \lambda
$$
Possiamo quindi calcolare il **valor medio del response time**:
$$
E[R] = \frac{E[N]}{\overline{\lambda}} = \frac{E[N]}{\lambda \cdot (1-p_{K})}
$$
e il **valor medio del waiting time**:
$$
E[W] = \frac{E[N_{q}]}{\overline{\lambda}} = \frac{E[N_{q}]}{\lambda \cdot (1-p_{K})}
$$
4. **L'Arrival-Time probability** sarà:
$$
\begin{equation}
r_{n}= \frac{\lambda_{n}}{\overline{\lambda}}\cdot p_{n}=
\begin{cases}
\frac{p_{n}}{1-p_{L}}& n<K \\
0 & n=K
\end{cases}
\end{equation}
$$
5. Infine, il **Throughput** sarà:
$$
\gamma = \overline{\lambda} = \lambda \cdot (1-p_{L})
$$
### Aggiungere spazio alla coda aumenta l'utilizzazione

Sappiamo che il motivo per cui si utilizzano le code è quello di *aumentare l'utilizzazione del sistema*. Calcoliamo quindi l'utilizzazione del sistema M/M/1/K come funzione di $K$. Avremo:
$$
p(K) = 1-p_{0}(K) = 1- \frac{1-u}{1-u^{K+1}} = u \cdot \frac{1-u^{K}}{1-u^{K+1}}
$$
Quando $u < 1$, la precedente espressione aumenta con $K$ e:
$$p(\infty) = \lim_{k\rightarrow +\infty} p(K) = \frac{\lambda}{\mu}= u$$
Quindi aggiungere spazio alla coda aumenta l'utilizzazione del sistema. Inoltre, aumentare la coda aumenta anche il **throughput**:
$$
\gamma(K) = \mu \cdot (1-p_{0}(K)) = \mu \cdot p(K)
$$
e anche in questo caso:
$$
\gamma(\infty) = \lim_{K \rightarrow +\infty}\gamma(K) = \lambda
$$
Tuttavia, non è tutto oro ciò che luccica perché aumentare la coda aumenta anche il **Response Time**, calcoliamolo in funzione di $K$:
$$
E[R] = \frac{E[N]}{\gamma} = \frac{u}{\lambda\cdot(1-u)}- \frac{K \cdot u^{K+1}}{\lambda \cdot (1-u^{K})}
$$
e facendo il limite otteniamo:
$$
\lim_{K \rightarrow +\infty} = \frac{u}{\lambda\cdot(1-u)}= \frac{1}{\mu-\lambda}
$$
## Sistemi con popolazione finita: M/M/1/$*$/U

Succede spesso di avere sistemi che modellano un servizio per una popolazione finita di utenti. 
Assumiamo che gli utenti siano **indipendenti** e che il tempo che essi spendono *fuori* dal sistema sia [[Teoria della probabilità#Distribuzione esponenziale|esponenzialmente distribuito]] con un rate $\lambda$. Segue che se $U$ utenti sono la popolazione e che $n$ utenti sono all'interno del sistema, allora avrò $U-n$ utenti fuori dal sistema e forse entreranno in futuro.
Il prossimo utente ad entrare nel sistema sarà quello con il *tempo esterno residuo minimo*.
Visto che i tempi sono esponenziali IID:
- il tempo esterno residuo minimo è distribuito (è memoryless).
- il minimo degli $U-n$ utenti è anch'esso esponenziale IID con rate $(U-n) \cdot \lambda$.
Quindi, gli arrival rate saranno:
$$\lambda_{n}= (U-n)\cdot \lambda$$
mentre i service times sono costanti $\mu_{n}= \mu$.
Il sistema avrà $U+1$ stati:

![[mmu.webp|center|400]]

ed è sempre stabile perché ha un numero finito di stati. Le equazioni all'equilibrio sono:
- **Globali**:
$$
\begin{equation}
\begin{cases}
U\cdot \lambda \cdot p_{0}= \mu \cdot p_{1} \\[4pt]
[(U-n)\cdot \lambda +\mu]\cdot p_{n}=\mu \cdot p_{n+1}+(U-(n-1))\cdot \lambda \cdot p_{n-1} & 1 \le n < U \\[4pt]
\mu \cdot p_{U}= \lambda \cdot p_{U-1}
\end{cases}
\end{equation}
$$
- **Locali**:
$$
(U-n)\cdot \lambda \cdot p_{n}=\mu \cdot p_{n+1}
$$
Dalle equazioni locali è facile ricavare:
$$
p_{n}= \left(\frac{\lambda}{\mu}\right)^{n} \cdot \frac{U!}{(U-n)!}\cdot p_{0}
$$
per $0 \le n \le U$.
Come detto, il sistema è sempre stabile quindi dalla normalizzazione possiamo ricavare:
$$
p_{0} = \frac{1}{\sum\limits_{n=0}^{U} \left(\frac{\lambda}{\mu}\right)^{n} \cdot \frac{U!}{(U-n)!}}
$$
Vediamo per quanto riguarda i performance indexes:
- **Valor medio di jobs nel sistema**:
$$
E[N] = U- \frac{\mu}{\lambda}\cdot (1-p_{0})
$$
- **Valor medio di jobs nella coda**:
$$
E[N_{q}] = E[N] - (1-p_{0}) = U - \frac{\mu + \lambda}{\lambda} \cdot (1-p_{0})
$$
- Tramite la Little's Law possiamo calcolare:
$$
\overline{\lambda} = \gamma = \mu \cdot (1-p_{0})
$$
- **Valor medio del response time**:
$$
E[R] = \frac{U}{\mu\cdot(1-p_{0})}- \frac{1}{\lambda}
$$
- **Valor medio del waiting time**:
$$
E[W] = E[R] = \frac{U}{\mu\cdot(1-p_{0})}- \frac{1}{\lambda} - \frac{1}{\mu}
$$
- **Arrival-Time probability**:
$$
r_{n}= \frac{p_{n+1}}{1-p_{0}}
$$
con $n < U$
## Sistemi con bulk arrivals

Fino ad ora abbiamo studiato sistemi con transizioni solo tra vicini, tuttavia ci possono essere casi in cui troviamo transizioni non tra vicini. In tutti questi casi, assumiamo che gli arrivi siano esponenziali e manteniamo la proprietà che l'unico parametro rilevante per descrivere lo stato del sistema siano il numero di job. In questo caso però, un arrivo **aumenterà il numero di job in coda di più di un'unità**, per cui avremo delle transizioni non tra vicini nello schema CTMC.

Per rappresentare il CTMC chiamiamo $g$ la variabile aleatoria del numero di job per arrivo e sia $g_{k} = P\{g = k\}$ la sua PMF. Avremo sicuramente che $g_{0}= 0$ e che $\sum\limits_{k=1}^{+\infty}g_{k}= 1$.
Assumiamo che $g_{k}$ sia indipendente dall'interarrival time e service time (cosa che in realtà non avviene perché di solito più un camion ci mette ad arrivare più sarà carico quindi non sono indipendenti).
Quindi il rate di un arco che va dallo stato $i$ allo stato $k$ sarà pari a $\lambda\cdot g_{k-i}$, e la CTMC può essere rappresentata nel seguente modo:

![[bulk_arrivals.webp|center|400]]

Questo diagramma è difficile da seguire in quanto:
- tutti gli stati avranno tanti archi uscenti verso destra quanto il dominio di $g$, ognuno dei quali con rate $\lambda \cdot g_{j}$ con $j \ge 1$.
- Ogni stato $n$ avrà esattamente $n$ archi entranti da sinistra con rate $\lambda \cdot g_{n-j}$ con $0\le j < n$ proveniente dallo stato $j$.
- Invece, tutte le transizioni dei sercice avranno rate $\mu$.
Possiamo scrivere le equazioni globali:
- $n = 0$:
$$\sum\limits_{i=1}^{+\infty}\lambda \cdot g_{i}\cdot p_{0}=\mu \cdot p_{1} \Rightarrow \lambda \cdot p_{0}=\mu \cdot p_{1}$$
- $n \ge 1$:
$$
\left[\sum\limits_{i=1}^{+\infty}\lambda\cdot g_{i}+\mu\right]\cdot p_{n}=\mu \cdot p_{n+1}+\sum\limits_{i=0}^{n-1}\lambda\cdot g_{n-i}\cdot p_{i}
$$
E sviluppando otteniamo:
$$
(\lambda+\mu)\cdot p_{n}=\mu \cdot p_{n+1}+\lambda \cdot \sum\limits_{i=0}^{n-1}g_{n-i}\cdot p_{i}
$$
Il calcolo delle SS probabilities usando il metodo diretto è difficoltoso in questo caso quindi si utilizza la PGF moltiplicando ogni equazione per $z^{n}$ e sommando tutto (calcoli omessi):
$$
P(z) = \frac{\mu \cdot p_{0}\cdot (1-z)}{\mu \cdot (1-z )-\lambda \cdot z \cdot [1-G(z)]}
$$
Per calcolare $p_{0}$ si utilizza la condizione di normalizzazione che è $P(1) = 1$ che in questo caso è $P(1) = \frac{0}{0}$ che è indeterminata. Per risolverla utilizziamo De L'Hopital:
$$
\lim_{z=1^{-}} P(z) = \frac{-\mu\cdot p_{0}}{-\mu-\lambda \cdot [1-G(z)]+\lambda\cdot z \cdot G'(z)}|_{z=1} = \frac{-\mu \cdot p_{0}}{-\mu +\lambda \cdot G'(1)} = \frac{p_{0}}{1-(\frac{\lambda}{\mu})\cdot E[g]} = 1
$$
Da cui si ricava:
$$
p_{0}= 1-\left(\frac{\lambda}{\mu}\right)\cdot E[g]
$$
Essendo $p_{0}$ una probabilità sarà non negativa, ciò significa che $\left(\frac{\lambda}{\mu}\right)\cdot E[g] < 1$ che è la **condizione di stabilità**. Infatti **l'utilizzazione** sarà pari a $\rho = (1-p_{0}) = \left(\frac{\lambda}{\mu}\right)\cdot E[g]$.
E infine, sostituendo nell'espressione otteniamo:
$$
P(z) = \frac{\mu\cdot (1-\rho)\cdot (1-z)}{\mu \cdot (1-z)-\lambda\cdot z \cdot [1-G(z)]}
$$

La stessa procedura può essere utilizzata per sistemi con **bulk services**.

## Sistemi con distribuzione del service time non esponenziale

Fino ad ora abbiamo assunto interarrival times e service times siano esponenziali in modo da sfruttare la proprietà memoryless. 
Tuttavia, ci sono casi in cui l'interarrival e il service times non possono essere modellati come una distribuzione esponenziale. In questi casi, l'analisi si fa complessa per il fatto he il numero di job nel sistema non può essere indicatore dello stato del sistema stesso.
Nonostante questo, alcuni risultati si possono trovare, per esempio nel sistema M/G/1 (ha interarrival esponenziali, *service times generale* e un server). Con la parola *generale* intendiamo che si può utilizzare qualsiasi tipo di distribuzione. 
Sia $t_{S}$ la variabile aleatoria che modella i service times e assumiamo che $E[t_{S}] = \frac{1}{\mu}$ e $Var(t_{S})$ sia noto. 
Se $\rho = \frac{\lambda}{\mu}< 1$ allora il sistema è stabile e la formula di **Pollaczek and Khinchin (PK)** sostiene che:
$$
E[N] = \rho + \frac{\rho^{2}+\lambda^{2}\cdot Var(t_{S})}{2\cdot (1-\rho)} = \rho + \frac{\rho^{2}\cdot [1+CoV(t_{S})^{2}]}{2\cdot (1-\rho)}
$$
Da notare che:
- Noto $E[N]$ possiamo calcolare gli altri tre performance indexes.
- Quando $t_{S}$ è esponenziale la formula PK si trasforma nella formula di **Kleinrock** valida per i sistemi M/M/1.
La versione della formula PK con la covarianza ha un importante significato perché mette in evidenza il fatto che il numero di job nel sistema dipende anche da *quanto il service time è variabile*. Infatti, se il service time è deterministico allora avremo $Var(t_{S}) = CoV(t_{S}) = 0$ e avremo il minor numero di job possibili nel sistema.
Questa formula mette quindi in evidenza come la **variabilità** del service time **aumenta** l'occupazione della coda. Quindi bisogna porre particolare attenzione alla scelta del service time del nostro sistema.

![[non-exp service.webp|center|400]]

La formula PK ha anche un significato fisico: **la coda si origina dalla variabilità (o randomness)**. Nei sistemi dove sia interarrival che service times sono costanti non ci sarà coda finché si avrà $\rho \le 1$. Se invece entrambi i tempi sono variabili ci sarà coda.
Se gli interarrival times sono stocastici, ci saranno job che arriveranno molto ravvicinati tra loro e genereranno code. 
Se i service times sono stocastici, ci sarà occasionalmente un grande service time durante il quale molti job rimarranno in coda. 
Quindi, più è alta la variabilità, più ci saranno job in coda e vista la Little's Law lo stesso accadrà con tutti gli altri parametri. 

La formula PK calcola esattamente $E[N]$ ma non ci permette di calcolare le SS probabilities in forma chiusa. Se è richiesto di ottenere queste informazioni si possono utilizzare due approcci:
1. analizzare il sistema utilizzando il metodo **Embedded Markov Chain** che consiste nell'estrarre risultati esatti con passaggi matematici complessi.
2. ottenere risultati approssimati utilizzando il metodo **Phase-Type distributions**. Con questo metodo, il modello generale del service time viene distribuito utilizzando una composizione di distribuzioni esponenziali. 

### Sistemi M/G/$\infty$ e insensitivity

In questo caso si hanno infiniti server ma una distribuzione dei service times generale. In questo caso, le SS probabilities sono le stesse del sistema M/M/$\infty$ poiché dipendono solo dal valor medio del service time e non dalla particolare distribuzione. 
Questa proprietà è chiamata **insensitivity**.
In questo tipo di sistemi *non c'è coda*. 

# Queueing Networks

Finora abbiamo analizzato le queueing systems isolate, tuttavia la queueing theory permette di studiare sistemi composti anche da più sottosistemi formati da code e server che vengono chiamati **Queueing Networks**.
Un Queueing Network può essere:
- *aperto*: se interagisce con il mondo esterno che immette jobs nel sistema (external arrivals). In questo caso, *il numero di jobs nel sistema è una variabile* infatti capire a quanto ammonta questo numero è lo scopo principale dell'analisi.
- *chiuso*: se non c'è nessun tipo di interazione con il mondo esterno quindi il numero di jobs nel sistema è una costante ed è un dato del problema. In questo caso, generalmente lo scopo dell'analisi è di capire quanto vale il throughput. 
Ecco un esempio chiamato transactional server: 

![[transactional_server.webp|center|400]]

Il server ha una CPU e due IO devices. I job arrivano dall'esterno quindi siamo nel caso di una Open QN. Quando questi ultimi arrivano, spendono un po' di tempo nel sottosistema con la CPU il cui service time è una distribuzione esponenziale con media $\frac{1}{\mu_{1}}$. 
Dopo questo tempo, essi possono:
- lasciare il sistema con probabilità $\pi_{0}$.
- mettersi in coda nel sottosistema con l'IO device 1 con probabilità $\pi_{1}$.
- mettersi in coda nel sottosistema con l'IO device 2 con probabilità $\pi_{2}$.
Ovviamente vale la relazione $\pi_{0}+\pi_{1}+\pi_{2}= 1$. 
Il job, dopo esser stato processato nell'IO device, torna nel sottosistema della CPU e ricomincia il ciclo. Questo significa che un job potrebbe visitare molte volte lo stesso service center prima di lasciare il sistema complessivo.

Nel precedente sistema tutti i SC erano $M/M/1$ ma in generale non è detto che ciò valga per tutti i SC del sistema.

Un modo per realizzare un Closed QN è quello di collegare l'output di un Open QN al suo input in modo da formare un anello chiuso in modo che un job che lascia il sistema venga immediatamente reinserito. 
Questo fa sì che il sistema ammetta *un numero finito di jobs simultaneamente* quindi se un job lascia il sistema, esso viene immediatamente rimpiazzato da uno in entrata. Un esempio di questo sistema è un sistema multiprogrammato con un numero di processi costante. 

Per descrivere una QN abbiamo bisogno di conoscere:
1. La **topologia** del network che può essere rappresentata con un grafo direzionato.
2. Il **rate dell'arrivo dei job** ad ogni SC, o almeno il rate dei job che arrivano ai SC direttamente connessi col mondo esterno, indicati con $\gamma_{i}$.
3. I **service rate** di ogni SC, chiamati $\mu_{i}$ ed il numero complessivo di server $C_{i}$.
4. La **routing matrix** $\Pi$ le cui $\pi_{ij}$ sono *le probabilità che un job lasci il SC $i$ e raggiunga il SC $j$.* 
	Se $\sum\limits_{j=1}^{M} \pi_{ij}< 1$ allora $\pi_{i,0}= 1-\sum\limits_{j=1}^{M} \pi_{ij}< 1$ sarà la probabilità che un job lasci la QN dopo aver visitato il SC $i$.

Lo **stato** del network ad un certo istante $t$ sarà un vettore $n(t) = [n_{1}(t), n_{2}(t), ..., n_{M}(t),]^{T}$ dove ogni $n_{i}(t)$ rappresenta il numero di job in ogni SC $i$ al tempo $t$. 

Se il network ammette lo steady state, allora ci saranno $M$ SS probabilities associate ad ogni vettore $n = [n_{1}, n_{2}, ..., n_{M}]^{T}$. Possiamo quindi definire la JPDF delle RV $N_{1},N_{2},...,N_{M}$ ognuna rappresentante il numero di jobs in ogni SC $i$ come: $$p_{n}=p(n_{1}, n_{2}, ..., n_{M}) = P\{N_{1}= n_{1},N_{2}= n_{2},...,N_{M}= n_{M}\}$$
## Caratterizzazione dell'output di un service senter

In una QN, *l'output di un SC costituisce l'input di un altro SC.*
Consideriamo un SC $M/M/1$ allo steady state avente:
- interarrival times distribuito come $F_{A}= 1-e^{-\lambda\cdot t}$ 
- service times distribuito come $F_{B}= 1-e^{-\mu\cdot t}$ 
Vogliamo calcolare la distribuzione dell'**inter-departure time**, cioè il tempo che trascorre prima che il prossimo job lasci il SC: $$F_{D}(t) = P\{D \le t\}$$
Sviluppiamo utilizzando la legge della probabilità totale, ottenendo: $$P\{D \le t\} = P\{D \le t|n > 0\}\cdot P\{n > 0\} + P\{D \le t|n = 0\}\cdot P\{n = 0\}$$
E discutiamo i termini separatamente:
1. $P\{D \le t|n > 0\}$: quando $n > 0$ il prossimo job inizia ad essere servito immediatamente dopo che il precedente ha lasciato il SC. Quindi, l'inter-departure time è distribuito *allo stesso modo del service time* quindi: $P\{D \le t|n > 0\}= F_{B}(t)$
2. $P\{D \le t|n = 0\}$: quando $n=0$, un job che lascia il sistema a $t_{0}$ lascia il sistema vuoto. Quindi, l'inter-departure time (cioè il tempo prima del quale il prossimo job lascerà il SC) sarà composto da due contributi: uno sarà il tempo residuo di *interarriaval time* che bisogna aspettare prima che il prossimo job arrivi, e l'altro sarà un *service time* completo per servire tale job. Tuttavia, ricordiamo che l'interarrival time è distribuito esponenzialmente che è **memoryless**, quindi possiamo scrivere: $P\{D \le t|n > 0\} = P\{A+B \le t\}$.
3. $P\{n > 0\} = \rho$ cioè l'utilizzazione del sistema. 
4. $P\{n = 0\} = 1-\rho$ 

Possiamo quindi riscrivere l'espressione: $$F_{D}(t) = P\{D \le t\} = F_{B}(t)\cdot \rho + P\{A+B \le t\}\cdot (1-\rho)$$
Per semplificare ulteriormente l'espressione andiamo nel dominio di [[Teoria della probabilità#Laplace-Stieltjes Transform LS|Laplace]] dove si ha $L_{A+B}= L_{A}\cdot L_{B}$ poiché vale la proprietà di convoluzione essendo $A$ e $B$ indipendenti. 
Ora $L_{A}= \frac{\lambda}{\lambda+s}$ e $L_{B}= \frac{\mu}{\mu+s}$ e  $\rho = \frac{\lambda}{\mu}$, si può scrivere: $$L_{D}= L_{B}\cdot \rho + L_{A}\cdot L_{B}\cdot (1-\rho) = \dots = L_{A}$$
**"GOD, THIS IS HEAVEN"** *Cit. Stea*
Considerando che le LTS godono della proprietà di univocità, questo risultato significa che: $$F_{D}(t) = F_{A}(t) = 1-e^{-\lambda\cdot t}$$
Ovvero, **gli inter-departure times hanno la stessa distribuzione degli interarrival times**.
Inoltre, questo risultato è vero in generale, e vale per ogni sistema $M/M/C$ per qualunque valore di $C$. Scriviamolo più formalmente:

> [!note] Burke's Theorem
> Dato un sistema $M/M/C$ allo steady state, i cui interarrival rate medio è $\lambda$, allora:
> - Il **Departure Process** è di Poisson con rate $\lambda$.
> - $\forall t$, il numero di jobs nel sistema $n(t)$ è *indipendente* dall'inter-departure time in $[0,t)$

Prendiamo una QN *tandem* formata da due SC $M/M/1$: 

![[tandem.webp|center|300]]

Per il Burke's Theorem gli arrivi al SC2 sono **Poisson Process** con rate $\lambda$ ed $n_{1}(t)$ è indipendente da $n_{2}(t)$. 
Questo significa che: 
$$
\begin{align*}

p_{n} &= p(n_{1},n_{2}) = \\[5pt]
&= p(n_{1})\cdot p(n_{2}) =\\[5pt]
&= [(1-\rho_{1})\cdot \rho_{1}^{n_{1}})]\cdot [(1-\rho_{2})\cdot \rho_{2}^{n_{2}})]
\end{align*}
$$
Dove per noi $p(n_{1})$ sarà *la probabilità che $n_{i}$ jobs si trovino nel SC $i$* e dovremo controllare a posteriori che sia valida $p_{i}< 1$ $\forall i$. 
Chiameremo il precedente prodotto **product form**, ovvero possiamo scrivere le joint SS probabilities a livello di network globale come il prodotto delle SS probabilities dei vari SC.

## Dal Burke's theorem ai queueing newtorks

Consideriamo un network aciclico (ovvero non c'è nessun feedback loop) con determinate probabilità di routing in cui ogni SC è un $M/M/C$. 

![[burke_example.webp|center|400]]

Dal Burke's Theorem sappiamo che l'output del SC1 è un Poisson Process con rate $\lambda$, proviamo a caratterizzare l'output dei SC2, SC3, SC4 e SC5.

Osserviamo che la **sovrapposizione di Poisson Process indipendenti** è anch'esso un Poisson Process con rate uguale alla somma dei rate. 
Il prossimo arrivo lo avremo quando *il più piccolo (residio) interarrival time si esaurisce* ma sappiamo che, con esponenziali indipendenti:
- l'aggettivo *residual* è irrilevante in quanto vale la memoryless.
- il *minimo* è ancora un esponenziale con rate uguale alla somma dei rate. 
Quindi, la sovrapposizione di due Poisson Process indipendenti è un Poisson Process con rate uguale alla somma dei rate. 
Se invece un **Poisson Process è diviso in base a determinate probabilità**, in particolare si viene indirizzati verso un SC con probabilità $\pi$ e verso un altro con probabilità $1- \pi$, allora gli arrivi ad ognuno di questi SC sono anch'essi Poisson Process con rate $\lambda_{1}= \lambda\cdot \pi$ e $\lambda_{2}= \lambda\cdot (1-\pi)$.

![[split_superimposition.webp|center|400]]


Tornando all'esempio precedente, sappiamo che:
- dal Burke's Theorem, l'output del process al SC1 è di Poisson con rate $\lambda$.
- gli arrivi al SC2 e SC3 sono anch'essi un Poisson Process con rate rispettivamente $\lambda \cdot \pi_{1}$ e $\lambda \cdot (1-\pi_{1})$.
- sempre per il Burke's Theorem, il numero di jobs ad ogni SC è indipendente dagli altri.
- gli arrivi al SC4 e al SC5 sono anch'essi Poisson Process con rate rispettivamente $\lambda \cdot \pi_{1}\cdot \pi_{2}$ e $\lambda \cdot [\pi_{1}\cdot (1-\pi_{2}) + (1-\pi_{1})] = \lambda \cdot (1-\pi_{1}\cdot \pi_{2})$

Tutti i SC nel sistema sono $M/M/C$ in cui gli arrival e service rates sono noti, quindi possiamo calcolare:
1. la condizioni di stabilità per ognuno di essi *in isolamento*.
2. le SS probabilities per ognuno di essi *in isolamento*, chiamiamole $p_{i}(n_{i})$, sotto le rispettive condizioni di stabilità. 
Il Burke's Theorem garantisce che, *se le condizioni di stabilità sono tutte verificate*, allora: $$p_{n}= p(n_{1}, n_{2},n_{3}, n_{4}, n_{5}) = \prod_{i=1}^{5}p_{i}(n_{i})$$
Quindi, **tutti i network aciclici con routing probabilistico hanno una product form**, e i suoi input e output sono Poisson Processes. 
Di conseguenza, possiamo facilmente calcolare $E[N_{i}]$, $E[R_{i}]$ (applicando la Little's Law ad ogni SC) e poi calcolare $E[N] = \sum\limits_{i=1}^{5}E[N_{i}]$ e $E[R] = \frac{E[N]}{\lambda}$.

Da notare che la precedente trattazione vale *solo se le code sono infinite* che sono necessarie per applicare il Burke's Theorem.

## Queueing Networks con Feedback Loops

Consideriamo ora una Open QN con Feedbak Loop.  

![[open_qn_fl.webp|center|500]]

In questo sistema, i job lasciano il sistema con probabilità $1-\pi$, mentre vengono rimandati al SC1 con probabilità $\pi$ (non confondere con un sistema tandem che ha più di un SC).

Sappiamo che la sovrapposizione di Poisson Process *indipendenti* è un Poisson Process, tuttavia sappiamo anche che gli arrivi esterni e gli arrivi dal feedback loop *non sono affatto indipendenti*. Inoltre, il processo di arrivo al SC1 non è nemmeno di Poisson. 

Ciò che sorprende è che, nonostante tutto, *il departure process è ancora di Poisson* con rate $\gamma \cdot (1-\pi)$. Infatti, finché gli arrivi esterni sono di Poisson, allora pure le QN con feedback loop ammettono una product form. 

## Risultati generali per Open QN

Definiamo le ipotesi sotto le quali le Open QN ammettono una product form. 

Indiciamo una **Open Jackson Network** come un *arco direzionato* i cui nodi sono $M/M/C$ SCs i cui archi rappresentano l'instradamento tra i SCs. Ogni arco ha un *peso* dato dalla probabilità di routing $\pi_{ij}$, ovvero la probabilità che un job lasci il SC $i$ e raggiunga il SC $j$.
Chiameremo inoltre $\pi_{i0}$ la probabilità che un job lasci il sistema dopo aver visitato il SC $i$. 
Ovviamente vale la relazione $\sum\limits_{j=0}^{M}\pi_{ij}= 1$ $\forall i$.
Infine, è importante fare l'ipotesi che $\pi_{ij}$ siano *indipendenti dallo stato del sistema* (static load balancing) e ad ogni SC gli arrivi esterni siano di Poisson con rate $\gamma_{i}$. 
Il vettore $\gamma = [\gamma_{1}, \gamma_{2},..., \gamma_{M}]^{T}$ *non deve essere identicamente nullo*, altrimenti avremmo una QN che non è open. 

![[open_jackson.webp|center|300]]


Riordinando, otteniamo le ipotesi di una Open Jackson Network:
1. Un numero M di $M/M/C_{i}$, ad ogni SC $i$, i $C_{i}$ server hanno un service rate di $\mu_{i}$.
2. Gli arrivi esterni sono di Poisson con $\gamma = [\gamma_{1}, \gamma_{2},..., \gamma_{M}]^{T}$.
3. Il routing è di *Markovian*, cioè le probabilità sono *state-indipendent*.
4. Gli archi vengono attraversati in un tempo pari a zero (quindi non c'è delay negli archi ma solo nei nodi).
Le Open Jackson Networks ammettono una product form definita dal seguente teorema:

> [!note] Jackson's Theorem
> In una **Open Jackson Network**, sotto le ipotesi precedentemente definite, se $\rho_{i}= \frac{\lambda_{i}}{C_{i}\cdot \mu_{i}} < 1$ $\forall i$, allora avremo $$p_{n}= p(n_{1}, n_{2},..., n_{M}) = \prod_{i=1}^{M}p_{i}(n_{i})$$ dove: $$\begin{equation}p_{i}(n_{i}) =
\begin{cases}
p_{i}(0)\cdot \frac{(C_{i}\cdot \rho_{i})^n_{i}}{n_{i}!} & n_{i}\le C_{i} \\[4pt]
p_{i}(0)\cdot \frac{C_{i}^{C_{i}}\cdot p_{i}^{n_{i}}}{C_{i}!} & n_{i}\ge C_{i}
\end{cases}
\end{equation}$$ sono le **SS probabilities** di un sistema $M/M/C_{i}$ i cui server hanno rate $\mu_{i}$. Se $C_{i}= 1$ allora le SS probabilities si riducono a $(1-p_{i})\cdot p_{i}^{n_{i}}$.

L'unico mattoncino mancante nell'immagine precedente sono gli arrival rates $\lambda_{i}$ che sono necessarie per applicare il teorema. Ottenere questi valori è semplice, un job arriva al SC $i$ perché:
- arriva dall'esterno, con un rate $\gamma_{i}$ oppure
- lascia il SC $j$ e viene indirizzato al SC $i$, secondo una probabilità $\pi_{ij}$.
Quindi per oggi SC varrà la relazione: $$\lambda_{i}= \gamma_{i}+\sum\limits_{j=1}^{M}\pi_{ij}\cdot \lambda_{j}$$
Questa quantità può essere scritta in forma matriciale: $$\lambda = \gamma + \Pi^{T}\cdot \lambda$$dove $\Pi = \{\pi_{ji}\}$ è la routing matrix. Quindi gli arrival rates possono essere calcolati risolvendo (con software appositi): $$\lambda = (I-\Pi^{T})^{-1}\cdot \gamma$$
Se volessimo conoscere quale SC contribuisce al response time complessivo, bisogna fare attenzione perché non c'è nessuna ragione per il quale il response time complessivo $E[R]$ dipenda dal response time dei singoli SC $E[R_{i}]$. 
Per la Little's Law possiamo scrivere: $$E[R] = \frac{E[N]}{\gamma_{tot}}$$
dove $\gamma_{tot}= \sum\limits_{i=1}^{M}\gamma_{i}$. Sostituendo e svolgendo i calcoli otteniamo: 
$$
\begin{align*}
E[R] &= \frac{E[N]}{\gamma_{tot}} =\\[5pt]
&= \sum\limits_{i=1}^{M} \frac{E[N_{i}]}{\gamma_{tot}} =\\[5pt]
&= \sum\limits_{i=1}^{M} \frac{E[N_{i}]}{\lambda_{i}}\cdot \frac{\lambda_{i}}{\gamma_{tot}} \\[5pt]
&= \sum\limits_{i=1}^{M} E[R_{i}]\cdot \frac{\lambda_{i}}{\gamma_{tot}}
\end{align*}$$
dove $\frac{\lambda_{i}}{\gamma_{tot}}$ corrisponde al *numero medio di visite ad ogni SC* ed è un fattore moltiplicativo che evidenzia il fatto che un SC può essere visitato più di una volta o anche mai, per cui ogni response time $R_{i}$ deve essere moltiplicato per il numero medio di visite a quel SC.

Definiamo ora $V_{i}$ la RV che conta il numero di visite al SC $i$. In questo modo possiamo definire il **mean residence time** al SC $i$ come $$E[T_{i}] = E[R_{i}]\cdot E[V_{i}]$$
E in particolare, *nelle OJN il response time è la somma dei residence time*: $$E[R] = \sum\limits_{i=1}^{M}E[T_{i}]$$
che implica che: $$E[V_{i}] = \frac{\lambda_{i}}{\gamma_{tot}}$$
ovvero, il valor medio di visite ad un SC è pari al suo arrival rate diviso per il rate totale di arrivi esterni. 

## Closed Queueing Networks

Passiamo ora ad analizzare le Closed QN. Le ipotesi che consideriamo sono le stesse della Open QN con una impostante differenza: assumeremo che *non ci siano arrivi o partenze di job verso l'esterno* e che *il numero di job sia costante* pari a $K$ (sarà un dato del problema).

Si noti che, se il numero di job è fissato, lo **state space** $\varepsilon$  un una *cardinalità finita* poiché ci sarà un numero finito di modi per distribuire $K$ jobs tra gli M SCs.
Tuttavia, il fatto che questo numero sia finito non significa che sarà anche piccolo, infatti vale la seguente proprietà: $$|\varepsilon| = \binom{K+M-1}{M-1}$$
che rappresenta il numero di modi di inserire $M-1$ "division marks" in una linea di $K+M-1$ elementi. Ciò implica che $|\varepsilon| = O(K^{M-1})$ che è un numero molto grosso. 

Visto che le CJNs sono una variazioni delle OJNs, un modo di agire potrebbe essere quello di usare la stessa procedura utilizzata prima, tenendo presente che $\gamma_{i}= 0$ e $\pi_{i0} = 0$ $\forall i$. Tuttavia, questo procedimento è inapplicabile perché se proviamo a calcolare gli arrial rates ad ogni SC, troviamo la seguente equazione: $\lambda = 0 + \Pi^{T}\cdot \lambda$ che è un *sistema omogeneo* che quindi ammette:
- nessuna soluzione se la routing matrix $I-\Pi^{T}$ ha rango pieno.
- infinite soluzioni
Fortunatamente, la nostra routing matrix *non ha rango pieno* perché $\sum\limits_{j=1}^{M} \pi_{ij} = 1$ $\forall i$ che ci garantisce di avere infinite soluzioni. 
In particolare scegliamo $e = [e_{1}, e_{2}, ..., e_{M}]^{T}$ una soluzione del sistema, inoltre $k\cdot e$ con $k\in R$ sarà anch'essa una soluzione del sistema. Questo significa che possiamo ottenere solo *soluzioni che dipendono da una costante moltiplicativa* e dobbiamo trovare il modo di sbarazzarci di questa costante. 
Il nostro problema si risolve con il seguente teorema: 

> [!note] Gordon and Newell's Theorem
> In una Closed Jackson Network, si consideri *una soluzione* del sistema $\lambda = 0 + \Pi^{T}\cdot \lambda$ chiamata $e = [e_{1},e_{2},...,e_{M}]^{T}$. Chiamare $\rho_{i}= \frac{e_{i}}{\mu_{i}}$. Allora, $p_{n}= p(n_{1},n_{2},...,n_{M}) = \frac{1}{G(M,K)}\cdot \prod_{i=1}^{M} f_{i}(n_{u})$ dove: $$\begin{equation}f_{i}(n_{i}) =
\begin{cases}
\frac{(C_{i}\cdot \rho_{i})^n_{i}}{n_{i}!} & n_{i}\le C_{i} \\[4pt]
\frac{C_{i}^{C_{i}}\cdot p_{i}^{n_{i}}}{C_{i}!} & n_{i}\ge C_{i}
\end{cases}
\end{equation}$$ E $G(M,K)$ sono una costante normalizzante tale che $\sum\limits_{n\in \varepsilon} p_{n}=1$, ovvero $G(M,K) = \sum\limits_{n\in \varepsilon}(\prod_{i=1}^{M}f_{i}(n_{i}))$.

Si noti che l'espressione $f_{i}(n_{i})$ sono *quasi* SS probabilities di un SC $M/M/C_{i}$ perché gli manca un fattore moltiplicativo $p_{i}(0)$. 
Se il SC è un $M/M/1$ allora si $f_{i}(n_{i}) = \rho_{i}^{n_{i}}$ (questo è un caso che descriveremo in seguito).
Inoltre, notare che abbiamo una *costante normalizzante* $G(M,K)$ che è equa visto che abbiamo scelto le condizioni iniziali arbitrariamente. Il fatto che possiamo scrivere $G(M,K) = \sum\limits_{n\in \varepsilon}(\prod_{i=1}^{M}f_{i}(n_{i}))$ non significa che questo sia il modo più efficiente di calcolarlo, infatti il precedente calcolo è proibitivo nella maggior parte dei casi visto il grande numero di somme e moltiplicazioni richieste. 
Fortunatamente, esistono degli algoritmi efficienti per calcolare $G(M,K)$.

## Buzen's Convolution Algorithm

Questo algoritmo ci permette di calcolare $G(M,K)$ in modo efficiente. Se tutti i SCs sono $M/M/1$ allora l'algoritmo opera con una complessità $O(M\cdot K)$ e i passaggi per applicarlo richiedono solo di compilare una tabella. 

Consideriamo la definizione: $$G(M,K) = \sum\limits_{n\in\varepsilon} \prod_{i=1}^{M}f_{i}(n_{i})$$
e mettiamo da parte tutti i vettori di $\varepsilon$ in cui $n_{M}= 0$. Possiamo scrivere la precedente quantità come:
$$
\begin{align*}
G(M,K) &= \sum\limits_{n\in \varepsilon, n_{M}=0} \prod_{i=1}^{M}f_{i}(n_{i}) + \sum\limits_{n\in \varepsilon, n_{M}> 0} f_{i}(n_{i}) =\\[5pt]
&= \sum\limits_{n\in \varepsilon, n_{M}=0} \prod_{i=1}^{M} \rho_{i}^{n_{i}}+\sum\limits_{n\in \varepsilon, n_{M}>0} \prod_{i=1}^{M}\rho_{i}^{n_{i}}
\end{align*}
$$
Nel primo addendo possiamo fermare il prodotto ad $M-1$ perché l'ultimo fattore sarebbe $\rho_{M}^{n_{M}}= \rho_{M}^{0}= 1$ e nel secondo addendo l'ultimo termine nel prodotto può essere scritto come $\rho_{M}^{n_{M}}= \rho_{M}\cdot \rho_{M}^{\hat{n}_{M}}$ con $\hat{n}_{M} \ge 0$ visto che $n_{M}> 0$. 
Questo ci porta a scrivere: 
$$
G(M,K) = \sum\limits_{n\in \varepsilon, n_{M}=0} \prod_{i=1}^{M-1} \rho_{i}^{n_{i}}+ \rho_{M}\cdot \sum\limits_{n\in \varepsilon, \hat{n}_{M}>0} \left(\prod_{i=1}^{M}\rho_{i}^{n_{i}}\right)
$$
Possiamo osservare che:
- il primo addendo è la *costante normalizzante* di una CJN con *$K$ jobs circolanti tra gli $M-1$ SCs*, infatti il SC $M$-esimo è di fatto vuoto. Lo state space per questo CJN è $$\varepsilon = \{(n_{1},n_{2}, ..., n_{M-1}, 0): n_{i}\ge 0, \sum\limits_{i=1}^{M-1}n_{i}= K\}$$ Perciò, il primo addendo è proprio $G(M-1, K)$.
- nel secondo addendo abbiamo "messo da parte" un fattore $\rho_{M}$ ovvero uno dei job circolanti nel sistema e lo abbiamo associato al SC $M$. La somma è quindi *la costante normalizzante* di una CJN con *$K-1$ job circolanti tra gli $M$ SCs*, infatti il job $K$-esimo è di fatto quello associato al SC $M$. Lo state space di questo CJN è $$\varepsilon = \{(n_{1},n_{2},..., n_{M-1}, \hat{n}_{M}): n_{i}\ge 0, \hat{n}_{M}\ge 0, (\sum\limits_{i=1}^{M-1}n_{i})+ \hat{n}_{M} = K-1\}$$ e possiamo rimpiazzare tale somma con $G(M,K-1)$.
Otteniamo quindi la seguente formula ricorsiva: $$G(M,K) = G(M-1,K)+\rho_{M}\cdot G(M,K-1)$$
e, se conosciamo le condizioni iniziali, possiamo calcolare $G(M,K)$ per valori arbitrari di $M$ e $K$.
Per ottenere le condizioni iniziali dobbiamo calcolare:
1. $G(1,k)$: la costante normalizzante di un CJN con un SC e $k$ jobs. Ma lo state space $\varepsilon$ di questo CJN include *solo uno stato* perché tutti e $k$ i job si trovano nello stesso SC. Quindi si avrà $G(1,k) = \rho_{1}^{k}$ $\forall k \ge 0$.
2. $G(j,0)$: la costante normalizzante di un CJN con $j$ SCs e 0 job circolanti nel sistema. Anche in questo caso, questo CJN include *solo uno stato* ovvero quello in cui tutti e $j$ i SCs sono vuoti. Quindi si avrà $G(j,0) = 1$ $\forall j \ge 0$.

Per usare l'algoritmo ci aiutiamo con una tabella di $K+1$ righe ed $M$ colonne. 

![[buzen_table.webp|center|400]]

Una volta inizializzate la prima riga e la prima colonna, iniziamo a riempire la tabella partendo dall'angolo *in alto a sinistra* e *scendendo* per poi andare *un passo a destra*. In ogni cella, l'unica operazione richiesta è quella di *moltiplicare il valore sopra* per quello *all'intestazione della colonna* e *sommare* tale risultato *con il valore nella cella a destra*. 
Alla fine troveremo $G(M,K)$ nella cella *in basso a destra*. 
Vedremo più avanti che anche gli altri elementi della tabella sono importanti. 
Un buon modo di scegliere la soluzione iniziale è quello di fare in modo che i $\rho_{i}$ siano il più vicini possibili ad 1.

### Performance indexes nelle Closed Jackson Networks

Dato $G(M,K)$ si può calcolare $p_{n}$ per ogni vettore $n\in \varepsilon$, quindi possiamo calcolare tutti i performance indexes. Questo processo è molto semplice in quanto richiedono di utilizzare i vari $G(i,j)$. Iniziamo:
1. **CDF, PMF e numero medio di jobs in un singolo SC**: 
	vogliamo calcolare $F_{i}(j) = P\{N_{i}\le j\}$ e $p_{i}(j) = P\{N_{i}= j\}$.
	Iniziamo calcolando $P\{N_{i}\ge j\}$ che può essere calcolato "mettendo da parte" $j$ jobs al SC $i$ e ripetere le stesse considerazioni fatte con l'algoritmo di Bulzen. 
$$
\begin{align*}
P\{N_{i}\ge j\} &= \sum\limits_{n\in\varepsilon, n_{i}\ge j} p_{n}=\\[5pt]
&= \frac{1}{G(M,K)}\cdot \sum\limits_{n\in\varepsilon, n_{i}\ge j} \prod_{h=1}^{M}\rho_{h}^{n_{h}} = \\[5pt]
&= \frac{1}{G(M,K)} \cdot \sum\limits_{n\in\varepsilon, n_{i}\ge j} \left(\prod_{h=1, h\not=i}\rho_{h}^{n_{h}}\cdot \rho_{i}^{\hat{n}_{i}+j} \right) = \\[5pt]
&= \frac{G(M,K-j)}{G(M,K)}\cdot \rho_{i}^{j}
\end{align*}
$$
	sia $G(M,K-j)$ che $G(M,K)$ si trovano nell'ultima colonna della tabella e una volta trovato questo risultato otteniamo la CDF come: 
$$
F_{i}(j) = P\{N_{i}\le j\} = 1-P\{N_{i}\ge j+1\} = 1-\frac{G(M,K-(j+1))}{G(M,K)}\cdot \rho_{i}^{j+1}
$$
	e la PMF come:
$$
p_{i}(j) = P\{N_{i}\ge j\} - P\{N_{i}\ge j+1\} = \frac{\rho_{i}^{j}}{G(M,K)}\cdot [G(M,K-j)-\rho_{i}\cdot G(M,K-(j+1))]
$$
	se definiamo $G(M,x) = 0$ quando $x < 0$ allora possiamo anche calcolare la CDF e la PMF con $j = K$.
	Poi da $p_{i}(j)$ possiamo calcolare tutti gli altri performance indexes.
2. **Numero medio di jobs nel sistema**: 
$$
E[N_{i}] = \sum\limits_{j=1}^{K}P\{N_{i}\ge j\}
$$
3. **Joint Probabilities a due o più SCs**: 
	cerchiamo di calcolare $P\{N_{i}\ge j, N_{l}\ge m\}$, il trick qui è sempre lo stesso: mettiamo da parte $j$ jobs al SC $i$ ed $m$ jobs al SC $l$. Il risultato è:
$$
P\{N_{i}\ge j, N_{l}\ge m\} = \frac{G(M,K-(j+m))}{G(M,K)}\cdot \rho_{i}^{j}\cdot \rho_{l}^{m}
$$
4. **Utilizzazione**: 
	per un singolo SC, l'utilizzazione può essere calcolata come:
$$
U_{i}= P\{N_{i}\ge 1\} = \rho_{i}\cdot \frac{G(M,K-1)}{G(M,K)}
$$
	mentre se vogliamo calcolarla per due o più SCs simultaneamente occupati, possiamo calcolarla come:
$$
U_{i,l}= P\{N_{i}\ge 1, N_{l}\ge 1\} = \frac{G(M,K-2)}{G(M,K)}\cdot \rho_{i}\cdot \rho_{l'}
$$
5. **Throughput di un SC**:
	il throughput di un SC $i$ è pari a $\mu_{i}$ quando quest'ultimo è pieno, quindi:
$$
\gamma_{i}= \mu_{i}\cdot U_{i}= \frac{G(M,K-1)}{G(M,K)}\cdot \mu_{i}\cdot \rho_{i}
$$
6. **Response time di un SC**:
	il response time può essere calcolato tramite la Little's Law:
$$
E[R_{i}] = \frac{E[N_{i}]}{\gamma_{i}}= \frac{\sum\limits_{h=1}^{K} \rho_{i}^{h}\cdot G(M,K-h)}{e_{i}\cdot G(M,K-1)}
$$

## Classi di Queueing Networks

Il fatto che le OJN e le CJN abbiano un routing probabilistico è alquanto sgradevole. Spesso i sistemi che vogliamo modellare come QNs offrono servizi a varie classi di job che vengono instradati a seconda della classe di appartenenza. 

![[qn_classes.webp|center|400]]

Per esempio potremmo avere una QN in cui:
- il flow1 attraversa i SCs 1,2,3 che chiameremo route1,
- il flow2 attraversa i SCs 3,2,4,5 che chiameremo route2.
In questo caso non abbiamo routing probabilistico, nel senso che tutti i jobs delle rispettive classi seguiranno obbligatoriamente o la route1 o la 2.

Le ipotesi di Jackson del Markovian routing non valgono più.
Se invece assumiamo che le routing probabilities dipendano dalla classe dei job, possiamo semplicemente assegnare diversi classi al flow1 e al flow2 e scrivere le due routing matrix come sempre. 
### Classi di Queueing Systems in isolamento

Diamo un'occhiata inizialmente alle classi di sistemi in isolamento. 
Definire lo stato di un SC in una classed network non è così facile perché il comportamento del sistema non è più determinato dal numero di job nel sistema, ma anche dalla propria classe. Infatti, lo stato di un SC con $n$ jobs è una $n$-tupla di class indicators:
$$
S = (c^{(1)}, c^{(1)},...,c^{(n)},)
$$
a significare che il job 1 è di classe $c^{(1)}$, il job 2 è di classe $c^{(2)}$ e così via. 
Questo implica che analizzare una classed QN è qualcosa che dal punto di vista notazionale è oneroso, mentre dal punto di vista concettuale è semplice.
Diamo una serie di teoremi:

> [!note] Classed $M/M/1$ (theorem 1)
> Consideriamo un $M/M/1$ SC con $c$ classi da 1 a $c$. Assumiamo che l'arrival processes di ogni classe $j$ sia indipendente dalle altre. Chiamiamo $\lambda^{(j)}$ il loro arrival rate, e sia $\lambda = \sum\limits_{j=1}^{c}\lambda^{(j)}$. Chiamiamo $\rho = \frac{\lambda}{\mu}$, se $\rho < 1$ allora avremo: $$p_{S}=\frac{\lambda^{(c^{(1)})}\cdot \lambda^{(c^{(2)})}\dots \lambda^{(c^{(n)})}}{\mu^{n}}\cdot (1-\rho)$$

In altre parole, per ottenere le SS probabilities allo stato $S$, è sufficiente moltiplicare gli arrival rates *delle classi a cui ogni job appartiene*, e tutto il resto rimane la stessa formula di un sistema $M/M/1$.

Da notare che: 
- se si ha solo una classe, la formula collassa in quella dell'$M/M/1$ e dipende solo dal numero complessivo di jobs.
- in un sistema classed, la probabilità è la stessa per ogni due stati con lo stesso numero di job per classe, organizzati in qualsiasi ordine (perché il prodotto è commutativo). Quindi, *dipende solo dal numero di jobs in ogni classe e non dalla loro posizione della coda*. 
Quest'ultima proprietà ci permette di trovare una proprietà molto utile:

> [!note] Classed $M/M/1$ (colorrario)
> La probabilità che un classed $M/M/1$ SC abbiam $n$ jobs in totale (appartenenti alla propria classe) è: $$P\{\text{n jobs at SC}\} = \rho^{n}\cdot (1-\rho)$$

### Open Classed Queueing Networks

È possibile evincere che classed Open QNs ammettono una product form, cioè la probabilità che un certo network state è il prodotto della probabilità degli stati ai singoli SCs.

> [!note] Product Form per Classed OJNs
> Un un classed OJN di $M$ $M/M/1$ SCs, finché $\rho_{i}< 1$, $1 \le i \le M$, *le steady state probabilities ammettono una product form*: $$p_{S_{1},S_{2},...,S_{M} = \prod_{i=1}^{M}p_{S_{i}}}$$ dove $p_{S_{i}}$ sono quelle del teorema 1. Al fine di calcolare $\lambda_{i}= \sum\limits_{j=1}^{c}\lambda_{i}^{(j)}$, l'arrival rate $\lambda_{i}^{(j)}$ di class-$j$ jobs al SC $i$ devono essere determinati risolvendo $c$ volte le per-class versioni delle equazioni dell'arrival-rate, cioè $$\lambda^{(j)}=\gamma^{(j)}+\Pi^{(j)^{T}}\cdot \lambda^{(j)}$$ Inoltre, la probabilità di trovare $n = (n_{1},n_{2},...,n_{M})$ job al SCs $M$ *ha anch'essa una product form*: $$p_{n}= \prod_{i=1}^{M}P\{n_{i}\text{ jobs at SC i}\} = \prod_{i=1}^{M}\rho_{i}^{n_{i}}\cdot (1-\rho_{i})$$
> 

Questo significa che semplicemente aggiungendo un po' di complessità perché necessita di calcolare gli arrival rates per ogni classe individualmente e poi aggragarle nel per-SC arrival rates. 
Per cui, questa trattazione non è più difficile, richiede solo più calcoli.

# Processor-Sharing Queueing Systems

Quando si modellano i computer systems come queueing systems, vorremmo modellare i task che vengono eseguiti dal processore come dei job, considerando la CPU come un server. Tuttavia, generalmente i task condividono i tempo del processore, molto spesso equamente (Round Robin). In questo caso, modellare il sistema in termini di FCFS non è una buona idea. 
Per $K$ task che condividono il service rate $\mu$ del processore, sarebbe più ragionevole assumere che *tutti i jobs siano serviti simultaneamente con un rate $\frac{\mu}{K}$*, in cui la parola "simultaneamente" viene utilizzata come astrazione. 
I sistemi di questo tipo vengono chiamati **procesor-sharing queueing system**. In notazione di Kendall vengono denotati con $M/M/1/PS$, questo tipo di sistemi sono in realtà più facili da analizzare di quelli con politica FCFS perché *non hanno nessuna coda*. 

Modelliamo un sistema $M/M/1/PS$ tramite CTMC. Possiamo farlo in quanto gli inter-arrival e i service times sono memoryless e quindi il numero di jobs nel sistema sarà il nostro stato. Andiamo per step:
- quando ci sono *zero job*, l'unica cosa che può accadere è che ne arrivi uno, con rate $\lambda$. Chiaramente, tutti gli arrivi avvengono ad un rate $\lambda$, indipendentemente dallo stato, per cui l'unica cosa che dobbiamo discutere è il service rate.
- quando c'è solo *un job*, esso ha il server tutto per sé, quindi l'arco che va a sinistra avrà rate $\mu$ in quanto questo è praticamente un $M/M/1/FCFS$.
- quando ci sono *due job*, entrambi i job avranno il server dedicato per $\mu/2$. Da notare che quando il secondo job arriva, il primo avrà già iniziato la fase di servizio, tuttavia il suo residual service time è anch'esso esponenziale e vale la proprietà memoryless. Per cui l'arco che va a sinistra è quello di un $M/M/2/FCFS$, ed avrà quindi rate $2\cdot \frac{\mu}{2}= \mu$.
- quando ci troviamo allo *stato $n$*, la transizione verso $n-1$ avverrà quando il minimo tra gli $n$ residual service time scade. Tuttavia, tutti i service times sono esponenziali con rate $\mu/n$, quindi l'arco che va verso sinistra avrà rate $n\cdot \frac{\mu}{n}= \mu$. 
Quindi il CTMC avrà il seguente aspetto: 

![[rr_qn.webp|center|400]]

Se $\rho < 1$ allora $p_{n}= (1-\rho)\cdot \rho^{n}$, quindi $E[N] = \frac{\rho}{1-\rho}$, $E[R] = \frac{E[N]}{\lambda} = \frac{1}{\mu-\lambda}$ esattamente come in un FCFS. Si noti che $E[N_{q}]$ e $E[W]$ non hanno alcun senso qui perché in un sistema PS non c'è coda. 
Infatti, i sistemi PS sono **insensitive** verso la distribuzione dei service times. Fintanto che i service times sono Coxian, tutti i risultati sono gli stessi e l'unico parametro rilevante è il *mean sercie time $1/\mu$*. Di conseguenza, tutti i sistemi $M/Cox/1/PS$ si comportano come quello descritto precedentemente. 

Le distribuzioni Coxiane sono particolari distribuzioni *phase-type* che possono essere descritte come segue. Immaginiamo che il server includa al massimo $n$ stages. Ognuno di questi ha tempo esponenziale (eventualmente con mean diverse) ed ogni stage è indipendente dagli altri. Dopo uno stage $j$, si può andare al prossimo stage con probabilità $\pi_{j}$ oppure lasciare il server, con probabilità $1-\pi_{j}$. 
Da notare che una Coxiana collassa in una Erlang quando $\pi_{j}= 1$ $\forall j$ e tutti gli stage sono identici. 

![[coxian.webp|center|400]]

Le distribuzioni Coxiane possono approssimare le distribuzioni con:
- $CoV < 1$: quindi la Erlang o una distribuzione deterministica. 
- $CoV > 1$: quelle con una grande variabilità come ad esempio le heavy-tailed.
Per esempio un'approssimazione Coxian di una distribuzione $E[X] = m$ e $C$

Troviamo le SS probabilities in un sistema $M/Cox/1/PS$

