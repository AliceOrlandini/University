
# Laboratorio 1

Prima cosa: il segnale deve essere digitale, la frequenza di campionamento sarà 48kHz. 
$f_s >> 2f_{max}$ 
Dobbiamo digitalizzare anche il coseno e lo faremo campionando ad una frequenza: $f_{max} = 2f_c + \frac{f_{s}^{audio}}{2}$ 
quindi alla fine avrò come minimo $f_s = 4f_c + f_s^{audio}$

Qui ci sono calcoli che non ha mai spiegato ma pensa di averlo fatto. 

Dobbiamo regoalrizzare i clock:
$f_s^{(audio)} = 48$ ks/s
$f_s^{(cos)} = 368$ ks/s

Dobbiamo aumentare la frequenza di campionamento del segnale audio, lo facciamo tramite la funzione *resample*.
Il motivo per cui possiamo farlo sempre problemi è che un segnale campionato nel tempo ha lo spettro periodicizzato per cui andando a campionare più velocemente semplicemente avrò le repliche più distanti tra loro.
Per fare il resample bisognerebbe tornare in analogico e poi tornare in digitale. Su matlab ovviamente non si può fare. 

Ora facendo la modulazione il segnale si sposta a destra e a sinistra.

Poi moltiplicando di nuovo per un coseno lato ricevitore il segnale ritorna la centro e l’ultima cosa che ci rimane da fare è filtrarlo con un passa-basso di banda 24 kHz.

Ma porcodio ho dato la risposta giusta e mi ha detto di no, la dice uguale un altro e dice che va bene. 
$x_{db} = 10\cdot log_{10}(x)$

Infine, dopo il filtro devo fare il resampling di nuovo per riportarlo a 48 kHz. 

Questa parte l’ha spiegata decisamente meglio Saguinetti, studiala dai suoi appunti. 

Ascoltando la canzone, non c’è distorsione. 

# Laboratorio 2

Le frequenze del coseno del trasmettitore e del ricevitore non saranno mai le stesse perché è praticamente impossibile sincronizzarli completamente.
Ci sarà una extra fase $\phi_R$  tale che il coseno del ricevitore sia: $2cos(2\pi f_c t + \phi_R)$ 

Il segnale sarà $v(t) = s_{DSB}(t) 2cos(2\pi f_c t + \phi_R)$ = 
$2cos\alpha cos\beta = cos(\alpha + \beta) + cos(\alpha - \beta)$
$$= 2m(t)cos(2\pi f_c t)cos(2\pi f_ct + \phi_R) =$$
$$= m(t)[cos(2\pi \Delta f t + \Delta \phi) + cos(2\pi(f_c+f_c)t + \Delta \phi)]= $$
Il secondo termine puo essere eliminato ma il primo può dare problema. 

Se $\Delta f = 0$ e $\Delta \phi \not = 0$ allora $m(t) = m(t)cos(\Delta \phi)$
Se $\Delta f \not= 0$ e $\Delta \phi = 0$ allora $m(t) = m(t)cos(2\pi \Delta ft)$

Quando usiamo sistemi wireless dobbiamo tenere in considerazione questa questione. 
Non vedremo come risolverlo. 

# Laboratorio 3 - PAM in MATLAB

Il trasmettitore è composto da:
- $S$ un generatore di bit casuali.
- Il mappatore
- Il Filtro $g_T(t)$ che sarà un filtro a coseno rialzato
Trascureremo il canale, aggiungeremo solo del rumore.
Il ricevitore invece è composto da:
- Un filtro di ricezione $g_R(t)$.
- Un campionatore con un periodo $t=mT$
- Un decisore basato sul criterio a massima verosimiglianza.

La prima parte del codice è l’inizializzazione, impostiamo la creazione di 1 milione di bit. Decido il tipo di mappatore e la grandezza della costellazione $M = 8$ (in questo caso il numero di bit per simbolo è 3).
Per quanto riguarda il coseno rialzato devo impostare:
- Il roll-off.
- Oversapling function: numero di sample per simbolo, in questo caso per ogni simbolo abbiamo 4 campionamento.
- La truncation.
Poi imposto il valore massimo $E_S$ $N_0$.
Infine, imposto il seed del random number generator, per fare ciò uso $rng(1)$.

$A$ è l’energia della costellazione (?)

Dobbiamo aggiustare il rate del simbolo con quello del filtro in trasmissione, questo perché il filtro in trasmissione sta over samplando di un fattore 4 quindi i clock sono diversi, uno è $T$ e l’altro è $4T$ quindi piazzo 3 zeri per ogni simbolo.
Questa cosa la fa la funzione *upsample* che aggiunge zeri per ogni simbolo.
Ora dobbiamo fare la convoluzione tra i simboli e il filtro.

Adesso dobbiamo considerare l’effetto del rumore (come detto il canale lo trascuriamo). Facciamo un vettore di rumore perché dobbiamo scrivere $r(t) = s(t)+w(t)$ quindi il vettore deve avere una grandezza uguale a quella di $s(t)$.

La varianza del rumore dovrà essere tale che $\sigma^{2}=\frac{E_{s}}{N_{0}}= 10$
impostato quando ho definito i parametri. Nel mondo reale si lavora su $E_{s}$ per aggiustare il valore perché non abbiamo controllo su $N_{0}$. In MATLAB però è più comodo lavorare su $N_{0}$ perché è più facile.
In pratica $E_{s}= \frac{A}{2}$ e $N_{0} = \sigma^{2}$ quindi sostituendo ottengo:
$\frac{E_{s}}{N_{0}}=\frac{A}{2}\cdot \frac{1}{\sigma^{2}}$
$\sigma^{2}=\frac{A}{2}\cdot (\frac{E_{s}}{N_{0}})^{-1}$
Infine, prendo la distribuzione standard generata da MATLAB$n \thicksim \mathcal{N}(0,1)$ e la moltiplico per $\sigma^{2}$ in modo da ottenere $n^{’} \thicksim \mathcal{N}(0,\sigma^{2})$

Lato ricevente la prima cosa da fare è filtrare il segnale con $g_{R}(t)$.
Ora devo campionare $x(t)$, in MATLAB è più facile perché non esistono funzioni continue ma solo discrete quindi tutto ciò che dobbiamo fare è selezionare i campioni corretti.
Per fare ciò, troviamo il sample in corrispondenza di 1 e poi gli altri vengono di conseguenza. 