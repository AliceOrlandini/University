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
L’addizione verrà effettuata in N cicli di clock.
