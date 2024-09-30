### Principi e concorrenza nella programmazione funzionale

Un paradigma di programmazione è un insieme di principi e strategie che definiscono un *approccio* per risolvere problemi informatici. In pratica, è una modalità di pensiero che guida il modo in cui sviluppiamo e organizziamo il codice per raggiungere un determinato obiettivo. La scelta del paradigma influisce non solo sulla struttura del programma, ma anche sulle sue *performance* e sulla sua *manutenibilità*.

I principali paradigmi si collocano su un ampio spettro, con due estremi significativi:
- **Paradigma imperativo**: il più comune nella programmazione tradizionale, si basa su una sequenza di istruzioni che modificano lo *stato del programma* e controllano il flusso di esecuzione. In questo approccio, il programmatore indica esplicitamente come il calcolo deve essere eseguito, passo dopo passo.

```python
if(x > 0): 
	result = 15/3
else:
	result = 2*3
result = result - 1
```

- **Paradigma funzionale**: in questo modello, non esiste uno stato mutabile; le variabili, una volta assegnate, restano costanti. L'elaborazione avviene tramite la valutazione di espressioni piuttosto che attraverso l'esecuzione di comandi. Questo approccio riduce la possibilità di *effetti collaterali*, rendendo il codice più *prevedibile* e spesso più adatto alla parallelizzazione.

```erlang
(15/3 if x > 0 else 2*3) - 1
(5 if 7 > 0 else 6) - 1
5 - 1
4
```

Altri paradigmi, come quello logico o orientato agli oggetti, si trovano tra questi due estremi, ciascuno con caratteristiche specifiche per affrontare problemi in modo diverso.

Vediamo ora alcuni concetti della programmazione funzionale che non sono comuni in quella imperativa.

#### Expression & Referential Transparency

Le computazioni in ambito funzionale consistono nella *valutazione di espressioni*. Un'espressione si definisce **referentially transparent** se può essere sostituita dal suo valore risultante senza alterare il comportamento del programma. In questo caso, l'espressione è priva di effetti collaterali (*side effects*).

Una funzione è detta **pura** quando la sua chiamata è referentially transparent, il che significa che l'esecuzione della funzione non ha effetti collaterali esterni e restituisce sempre lo stesso risultato per gli stessi argomenti.

Quando si verifica la referential transparency, è possibile formalizzare il programma come un **sistema di riscrittura** (rewriting system), composto da oggetti e regole che ne modificano lo stato. Questo approccio risulta particolarmente utile per la **verifica automatica** del codice, per l'**ottimizzazione** e per la **parallelizzazione** delle operazioni, poiché l'assenza di effetti collaterali garantisce un comportamento prevedibile e indipendente dal contesto.

Inoltre, questo concetto abilita l'uso della **memo-ization**: supponiamo di avere una funzione costosa in termini computazionali, ad esempio `f`, e di dover calcolare più volte `f(10)` all'interno del codice. Invece di rieseguire la funzione ogni volta, possiamo memorizzare il risultato in una struttura dati come una hash table, migliorando significativamente le prestazioni. Questo tipo di ottimizzazione è possibile solo in presenza di funzioni pure, ovvero prive di effetti collaterali, rendendolo ideale per sistemi robusti e formalmente verificabili.

#### Lack of State

Nella programmazione funzionale, *non esiste uno stato mutabile*: le variabili, una volta assegnate, non possono essere modificate e agiscono quindi come costanti. Invece di aggiornare i valori esistenti, si creano nuove "variabili" per rappresentare i nuovi dati.

La gestione della memoria in questi contesti è affidata al **garbage collector**, che si occupa di liberare automaticamente la memoria occupata da variabili che non sono più in uso.

#### Eager vs Lazy Evaluation

