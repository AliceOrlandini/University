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
Per compilare ed eseguire si usano i seguenti comandi:
```
> javac HelloWorld.class
> java HelloWorld
```

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
	public void makeNoise() {
		System.out.println("Some sound");
	}
}

class Dog extends Animal {
	// override method
	public makeNoise() {
		System.out.println("Bark");
	}
}

class Cat extends Animal {
	// override method
	public makeNoise() {
		System.out.println("Meawoo");
	}
}

public class Demo {
	public static void main(String[] args) {
		Animal a1 = new Cat();
		a1.makeNoise(); // prints Meowoo
		Animal a2 = new Dog();
		a2.makeNoise(); // prints Bark
	}
}
```

Un **package** è un insieme di classi relative alla stessa funzione. Per importare una classe si usa la direttiva *import nome_package.*\*.

#Attenzione In Java gli oggetti sono passati tutti per *riferimento*, i tipi primitivi invece sempre per *valore*. 

I tipi primitivi sono:
- byte (8 bit)
- short (16 bit)
- int (32 bit)
- long (64 bit)
- float 
- double
- char (8 bit)
E si riconoscono perché iniziano con la lettera minuscola.

Un metodo è statico se può essere richiamato senza instanziare l'oggetto.
Una variabile è statica se ne esiste una sola copia (è una variabile globale).