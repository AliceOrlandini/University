# Full Adder 1 bit

Andiamo a studiare il full adder poiché i processori utilizzano molto questo componente (input-output, memory, datapath, control).
Nel data book generalmente possiamo trovare la tabella di verità, il blocco coi relativi piedini, le capacità, l’area occupata e la potenza consumata.

Per costruire il full adder possiamo utilizzare componenti ad 1 bit e combinarli insieme, con la seguente logica:
$$s_{i}= a_{i} \text{ xor } b_{i} \text{ xor } c_{i}$$
$$c_{i}= (a_{i}\cdot b_{i})+(a_{i}\cdot c_{i-1})+(b_{i}\cdot c_{i-1})$$
Per fare l'architettura a CMOS si utilizza la tabella di verità.

![full_adder|center|300](https://www.build-electronic-circuits.com/wp-content/uploads/2022/10/fullAdder-1-768x355.png)

Lo schema in CMOS è il seguente: 

![[full_adder_cmos.webp|center|150]]

Il risultato è composto da *16 transistor*.
Una proprietà importante è quella di *inversione*: negando tutti gli input si ottengono gli output negati. 

![[inversion_prop.webp|center|300]]

Per fare la versione CMOS del carry out si può utilizzare la tabella di verità con la mappa di Karnaugth. In questo caso abbiamo un totale di *10 transistor* e questo componente viene chiamato **Binary Adder**.

![[binary_adder.webp|center|150]]

Utilizzando questo elemento e combinandolo con il precedete servono un totale di 32 transistor: 16 per S, 10 per C e 6 per gli inverter di B,A e C.

Un altro approccio consiste nello scrivere il carry e poi ricavare la somma, in questo caso: $$s = \overline{c}_{o}\cdot (a+b+c)+(a\cdot b\cdot c_{i})$$
Facendo la rappresentazione a CMOS ci serviranno* 28 transistor* quindi c'è di fatto un risparmio ma al costo di rinunciare alla simmetria.

![[binary_adder_simp.webp|center|150]]

Ora proveremo a rispondere alle seguenti domande: dato un full adder a 1 bit,
- come posso usarlo per costruire un full adder a 64 bit?
- come lo rendiamo veloce?
- come lo modifichiamo per fare un adder/substractor?
Per rispondere alla prima domanda abbiamo due possibilità: Seriale o Parallela.
# Serial Adder

![[serial_adder.webp|center|300]]

Il **Serial Adder** è composto da:
- 1 full adder a 1 bit
- 3 N-bit shift register
- 1 D-Flip-Flop positive edge triggered
- Memoria per salvare $a$, $b$ e $s_{i}$ per un totale di 3N celle di memoria
Il funzionamento è il seguente: vengono dati in input  e , vengono sommati e il risultato  va nello shift register della somma. Il Carry Out invece viene memorizzato in un Flip-Flop in modo da riutilizzare quel dato come Carry In della prossima somma. 
C'è un ipotesi da rispettare ovvero che il carry sia più veloce della somma $t_{C}> t_{S}$, altrimenti non verrebbero rispettati i vincoli di timing. 
L’addizione verrà effettuata in $N$ cicli di clock, quindi $t_{tot}= N\cdot T_{clock}$ con $T_{clock} \ge t_{cq}+t_{c}+t_{su}$.
Il vantaggio è che è molto semplice in termini di risorse mentre lo svantaggio è che bisogna aspettare $N$ cicli di clock prima di ottenere il risultato. 
I path in totale sono 6. 
# Parallel Adder

Se utilizziamo soluzioni parallele andiamo ad aumentare la velocità al costo di aumentare il numero di full adder. 

## Ripple Carry Adder

Un esempio di questa architettura è il **Ripple Carry Adder**. 

![[ripple_carry_adder.webp|center|300]]

Un altro vantaggio di questo approccio è la modularità. 
Il critical path sarà quello che da $A_{0}$ e $B_{0}$ va a $Ow$ e $S_{3}$. Il peggiore tra i due path dipende dai propagation delay, infatti se si ha $\tau_{C}> \tau_{S}$ allora il critical path sarà pari a $4 \cdot \tau_{C}$, mentre se si ha $\tau_{S}> \tau_{C}$ allora sarà $3\tau_{C}+ \tau_{S}$. 
Quindi il tempo di attesa sarà soprattutto dovuto alla propagazione del carry. 
Inoltre $T_{tot}= N\cdot T_{c}$ con $T_{c}$ il caso peggiore di propagation delay per un full adder 1-bit. 

Esiste un’altra soluzione che sfrutta la *proprietà di inversione*. In questo caso si riduce il critical path perché metto meno inverter ma si lasciano $s$ e $c_{o}$ negate. Questo riduce la complessità nel critical path.

![[adder_invert.webp|center|300]]

## Carry Look Ahead Adder

Un’altra configurazione è quella del **Carry Look Ahead Adder**. 
Il carry è una funzione di questo tipo: 
$$
C_{i}= A_{i}\cdot B_{i}+ C_{i-1}\cdot (A_{i}\text{ xor } B_{i})
$$
E si nota che ogni volta che $A_{i}\cdot B_{i} = 1$ allora il carry sarà pari ad 1, questo termine viene chiamato *carry generate*.
Poi, si definisce questo termine $A_{i}\text{ xor } B_{i}$ come *carry propagate*, poiché quando questo termine è pari ad 1 allora il carry sarà pari a zero. 
Possiamo quindi definire:
$$G_{i}= A_{i}\cdot B_{i}$$
$$
P_{i} = A_{i}\text{ xor } B_{i}
$$
E riscrivere il carry come: 
$$
C_{i}= G_{i}+C_{i-1}\cdot P_{i}
$$
Si nota che tutti i carry possono essere *generati allo stesso tempo*.

Lo schema del blocco base è la seguente:

![[carry_look_ahead.webp|center|250]]

Possiamo quindi costruire il circuito che calcola il carry come:

![[carry_look_ahead_scheme.webp|center|250]]

Ha il vantaggio di avere solo *3 livelli di logica* per il 64-bit full adder che lo rende molto veloce. 
Il limite è la *fan-in* all’aumentare dei $C_{i}$ quindi generalmente ci si ferma ad $i=3$.

Lo schema complessivo dell'adder che sfrutta il carry look ahead adder è: 

![[adder_ahead_scheme.webp|center|300]]

Se volessi fare un full adder con $i>3$ posso usare l’approccio del ripple carry, che viene chiamato **CLA4 Ripple Carry**. Oppure abbiamo la soluzione **CLA Gerarchica**. 

# Pipeline

Per andare ancora più veloci si utilizza la **pipeline**. 

![[pipeline.webp|center|250]]

Il concetto di pipeline consiste nel dividere il critical path ponendo un registro. In totale si perdono 2 cicli di clock perché dobbiamo aggiungere 3 registri. 
I registri in input ed output servono per salvare i risultati mentre viene svolta la seconda somma, altrimenti verrebbero persi al ciclo di clock successivo. 
Facciamo un esempio: 
- $t_{c} = 2ns$
- $t_{cq}= 1.6ns$
- $t_{su}= 0.4 ns$
Sappiamo che senza pipeline si ha: 
$$
T \ge t_{cq}+ 4\cdot t_{c} + t_{su} = 1.6 + 8 + 0.4 = 10ns
$$
Mentre con pipeline otteniamo: 
$$
T \ge t_{cq}+ 2\cdot t_{c} + t_{su} = 1.6 + 4 + 0.4 = 6ns
$$
Riassumendo i parametri in una tabella: 

|  | Senza Pipeline | Con Pipeline |
| ---- | ---- | ---- |
| Throughput | 100 MHz | 166 MHz |
| Latency | 1 ciclo di clock (10ns) | 2 cicli di clock (12ns) |

*cancella per sbaglio tutta la slide* Fanu: “OH CAZZO”.
C'è da considerare che in generale si aumenta *l’hardware complexity*. 

# Sottrattore

In complemento a 2 sappiamo che:
$$D = A-B = A+\overline{B}+1$$
Quindi, sfruttando l'adder:

![[substractor.webp|center|250]]

E volendo possiamo combinare il sommatore con un sottrattore in modo da utilizzare un unico elemento circuitale che si comporta come sommatore o sottrattore secondo dei segnali di comando: 

![[adder_substractor.webp|center|250]]

Servono due segnali $X_{1}$ e $X_{2}$ perché si vuole implementare $A+B$, $A-B$ e $B-A$.
# ALU

Nell’ALU di un computer generalmente si utilizzano dei segnali che definiscono il comportamento del blocco. Infatti, a seconda del valore di questi, il blocco svolgerà una particolare operazione. 
Esistono anche CPU open source in VHDL come la **RISC-V**.
# Moltiplicatore

Alcune CPU hanno anche il MAC: **Multiplier Accumulator**: $\sum\limits_{i}x_{i}c_{j}$.
Per fare un moltiplicatore possiamo ad esempio usare la lookup table, considerando $A$ e $B$ come indirizzo ma il problema è la grandezza della memoria e anche il tempo richiesto per accedervi. Inoltre, non è nemmeno possibile includere il concetto di pipeline con questo approccio. 

Un altro approccio è quello di effettuare la moltiplicazione aritmeticamente. L’algoritmo è lo **Shift and Sum** (quello delle elementari) e il circuito risultate è chiamato **Parallel Multiplier**. 
In questo caso avrò bisogno dei seguenti blocchi:
1. Blocco per il prodotto
2. Half Adder: un sommatore ma senza carry in
3. Full Adder
4. Product + Half Adder 
5. Product + Full Adder

![[parallel_mul.webp|center|200]]

In termini di complessità richiede:
- $N^{2}$ porte AND
- $N\cdot (N-2)$ Full Adder
- $N$ Half Adder

E il propagation delay totale sarà $T_{tot} = 2\cdot (N-1)\cdot T_{b}$ con $T_{b}$ il single cell propagation delay.