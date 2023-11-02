# Statistica

La statistica è quella scienza per cui risultati probabilistici sono dedotti da dati sperimentali. Infatti, dato un insieme di dati si cerca di ottenere una CDF, al contrario della teoria della probabilità in cui data una CDF si cerca di ottenere dei dati. 
Ci sono 2 concetti alla base della statistica:
1. **Popolazione**: il set di individui sul quale si vogliono compiere statistiche. In questo caso, le informazioni relative alla popolazione (come il valor medio o la varianza) sono costanti e vengono chiamate *parametri*. Questi ultimi sono sconosciuti e vanno stimati con i sample.
2. **Sample**: un sottoinsieme della popolazione.  Si stimano le proprietà dei sample per poi poter astrarre a tutta la popolazione (con un po' di approssimazione). Le informazioni relative ai sample vengono chiamate *statistiche* e sono *variabili aleatorie*.
Il nostro compito sarà quello di stimare i parametri con le statistiche. 
Quindi, data una popolazione di $N$ individui con un determinato valor medio $\mu$ e una varianza $\sigma^{2}$ si effettua un esperimento casuale chiamato *random sampling* che consiste nello scegliere casualmente $n$ elementi dalla popolazione con $n \ll N$, cioè $\binom{N}{n}$. A questo punto si effettuano le statistiche, ad esempio la *sample mean* calcolata come $\overline{X} = \frac{\sum\limits_{i=1}^{n} X_{i}}{n}$   che è una variabile aleatoria in quando risultato di un random sampling. 

> [!danger] Ricorda
> *Sampling is a random experiment!*

Per adesso assumeremo che le variabili aleatorie ottenute dopo il sampling siano **indipendenti identicamente distribuite** (IID). Quest'assunzione è fondamentale e più avanti nel corso vedremo il caso in cui questa condizione non sussiste. 

Due problemi fondamentali legati al fatto che il campionamento è un esperimento casuale riguardano il fatto che le statistiche osservate potrebbero essere causate da:
- **proprietà strutturali**: fatti veri per la popolazione.
- **randomness**: ad esempio la "fortuna del sorteggio".
Ci sono dei modi per quantificare gli effetti della randomness che vedremo più avanti.
## Rappresentazione dei dati

Assumere di avere un sample di $n$ variabili aleatorie IID $X_{i}$ con $1 \le i \le n$ chiamate *osservazioni* e ottenute tramite misurazioni del sistema. $n$ viene chiamato *sample width* e il problema che ci poniamo è quello di rappresentare al meglio i dati tramite grafici. Scegliere il grafico giusto è un'arte, bisogna sempre tenere a mente che la visualizzazizone dei dati deve aiutarci a capire come funziona il sistema  e quindi bisogna scegliere il grafico che ci permette di dedurre quante più informazioni possibili.
### CDF Empirica

La CDF Empirica (ECDF) può essere ottenuta plottando la seguente funzione: $$F(x ) = \frac{1}{n}\sum\limits_{i=1}^{n}1_{\{X_{i}\le x\}}$$
In altre parole, $F(x)$ è la proporzione di dati che non supera il valore di $x$. Ciò ci permette di ottenere una funzione a scalini, utile quando la distribuzione è discreta.
Operativamente non si usa mai la formula ma si prende un sample e si calcolano le sue *ordered statistics*. Queste sono ottenute ordinando in modo crescente i sample in modo che:
$X_{(1)} = min\{X_{i}\}$
$X_{(i)} \le X_{(i+1)}$
$X_{(n)} = max\{X_{i}\}$
A questo punto si ottiene una funzione scalino con step di valore $\frac{1}{n}$.
La forza delle ECDF è che sono comparabili quindi ci permette di confrontare le performance di sistemi diversi.
### Histograms

Ipotizziamo di star trattando delle variabili aleatorie continue. Gli **istogrammi** ci permettono di ottenere delle PDF empiriche (EPDF) a partire dal sample. L'algoritmo per creare un istogramma è il seguente:
1. Selezionare la grandezza del bucket $\Delta$, cioè un range in cui le osservazioni verranno contate come stessa colonna dell'istogramma. 
2. Inizializzare i bucket a zero.
3. Per ogni osservazione $X_{i}$ aggiungere 1 al bucket $j = \lfloor \frac{X_{i}}{\Delta} \rfloor$.
Il problema più grande degli istogrammi è che il loro aspetto dipende da come si sceglie $\Delta$. Ci sono dei modi per scegliere il $\Delta$ ma la cosa migliore per noi è andare a tentativi. Infatti:
> [!danger] Ricorda
> *La conoscenza del dominio del problema prevale sempre sui metodi formali!*

Inoltre, bisogna ricordarsi di normalizzare le ordinate con i rispettivi sample widths, altrimenti è impossibile effettuare comparazioni. 

![Istogramma|center|400](https://upload.wikimedia.org/wikipedia/commons/thumb/c/c3/Histogram_of_arrivals_per_minute.svg/1200px-Histogram_of_arrivals_per_minute.svg.png)

### Scatterplots

Lo **scatterplot** è un grafico in cui i sample sono sparsi su un piano orizzontale in cui ogni punto è disegnato in una giusta altezza.
Questo tipo di grafico è utile per date un'occhiata a come i dati sono dispersi e se ci sono dei raggruppamenti. Sono anche utili per individuare gli *outliers* che sono invece difficili da individuare con gli istogrammi.

![Scatterplot|center|400](https://www.health.state.mn.us/communities/practice/resources/phqitoolbox/images/scatter_ex_atlanticcities.jpg)

### Box Plots

I **Box Plots** detti anche *box and whiskers* consistono nel plottare:
1. Un *box* i cui bordi sono il $1^{st}$ e il $3^{rd}$ quartili (o il $25^{th}$ e il $75^{th}$ percentile) che chiameremo $Q_{1}$ e $Q_{3}$.
2. Una *linea* che rappresenta la *mediana* (cioè il $50^{th}$ percentile).
3. Due *baffi* (whiskers) sporgenti dal box che possono rappresentare varie cose a seconda del software utilizzato, di solito rappresentano:
	- il massimo e il minimo (non comune).
	- il $2^{nd}$ e il $98^{th}$ percentile.
	- l'inter quartile range IQR.

![BoxPlot|center|400](https://doc.arcgis.com/it/insights/latest/create/GUID-5C7AAF44-C609-472D-9193-0E9B23C6B68F-web.png)

### Lorenz Curves

I grafici che abbiamo visto finora erano atti a comparare diversi sistemi, ora ne vediamo uno per valutare la *variabilità* di un sample: le **curve di Lorenz**.
Si utilizzano per esempio per misurare se l'output del sistema è perfettamente equo tra tutti gli utilizzatore. Un esempio può essere il tempo di risposta di un server che non è sempre lo stesso per tutti gli utenti ma può essere molto buono per la maggior parte degli utenti e molto alto per una minoranza. In questo caso,  il sistema non è equo e questa misura si può quantificare con le curve di Lorenz che sono applicabili alle variabili aleatorie *non negative* (per noi tutte lo sono).
L'algoritmo è il seguente:
1. ordinare il sample in modo da ottenere *ordered statistics*: $X_{(1)} \le X_{(2)} \le ... \le X_{(n)}$.
2. calcolare $T = \sum\limits_{i=1}^{n}X_{i}$ la somma totale del sample.
3. plottare i punti $(\frac{1}{n}, \frac{X_{(1)}}{T})$, $(\frac{2}{n}, \frac{X_{(1)}+X_{(2)}}{T})$, ... , $(\frac{j}{n}, \frac{\sum\limits_{i=1}^{j}X_{j}}{T})$ con $1 \le j \le n$.
4. interpolare assumendo il punto $(0,0)$ come primo punto della curva. 
Ovviamente, la curva intersecherà il punto $(1,1)$. 
Se tutti i valori sono equi allora la curva apparirà come la bisettrice del primo quadrante, questa linea viene anche chiamata *linea di massima equità* e viene disegnata per reference.
Se $T = X_{(n)}$ e tutti gli altri valori sono nulli allora la curva segue l'asse $x$ fino al punto $(1- \frac{1}{n},0)$ per poi crescere fino al punto $(1,1)$.
## Riassumere i dati

Molto spesso è richiesto di fornire un singolo numero che descrive le performance del sistema. Ciò è utile ad esempio per effettuare ad esempio comparazioni tra sistemi senza utilizzare le ECDF che sono invece costose da memorizzare. 
Esistono 3 possibili alternative: il sample mean, median e mode. 
### Indici di tendenza

> [!note] Sample Mean
> Si definisce **Sample Mean** come $$\overline{X} = \frac{1}{n}\cdot \sum\limits_{i=1}^{n}X_{i}$$ con $\overline{X}$ una variabile aleatoria visto che è il risultato di una somma di variabili aleatorie. 

Le sue proprietà sono:
- **Valor Medio**: $E[\overline{X}] = \frac{1}{n}\cdot E\left[\sum\limits_{i=1}^{n}X_{i}\right]= \frac{1}{n}\cdot n \cdot E[X_{i}] = \mu$
- **Varianza**: $Var(\overline{X}) = Var\left(\frac{1}{n}\cdot \sum\limits_{i=1}^{n}X_{i}\right)= \frac{1}{n^{2}}\cdot n \cdot Var(X_{i}) = \frac{\sigma^{2}}{n}$
ovvero $\overline{X} \thicksim \mathcal{N}(\mu, \frac{\sigma^{2}}{n})$.

Più grande è il sample più la $\overline{X}$ converge ad una variabile aleatoria uguale alla popolazione con valor medio $\mu$ e senza varianza.
Se le $X_{1}, ..., X_{n}$ sono normali (a meno che non siano heavy tail) allora anche $\overline{X}$ è normale, per qualsiasi valore di $n$.
Se invece non fossero normali, $\overline{X}$ tende ad una normale per $n$ grande ($n \ge 30$).

> [!note] Sample Median
> La **Sample Median** è un valore ottenuto ordinando gli *ordered statistic* ($X_{(1)}, X_{(2)}, ..., X_{(n)}$) e prendendo il valore centrale. Se non ci fosse un valore centrale si prendono i due centrali e si fa la media. 

> [!note] Sample Mode
> La **Sample Mode** è il valore con la probabilità più alta. Per ottenerlo si può considerare la colonna più alta dell'istogramma associato all'esperimento.

Ma quale di questi bisogna utilizzare? La risposta è: dipende. In generale possiamo dire che se la distribuzione è simmetrica allora la media e la mediana sono uguali. Inoltre, il mode potrebbe non essere un valore unico. 
La media è l'unica ad avere la proprietà di *additività* cioè la media di una somma è la somma delle medie. 
Infine, la media è *sensibile agli outliers* quindi se si ha un sample piccolo piccoli outliers possono cambiare la media in modo considerevole. In quest'ultimo caso, è meglio considerare la mediana, vediamo di dimostrarlo matematicamente:
supponiamo di avere un sample di $n$ osservazioni (con $n$ dispari) la cui sample mean è pari a $\overline{X}$. 
Visto che le osservazioni sono dispari la mediana sarà $X_{0.5} = X_{(\lceil n/2\rceil)}$. 
Assumer
### Indici di dispersione

## Fitting di una distribuzione

## Intervalli di Confidenza





