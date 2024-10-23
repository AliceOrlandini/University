### Generics

Gli algoritmi possono essere scritti utilizzando **tipi di dato parametrizzati** (parametric data types), che vengono definiti in modo generico e specificati solo a tempo di esecuzione. Questo approccio ha lo scopo di evitare la duplicazione del codice, consentendo di scrivere funzioni o strutture dati che funzionano con diversi tipi di dato, migliorando così il controllo e la manutenibilità del codice.

In **Java**, i tipi di dato parametrizzati sono chiamati **Generics** e permettono di definire classi, metodi o interfacce utilizzando placeholder per i tipi di dato, che verranno assegnati al momento dell'uso.

Un esempio è: 

```java
public class Box<T> {
    private T value;

    public void set(T value) {
        this.value = value;
    }

    public T get() {
        return value;
    }
}
```

In questo esempio, la classe `Box` utilizza un tipo generico `T`, che può essere specificato quando si crea un'istanza della classe. Ciò consente di utilizzare la stessa classe `Box` per contenere diversi tipi di oggetti, senza riscrivere il codice.

Il meccanismo che consente ai **Generics** di funzionare in Java è chiamato **Type Erasure** (cancellazione dei tipi). Durante la compilazione, il tipo generico viene mantenuto solo fino a quando il compilatore ha eseguito tutti i controlli di tipo necessari. Una volta terminati questi controlli, il tipo specifico viene "cancellato" o sostituito con il tipo più generico (come `Object` o un tipo specifico per i limiti), e quindi la **JVM** non ha più informazioni sui tipi generici a tempo di esecuzione.

