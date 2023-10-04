Oggi parleremo molto di Event Queue

# General Porpouse vs ad hoc simulator

Nel simulatore general porpouse avremo una serie di funzioni per gestire:
- event queue
- event scheduler
- statistic computation
- **simulation model**: questo è ciò che dovremo realizzare nel progetto perché tutto il resto è fornito da omnet++.
I primi 3 costituiscono il simulation engine che è molto efficiente, si può infatti utilizzare lo stesso simulation engine con diversi simulation model. 

Nel simulatore ad hoc bisogna realizzare anche il simulation engine, questo lo si fa per ottimizzazione. Ad esempio magari ho bisogno di una event queue con inserimento in testa per chissà quale motivo e con il simulatore ad hoc lo possiamo fare. Così ottimizziamo le strutture dati che abbiamo a disposizione. 
Gli svantaggi sono che richiede lavoro in più, spesso difficile e poi che una modifica ad esempio la event queue magari richiede di cambiare anche l’event scheduler. (?)


# Esempio

Prendiamo una single queue system, vogliamo sapere il delay medio dei pacchetti nella coda prima di essere inviati nel network.

Assumiamo che:
- ogni pacchetto abbia la stessa lunghezza in byte L
- La banda del canale è C (bytes/s)
- La coda può contenere al massimo N pacchetti
- Lo scheduler dei pacchetti verso la rete abbia un tempo di processazione nullo. Il tempo per inviare un pacchetto sarà pari a $L/C$ 
- La distanza temporale di arrivo dei pacchetti è una variabile aleatoria $X$ tale che $E[X] = 1/\lambda$ 

Cose da fare: 
1. Identificare le variabili di sistema: 
	1. Quanti slot della coda sono occupati $M$
2. Identificare gli statistical counters:
	1. dobbiamo trovare il delay medio ovvero $\frac{\sum_{i = 1}^{k} x_i}{k}$ con $k$ il numero di pacchetti inviati ed $x_i$ il tempo di attesa del pacchetto): 
3. Poi ovviamente ci servirà il simulation clock.

Inizializzazione:
- Simulation clock = 0
- Zero pacchetti nella queue $M = 0$ 
- Zero pacchetti inviati $K = 0$
- Somma dei delay $D = 0$

Parametri di configurazione:
- Size dei pacchetti $L$
- Banda del canale $C$
- rate della distribuzione esponenziale $\lambda$

Questi possono essere impostati tramite i file di configurazione oppure da linea di comando. 

MAI fare cose hardcoded, esempio N = 3 MAI perché dovremmo ricompilare il simulatore.

- Eventi *rilevanti* del sistema:
Un evento è qualcosa che cambia lo stato del sistema, in questo sistema quali sono:
1. L’arrivo di un nuovo pacchetto
2. Inizio della trasmissione di un pacchetto
3. Fine della trasmissione del pacchetto
4. Evento di fine simulazione perché se i pacchetti continuano ad arrivare e vogliamo terminare la simulazione abbiamo bisogno di un evento. Questo è stabilito da un parametro di configurazione che racchiude il tempo massimo di simulazione. 

Tutti questi eventi hanno una funzione associata che costituiranno l’event handler (è il codice associato ad un evento).
#Domanda Esistono eventi senza handler? 

Vediamo cosa fanno gli handler:

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

