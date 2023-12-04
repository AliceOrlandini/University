## Adaptive modulation and coding (AMC)

Ogni sistema wireless può adattare il rate di trasmissione in base alla qualità del canale. Questo è stato studiato anche da Shannon che introdusse una misura della qualità del canale:
$$
C = B\cdot log_{2}(1+SNR)
$$
dove, dato il guadagno di canale $H$, l’SNR può essere misurato come:
$$
SNR = \frac{|H|^{2}\cdot P}{\sigma^{2}}
$$
infine è possibile definire l’efficienza spettrale come 
$$
\mu = log_{2}(1+SNR)
$$
misurato in $b/s/Hz$.
Figura sulle slide.

Ogni $k$ bit di informazioni sono mappati su $n$ encoded bit con $n > k$, quindi $kT_{b}= nT_{b}’ \Rightarrow T_{b}’ = \frac{k}{n}T_{b}$.
Ma $mT_{b}’ = T$ quindi se voglio trovare una relazione tra $T_{b}’$ e $T$ avrò che:
$$
T = mT_{b}’ = m \frac{k}{n}T_{b}=mRT_{b}
$$
con $R = \frac{k}{n}$ il rate del codice utilizzato.
Inoltre, $R_{b}= \frac{1}{T_{b}} = mR \frac{1}{T} \approx mRB$
perché $\frac{1}{T}$ può essere approssimato come la banda del segnale.
Sappiamo per il teorema di Shannon che $R_{b}<C$ e sostituendo:
$$
mRB < B log_{2}(1+SNR) \Rightarrow mR < log_{2}(1+SNR)
$$
che ci dice che devo scegliere $mR$ in modo che sia minore del risultato del logaritmo.
In pratica, c’è una mappatura diretta tra l’SNR misurato (che è indicatore della qualità del canale) e l’ordine di modulazione $M$ utilizzato e il channel coding rate $R$.
Tabella sulle slide.
Se si utilizza l’OFDM le cose sono un po’ più complesse perché il canale è frequency selective, quindi si può variare il modulation rate in ogni pezzo.
La potenza totale $P_{0}$ può essere distribuita negli $N$ channels con l’obiettivo di massimizzare il rate totale.
Impostando: 
$$
\sigma^{2}_{n} = \frac{\sigma^{2}}{|H(n)|^{2}}
$$
possiamo formulare il **waterfalling problem** che è un problema di *constraint*:
$$
\begin{equation}
\begin{cases}
max_{P} \sum\limits_{n=1}^{N}log_{2}(1+ P_{n}/\sigma^{2}_{n}) \\
P_{n}\ge 0 & i = 1,…,n \\
\sum\limits_{n=1}^{N} P_{n}=P_{0}
\end{cases}
\end{equation}
$$
Che nel nostro la soluzione ottima assume i valori:
$$
P_{n} = (\mu - \sigma^{2}_{n})^{+}=max\{0,\mu-\sigma^{2}_{n}\}
$$
con $n = 1,…,N$ dove il valore di $\mu$ che è una costante è scelto in modo tale che il requisito di potenza venga raggiunto:
$$
\sum\limits P_{n}= P_{0} \Rightarrow \sum\limits (\mu-\sigma^{2}_{n})^{+} = P_{0}
$$
Per risolvere questo problema ci sono vari algoritmi, noi usiamo il **metodo di bisezione** che è applicabile alle funzioni monotone crescenti $f: R^{n}\rightarrow R$. La nostra funzione è:
$$
f(\mu) = \sum\limits_{n=1}^{N}(\mu-\sigma_{n}^{2})^{+}-P_{0}
$$
i punti iniziali saranno:
- $a_{1}= 0 \Rightarrow f(0) = -P_{0} < 0$
- $b_{1}= \frac{\sum\limits_{n=1}^{N}\sigma_{n}^{2}+P_{0}}{N} \Rightarrow f(b) \ge 0$
L’idea è che ad ogni iterazione l’intervallo che contiene la soluzione si rimpicciolisce sempre più fino all’iterazione $k-esima$ dove la soluzione è nell’intervallo $(a_{k},b_{k})$ con $f(a_{k})< 0$ e $f(b_{k})>0$. 
Eseguiamo i seguenti passaggi:
- Prendiamo il valore centrale $c_{k}= \frac{a_{k}+b_{k}}{2}$
- Calcolare $f(c_{k})$:
- Se $f(c_{k}) < 0$ allora $a_{k+1} = c_{k}$ e $b_{k+1} = b_{k}$
- Se $f(c_{k}) > 0$ allora $a_{k+1} = a_{k}$ e $b_{k+1} = c_{k}$
Infine, quando $|f(c)|$ è sufficientemente piccolo l’algoritmo smette di operare e ritorna $c_{k}$.
### Esempio
Immagini sulle slide. 

