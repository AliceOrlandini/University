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
Nella rete di pull-down ho una condizione di OR ($+$) allora connetterò i *n-MOS* in parallelo, invece se ho una condizione di AND ($\cdot$) allora li connetterò in serie. 
Nella rete di pull-up si avrà la stessa cosa ma con i *p-MOS*.
Per implementare una qualsiasi funzione composta da due input $A$ e $B$ si usa:
- $\bar{Y} = f_{PDN}(A,B)$
- $Y = f_{PUN}(\bar{A},\bar{B})$
Una è sempre la duale dell'altra. 
Per verificare la correttezza, si testa mettendo in input dei valori e vedere come varia l'uscita (si può fare una tabella di verità).
Generalmente poi si trattano la pull-up e la pull-down come componenti di un inverter: 

![[complementary_cmos.webp|center|300]]

Quando si hanno transistor in parallelo, la conduttanza equivalente è la somma delle singole conduttanze: $\beta_{eq}= \sum\limits_{i=1}^{n}\beta_{i}$.
Mentre quando si hanno transistor in parallelo è: $\beta_{eq}= \cfrac{1}{\sum\limits_{i=1}^{n} \frac{1}{\beta_{i}}}$.
In questo modo posso stimare le caratteristiche della rete, ad esempio posso considerare la tabella di verità e calcolare per ogni uscita la conduttanza, basandomi sui transistor attivi.

![[complementary_cmos_example.webp|center|300]]

In questo caso, gli n-MOS sono responsabili del $t_{pHL} \propto \cfrac{KC}{\beta_{eqn}} = \cfrac{2KC}{\beta_{n}}$ e i p-MOS del $t_{pLH} \propto \cfrac{KC}{\beta_{eqp}} = \cfrac{KC}{\beta_{p}}$ considerando sempre il caso peggiore. 

Da design si può porre $t_{pLH} = t_{pHL}$ perché così è possibile scegliere il maggiore come tempo di propagazione massimo della rete sincronizzata, non avrebbe senso avere uno dei due ottimizzato e l'altro molto peggio. 
Se imponiamo questa condizione otteniamo: $\beta_{p}= \cfrac{\beta_{n}}{2}$.
La simmetria è garantita imponendo $W_{n}= W_{p}$.

## Comparazione tra NOR e la NAND

Proviamo a comparare NOR gate e NAND gate: prendiamo una porta NAND e una NOR e identifichiamo le condizioni per avere la *simmetria*, ovvero $t_{pHL}= t_{pLH}$. Ora definiamo i parametri, tipicamente $W_{n}$ e $W_{p}$ per avere lo stesso propagation delay sia nella NAND che nella NOR. 
Per fare ciò consideriamo:
$\beta_{eq}^{NAND} = \beta_{n}^{NAND} = \cfrac{\beta_{p}^{NAND}}{2}$
$\beta_{eq}^{NOR} = \beta_{n}^{NOR} = \cfrac{\beta_{p}^{NOR}}{2}$

Definiamo le grandezze della NOR:
$W_{n}^{NOR}= W$
$W_{p}^{NOR} = 4W$

Poniamo l'uguaglianza tra i due valori: 
$\beta_{n}^{NAND}= 2\beta_{n}^{NOR} \Rightarrow W_{n}^{NAND}=2W_{n}^{NOR} = 2W$
e:
$\beta_{p}^{NAND}= \cfrac{\beta_{p}^{NOR}}{2} \Rightarrow W_{p}^{NAND}=\cfrac{W_{p}^{NOR}}{2} = \cfrac{4W}{2W}=2W$

Quindi abbiamo lo stesso propagation delay e possiamo basarci sulla complessità dell'area. 
## Body Effect

Tutte le volte che si ha un block di CMOS, lo switch è più veloce se sto commutando un input che è vicino all’output. Tutta una questione di carica e scarica del condensatore.

Non usare le NAND con più di 4 input perché si deteriora il tempo di propagazione che cresce quadraticamente. (fan-in)
