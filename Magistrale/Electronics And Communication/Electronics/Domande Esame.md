1. Data una rete combinatoria valutare la massima frequenza di clock.
Il critical path è quello col delay massimo
$T_{CK} \ge t_{cp}+t_{p}+t_{su}$
I 6 path sono:
1-A-X-1
1-B-X-1
1-A-Y-2
1-B-Y-2
2-C-X-1
2-C-Y-2
e per ognuno di loro bisogna scrivere l'equazione.
Poi implementare le PUN e PDN con la X negata e la Y negata. 

È possibile ridurre il consumo di potenza di Leakedge senza peggiorare la massima frequenza di clock? 
PowerSupply * LikedgeCa??? 
Sì rallentando gli altri percorsi. Oppure si aumenta la power supplintion ma nei piccoli circuiti non conviene. 

2. Date le informazioni di sheet di un full adder, fare un'architettura su 4 bit. 
Si fa un ripple carry adder. 
Valutare la massima frequenza, si scrivono le regole e si fa come al punto precedente. 
$\tau_{c} = 4ns$ e $\tau_{s}= 3ns$ allora $T_{CK}=t_{cp1}+(3\cdot \tau_{c}+\tau_{s})+t_{su2} = 4.2 + 15 + 0.8 = 20ns$ che in Hz è $50mHz$ perché quello più lungo è il critical path.
È possibile modificare l'architettura per portarlo a $75mHz$. Usiamo la pipeline, mettiamo un registro nel mezzo del circuito e un registro di ingresso e uno di uscita.
Poi si rifà il calcolo con il path più corto.

3. Data una porta AND a 3 ingressi, comparare la Leakedge Power, Propagation Delay, Power Consumption, Area, F per il tree e per il chain 
Leakedge Power: non abbiamo tanti elementi per dirlo perché dipende dall'area quindi tree = chain
Propagation Delay: tree < chain
Power Consumption: tree > chain se non consideriamo il glitch
Area = tree = chain
F : tree = chain ?

4. Serial Adder on 4 bits
Valutare la massima frequenza di clock. In questo caso, non è facile capire quanti sono i path. Sono 6+9 = 15 path perché ci sono 9 shift register.
È difficile capire la $t_{HOLD} \le t_{cdreg}+ t_{cdlogic} = t_{cdlogic}$ perché non c'è logica quindi la t logic è zero. Per risolvere questo problema si mette un buffer.

5. Dato un circuito con Vdd, input, output, ground, clock e reset. Non ottengo il risultato sperato, come posso usare gli input per capire il problema? 
Siamo certi che il design è giusto perché lo abbiamo simulato.
Riduco la frequenza di clock e se il sistema lavora allora vuol dire che c'è una setup violation. In caso contrario, c'è un problema di old time che però non c'è modo di modificare. 

6. Dato un grafico con speed sulle x e power consumption sulle y. Ci da CPU, DSP, FPGA e ASIC SC. Dire dove posizionarle. 
CPU: alta PC e bassa velocità
Poi a scendere DSP, FPGA e ASIC SC. 

