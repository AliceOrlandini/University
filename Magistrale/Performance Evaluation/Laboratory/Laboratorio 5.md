## Implementation

Qui si applicano tutte le best practice nello scrivere codice e bisognerà sicuramente fare debugging. 
È diverso da programmare software normale perché non si sa cosa vogliamo ottenere ma vogliamo capire come funziona un sistema quindi debuggare richiede capacità addizionali.
Di solito si utilizza una tecnica chiamata *anti-bugging* che consiste nell’inserire del codice addizionale nel codice che aiuta a trovare gli errori. Ad esempio se ho a che fare con delle probabilità posso ogni tanto inserire delle righe per verificare che quella probabilità sia tra 0 e 1. Se troviamo un’anomalia si lancia un’eccezione e si ferma la simulazione.
Si può usare anche lo *structured testing* in cui ho delle costanti e cambiandone il valore si guarda come si comporta il simulatore (vedendo se rispetta le aspettative).

## Verification

Il fatto che il simulatore non crashi non significa che implementi il modello che ho preparato. Può sembrare banale ma è il momento della progettazione in cui si perde più tempo perché va validato tutto il simulatore e non il semplice pezzo di codice.
**Mai iniziare a collezionare i risultati prima di aver verificato il simulatore**
Per fare ciò si fanno delle simulazioni di test e si verifica la correttezza dei risultati. Si inizia da scenari semplici per poi andare sempre più nel complesso.

Attenzione alla differenza tra verification e validation.

Un altro modo per fare verification è utilizzare dei tool per l’incremental verification in cui si modifica una porzione di simulatore e poi si verifica che le altri parti di simulatore che non sono state modificate diano gli stessi risultati della simulazione precedenti. Non li vedremo nel corso ma se dovessimo lavorare per davvero si userebbero. 

Un altro modo è usare la paperella, ripetere ad un collega che può far emergere degli errori solo parlando. 

Bisogna fare attenzione che l’implementazione del modello potrebbe essere corretta ma potrebbero essere dei bug nella computazione delle statistiche. Quindi potremmo star cercando l’errore nel punto sbagliato. 

Poi ci sono dei test formali:
- Consistency test
- Degeneracy test: ad esempio testare il simulatore con zero utenti e vedere se ritorna qualcosa.
- Continuity test: ad esempio dato $input_{i}$ e faccio $input_{i}+ \Delta$ mi aspetto $output_{i}+ \Delta$.

Da notare che Nardini ha speso una slide sull’implementazione e 5 per la verifica, nel progetto staremo più a verificare che implementare. 

## Calibration

In questa fase si assegnano valori ai parametri. 
I parametri sono diversi dai fattori perché i parametri si assegnano una volta per tutte mentre i fattori vengono cambiati per effettuare simulazioni diverse. 
Quindi valori fissi sono parametri mentre variabili sono fattori. 
Bisogna sceglierli entrambi *realistici*.
Esempi di parametri sono il tempo di simulazione e il tempo di riscaldamento (warm-up).

## Experiment Design

In questo caso vado a:
1. definire il range di valori di interesse per ogni fattore (*livelli*).
2. rimuovere i fattori che hanno un impatto trascurabile.
Generalmente questo viene fatto empiricamente perché è molto difficile da fare ma ci sono dei metodi formali come l’analisi $2^{k}r!$ che vedremo con Stea.

## Run Simulation

Finalmente lanciamo la simulazione.
Questo processo può richiedere molto tempo a seconda della complessità del modello (a volte può richiedere settimane) quindi questo fatto del tempo va tenuto presente.
Per velocizzare il processo si possono rimuovere tutte le stampe di debugging o variabili che venivano usate per debuggare. 

Possiamo anche considerare l’utilizzo di:
- **PDES**: Parallel Discrete-Event Simulation.
- **MRIP**: Multiple Replications In Parallel (questo lo useremo) in cui se abbiamo 3 core nel pc si eseguono 3 simulazioni in parallelo. 

Se ci sono dei componenti indipendenti l’uno dall’altro si può considerare di eseguirli separatamente, questo però non succede quasi mai perché nei simulatori è generalmente tutto collegato.

## Data Collection

## Data analysis

Per fare ciò si usano grafici di tutti i tipi. 