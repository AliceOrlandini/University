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
È possibile modificare l'architettura per portarlo a $75$
1. 