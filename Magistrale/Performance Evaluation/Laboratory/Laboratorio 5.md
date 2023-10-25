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

