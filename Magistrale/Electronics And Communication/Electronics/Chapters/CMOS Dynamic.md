
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




