# Processi Stocastici

## Tipi di Processi

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
# PAM

Vedremo la differenza tra la modulazione analogica e quella digitale, infatti la modulazione rimane analogica ma il segnale era originariamente digitale e solo dopo convertito.

Una sorgente genera i bit $d_{k}$ che vengono mappati nel mappatore in simboli $a_{i}$ che vengono usati per realizzare un segnale tempo continuo (analogico) di questo tipo: $$\tilde{s}_{PAM} (t) = \sum\limits_{i} a_{i} g_{T}(t-iT)$$E:
$$
\begin{align*}
s_{PAM}(t) &= Re\{\tilde{s}_{PAM}(t)e^{j2 \pi f_{c}t}\} \\
&= \tilde{s}_{PAM}(t)cos(2\pi f_{c}t)\\
\end{align*}$$
Da ora in poi useremo sempre l’inviluppo complesso ma per motivi di notazione rimuoveremo la tilde tenendo in mente che stiamo studiando l’equivalente in banda base.

Studiamo nel dettaglio i blocchi:
## Mappatore

Lo scopo del **mappatore** è convertire i bit $d_k \in \{0,1\}$ in simboli $a_i$. 
I bit $d_k$ possono essere sia 0 che 1, e la loro aspettativa matematica è $E[d_k] = \frac{1}{2}$. Per mantenere il valore medio a zero, il mappatore assegna 0 a -1 e 1 a +1, quindi i simboli $a_i$ appartengono all'insieme $\{-1,+1\}$.

I bit generati sono un processo stocastico discreto nel tempo e con stati discreti. Oltre a riportare la media a zero, il mappatore ha il compito di comprimere l'informazione, in quanto un insieme di bit può essere rappresentato da un singolo simbolo.

$M$ indica la dimensione della *costellazione dei simboli*. Nell'esempio precedente, $M = 2$, quindi il numero di bit per simbolo è $m = \log_2 M = 1$. Anche i simboli, come i bit, possono essere considerati processi stocastici, che spesso assumiamo essere stazionari e indipendenti.

Un'altra funzione importante del mappatore è ottimizzare l'uso della banda occupata. La sorgente genera bit con una velocità (rate) di trasmissione $R_b = \cfrac{1}{T_b}$, dove $T_b$ è il tempo tra due bit consecutivi. Se un simbolo rappresenta più di un bit, l'efficienza spettrale aumenta, cioè si trasmette più informazione con meno banda. Il tempo associato a ciascun simbolo è $T = m T_b$, e la velocità di trasmissione dei simboli è $R = \cfrac{1}{T} = \cfrac{1}{m T_b} = \cfrac{R_b}{m} = \cfrac{R_b}{\log_2 M}$.

**L'efficienza spettrale** può essere valutata considerando la banda occupata $B$, che è inversamente proporzionale alla durata di un simbolo: $B \propto \cfrac{1}{T}$. Di conseguenza, aumentando $M$, la banda occupata diminuisce, migliorando l'efficienza complessiva.

### Efficienza Energetica e Spettrale

L'efficienza energetica e l'efficienza spettrale sono due concetti spesso in contrasto tra loro. Ad esempio, la modulazione FM (Frequency Modulation) è buona in termini di efficienza energetica, ma non in termini di efficienza spettrale. In generale, un miglioramento nell'efficienza spettrale porta a una riduzione dell'efficienza energetica, e viceversa. Questo avviene anche in tecniche come la PAM (Pulse Amplitude Modulation).

#### Calcolo dell'energia minima per trasmettere un bit

Il segnale PAM può essere espresso come:
$$ s_{PAM}(t) = \sum\limits_{i} a_{i} g_{T}(t - iT) $$
Dove:
- $a_i$ sono i simboli PAM.
- $g_T(t)$ è la forma d'onda del simbolo.
- $T$ è il periodo simbolo.

Questo segnale è continuo nel tempo e nello stato. Tuttavia, non è stazionario, bensì ciclostazionario, poiché la stazionarietà (cioè le proprietà statistiche) è periodica con periodo $T$. Inoltre, i campioni non sono più indipendenti tra loro: il prossimo campione dipenderà dal precedente.

