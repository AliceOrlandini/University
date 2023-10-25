Ecco alcuni problemi:
- I document database molte volte non hanno supporto per le funzioni dei database relazionali come ad esempio SQL-like query language. 
- Il problema del key value me lo sono perso

All'inizio si cercò di lasciare il database relazionale ma includendo le operazioni di analisi, nel 2006 Google decise di sviluppare il suo database chiamato **Big Table**. 
I dati erano ancora memorizzati in tabelle ma con alcune caratteristiche:
- era organizzato in *colonne* sul disco
- i dati venivano indicizzati con: *riga*, *nome della colonna* e un *timestamp*.
- le operazioni di lettura e scrittura erano *atomiche*
- le righe venivano mantenute *ordinate*
È una sorta di key based ma sempre relazionale perché dal *row id* si identifica un'informazione (non una riga, perché in una riga possiamo mettere più informazioni).

![Google Big Table|center|700](https://cloud.google.com/bigtable/img/storage-model.svg)

Ad alto livello ci saranno molti valori null ma a basso livello il database sarà ottimizzato a colonna. 
Una volta definita la lista delle column family il sistema si occuperà di allocarle sul disco in modo ottimizzato.
La column family è un insieme di attributi che di solito vengono usati insieme per eseguire le statistiche. Le colonne della famiglia verranno allocate vicine in memoria.  

Quindi data una row key, si individuano le column family e oltre ai valori si ha anche il timestamp. 
Un’altra funzione riguarda l’ordinamento per un attributo, questa operazione può essere fatta efficientemente perché è possibile comprimere il quantitativo di informazioni tramite algoritmi specifici.

# Cluster and Partitions

I column database sono progettati per garantire alti livelli di *availability* e *scalability* con diversi livelli di *consistency*. 
Si può realizzare un’architettura distribuita basata su partizioni e cluster. 
Come possiamo partizionare le informazioni?
La partition key può essere il nome di una colonna, di una riga o di un insieme di colonne che vogliamo usare insieme (le column family). Sulla base di ciò, si possono partizionare le informazioni tra i vari server. 
Da notare che siamo dividendo i dati per colonna, quindi su un server potremo avere dati incompleti dal punto di vista delle righe ma per le analisi saranno tutti.
Possiamo applicare le tabelle hash per distribuire le informazioni e per sapere dove accedere, lo stesso principio dei key-value database.

Cassandra ha un’architettura peer to peer, si usa per grandi quantità di dati per cui abbiamo bisogno di numerosi cluster. 
Un’alternativa è Apace HBase Architecture sviluppata da Google in cui HDFS file system, un file system in cui si ha un master server che comunica con una lista di regions che contengono una partizione dei dati. Il master assegna e gestisce le comunicazioni tra region server e client. Un nodo di Hadoop è lo Zookeeper che permette la coordinazione tra noti con cluster Hadoop. 
![Apache HBase|center|600](https://media.geeksforgeeks.org/wp-content/uploads/1111-7.png)

Il problema di questa architettura è che il master e lo zookeeper rappresentano un single point of failure. 

In cassandra abbiamo che su ogni nodo gira lo stesso software, non si ha un single point of failure ed è facile aggiungere e rimuovere server applicando ciò che abbiamo già visto con il key-value.

![Cassandra|center|600](https://cassandra.apache.org/_/_images/diagrams/apache-cassandra-diagrams-01.jpg)

Vediamo ora una serie di tools che vengono usati con i column databases (ma possono essere usati in generale per i database distribuiti). 

# Tools
## Commit Logs

> [!note]
> Un **commit** è la *conclusione* di una *transazione*.
> Il commit log è un file in cui vengono aggiunte righe (append) relative alle transizioni.

Questo tipo di database usa i commit logs per velocizzare le transizioni, questo perché se faccio una serie di operazioni di update sul database le scriverò sul commit logs e qualche entità prima o poi le eseguirà sul database. 
È molto comodo anche per propagare informazioni su server differenti. 

## Bloom Filters

> [!note]
> Un **bloom filter** è una struttura dati *probabilistica*. 
> Si usa per velocizzare le operazioni di lettura.

Il metodo che opera su questa struttura è una *find* che prende in input una partizione e un tipo di dato, se la risposta della find è no significa che il dato non è nella partizione e quindi non ho bisogno di fare una ricerca nella partizione. L’altra risposta è *maybe* per cui l’informazione si può trovare oppure no.
Non è possibile avere falsi negativi perché se risponde no allora nel 100% dei casi qual dato non è settato, invece se risponde maybe non lo sappiamo. 
Per implementare un bloom filter ipotizziamo di avere un blocco di dati gestito con column database, si associa al blocco un vettore di $N$ valori binari inizializzato a zero inizialmente. Poi seleziono $k \ll N$ funzioni hash. Quando inserisco un dato nel blocco tramite una *insert(x)* se $k = 3$, calcolo:
- $i_{1} = |h_{1}(x)|_N$
- $i_{2} = |h_{2}(x)|_N$
- $i_{3} = |h_{3}(x)|_N$
Poi metterò un 1 nel vettore in corrispondenza dei tre index trovati. 
Quando dovrò fare $find(x)$ ricalcolo le tre hash e controllo il vettore in corrispondenza dell’indice. 
Se trovo *almeno* un bit a 0 allora la risposta sarà *no* perché non ho attivato quella combinazione di hash. Mentre se trovo tutti 1 allora la risposta sarà *maybe* perché non posso essere certa che quelle hash siano state attivate da quel valore. 
$N$ è un numero che rappresenta la lunghezza del vettore, maggiore è $N$ e minore è la probabilità di avere falsi positivi. 

![Bloom Filter|center|600](https://ilyasergey.net/YSC2229/_images/bloom.png)

Ogni tanto verranno effettuate delle operazioni di *flushing* per resettare il vettore perché non si mettono mai a zero i bit con le funzioni che agiscono sulla struttura. 
Si consiglia di usare $N = O(h)$ e $k = log(\frac{n}{h})$ quindi $N$ dello stesso ordine di grandezza della funzione hash.
Quindi il commit log è usato per velocizzare le scritture mentre il bloom filter per quelle di lettura. 
## Hash Tree

# Consistency Level

Si possono considerare diversi livelli di consistenza:
1. **Strict Consistency**: tutte le repliche devono avere gli stessi dati.
2. **Low Consistency**: i dati devono essere scritti in almeno una replica.
3. **Moderate Consistency**: include tutte le soluzioni precedenti, magari alcune porzioni di database hanno una tipologia di consistenza. 
Il tipo di consistenza dipende dalle specifiche di progetto. 

# Replication vs Anti-Entropy

#Attenzione La chiede sempre all’esame
La anti entropy è un processo per fare detection di differenze sulle repliche. 
È implementato nel seguente modo: dato un blocco di dati in una partizione, si può calcolare l’hash di quel blocco di dati.

![Anti Entropy|center|600](https://docs.datastax.com/en/dse/5.1/docs/architecture/_images/graphics/dbMerkleTree.png)

Si calcola l’hash dell’hash dell’hash, …
poi se ci sono differenze tra le root allora si trovano i dati differenti sulla foglia. 
In questo modo possiamo evitare di confrontare tutte le repliche ma ci basta confrontare gli alberi. (da rivedere che non ho capito)
[Sito fatto bene](https://www.designgurus.io/course-play/grokking-the-advanced-system-design-interview/doc/636cedb6d470f8ced9e69121)

# Communication Protocol

- Centralized: un singolo server che comunica con tutti gli altri (single point of failure)
- Fully Connected: tutti connessi 
- **Gossip**: ogni nodo manda le proprie informazioni ad un certo numero di noti sorteggiati randomicamente. Alla fine, in modo molto veloce tutti avranno le stesse informazioni. 

In Cassandra ogni nodo è uguale agli altri, un’operazione di lettura si compone di:
1. Richiesta di lettura fatta ad un certo nodo.
2. Inoltro della richiesta verso il nodo che effettivamente ha le informazioni.
3. Il nodo risponde all’altro che risponde al client. 
Se si vuole fare un’operazione di scrittura tutti i nodo risponderanno (?)
Se si ha un problema sul server che deve rispondere alla richiesta, in caso di operazioni di lettura, il nodo sa chi contiene le repliche quindi ci si sposterà su di lui. Nel caso di operazioni di scrittura è importante evitare di perdere gli update quindi il nodo fa l’**Hinted Handoff** (anche questa domanda la fa spesso).
L’hinted handoff è un *processo* che:
1. crea una struttura dati che memorizza le informazioni delle operazioni di scrittura
2. periodicamente controlla lo status del server target
3. manda le operazioni di scrittura quando il server target è disponibile