La valutazione delle espressioni, in particolare per gli argomenti delle funzioni, può essere gestita tramite due principali strategie:
- **Eager evaluation**: esegue il calcolo delle espressioni *il prima possibile*. Questa strategia è comunemente utilizzata nei linguaggi che adottano il passaggio per valore o per riferimento, poiché calcola immediatamente il risultato delle espressioni non appena vengono incontrate.
- **Lazy evaluation**: posticipa la valutazione delle espressioni fino a quando il loro risultato è *strettamente necessario*. Questo approccio, noto anche come **call-by-need**, viene usato per evitare computazioni inutili e può essere più efficiente in determinati contesti, specialmente quando non tutte le espressioni vengono effettivamente utilizzate nel corso dell'esecuzione del programma.

In alcuni casi, la lazy evaluation permette di gestire strutture dati infinite o evitare calcoli pesanti fino a quando non sono richiesti, ottimizzando così le risorse computazionali.

#### First-class & Higher-order Functions

Le funzioni, nei linguaggi che supportano la programmazione funzionale, sono trattate come **first-class citizens**, ovvero possono essere manipolate come qualsiasi altro valore. Ciò significa che possono essere passate come argomenti ad altre funzioni, restituite come risultato da una funzione, o assegnate a variabili.

Le **Higher-order functions** sono quelle che accettano altre funzioni come parametri o che restituiscono funzioni come risultato. Queste funzioni offrono un alto livello di astrazione, permettendo la costruzione di comportamenti complessi in modo modulare e riutilizzabile.

In alcuni casi, le funzioni restituite dipendono dai parametri della funzione che le genera. Per preservare questi valori nel tempo, si utilizzano le **closures**. Una closure è una funzione che "ricorda" l'ambiente in cui è stata creata, mantenendo accesso alle variabili locali anche dopo che la funzione esterna è terminata. Questo consente alle closure di gestire e mantenere lo stato in modo efficace senza ricorrere a variabili globali o mutabili.

#### Uso della Ricorsione

Come è noto, ogni operazione eseguibile con la ricorsione può essere eseguita anche tramite iterazione, e viceversa. Tuttavia, nei linguaggi funzionali, dove *non esistono variabili mutabili* e quindi non si mantiene uno stato, la ricorsione diventa l'unico strumento disponibile per ripetere computazioni.

Uno svantaggio della ricorsione è la gestione della memoria, specialmente nel caso della **tail recursion** (ricorsione di coda). Se una funzione ricorsiva effettua come ultima operazione una nuova chiamata a se stessa, senza particolari ottimizzazioni, questo può causare problemi di performance e portare al superamento dei *limiti dello stack*.

Per mitigare questi problemi, molti linguaggi supportano la **Tail Call Optimization**. In pratica, se l'ultima operazione eseguita da una funzione è una chiamata a un'altra funzione (o a se stessa), il linguaggio può ottimizzare l'esecuzione sostituendo il frame corrente della funzione con quello della successiva, evitando di mantenere entrambi. Questo riduce l'uso dello stack e migliora le prestazioni in scenari ricorsivi.

Alcuni linguaggi, come Python, non implementano questa ottimizzazione, limitando l'efficienza della ricorsione. Al contrario, in linguaggi come C++, è possibile attivare la tail call optimization utilizzando l'opzione di compilazione `-O2` per migliorare le performance e ridurre l'uso della memoria nello stack.

![[Tail Call Optimization.png|center|400]]


#### Uso di strutture dati ricorsive

Nei linguaggi funzionali, dove la ricorsione è l'unico modo per ripetere computazioni, le **strutture dati ricorsive** come le liste diventano particolarmente utili. Una lista può essere vista come composta da due parti: una *"testa"*, che rappresenta il primo elemento, e una *"coda"*, che è il resto della lista. Poiché la coda è anch'essa una lista, questa definizione è intrinsecamente ricorsiva.

Questo approccio ricorsivo alle strutture dati si adatta perfettamente alla logica funzionale, dove non esiste uno stato mutabile. Ad esempio, non possiamo utilizzare strutture come gli array che mantengono uno stato interno mutabile, perché ciò violerebbe il principio di assenza di effetti collaterali. Le liste, invece, permettono di costruire e manipolare dati in modo sicuro e senza mutazione, garantendo che ogni nuova operazione produca una nuova lista senza alterare quella originale.
### Introduzione ad Erlang

### Il modello Actor

### Going concurrent & distributed actually