La funzione di autocorrelazione per i simboli $a_i$ è:
$$ R_{a}(\tau) = E[a^2]\delta(\tau) $$
Dopo il campionamento, la funzione di autocorrelazione discreta diventa:
$$ R_{a}[\tau] = E[a^2]\delta[\tau] $$
La densità spettrale di potenza dei simboli è:
$$ S_{a}(f) = E[a^2] $$
Quando il segnale passa attraverso un filtro lineare, la densità spettrale del segnale filtrato è:
$$ S_{s}(f) = \frac{1}{T} S_{a}(f) |G_T(f)|^2 $$
Dove $G_T(f)$ è la trasformata di Fourier della forma d'onda $g_T(t)$.

Infine, definiamo l'energia dei simboli come:
$$ A = E[a_i^2] $$
E, in questo contesto, assumiamo che la media dei simboli sia nulla:
$$ E[a_i] = 0 $$
Questa assunzione facilita i calcoli e riflette che il segnale è bilanciato intorno a zero.

Immaginiamo di avere un segnale e di voler capire come si comporta nel tempo e nello spettro delle frequenze. La **funzione di autocorrelazione** misura quanto il segnale è simile a se stesso in momenti diversi, ma non ci dice molto su come il segnale si distribuisce in frequenza. Per questo, usiamo la **densità spettrale di potenza**, che ci mostra quanta energia del segnale è presente a diverse frequenze.

Questa densità spettrale dipende dal tipo di filtro che applichiamo al segnale. Scegliendo un filtro diverso, otteniamo una diversa distribuzione dell'energia sulle frequenze. Un esempio è il filtro `rect`, che ha una forma a rettangolo. In frequenza, questo filtro si comporta come una funzione `sinc` (che si estende molto in frequenza), quindi ha uno spettro ampio, cioè occupa molte frequenze.

Se invece vogliamo limitare lo spettro, possiamo scegliere un altro tipo di filtro che ha uno spettro più stretto. Ad esempio, il filtro rettangolare in frequenza ($G_T(f) = rect(fT)$) produce una funzione `sinc` in tempo. Il trucco è trovare un equilibrio tra il tipo di segnale che vogliamo trasmettere e quanta banda (frequenza) vogliamo occupare: si cerca un compromesso.

#### Segnale attraverso un canale

Ora vediamo cosa succede quando il segnale viaggia attraverso un canale. 
Il canale agisce come un **filtro lineare** $h(t)$, che può modificare il segnale in modo prevedibile. Quindi, il segnale che riceviamo, $y(t)$, è il risultato del segnale trasmesso $s(t)$ convoluto con l’effetto del canale. Matematicamente, scriviamo questa operazione come:
$$ y(t) = s(t) \circledast h(t) $$
In frequenza, diventa una moltiplicazione:
$$ Y(f) = S(f) \cdot H(f) $$
Se il canale non altera il segnale, possiamo dire che $h(t) = \delta(t)$, che è come se il segnale passasse attraverso il canale senza essere toccato.

Ma c'è un'altra cosa da considerare: *il rumore*. Quando il segnale attraversa il canale, c'è sempre del **rumore termico** che interferisce. Questo rumore può essere modellato come un *rumore bianco gaussiano* (una distribuzione normale con media zero). Ha una densità spettrale di potenza costante:
$$ S_w(f) = \frac{N_0}{2} $$
Il rumore è sempre presente, ma possiamo ridurre i suoi effetti con buoni filtri e tecniche di elaborazione.

#### Struttura della PAM in ricezione

Il segnale trasmesso è ricevuto come $r(t)$, che è il segnale originale più il rumore. Consideriamo che questo segnale si trovi in banda base, cioè non è modulato a frequenze alte, ma è già pronto per essere elaborato.

Il primo passo nel ricevitore è far passare il segnale attraverso un **filtro passa basso** $g_R(t)$. Questo filtro serve a eliminare tutte le componenti di frequenza che non ci interessano. Quindi, il segnale ricevuto è dato da:
$$ r(t) = s(t) \circledast h(t) + w(t) = s(t) + w(t) $$
In parole semplici, questo significa che il segnale $r(t)$ è una combinazione del segnale trasmesso $s(t)$, l'effetto del canale $h(t)$ (che qui trascuriamo) e del rumore $w(t)$.

Dopo il filtro, l'output è:
$$ x(t) = r(t) \circledast g_R(t) = s(t) \circledast g_R(t) + w(t) \circledast g_R(t) $$
Questo output può essere riscritto come:
$$ x(t) = \sum\limits_{i}a_{i} g(t - iT) + n(t) $$
Dove $g(t)$ è la combinazione del filtro del trasmettitore $g_T(t)$ e del filtro del ricevitore $g_R(t)$, cioè:
$$ g(t) = g_T(t) \circledast g_R(t) $$
L'unico elemento che disturba il segnale è il rumore $n(t) = w(t) \circledast g_R(t)$, che affronteremo più avanti.

