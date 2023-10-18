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

Tutti questi eventi hanno una funzione associata che viene chiamata **event handler**. Vediamo allora, nel nostro caso il compito di ogni handler: 
Poi dobbiamo calcolare la average delay le statistiche facendo $\frac{D}{k}$ 
Possiamo misurare la varianza? Con questi numeri no, dovremmo memorizzare tutti i delay in un array e non solo la somma D.

# Implementazione della event queue

È importante capire come questa è fatta è importante per capire le performance del nostro sistema. 
Viene implementata in due modi:
1. Min-heap tree
2. Calendar queue

## Min-heap tree

È un albero binario tale che:
1. il parent node è sempre più grande o al massimo uguale ai figli. 
2. Ogni livello è riempito da sinistra a destra. 

Queste condizioni sono importanti perché il primo nodo è sempre il minimo: quando vogliamo prendere l’evento vogliamo quello con minimo firing time e sarà $O(1)$ perché basta estrarre il primo nodo. 
Inoltre, la profondità sarà $log_2(\#nodes)$ e questa sarà la complessità di tutte le funzioni che lavorano sull’albero. 

#Domanda Ma se estraggo il primo, poi non devo anche riorganizzare l’albero? Quindi la complessità è O(1) o O(log(n))?

Può essere implementata tramite un array (no con i puntatori):
- posizione del figlio: 2j + 1, 2j +2
- posizione del padre floor((j-1)/2)

Per estrarre il prossimo evento dobbiamo fare un’operazione chiamata *rehepification* la complessità finale sarà $O(1) + O(log(n)) = O(log(n))$ 

Per inserire un nuovo elemento lo inseriamo nell’ultima posizione e poi rifare la *rehepification*, la complessità sarà $O(1) + O(log(n)) = O(log(n))$ 

Per l’eliminazione di un evento bisogna controllare tutto l’albero, quindi la complessità è $O(n)$ e poi rifare la *rehepification*, quindi avrei $O(n) + O(log(n)) = O(n)$.

#Domanda se avessi l’indice sarebbe $O(log(n))$?

## Calendar queue

Sulle slide. 