Una limitazione è che poiché i tipi generici vengono cancellati dopo la compilazione, non è possibile determinare il tipo effettivo di un oggetto generico a tempo di esecuzione. Questo significa che non è possibile, ad esempio, interrogare il tipo di un oggetto generico usando il reflection per scoprire il tipo effettivo che è stato utilizzato (ad esempio usando `lista.getClass()`.

### Collections Frameworks

Lo scopo dei **collections frameworks** è quello di fornire supporto per le strutture dati più comuni e utilizzate. Questi framework sfruttano largamente i **Generics** per rendere le API, definite principalmente sotto forma di interfacce, il più riutilizzabili e flessibili possibile. Questo consente agli sviluppatori di utilizzare e manipolare vari tipi di dati senza duplicare il codice per ogni tipo.

Le API delle collections, che fanno parte di pacchetti come `java.util`, offrono una serie di strumenti per la gestione efficiente di raccolte di oggetti, come liste, insiemi e mappe.

Le **interfacce** nel framework possono essere implementate o ereditate dalle classi, consentendo alle classi di estendere le loro funzionalità in modo flessibile e personalizzato, mantenendo al contempo la compatibilità con il framework.

La maggior parte delle collezioni nel framework delle **collections** in Java sono **iterabili**, il che significa che possono essere percorse utilizzando gli **Iterators**. Un **Iterator** fornisce un metodo standard per scorrere gli elementi di una collezione uno alla volta, consentendo di accedere agli elementi senza esporre i dettagli interni della struttura dati.

```java
import java.util.ArrayList;
import java.util.Iterator;

public class Main {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        list.add("Erlang");
        list.add("Java");
        list.add("Python");

        // creazione di un Iterator per scorrere la lista
        Iterator<String> iterator = list.iterator();

        // utilizzo di Iterator per iterare 
        // sugli elementi della lista
        while (iterator.hasNext()) {
            String element = iterator.next();
            System.out.println(element);
        }
    }
}
```

### Funzioni (Lambda)

Se vogliamo passare una funzione come parametro a un'altra, possiamo evitare di creare una classe esplicita con un solo metodo `myFunction`, utilizzando le **functional interfaces** di Java. 
Le functional interfaces sono interfacce che hanno un solo metodo astratto, rendendo il codice più conciso e leggibile. Java fornisce interfacce funzionali predefinite come `Function<T, R>`, `Predicate<T>`, `Consumer<T>`, che possono essere utilizzate per eseguire operazioni con i **lambda expressions**.

Un esempio di utilizzo senza functional interfaces è:

```java
class MyFunction {
    public String apply(String s) {
        return s + "!";
    }
}

public class Main {
    public static void main(String[] args) {
        MyFunction func = new MyFunction();
        System.out.println(func.apply("Hello"));
    }
}
```

In questo esempio, dobbiamo creare la classe `MyFunction` e poi istanziarla ogni volta che vogliamo applicare la funzione `apply`.

Per evitare di creare una classe dedicata, possiamo usare una **functional interface** e un'espressione lambda:

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        // usando una lambda expression per definire la funzione
        Function<String, String> addExclamation = s -> s + "!";
        
        // chiamata della funzione
        System.out.println(addExclamation.apply("Hello"));
    }
}
```

Invece di creare una classe `MyFunction`, usiamo l'interfaccia funzionale `Function<String, String>`, che rappresenta una funzione che accetta un argomento di tipo `String` e restituisce un valore di tipo `String`. 
La **lambda** `s -> s + "!"` invece definisce il comportamento della funzione direttamente, evitando la creazione di una classe separata.

Le **lambda expressions** possono essere utilizzate nelle funzioni di *sorting* per specificare un criterio di ordinamento. Invece di creare una classe separata che implementa un comparatore, possiamo passare direttamente una lambda che definisce la logica di confronto.
Un esempio è l'ordinamento in ordine alfabetico: 
`Collections.sort(list, (s1, s2) -> s1.compareTo(s2));`
Oppure l'ordinamento in base alla lunghezza delle stringhe:
`Collections.sort(list, (s1, s2) -> Integer.compare(s1.length(), s2.length()));`
Da notare che `s1` ed `s2` non hanno un tipo, quindi sono totalmente generiche.

### Streams

Gli **streams** sono stati introdotti in Java per lavorare in modo più semplice e fluido con le **collections**, al fine di eseguire operazioni come filtraggio, trasformazione e riduzione dei dati, in modo dichiarativo e più funzionale. 
Gli streams forniscono un livello di astrazione che permette di applicare operazioni come `map`, `filter`, `reduce`, e molto altro, utilizzando uno stile di programmazione funzionale simile a quello utilizzato nei modelli **Map-Reduce**.

L'interfaccia chiave `Stream<T>` rappresenta una sequenza di elementi su cui è possibile eseguire una catena di operazioni, senza modificare la struttura sottostante della collection. 

Un esempio è:

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "cherry", "date", "elderberry");

        // utilizzo di uno stream per filtrare e 
        // trasformare i dati
        List<String> result = words.stream()
            .filter(word -> word.length() > 5)
            .map(String::toUpperCase)     
	        .collect(Collectors.toList());

        System.out.println(result);  
    }
}
```


I vantaggi sono:
- **Stile dichiarativo**: Gli streams permettono di scrivere il codice in modo più leggibile e dichiarativo, concentrandosi sul *cosa* fare piuttosto che sul *come* farlo.
- **Operazioni intermedie**: Gli streams supportano operazioni come `filter`, `map`, `sorted`, che vengono eseguite in modo *lazy* (solo quando necessario).
- **Operazioni terminali**: Le operazioni come `collect`, `reduce`, `forEach` terminano lo stream e forniscono il risultato.

Gli **streams** in Java sono immutabili, il che significa che ogni operazione applicata su di essi restituisce un *nuovo stream* senza modificare quello originale. Gli streams non possiedono direttamente gli elementi su cui operano; gli elementi appartengono alle collezioni (o altre fonti) da cui gli streams sono generati. 