Dopo il filtro, c'è il **campionatore**, che ha il compito di prendere un campione del segnale a intervalli regolari, precisamente ai tempi $t = mT$, dove $T$ è il periodo simbolo. Il campionatore ci fornisce:
$$ x(mT) = \sum\limits_{i} a_{i} g(mT - iT) + n(mT) $$
A questo punto, facciamo un cambio di variabile, ponendo $m - i = l$, quindi otteniamo:
$$ \sum\limits_{l} a_{m-l} g(lT) + n[m] $$
Il termine $a_m g(0)$ è il simbolo desiderato, mentre i termini successivi rappresentano l'**interferenza intersimbolica (ISI)**, cioè l'effetto dei simboli vicini che disturbano il simbolo attuale.

L'ISI si verifica quando il filtro $g_T(t)$ non ha una forma perfetta, come ad esempio una rect, che avrebbe eliminato l'ISI perché le sinc si annullano nei multipli del periodo $T$. In pratica, il filtro ideale sarebbe un filtro che fa in modo che $g(lT) = 1$ quando $l = 0$ (cioè quando si tratta del simbolo desiderato) e $g(lT) = 0$ per tutti gli altri $l \neq 0$, in modo da evitare che i simboli vicini interferiscano.

Il problema della rect è che occupa uno spettro di frequenze illimitato, il che non è pratico. Quindi, in realtà, si tollera una certa ISI. Tuttavia, la condizione per eliminare l'ISI è quella descritta sopra, e per far sì che il ricevitore funzioni correttamente, il trasmettitore e il ricevitore devono essere **perfettamente sincronizzati**. Se non lo sono, anche con un buon filtro, ci sarà comunque disturbo.

La condizione di **Nyquist** nel dominio delle frequenze ci aiuta a evitare l'**interferenza intersimbolica (ISI)**. Utilizzando Nyquist, possiamo esprimere il segnale $g(t)$ nel dominio della frequenza come:
$$ F(g(l)) = \sum\limits_{l} g(l) e^{-j2\pi flT} = \frac{1}{T} \sum\limits_{k} G(f - \frac{k}{T}) $$
Qui, scegliamo la funzione $g(t)$ in modo tale che la sua trasformata $G(f)$ sia pari a 1 nelle bande di frequenza di interesse. Questo ci garantisce che il segnale abbia un periodo $T$ che soddisfi le condizioni di Nyquist e che riduca al minimo l'ISI.

#### Coseno rialzato e banda

Un filtro comunemente utilizzato è il **coseno rialzato**, che ha una banda di frequenza definita come:
$$ B_{RC} = \frac{1 + \alpha}{2T} $$
Dove $\alpha$ è il **fattore di roll-off**. Se $\alpha = 0$, otteniamo un filtro `rect`, che ha un comportamento ideale in termini di riduzione dell'ISI, ma occupa una banda infinita (che non è praticabile). Quando $\alpha$ aumenta, il filtro ha una banda più larga, ma diminuisce l'occupazione spettrale.

#### Rumore bianco additivo

Non dobbiamo dimenticare che nel canale c'è sempre del **rumore bianco additivo**. Il rumore $w(t)$ rappresenta il rumore gaussiano bianco, che ha una potenza distribuita uniformemente su tutte le frequenze. Tuttavia, quando filtriamo il segnale, il rumore che ci rimane in banda base ha una densità spettrale di potenza data da:
$$ S_{\tilde{w}}(f) = 2N_0 $$
Questo significa che in banda base, dopo il filtro, il rumore non è più perfettamente bianco, ma dipende dal filtro utilizzato. La densità spettrale del rumore filtrato $n(t)$ è:
$$ S_{n}(f) = S_{w}(f) |G_{R}(f)|^2 $$
Poiché $|G_{R}(f)|^2$ non è costante, il rumore risultante non è più uniforme, ma varia in base alla frequenza del filtro.

