
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

Generalmente i server dei cluster devono essere indipendenti tra loro, devono avere la possibilità di scambiare informazioni ma solo per una coordinazione minima. Non dobbiamo assolutamente sfruttare questo collegamento per fare catene di query che coinvolgono dati presenti su più server perché ci sarebbe un rallentamento complessivo. 
Ad esempio, se un server cade, i suoi vicini possono prendere il suo posto ed eseguire le sue operazioni (*avaialability*) e quando torna attivo i due server possono comunicargli i risultati. 
L’availability dipende anche dalle repliche dei dati. Lo svantaggio della ridondanza è l’efficienza perché le repliche vanno aggiornate periodicamente. Queste scelte dipendono dai requisiti dell’applicazione. 

## Operazioni di lettura e scrittura con le repliche

Definiamo:
- N: numero di repliche, ad esempio 3
- W: numero di copie che devono essere scritte prima che la scrittura venga completata
- R: numero di copie da leggere per leggere un dato

Prima soluzione: vogliamo mantenere aggiornate sempre tutte le copie: 
- a livello di scrittura sarà oneroso
- a livello di lettura ci basterà leggerne una

Seconda soluzione: manteniamo un buon livello di consistenza andando ad aggiornare almeno una delle due repliche del dato
- a livello di scrittura è un po’ meno oneroso
- a livello di lettura il tempo aumenta ma 2 dei 3 dati sono aggiornati.

Terza soluzione: scriviamo solo una replica, avrò la latenza minore ma anche la consistenza minore:
- a livello di scrittura sarà molto veloce
- a livello di lettura dovrò leggere 3 volte perché devo leggere tutte le copie

La scelta dipende dalla situazione e dai requisiti funzionali e non funzionali. 
## Hash Mapping 

Si va a trasformare una chiave in un hash tramite una funzione hash. 
La prima cosa da tenere a mente è che anche se c’è un piccolo cambiamento nella chiave ci potrebbero essere grandi cambiamenti nell’hash. 
Il problema più importante è quello della collisione, ovvero due o più valori con la medesima hash. Una prima soluzione è usare dei puntatori, nel campo value troverò quindi un puntatore ad una lista. Se voglio scrivere dovrò aggiungere un elemento in coda alla lista. Un’altra soluzione è utilizzare alberi binari.

Un altro problema riguarda la consistenza. 
Ipotizziamo di avere una serie di server, se scambiamo due server o aggiungiamo/rimuoviamo un server dovremmo cambiare tutte le hash e poi fare modulo del numero di server. 
La riallocazione richiede di interrompere il servizio per vario tempo. 
Una soluzione a questo problema si chiama *consistent hashing*: si ha un’organizzazione circolare e partizionata delle chiavi ed ogni server responsabile di una delle sezioni. Se aggiungiamo un nuovo server dovremo ridistribuire solo le chiavi dei suoi vicini, e non dovremo nemmeno cambiare le funzioni hash. 
Questa è una rappresentazione logica.

## Compressione dati

Questo tipo di database sono molto onerosi dal punto di visto della memoria quindi si effettua un’operazione di compressione dati. 
Non è un’operazione banale perché bisogna considerare che più comprimiamo più complicata sarà l’operazione di compressione che quindi richiederà tempo. Anche la decompressione richiede tempo. 

# Linee guide per un key-value db

## Design della chiave

Per fare un buon design della chiae bisogna per prima cosa bisogna mettere il prefisso, il prefisso deve essere uguale per tutte le chiavi appartenenti alla stessa entità.
Quindi tutti gli attributi dell’entità avranno lo stesso prefisso.
Subito dopo il prefisso si mette l’ID e infine il nome dell’attributo. 
La chiave deve avere un nome non ambiguo e con un determinato significato ed è importante tenerle il più corte possibili. 
Come delimiter è consigliato usare “:”. 
## Metodi Get e Set 

I metodi set e get sono metodi per impostare e prelevare i valori da un’entità data una certa chiave. 
Ogni classe deve avere questi metodi. 

