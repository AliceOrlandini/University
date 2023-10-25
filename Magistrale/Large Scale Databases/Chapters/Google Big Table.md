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

Il metodo che opera su questa struttura è una *find* che prende in input una partizione e un tipo di dato, se la risposta della find è no significa che il dato non è nella partizione e quindi non ho bisogno di fare una ricerca nella partizione. L’altra risposta è *maybe* l’informazione si può trovare oppure no.
Quindi il commit log è usato per velocizzare le scritture mentre il bloom filter per quelle di lettura. 

## Hash Tree