Per ridurre al massimo l'effetto del rumore, scegliamo un buon filtro. Una strategia è utilizzare ancora una volta un filtro **coseno rialzato**, ma in questo caso nella sua forma adattata, cioè:
$$ H_{RRC}(f, \alpha) = \sqrt{H_{RC}(f, \alpha)} $$
Questo filtro è scelto in modo che il filtro di ricezione $G_R(t)$ sia uguale a quello di trasmissione $G_T(t)$, garantendo una perfetta corrispondenza tra trasmettitore e ricevitore e migliorando la capacità di filtrare il rumore.

#### Banda della PAM

Infine, possiamo definire la banda occupata dalla modulazione PAM. La banda della PAM a banda passante (PB) è il doppio di quella a banda base (BB) e viene data da:
$$ B_{PAM}^{(PB)} = 2 B_{PAM}^{(BB)} = \frac{1 + \alpha}{T} = (1 + \alpha) \frac{R_b}{\log_2 M} $$
Qui, $R_b$ è la velocità di trasmissione dei bit, $M$ è il numero di simboli nella costellazione, e $\alpha$ è il fattore di roll-off del filtro coseno rialzato. Questo ci fornisce una misura diretta della banda necessaria per trasmettere i dati usando la modulazione PAM.

### MATLAB RRC Filter Design

Innanzitutto capiamo come è fatto un filtro a coseno rialzato in matlab. 
$g = rcosdesign(\beta, SPAN, SPS, SHAPE)$ 
Cioè $g_{T}(t) = h_{RRC}(t,\beta)$
$\beta$ è il roll-off, se è zero ho una rect. 
La risposta in frequenza sarà limitata perché lo spettro $H_{RC}(f,\beta)$ è un coseno rialzato. In matlab la sinc verrà tagliata ma si può fare senza troppo degrado dello spettro, il troncamento è dato da $SPAN$.
$SPS$ è il sampling time(sample per simbols), ad esempio $SPS = 4$ allora $\frac{T}{T_{s}} = 4$ con $f_{s}= \frac{1}{T_{s}}$.
L’$SPS$ minimo è 1 se il roll off è diverso da zero altrimenti è 2.

### Potenza di una PAM

Quando abbiamo un segnale che occupa una certa banda di frequenze, possiamo calcolare la potenza totale del segnale integrando la sua densità spettrale su tutte le frequenze.
In questo caso, l'integrazione della densità spettrale di potenza a banda base ($P_S^{(BB)}$) si esprime come:
$$ P_S^{(BB)} = \frac{A}{T} \int_{-\infty}^{+\infty} H_{RC}(f, \alpha) df $$
Qui, $H_{RC}(f, \alpha)$ rappresenta la risposta in frequenza del filtro coseno rialzato, con $\alpha$ che è il fattore di roll-off. L'integrale di questa funzione su tutte le frequenze dà 1, quindi possiamo semplificare il calcolo della potenza utilizzando il valore della funzione al tempo $t=0$:
$$ \int_{-\infty}^{+\infty} H_{RC}(f, \alpha) df = h_{RC}(t, \alpha)|_{t=0} = 1 $$
Di conseguenza, la potenza complessiva a banda passante ($P_S$) è la metà di quella a banda base:
$$ P_S = \frac{1}{2} P_S^{(BB)} = \frac{A}{2T} $$
### Energia per trasmettere un simbolo

L'energia necessaria per trasmettere un simbolo a banda base è data dalla potenza a banda base moltiplicata per il tempo del simbolo $T$:
$$ E_S^{(BB)} = P_S^{(BB)} \cdot T $$
Ora, per il caso di simboli simmetrici ed equidistanti (come nella modulazione PAM), possiamo definire l'energia media dei simboli, dove $A = E[a_i^2]$ rappresenta l'energia media dei simboli. Se $M$ è il numero di simboli nella costellazione, possiamo calcolare $A$ come:
$$ A = \frac{M^2 - 1}{3} $$
Questo valore viene dalla somma delle potenze dei simboli nel caso di una costellazione simmetrica ed equidistante, con una distanza tra i simboli pari a 2.

### Energia per trasmettere un simbolo a banda passante

Infine, l'energia necessaria per trasmettere un simbolo a banda passante si ottiene moltiplicando la potenza a banda passante per il tempo del simbolo $T$:
$$ E_S^{(PB)} = P_S T = \frac{A}{2T} T = \frac{M^2 - 1}{6} $$
Questo risultato mostra quanta energia serve per trasmettere un simbolo, tenendo conto della potenza del segnale e della struttura della costellazione dei simboli.
### Potenza del rumore

