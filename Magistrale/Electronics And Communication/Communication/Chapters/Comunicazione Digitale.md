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

$A = E[a_{i}^{2}]$
Noi assumeremo che $E[a_{i}] = 0$

La funzione di autocorrelazione è deterministica e per questo calcoliamo la densità spettrale di potenza. Quest’ultima dipende solo dal filtro. Quindi scegliendo il filtro di partenza con cui realizzo il segnale, cambia la densità spettrale di potenza.
Scegliamo per esempio una rect $g_{T}(t) = rect(\frac{t-T/2}{t})$ 
che in frequenza è una sinc e quindi avrà uno spettro esteso.
Se vogliamo uno spettro ristretto segliamo una rect: $G_{T}(f) = rect(fT)$ che ha $g_{T}(t) = \frac{1}{T}sinc(\frac{t}{T})$
Generalmente si trova un compromesso.

Ora vediamo la struttura del ricevitore. 
Il segnale passa attraverso il canale e otteniamo $y(t) = h(t) conv s(t)$
$r(t) = y(t)+n(t)$ 
$h(t)$ rappresenta l’effetto del canale sul segnale, può essere visto come un linear time invariant filter. (LTI)
In questo tipo di segnali $y(t) = x(t) \circledast h(t)$ che in frequenza diventa $Y(f) = X(f)\cdot H(f)$
Se $h(t) = \delta (t)$ l’effetto del segnale non si nota. 

$w(t)$ è il rumore termico, veniva modellato come gaussiano bianco $\mathbb{N}(0,\frac{N_{0}}{2})$ quindi ha $S_{w}(f) = \frac{N_{0}}{2}$. 

Infine, la struttura della PAM in ricezione.
Dato il segnale $r(t)$ esso è in banda base (perché consideriamo l’inviluppo complesso)
La prima cosa che troviamo è il filtro passa basso $g_{R}(t)$ 
$r(t) = s(t) \circledast h(t) + w(t) = s(t)+w(t)$
L’output del filtro passa basso sarà $x(t) = r(t) \circledast g_{R}(t) = s(t) \circledast g_{R}(t) + w(t) \circledast g_{R}(t)= \sum\limits_{i}a_{i}g(t-iT)+n(t)$
Con $g(t) = g_{T}(t) \circledast g_R(t)$.
L’unico elemento di disturbo per ora è il rumor $n(t) = w(t) \circledast g_{R}(t)$ di cui poi parleremo.

Subito dopo c’è il campionatore, il suo scopo è estrapolare un campione.
$x(m)|_{t=mT} = x[mT] = \sum\limits_{i}a_{i}g(mT-iT) +n[mT] = \sum\limits_{i}g((m-i)T)+n[m]$
poniamo $m-i = l$ da cui ottengo $i = m- l$ e $m-i = l$
Sostituendo: $\sum\limits_{l}a_{m-l}g(lT)+n[m] = a_{m}g(0) + …$
Ho estratto il primo elemento. Gli altri elementi sono l’interferenza intersimbolica, si origina quando il filtro $g_{T}(t)$ non è una rect (perché le sinc si annullano nei multipli del periodo). 
$g(t) = g_{T}(t) \circledast g_{R}(t)$ è un triangolo.
Il problema della rect è che è illimitata in frequenza quindi si ha uno spettro illimitato che non si usa in realtà. Quindi si ha interferenza intersimbolica. 
La condizione per non avere ISI è $g(lT) = 1$ se $l=0$ e $g(lT) = 0$ se $l \not= 0$ perché così $x(m) = a_{m}+n(mT)$
L’importante è che in multipli interi di T sia nulla, non deve essere necessariamente una rect. 
Da notare che la condizione non basta perché trasmettitore e ricevitore devono essere perfettamente sincronizzati. 

La stessa condizione può essere ricavata con Nyquist nel dominio della frequenza: $F(g(l)) = \sum\limits_{l}g(l)e^{-j2\pi flT} = \frac{1}{T}\sum\limits_{k}G(f-\frac{k}{T})$
Bisogna scegliere la $g(t)$ in modo che $G(f)$ sia pari a 1 perché così ottengo T. (calcoli sulle slide)

