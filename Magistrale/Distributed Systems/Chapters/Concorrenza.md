I due tipi di modelli per lo scambio di messaggi nei sistemi concorrenti sono:
1. **Memoria Condivisa (Shared Memory)**: I processi comunicano condividendo variabili in una memoria comune accessibile a tutti.
2. **Passaggio di Messaggi (Message Passing)**: I processi comunicano inviando e ricevendo messaggi attraverso canali di comunicazione definiti.
Si può dimostrare che questi due modelli sono equivalenti ma noi per evidenziare lo scambio di messaggi tra i nodi useremo il secondo metodo.

La differenza principale tra **processi** e **thread** riguarda la condivisione delle risorse e lo spazio di indirizzamento:
- **Processi**: Sono entità indipendenti con il proprio spazio di indirizzamento in memoria. Ogni processo ha le sue risorse allocate dal sistema operativo e comunica con altri processi tramite meccanismi come la memoria condivisa o il passaggio di messaggi.
- **Thread**: Sono flussi di esecuzione all'interno dello stesso processo. Condividono lo spazio di indirizzamento e le risorse del processo padre, il che permette una comunicazione e sincronizzazione più efficienti tra thread.

### Esecuzione Concorrente

#### Definizioni

Definiamo i seguenti elementi per descrivere un modello di esecuzione concorrente:
- **A (Actions)**: Un insieme di azioni, che rappresenta le operazioni o le attività da eseguire in un sistema. Ogni azione può essere considerata un'entità atomica che, all'interno di un contesto concorrente, può essere eseguita indipendentemente o in coordinamento con altre azioni.
- **PC (Precedence Constraints)**: Un insieme di vincoli di precedenza, che definiscono le dipendenze temporali tra le azioni. Alcune azioni devono necessariamente essere completate prima che altre possano iniziare.

Nel contesto di un grafo, rappresentiamo:
- Le **azioni** come nodi. Ogni nodo nel grafo rappresenta una singola azione.
- Le **relazioni di precedenza** come archi diretti tra i nodi. Un arco che collega il nodo di un'azione A a quello di un'azione B (A → B) indica che l'azione A deve essere completata prima che l'azione B possa iniziare.

Il concetto di **relazione** tra due azioni può essere inteso come una funzione che associa elementi del set di azioni a vincoli di precedenza, formando un sottoinsieme del prodotto cartesiano tra l'insieme delle azioni. Questo significa che, dato un insieme di azioni, ogni vincolo (arco) è una coppia ordinata che rappresenta una relazione di dipendenza tra due azioni. 

In pratica, questo modello ci permette di visualizzare e organizzare l'esecuzione concorrente delle azioni, garantendo che i vincoli di precedenza siano rispettati per evitare conflitti o esecuzioni scorrette.

```mermaid
graph TD; 
	A-->B; 
	A-->C; 
	A-->D;
	B-->E;
	C-->E;
	D-->E;
```

Il grafo risultante viene chiamato DAG (Directly Acyclic Graph) poiché è orientato e senza cicli. Da notare che le operazioni B, C, D possono essere eseguite in parallelo, mentre E deve essere eseguita solo dopo che B, C e D sono state completate. 

#### Rappresentazione Matematica

##### Partial Order
Un **partial order** (o ordine parziale) è una relazione che organizza un insieme di elementi in cui non tutti gli elementi sono necessariamente confrontabili tra loro. In altre parole, ci sono alcuni elementi che possono essere messi in ordine rispetto a una relazione, ma per altri elementi non è possibile dire quale precede l'altro.

Per esempio, immagina di avere un gruppo di compiti da fare. Alcuni compiti devono essere fatti in un certo ordine (ad esempio, devi leggere un documento prima di scriverne un riassunto), mentre altri possono essere fatti in qualsiasi ordine, senza dipendenze. Questo insieme di compiti con vincoli di esecuzione è un esempio di ordine parziale.

Definito matematicamente, un **ordine parziale** su un insieme A è una relazione binaria $\leq_{P} = PC^{+}$.

