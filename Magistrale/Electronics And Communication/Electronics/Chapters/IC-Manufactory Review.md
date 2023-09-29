Vediamo come si fa un circuito integrato. Facciamo un ripassino di ciò che ci ha spiegato ”P8” giusto per essere tutti allineati. Noi lavoreremo ad un livello di astrazione elevato tramite il VHDL ma bisogna sapere “cosa c’è sotto”.
# Transistore MOSFET

Mosfet
Abbiamo 3 terminali: Source, Drain e Gain.
Width e Lenght: determinano le caratteristiche elettriche del transistor. 
C’era il problema delle scariche elettrostatiche che rischiavano di “forare il dielettrico”. Per evitare ciò si abbassò $V_{DD}$ da 5V a 3.3, 2.5, …
Il problema era poi il rumore che era preponderante tanto da non riuscire a riconoscere il segnale. 
Nmos
Pmos: è duale all’nmos.

Pass-Gate.
Bisogna considerare: Timing, Power Consumo, Area occupata.
Non si riesce a raggiungere il livello alto pieno.
Si mettono un nmos e un pmos in parallelo. L’area totale è quella occupata dai due transistor e poi i collegamenti. 

Inverter
La verifica richiede più persone che la parte di design. Ad esempio se ho un team di 10 persone dividerò 3 persone al design e 7 alle verifiche (è importante che non facciano entrambi sia design che verifiche perché sennò un errore concettuale non verrebbe rilevato).
Poi serve anche la caratteristica.

Tempo di propagazione dipende dalla capacità equivalente
# Logica Pass-Gate

# Inverter CMOS

# Review of IC Manufacturing