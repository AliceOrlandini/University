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
E[N_{q}] = E[N] -
$$
- Tramite la Little's Law possiamo calcolare $\overline{\lambda}$ 
- **Valor medio del response time**:
- **Valor medio del waiting time**:
- **Arrival-Time probability**:
## Sistemi con bulk arrivals