## Optimal power distribution

## Waterfilling

# Spartial diversity

Quando abbiamo studiato il canale abbiamo visto la correlation distance. Possiamo expoit diversity utilizzando antenne multiple al tramettitore e al ricevitore per sftuttare questa diversità della distanza per il fatto che il canale è incorrelato. 
Lo svantaggio riguarda il costo dei device che aumenta per ogni antenna aggiunta, ma il vantaggio è quello di aggiungere diversità senza perdere risorse spettrali che sono ancora più costose. 
$$
d_{c}= \frac{\lambda}{2}
$$
Ad esempio, il WiFi a $@2.4 \ GHz$ ha $\lambda = 12.5 \ cm$.
C’è anche da considerare che più aumentiamo la frequenza più piccola è l’antenna quindi possiamo considerare array di antenne. La tecnologia che ne deriva si chiama **Massive-MIMO**.
Avendo tanta diversità possiamo accettare l’interferenza. 
## Antennas and carrier frequency

## MIMO Channel

MIMO è acronimo di **Multiple-Input Multiple-Output** e indica quei sistemi in cui si hanno più antenne in trasmissione e in ricezione.
Generalmente si modella il canale come una matrice $H$ che ha $N_{R}$ righe e $N_{T}$ colonne ($R$ = receiver e $T$ = trasmitter). 
Quando si utilizza questo canale si fa l’assunzione chiamata **Narrowband** per cui $h_{n,m}$ è uno scalare complesso.
## SIMO Channel

Sono una semplificazione delle MIMO in cui il trasmettitore ha una sola antenna mentre il ricevitore ne ha molte.
In questo caso si ha $N_{R} = N >1$ al ricevitore ed $N_{T}=1$ al trasmettitore. 
La variabile di decisione nell’antenna $i-esima$ è:
$$
x_{i}(m) = h_{i}c_{m}+n_{i}(m)
$$
Il rumore è sempre indipendente quindi ogni variabile ha il suo mentre la noise power si assume che sia la stessa per tutti.
I segnali ricevuti alle $N$ antenne sono combinati insieme e la variabile di decisione è:
$$
z(m) = w_{1}^{*}\cdot x_{1}(m) + \dots + w_{N}^{*}\cdot x_{N}(m)
$$
Slide di calcoli.
Il signal noise è:
$$
SNR = \frac{|\sum\limits_{i=1}^{N}w_{i}^{*} \cdot h_{i}|^{2}}{\sum\limits_{i=1}^{N}|w_{i}|^{2}}\cdot \frac{A}{\sigma^{2}}
$$
altri calcoli che portano a:
$$
SNR = \frac{A}{\sigma^{2}}\cdot ||h||^{2}
$$

## MISO Channel

Assumiamo ora di avere $N_{T}>1$ e $N_{R}=1$. 
L’SNR mi aspetto che sia lo stesso perché cambiare direzione non cambia.
Altra slide di calcoli.
dobbiamo normalizzare

Grafico 
unn’ho più voglia

## MIMO: spartial multiplexing

### Singular value decomposition

è roba di algebra lineare.