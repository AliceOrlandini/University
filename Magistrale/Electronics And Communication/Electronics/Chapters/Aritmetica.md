# Full Adder 1 bit

Studiamo il full adder perché i processori utilizzano molto questo componente (input-output, memory, datapath, control).
Nel data book troviamo la tabella di verità, il blocco coi relativi piedini, le capacità, l’area occupata e la potenza consumata.

Per costruire il full adder possiamo utilizzare componenti ad 1 bit e combinarli insieme, con la seguente logica:
$s_{i}= a_{i} \text{ xor } b_{i} \text{ xor } c_{i}$
$c_{i}= (a_{i}\cdot b_{i})+(a_{i}\cdot c_{i-1})+(b_{i}\cdot c_{i-1})$
Per fare la rappresentazione a CMOS si utilizza la tabella di verità. Il risultato ha 16 transistor.
Una proprietà importante è quella di *inversione*: negando tutti gli input si ottengono gli output negati. 

Per fare la versione CMOS del carry out si può utilizzare la tabella di verità con la mappa di Karnaugth. In questo caso abbiamo un totale di 10 transistor. 

Con questo approccio ci servono un totale di 32 transistor.

Un altro approccio consiste nello scrivere il carry e poi ricavare la somma, in questo caso: $s = \overline{c}_{o}\cdot (a+b+c)+(a\cdot b\cdot c_{i})$
Facendo la rappresentazione a CMOS ci serviranno 28 transistor quindi risparmiamo, tuttavia la soluzione non è simmetrica. 

Ora proveremo a rispondere alle seguenti domande: dato un full adder a 1 bit,
- come posso usarlo per costruire un full adder a 64 bit?
- come lo rendiamo veloce?
- come lo modifichiamo per fare un adder/substractor?

# Serial Adder

Schema a blocchi sulle slide. 
È composto da:
- 1 full adder a 1 bit
- 3 N-bit shift register
- 1 D Flip Flop positive edge triggered
- Memoria per salvare $a$, $b$ e $s_{i}$ per un totale di 3N celle di memoria
L’addizione verrà effettuata in N cicli di clock, $t_{tot}= N\cdot T_{clock}$ con $T_{clock} \ge t_{cq}+t_{c}+t_{su}$.

# Parallel Adder

Se utilizziamo soluzioni parallele andiamo ad aumentare la velocità al costo di aumentare il numero di full adder. 
Un esempio di questa architettura è il ripple carry adder. 
Schema a blocchi sulle slide.
Un altro vantaggio di questo approccio è la modularità. Inoltre $T_{tot}= N\cdot T_{c}$ con $T_{c}$ il caso peggiore di propagation delay per un full adder 1-bit. 

Esiste un’altra soluzione che sfrutta la proprietà di inversione. In questo caso si riduce il critical path perché metto meno inverter ma si lasciano $s$ e $c_{o}$ negate. 

Un’altra configurazione è quella del carry look ahead adder. 
Ha il vantaggio di avere solo 3 livelli di logica per il 64-bit full adder che lo rende molto veloce. Il limite è la fan-in all’aumentare dei $c_{i}$ quindi generalmente ci si ferma a $i=3$.
Se volessi fare un full adder con $i>3$ posso usare l’approccio del ripple carry, che in questo caso viene chiamato CLA4 ripple carry. Oppure abbiamo la soluzione CLA Gerarchica. 

Tabella riassuntiva sulle slide (comparison for N=64)
# Pipeline

Per andare ancora più veloci si utilizza la **pipeline**. 
Schema sulle slide. 
Il concetto di pipeline consiste nel dividere il critical path ponendo un registro. In totale si perdono 2 cicli di clock perché dobbiamo aggiungere 3 registri (disegno sulle slide).
*cancella tutto ciò che c’è nella slide* Fanu: “OH CAZZO”.
I parametri da valutare sono il throughput e la latenza, se si aumenta il primo aumenterà di un pochino anche il secondo (è un trade off).
Inoltre aumenta l’hardware complexity. 

# Sottrattore

In complemento a 2 sappiamo che $D = A-B = A+\overline{B}+1$.
Schema sulle slide.

# ALU

Nell’ALU di un computer generalmente si utilizzano due piedini $X_{1}$ e $X_{2}$ che definiscono il comportamento del blocco, a seconda del valore di questi il blocco sarà un sommatore o un sottrattore. 
Tabelle di programmazione sulle slide.
Esistono anche CPU open source in VHDL come la RISC-V.
# Moltiplicatore

Alcune CPU hanno anche il MAC: Multiplier Accumulator.
$\sum\limits_{i}x_{i}c_{j}$
Per fare un moltiplicatore possiamo ad esempio usare la lookup table, considerando $A$ e $B$ come indirizzo. Il problema è la grandezza della memoria e anche il tempo richiesto per accedervi. Non è nemmeno possibile includere il concetto di pipeline con questo approccio. 

Un altro approccio è quello di effettuare la moltiplicazione aritmeticamente. L’algoritmo è lo shift and sum (quello delle elementari). 
Avrò bisogno dei seguenti blocchi:
1. Blocco per il prodotto
2. Half Adder (è un sommatore ma senza carry in)
3. Full Adder
4. Product + Half Adder 
5. Product + Full Adder

Schema dell’architettura completa sulle slide. 

In termini di complessità richiede:
- $N^{2}$ porte AND
- $N(N-2)$ Full Adder
- $N$ Half Adder
E il propagation delay totale sarà:
- $T_{b}$ single cell propagation delay
- $T_{tot} = 2(N-1)T_{b}$




