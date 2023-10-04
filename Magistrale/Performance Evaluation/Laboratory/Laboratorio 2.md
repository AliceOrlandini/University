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







