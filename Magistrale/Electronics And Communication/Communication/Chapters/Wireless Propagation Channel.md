
Vediamo come modellare ciò che si trova tra il trasmettitore e il trasmettitore. All’inizio del corso avevamo visto la divisione della banda in base alla frequenza. 
A seconda della carrier frequency cambia il tipo di propagazione:
1. **Ground wave**: $f_{c}<2\text{ MHz}$ usate in ambito militare, la terra opera come conduttore, per questo la propagazione può arrivare a centinaia di chilometri. Il problema è che 2 MHz è una frequenza molto piccola quindi le applicazioni sono limitate. 
2. **Sky wave**: $f_{c} \approx 10 \text{ MHz}$ il segnale colpisce la ionosfera e la terra. Il problema è che la trasparenza della ionosfera è legata al plasma che dipende dal sole. 
3. **Space wave**: $f_{c} > 30 \text{ MHz}$ il segnale si propaga nell’aria e non rimbalza da nessuna parte, per questo motivo il range in termini di metri è limitato. L’importante è che non ci siano ostacoli tra trasmettitore e ricevitore. 

Noi ci concentreremo sullo space wave.
Ci sono 3 fenomeni da
- rifrazione: quando l’onda elettromagnetica urta una superficie che ha una dimensione comparabile con la lunghezza d’onda del segnale
- diffrazione: quando il segnale urta un ostacolo ruff
- scattering: quando l’onda urta molti piccoli ostacoli (ad esempio un albero con molti rami) e ciò che succede è che il segnale viene replicato in molte direzioni.

Se vogliamo studiare come il segnale si propaga la prima cosa che bisogna studiare è l’ambiente. Ciò è molto difficile perché per ogni trasmettitore e ricevitore (anche uguali) dobbiamo studiare tutti i sistemi. Per cui, questo sistema, chiamato *Ray Tracing*, è utilizzato solo in alcuni contesti particolari. 

Generalmente si utilizzano sistemi semplificati basati su modelli probabilistici. 

## Large Scale Fading

È un modello che caratterizza la media di attenuazione del segnale che separano trasmettitore e il ricevitore.
Si combina di due modelli: *path-loss* e *shadowing* che insieme restituiscono il valor medio del canale.

Il segnale si propaga dall’antenna in modo sferico e la potenza è distribuita uniformemente su tutta la sfera. Se volessi calcolare la potenza che arriva ad una certa distanza $d$, essa sarà proporzionale alla potenza del trasmettitore e dalla distanza. 

La superficie della sfera è pari a: $S=4\pi r^{2}$.
La potenza ricevuta sarà: 
$$P_{RX} = P_{TX}\cdot \frac{1}{S}\cdot A$$
con $A = (\frac{\lambda}{2})^{2}\cdot \frac{1}{\pi}$ l’area dell’antenna.

Sostituendo si ottiene: $$P_{RX}= P_{TX}\cdot \frac{1}{4\pi d^{2}} \frac{\lambda^{2}}{4}\cdot \frac{1}{\pi} = P_{TX}\cdot \frac{1}{(4\pi d)^{2}}$$
ma $\lambda = \frac{c}{f_{0}} = 0.1 \text{ m}$.
L’attenuazione è pari a -60 db che è pari a 1 milione.

C’è da considerare che più il segnale si propaga e più incontra ostacoli. 

## Path Loss

$\Gamma(f_{0},d_{0}) \approx (\frac{\lambda}{4\pi d_{0}})^{2}$: free space propagation, mostra la dipendenza con la carrier frequency.
$(\frac{d_{0}}{d})^{n}$: $n > 2$ viene chiamato *path-loss exponent*.
$A_{PL}= \frac{P_{Tx}}{P_{Rx}}$
Tabella sulle slide.

- Trasmitted power.
- Due termini legati all’ambiente.

Un componente importante è $f_{0}$. Se aumenta la frequenza aumenta anche l’attenuazione del canale. Il segnale oscilla anche di più quindi ci mette di più ad arrivare a destinazione. 

Intorno ai 60 GHz si ha una finestra in cui l’attenuazione è molto elevata, queste zone agiscono come filtri. Il motivi di questi effetti sono legati alla presenza di acqua e di ossigeno.