Per unire due streams, è necessario utilizzare specifiche **funzioni di aggregazione**, poiché gli streams non supportano direttamente l'aggregazione degli elementi. Queste funzioni permettono di combinare o ridurre gli elementi per produrre un unico risultato. Inoltre, gli streams permettono di creare delle **pipeline**, dove più operazioni intermedie possono essere concatenate e applicate in sequenza, con un'operazione terminale che conclude la pipeline e restituisce il risultato finale.

### Java Annotations

Le **annotazioni** in Java sono utilizzate per aggiungere *metadati* a un pezzo di codice. Anche se non influenzano direttamente il comportamento del codice, sono preziose per strumenti esterni, come il compilatore o altre applicazioni di analisi. Ad esempio, il compilatore può modificare il suo comportamento in base alle annotazioni presenti, oppure esse possono essere esaminate a tempo di compilazione.

Le annotazioni possono essere applicate a: **Classi**, **Interfacce**, **Metodi**, **Parametri dei metodi**, **Campi**, **Variabili locali**.

Esistono diverse **retention policies**, che indicano fino a quale momento un'annotazione è conservata:
- **Source**: l'annotazione è presente solo nel codice sorgente e viene scartata dopo la compilazione.
- **Class**: l'annotazione è conservata nel file `.class` ma non è disponibile a runtime.
- **Runtime**: l'annotazione è disponibile anche a runtime e può essere recuperata tramite la **reflection**.  La reflection è un meccanismo che consente di esaminare o modificare il comportamento di classi, metodi e campi a runtime. Questo permette di leggere le annotazioni presenti in una classe o metodo e, in base ad esse, eseguire azioni specifiche durante l'esecuzione del programma.

Un esempio di annotazione è: 

```java
@Author (
	name = "Alice Orlandini",
	date = "29-01-2000"
)
class MyClass () {}
```

Alcune delle **built-in annotations** in Java sono:
- **@Deprecated**: Segnala che l'elemento (metodo, classe o campo) non è più consigliato per l'uso e potrebbe essere rimosso nelle versioni future.
- **@Override**: Indica che un metodo sta sovrascrivendo un metodo definito in una superclasse o interfaccia. Questa annotazione aiuta il compilatore a garantire che la firma del metodo sia corretta.
- **@SuppressWarnings**: Utilizzata per sopprimere gli avvisi generati dal compilatore per un determinato pezzo di codice. È spesso utilizzata per disabilitare avvisi specifici (ad esempio, avvisi su tipi generici non sicuri o deprecated).
- **@Contended**: È un'annotazione meno comune utilizzata per ridurre il contenzioso tra thread. È parte del pacchetto `sun.misc` e viene utilizzata in scenari di concorrenza per ridurre il problema della *falsified contention*, separando i dati nelle cache di CPU.

### Threads

Definire un **thread** in Java significa:
1. **Costruire le strutture dati necessarie**: Queste strutture rappresentano lo stato del thread e possono includere parametri come priorità, nome, e altre informazioni utili per gestire le caratteristiche del thread.
2. **Specificare le azioni per la gestione del thread**: Queste azioni includono la creazione, l'esecuzione, la sospensione, la terminazione, e altre operazioni che regolano il ciclo di vita del thread.

La classe che rappresenta un thread è la classe **`Thread`**. Per definire il comportamento del thread, bisogna specificare il **body**, ovvero il blocco di codice che il thread eseguirà.

#### JVM Runtime Data Area

La memoria in Java è organizzata in tre parti principali:
1. **Method Area (Bytecode)**: Quest'area è condivisa tra tutti i thread e contiene informazioni essenziali per l'esecuzione del programma, tra cui:
   - **Runtime Constant Pool**: Un insieme di costanti, utilizzato come una sorta di tabella dei simboli, che memorizza valori costanti e riferimenti.
   - **Method Code**: Il bytecode delle funzioni, condiviso tra i thread. Il codice non viene replicato, ma usato in modo comune da tutti i thread.
   - **Field e attributi (reference variables)**: Questi contengono solo riferimenti agli oggetti, ma non gli oggetti stessi.
