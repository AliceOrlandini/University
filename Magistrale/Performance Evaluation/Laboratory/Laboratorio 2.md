Oggi andremo a studiare i vari modi per implementare in modo efficiente una coda degli eventi.
# General Porpouse vs ad-hoc simulator

Riprendiamo innanzitutto la differenza tra simulatore **general porpouse** ed uno **ad-hoc**.
Nel simulatore general porpouse avremo una serie di funzioni per gestire:
- event queue
- event scheduler
- statistic computation
- **simulation model**: in particolare questo è il componente che dovremo realizzare nel progetto perché i precedenti sono forniti da omnet++.
I primi 3 costituiscono il **simulation engine** che è, nei simulatori general porpouse, di per sé molto efficiente. Infatti, ciò che si fa generalmente è utilizzare lo stesso simulation engine con diversi simulation model sviluppati dal programmatore. 

Nel simulatore ad-hoc, bisogna invece sviluppare anche il simulation engine, questa scelta può essere presa per aumentare le perfornance in termini di ottimizzazione del codice ma richiede tempo e competenze specifiche. 
Ad esempio, se avessi bisogno di ottimizzare le strutture dati con ad esempio una event queue con inserimento in testa con il simulatore ad-hoc possiamo realizzarla. 
Gli svantaggi di questo approccio però riguardano:
- lavoro aggiuntivo che comprende tempo e soldi.
- spesso difficile, richiede delle competenze specifiche.
- una piccola modifica in un componente può richiedere di modificarne altri, per esempio se modifico la event queue, potrei dover cambiare anche l’event scheduler.

## Esempio

Prendiamo una single queue system, vogliamo sapere il delay medio dei pacchetti nella coda prima di essere inviati nella rete.

Assumiamo che:
- ogni pacchetto abbia la stessa lunghezza $L$ (byte).
- La banda del canale sia $C$ (bytes/s).
- La coda possa contenere al massimo $N$ pacchetti.
- Lo scheduler dei pacchetti nella la rete abbia un tempo di processazione nullo. 
- Il tempo per inviare completamente un pacchetto sia pari a $L/C$.
- La distanza temporale di arrivo dei pacchetti sia una variabile aleatoria $X$ tale che $E[X] = 1/\lambda$ (distribuzione esponenziale).

Definiamo gli step da intraprendere per modellare il problema: 
1. **Identificare le variabili di sistema**: 
	1. Quanti slot della coda sono occupati, li salveremo in una variabile $M$.
2. **Identificare gli statistical counters**: lo scopo della simulazione è effettuare qualche tipo di statistica quindi identifichiamo ciò di cui abbiamo bisogno per compiere le nostre statistiche.
	1. Dobbiamo trovare il *delay medio* dei pacchetti ovvero somma dei delay $x_i$ fratto $k$ il numero totale di pacchetti:  $\frac{\sum_{i = 1}^{k} x_i}{k}$.
3. Idendificare i **parametri che dovranno essere forniti in input** al simulatore:
	1. In questo caso avremo: $L$ la grandezza dei pacchetti, $C$ la banda del canale e $\lambda$ la distribuzione esponenziale.
4. Poi ovviamente ci servirà il **simulation clock**.

