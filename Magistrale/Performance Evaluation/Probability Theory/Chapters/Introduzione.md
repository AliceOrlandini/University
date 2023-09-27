Gli scopi del corso *Performance Evaluation* sono:
1. fornire gli strumenti per **determinare se un sistema è migliore** di un altro secondo specifici criteri.
2. **progettare un sistema con specifici requisiti** (ad esempio un sistema con un tempo di risposta inferiore a 10ms per il 90% delle richieste)

# Generalità sui sistemi

Un sistema $A$ che avrà un input chiamato *workload* e un output chiamato *performance metric*. 
Un workload ad esempio può essere una richiesta di una pagina web mentre le performance metric possono ad esempio essere: il tempo di risposta, l'energia consumata o il throughput (quantità di dati inviati nell'unità di tempo).
Le performance metric si dividono in *Lower is Better* (LB) ed *Higher is Better* (HB), nel primo caso infatti, se abbiamo più sistemi, si preferisce il sistema con metrica minore mentre nel secondo caso quello con metrica maggiore.

## Esempio di confronto tra sistemi

Consideriamo la seguente tabella:

| Sistema | Potenza (w) | Tempo di Risposta (ms) | Throughput (tps) |
| ------- | ----------- | ---------------------- | ---------------- |
| A       | 23.5        | 3.78                   | 42.2             |
| B       | 40.8        | 5.30                   | 29.1             |
| C       | 92.7        | 4.03                   | 22.6             |
| D       | 53.1        | 2.19                   | 73.1             |
| E       | 54.7        | 5.92                   | 23.3             |
| -       | LB          | LB                     | HB                 |

Potenza e tempo di risposta sono due performance metric *LB* poiché si preferisce il sistema con potenza minore e tempo di risposta minore.
Il throughput invece è una performance metric *HL* in quanto è un parametro che rappresenta la quantità di dati trasmessi nell’unità di tempo per cui noi vogliamo che il numero di dati trasmetti sia elevato.

Analizziamo la tabella:
- Il sistema D è il sistema con performance migliori perché ha il throughput maggiore tra tutti e il tempo di risposta minore, la potenza è più o meno nella media.
- Se invece consideriamo la potenza consumata (considerando che viviamo in un periodo storico di crisi energetica) il sistema A è colui che garantisce il minor consumo di potenza con un buon tempo di risposta e throughput.
- Gli altri tre sistemi sono totalmente superflui perché, se esistono A e D, possiamo farne a meno visto che nemmeno una delle performance metric è migliore di quelle di A e D.

*Ma questi numeri da dove arrivano?* 
La prima parte del corso sarà proprio su come determinare tali valori e solo successivamente studieremo tecniche per ridurli/aumentarli (a seconda che siano LB o HB).

# Modelli per rappresentare i sistemi

Un altro aspetto importante è realizzare dei **modelli** per poi effettuare dei testi con dei [[#Generalità sui sistemi|workload]]; in questo modo potremo stabilire qual è il sistema migliore prima di realizzarlo effettivamente (con un cospicuo risparmio di soldi).

Un modello può essere realizzato tramite:
1. **Equazioni**: $o = f(i)$ usate solo per sistemi semplici
2. **Simulazioni**: $o_j = f(i_j)$ si va a simulare il comportamento del sistema in relazione allo specifico input. Questo metodo è utilizzabile con qualsiasi sistema a patto di avere il tempo per la realizzazione della simulazione stessa. 

Dovremo avere a che fare anche con la *casualità* dei dati per cui inizieremo il corso con lo studio dei [[Teoria della probabilità|principi di probabilità]].