2. **Heap (Objects)**: È la parte della memoria dove vengono allocati gli oggetti e gli array. Anche l'heap è condiviso tra tutti i thread e soggetto al **Garbage Collector**, che libera lo spazio occupato da oggetti non più utilizzati. Gli oggetti con lo stesso valore (come alcune stringhe immutabili) possono essere condivisi in memoria per ottimizzare lo spazio.
3. **Thread-specific Memory (Stack)**: Ogni thread ha il proprio **stack**, che contiene i frame relativi alle chiamate di funzione. Ogni chiamata a una funzione genera un frame nello stack del thread che contiene:
   - **Variabili locali**: Le variabili create all'interno della funzione.
   - **Operand Stack**: I dati temporanei utilizzati durante l'esecuzione delle operazioni della funzione.
   - **Riferimenti al Constant Pool**: Collegamenti al pool costante per accedere a valori e riferimenti utili durante l'esecuzione.

#### Basic Thread Handling in Java

Per creare un thread in Java abbiamo due opzioni principali:
1. **Estendere la classe `Thread`**: questo approccio ha lo svantaggio di limitare la possibilità di estendere altre classi, poiché in Java si può estendere una sola classe alla volta.
2. **Usare un'istanza di `Runnable`**: si tratta di un meccanismo più flessibile, in cui si implementa l'interfaccia `Runnable`, e poi si passa l'istanza al costruttore di `Thread`. Questo approccio permette di separare il comportamento del thread dalla classe `Thread` stessa.

Dopo aver creato il thread con uno dei due metodi, è possibile avviarlo utilizzando il metodo `start()` per eseguire il codice all'interno del metodo `run()`.

Per creare un thread in Java utilizzando la seconda opzione, ovvero l'implementazione dell'interfaccia `Runnable`, seguiamo questi passaggi:
1. Definiamo una classe che implementa `Runnable`.
2. Creiamo un'istanza di questa classe e la passiamo al costruttore della classe `Thread`.
3. Utilizziamo il metodo `start()` per avviare l'esecuzione del thread.

Ecco un esempio:

```java
// classe che implementa l'interfaccia Runnable
class MyRunnable implements Runnable {
    @Override
    public void run() {
        // codice eseguito dal thread
        for (int i = 0; i < 5; i++) {
            System.out.println("Thread in esecuzione: " + i);
            // simula un'attività con un intervallo di 1 secondo
            Thread.sleep(1000);
        }
    }
}

public class ThreadExample {
    public static void main(String[] args) {
        // creazione di un'istanza di MyRunnable
        Runnable runnable = new MyRunnable();
        
        // creazione di un'istanza di Thread 
        // passando l'istanza di MyRunnable
        Thread thread = new Thread(runnable);
        
        // avvio del thread
        thread.start();
        
        // codice eseguito dal thread principale
        System.out.println("Thread principale terminato.");
    }
}
```

Per fermare un thread in Java, possiamo inviargli un'interruzione tramite un altro thread utilizzando il metodo `interrupt()`. Questo setterà un flag di interruzione nel thread target, segnalando che dovrebbe terminare. Il thread controlla periodicamente questo flag e può gestire l'interruzione con una corretta logica di terminazione.

Alcune linee guida generali per la gestione delle interruzioni sono:
- Rilanciare l'eccezione (`InterruptedException`) se il thread è interrotto, in modo che il chiamante possa gestirla.
- Implementare una pulizia adeguata e un'uscita controllata del thread in risposta all'interruzione.
- Evitare l'uso del metodo `stop()` deprecato, che può causare incoerenze di stato e altri problemi legati alla concorrenza.

#### Thread Pooling e Executor Framework 

Un task in Java può essere definito utilizzando l'interfaccia `Runnable`, che permette di specificare il codice da eseguire. Per eseguire questi task in modo flessibile, esiste l'interfaccia `Executor`, che fornisce un framework per gestire l'esecuzione asincrona e multithread.

