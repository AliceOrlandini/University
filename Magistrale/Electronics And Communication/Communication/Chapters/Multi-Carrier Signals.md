
Questa tecnologia è la soluzione alla frequency selecting fading, è alla base dei sistemi LTE, 5G e Wi-Fi. 

Il primo motivo per cui viene impiegata in queste applicaizoni sono:
1. Robustezza
2. Efficienza spettrale
3. Flessibile riguardo all’occupazione di risorse OFDMA

# OFDM technology

Quando abbiamo il frequency selective fading, la probabilità di errore $P_{e}$ decresce con l’SNR fino ad un asintoto orizzontale dato dall’error floor.
$B_{s}>B_{c}\approx \frac{1}{5\cdot6\tau}$
Dividiamo la banda del segnale originale in $N$ parti che occupano meno banda della cohearence bandwidth: $\frac{B_{s}}{N}<B_{c}$.
In questo modo si risolve il problema del frequency selecting fading. 
Un modo per fare la divisione della banda del segnale è usare dei filtri passa banda e poi sommare i segnali generati. Questo però è molto complicato da fare perché i filtri non saranno mai perfettamente ortogonali quindi ci sarà interferenza non essendo molto precisi.

Facciamo un passo indietro al multipath propagation channel:
il segnale generato è la somma delle repliche del segnale $$h(t) = A_{LS}\sum\limits_{l=0}^{N_{C}-1}\alpha_{l}e^{j\phi_{l}}\delta(t-\tau_{l})$$ che ha come trasformata: $$H(f) = \sum\limits \alpha_{l}e^{j\phi_{l}}e^{-j2\pi f\tau_{l}}$$ e $Y(t) = H(f)\cdot S(f)$ con $S(f)$ il filtro applicato.
Ciò che si vuol sapere è solo il segnale filtrato, visto che tutto ciò che è all’esterno verrà tagliato. La banda del filtro sarà $B_{S}\approx \frac{1}{T}$
Portando il segnale in banda base si vede che il sampling time (tempo di campionamento) sarà $T$. (grafico sulle slide)
Campioniamo solo l’equivalente in banda base $$h_{eq}(t) = \sum\limits_{l=0}^{L-1}h(l)\delta(t-lT)$$
e il segnale sarà: $$y(t) = $$
avrò un solo campionamneto ogni $T$ secondi che è molto più semplice di utilizzare la rappresentazione fisica del segnale (?).

Consideriamo cosa otteniamo all’output del canale, sarà la somma del segnale desiderato $s(k)$ più dell’interferenza: $$y(k) = $$ i samples vanno da zero a $N-1$ per un totale di $N$ samples. L’output può essere quindi calcolato nel seguente modo: $$y(k) = \sum\limits_{l=0}^{L-1}h(l)s(k-l) = h(0)s(k) + …+h(L-1)s(k-L+1)$$
Quando: $k=0 \Rightarrow y(0) = h(0)s(0)+h(1)s(-1)+…+h(L-1)s(-L+1) = s(0)h(0)$ questo per l’operazione di convoluzione (?). Ma i termini a partire da $s(-1)$ sono tutti zero quindi l’espressione si riduce. 
$k=1 \Rightarrow y(1) = h(0)s(1)+h(1)s(0)+h(2)s(-1) + … = h(0)s(1) + h(1)s(0)$
e così via…

Troviamo una rappresentazione matriciale dell’operazione $y = \mathcal{H}s$ che avrà $N\text{x}N$: (matrice sulle slide)
Ogni diagonale avrà sempre lo stesso valore il valore di $h(k)$
La matrice $\mathcal{H}$ è chiamata **Matrice di Toeplitz** e vogliamo renderla circolare (non ho capito perché) cycling extentions. 

Ricordiamoci che stiamo inviando un vettore di $N$ samples quindi posso prendere gli ultimi sample $N_{cp}$ e appenderlo in testa al vettore dei samples in modo da avere un nuovo vettore $\overline{s}$ di lunghezza $N+N_{cp}$. In questo vettore, l’elemento $\overline{s}(-1) = s(N-1)$.
Abbiamo quindi creato sample con indici negativi e possiamo riscrivere l’equazione come: 
$$
\begin{align*}
y(0) &= h(0)\overline{s}(0)+h(1)\overline{s}(-1)+…+h(L-1)\overline{s}(-L+1) =\\[4pt]
&= h(0)s(0)+h(1)s(N-1)+…+h(L-1)s(N-L+1)
\end{align*}$$

Quindi ora la matrice $\overline{\mathcal{H}}$, che è ancora di **Toeplitz**, non ha più gli zeri nel triangolo superiore. Ogni riga della matrice può essere ottenuta da tutte le altre facendo uno shift circolare verticale. 
Questa proprietà rende la matrice **Circulant**.

Il prezzo da pagare è la banda e l’energia occupata per trasmettere più volte lo stesso sample.

La matrice può essere diagonalizzata come $\overline{\mathcal{H}} = F^{H}HF$ con $F$ unitaria con e normalizzata di Fourier e $H$ una matrice diagolane.
Se una matrice è unitaria allora $A^{H}A = AA^{H} = I$ e quindi $A^{-1}=A^{H}$.
Discrete Fourier Transform: $[F]_{k,n} = \frac{1}{\sqrt{N}}e^{-\frac{j2\pi kn}{N}}$

Vediamo perché questa proprietà è importante.
Definiamo $Y = Fy$ e $S = Fs$. 
Sviluppando si ottiene $Y = Fy = F\overline{\mathcal{H}}s = FF^{H}HFs = HS$
Visto che $H$ è una matrice diagonale allora non c’è l’ISI perché non c’è più quel termine a sommare. Ecco perché è così potente, risolve il problema principale del canale di comunicazione pagando il costo di energia e banda occupata.

Schema a blocchi sulle slide. 

Possiamo dimenticare tranquillamente PAM & co ma resta valida la mappa di Gray e tutto ciò che si è detto sui simboli e sul decisore.
Il vettore di simboli viene creato in frequenza e poi si converte nel tempo.

$Y(n) = H(n)S(n) +N(n)$ non ha interferenza intersimbolica. 
$NT$ è il trasmission rate del vettore, trasmetto $N$ simboli ogni $NT$ secondi. Il vantaggio è nel delay $T > \sigma_{\tau}$ quindi l’ISI si riduce.
on the receiver side, can we use replicas for checking errors? 
CALIFFO

# OFDM interpretation

formule e grafici sulle slide.

La banda del singolo segnale sarà $\Delta f = \frac{B_{s}}{N}$.