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
3. 