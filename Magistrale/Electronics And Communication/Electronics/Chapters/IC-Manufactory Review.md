In questa lezione faremo un ripasso degli argomenti principali del programma di Elettronica Digitale. Infatti, noi con VHDL lavoreremo ad un livello di astrazione elevato ma bisogna comunque sapere “cosa c’è sotto”.
# Transistore MOSFET

![Mosfet|center|300](https://www.edutecnica.it/elettronica/mosfet/1.png)

Nel transistore MOSFET si hanno 3 terminali: **Source**, **Drain** e **Gate**.
La larghezza del gate $W$ e la distanza tra le zone drogate di tipo $n^+$ determinano le caratteristiche elettriche del transistor:

![[mosfet.png|center|400]]

Un problema che si è iniziato a riscontrare intorno al 1980 è quello delle scariche elettrostatiche che provocavano una foratura del dielettrico. Per evitare ciò si abbassò gradualmente $V_{DD}$ da un valore di partenza di 5V a 3.3V, 2.5V, 1.8V …
Il problema però di abbassare la tensione è che il rumore può diventare preponderante tanto da non riuscire a riconoscere il segnale. 
L’**NMOS** può essere rappresentato nel seguente modo:

![[nmos.png|center|300]]

Il **PMOS** è il duale dell’NMOS e può essere rappresentato nel seguente modo: 

![[pmos.png|center|300]]

In generale, sia NMOS che PMOS possono essere visti come interruttori ideali. 

# Logica Pass-Gate

La **logica pass-gate** viene utilizzata per la progettazione di varie famiglie di circuiti integrati. Il vantaggio di utilizzare questa tecnologia è che si riduce al massimo il numero di transistor utilizzati per implementare una certa funzione logica poiché si vanno infatti ad utilizzare i [[#Transistore MOSFET|MOSFET]] come interruttori. 

Quando si utilizza questa logica bisogna considerare: 
- tempo di propagazione
- consumo di potenza dinamica
- area occupata

Inoltre, è bene ricordarsi che quando si utilizza un nmos allora non si riuscirà a raggiungere il livello alto pieno. Nel caso del pmos invece non si riuscirà a raggiungere il livello basso pieno.
Per ovviare a ciò, si pone un nmos e un pmos in parallelo. L’area totale sarà quella occupata dai due transistor e da i collegamenti necessari per connettere ingresso e uscita. 

Un nmos può essere rappresentato con una resistenza equivalente di valore: $$R = \frac{1}{\beta _n(V_{GS}-V_{T})} = \frac{1}{\beta _n (4-V_U)}$$
che ha un andamento di tipo *iperbolico* all’aumentare di $V_U$:

![[nmos R.png|center|300]]


Se si pongono un nmos e un pmos in parallelo allora le resistenze varranno:
- $R_n = \frac{1}{\beta _n (4 -V_U)}$
- $R_p = \frac{1}{\beta _p (4 -V_U)}$
# Inverter CMOS

L’inverter CMOS implementa la funzione logica **NOT**:

![Inverter CMOS|center|300](https://media.geeksforgeeks.org/wp-content/uploads/20220831213130/CMOSTechnologyCMOSInverter.jpg)

La caratteristica è fatta nel seguente modo:

![Inverter caratteristica|center|300](https://gs-post-images.grdp.co/2022/8/vtc-of-cmos-img1660036851929-59.png-rs-high-webp.png?noResize=1)

## Tempo di propagazione inverter CMOS

Il tempo di propagazione è il tempo che intercorre tra la variazione dell’ingresso e la conseguente variazione dell’uscita.
Può essere calcolato nel seguente modo:
- $t_{pHL} = \cfrac{KC}{\beta _n}\cfrac{1}{V_{DD}-V_{Tn}}$
- $t_{pLH} = \cfrac{KC}{\beta _p}\cfrac{1}{V_{DD}-|V_{Tp}|}$
Da notare che questo è proporzionale alla capacità del condensatore $C$, alle caratteristiche dei mosfet $\beta _n = \mu _n C_{ox}\frac{W_n}{L_n}$ e alla tensione di threshold $V_{Tn}$. Inoltre, bisogna anche considerare che la mobilità $\mu _n$ dipende dalla temperatura, in particolare, se la temperatura aumenta, quest’ultimo diminuisce perché la vibrazione degli atomi aumenta e le particelle hanno molta più energia; questo significa che si verificheranno molti più urti che renderanno più difficoltoso il movimento della particella.

## Consumo di energia Inverter CMOS

Il consumo di energia dinamica è pari a $E_D = CV^2$ mentre il consumo di potenza sarà $P_D = \frac{E_D}{T}$. Anche qui si nota che dipende dalla capacità del condensatore $C$.
# Review of IC Manufacturing

La fabbricazione di dispositivi a semiconduttore è il processo usato per realizzare i circuiti integrati e i chip che sono presenti nella maggior parte dei dispositivi elettronici.

Tale processo industriale è messo in atto attraverso molteplici fasi, che implicano l’uso di tecnologie fotolitografiche e chimico-fisiche, durante le quali i circuiti elettronici sono gradualmente realizzati su un substrato (il cosiddetto [[#Wafer|wafer]]) costituito da un unico cristallo di un semiconduttore ad elevatissima purezza.

Vediamo nel dettaglio le fasi di lavorazione: 
## Wafer 

Nella maggior parte dei casi, per realizzare il wafer viene impiegato il silicio. Il processo di produzione del wafer richiede l’uso di fabbriche altamente specializzate, e quindi estremamente costose, ne consegue che il settore dei semiconduttori ha necessità di realizzare grendi economie di scale, raggiungere alte rese dei processi e massimizzare la produttività.

## Processo produttivo

Nel seguente sito viene spiegato molto bene il processo di costruzione di un transistore (sicuramente è spiegato meglio di come lo ha spiegato Fanucci…): [Costruzione Transistor](https://www.micheleangeletti.it/articoli/140725-costruzione-di-un-processore.html)
