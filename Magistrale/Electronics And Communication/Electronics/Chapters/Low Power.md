In quest'ultima parte del corso parleremo di design orientati al basso consumo energetico. 
Ciò è importante anche per capire quale package si adatta meglio al nostro circuito in modo da trasferire adeguatamente l'energia. Altri problemi da considerare sono la vita della batteria e il raffreddamento che hanno un costo.

Bisogna iniziare a pensare alla power consumption fin dal design del sistema in modo da non avere problemi quando si avrà il prodotto finale. Ad esempio in questo contesto si potrebbero confrontare più algoritmi implementativi e ridurre al minimo il numero di somme e moltiplicazioni. 

## CMOS Energy and Power Equations

Vediamo le tipologie di power consumption che si hanno in tecnologia CMOS: 
$$
E = C_{L}\cdot V_{DD}^{2}\cdot P_{0\rightarrow 1} + t_{sc}\cdot V_{DD}\cdot I_{peak}\cdot P_{0\rightarrow 1} + V_{DD}\cdot I_{leakadge}
$$
con $f_{0\rightarrow 1} = P_{0\rightarrow 1}\cdot f_{clock}$ e $P_{0 \rightarrow 1}$ la probabilità che l'output transisca da 0 ad 1.
L'espressione della potenza sarà:
$$
P = C_{L}\cdot V_{DD}^{2}\cdot f_{0\rightarrow 1} + t_{sc}\cdot V_{DD}\cdot I_{peak}\cdot f_{0\rightarrow 1} + V_{DD}\cdot I_{leakadge}
$$
Il primo termine si chiama *Dynamic Power* dovuta al caricamento o allo scaricamento delle capacità nel circuito. 
Il secondo termine si chiama *Short-Circuit Power* che non approfondiremo.
Il terzo termine si chiama *Leakage Power* ed è quella utilizzata quando si accende il circuito ma non si effettua alcuna operazione. 

Proviamo a migliorare la power consumption diminuendo la dynamic power: 
$$
P_{dyn} = C_{L}\cdot V_{DD}^{2}\cdot f_{0\rightarrow 1} = C_{L}\cdot V_{DD}^{2}\cdot P_{0\rightarrow 1}\cdot f_{clock}
$$
Noto che potrei diminuire la power supply $V_{DD}$, tuttavia riducendola avrei il problema di aumentare il propagation delay $t_{pHL}= \cfrac{KC_{L}}{\beta_{neq}}\cdot \cfrac{1}{V_{DD}-V_{tn}}$ e quindi diminuire la velocità del sistema. 
Potrei invece lavorare sulla switching activity $P_{0\rightarrow 1}$ implementando l'algoritmo in modo da ridurre il numero di switch. 
L'ultimo elemento su cui si potrebbe lavorare è la frequenza di clock $f_{clock}$ perché più è alto più è alta la power consumption. Per ovviare a ciò si utilizza una tecnica chiamata *just in time computing* che prevede di utilizzare solo clock adeguati all'applicazione che bisogna soddisfare. 

Vediamo ora a migliorare considerando la leakedge power:



