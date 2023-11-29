# Output data analysis

Lo scopo è quello di analizzare i dati ricevuti dalla simulazione. Interagiremo quindi con:
- variabili aleatorie: un singolo valore casuale.
- processi stocastici: valori casuali ma nel tempo, ogni valore della sequenza è una variabile aleatorie. 
La prima cosa da ricordare è che abbiamo un output casuale che segue la casualità dell’input, in particolare è legato al seed. Di conseguenza, dobbiamo ripetere la simulazione per molti seed per avere risultati diversi.
Siamo interessati a capire come il sistema di comporta:
- per lungo tempo.
- in condizioni operative *normali* (steady state).
Dobbiamo quindi escludere gli output ottenuti durante il periodo di *warm-up* (ad esempio quando la coda è vuota). Per fare ciò dobbiamo estimare la lunghezza del transitorio dal warm-up allo steady state, e capire quanto è lunga la simulazione. 

## Stimare la lunghezza del warm-up

Un approccio sbagliato è quello di dire “escludo il 10% della simulazione”. Non ha senso perché vogliamo escludere solo le parti che non ci interessano e non un valore fisso. Le simulazioni già durato tanto quindi non vogliamo escludere dati utili.
Un modo per stimare il periodo di warm-up è quello di basarsi sull’esperienza acquisita studiando il sistema. 
L’approccio ideale è quello di utilizzare la statistical inference: fare $n$ repliche dello scenario collezionando i dati da stimare e infine usando dei test statistici sul sample. 
Lo svantaggio è quello relativo alla necessità di runnare la simulazione varie volte che impiega tempo.

Un altro modo è quello di utilizzare i vettori generati tramite OMNeT++ e applicare l’operatore media su intervalli temporali sempre più grandi. Facendo ciò si passa da qualcosa di caotico a qualcosa che converge. Tuttavia, questo è il modo di vedere come la *media* sta convergendo. Per evitare ciò, invece di utilizzare l’operatore media si usa ma tramite window ovvero intervalli temporali fissi che vengono traslati (in OMNeT si fa facilmente impostando un parametro nell’INI).

## Simulation Duration

Quanto deve essere lunga la simulazione per ottenere risultati significativi?
Un primo modo è ancora una volta quello di usare il buon senso e l’esperienza. 