## Shadowing

Ipotizziamo di avere una cella con due ricevitori alla stessa distanza dal trasmettitore. In teoria, visto che $d_{A}= d_{B}$ mi aspetterei che l’attenuazione sia la stessa. In realtà non è così per la presenza dello shadowing che accade in presenza di ostacoli specifici che hanno effetti più importanti sulla propagazione. La shadowing è una VA Gaussiana misurata in dB (log normally distributed).

In questo caso: 
$$P_{Rx}[dBm] = P_{Tx}[dBm] + A_{PL}[dB] + A_{S}[dB]+ A_{SS}[dB]$$
con:
- $A_{PL}$ un valore deterministico che dipende dalla distanza $d$.
- $A_{S}$ una variabile aleatoria log-normally distributed.
- $A_{SS}$ l’attenuazione data dalla small scale fading.

## Small Scale Fading

Per descrivere questo effetto dobbiamo considerare lo schema a blocchi. Lo small scale fading rappresenta l’impulse response $h(t)$ del canale. 
Il Large era un fattore moltiplicativo mentre questo è una funzione lineare tempo invariante.

$y(t) = s(t) \circledast h(t)$

Lo small scale fading è dovuto al fatto che riceviamo la somma di molte repliche del segnale con delay differenti che si originano con l’urto con gli ostacoli. Il risultato finale è un segnale disturbato perché le repliche del segnale potrebbero interferire tra loro e generare quindi ISI.
Attenuazione: dovuta alla presenza del large scale phading $a_{1}$: aleatoria.
Delay $\tau_{1} =s/c$: è una deterministico perché visto che è legato alla velocità della luce anche se lo spazio cambia un po’ l’effetto sul delay è trascurabile.
Fase $\phi_{1}$: aleatoria.

$y(t) = A_{LS}\sum\limits_{\mathcal{l}=0}^{N_{c-1}}a_\mathcal{l} e^{j\phi_\mathcal{l}}s(t-\tau_\mathcal{l})$

Il tempo intercorso tra l’arrivo delle repliche si chiama **Time Dispersion**.
*calcoli*
$x(t) = \sum\limits_{i}c_{i}g(t-iT)$
$g(t) = g_{T}(t) \circledast h(t) \circledast g_{R}(t) = g_{N}(t)\circledast h(t)$
$g(t) = \sum\limits_{l=0}^{N_{c}-1} a_{l}e^{j\phi _{l}}g_{N}(t-\tau _{l})$
$x(m) = x(t)|_{t=mT}=\sum\limits_{k}c_{m-k}g(kT) = c_{m}g(0) +\sum\limits_{k,k\not=0}c_{m-k}g(kT)$
$g(kT) = g(t)|_{t=kT} =\sum\limits_{l=0}^{N_{c}-1}a_{l}e^{j\phi _{l}}g_{N}(kT-\tau _{l})$
Si nota che $\tau_{l}$ non è controllabile quindi l’interferenza è inevitabile. 
Dobbiamo chiederci quanta interferenza intersimbolica abbiamo nel nostro sistema.
## Delay Spread

Il delay spread è il parametro che misura la dispersione temporale del canale e si indica con $\sigma_{\tau}$.
È meglio che la time dispersion sia **piccola** perché se invece le repliche non arrivano allo stesso instante avrei molto più rumore.
Se c’è solo una replica allora $H(f) = \alpha_{0}e^{j\phi_{0}}$ e se lo prendo in modulo avrò $|H(f)| = \alpha_{0}$.

# Coherance Bandwidth

La **Coherence Bandwidth** $B_{c}$ del canale è l’intervallo di frequenza in cui si ha la frequence response $H(f)$ (e di conseguenza la sua densità spettrale di potenza $S_{h}(f)$).
Questo valore è inversamente proporzionale al delay spread. 
Un’approssimazione generalmente usata è: $$B_{C}\approx \frac{1}{5\sigma_{\tau}}$$
Se $\sigma_{\tau} \ll T$ allora $B_{c} > B_{s}$ e il canale è *flat*.
Se $\sigma_{\tau} > T$ allora $B_{c} < B_{s}$ e il canale è freque




