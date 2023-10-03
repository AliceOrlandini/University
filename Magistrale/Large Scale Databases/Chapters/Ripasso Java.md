Java è:
- semplice
- sicuro
- portabile
- orientato agli oggetti
- robusto
- multi-thread
- non dipendente dall'architettura
- interpretato
- garantisce grandi prestazioni
- distribuito
- ...
E anche modesto aggiungerei. /s

Il nome del file deve essere uguale al nome della classe contenuta nel file.

Iniziamo con un semplice HelloWorld:
```java
class HelloWorld {
	public static void main(String[] args) {
		System.out.println("Hello World!");
	}
}
```

Ecco un esempio di dichiarazione di una classe:

```java
public class Dog {
	// Instance Variables
	String breed;
	String size;
	int age;
	String color;

	// method 1
	public String getInfo() {
		return ("Bredd is: " + breed + "\nSize is: " + size + "\nAge is:" + age + "\nColor is: " + color);
	}
}

public static void main(String[] args) {
	Dog maltese = new Dog();
	maltese.breed = "Maltese";
	maltese.size = "Small";
	maltese.age = 2;
	maltese.color = "White";
	System.out.println(maltese.getInfo());
}
```
Questo codice è corretto ma non usa i principi di Java che prevedono ad esempio di usare un costruttore per inizializzare una classe, oppure si vanno a modificare direttamente gli attributi mentre sarebbe meglio implementare i **"setters"** e i **"getters"**.

Un'altra caratteristica di Java è la possibilità di usare il **polimorfismo**:
```java
public class Animal{
	public voi
}
```