##### Non-strict partial order
Esistono anche altri ordini come il **non-strict partial order** (o ordine parziale non rigoroso), un tipo di relazione d'ordine in cui alcuni elementi possono essere confrontati con altri, ma non è richiesto che tutti gli elementi siano confrontabili tra loro. Questo tipo di ordine è chiamato "non-strict" perché permette la possibilità che un elemento possa essere in relazione con sé stesso.

```mermaid
graph TD; 
	A-->A;
	A-->B; 
	B-->C;
	A-->C;
	D-->C;
```

Le proprietà di un non-strict partial order sono:
1. **Riflessività**: Ogni elemento è in relazione con sé stesso. Per esempio, un compito è sempre "prima o uguale" a sé stesso, $\forall a\in A$,  $a\leq_{P} a$.
2. **Antisimmetria**: Se un elemento A è in relazione con un elemento B, e B è in relazione con A, allora A e B sono lo stesso elemento, $\forall a, b \in A, (a\leq_{P} b \text{ e } b\leq_{P} a) \Rightarrow a = b$.
3. **Transitività**: Se A è in relazione con B e B è in relazione con C, allora A è in relazione con C, $\forall a, b, c \in A, (a\leq_{P} b \text{ e } b\leq_{P} c) \Rightarrow a \leq_{P} c$.

##### Strict partial order
Uno **strict partial order** (ordine parziale rigoroso) differisce dal non-strict in quanto:
- Non è **riflessivo** ma **irriflessivo**: in un ordine rigoroso, $\not\exists \text{ } a < a$ la relazione  per nessun elemento a. In altre parole, nessun elemento è in relazione con sé stesso.
- La relazione viene spesso indicata con il simbolo $<$ invece di $\leq$, per sottolineare che è rigorosa.

##### POSET (Partially Ordered Set)
Un **POSET** (insieme parzialmente ordinato) è un insieme di elementi dotato di una relazione d'ordine parziale, che segue tre proprietà fondamentali: **riflessività**, **antisimmetria** e **transitività**.

##### Hasse Diagram
Un **Hasse diagram** è una rappresentazione grafica di un POSET. È un modo conveniente per visualizzare un insieme parzialmente ordinato, semplificando la rappresentazione della relazione d'ordine parziale. 
In un Hasse diagram:
- **Nodi** rappresentano gli elementi del POSET.
- **Archi** rappresentano la relazione d'ordine, ma vengono mostrati solo tra gli elementi immediatamente successivi nel POSET. Gli archi transitivi non sono rappresentati direttamente per evitare ridondanze.
- Le relazioni riflessive (come $a \leq_{P} a$) non sono mai rappresentate.
- L'orientamento degli archi è implicito: gli elementi più "alti" nel diagramma sono maggiori rispetto a quelli più "bassi" secondo la relazione d'ordine.

Esempio. 
Elementi di PC:
```mermaid
graph TD; 
	A-->D;
	A-->B; 
	A-->C;
	B-->D;
	C-->D;
	D-->E;
	C-->E;
```

Elementi di $PC^{+}$, vado ad aggiungere la proprietà transitiva:
```mermaid
graph TD; 
	A-->B; 
	A-->C;
	B-->D;
	A-->D;
	A-->E;
	B-->E;
	C-->D;
	D-->E;
	C-->E;
```
Elementi dell'Hasse Diagram, vado a togliere tutte le ridondanze:
```mermaid
graph TD; 
	A-->B; 
	A-->C;
	B-->D;
	C-->D;
	D-->E;
```

#### Concorrenza formalizzata

Formalmente, date due azioni $a\in A$ e $b\in A$ , esse sono **concorrenti** se e solo se non esiste una relazione $a \leq_{P} b$ o $b \leq_{P} a$. 
Questo implica che le azioni non sono soggette a vincoli di precedenza e quindi possono essere eseguite in modo indipendente, la concorrenza viene indicata con $a || b$.

Attenzione a non confondere la concorrenza con la transitività, ecco un esempio per provarlo: 
```mermaid
graph TD; 
	A-->B; 
	A-->C;
	B-->E;
	C-->D;
	E-->D;
```
In questo caso, $b || c$, $c||e$ ma b non è concorrente ad e, cioè non si può fare il passaggio successivo come si fa con la regola transitiva. 

