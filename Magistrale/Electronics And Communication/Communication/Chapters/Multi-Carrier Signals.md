
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

Troviamo una rappresentazione matriciale dell’operazione $y = \mathca$: (matrice sulle slide)