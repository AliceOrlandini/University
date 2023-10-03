In questa lezione faremo un ripasso degli argomenti principali del programma di Elettronica Digitale. Infatti, noi con VHDL lavoreremo ad un livello di astrazione elevato ma bisogna comunque sapere “cosa c’è sotto”.
# Transistore MOSFET

![Mosfet|center|400](https://www.edutecnica.it/elettronica/mosfet/1.png)

Nel transistore MOSFET si hanno 3 terminali: **Source**, **Drain** e **Gate**.
La larghezza del gate $W$ e la distanza tra le zone drogate di tipo $n^+$ determinano le caratteristiche elettriche del transistor. 
Un problema che si è iniziato a riscontrare intorno al 1980 è quello delle scariche elettrostatiche che provocavano una foratura del dielettrico. Per evitare ciò si abbassò gradualmente $V_{DD}$ da un valore di partenza di 5V a 3.3V, 2.5V, 1.8V …
Il problema però di abbassare la tensione è che il rumore può diventare preponderante tanto da non riuscire a riconoscere il segnale. 
L’NMOS può essere rappresentato nel seguente modo:
![NMOS]()
Il PMOS è il duale dell’NMOS e può essere rappresentato nel seguente modo: 
![PMOS]()

In generale, sia NMOS che PMOS possono essere visti come interruttori ideali. 

# Logica Pass-Gate

La **logica pass-gate** viene utilizzata per la progettazione di varie famiglie di circuiti integrati. Il vantaggio di utilizzare questa tecnologia è che si riduce al massimo il numero di transistor utilizzati per implementare una certa funzione logica poiché si vanno infatti ad utilizzare i [[#Transistore MOSFET|MOSFET]] come interruttori. 

Quando si utilizza questa logica bisogna considerare: 
- tempo di propagazione
- consumo di potenza dinamica
- area occupata

Inoltre, è bene ricordarsi che quando si utilizza un nmos allora non si riuscirà a raggiungere il livello alto pieno. Nel caso del pmos invece non si riuscirà a raggiungere il livello basso pieno.
Per ovviare a ciò, si pone un nmos e un pmos in parallelo. L’area totale sarà quella occupata dai due transistor e da i collegamenti necessari per connettere ingresso e uscita. 

Un nmos può essere rappresentato con una resistenza equivalente di valore $$R = \frac{1}{\beta _n(V_{GS}-V_{T})} = \frac{1}{\beta _n (4-V_U)}$$
che ha un andamento di tipo *iperbolico* all’aumentare di $V_U$:
![NMOS Resistenza equivalente]()
# Inverter CMOS

La verifica richiede più persone che la parte di design. Ad esempio se ho un team di 10 persone dividerò 3 persone al design e 7 alle verifiche (è importante che non facciano entrambi sia design che verifiche perché sennò un errore concettuale non verrebbe rilevato).
Poi serve anche la caratteristica.

Tempo di propagazione dipende dalla capacità equivalente
# Review of IC Manufacturing