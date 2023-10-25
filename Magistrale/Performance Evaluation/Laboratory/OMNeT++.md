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

Questi elementi sono stati definiti ma non fanno nulla perché non ho ancora specificato il comportamento. In una simulazione *discrete-event* il sistema si evolve secondo il paradigma **causa-effetto** quindi definire il comportamento del sistema significa specificare l'effetto in codice c++.

| Evento                                          | Comportamento                    |
| ----------------------------------------------- | -------------------------------- |
| Inizio simulazione                              | *tic* manda un messaggio a *toc* |
| *Txc* riceve un messaggio                       | *Txc* invia una risposta         |
| Il tempo massimo di simulazione viene raggiunto | Si termina la simulazione        |

Gli handler si definiscono tramite 3 funzioni:
1. *initailize()*: eseguito quando il modulo viene creato, di solito tutti i moduli vengono creati all'inizio della simulazione (una sorta di costruttore).
2. *handleMessage()*: eseguito quando arriva un messaggio.
3. *finish()*: eseguita alla fine della simulazione. 
Ma come si crea un evento? Tramite le funzioni:
1. *send()*: invia un messaggio ad un altro modulo, il momento di generazione di questo evento dipende dai parametri specificati (delay = 100ms).
2. *scheduleAt()*: utile per i timer, è una send che invia a me stesso tra tot secondi.
3. *cancelEvent()*: usata per rimuovere dalla event queue un evento. 