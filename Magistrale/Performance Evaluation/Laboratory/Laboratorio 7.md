# Analyzing output data

## Representativeness

Anche scegliendo il warm-up period corretto ed effettuando più simulazioni, vedremo che lo steady state sarà diverso fra tutte. Per capire qual è lo steady state corretto si può prendere il valor medio.

## IIDness

Un altro fattore importante è l'indipendenza tra i punti della traiettoria, la correlazione tuttavia è inevitabile. Dobbiamo quindi trovare un modo per ottenere osservazioni IID. Ci sono 2 modi per fare ciò:
1. **Usare repliche indipendenti dello stesso scenario**: si runnano N replica della stesso scenario con condizioni iniziali indipendenti, scartando N initial transient. Poi si calcola il valor medio e i confidence intervals. 
2. **Batch means**: si fa una sola simulazione ma molto lunga e si divide in N intervalli. Si assume che tutti gli intervalli siano indipendenti. Generalmente rimangono dipendenti l'uno dall'altro infatti bisogna scegliere adeguatamente gli intervalli. Il vantaggio è che posso scartare solo 1 initial transient.
Nardini preferisce il primo metodo perché non abbiamo problemi a runnare N simulazioni e a memorizzare i dati. 

## Displaying results

Ovviamente non dobbiamo mostrare un tabellone di dati perché sarebbero insignificanti, è importante quindi capire qual è il modo migliore per rappresentare i dati. 
*"WHAT THE... FUCK" cit. Nardini*
In ambiente accademico presentare adeguatamente i risultati è fondamentale perché anche se il lavoro è corretto, potrebbero non credere se la rappresentazione finale non è chiara. 

I grafici più comuni sono:
- **Bar Chart**: utile quando si hanno categorie differenti da mostrare e per ognuna di esse si vogliono mostrare dei valori assoluti. 
- **Line Chart**: utile quando si vuole mostrare qualcosa in una linea temporale, o con una certa sequenzialità.
- **Pie Chart**: utile quando si ha un valore assoluto che sarà il 100% e si vuole vedere come si divide tra i fattori. 
- **Scatter Plot**: utile per vedere i cluster. 
Possiamo scegliere quello che vogliamo ma teniamo conto di cosa vogliamo trasmettere. 

Alcune best practice:
- mostrare in un grafico un numero ragionevole di curve, altrimenti diventa incomprensibile. 
- selezionare colori adeguati, per esempio ogni categoria deve avere i propri colori, oppure la divergenza può essere rappresentata da una linea che da arancione diventa rossa.
- indicare sempre le label per le assi e le unità di misura, per esempio c'è un delay di $10\mu s$ è molto diverso se quest'ultimo è di $10s$. 
- indicare delle caption per spiegare cosa rappresenta il grafico.
- usare delle scale coerenti per le assi, partire sempre da zero, volendo si può zoommare per mostrare il dettaglio, ma il grafico che parte da zero va messo. 
- se due grafici devono essere comparati devono avere la stessa scala.
- rappresentare sem
È concesso utilizzare dei metodi automatici come ad esempio Excel o Python. 