La funzione coseno rialzato ha una banda $B_{RC}= \frac{1+\alpha}{2T}$ con $\alpha$ detto fattore di roll-off. 
Se $\alpha$ è pari a zero si ottiene una rect. 
In frequenza si ottengono delle sinc. 

Non dobbiamo dimenticarci che c’è sempre rumore bianco additivo, anche $w(t)$ è l’inviluppo complesso del rumore gaussiano bianco. L’equivalente in banda base del rumore è il rumore bianco in banda perché in teoria è bianco ovunque ma noi filtriamo in banda base quindi tagliamo tutte le altre parti.  
$S_{\tilde{w}}(f) = 2N_{0}$ densità spettrale di potenza del rumore bianco in banda. 
$E[w_{I}(n) \cdot w_{Q}(n)] = E[w_{I}(n)]\cdot E[w_{Q}(n)] = 0$ perché il valor medio del rumore bianco è zero. 
Ora concentriamoci su $n(t)$, la sua densità spettrale di potenza sarà $S_{n}(f) = S_{w}(f)|G_{R}(f)|^{2}$ che *non* è bianco non essendo costante in frequenza ma dipendente dal filtro.
Questo ci serve per integrarla e ottenere la potenza.
Dobbiamo capire qual è la strategia migliore per filtrare il rumore in modo da toglierne il più possibile. Possiamo usare ancora una volta un coseno rialzato (filtro adattato).

Ora possiamo mettere delle condizioni sui filtri, una soluzione buona è quella di usare $H_{RRC}(f,\alpha) = \sqrt{H_{RC}(f,\alpha)}$
di modo che $G_{R}(t) = G_{T}(t)$

Infine, possiamo definire la banda della PAM: $$B_{PAM}^{(PB)}= 2B_{PAM}^{(BB)} = \frac{1+\alpha}{T}= (1 + \alpha) \frac{R_{b}}{log_{2}M}$$
# MATLAB RRC Filter Design

Innanzitutto capiamo come è fatto un filtro a coseno rialzato in matlab. 
$g = rcosdesign(\beta, SPAN, SPS, SHAPE)$ 
Cioè $g_{T}(t) = h_{RRC}(t,\beta)$
$\beta$ è il roll-off, se è zero ho una rect. 
La risposta in frequenza sarà limitata perché lo spettro $H_{RC}(f,\beta)$ è un coseno rialzato. In matlab la sinc verrà tagliata ma si può fare senza troppo degrado dello spettro, il troncamento è dato da $SPAN$.
$SPS$ è il sampling time(sample per simbols), ad esempio $SPS = 4$ allora $\frac{T}{T_{s}} = 4$ con $f_{s}= \frac{1}{T_{s}}$.
L’$SPS$ minimo è 1 se il roll off è diverso da zero altrimenti è 2.

# Power of a pam simbol

Si integra la densità spettrale di potenza: $$P_{S}^{(BB)}) \frac{A}{T}\int_{-\infty}^{+\infty} H_{RC}(f,\alpha) df$$ ma visto che $\int_{-\infty}^{+\infty} H_{RC}(f,\alpha) df = h_{RC}(t,\alpha)|_{t=0} = 1$ quindi $$P_{S}= \frac{1}{2}P_{S}^{(BB)} = \frac{A}{2T}$$
L’energia per trasmettere un simbolo è $$E_{S}^{(BB)} = P^{(BB)}\cdot T$$
con $A = E[a_{i}^{2}] = \frac{M^{2}-1}{3}$ se $M$ è fissato abbiamo i simboli simmetrici ed equidistanti con una distanza pari a 2. 
L’energia necessaria per trasmettere un simbolo è: $$E_{S}^{(PB)}= P_{S}T = \frac{A}{2T}T = \frac{M^{2}-1}{6}$$
# Rumore additivo gaussiano bianco

