
Questa è una prima struttura di NoSQL database. 
Iniziamo da un array: una lista ordinata di valori tutti dello stesso tipo indicizzati da un indice. I limiti sono che l’indice può essere solo un intero e che i valori devono avere tutti lo stesso tipo. 
Gli array associativi (dizionari in python) superano questi limiti perché il contenuto dell’array può variare di tipo e l’index può essere qualsiasi cosa come una stringa ad esempio. 

I dati devono essere salvati permanentemente nel database, in questo caso non c’è bisogno di tabelle ma ci basta salvare ogni valore con un identificatore unico. Possiamo usare questo tipo di database quando non dobbiamo mantenere relazioni specifiche tra entità. 
Esempio: e-commerce Prodotti, Ordini, Utenti ha senso usare i relazionali.

namespace: collezione di nomi usata nel nostro database. Il namespace è specificato nei bucket. Ad esempio se uso il bucket1 allora avrò un set di chiavi e il corrispettivo set di valori. Una chiave deve essere unica in bucket. 
Una entry in un bucket rappresenta una singola informazione. 

Motivi per usare questi db:
- semplicità
- velocità
- scalabilità

## Semplicità

Perché non dobbiamo definire lo schema del database né il tipo di dato per ogni attributo. Dobbiamo solo definire il bucket e le sue chiavi. 
È molto facile modificare attributi senza cambiare la struttura del db. 
È flessibile e forgiving: nessuno controllerà che venga inserito il valore corretto, ad esempio se un campo rappresenta un numero di telefono il DBMS non ci avvertirà se viene inserito un indirizzo di casa. 
Se il data model è semplice non ci dovrebbero essere problemi.

Dipende tutto da ciò che si deve fare: se devo scrivere la lista della spesa uso le note del telefono, se devo scrivere la tesi uso Latex. 

## Velocità

La maggior parte di questi database vengono copiati interamente in cache quindi le operazioni di lettura e scrittura sono molto veloci. Poi a un certo punto verranno effettuate le operazioni sul DB allocato sul disco. 

Anche nei DB relazionali c’è un sistema di caching ma qui i database sono stati ottimizzati per la cache.

Dobbiamo però prestare attenzione al fatto che la memoria non è infinita, il DBMS dovrà occuparsi della gestione della memoria
in modo da eliminare dati vecchi e sostituirli con quelli più usati (algoritmo LRU last recently used).
Ad esempio: il carrello di un e-commerce sarà salvato in cache poi al checkout si può fare il flush. 

## Scalabilità 

La scalabilità è la capacità di un database di aggiungere o rimuovere server da un insieme di server se necessario per accogliere il carico del sistema. 

Qui non abbiamo operazioni di join quindi possiamo tranquillamente dividere il database. In questo modo se serve un nuovo server lo si aggiunge.

Ci sono due possibilità:
1. Master slave replication: 
	Il master è un server che accetta richieste di lettura e scrittura. Le operazioni di scrittura verranno effettuate su tutti gli slave dal master.
	Gli slave può rispondere solo a operazioni di lettura. 
	Questa architettura si utilizza con servizi che hanno operazioni di lettura crescenti. 
	Pro:
	- l’architettura è molto semplice perché solo il master comunica con gli slaves
	- non c’è bisogno di coordinare le operazioni di lettura (conflitti)
	Contro:
	- se il master diventa fuori uso allora il nodo non può accettare nuove scritture. In questo caso, il sistema collassa o comunque perde la capacità di scrittura. 
2. Masterless replication:
	Si ha una struttura ad anello tra i server, tutti i server possono rispondere alle richieste e i server aiutano i propri vicini per propagare le informazioni. 
	Si può anche fare una struttura semi-stella. 
	Pro:
	- non c’è un unico point of failure.

## Come fare il design di un key-value database