Tuttavia, ci sono delle penalità sulle performance che possono verificarsi quando si eseguono task in parallelo:
- **Context switches**: il passaggio tra thread può causare un sovraccarico, riducendo l'efficienza.
- **Lock contention**: quando più thread competono per lo stesso lock, possono verificarsi ritardi dovuti all'attesa per l'accesso a risorse condivise.
- **Memory synchronization**: la sincronizzazione tra thread può richiedere un uso intensivo della memoria, rallentando il sistema.

I thread pool vengono utilizzati per creare un insieme di thread all'inizio del programma, evitando il costo ripetuto della creazione di thread durante l'esecuzione. Questo approccio riduce l'overhead, dato che la creazione dei thread avviene una sola volta. Il numero di thread nel pool è solitamente proporzionato alle risorse hardware disponibili, come CPU e memoria, poiché queste risorse sono limitate.

Ogni thread del pool viene associato a un task e, una volta completato il task, il thread ritorna disponibile per eseguire altri compiti, ottimizzando così l'uso delle risorse.

Un design pattern è il seguente: 

![[Thread Pool.png|center|500]]

I Futures vengono utilizzati per raccogliere risultati da calcoli che verranno eseguiti in futuro e sono utili per gestire operazioni asincrone. Le Promise, invece, sono funzioni che permettono di impostare un valore che sarà disponibile in un momento successivo.
#### Java Synchronizers

A volte, i thread devono essere sincronizzati tra loro per coordinare l'esecuzione e garantire che procedano solo quando determinate condizioni sono soddisfatte. Esistono diverse classi in Java per gestire la sincronizzazione:
- **Latches**: Un latch permette a un gruppo di thread di attendere fino a quando tutti non hanno raggiunto uno stato terminale. Un esempio è il `CountDownLatch`, che ha un contatore interno che viene decrementato ogni volta che un thread raggiunge il traguardo. Quando il contatore arriva a zero, tutti i thread vengono rilasciati. È anche possibile specificare un *timeout*, dopo il quale i thread vengono sbloccati indipendentemente dal raggiungimento della condizione.
- **Barriers**: Una barriera (`Barrier`) fa sì che un gruppo di thread si blocchi finché tutti i thread non hanno raggiunto la barriera. Un esempio comune è la `CyclicBarrier`, che può essere resettata e riutilizzata più volte. Il suo stato interno tiene traccia di quanti thread devono raggiungere la barriera prima che tutti possano procedere.
- **Phasers**: Un phaser è una versione avanzata delle `Barriers` che offre maggiore flessibilità e controllo su più fasi di sincronizzazione. I thread possono registrarsi per partecipare e procedere insieme attraverso più fasi di esecuzione.
- **Semaphores**: I semafori sono simili a quelli visti nei sistemi operativi, ma con un livello di astrazione superiore in Java. Possono essere utilizzati per implementare la mutua esclusione o per coordinare la sincronizzazione tra produttore e consumatore. In Java, il metodo `wait` diventa `acquire`, e `signal` diventa `release`, fornendo un meccanismo per limitare l'accesso a risorse condivise.
- **Exchangers**: Un exchanger permette a due thread di sincronizzarsi e scambiarsi dati. Quando entrambi i thread raggiungono il punto di sincronizzazione, si scambiano gli oggetti e possono procedere con la loro esecuzione.

#### Thread-Safe Data Structures

Le **thread-safe data structures** sono strutture dati progettate per essere sicure da utilizzare in ambienti *multithread*. "Safe" significa che l'accesso concorrente da parte di più thread non causa corruzione dei dati o problemi di sincronizzazione. L'uso di queste strutture evita la necessità di gestire manualmente la sincronizzazione, riducendo il rischio di bug legati alla concorrenza.

Per rendere sicure le strutture dati in un sistema multithread, spesso si utilizza il **Decorator Pattern**. Questo pattern permette di aggiungere dinamicamente comportamenti (come la sincronizzazione) a oggetti esistenti senza modificare il loro codice originale. 

