# OMNet++ basics

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

Nel file C++ che racchiude il comportamento del sistema la prima cosa da fare è importare la libreria ed estendendo la classe del simple module e ridefinendo le funzioni:
```c++
#include <omnetpp.h>
class Txc: public cSimpleModule {
protected:
	virtual void initialize();
	virtual void handleMessage(cMessage* msg);
	virtual void finish();
public:
	Txc(); // costruttore
}

Define_Module(Txc); // il modulo deve essere registrato, lo si fa con una macro
```
in questo modo sarà la libreria che si occuperà di invocare le funzioni quando necessario. Da qui si vede che c'è differenza tra *initialize* e un *costruttore* perché la classe può avere sia il costruttore che la initialize con comportamenti diversi.

Definiamo il comportamento:
1. Quando la simulazione inizia, Tic manda un messaggio a Toc:
	```c++
	void Txc::initialize() {
		// se il nome del modulo è tic creo 
		// e mando il messaggio
		if(strcmp("tic", getName()) == 0) {
			// creo il messaggio
			cMessage* msg = new cMessage("CIAO");
			// invio il messaggio tramite il gate 
			// chiamato "out"
			send(msg, "out");
			// il messaggio arriverà dopo 100ms (delay)
		}
	}
	```
	Si noti che non c'è alcun riferimento al modulo Toc, questa cosa è fatta per poter facilmente sostituire un modulo con un altro senza dover cambiare il codice. Un gate è connesso ad un solo modulo.
2. Quando Txc riceve un messaggio, manda una risposta:
	```c++
	void Txc::handleMessage(cMessage* msg) {
		// in generale dovremmo controllare il tipo del 
		// messaggio perché un modulo potrebbe ricevere
		// più tipologie di messaggi, in questo caso non 
		// serve
		send(msg, "out");
	}
	```
	Il trasferimento del messaggio consiste solo nello spostare il puntatore al messaggio. OMNeT impedisce di fare duplici send dello stesso messaggio allo stesso ricevitore.
	Durante i 100ms il possessore del messaggio non è né tic né toc ma la *event queue* (è tipo il postino). 

La event queue può essere acceduta solo dall'event scheduler che preleva il primo evento della coda e lo esegue. 
Volendo possiamo estendere il suo comportamento definendo una nuova classe che estende le classi *cFutureEventSet* e *cScheduler*.
Il real time scheduler si utilizza quando si vuole runnare il simulatore al tempo reale. Questo è utile quando si vuole fare l'hardware in the loop simulation cioè connettere il simulatore con device reali. 

## Timer

Molto spesso si vuole eseguire un'azione dopo un certo periodo di tempo, ad esempio toc risponde dopo 10 secondi. Per fare ciò si può schedulare un evento per noi stessi tra 10 secondi, questo evento invierà il messaggio. 
```c++
class Txc: public cSimpleModule {
private:
	cMessage* beep_;
	cMessage* tictocMsg_;
};
void Txc::initialize() {
	if (strcmp("tic", getName()) == 0) {
		tictocMsg_ = new cMessage("tictocMsg");
		send(msg, "out"); 
	}
	beep_ = new cMessage("beep");
}
void Txc::handleMessage(cMessage* msg) {
	if(strcmp(msg->getName(), "tictocMsg") == 0) {
		scheduleAt(simTime()+10, beep_);
		tictocMsg_ = msg; // memorizzo il messaggio
	} else if(msg->isSelfMessage()) { // è come msg == beep_
		send(tictocMsg_, "out");
	}
}
Txc::~Txc() { 
	// questa forse era meglio metterala nella finish()
	cancelEvent(beep_); 
	// questa ci sta nel distruttore
	delete beep_; 
}
```

## Parametri

Un modulo può avere dei parametri ai quali si può assegnare un valore di default e un'unità di misura:
```c++
simple Txc {
	parameters:
		double proctTime = default(10);
	gates:
		input in;
		output out;
}
```
```c++
simple Txc {
	parameters:
		double proctTime @unit(s) = default(10s);
	gates:
		input in;
		output out;
}
```
I parametri si usano perché ad esempio tic potrebbe essere più veloce di toc a processare le informazioni. Inoltre, non si fanno le cose hardcoded, con i parametri ci basta ricompilare un solo file. Poi anche perché permette di memorizzare dei valori di default. 
```c++
void Txc::handleMessage(cMessage* msg) {  
	if( strcmp(msg->getName(),"tictocMsg") == 0 ) {
		// leggo il parametro
		simtime_t time = par("procTime");  
		scheduleAt( simTime() + time, beep_ );  
		tictocMsg_ = msg; // store msg for later transmission
	} else if (msg->isSelfMessage()) // same as msg==beep_
		send(tictocMsg_, "out");
}
```
Per noi simTime è un double ma in realtà viene memorizzato come un 64bit integer con rappresentazione in virgola mobile. 
Questo è importante perché cambiando l'esponente si può cambiare il livello di precisione temporale del simulatore. L'esponente di default è -12.

# Providing Random Inputs