Il rumore in un ricevitore digitale è una *variabile aleatoria* che cambia con il tempo. Quando campioniamo il segnale, anche il rumore viene campionato, quindi $n(m)$ rappresenta il rumore al momento $t = mT$.

Il rumore campionato può essere rappresentato come:
$$ n(m) = n(t)|_{t=mT} = n_I(m) + jn_Q(m) $$
Qui, $n_I(m)$ è la componente in fase e $n_Q(m)$ è la componente in quadratura del rumore.

Per misurare la potenza del rumore, dobbiamo calcolare la **varianza** del rumore campionato, $\sigma^2_n$. La varianza rappresenta l'energia media del rumore e può essere trovata tramite l'**aspettativa del valore quadrato del rumore**:
$$ \sigma^2_n = E[|n(m)|^2] $$
Questo è equivalente all'integrazione della **densità spettrale di potenza** del rumore $S_n(f)$ su tutte le frequenze:
$$ \sigma^2_n = \int_{-\infty}^{+\infty} S_n(f) df $$
La densità spettrale di potenza del rumore dopo il filtro passa-basso è data da:
$$ S_n(f) = 2N_0 |G_R(f)|^2 $$
Dove $N_0$ è la densità spettrale di potenza del rumore bianco e $G_R(f)$ è la risposta in frequenza del filtro di ricezione. Integrando questa espressione:
$$ \sigma^2_n = 2N_0 \int_{-\infty}^{+\infty} |G_R(f)|^2 df $$
Poiché il filtro è normalizzato in modo che l'integrale della sua risposta in frequenza $|G_R(f)|^2$ su tutte le frequenze sia 1:
$$ \int_{-\infty}^{+\infty} |G_R(f)|^2 df = 1 $$
Otteniamo:
$$ \sigma^2_n = 2N_0 $$
Questo significa che la **varianza del rumore** campionato è proporzionale alla densità spettrale di potenza del rumore bianco e al fattore 2, che tiene conto delle componenti in fase e in quadratura.

## Decisore

Immagina di dover prendere una decisione su quale simbolo è stato trasmesso, basandoti su un segnale ricevuto $x(m)$. Il segnale ricevuto è composto dal simbolo trasmesso $a_m$ più del rumore $n(m)$, che è un rumore gaussiano bianco, indicato come $\mathcal{N}(0, N_0)$.

Quindi, la variabile $x(m)$ rappresenta la somma di un valore fisso (il simbolo trasmesso) e una variabile aleatoria (il rumore). Questo significa che anche $x(m)$ sarà una variabile aleatoria, distribuita in modo normale con media pari al simbolo trasmesso $a_m$ e varianza pari a quella del rumore:
$$ x(m) \thicksim \mathcal{N}(a_m, \sigma^2) $$
Il nostro obiettivo è decidere quale simbolo è stato trasmesso durante l'intervallo $m$. Per fare questo, usiamo una regola basata sulla probabilità. Vogliamo trovare il simbolo $a_m$ che massimizza la probabilità che quel simbolo sia stato trasmesso, dato il valore di $x(m)$. Questo viene formalizzato come:
$$ a_m = \text{arg max}_{a^{(i)} \in \mathcal{A}} p(a^{(i)} | x(m)) $$
Per semplificare il calcolo, applichiamo il **teorema di Bayes**, che ci dice che:
$$ p(a_m | x(m)) p(x(m)) = p(x(m) | a_m) p(a_m) $$
Se i simboli sono tutti *equiprobabili* (cioè, ognuno ha la stessa probabilità di essere trasmesso), possiamo ignorare $p(a_m)$ e ci basta massimizzare la probabilità $p(x(m) | a_m)$. In altre parole, scegliamo il simbolo $a_m$ che rende più probabile il valore di $x(m)$.

Per esempio, se $M = 2$, abbiamo due simboli possibili: $a^{(0)} = -1$ e $a^{(1)} = 1$. La probabilità condizionata $p(x(m) | a_m = a^{(i)})$ segue una distribuzione normale, che conosciamo:
$$ p(x(m) | a_m = a^{(i)}) = \frac{1}{\sqrt{2\pi} \sigma} \cdot e^{-\frac{(x(m) - a^{(i)})^2}{2\sigma^2}} $$
Il simbolo più probabile sarà quello che minimizza la distanza tra $x(m)$ e il simbolo $a^{(i)}$. Quindi scegliamo il simbolo che minimizza l'espressione:
$$ a^{(i)} = \text{arg min} (x(m) - a^{(i)})^2 $$
Questo metodo è chiamato **criterio di massima verosimiglianza** perché sceglie il simbolo che rende il valore di $x(m)$ più probabile.

