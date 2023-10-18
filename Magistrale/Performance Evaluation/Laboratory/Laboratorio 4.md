Dopo essere certi che i nostri numeri sono uniformemente distribuiti dobbiamo chiederci anche se sono indipendenti gli uni dagli altri. Per testare questa condizione possiamo 
- effettuare un’ispezione visuale con ad esempio *lag plots*
- numericamente, tramite la funzione di autocorrelazione (non scenderemo nei dettagli).

Per verificare la bontà di un generatore di numeri casuali tramite la cosa migliore è basarsi sulla letteratura. 
#Attenzione non usare mai generatori non testati. 

Abbiamo visto come generare numeri uniformemente distribuiti ma ora proviamo a trasformarli in un altro tipo di distribuzione, ad esempio quella esponenziale. 
I metodi per fare ciò sono:
- inverse transform 
- convolution

## Inverse transform

Supponiamo di volere un sample $x$ da una variabile aleatoria data la sua CDF $F(x)$. Il codominio della CDF è tra 0 e 1, estraiamo usando un generatore che genera numeri uniformemente distribuiti un valore $u$ e computiamo la funzione inversa della CDF $x = F^{-1}(u)$. Quindi $x$ è un sample originato dalla distribuzione $F(x)$. 
Il problema di questo metodo è che c’è bisogno di invertire la CDF che molte volte non si può fare.

Nel caso di funzioni a gradino se conosco la lunghezza di ogni segmento non ho bisogno di calcolare l’inversa perché uno gradino corrisponde a un valore della PMF. 
Per fare ciò avrò bisogno di memoria per contenere una struttura dati che contiene per ogni gradino il valore della PDF.

Ad esempio la distribuzione normale non ha una forma chiusa. Per risolvere questo problema abbiamo un altro metodo quello della convoluzione con il Central Limit Theorem. Generiamo una serie di numeri indipendenti e uniformemente distribuiti tra 0 e 1 e poi li sommiamo tutti. Troviamo X che avrà un certo valor medio e varianza dato dal teorema. 

Il problema qui è il periodo, per generare questi numeri avremo bisogno di tanti numeri casuali e quindi raggiungeremo prima il periodo.

# How to do a simulation analysis

Ora che abbiamo visto tutto sui simulatori occupiamoci delle analisi. Il grande problema è che non c’è una procedura standard quindi Nardini ci darà solo delle linee guida che noi dovremo usare a seconda delle esigenze. 
Fare simulation analysis si compone dei seguenti step:
- Objectives
- KPI
- Model (non è il codice)
- Validation (prima del codice)
- Factors
- Tool
- Implementation (l’unica parte divertente)
- Verify (che l’implementazione combacia con il modello)
- Calibrate
- Design Experiments (parte delicata)
- Run
- Collect and Analyse Results

Nessuno di questi step può essere saltato. È importante avere un **workflow** soprattutto se il progetto è grande. (questo vale in generale, per qualsiasi progetto)

## Objectives

Sembra ovvio ma non lo è. Dovremo individuare degli oggetti SMART: Specific, Measurable, Achiabable, Repeatable and Thorough. 
Definire con chiarezza l’oggetto aiuta anche nella pianificazione di risorse (di tempo o umane) per realizzare il progetto.

#Domanda si può simulare quante risorse mi serviranno per realizzare un progetto? Una roba ricorsiva tipo.

## KPI

Dobbiamo definire gli indici di performance da misurare, per esempio di un protocollo video potremmo voler definire le metriche in termini di throughput o di latenza. 
Così sappiamo già le statistiche che il simulatore produrrà, va fatto subito così si ha già un’idea del sistema.

## Model

Abbiamo un sistema reale e dobbiamo modellarlo. Non saremo mai in grado di riprodurre tutto il comportamento del sistema, spesso nemmeno ne ho bisogno. L’approccio tipico è quello di togliere gli aspetti che non ci interessano. 
#Domanda si può fare modulare? Del tipo che all’inizio tolgo tanti dettagli ma il simulatore è predisposto ad essere espanso. 
Si può dividere il sistema in modelli separati, ognuno avrà un ruolo. 
Questa parte richiede di avere una visione completa del sistema reale, spesso non la si ha infatti il modelling è un’arte, non c’è un modo preciso per farla e si migliora con l’esperienza.
Bisogna anche mantenere un *documento delle assumption* per descrivere al meglio le assunzioni fatte per sicurezza nostra e dei clienti.

Attenzione ad una cosa, non necessariamente un modello dettagliatissimo è il modello migliore del sistema. Questo non è vero perché modellando tanti aspetti prima cosa bisogna essere esperti in ogni parte (cosa che non siamo) e anche un piccolo errore logico influisce tantissimo sui risultati del modello. Un’altra ragione è la flessibilità, se a un certo punto volessi sostituire il router con un altro dovrei dettagliare quello nuovo in tutte le specifiche ed è un problema di tempo e risorse. Se invece non è troppo dettagliato in cinque minuti questa cosa si fa.

## Validation

Ora che abbiamo il modello dobbiamo validarlo prima di implementarlo per davvero. 
Devo capire se le assunzioni sono ragionevoli.
Devo essere certa che il modello si comporti come il sistema reale e se trovo delle differenze devo capire se queste sono accettabili. 
Questo va assolutamente fatto prima di iniziare a scrivere codice.
Per validare un modello ancora una volta non c’è un modo preciso, alcune tecniche sono:
- intuizioni date dall’esperienza
- utilizzare misurazioni di un sistema reale (simile se non lo si ha a disposizione)
- con risultati teorici
Questa validazione si chiama *preliminare* perché poi ce ne sarà un’altra dopo aver scritto il codice che chiameremo *verification*.

## Factors

Ancora non mettiamo mani al codice, dobbiamo definire i fattori. Ad esempio se volessi valutare più opzioni devo modellare il sistema in modo che sia possibile variare i parametri del sistema per valutare scenari differenti. 
I fattori cambiano l’accuratezza del sistema.
Alcuni fattori non possono essere modificate in real life, di questo bisogna tenerne conto. 
#Domanda se un fattore non si verificherà in real life, perché inserirlo? 

## Tools

Ci sono molte alternative, ad esempio *omnet++*, nella realtà abbiamo molte opzioni. Tra queste opzioni si scegli in base a:
- il costo
- esperienza pregressa con un simulatore specifico.
- capacità del simulatore.
- tool di analisi compatibili o integrati nel software.

## Implementation

Da fare.