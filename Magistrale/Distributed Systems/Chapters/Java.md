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

