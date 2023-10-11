$$D = \frac{\sum\limits_{i = 1}^{M} delay_{i}}{M}$$
dove $delay_{i}= departure\_time_{i} - arrival\_time_{i}$ 
Abbiamo 2 opzioni:
1. Usare un file di log (trace file), i problemi sono che si va ad utilizzare molta memoria e le scritture richiedono tempo. Il vantaggio è che posso calcolare tutto ciò che voglio senza rieseguire la simulazione. Tuttavia, nonostante il consumo di memoria è comodo perché una volta finita la simulazione posso effettuare la computazione dei parametri separatamente.
2. Calcolare la somma ”nel codice” e alla fine della simulazione calcolare la media. In questo caso lo svantaggio è bisogna inserire del codice aggiuntivo e nel punto esatto. 

Consiglio di Nardini: a volte cerchiamo il bug nel codice del simulatore quando in realtà il problema non è il sistema ma il modo di calcolare le metriche. 

# Averaging quantities

Bisogna fare attenzione perché a seconda di cosa vogliamo valutare bisogna usare la media giusta (ne esistono di molte tipologie).

Ad esempio abbiamo:
- **Rate**: $y = \frac{\sum\limits_{i = 1}^{N}x_{i}}{\tau}$ utile ad esempio per il throughput. Avrò bisogno di 3 variabili, la somma, il tempo iniziale e il tempo finale. 
- **Time indipendent samples**: $y = \frac{\sum\limits_{i = 1}^{N}x_{i}}{N}$ Si può mettere N perché l’arrivo dei campioni non dipende dal tempo quindi basta dividere per il numero di campioni $N$. Questa tipologia di campioni si chiamano *impulsi*. Qui dovremo avere 2 variabili, la somma e il numero di sample.

Ipotizziamo di voler calcolare il numero medio di studenti in una classe:
- all’inizio avrò zero studenti perché l’aula è vuota
- poi si salirà a gradino
- infine, alla fine della lezione si riscenderà a gradino fino ad arrivare al momento in cui non c’è più nessuno
Abbiamo quindi un caso di:
- **Time dependent samples**: $y = \frac{\sum\limits_{i=1}^{N}x_{i}\cdot (t_{i+1}-t_{i})}{\sum\limits_{i=1}^{N}(t_{i+1}-i_{i})} = \frac{\sum\limits_{i=1}^{N}(t_{i+1}-t_{i})}{t_{N+1}-t_{1}}$ in questo caso dobbiamo considerare il valore del campione ma anche il tempo in cui arriva. È di fatto una media pesata. Qui abbiamo bisogno di più variabili perché ci serve memoria per tutti i samples e il tempo di arrivo di tutti i samples. 

# Not only averadges

Proviamo a rispondere a queste domande: 
1. Quant’è la probabilità che il nostro buffer sia occupato sopra i 100KB?
2. Qual è il 99th percentile del ritardo?
Queste domande hanno risposte molto complicate a meno che non si applicano delle approssimazioni. 
Esempio: ipotizziamo di avere una PDF, questa non sarà mai uguale alla realtà perché dovremmo avere un numero infinito di esperimenti. 

1. Definire il range, ovvero il valore minimo e massimo della distribuzione 
2. Dividiamo la granulatità dei segmenti, dovranno essere tutti della stessa lunghezza. Ogni segmento è chiamato *bin*

Alla fine della simulazione si calcola la distribuzione PMF, con questa si calcola la domanda $P\{X < 20\} = \frac{5}{13} + \frac{3}{13} = \frac{8}{13}$
Bisogna fare attenzione alla dimensione dei bins perché potrebbero essere o troppo stretti o troppo larghi. 
Qui bisogna mantenere un array di interi e un contatore per l’overflow. 

Se si volesse solo la CDF possiamo evitare di usare i bins, basta salvare i samples. 

# Come si da un input al simulatore?

Chi mette i pacchetti nella coda? Chi da gli input?

