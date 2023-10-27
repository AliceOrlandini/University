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
3. i tempi di propagazione 
	$t_{pHL} \propto \frac{KC}{\beta_{n}} \frac{1}{V_{DD}+V_{Tn}}$ 
	$t_{pLH} \propto \frac{KC}{\beta_{p}} \frac{1}{V_{DD}+V_{Tp}}$
	$\beta_{n}= \mu_{n}C_{ox} \frac{W_{n}}{L_{n}}$
	$\beta_{p}= \mu_{p}C_{ox} \frac{W_{p}}{L_{p}}$