Anche per il rumore dovremo calcolare la potenza.
$n(m)$ è una variabile aleatoria perché è ottenuta campionando il processo aleatorio quindi può essere scritto come:
$n(m) = n(t)|_{t=mT} = n_{I}(m)+jn_{Q}(m)$.
Per trovare la densità spettrale di potenza devo calcolare:
$\sigma^{2}_{n}= E[|n(m)|^{2}] = \int_{-\infty}^{+\infty} S_{n}(f) df = 2N_{0}\int_{-\infty}^{+\infty} |G_{R}(f)|^{2}df = 2N_{0}$
perché $\int |G_{R}(f|^{2}df = 1$.

# Strategia di Decisione

La variabile sulla quale bisogna prendere una decisione è $x(m) = a_{m}+n(m)$
$n(m) \thicksim \mathcal{N}(0,N_{0})$ rumore gaussiamo bianco

$x(m)$ è ottenuta facendo la somma di valori costanti più una variabile aleatoria e quindi anche $x(m)$ sarà una variabile aleatoria con media e varianza uguale a quella variabile aleatoria che fa parte della somma:  $x(m) \thicksim N(a_{m}, \sigma^{2})$

La decisione sarà $a_{m}=\text{arg max}_{a^{(i)}\in \mathcal{A}} p(a^{(i)}|x(m))$
ma per il teorema di Bayes: 
$p(a_{m}|x(m))p(x(m)) = p(x(m)|a_{m})p(a_{m})$

$$
\begin{align*}
p(a_{m}|x(m)) &= \frac{p(x(m)|a_{m})p(a_{m})}{p(x_{m})} =\\
&= \frac{1}{M}\frac{p(x(m)|a_{m})}{p(x_{m})}
\end{align*}
$$
quinid mi basta massimizzare $p(x(m))$.
$a_{m}$ simbolo trasmesso durante l’m-esimo intervallo.
$a^{(i)}$ identifica un simbolo specifico della costellazione.
Se $M = 2$ allora $a^{(0)} = -1$ e $a^{(1)} = 1$.
I simboli sono equiprobabili quindi la probabilità per simbolo è $\frac{1}{M}$.

$a_{m} = \text{arg max } p(x(m)|a_{m}=a^{(i)})$
che conosciamo perché $p(x(m)|a_{m}=a^{(i)}) = \frac{1}{\sqrt{2\pi}\sigma}\cdot e^{-\frac{x(m-a^{(i)})^{2}}{2\sigma^{2}}}$
e il valore massimo è quello che minimizza la distanza tra $x(m)$ e la distanza:
$a^{(i)} = \text{arg min} (x(m)-a^{(i)})^{2}$
Questo metodo si chiama **criterio di massima verosimiglianza**.
Abbiamo fatto però varie assunzioni:
1. il canale genera solo rumore gaussiano bianco
2. la convoluzione non genera interferenze
3. il ricevitore e il trasmettitore sono sincronizzati
4. i simboli sono equiprobabili

Praticamente, per decidere un simbolo si guarda in quale area “casca”, detto formalmente: 
$$
Z^{(i)}= \{x|d(x,a^{(i)}) < d(x,a^{(j)}), j\not =i, j=1,…,M\}
$$

# Probabilità di errore di una PAM

$a_{m} = 1$
$x(m) = -0.1 \Rightarrow a_{m} = -1$ 
$x(m) = a_{m}+n(m)$ se $n(m) = -1.1 \Rightarrow x(m) = -0.1$ che è un errore. 

$P(e|a^{(i)})$ probabilità di sbagliare un simbolo.
$P_{e} = \sum\limits P(e|a^{(i)})P(a^{(i)}) = \lim_{N_{s}\rightarrow \infty} \frac{N_{e}}{N_{s}}$ probabilità di errore. 
noi calcoleremo la prima perché è impossibile trasmettere infiniti simboli e calcolare il rapporto.
Inoltre i simboli sono equiprobabili quindi $P(a^{(i)})$ è la stessa.
$P_{e}= \frac{1}{M}\sum\limits_{i=0}^{M-1}P(e|a^{(i)})$
e quella probabilità non è difficile da calcolare, ad esempio:
$P(e|a^{(i)}=1)$ la calcolo con la Q-function calcolata nel valor medio meno il treshold fratto la deviazione standard. $$Q(\frac{m_{x}-t_{1}}{\sigma_{x}})$$
che nel nostro caso diventa $Q(\frac{1}{\sigma})$.
Se aumento la distanza tra simboli aumenta l’energia per trasmettere quel simbolo però commetto meno errori. 

$Q(x) = \int_{-\infty}^{x}e^{\frac{-t^{2}}{2}} dt$:
$Q(-\infty) = 0$
$Q(+\infty) = 1$
$Q(0) = 0.5$
$Q(-x) = 1-Q(x)$

$P_{e}^{2-PAM}=Q(\frac{1}{\sigma})$
$P_{e}^{4-PAM}=\frac{3}{2}Q(\frac{1}{\sigma})$

È più utile esprimere la probabilità di errore in termini di energia per simbolo $E_{s}$ ed $N_{0}$.
$E_{s}= \frac{M^{2}-1}{6}$
Se $M=2$ allora $E_{s}=\frac{1}{2} \Rightarrow 2E_{s} = 1$
$P_{e}^{2-PAM}= Q\left(\frac{1}{\sigma}\right)= Q(\sqrt{\frac{1}{\sigma^{2}}})= Q(\sqrt{\frac{2E_{s}}{N_{0}}})$

# Mappa di Gray e Bit Error Rate

È una strategia che mappa i bit in modo che i simboli corrispondenti siano tali che simboli adiacenti differiscano solo per un bit. 
Questa è una strategia iterativa:
Se $M=2 \Rightarrow \{0,1\}$ 
Se $M=4 \Rightarrow \{00,01,11,10\}$
È utile per calcolare la bit error probability (BER), per calcolarla dobbiamo prima calcolare l'energia necessaria per trasmettere un bit:
$$E_{b}=\frac{E_{s}}{log_{2}M}$$
È praticamente l'energia per simbolo diviso il numero di bit per simbolo.

Per calcolare la probabilità di errore per bit sfruttando la definizione di probabilità di errore per simbolo $P_{e}^{(s)}$.
Si trascura il caso in cui il bit ricevuto sia due o più regioni di decisione distante da quella corretta. In questo modo posso assumere che un errore sul simbolo corrisponde ad un errore sul bit. 
Quindi, le due assunzioni che si fanno sono:
1. Utilizzare la mappatura di gray
2. La potenza del rumore non è troppo elevata, quindi al massimo si sbaglia un bit
Sotto queste ipotesi posso scrivere:
$$
\begin{align*}
P_{e}^{(b)} &= \lim_{N^{(b)}\rightarrow \infty} \frac{N_{e}^{(b)}}{N^{(b)}} \approx \\
&\approx \lim_{N^{(s)}\rightarrow \infty} \frac{N_{e}^{(b)}}{log_{2}MN^{(s)}} =\\
&= \frac{1}{log_{2}M} \lim_{N^{(s)}\rightarrow \infty} \frac{N_{e}^{(s)}}{N^{(s)}} =\\
&= \frac{1}{log_{2}M}\cdot P_{e}^{(s)}
\end{align*}
$$
L'approssimazione è dovuta alla seconda assunzione in cui il rumore non è elevato.

Ora convertiamola in bit error probability:
$M=2 \Rightarrow \frac{1}{log_{2}2}= 1$ quindi $E_{s}= E_{b} \Rightarrow P_{e}^{(b)} = Q(\sqrt{\frac{2E_{b}}{N_{0}}})$
$M=4 \Rightarrow \frac{1}{log_{2}2}= 2$ quindi $E_{s}=2E_{b}$ perché un simbolo contiene 2 bit inoltre, $P_{e}^{(s)} = \frac{3}{2} Q\left(\sqrt{\frac{2E_{s}}{5N_{0}}}\right)$ e sostituendo $simbolo contiene 2 bit inoltre, $P_{e}^{(b)} \approx \frac{3}{2} Q\left(\sqrt{\frac{4E_{b}}{5N_{0}}}\right)$ 

# QAM

È lo stesso concetto della QAM vista in campo analogico ma applicata al campo digitale in cui trasmetteremo 2 segnali PAM.
$$s_{QAM}(t) = \sum\limits_{i}a_{i}g_{T}(t-iT) + j \sum\limits_{i}b_{i}g_{T}(t-iT)$$
Il primo è l'elemento in fase e il secondo è quello in quadratura. 
Sviluppando possiamo ottenere uno schema semplificato: $$s_{QAM}(t) = \sum\limits_{i}(a_{i}+jb_{i})g_{T}(t-iT)= \sum\limits_{i}c_{i}g_{T}(t-iT)$$
Schema a blocchi sulle slide. 

Il numero di simboli nella QAM è $M_{QAM}= M_{PAM}^{2}$.

$E\{c_{m}\} = E\{a_{m}+jb_{m}\} = E\{a_{m}\} + jE\{b_{m}\} = 0$ perché per design $a_{m}$ e $b_{m}$ sono equiprobabili e simmetrici quindi hanno valor medio nullo. 
Mean Square Value: 
$$
\begin{align*}
A &= E\{c_{m}\cdot c_{m}^{*}\} =\\[4pt]
&= E\{(a_{m}+jb_{m}) \cdot (a_{m}-jb_{m})\} =\\[4pt]
&= E\{a_{m}^{2}+ b_{m}^{2}\} =\\[4pt]
&= E\{a_{m}^{2}\} + E\{b_{m}^{2}\} =\\[4pt]
&= 2\cdot \frac{M_{PAM}^{2}-1}{3} =\\[4pt]
&= 2 \cdot \frac{M_{QAM}-1}{3}
\end{align*}
$$
L'energia per simbolo sarà: $$E_{s}= \frac{A}{2}= \frac{M_{QAM}- 1}{3}$$
Il grande vantaggio è che l'energia necessaria per trasmettere lo stesso quantitativo di dati è molto minore rispetto alla PAM. 

Calcoliamo la probabilità di errore.
Banda occupata dalla QAM rispetto alla PAM:
$S_{s}^{(PAM)}(f) = \frac{A}{T}|G_{T}(f)|^{2}$
$S_{s}^{(QAM)}(f) = \frac{A^{(QAM)}}{T}|G_{T}(f)|^{2}$
Non fa alcuna differenza sulla banda se abbiamo la stessa costellazione perché $A$ è solo un fattore moltiplicativo
Assumiamo di avere un filtro a coseno rialzato:
$B^{(PAM)} = \frac{1+\alpha}{T}= \frac{1+\alpha}{log_{2}M^{(PAM)}T_{b}} = \frac{1+\alpha}{log_{2}M}R_{b}$

Il simbolo (complesso) sul quale si vuole calcolare la probabilità di errore è:
$$
\begin{align*}
x(m) &= c_{m}+n(m) =\\[4pt]
&= (a_{m}+jb_{m})+(n_{I}(m)+jn_{Q}(m)) =\\[4pt]
&= a_{m}+n_{I}(m) +j(b_{m}+n_{Q}(m))
\end{align*}
$$
Ripasso sull'unione:
$A\cup B = A + B - A\cap B$
$P(A\cup B) = P(A) + P(B) - P(A\cap B)$
che è sempre positiva quindi posso scrivere:
$P(A\cup B) \le P(A)+P(B)$
che può essere usato come bound.

L'evento $\epsilon^{(i)} = \{\text{error}|c^{(i)}\}$ è l'evento in cui si commette un errore avendo trasmesso il simbolo $c^{(i)}$ e può essere calcolato come l'unione di due eventi:
$\epsilon_{I}^{(i)} = \{\text{errore nel canale I}|c^{(i)}\}$
$\epsilon_{Q}^{(i)} = \{\text{errore nel canale Q}|c^{(i)}\}$
da cui:
$\epsilon^{(i)} = \epsilon_{I}^{(i)}\cup \epsilon_{Q}^{(i)}$
il cui limite superiore è:
$P(e|c^{(i)}) = P(\epsilon^{(i)}) \le P(\epsilon_{I}^{(i)}) + $