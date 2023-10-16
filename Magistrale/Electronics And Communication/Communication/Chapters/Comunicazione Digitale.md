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
> Il **valor medio** di un processo stocastico è definito come: $$\mu_{X}(t_{0}) = E\{X(t_{0})\} = \int_{-\infty}^{+\infty} x f_{X(t_{0})}(x) dx$$

In generale, il valor medio dipende dal tempo $t_{0}$.

> [!note] Funzione di Autocorrelazione
> La **funzione di autocorrelazione** di un processo stocastico è definita come: $$R_{XX}(t_{1},t_{2}) = E\{X(t_{1})X^{*}(t_{2})\} = \int_{-\infty}^{+\infty}\int_{-\infty}^{+\infty} x_{1}x_{2}f_{X(t_{1}),X(t_{2})}(x_{1},x_{2})dx_{1}dx_{2}$$

e rappresenta la relazione tra le variabili aleatorie $X_{1} = X(t_{1})$ e $X_{2} = X(t_{2})$.
Questa doppia integrazione è generalmente molto difficile da calcolare quindi introduciamo alcune proprietà che ci aiutano nei calcoli.

## Stazionarietà

> [!note] Processo Stocastico Stazionario
> Un processo stocastico si dice stazionario se le sue proprietà statistiche sono invarianti da spostamenti dell'indice nel tempo oppure non dipendono affatto dal tempo (stazionarietà del primo ordine).
> 

Nella stazionarietà del primo ordine si ha che le proprietà statistiche di $X(t_{0})$ e di $X(t_{0}+c)$ sono le stesse per qualsiasi valore di $c$. (per esempio il valor medio non dipenderà da $t_{0}$).
Se invece il processo è stazionario del secondo ordine allora si ha che le proprietà statistiche di $\{X(t_{1}),X(t_{2})\}$ e di $\{X(t_{1} + c),X(t_{2} + c)\}$ sono le stesse per qualsiasi $c$. In questo caso la densità di probabilità assume questa forma: $$f_{X(t_{1}),X(t_{2})} (x_{1},x_{2}) = f_{X}(x_{1},x_{2},t_{2}-t_{1})$$ e di conseguenza l'autocorrelazione dipenderà solo dalla differenza di tempo $t_{2}-t_{1}$.

Tra indipendenza e stazionarietà la proprietà più difficile da ottenere è l'indipendenza. 
Il processo stocastico più comune a cui possiamo pensare in ambito di comunicazione è il rumore (il rumore è anche stazionario). 

## Stazionarietà in senso lato

Se un processo è stazionario di primo e secondo ordine allora possiamo usare la definizione di stazionarietà in senso lato. 
> [!note] Stazionarietà in Senso Lato
> Un processo è **stazionario in senso lato** (SSL) se:
> - $E\{X(t)\} = \mu _{X}$
> - $E\{X(t_{1})X(t_{2})\} = R_{XX}(t_{2}-t_{1})$

In questo caso la funzione di autocorrelazione descrive l'interazione tra due variabile aleatorie ottenute campionando il processo $X(t)$ con una differenza di tempo pari a $\tau = t_{2}-t_{1}$.
Quindi, più il processo cambia velocemente nel tempo più rapidamente diminuirà la funzione di autocorrelazione rispetto al suo massimo $R_{XX}(0)$ e lo spettro del processo associato sarà più esteso (perché il segnale cambia velocemente e di conseguenza ha molte più componenti ad alta frequenza).

## Densità spettrale di potenza

La densità spettrale di potenza $S_{XX}(f)$ di un processo stocastico stazionario in senso lato $X(t)$ descrive la distribuzione di potenza intorno alle componenti frequenziali e si misura in watt per hertz (W/Hz).

Esiste un teorema che lega la densità spettrale di potenza con la trasformata di Fourier della funzione di autocorrelazione:

> [!note] Teorema di Wiener-Kintchine
> $$S_{XX}(f) = \mathbb{F}\{(R_{XX}(\tau))\} = \int_{-\infty}^{+\infty} R_{XX}(\tau)e^{-j2 \pi f \tau} d\tau$$
> 

