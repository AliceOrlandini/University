Noi ci concentreremo sui Flip-Flop edge triggered che, a differenza del Latch, *non* è trasparente. 
Ciò che si vuole è che il fronte di saluta del clock arrivi a tutti gli elementi del circuito nello stesso istante. Invece, in generale c’è un ritardo $\tau$ chiamato **clock skew**.
Ad esempio nell’FPGA avevamo detto che c’era una linea solo per il clock. 
Spesso si utilizza un approccio Global Asyncronous Local Syncronous (**GALS**) in cui abbiamo delle zone localmente sincronizzate. Ad esempio 4 zone in cui i dispositivi sono sincronizzati con rispettivamente $clock_{1}$, $clock_{2}$, $clock_{3}$ e $clock_{4}$.
# Timing Classifications

1. Sistemi sincronizzati: quelli che vedremo noi.
2. Sistemi asincroni
3. Sistemi ibridi: i GALS.

La regola di pilotaggio fondamentale è che il dato in input sia stabile per almeno $t_{setup}$ tempo prima del fronte di salita del clock e $t_{hold}$ dopo. L’output del registro si adeguerà dopo $t_{\text{p logic}}$ che rappresenta la differenza tra il fronte di salita del clock e il momento in cui l’output si adegua e diventa stabile.

Un altro parametro è la *contamination delay* cioè la differenza tra il fronte di salita del clock e la prima modifica dell’output che può non essere l’output finale, per questo viene chiamato contaminazione $t_{\text{ed logic}}$.

