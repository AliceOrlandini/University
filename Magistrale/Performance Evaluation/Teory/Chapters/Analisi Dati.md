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


### CDF Empirica

### Histograms

### Scatter Plots

### Box Plots

### Lorenz Curves

## Riassumere i dati