##### Chain

Una **chain** (catena) in un insieme parzialmente ordinato (POSET) è un sottoinsieme in cui tutti gli elementi sono completamente ordinati rispetto alla relazione d'ordine parziale. In altre parole, ogni coppia di elementi nella catena è confrontabile.

In una chain vale **l'ordine totale** ($\leq_{T}$), cioè un partial order ma con la proprietà aggiuntiva di **totalità**.
Formalmente, dato un POSET P, un sottoinsieme $C\subseteq P$ è una chain se $\forall a,b \in C$, o $a\leq_{P} b$ oppure $b \leq_{P} a$. Questo significa che non ci sono elementi incomparabili all'interno di C, questa proprietà è detta totalità.

Esempio.
Dato il seguente Hasse Diagram:
```mermaid
graph TD; 
	A-->B; 
	A-->D;
	B-->C;
	D-->E;
	D-->F;
	C-->J;
	C-->H;
	J-->I;
	H-->I;
	E-->G;
	F-->G;
	G-->H;
```

Una chain rappresentata con l'Hasse diagram è:
```mermaid
graph TD; 
	A-->D;
	D-->F;
	F-->G;
```

Se le volessi rappresentare senza l'Hasse Diagram dovrei inserire anche gli archi dati dalla proprietà transitiva, cioè:
```mermaid
graph TD; 
	A-->D;
	A-->G;
	D-->F;
	A-->F;
	D-->G;
	F-->G;
```
Potenzialmente, anche una singola azione può essere considerata una chain, è di fatto un caso limite.

La **chain più lunga** in un **DAG** corrisponde al **critical path** (cammino critico). Questa chain ci è utile per diversi motivi, tra cui **determinare il tempo di esecuzione minimo**. 
Il cammino critico rappresenta la sequenza di attività che impone la durata minima totale in termini di tempo di esecuzione per completare tutte le operazioni. Qualsiasi ritardo in questa chain ritarderà l'intera esecuzione perché non c'è possibilità di parallelizzare queste operazioni. 

##### Synchronization Actions

Nell'esempio precedente abbiamo identificato la chain A, D, F, G. Tuttavia, prima di eseguire l'azione G, è necessario completare anche l'azione E, che non fa parte della chain.
```mermaid
graph TD; 
	A-->D;
	D-->F;
	D-.->E;
	E-.->G;
	F-->G;
```

Questo significa che dobbiamo creare una relazione tra la chain e le azioni esterne ad essa, evidenziando la dipendenza tra E e G. Per rappresentare questo legame, ogni volta che identifichiamo una chain, disegniamo tutti gli archi entranti verso il relativo nodo. Queste dipendenze vengono chiamate **synchronization actions**, e da esse si derivano i **precedence constraints**.

Un caso particolare si presenta quando abbiamo un solo worker (o un solo core). In questo scenario, possiamo semplicemente inserire il nodo E all'interno della chain, prima del nodo G, rispettando così le dipendenze. Questo processo è noto come **ordinamento topologico** (topological sorting) o **linear extension**, e può essere realizzato tramite l'algoritmo di Kahn.

### Algoritmo di Kahn

L'algoritmo di Kahn risolve il problema di ordinare le actions di un sistema concorrente, affinché vengano eseguite in modo sequenziale da un singolo worker (o una sola CPU). L'ordine delle azioni non può essere casuale, ma deve rispettare i vincoli di precedenza. Vediamo come funziona l'algoritmo.

Le azioni vengono suddivise in tre insiemi:
1. **Start Nodes (SN)**: insieme di tutti i nodi senza archi entranti, ovvero quelli da cui è possibile iniziare poiché non dipendono da altre azioni precedenti.
2. **Solution List (S)**: lista ordinata delle actions, che rappresenta la soluzione al problema. Alla fine dell'algoritmo, questa lista conterrà l'ordine con cui il worker eseguirà le operazioni.
3. **Other Nodes (M)**: tutti gli altri nodi che non appartengono agli altri insiemi.