Ci sono però alcune **assunzioni** che stiamo facendo:
1. Il canale introduce solo rumore gaussiano bianco.
2. La convoluzione del segnale con il canale non genera interferenze.
3. Il ricevitore e il trasmettitore sono perfettamente sincronizzati.
4. I simboli trasmessi sono equiprobabili (hanno la stessa probabilità di essere scelti).

In pratica, per decidere quale simbolo è stato trasmesso, controlliamo in quale "area" cade $x(m)$. Ogni simbolo ha una regione associata, definita come:
$$ Z^{(i)} = \{ x \mid d(x, a^{(i)}) < d(x, a^{(j)}), \text{ per ogni } j \neq i \} $$
Quindi, scegliamo il simbolo la cui distanza da $x(m)$ è la più piccola rispetto agli altri simboli.
## Probabilità di errore di una PAM

Immagina di trasmettere un simbolo $a_m = 1$, ma a causa del rumore, il segnale ricevuto $x(m)$ risulta diverso. Ad esempio, se il rumore $n(m) = -1.1$, allora il segnale ricevuto sarà:
$$ x(m) = a_m + n(m) = 1 + (-1.1) = -0.1 $$
Questo valore di $x(m)$ potrebbe indurre il ricevitore a decidere che è stato trasmesso $a_m = -1$, il che costituisce un errore.

Ora, vogliamo capire qual è la probabilità di commettere un errore quando si trasmette un simbolo, chiamata **probabilità di errore simbolo**. Questa probabilità viene indicata come $P(e|a^{(i)})$, cioè la probabilità di sbagliare dato che il simbolo trasmesso è $a^{(i)}$. Per calcolare la probabilità complessiva di errore, sommiamo queste probabilità per tutti i simboli trasmessi, ponderate per la probabilità che ogni simbolo venga effettivamente trasmesso.

Dato che non possiamo trasmettere infiniti simboli e calcolare direttamente la probabilità di errore (come sarebbe il caso ideale), usiamo la seguente formula per stimare la **probabilità di errore complessiva**:
$$ P_e = \frac{1}{M} \sum\limits_{i=0}^{M-1} P(e | a^{(i)}) $$
Qui $M$ è il numero di simboli nella costellazione, e poiché i simboli sono **equiprobabili** (cioè, tutti hanno la stessa probabilità di essere trasmessi), possiamo semplificare il calcolo.

### Funzione Q

La **probabilità di errore** per ogni simbolo si può calcolare usando la **funzione Q**. La funzione Q rappresenta la probabilità che una variabile casuale con distribuzione normale superi un certo valore soglia. Per calcolare $P(e|a^{(i)} = 1)$, usiamo:
$$ Q\left( \frac{m_x - t_1}{\sigma_x} \right) $$
Dove:
- $m_x$ è la media del segnale,
- $t_1$ è la soglia di decisione (il valore al quale decidiamo tra simboli),
- $\sigma_x$ è la deviazione standard del rumore.

Nel nostro caso, la media $m_x = 1$ e la soglia è zero, quindi la formula diventa:
$$ P(e|a^{(i)} = 1) = Q\left( \frac{1}{\sigma} \right) $$
Se aumentiamo la distanza tra i simboli trasmessi (cioè facciamo sì che i simboli siano più distanti tra loro), riduciamo la probabilità di errore, perché è meno probabile che il rumore porti a una decisione errata. Tuttavia, aumentare la distanza tra i simboli richiede più energia per trasmetterli.

La funzione Q ha alcune proprietà utili da ricordare:
- $Q(-\infty) = 0$ (nessuna probabilità di errore per valori estremamente negativi),
- $Q(+\infty) = 1$ (certezza di errore per valori estremamente positivi),
- $Q(0) = 0.5$ (probabilità del 50% di errore a metà del range),
- $Q(-x) = 1 - Q(x)$.

### Probabilità di errore per 2-PAM e 4-PAM

Nel caso di modulazioni a 2 livelli (2-PAM), la probabilità di errore è data da:
$$ P_e^{2-PAM} = Q\left( \frac{1}{\sigma} \right) $$
Per la modulazione a 4 livelli (4-PAM), la probabilità di errore è leggermente più alta:
$$ P_e^{4-PAM} = \frac{3}{2} Q\left( \frac{1}{\sigma} \right) $$
### Probabilità di errore in termini di energia