Possiamo usare 2 approcci:
1. **Trace-driven** simulation: si prende un file in cui ogni riga contiene il momento in cui avverrà quell’evento e una descrizione (ad esempio la size del pacchetto). All’inizio della simulazione si legge questo file. La maggior parte delle volte useremo una distribuzione esponenziale perché non è facile trovare la distribuzione adatta al nostro problema. 
2. **Self-driven** simulation: l’input è generato artificialmente usando dei generatori random che genererà il tempo al quale il pacchetto deve arrivare. 
In generale, tutti e due gli approcci sono possibili ma a volte il self-driven è l’unico possibile, per esempio nei video compressi a volte i frame dipendono l’uno dall’altro.
#Domanda nell’approccio self-driven il tempo viene assegnato prima della simulazione o durante? 
Mi sento poco bene, scriverò meglio questi appunti quando starò meglio. 

L’ultima risorsa è usare *l’empirical distributions*, quella dei range e bins. 

Dopo la pausa sto un po’ meglio, ci riprovo.
# Generatore di numeri casuali per una distribuzione

Queste distribuzioni non sono usate solo per generare dei numeri ma anche per variare il comportamento del sistema.
Ad esempio, se un pacchetto arriva o meno a destinazione. 

Per generare i numeri casuali si possono usare due approcci:
1. Usare un generatore di numeri uniformemente distribuiti
2. Usare un generatore variabile che genera un numero a caso e poi lo trasforma in un numero accettabile per la distribuzione che stiamo considerando

Noi per generare i numeri useremo i **metodi aritmetici**, ad esempio uno dei primi era il *midsquare method*:
1. Inizia un numero positivo di 4 cifre $Z_{0}$
2. Lo faccio al quadrato in modo da ottenere un numero da 8 cifre 
3. SI prendono le 4 cifre centrali e chiamo questo numero $Z_1$
4. Rendo il numero decimale mettendo zero virgola davanti, questo sarà il numero casuale 
5. Si ripete il procedimento considerando $Z_1$

Problemi: 
- tende a zero 
- non è veramente casuale perché è deterministico se conosco $Z_0$ (vedremo che questo non sarà un grande problema)

Due importanti verità:
**Non si possono generare numeri casuali con un computer**
**Non abbiamo veramente bisogno di numeri casuali**
Non ne abbiamo bisogno nel senso che adottiamo nella vita di tutti i giorni, a noi basta che abbia caratteristiche specifiche come:
- Uniformemente distribuiti 
- Incorrelati 
Poi vorremmo che il generatore sia veloce e che consumi poche risorse, perché generalmente dovremo generare un numero elevato di numeri. 
Un altro fattore importante (che è controintuitivo) è che voglio qualcosa di random ma allo stesso tempo che dia *risultati riproducibili*: se mi viene in mente, voglio avere la possibilità di riprodurre lo stesso identico output. Perché vorrei farlo? Perché se sto debuggando non troverei il bug se i risultati della simulazione cambiano. Inoltre, se sto valutando due algoritmi differenti per il sistema devo testarli con gli stessi input per valutarli al meglio. Infine, bisogna che siano riproducibili per la scienza, un esperimento è valido scientificamente se è riproducibile. 

Il simulatore deve anche garantire *separate streams* ovvero degli stream separati di numeri casuali. 

# Pseudo-random number generator

Generiamo un numero che è funzione di input che sono i numeri generati precedentemente:
$$x_{n}= f(x_{n-1}, x_{n-2},…, x_{0})$$
Questa funzione $f$ è deterministica (proprietà di riproducibilità).
Gli $n$ input sono chiamati *seeds* della sequenza, molto spesso $n = 1$ nel senso che per generare il numero successivo si usa il seed precedente. 

Come scelgo $f$? Sicuramente $f$ non può essere troppo complessa perché altrimenti non sarebbe efficiente e poi le sequenze dovranno essere incorrelate e uniformemente distribuite.
Alla fine faremo sempre un loop perché $f$ è deterministica e otterrei sequenze correlate, quindi ci serve una funzione $f$ che ci garantisca un periodo abbastanza lungo. 
#Attenzione se una funzione è molto complessa non significa che i risultati siano randomici. Non è che più la funzione è complessa e più i risultati sono randomici. 

# Linear Congruential Generators (LCGs)

$$x_{n}= |(a\cdot x_{n-1}+b)|_{m}$$
dove $0 < m$, $a < m$, $b < m$, $x_{0}< m$