Per generare numeri casuali OMNeT mette a disposizioni le principali distribuzioni:
- uniform();
- exponential();
- normal();
- ...
E permette di impostare il seed. 
```c++
void Txc::handleMessage(cMessage* msg) {  
	if(strcmp(msg->getName(),"tictocMsg") == 0) {
		// il tempo ora non è costante ma è una VA 
		// di una distribuzione uniforme tra 0 e 10
		double time = uniform(0,10);  
		scheduleAt(simTime() + time, beep_);  
		tictocMsg_ = msg; // store msg for later transmission
	}
	... 
}
```
Possiamo anche impostarla nel parametro: 
```c++
procTime = uniform(0s,10s);
```
ma in questo caso ogni volta che accediamo al parametro avremo lo stesso valore perché ormai è stato inizializzato.
Se volessi definire una VA variabile come parametro nel ned file per non fare le cose hardcoded dovrei usare la clausola *volatile*. 
Altrimenti si può fare così:
```c++
simtime max_time = par("maxTime");
scheduleAt(simTime() + uniform(0, max_time));
```

# Collecting Metrics

Il simulatore serve proprio per raccogliere dati (metrics).
Non useremo *cout* come facevamo nei programmi scritti in C++ normale ma utilizzeremo un approccio **build-in**: EV che stampa direttamente nell'interfaccia grafica. 
In pratica sostituiremo cout con EV.
Esistono varie tipologie di EV come: FATAL, ERROR, WARN, INFO, DETAIL, DEBUG e TRACE. 

Per raccogliere le metriche si utilizzano i **segnali** che sono una struttura dati che contiene tutti i dati collezionati nel corso della simulazione.
Il segnale viene dichiarato nel NED file e questo viene associato tramite un puntatore al file C++. La struttura dati è collegata ad un *recorder* cioè una classe che alla fine della simulazione produce le statistiche con i dati della struttura. Questo meccanismo ha il vantaggio di essere molto flessibile. 

Nel NED file scriveremo:
```c++
parameters:
	// definisco il segnale 
	@signal[delay](type=long);
	// il seguente è il recorder, in questo caso prende i 
	// dati dal segnale delay e produce la media
	@statistic[delayStat](source="delay"; record=mean;)
```

Nel file C++ scriveremo:
```c++
private:
	// definisco il puntatore al segnale
	simsignal_t delaySignal;
...
// nella funzione initialize() associo il segnale 
// alla variabile simsignal_t
registerSignal("delay");

...
// in qualsiasi punto del codice, quando ho un dato
// da scrivere nel segnale uso
emit(delaySignal, value);
```

I valori scalari producono un singolo valore, ad esempio sono valori scalari la somma, la media, la varianza. 
Un'altra tipologia di valori sono quelli vettoriali che permettono di vedere l'evoluzione temporale di un parametro. La struttura oltre che a memorizzare il valore, conterrà anche il timestamp di quando quel valore è stato prodotto.  Questa tipologia di valori sono molto utili in fase di debugging. 

# Defining Simulation Scenarios

Il file di configurazione (.ini) contiene uno scenario da simulare. 
La struttura è la seguente, è diviso in sezioni, la prima è sempre la General: 
```c++
[General]
// definisco il modulo da simulare, questo è utile
// per poter sostituire il modulo con un altro 
network = Tictoc
// il secondo parametro è il tempo massimo di simulazione
sim-time-limit = 100s
// questo invece serve per specificare il tempo reale
cpu-time-limit = 100s
// per ignorare i primi sample perché il sistema non 
// è ancora a regime e quindi sballerebbe le statistiche
warmup-period = 10s
...
[Config Test1]
description = "first TicToc campaign"
// questa serve per assegnare il valore del procTime che 
// è presente nel NED file
// visto che il network può cambiare, di solito si scrive:
// *.t*c.procTime = 50s
// oppure: *.*.procTime
Tictoc.t*c.procTime = 50ms
...
[Config specificTest]
// in questo caso, vado ad estendere la configurazione 
// Test1
extends = Test1
sim-time-limit = 5s
```

Nell'INI file definiamo:
- Parametri: una volta impostati rimangono gli stessi.
- Fattori: questi li cambieremo per vedere come cambia la simulazione.
Esempio di fattori:
```c++
**.t*c.delay = ${1, 2, 5, 10}
**.t*c.packetSize = ${50 , 100}
```
In questo modo, in due righe abbiamo definito 8 simulazioni.
Alla fine di ogni run posso confrontare le diverse simulazioni ma attenzione: i risultati valgono per i valori casuali che sono stati generati per QUELLA simulazione, per confrontare le varie opzioni dobbiamo ripetere le simulazioni varie volte. Per fare ciò aggiungo un'ulteriore parametro:
```c++
// ripetere ogni simulazione per 10 volte, genero colonne
repeat = 10
// questa serve per specificare che vogliamo lo stesso 
// seed delle riga
seed-set = ${repetition}
```
In questo caso possiamo anche mostrare il confidence interval.