È utile esprimere la probabilità di errore in termini di energia per simbolo $E_s$ e densità spettrale del rumore $N_0$. Per 2-PAM, l'energia per simbolo è:
$$ E_s = \frac{M^2 - 1}{6} $$
Se $M = 2$, allora:
$$ E_s = \frac{1}{2} $$
La probabilità di errore diventa quindi:
$$ P_e^{2-PAM} = Q\left( \sqrt{\frac{2 E_s}{N_0}} \right) $$
Questo ci dice che la probabilità di errore dipende dalla **relazione tra l'energia del simbolo** e il rumore di fondo $N_0$.

## Mappa di Gray e Bit Error Rate

Questo paragrafo descrive una strategia utilizzata per ridurre gli errori durante la trasmissione dei bit, chiamata **mappatura di Gray**. Questa tecnica associa ai simboli trasmessi delle sequenze di bit in modo che i simboli adiacenti differiscano solo per un bit. Questo è utile perché se un errore si verifica durante la trasmissione, è più probabile che coinvolga solo un bit, riducendo la probabilità di errore globale.

Vediamo come funziona questa mappatura:
- Se $M = 2$, abbiamo solo due simboli possibili: $\{0, 1\}$.
- Se $M = 4$, possiamo avere quattro simboli $\{00, 01, 11, 10\}$, dove notiamo che ogni simbolo differisce da quello vicino solo per un bit.

### Energia per bit

Per calcolare l'energia necessaria a trasmettere un singolo bit, partiamo dall'**energia per simbolo**, $E_s$, e la dividiamo per il numero di bit trasmessi per simbolo, che è $\log_2 M$:
$$ E_b = \frac{E_s}{\log_2 M} $$
L'energia per bit $E_b$ rappresenta quanta energia è necessaria per trasmettere ogni singolo bit del simbolo.

### Probabilità di errore per bit (BER)

Per determinare la **bit error probability** (BER), usiamo la probabilità di errore per simbolo $P_e^{(s)}$. In pratica, si assume che un errore sul simbolo comporti un errore su un solo bit. Questo si basa su due assunzioni:
1. Si utilizza la mappatura di Gray, dove i simboli vicini differiscono solo per un bit.
2. La potenza del rumore non è troppo elevata, quindi al massimo si sbaglia solo un bit.

Con queste assunzioni, la probabilità di errore per bit è approssimativamente:
$$ P_e^{(b)} \approx \frac{1}{\log_2 M} P_e^{(s)} $$
Questa formula mostra che la probabilità di errore per bit è legata alla probabilità di errore per simbolo, ma normalizzata in base al numero di bit per simbolo.

### Esempi pratici

1. **Caso $M = 2$**: Se abbiamo solo due simboli, allora $\log_2 2 = 1$, quindi l'energia per bit è uguale all'energia per simbolo, $E_b = E_s$. In questo caso, la probabilità di errore per bit diventa:
   $$ P_e^{(b)} = Q\left(\sqrt{\frac{2E_b}{N_0}}\right) $$
2. **Caso $M = 4$**: Se abbiamo quattro simboli (quindi due bit per simbolo), $\log_2 4 = 2$, quindi l'energia per simbolo è il doppio dell'energia per bit, $E_s = 2E_b$. La probabilità di errore per simbolo $P_e^{(s)}$ è leggermente più complessa e viene espressa come:
   $$ P_e^{(s)} = \frac{3}{2} Q\left(\sqrt{\frac{2E_s}{5N_0}}\right) $$
   Sostituendo $E_s = 2E_b$, otteniamo la probabilità di errore per bit:
   $$ P_e^{(b)} \approx \frac{3}{2} Q\left(\sqrt{\frac{4E_b}{5N_0}}\right) $$
In sintesi, la **mappatura di Gray** aiuta a ridurre gli errori trasmettendo i bit in modo che i simboli adiacenti differiscano solo per un bit, il che riduce la possibilità di errori gravi. La probabilità di errore per bit è poi legata alla probabilità di errore per simbolo e all'energia utilizzata per trasmettere ogni bit.

# QAM

