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
- $b_{1}= \frac{\sum\limits_{n=1}^N}{2}$
## Optimal power distribution

## Waterfilling

# Spartial diversity

## Receive diversity

## Antennas and carrier frequency

## MIMO Channel

## SIMO Channel

## Maximal ratio combining (MRC)

##