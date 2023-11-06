
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
ma $\lambda = \frac{c}{f} = 0.1 \text{ m}$.
L’attenuazione è pari a -60 db che è pari a 1 milione.

C’è da considerare che più il segnale si propaga e più incontra ostacoli. 

$\Gamma(f_{0},d_{0}) \approx (\lambda/4\pi)^{2}$: free space propagation
$(\frac{d_{0}}{d})^{n}$: $n > 2$ viene chiamato *path-loss exponent*.