L'obiettivo dell'algoritmo è spostare gradualmente tutti i nodi da SN e M alla lista S, rispettando i vincoli di precedenza.

```c++
while(SN != 0) {
	// preleva un nodo n da SN e aggiungilo 
	// in append alla lista S
	n = SN.getOne();
	S.append(n);
	// per ogni nodo m raggiungibile da n tramite l'arco r
	for(m) {
		// togli l'arco r dal grafo
		graph.remove(r);
		// se m togliendo l'arco r è diventato 
		// uno starting node (cioè non ha archi entranti)
		if(isStartingNode(m)) {
			// sposta m da M a SM perché è diventato uno
			// starting node
			M.remove(m);
			SM.append(m);
		}
	}
}
// quando arrivo qui vuol dire che SN è vuoto
// se il grafo ha ancora archi vuol dire che 
// c'è stato un problema, il grafo non era DAG
if(hasArchs(graph)) {
	throw e; 
}
// altrimenti ritorno la lista con la soluzione
return S;
```

Esempio. 

```mermaid
graph TD; 
	A-->C;
	B-->D;
	C-->E;
	C-->F;
	C-->D;
	D-->F;
	E-->G;
	F-->G;
```

*Stato Iniziale*
SN = {A, B};
S = [];
M = {C, D, E, F, G};

*Passo 1*
Seleziono un nodo qualsiasi da SN, ad esempio A, e lo aggiungo a S. A ha un solo arco uscente verso C, quindi rimuovo l'arco che va da A a C. A questo punto verifico se C è diventato uno start node. In questo caso lo è, quindi sposto C da M a SN.
SN = {B, C};
S = [A];
M = {D, E, F, G};

*Passo 2*
Seleziono C da SN e lo aggiungo a S. C ha tre archi uscenti, quindi li rimuovo e verifico se i nodi D, E e F sono diventati start nodes. E lo è, quindi lo sposto in SN, mentre gli altri due rimangono in M.
SN = {B, E};
S = [A, C];
M = {D, F, G};

*Passo 3*
Seleziono B.
SN = {E, D};
S = [A, C, B];
M = {F, G};

*Passo 4*
Seleziono D.
SN = {E, F};
S = [A, C, B, D];
M = {G};

*Passo 5*
Seleziono F.
SN = {E};
S = [A, C, B, D, F];
M = {G};

*Passo 6*
Seleziono E.
SN = {G};
S = [A, C, B, D, F, E];
M = {};

*Passo 7*
Seleziono G.
SN = {};
S = [A, C, B, D, F, E, G];
M = {};

Il controllo finale è superato in quanto tutti gli archi sono stati tolti.
Da notare che la soluzione non è unica e che gli archi da C a B e da F a E non erano presenti nel partial order originale.

```mermaid
graph TD; 
	A-->C;
	C-->B;
	B-->D;
	D-->F;
	F-->E;
	E-->G;
```

### Performance con più worker

