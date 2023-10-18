Iniziamo questa lezione calcolando il **delay medio** della coda:
$$D = \frac{\sum\limits_{i = 1}^{M} delay_{i}}{M}$$
dove $delay_{i}= departure\_time_{i} - arrival\_time_{i}$.
Per calcolare $D$ abbiamo 2 opzioni:
1. Usare un **file di log** (trace file): i problemi però sono che si va ad utilizzare molta memoria e le scritture su file richiedono tempo. Il vantaggio invece è che posso calcolare tutto ciò che voglio senza rieseguire la simulazione. Quindi, nonostante il consumo di memoria è comodo perché una volta finita la simulazione posso effettuare la computazione delle statistiche separatamente.
2. Calcolare la somma **”nel codice”** e alla fine della simulazione calcolare la media. In questo caso lo svantaggio è bisogna inserire del codice aggiuntivo e nel punto esatto con possibilità di errore. 

Consiglio di Nardini: a volte cerchiamo il bug nel codice del simulatore quando in realtà il problema non è il sistema in sé ma nel modo di calcolare le statistiche. 

# Averaging quantities

Bisogna fare attenzione perché a seconda di cosa vogliamo valutare bisogna usare la media giusta e di conseguenza le strutture dati adeguate.
Ad esempio abbiamo:
- **Rate**: dati $N$ samples $x_{1},x_{2},...,x_{N}$ la *rate mean* è: $$y = \frac{\sum\limits_{i = 1}^{N}x_{i}}{\tau}$$con $\tau$ l'intervallo di tempo totale tra il primo campionamento e l'ultimo. Questa media è utile ad esempio per calcolare il throughput e avrò bisogno di 3 variabili, la somma, il tempo iniziale e il tempo finale. 
- **Time indipendent samples**: dati $N$ samples $x_{1},x_{2},...,x_{N}$ la *sample mean* è: $$y = \frac{\sum\limits_{i = 1}^{N}x_{i}}{N}$$In questo caso, si può utilizzare $N$ perché l’arrivo dei campioni non dipende dal tempo quindi è sufficiente dividere per il numero di campioni. Questa tipologia di campioni si chiamano *impulsi*. Qui dovremo avere 2 variabili, la somma e il numero $N$ di sample.

Ipotizziamo di voler calcolare il numero medio di studenti in una classe:
- all’inizio avrò zero studenti perché l’aula è vuota
- poi si salirà a gradino
- infine, alla fine della lezione si riscenderà a gradino fino ad arrivare al momento in cui non c’è più nessuno
Abbiamo quindi un caso di:
- **Time dependent samples**: dati $N$ samples *dipendenti dal tempo* $(x_{1},t_{1}),(x_{2},t_{2}),...,(x_{N},t_{N})$ la *media pesata* sarà:  $$y = \frac{\sum\limits_{i=1}^{N-1}x_{i}\cdot (t_{i+1}-t_{i})}{\sum\limits_{i=1}^{N-1}(t_{i+1}-t_{i})} = \frac{\sum\limits_{i=1}^{N-1}(t_{i+1}-t_{i})}{t_{N+1}-t_{1}}$$In questo caso avremo bisogno di più variabili perché ci servono strutture per tutti i samples e per il loro tempo di arrivo. 

# Not only averadges

Proviamo a rispondere a queste domande: 
1. Quant’è la probabilità che il nostro buffer sia occupato sopra i 100KB?
2. Qual è il 99th percentile del ritardo?
Queste domande hanno risposte molto complicate a meno che non si applichino delle approssimazioni. 
Ad esempio, ipotizziamo di avere una **Probability Density Function** (PDF) che non sarà mai uguale alla realtà perché dovremmo avere un numero infinito di esperimenti ma ci può dare delle indicazioni utili. Per poterla considerare dovremo:
1. Definire il *range*, ovvero il valore minimo e massimo della distribuzione.
2. Dividere la granulosità dei segmenti, dovranno infatti essere tutti della stessa lunghezza. 
3. Dividere il range di interesse in segmenti chiamati *bin* con granulosità scelta al punto precedente.

Alla fine della simulazione si calcola la Probability Mass Function (PMF) (siamo in ambito discreto), e con questa si risponde ad esempio alla prima domanda: $P\{X < 20\} = \frac{5}{13} + \frac{3}{13} = \frac{8}{13}$.
I bin vengono memorizzati in un array di interi con una variabile in più *overflow*.
Bisogna fare attenzione alla dimensione dei bin perché potrebbero essere o troppo stretti o troppo larghi. 
Se invece si volesse solo la CDF possiamo evitare di usare i bin, basta infatti salvare i sample. 

# Come si da un input al simulatore?