Ora che abbiamo individuato i componenti principali del problema ci occupiamo dell'**inizializzazione**:
- Il simulation clock lo inizializziamo a 0.
- $M = 0$ perché all'inizio avrò zero pacchetti nella coda.
- $k = 0$ perché all'inizio avrò inviato zero pacchetti.
- $d = \sum_{i = 1}^{k} x_i = 0$ perché all'inizio la somma dei delay sarà zero.
- $L$, $C$ e $\lambda$ possono essere importati tramite i file di configurazione oppure da linea di comando. ( #Attenzione MAI MAI MAI fare cose *hardcoded*, ad esempio nel codice scrivere $N = 3$ perché se volessimo cambiare quel numero dovremmo ricompilare tutto il simulatore).

Dopo l'inizializzazione **individuiamo gli eventi rilevanti** del sistema ricordando che un evento è *qualcosa* che cambia lo stato del sistema, nel nostro caso avremo i seguenti eventi:
1. L’arrivo di un nuovo pacchetto $f_{1}$.
2. Inizio della trasmissione di un pacchetto $f_{2}$.
3. Fine della trasmissione del pacchetto $f_{3}$.
4. Evento di fine simulazione $f_{4}$. È un evento particolare perché se i pacchetti continuano ad arrivare e vogliamo terminare la simulazione abbiamo bisogno di un evento speciale da invocare. Il momento di arrivo di questo evento viene stabilito tramite un parametro di configurazione che racchiude il tempo massimo di simulazione. 

Tutti questi eventi hanno una funzione associata che viene chiamata **event handler** che può anche generare altri eventi. 
Una prima osservazione è che dato che la lunghezza dei pacchetti è costante, il terzo evento, quello di fine trasmissione dei pacchetti, potrebbe essere eliminato. Noi però lo lasciamo perché *non si sa mai*, se in futuro volessimo modificare il simulatore togliendo l'ipotesi dei pacchetti costanti dovremmo cambiare il simulatore, lasciando invece gli eventi separati siamo già predisposti a future modifiche. 
Ma quale struttura dati dovrei usare per memorizzare gli eventi? Per i sistemi come questo è conveniente utilizzare un simulatore ad-hoc considerando che se $\frac{L}{C} \ll E[X]$ possiamo ipotizzare che:
- L'arrivo di un nuovo pacchetto sia un inserimento in coda.
- L'inizio e la fine della trasmissione siano inserimenti in testa. 
Tutte queste osservazioni possono essere utilizzate per rendere il sistema più efficiente ma bisogna fare attenzione perché generalmente *più si ottimizza e meno il sistema è flessibile*, bisogna quindi trovare un compromesso.

Alla fine della simulazione troveremo nelle variabili $D$ e $k$ il risultato della simulazione quindi non ci resta che calcolare il delay medio con la formula $\frac{D}{k}$.
Domanda, possiamo misurare anche la varianza? Con questi numeri no, dovremmo memorizzare tutti i delay in un vettore (non ci basta la somma $D$).
# Implementazione della event queue

La coda degli eventi rappresenta uno degli elementi fondamentali del nostro sistema. È quindi importante studiare le sue possibili implementazioni per capire come queste inficiano sulle performance globali del sistema. 
Principalmente, ci sono due modi per implementare la coda degli eventi: 
1. Min-heap tree
2. Calendar queue
## Min-heap tree

È un **albero binario** tale che:
1. il parent node ha sempre valore minimo o al massimo uguale ai figli. 
2. Ogni livello è riempito da sinistra a destra. 

Queste condizioni sono importanti perché estrarre il primo nodo significa estrarre il valore minimo e di solino noi quando vogliamo prelevare l’evento, vogliamo quello con firing time minimo e, con questa implementazione della coda, questa operazione sarà $O(1)$ perché basta estrarre il primo nodo. 
La profondità dell'albero sarà $d = log_2(n)$ con $n$ il numero di nodi, questa è pari alla complessità di tutte le funzioni che lavorano sull’albero. 

#Domanda Ma se estraggo il primo, poi non devo anche riorganizzare l’albero? Quindi la complessità è O(1) o O(log(n))? Riguardarsi algoritmi...

Al contrario di come abbiamo visto ad algoritmi, l'albero può essere implementato tramite un array invece che con i puntatori, la coda sarà strutturata in questo modo:
- posizione di due figli: $2j + 1$, $2j +2$.
- posizione del padre: $floor(\frac{j-1}{2})$.

Per estrarre il prossimo evento, cioè il primo nodo dell'albero, dobbiamo fare un’operazione chiamata **rehepification** ovvero una riorganizzazione dell'albero, la complessità finale sarà $O(1) + O(log(n)) = O(log(n))$ .

Per inserire un nuovo elemento la procedura consiste nell'inserirlo in ultima posizione e poi rifare la rehepification, la complessità sarà quindi anche in questo caso $O(1) + O(log(n)) = O(log(n))$.

Per l’eliminazione di un evento bisogna invece controllare tutto l’albero al fine di trovare quell'elemento e poi rifare la rehepification, quindi la complessità è $O(n) + O(log(n)) = O(n)$.
## Calendar queue

Questa coda viene chiamata così perchè possiamo immaginare l'array come il *mese*, il bucket come un *giorno* e gli eventi come *appuntamenti* fissati in quel giorno. 

Quindi, una calendar queue è un array di $M$ bucket. 
Un bucket è una lista *ordinata* di eventi e tutti i bucket hanno la stessa capacità $\delta$.
Un evento con firing time pari a $t$ è posizionato nel bucket in posizione $i = \lfloor \frac{t}{\delta} \rfloor mod M$ e tutti gli eventi memorizzati nello stesso bucket sono ordinati in base al loro tempo. 
Per quanto riguarda la complessità, il caso peggiore (worst-case) è quello in cui tutti gli eventi sono nello stesso bucket, in questo caso la complessità è $O(n)$ oppure $O(log(n))$ se si utilizza un'implementazione del bucket tramite min-heap e poi dobbiamo considerare anche che cerchiamo in tutti i bucket vuoti, cioè $O(M)$. La complessità finale peggiore sarà quindi $O(n + M)$.
La complessità media invece è molto bassa.
Ma quindi quale complessità devo considerare? Dipende dal tipo di simulatore. 

Per migliorare le prestazioni possiamo usare *l'anno* $j$ e controllare se il firing time dell'evento in cima all'albero del min-heap è maggiore di $j \cdot M \cdot \delta$ allora si avanza di bucket. Altrimenti mettiamo l'evento in quel bucket. 