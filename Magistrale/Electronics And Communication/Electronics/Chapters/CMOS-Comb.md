# Review on Sync and Async Sequential Logic

Oggi vediamo in dettaglio com’è fatta e da dove vengono i blocchi della la libreria della fundry al fine di capire come funzionano i tool di sintesi.

Quando parliamo di logica dividiamo in:
- **Logica Combinatoria**: l’output è funzione dell’input.
- **Logica sequenziale**: l’output è funzione dell’input e dagli input precedenti, c’è quindi il concetto di memoria.
Quando si parla di logica sequenziale si hanno due opzioni:
- **asincroni**: quando l’output cambia immediatamente con l’input (trasparenza).
- **sincroni**: quando l’output cambia in corrispondenza del fronte di salita del *clock*. Questa tipologia di logica permette di specificare un timing constraint. 
La maggior parte delle volte si opta per un approccio sincrono a meno che non si abbiano device semplici di cui si vuole ottimizzare il consumo di potenza. Nel metodo sincrono infatti c'è un po' di overhead che incrementa il consumo di energia, tuttavia questo metodo è anche quello più robusto, soprattuto in presenza di feedback loops (provoca problemi di stabilità). 

La rete di **Mealy** è un esempio di rete asincrona, infatti c’è un collegamento diretto tra ingresso e uscita mentre in quella di **Moore** è sincrona perché tra input e output c’è sempre un registro.
Mealy può essere sempre trasformata in una rete sincrona chiamata **Mealy Ritardata** in cui si aggiunge un registro tra ingresso e uscita.
In generale gli elementi di queste reti sono un *D-Flip-Flop* e una *rete combinatoria*, è quindi importante capire le loro caratteristiche perché tramite combinazioni di questi è possibile realizzare praticamente qualsiasi microprocessore. 

Per costruire una macchina a stati finiti (ad esempio se volessi imporre una tensione su un condensatore) ci sono due approcci:
- **hard node** (static logic): il nodo di output è fisso ad un valore che può essere o $0$ o $V_{dd}$, un esempio è l’inverter. Questo significa che a meno di un cambiamento dell'input, l'output non cambierò mai. È molto *robusta* come soluzione perché non risente troppo del rumore, tuttavia richiede di avere l'energia fissa collegata. Un esempio è l'inverter collegato ad un condensatore. 
- **soft node** (dynamic logic): quando si memorizza il valore in una capacità parassita, questo *non è tanto robusto* ma permette di staccare la potenza per poi effettuare dei refresh del valore. Un esempio è il pass-transistor collegato ad un condensatore. 

# Complementary CMOS Logic

Negli sheet delle librerie troviamo:
1. Il simbolo circuitale.
2. La tabella di verità.
3. La capacità dell’input.
4. I propagation delay, riferiti sempre ad un technology corner, sono: 
	$t_{pHL} \propto \cfrac{KC}{\beta_{n}}\cdot \cfrac{1}{V_{DD}+V_{Tn}}$ 
	$t_{pLH} \propto \cfrac{KC}{\beta_{p}}\cdot \cfrac{1}{V_{DD}+V_{Tp}}$
	$\beta_{n}= \mu_{n}\cdot C_{ox}\cdot \cfrac{W_{n}}{L_{n}}$
	$\beta_{p}= \mu_{p}\cdot C_{ox}\cdot \cfrac{W_{p}}{L_{p}}$
	$A \approx W_{n}\cdot L_{n}+ W_{p}\cdot L_{p}$
5.  L'area della cella.
6. la strength: forza della cella necessaria per pilotare il carico in uscita, 1 indica la corrente che riesce a erogare la cella ed è legato al tempo di propagazione della cella quando deve pilotare un carico in uscita capacitivo. Maggiore è la strenght maggiore è la corrente erogata e quindi a parità di carico, si riesce a caricare o scaricare più velocemente la capacità in uscita. 

I **technology corner** sono relativi al dado, infatti non tutti hanno le stesse caratteristiche e vengono catalogati generalmente in *typical*, *fast* oppure *slow*, e tutti quei dati al di fuori di questi tre range vengono buttati. Poi considerano anche il fatto che il propagation delay dipende dall'utilizzo che si fa del circuito, oppure ad esempio dalla temperature alle quali viene utilizzato. Tutte queste cose cambiano le performance del circuito. 
I technology corner sono rappresentati nel seguente modo:

![[tech_corners.webp|center|300]]

# Complementary CMOS 

La tecnologia Complementary CMOS permette di implementare qualsiasi funzione logica ed è composta da una rete di pull-up (**PUN**) e una di pull-down (**PDN**).
Nella rete di pull-down ho una condizione di OR ($+$) allora connetterò i n-CMOS in parallelo, invece se ho una condizione di AND ($\cdot$) allora li connetterò in serie. 
Nella rete di pull-up si avrà la stessa cosa 

Da design si può porre $t_{pLH} = t_{pHL}$ perché così possiamo scegliere il maggiore come tempo di propagazione massimo della rete sincronizzata.
La simmetria è garantita da $W_{n}= W_{p}$.
Inoltre è meglio avere dei paralleli per la mobilità.

## Compariamo la NOR2 e la NAND2

$\beta_{eq}^{NAND} = \beta_{n}^{NAND} = \frac{\beta_{p}^{NAND}}{2}$
$\beta_{eq}^{NOR} = \beta_{n}^{NOR} = \frac{\beta_{p}^{NOR}}{2}$
$W_{n}^{NOR}= W$
$W_{p}^{NOR} = 4W$

$\beta_{n}^{NAND}= 2\beta_{n}^{NOR}$
$W_{n}^{NAND}=2W_{n}^{NOR} = 2W$

$\beta_{p}^{NAND}= \frac{\beta_{p}^{NOR}}{2}$
$W_{p}^{NAND}=\frac{W_{p}^{NOR}}{2} = \frac{4W}{2W}=2W$

## Body Effect

Tutte le volte che si ha un block di CMOS, lo switch è più veloce se sto commutando un input che è vicino all’output. Tutta una questione di carica e scarica del condensatore.

Non usare le NAND con più di 4 input perché si deteriora il tempo di propagazione che cresce quadraticamente. (fan-in)