Per rispondere a questa domanda ci sono due approcci:
1. **Trace-driven** simulation: si prende un file in cui ogni riga contiene il momento in cui avverrà quell’evento e una descrizione (ad esempio la size del pacchetto). All’inizio della simulazione si legge questo file.
2. **Self-driven** simulation: l’input è generato artificialmente usando dei generatori random che genererà il tempo al quale il pacchetto deve arrivare. 
La maggior parte delle volte useremo una distribuzione esponenziale perché non è facile trovare la distribuzione adatta al nostro problema. 
In generale, tutti e due gli approcci sono possibili ma a volte il self-driven è l’unico possibile. Per esempio se consideriamo i video compressi a volte i frame dipendono l’uno dall’altro.

Se invece non disponiamo dei dati, dovremo affidarci alle distribuzioni generate tramite *l’empirical distributions*, quella dei range e bin. 

# Generatore di numeri casuali per una distribuzione

Queste distribuzioni non sono usate solo per generare dei numeri ma anche per variare il comportamento del sistema, ad esempio, se un pacchetto arriva o meno a destinazione. 

Per generare i numeri casuali si possono usare due approcci:
1. Usare un generatore di numeri *uniformemente distribuiti*.
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
Per migliorare le performance si divide per $m$.
Se:
- b = 0: abbiamo un *multiplicative LCG*
- b > 0: abbiamo un *mixed LCG*

Il periodo sarà per forza al massimo $m$ ma può essere più piccolo. 
Se $P = m$ allora si dice che ho un *full period*.
Se ho mixed LCG la condizione sufficiente per avere un full period è avere:
- $m$ una grande potenza di 2 (esempio $2^{32}$)
- $a = 4c + 1$ con c un numero intero
- $b$ un numero dispari
C’è una condizione necessaria sulle slide. 

Se ho un multiplicative LCG allora è impossibile avere un full period, al massimo si può avere un periodo pari a $\frac{m}{4}$ perché considerando ad esempio le condizioni precedenti, $b = 0$ e quindi non è dispari.

Ora che abbiamo impostato i parametri dobbiamo scegliere $x_0$:
se siamo in un mixed LCG possiamo sceglierlo come vogliamo perché il periodo non dipende dal seed iniziale.
Se siamo nel caso multiplicative LCG allora il periodo dipende dal seed, le linee guide generali dicono di:
- evitare zero e numeri pari specialmente con multiplicative LCGs.
- numeri impredicibili come il timestamp o l’id del processo perché verrei meno alla proprietà di riproducibilità dei numeri. Questi sono utilizzabili quando si usano i numeri casuali randomicamente come per esempio per un gioco di carte. Quando facciamo simulazione però non stiamo giocando. 

# Indipendent streams of random numbers

Se abbiamo ad esempio due utenti che mandano pacchetti dobbiamo evitare di usare lo stesso generatore per entrambi perché una sequenza è incorrelata e uniformemente distribuita ma se facciamo che due utenti usano un valore ciascuno, non è assicurato che quel valori siano incorrelati. 
Quello che possiamo fare però è spezzare la sequenza in due per avere due streams separate, bisogna sapere però esattamente il numero $k$ di numeri per ognuno degli utenti. 

Bisogna fare attenzione ad alcuni effetti collaterali nascosti:
- se cambio la sequenza per l’utente 1 mi aspetterei che quella per l’utente 2 rimanga la stessa ma non è così. Esempi sulle slide. 

# Alcune proprietà degli LCGs

Ogni numero può essere calcolato deterministicamente conoscendo $x_0$: 
$$x_{i}= (a^{i}x_{0}+\frac{b(a^{i}-1)}{a-1})$$
Mi serve per dividere gli stream se ho più utenti (più streams).
Problemi: 
- $x_{i}$ può assumere valori solo razionali come ad esempio $\frac{1}{m}$, $\frac{2}{m}$, $\frac{3}{m}$, …, $\frac{P}{m}$. 
- È per esempio impossibile ottenere $\frac{0.8}{m}$ 

C++ mette a disposizione un generatore di numeri casuali: 
int rand()
void srand(unsigned seed)
Il grande problema è che questa funzione è dipendente dal compilatore, è un problema perché la stessa stream non è riproducibile su differenti architetture. Quindi non usare mai questa funzione nei progetti. 

Su omnet++ useremo il generatore Mersenne-Twister che ha un periodo molto lungo ma occupa molta memoria, circa 2KB per istanza.

# Testing RNGs

Ci sono dei modi per testare se un generatore genera effettivamente dei numeri casuali. 

Data una sequenza di numeri, questi sono una distribuzione tra 0 e 1? 
Un primo modo per rispondere è l’ispezione visiva facendo un grafico e vedendo se è lineare (QQPlot).
Un secondo modo è fare un *chi-square test* che è un metodo puramente matematico: si prendono dei valori e li si dividono in k buckets. Se sono distribuiti mi aspetto che in ogni bucket ci sia lo stesso numero di samples. Calcoliamo la deviazione rispetto al valore atteso e se è maggiore di un tot allora non sono uniformemente distribuiti. 
Il numero k deve essere scelto in modo tale che $\frac{n}{k} > 10$
Un terzo modo è usare il test di *Kolmogorov-Smirnov* che permette di usare un numero minore di samples. Si trova la ECDF e la si valuta. 