Da questo teorema deriva che la potenza di un processo $X(t)$ può essere calcolata come: $$P_{X}= R_{XX}(0) = \int_{-\infty}^{+\infty}S_{XX}(f)df$$
# Comunicazione Digitale

Vedremo la differenza tra la modulazione analogica e quella digitale, infatti la modulazione rimane analogica ma il segnale era originariamente digitale e solo dopo convertito.

## Pulse Amplitude Modulation PAM

Una sorgente genera i bit $d_{k}$ che vengono mappati nel mappatore in simboli $a_{i}$ che vengono usati per realizzare un segnale tempo continuo (analogico) di questo tipo: $$\tilde{s}_{PAM} (t) = \sum\limits_{i} a_{i} g_{T}(t-iT)$$
E: 
$$
\begin{align*}
s_{PAM}(t) &= Re\{\tilde{s}_{PAM}(t)e^{j2 \pi f_{c}t}\} \\
&= \tilde{s}_{PAM}(t)cos(2\pi f_{c}t)\\
\end{align*}$$
Da ora in poi useremo sempre l’inviluppo complesso ma per motivi di notazione rimuoveremo la tilde tenendo in mente che stiamo studiando l’equivalente in banda base.

Studiamo nel dettaglio i blocchi:
## Mappatore

Lo scopo del mappatore è quello di convertire i bit $d_{k} \in \{0,1\}$ in simboli $a_{i}$.
L’expectation dei bit è $E[d_{k}] = 1/2$ quindi vado a mappare 0 in -1 ed 1 in +1 quindi $a_{i} \in \{-1,+1\}$
I bit sono un processo stocastico discreto nel tempo e con stato discreto.
Il mappatore non ha solo lo scopo di spostare il mean value in zero ma anche di compressare l’informazione perché un insieme di bit può essere compresso in un solo simbolo. 
$M$ è la grandezza della costellazione dei simboli. Nel caso di prima $M = 2$ e il numero di simboli è $m = log_{2}M = 2$.
Anche i simboli sono processi stocastici.
Questi processi possiamo assumere che siano stazionari e indipendenti. 
Un altra cosa che deve fare il mappatore è migliorare il più possibile la banda occupata. (ridire meglio)
La sorgente genera simboli con un rate $R_{b}= \frac{1}{T_{b}}$. Ogni $m$ bit si ottiene un simbolo, se un simbolo mappa più di un bit l’efficienza spettrale aumenta. 
$T = mT_{b}$ con $T$ il periodo del simbolo.
Quindi $R = \frac{1}{T} = \frac{1}{mT_{b}}=\frac{R_{b}}{m}= \frac{R_{b}}{log_{2}M}$
Dobbiamo misurare l’efficenza spettrale andando a considerare la banda occupata. 
$B \inverse \frac{1}{T}$ quindi più è grande M più è piccola la banda occupata.

Energy Efficenty
Spectral Efficenty
ad esempio FM è buona in termini di Energy Efficenty ma non in Spectral
Generalmente sono sempre uno l’opposto dell’altro, anche nella PAM succede. 
Calcoliamo l’energia minima per trasmettere un bit.
$s_{PAM}(t) = \sum\limits_{i} a_{i}g_{T}(t-iT)$ quest’ultima è continua in tempo e in stato. Non è stazionaria ma cyclostationary (perché la stazionarietà è periodica di periodo $T$, comunque non più stazionario). Non è nemmeno più indipendente perché dal disegno si nota che il prossimo sample dipenderà dal precedente. 
$R_{a}(\tau) = E[a^{2}]\delta (\tau)$ 
dopo il campionamento: $R_{a}[\tau] = E[a^{2}]\delta [\tau]$
$S_{a}(f) = E[a^2]$ 
Filtriamo con un filtro lineare:
$S_{s}(f)= \frac{1}{T} S_{a}(f)|G_{T}(f)^{2}|$