Il problema principale è quello di trovare un set di chiavi uniche.
È bene che il nome delle chiave (la maggior parte delle volte è una stringa) non cambi nel tempo, deve essere immutabile nel tempo.
Inoltre, è bene che abbiano un significato e non siano semplici ID.
Non fare l’errore di usare relazioni con questi tipi di database, se si ha la sensazione di usare database allora usare database relazionali. 
Non dobbiamo usare due bucket ma nello stesso possiamo mettere tutte le informazioni. 
Esempio:
Chiave formata da: 
customer: nome dell’entity
1838010: entity id
firstName: nome dell’attributo

Così abbiamo una sola entità senza usare relazioni.
Poi vedremo che con l’entity id si possono fare delle concatenazioni. 

Se le analisi sono molto complicati e le query vengono complicate allora molto probabilmente abbiamo scelto il database sbagliato.

A basso livello la stringa che rappresenta la chiave viene convertita in una tramite una funzione hash: è una funzione che prende in input una stringa e restituiscce un valore intero in formato esadecimale.

Distribuzione delle richieste:
le hash possono essere anche utilizzate per capire quale server deve rispondere alla richiesta. Ad esempio assegno le hash in modo che facendone il modulo 8 ottengo un numero da 0 a 7 che identifica il server che deve rispondere alla richiesta. 
#Attenzione questo non significa che si ha una sola copia, ci possono essere le repliche ma il server che risponderà alla richiesta sarà sempre lo stesso. 

Per quanto riguarda i value la maggior parte dei DB ammettono tutti i tipi di dati. 

Quali operazioni si possono fare?
- Prelevare dati per chiave
- Impostare il value relativo ad una chiave
- Eliminare valori per chiave
Per fare operazioni più complesse bisogna lavorare col codice.


Un index è una struttura in cui la key è una parola specifica e il value è la lista di chiavi il cui relativo value contiene quella parola.

Continuiamo la trattazione dei database key-value. 
Oggi studieremo ulteriori dettagli riguardanti il mantenimento di server distribuiti quindi lavorando con partizioni e repliche utilizzando strutture hash.

Un database key value “its very speed” cit.Ducange. 
La funzione hash possono essere usate per distribuire il carico sui server. 

Se scegliamo chiavi lunghe allora ci sarà più occupazione di memoria.
Se invece scegliamo chiavi corte ci sarà più probabilità di collisione usando le funzioni hash.
Per decidere bisogna considerare i requisiti e la documentazione ufficiale del db che stiamo considerando.

Per i valori invece abbiamo oggetti quindi un insieme di bytes associati ad una singola chiave.

Il namespace è una collezione di key-value che non hanno key duplicate. In un database si possono usare più namespace quindi avere più attributi ma con significato differente perché appartenenti a due namespace diversi. (bukets)
Questi sono molto utili quando si ha un team diviso in gruppi che lavorano in diverse parti del database, ogni gruppo avrà il proprio namespace in modo che non ci siano contrasti sulle chiavi. 

# Data Partitioning

Dato un dataset, lo dividiamo in parti (chunks) e memorizziamo questi dati in vari server. 
Se ad esempio abbiamo 2 server dividiamo i dati la cui chiave è compresa tra le lettere A-L nel primo server e M-Z nel secondo server. Il problema di questa divisione è che non sappiamo se sarà equilibrata, non vogliamo che un server sia sovraccarico mentre l’altro non abbia molto da fare. 
Quando usiamo invece le funzioni hash la distribuzione è randomica perché non sappiamo quale funzione avremo in output. 
Ogni volta che aggiungiamo o rimuoviamo un server la riallocazione dei dati deve essere *facile*.

> La **partition key** è una chiave che identifica una specifica partizione nel quale troveremo il valore salvato. 

Un alto tema importante è la flessibilità dei database che sono schema-less. 

Generalmente i server dei cluster devono essere indipendenti tra loro, devono avere la possibilità di scambiare informazioni ma solo per una coordinazione minima. Non dobbiamo assolutamente sfruttare questo collegamento per fare query che coinvolgono dati 