Analizziamo come le performance del programma variano quando abbiamo accesso a **più worker** (ovvero, più risorse computazionali). Il nostro obiettivo è eseguire le azioni concorrenti in parallelo.
Definiamo:
- $P$: il programma nella sua versione sequenziale.
- $P^{'}$: il programma nella versione parallelizzata (potenzialmente più efficiente).
- $T(P)$: il tempo di esecuzione del programma $P$.
- $T(P^{'})$: il tempo di esecuzione del programma $P^{'}$.

L'obiettivo è ottenere $T(P') \leq T(P)$, ma vedremo che, in alcuni casi, questo non si verifica.

Per misurare le performance utilizziamo lo **speedup**:
$$
\sigma = \cfrac{T(P)}{T(P^{'})}
$$
Sappiamo che la versione parallela può essere suddivisa in due parti, una sequenziale $s$ e una parallela $p$:
$$
T(P^{'}) = s + p
$$
Definiamo la **sequential fraction** come il rapporto tra la parte sequenziale e il totale:
$$
\alpha = \cfrac{s}{s + p}
$$
Se disponiamo di $n$ risorse computazionali, possiamo ridurre la durata della parte parallelizzabile (mentre quella sequenziale rimane invariata). Di conseguenza, $p$ può essere ridotto a $\cfrac{p}{n}$, anche se nella pratica è difficile ottenere questa riduzione esatta a causa di fattori come lo scambio di messaggi e la sincronizzazione. Questo introduce un lavoro aggiuntivo noto come *computational overhead*.

Riscriviamo lo speedup in funzione di $n$ ed $\alpha$:
$$
\sigma(n, \alpha) = \cfrac{1}{\alpha + \cfrac{1-\alpha}{n}}
$$
conosciuta come **Legge di Amdahl**.

Facendo il limite per $n \rightarrow \infty$, otteniamo:
$$
\lim_{n\to\infty} \cfrac{1}{\alpha + \cfrac{1-\alpha}{n}} = \cfrac{1}{\alpha}
$$

Osserviamo lo **speedup** $\sigma$ (asse Y) in relazione al numero di worker $n$ (asse X).
Il primo grafico mostra la versione ideale con $\alpha = 0$, ovvero un programma completamente parallelizzabile:

```chart
type: line
labels: [1, 2, 3, 4, 5]
series:
  - title: alpha = 0
    data: [1, 2, 3, 4]
tension: 0.2
width: 80%
labelColors: false
fill: false
beginAtZero: false
bestFit: false
bestFitTitle: undefined
bestFitNumber: 0
```

Il grafico successivo rappresenta il caso con $\alpha = 1$, cioè un programma interamente sequenziale. In questo caso, non si ottiene alcun vantaggio dalla parallelizzazione, quindi lo **speedup** rimane sempre pari a 1:

```chart
type: line
labels: [1, 2, 3, 4, 5, 6, 7, 8]
series:
  - title: alpha = 1
    data: [1, 1, 1, 1, 1, 1, 1]
tension: 0.2
width: 80%
labelColors: false
fill: false
beginAtOne: true
bestFit: false
bestFitTitle: undefined
bestFitNumber: 0
```

Al variare di $\alpha$ (in aumento verso il basso), il comportamento risultante è il seguente:

```chart
type: line
labels: [1,2,3,4]
series:
  - title: alpha = 0
    data: [1,2,3,4]
  - title: 1/alpha1
    data: [1,1.8,2.7,3.2]
  - title: 1/alpha2
    data: [1,1.6,2.5,3]
tension: 0.2
width: 80%
labelColors: false
fill: false
beginAtZero: false
bestFit: false
bestFitTitle: undefined
bestFitNumber: 0
```

In un sistema reale, invece, la situazione è diversa: non si raggiunge il massimo **speedup** a causa dell'overhead:

```chart
type: line
labels: [1,2,3,4,5,6,7,8,9,10]
series:
  - title: alpha = 0 ideale
    data: [1,2,3,4,5,6,7,8,9,10]
  - title: alpha = 0 reale
    data: [1,1.9,2.9,3.9,4.9,5.9,4,3,2,1]
tension: 0.2
width: 80%
labelColors: false
fill: false
beginAtZero: false
bestFit: false
bestFitTitle: undefined
bestFitNumber: 0
```

Come si può osservare, superato un certo numero di worker, la parallelizzazione non offre più vantaggi, anzi, le performance iniziano a peggiorare.

Infine, esiste un ultimo caso chiamato **superlinear behavior**. Questo fenomeno si verifica quando il programma e il working set risiedono interamente nella cache, migliorando significativamente le performance. Tuttavia, se si verificano **cache miss**, le performance possono peggiorare.

```chart
type: line
labels: [1,2,3,4,5,6,7,8,9,10]
series:
  - title: alpha = 0 ideale
    data: [1,2,3,4,5,6,7,8,9,10]
  - title: alpha = 0 reale superlinear behavior
    data: [1,2.2,3.2,4.2,5.2,6.2,4,3,2,1]
tension: 0.2
width: 80%
labelColors: false
fill: false
beginAtZero: false
bestFit: false
bestFitTitle: undefined
bestFitNumber: 0
```
