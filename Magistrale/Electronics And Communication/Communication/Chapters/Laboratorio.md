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

