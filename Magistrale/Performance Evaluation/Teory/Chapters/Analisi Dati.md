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
Questo tipo di grafico è utile per date un'occhiata a come i dati sono dispersi e se ci s
### Box Plots

### Lorenz Curves

## Riassumere i dati