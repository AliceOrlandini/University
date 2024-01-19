# Review on Sync and Async Sequential Logic

Oggi vediamo in dettaglio com’è fatta la libreria della fundry.
Quando parliamo di logica dividiamo in:
- Logica combinatoria: l’output è funzione dell’input
- logica sequenziale: l’output è funzione dell’input e dagli input precedenti, c’è il concetto di memoria.
Quando si parla di logica sequenziale si hanno due opzioni:
- asincroni: quando l’output cambia immediatamente con l’input (trasparente).
- sincroni: quando l’output cambia in corrispondenza del fronte di salita del clock.
La maggior parte delle volte si opta per un approccio sincrono a meno che non si abbiano device semplici in cui si vuole ottimizzare il consumo di energia.

Mealy è asincrona, infatti c’è un collegamento diretto tra ingresso e uscita mentre Moore è sincrona perché tra input e output c’è un registro.
L’esempio classico è il riconoscitore di sequenze. 
Mealy può essere sempre trasformata in una rete sincrona chiamata Mealy Ritardata in cui si aggiunge un registro tra ingresso e uscita.
Gli elementi di queste reti sono un flip flop e una rete combinatoria. 

Quando vogliamo cambiare la tensione sul condensatore presente in uscita ci sono due modi: 
- hard node (static): il nodo di output è fisso ad un valore che può essere o 0 o Vcc, un esempio è l’inverter. È molto robusta come soluzione perché non risente troppo del rumore. 
- soft node (dynamic): quando si memorizza il valore in una capacità, questo non è tanto robusto. Un esempio è il pass-transistor. 

# Complementary CMOS Logic

Negli sheet delle librerie troviamo:
1. il simbolo circuitale
2. la tabella di verità
3. la capacità dell’input
4. i tempi di propagazione 
	$t_{pHL} \propto \frac{KC}{\beta_{n}} \frac{1}{V_{DD}+V_{Tn}}$ 
	$t_{pLH} \propto \frac{KC}{\beta_{p}} \frac{1}{V_{DD}+V_{Tp}}$
	$\beta_{n}= \mu_{n}C_{ox} \frac{W_{n}}{L_{n}}$
	$\beta_{p}= \mu_{p}C_{ox} \frac{W_{p}}{L_{p}}$
5. la temporizzazione
6. la strength: forza necessaria per pilotare il carico in uscita, 1 indica la corrente che riesce a erogare la cella ed è legato al tempo di propagazione. Maggiore è la strenght maggiore è la corrente e quindi la carico più velocemente. 

La tecnologia CMOS complementare è composta da una rete di pull up (PUN) e una di pull down (PDN). Stesse cose viste con Piotto.

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
