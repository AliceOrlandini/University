Nelle prime righe della documentazione di OMNeT++ possiamo leggere: 

> [!note] OMNeT++
> **OMNeT++** itself is not a simulator of anything concrete, but rather *provides infrastructure and tools for writing simulations*.
> One of the fundamental ingredients of this infrastructure is a component architecture for simulation models. Models are assembled from reusable components termed modules.
> Well-written modules are truly reusable, and can be combined in various ways *like LEGO blocks*.

In OMNeT++ ciò che si vuole valutare si chiama **network**. Un network è un insieme di moduli composti da entità collegate tra loto tramite connessioni.

Divideremo in:
- NED: struttura
- C++: comportamento
- INI: parametri
Ognuno di questi file è indipendente, in questo modo, posso ad esempio cambiare i parametri lasciando invariata la struttura e il comportamento.

La definizione del network viene fatta nel NED, ogni modulo avrà interfacce (input e output *gate*) con cui collegarlo con gli altri moduli. 

Definiamo un semplice modulo nel .net:
```c++
simple Txc {
	gates:
		input in;
		output out;
}
```

Definiamo ora un network che usa il modulo definito prima:
```c++
network Tictoc {
	submodules:
		tic: Txc;
		toc: Txc;
	connections:
		tic.out --> { delay = 100ms; } --> toc.in;
		tic.in <-- { delay = 100ms; } <-- toc.out;
}
```
Le frecce indicano il verso della connessione mentre nelle parentesi graffe si specificano alcune caratteristiche come in questo esempio il delay del canale. 

Questi elementi sono stati definiti ma non fanno nulla perché non ho ancora specificato il comportamento.