La modulazione **QAM (Quadrature Amplitude Modulation)** è una tecnica molto simile alla modulazione analogica vista con la QAM, ma qui applicata in campo digitale. La modulazione QAM combina due segnali **PAM**, uno in fase e uno in quadratura, che possiamo rappresentare come:
$$ s_{QAM}(t) = \sum\limits_{i} a_{i} g_T(t - iT) + j \sum\limits_{i} b_{i} g_T(t - iT) $$
- Il primo termine, $a_i$, rappresenta la componente **in fase**.
- Il secondo termine, $b_i$, rappresenta la componente **in quadratura**.

Sviluppando ulteriormente, possiamo semplificare l’espressione come:
$$ s_{QAM}(t) = \sum\limits_{i} (a_i + j b_i) g_T(t - iT) = \sum\limits_{i} c_i g_T(t - iT) $$
Dove $c_i$ è il simbolo complesso che combina le componenti in fase e in quadratura.

## Numero di simboli nella QAM

Il numero di simboli nella costellazione QAM è $M_{QAM}$, che è il quadrato del numero di simboli della modulazione PAM, ossia:
$$ M_{QAM} = M_{PAM}^2 $$
### Energia del simbolo

L’energia media di un simbolo $c_m$ è calcolata considerando le componenti in fase e in quadratura. 
Poiché $a_m$ e $b_m$ sono distribuiti in modo simmetrico ed equiprobabile, il loro valor medio è zero, e possiamo calcolare il **Mean Square Value**:
$$
\begin{align*}
A &= E\{ c_m \cdot c_m^{*} \} = E\{ (a_m + j b_m) \cdot (a_m - j b_m) \} \\
  &= E\{ a_m^2 + b_m^2 \} \\
  &= E\{ a_m^2 \} + E\{ b_m^2 \} \\
  &= 2 \cdot \frac{M_{PAM}^2 - 1}{3} = 2 \cdot \frac{M_{QAM} - 1}{3}
\end{align*}
$$
L'**energia per simbolo** per la modulazione QAM diventa quindi:
$$ E_s = \frac{A}{2} = \frac{M_{QAM} - 1}{3} $$
## Vantaggio della QAM

Uno dei grandi vantaggi della QAM rispetto alla PAM è che l'energia necessaria per trasmettere la stessa quantità di dati è inferiore. Questo significa che la QAM è più efficiente dal punto di vista energetico.

## Probabilità di errore e banda occupata

La banda occupata dalla QAM è la stessa della PAM se si utilizza la stessa costellazione. La densità spettrale di potenza per la QAM è data da:
$$ S_s^{(QAM)}(f) = \frac{A^{(QAM)}}{T} |G_T(f)|^2 $$
E se si utilizza un filtro a coseno rialzato, la banda sarà:
$$ B^{(PAM)} = \frac{1 + \alpha}{T} = \frac{1 + \alpha}{\log_2 M^{(PAM)} T_b} = \frac{1 + \alpha}{\log_2 M} R_b $$
Dove $\alpha$ è il fattore di roll-off, e $R_b$ è il rate di trasmissione.

## Probabilità di errore simbolo

Per calcolare la **probabilità di errore simbolo** della QAM, dobbiamo considerare che il simbolo complesso ricevuto è dato da:
$$ x(m) = c_m + n(m) = (a_m + j b_m) + (n_I(m) + j n_Q(m)) $$
Quindi, l'errore può verificarsi sia nella componente in fase che in quella in quadratura. Poiché le due componenti del rumore sono indipendenti, possiamo calcolare la probabilità di errore totale come la somma delle probabilità di errore delle due componenti:
$$ P(e | c^{(i)}) \leq P(\epsilon_I^{(i)}) + P(\epsilon_Q^{(i)}) $$
Per la modulazione 4-QAM, la probabilità di errore simbolo è approssimativamente:
$$ P_e^{(4-QAM)} = 2 Q\left( \frac{1}{\sigma} \right) = 2 P_e^{(2-PAM)} $$
La **probabilità di errore simbolo** per una modulazione QAM è:
$$ P_e^{(QAM)} = 2 Q\left( \sqrt{\frac{E_s}{N_0}} \right) $$
## Probabilità di errore bit (BER)

Infine, la **bit error probability** (BER) per la modulazione QAM, assumendo la mappatura di Gray, si calcola come:
$$ P_e^{(M-QAM, b)} = \frac{1}{\log_2 M} P_e^{(M-QAM)} $$
Per la modulazione $\sqrt{M}$-PAM, possiamo esprimere la BER come:
$$ P_e^{(\sqrt{M}-PAM, b)} = P_e^{(M-QAM, b)} $$