```python
define getCustAttr(p_id, p_attrName) 
	v_key = ‘cust’ + ‘:’ + p_id + ‘:’ + p_attrName
	return(AppNameSpace[v_key])

define setCustAttr(p_id, p_attrName, p_value) 
	v_key = ‘cust’ + ‘:’ + p_id +’:’ + p_attrName
	AppNameSpace[v_key] = p_value
```

## Namespace

Considerare la possibilità di avere una convenzione di nomi per il namespace. 

## Controllo degli errori

È importante prevedere le eccezioni e gestirle con blocchi try-catch.

## Lavorare con range di valori

Vogliamo implementare una query che restituisce tutti i clienti che hanno aquistato in una particolare data. 
La chiave è fatta in questo modo: 
cust:061514:1:custId
cust:061514:2:custId
cust:061514:3:custId
cust:061514:4:custId
Questo tipo di chiave è utile per fare query sui i range di chiavi perché si può facilmente scrivere una funzione che fa questa cosa. 

```python
define getCustPurchByDate(p_date) 
	v_custList = makeEmptyList()
	v_rangeCnt = 1
	v_key = ‘cust‘ + ‘:’ + p_date + ‘:’ + v_rangeCnt + ‘:’ + ‘custId’
	while exists(v_key)
		v_custList.append(myAppNS[v_key])
		v_rangeCnt = v_rangeCnt + 1
		v_key = ‘cust‘ + ‘:’ + p_date + ‘:’ + v_rangeCnt + ‘:’ + ‘custId’
	return v_custList
```

Considerare il seguente codice:
```python
define getCustNameAddr(p_id)
	v_fname = getCustAttr(p_id, ‘fname’)
	v_lname = getCustAttr(p_id, ‘lname’)
	…
	v_fullName = v_fname + ‘ ‘ + v_lname
	v_fullAddr = v_city + ‘ ’ + v_state
	return makeList(v_fullName, vfullAddr)
```

Il problema in questo codice è che stiamo facendo 6 query per avere tutte le informazioni che ci servono. Se noto che tutte le volte che uso queste informazioni sempre insieme devo considerare di unirle.
Se invece alcune volte le uso insieme e altre separate allora uso chiavi separati, la ridondanza non è proibita ma bisogna fare attenzione all’aggiornamento.
Questi codici li chiede all’esame.

Esempio: se si fa un’applicazione sul fantacalcio, il set di dati che si trova online non va importato tutto nel database ma bisogna importare solo le parti che ci servono e con aggregazioni che scegliamo noi a seconda di come si andranno ad utilizzare i dati nell’applicazione. 

# Limitazioni del database key-value

Le limitazioni principali sono 3:
1. L’unico modo per accedere ai dati è usando la chiave
2. Alcuni database key-value non supportano le query con i range
3. Non c’è un linguaggio standard compatibile con l’SQL per i database relazionali.

La parte teorica dei key-value database è finita. 

# Caso studio

Prendiamo un’applicazione di tracciamento delle spedizioni. 
Le informazioni memorizzate sono:
- nome e numero di account del cliente
- prezzo della spedizione
- delle informazioni da mostrare sulla dashboard
- preferenze riguardo alert e notifiche 
- opzioni sull’interfaccia utente come ad esempio la light/dark mode o il fort

La maggior parte delle operazioni saranno di lettura. 

Per la chiave, le entità saranno:
- *cust*: per il cliente
- *dshb*: per la dashboard
- *alrt*: per gli alert e le notifiche
- *ui*: per le opzioni
Nota: non usare questi acronimi che fanno schifo, il libro da cui il prof ha preso l’esercizio li metteva così e li ha lasciati così.

TrakerNS[‘cust:473819’] = {‘name’: ‘Prime Machine, Inc.’, ‘currency’: ‘USD’}

TrakerNS[‘dshb:381909’] = {‘shpComp’, ‘shpState’, ‘shpDate’, ‘shpDelivDate’}

TrakerNS[alrt:349220] = {altList:{ ‘jane.washington@gmail.com’, ‘pickup’, ‘3810410385’, ‘delay’}}

TrakerNS[ui:4812984] = {‘fontName’: ‘Cambria’, ‘FontSize’: 9, ‘colorScheme’: ‘light’}

Non ho capito perché non mette le virgolette ad alrt e ui.