Nell'INI si specifica anche la destinazione delle simulazione.
Al termine della simulazione vengono generati due file:
- .sca: contiene valori scalari.
- .vec: contiene valori temporali.
Per specificare la destinazione si scrive:
```c++
output-scalar-file = $[resultdir]/${configname}-${runnumber}.sca
output-vector-file = $[resultdir]/${configname}-${runnumber}.vec
```
ai fattori è associabile un nome in modo da creare le *iteration variabies* che possono essere usate come nomi nei file generati:
```c++
**.tic.delay = ${dtic = 1, 10}
**.toc.delay = ${dtoc = 1, 10}

// e poi scrivo
output-scalar-file = $[resultdir]/${configname}-${iterationvars}.sca
// e sarà tradotto come "dtic-1-dtoc-10"
// bisogna fare attenzione a non sovrascrivere le simulazioni
// quindi si aggiunge:
output-scalar-file = $[resultdir]/${configname}-${iterationvars}-${repetition}.sca
```

# Compound Modules

Un compound module è un modulo che contiene due o più simple module. 
Definiamone uno nel NED file:
```c++
module Pc {
	parameters: 
		int year;
		int monitorSize; 
	submodules:
		hd : HardDisk; 
		wifi: Wifi; 
		explorer: Browser;
		// qui posso avere sia simple che compound module
	gates: 
		...
	connections: 
		...
}
simple HardDisk {
	...
}
simple Wifi {
	...
}
simple Browser {
	...
}
```

Nel C++ si possono ottenere informazioni da altri moduli:
```c++
// prendo un puntatore al modulo
cMoudle * pc;
pc = getParentModule();
int size = pc->par("monitorSize");

cModule * hd = pc->getSubmodule("hd"); 
int hdSize = hd->par("hdSize");
// è preferibile utilizzare le funzioni get
int size = hd->getSize();
```
Queste funzioni vanno usate con cautela perché ci sono molti nomi (ad esempio hd, wifi, exploler) quindi cambiando il NED file bisogna cambiare anche il codice.

Anche i compound module hanno dei gate per poterli connettere con altri moduli (non necessariamente compound ma anche single module):
```c++
module C {
	parameters: 
	...
	submodules: 
		a: A;
		b: B;
	gates:  
		output out;
	connections:
		out <-- hd.out;
}
```

## Cross-module calls

Siamo nel modulo PC e vogliamo sapere dove si trova la risorsa X, per fare ciò, si chiede **all'oracolo**.
```c++
// prendo il puntatore all'oracolo
cModule* temp;  
temp = findModuleByPath(“oracle”);

// converto tramite cast
OracleClass* oracle;  
oracle = check_and_cast<OracleClass*>(temp);

int serverId;  
serverId = oracle->aSearchFunction("x");
```

Anche l'oracolo va usato con attenzione perché se genera un evento, il proprietario sarà colui che ha chiamato l'oracolo. 

# Connecting Gates

Ipotizziamo di voler un network, cioè un insieme di moduli connessi tra loro. Dovremmo utilizzare $n$ interfacce, una per ogni gate, in OMNet è possibile utilizzare un array di gate. 
```c++
vector gates
	input in[];
A.in++ <-- 1.out; // aggiungo un nuovo gate
A.in++ <-- 2.out; 
```
Con i vector gate si realizzano quindi i network.

Per quanto riguarda le interfacce wireless non possiamo utilizzare un modello basato sui cavi, possiamo utilizzare invece il concetto di *direct transmission*.
Non si trasmettono messaggi attraverso il gate ma si utilizza una comunicazione wireless:
```c++
[... in the NED file ...]
// questo consente di ricevere messaggi anche se i moduli
// non sono direttamente connessi, quindi in modo wireless
input wirelessGate @directIn;

[... in the C++ file ...]
destModule = <...>
destGate = <...>
// questa funzione è un'evoluzione della send
sendDirect(msg, propDelay, txDuration, destModule, destGate);
```
Si può usare l'oracolo per memorizzare i puntatori ai moduli presenti nella rete. 

# Modularità del Codice

Il progetto può facilmente diventare molto grande quindi è necessario utilizzare la modularità. La cosa migliore da fare è chiamare funzioni, possibilmente riutilizzabili. 
È inoltre consigliato controllare i casi particolari e lanciare un errore.

# Multi-Stage initialization

Un server contiene una serie di hard disk con una certa quantità di spazio disponibile.
Il server in fase di inizializzazione vuole sapere lo spazio totale disponibile.
Se inizializzo prima il server degli hard disk, sto cercando una quantità che però non è ancora stata inizializzata. 
Bisogna quindi controllare l'ordine di inizializzazione dei moduli, per fare ciò si utilizza la funzione di OMNet++ chiamata **multi-stage initialization**. Se questa funzione è abilitata allora la fase di inizializzazione viene eseguita *più di una volta*.
```c++
// in class declaration (.h)  
virtual void initialize(int stage);  
virtual int numInitStages() const { return 2; }

// in initialize definition (.cpp) 
void MyModule::initialize(int stage) {

if(stage == 0)  { ... }  
else if(stage == 1) { ... }

}
```
