
### Models of distributed computations

Il nostro sistema target è composto da un set di n processi $p_{1}, p_{2}, \dots p_{n}$, ognuno posizionato in nodi differente.
I nodi sono connessi tramite *channels*.
L'esecuzione di ogni processo può essere modellato come una sequenza di eventi che possono essere:
- **Internal Events**: come ad esempio cambiamenti di stato.
- **Communication Events**: la send e receive di un messaggio tipicamente in modo asincrono ma non necessariamente.

Ogni singolo processo è sequenziali ovvero esiste un total order (tutte le azioni sono su una sola linea). La sequenza di eventi eseguiti da un processo è chiamata history $h_{i}= e^{1}_{i}, e^{2}_{i},\dots e^{k}_{i}$.

Per ogni messaggio $m$ si può definire un ordine sugli eventi eseguiti su di esso come send e receive. Ad esempio la send si verifica prima della receive. 

Quando vogliamo rappresentare l'esecuzione distribuita si utilizzano gli space-time diagrams.

Un set di prefissi per tutti i processi histories è chiamato **cut**. Il set di tutti gli ultimi eventi di un prefisso è chiamato *cut frontier*.
Un consistent cut è un concetto per formalizzare la nozione di *possible progression point* per tutta l'esecuzione distribuita.

Un cut è consistente se soddisfa la seguente proprietà:
> [!note] Consistent Cut
> per ogni messaggio m tale che receive(m) è nel cut, send(m) appartiene anch'esso al cut.

è possibile specificare un ordine tra cut tramite inclusion.
### Notion of time

Il problema dei sistemi distribuiti è che non è possibile avere i clock di tutti i nodi sincronizzati. Si può ottenere sincronizzazione supponendo una certa tolleranza, altrimenti sarebbe impossibile. Per questo motivo non possiamo fare affidamento ad un reference global time. 

Senza un global time dobbiamo capire come relate events sono eseguiti a diverse progression unit. 

Il controllo di flusso di una esecuzione distribuita è descritta tramite le precedence relations tra eventi (ad esempio facendo un DAG).
Questa relazione può sostituire il concetto di global time per analizzare la struttura dell'esecuzione complessiva. 
La relazione "happens before" può essere definita per account for alla precedences throughout the execution. Gode della proprietà transitiva.
### Timestamping

