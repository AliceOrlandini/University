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

Vediamo ora a migliorare considerando la leakage power:
$$
P_{leakage}= V_{DD}\cdot I_{leakage}
$$
La corrente di leakageè è dovuta a vari fenomeni:
- **Drain Junction**: dovuto alla presenza di diodi, infatti, quando esso è OFF, la corrente non è esattamente zero.
- **Gate**: in un CMOS l'ossido viene assunto come perfetto ma in realtà un po' di corrente passa dovuta all'effetto tunnel. 
- **Sub-Threshold**: questa è dovuta al fatto che prima di $V_{Tn}$ la corrente non è esattamente zero. Questa è la più importante perché è la più grande tra le tre e quindi la più critica.

![[leakage.webp|center|300]]

Il modo di risolvere questo problema è utilizzare transistor con $V_{Tn}$ grandi. 
Tuttavia anche qui vale $t_{pHL}= \cfrac{KC_{L}}{\beta_{neq}}\cdot \cfrac{1}{V_{DD}-V_{tn}}$ quindi aumenterà il propagation delay. 
Un altro aspetto da considerare è che la leakage power cresce con la temperatura quindi in generale va tenuta bassa.

Per ridurre la power consumption si può ridurre la grandezza dei componenti perché aumenta la leakage power. 

Un altro modo è analizzare la $f(x)$ da implementare, ci basiamo sulla probabilità $P_{0\rightarrow 1}$ di transire da 0 ad 1. 
Ad esempio per una NOR abbiamo 4 configurazioni di bit:

| A | B | Out |
| ---- | ---- | ---- |
| 0 | 0 | 1 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 0 |
Quindi l'output può essere 1 con una probabilità di $\cfrac{1}{4}$ mentre può essere 0 per $\cfrac{3}{4}$. 
Quindi la probabilità di transire da uno stato all'altro sarà:
$$
P_{0\rightarrow 1} = P_{out=0}\cdot P_{out=1} = P_{0}\cdot (1-P_{0})
$$
Quindi in questo caso si avrà: $P_{0\rightarrow 1} =\cfrac{3}{4}\cdot \cfrac{1}{4}= \cfrac{3}{16}$.
Questo se $P_{A=1}= \cfrac{1}{2}$ e $P_{B=1}= \cfrac{1}{2}$ ovvero sono equiprobabili, altrimenti dovrei usare la probabilità condizionata. 
L'importante è minimizzare la switching activity, in questo esempio è migliore la seconda soluzione:

![[probability.webp|center|350]]

## Glitchies

I glitchies sono dovuti ai cambiamenti di stato che provocano un propagation delay e sono un problema soprattutto quando si hanno molti livelli di locica poiché alterano il comportamento della logica temporaneamente.
È di fatto un valore temporaneo non desiderato che provoca un consumo di energia (nell'immagine quello rosso è il glitch).

![[glitch.webp|center|250]]

## Multi-$V_{dd}$

Si tratta di un modo per ridurre la power consumption in tutti i path non critici. Quello critico infatti non posso toccarlo perché altrimenti aumenterei il propagation delay, quindi riduco tutti gli altri.
Ovviamente bisogna anche stare attenti a non ridurlo troppo e farlo così diventare il critical path. 

## Multi-$V_{T}$

Qui consideriamo il fatto che possono essere transistor implementati con $V_{TnL}$ e $V_{TnH}$. Poi si fa lo stesso ragionamento di prima, non si tocca il critical path che sarà implementato con $V_{TnL}$ mentre gli altri li sostituisco con $V_{TnH}$ in modo da ridurre il power consumption riducendo la leakage current.

Queste tecniche permettono di ridurre fino all'80% la leakage power.