Una delle classi più comuni che utilizza il **Decorator Pattern** per creare versioni thread-safe di strutture dati standard è `Collections.synchronizedList()`. Questa funzione restituisce una lista decorata con meccanismi di sincronizzazione:

```java
List<String> threadSafeList = 
	Collections.synchronizedList(new ArrayList<>());
```

In questo caso, la lista originale viene decorata con sincronizzazione automatica, rendendola sicura per l'uso in un ambiente multithread.

Le collezioni di tipo **synchronized** possono avere prestazioni scarse in condizioni di **high contention** (alta contesa) a causa del livello di **coarse-grained synchronization**. Questo significa che la sincronizzazione avviene su un livello "granuloso", cioè bloccando l'intera collezione o grandi porzioni di essa ogni volta che viene eseguita un'operazione. Questo approccio porta a un significativo **overhead**, poiché solo un thread alla volta può accedere alla struttura dati, e quindi altri thread devono aspettare, riducendo l'efficienza in scenari di alto accesso concorrente.

Per ridurre l'**overhead di sincronizzazione**, si utilizza un approccio **concurrent** invece di **synchronized**. Questo permette a più thread di accedere alla struttura dati contemporaneamente senza compromettere l'integrità dei dati. Un esempio è l'interfaccia `ConcurrentMap<K, V>`, che fornisce *operazioni atomiche* di lettura e scrittura. Tuttavia, iterare su collezioni concorrenti può essere rischioso, poiché più thread possono accedere e modificare la struttura dati contemporaneamente.

Per risolvere il problema dell'iterazione sicura su collezioni concorrenti, sono stati introdotti diversi tipi di iteratori con semantiche specifiche:
1. **Fail-Fast**:
   - Utilizzato principalmente nelle collezioni non concorrenti.
   - Permette di attraversare una collezione, ma se un altro thread tenta di modificarla durante l'iterazione, viene sollevata una **`ConcurrentModificationException`**.
   - Per rimuovere un elemento durante l'iterazione, è necessario utilizzare il metodo `remove()` dell'**iteratore** anziché quello della collezione, per evitare l'eccezione.
   - È un meccanismo che garantisce che le modifiche concorrenti vengano rilevate, ma non sono consentite.

2. **Weakly-consistent**:
   - Utilizzato nella maggior parte delle collezioni concorrenti.
   - Non solleva eccezioni quando la collezione viene modificata durante l'iterazione.
   - L'iteratore garantisce di attraversare tutti gli elementi presenti nel momento della sua creazione, esattamente una volta. Tuttavia, potrebbe o non potrebbe riflettere le modifiche apportate alla collezione dopo la sua creazione.
   - È più flessibile per ambienti concorrenti, ma non garantisce un'istantanea coerente della collezione.

3. **Snapshot**:
   - Utilizzato in collezioni **copy-on-write**, dove viene effettuata una copia degli elementi ogni volta che la collezione viene modificata.
   - Quando si itera su una collezione con uno snapshot iterator, è come se si scattasse una "foto" della collezione al momento dell'iterazione, senza riflettere modifiche successive.
   - Non è necessaria la sincronizzazione durante l'iterazione, ma non supporta operazioni mutative come `remove()` sull'iteratore.
   - Adatto a collezioni in cui si privilegiano letture rapide e consistenti, come `CopyOnWriteArrayList`.

4. **Blocking queues**:
   - Questi iteratori sono utilizzati nelle **code bloccanti**, che sono strutture dati thread-safe molto performanti, come i **bounded buffer**.
   - Supportano operazioni di inserimento, estrazione e ispezione in vari modi.
   - Sono ideali per implementare il paradigma **produttore-consumatore**, dove i thread produttori inseriscono elementi nella coda e i thread consumatori li rimuovono, con meccanismi di attesa e notifica.
   - Le **blocking queues** gestiscono in modo efficiente la sincronizzazione e il passaggio di dati tra thread.

#### Memory Consistency Rules

