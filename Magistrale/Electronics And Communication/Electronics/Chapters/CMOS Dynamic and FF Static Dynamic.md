
# CMOS Dynamic 

Lo svantaggio di utilizzare la tecnologia CMOS risiede nel fatto che bisogna utilizzare un totale di $2\cdot N$ MOS: $N$ per la PUN ed $N$ per la PDN (considerando un caso simmetrico). 
Il vantaggio però è che questa è una soluzione molto robusta perché ci sono percorsi predefiniti per High to Low e Low to High ed è immune agli effetti delle radiazioni esterne in quando l'uscita sarà collegata a $V_{dd}$ oppure ground.

L'idea della **dynamic logic** nasce del fatto che at the end of the story non abbiamo bisogno della pull-up e della pull-down network perché il comportamento può essere cambiato in base a dei segnali esterni.
Introduco quindi un segnale di clock e divido il comportamento della logica in due fasi: la prima fase, dove il clock è pari a zero, sarà quella di pre ricarica dell'output, la seconda, in cui il clock è pari ad uno, userò solo la pull-down network e a seconda della configurazione di input posso avere che l'output deve rimanere ad 1 oppure deve scaricarsi a zero. 
Lo schema è il seguente: 

![[dynamic_logic.webp|center|300]]

I *vantaggi* sono il risparmio dei MOS perché siamo passati da $2\cdot N$ a $N+2$, e poi non devo duplicare gli ingressi per andare in input anche alla pull-up network. 
Gli *svantaggi* sono che l'output è valido solo quando il clock è pari ad 1 ed inoltre che gli ingressi devono essere stabili quando il clock è pari ad 1. 
Questo è un grande svantaggio quando si mettono più logiche in cascata. Il problema nasce dal fatto che la fase di pre ricarica porta l'output ad 1 e ciò influenza la seconda rete. Un modo per risolvere è mettere un **inverter**: 

![[domino_logic.webp|center|300]]


# Inverter Chain

Ipotizziamo di voler pilotare una grande capacità con un inverter che ha una piccola capacità.
Per fare ciò si pongono una serie di $N$ inverter in cascata con aree crescenti linearmente di un fattore $A$. 
Il delay ad ogni stage sarà:
$\tau_{1} = K \cdot \frac{A\cdot C_{in}}{\beta_{n}} = A \tau_{m}$
$\tau_{2} = K \cdot \frac{A^{2}\cdot C_{in}}{A \beta_{n}} = A \tau_{m} = \tau_{1}$
...
$\tau_{tot}= N\cdot A \tau_{m} = \frac{A}{ln(A)}\cdot \tau_{m}\cdot ln\left(\frac{C_{L}}{C_{in}}\right)$
Il valore di $A$ che minimizza questo delay è $e = 2.71$ e si ottiene facendo la derivata.

# FF Static Dynamic

# Flip Flop

Abbiamo già visto la differenza tra Flip-Flop e Latch, ora vedremo come si costruisce il Flip-Flop. 
Il Flip-Flop visto a reti logiche è formato da Latch in configurazione master slave implementati tramite architettura **set-reset NOR2**:

![nor2|center|300](https://media.geeksforgeeks.org/wp-content/uploads/20230924154744/Difference-between-SR-Flip---flop-and-RS-Flip---flop-04.png)

Si ha bisogno quindi di 8 transistor. La stessa cosa si può fare utilizzando l'architettura **set-reset NAND2**:

![nand2|center|300](https://media.geeksforgeeks.org/wp-content/uploads/20230924154106/Difference-between-SR-Flip---flop-and-RS-Flip---flop-02.png)

Infine si può aggiungere un enable (o *clock*) che va in AND con set e reset. 
Questo però non è il modo migliore per farlo, la funzione da implementare è:
$$
Y = \overline{Q} = \overline{S\cdot clock + Q}
$$
In questo modo ho che la pull-down è:
$$
\overline{Y}_{PDN} = S \cdot clock + Q
$$
e la pull-up, la duale della precedente. Questo approccio permette di utilizzare 6 transistor. 

Se si vuole utilizzare un D-Latch per ottenere un Flip-Flop, utilizziamo l'architettura master slave:

![master_slave|center|400](https://www.build-electronic-circuits.com/wp-content/uploads/2022/11/Dflipflop-Master-Slave-edge-triggered-2-1024x320.png)

L'unica cosa a cui bisogna stare attenti è che i due clock non siano pari ad 1 allo stesso tempo. Se per fare ciò utilizzassi un inverter non funzionerebbe perché ci sarebbero degli istanti in cui entrambi sono pari ad 1 a causa del ritardo di propagazione.

# Clock 

Un clock che non ha overlap può essere costruito da due NOR e degli inverter, nel seguente modo:

![clock|center|200](https://www.researchgate.net/profile/Hannu-Tenhunen/publication/224651349/figure/fig1/AS:668910529544205@1536492106648/Example-of-a-commonly-used-two-phase-non-overlapping-clock-generator-for-SC-SD-ADCs-1.png)

Utilizzando questo clock si può creare un *Flip-Flop-D Edge Triggered*:

![[flip_flop.webp|center|300]]

## Two ways multiplexer

Questo elemento circuitale implementa la funzione: 
$$
Q = A \cdot clock + B \cdot \overline{clock}
$$

![[multiplexer.webp|center|200]]

# Positive Latch 

