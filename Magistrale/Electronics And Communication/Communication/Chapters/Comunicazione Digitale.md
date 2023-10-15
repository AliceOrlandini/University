# Processi Stocastici

I processi di dividono in:
- **Deterministici**: il processo è rappresentato da una relazione matematica esplicita. In questo caso, dato un input si conosce esattamente l'output. 
- **Stocastici**: è un processo aleatorio ovvero un processo in funzione di variabili aleatorie. In fisica, ci sono molti parametri che influenzano il risultato di un esperimento e per questo il risultato non è predicibile a priori. 

Considerando che ci stiamo occupando di sistemi di comunicazione ci concentreremo sui processi stocastici.

> [!note] Processo Stocastico
> Un **processo stocastico** è una successione di variabili aleatorie con la quale si rappresenta un sistema che si sviluppa nel tempo e nello spazio secondo leggi probabilistiche. Si rappresenta nel seguente modo $X(t, \xi)$

Quando si va a campionare il processo in un punto si ottiene una variabile aleatoria, ad esempio: $t = t_{0}$ con $X(t_{0}, \xi)$ è una variabile aleatoria.

Esistono varie categorie di processi stocastici:
- Parameter Space: è un set $T$ di indici $t \in T$.
- State space: è un set $S$ di valori $X(t) \in S$
Questi possono essere discreti o continui. 

## Funzione di distribuzione e densità di probabilità

Dato $X(t)$ un processo stocastico e lo si campiona in $t = t_{0}$ ottenendo la variabile aleatoria $X(t_{0})$.

> [!note] Funzione di Distribuzione
> La **funzione di distribuzione** di $X(t_{0})$ è data da $$F_{X(t_{0})}(x) = P\{X(t_{0}) \le x\}$$

Si nota come questa funzione dipenda dall'istante di campionamento $t_{0}$ quindi per diversi valori di $t$ si ottengono diverse variabili aleatorie. 

> [!note] Densità di Probabilità
> La **densità di probabilità** è la derivata della funzione di distribuzione: $$f_{X(t_{0})}(x) = \frac{d}{dx} F_{X(t_{0})}(x)$$

## Indipendenza

Un processo stocastico ha la proprietà di indipendenza se le variabili aleatorie ottenute dal campionamento da $n$ campionamenti $t_{1},...,t_{n}$ sono variabili aleatorie indipendenti. 
Di conseguenza, la distribuzione sarà: (considerando la linearità)
$$
\begin{align*}
F_{X(t_{1}),...X(t_{n})}(x_{1},...,x_{n}) &= P\{X(t_{1}) \le x_{1} \} \cdots P\{X(t_{n}) \le x_{n} \} =\\[4pt]
&= F_{X(t_{1})}(x_{1}) \cdots F_{X(t_{n})}(x_{n})
\end{align*}
$$
e la densità di probabilità:
$$F_{X(t_{1}),...,X(t_{n})}(x_{1},...x_{n}) = f_{X(t_{1})}(x_{1})\cdots F_{X(t_{n})}(x_{n})$$
## Valor medio e autocorrelazione

Calcoliamo alcuni parametri dei processi stocastici.

> [!note] Valor medio 
> Il valor medio di un processo stocastico è definito come $$$$