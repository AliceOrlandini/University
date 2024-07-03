Ci sarà una prima parte teorica e solo dopo andremo a smanettare con OMNeT++. Solo verso il primo di novembre parleremo dei progetti.
# Introduzione alla simulazione

In questa prima lezione andremo innanzitutto a definire che cos’è una *simulazione*.
Prima domanda: performance evaluation di che cosa?
Ad esempio, come facciamo a decidere se un nuovo algoritmo/protocollo o metodo è migliore di uno già esistente? Dobbiamo valutarne la performance.
Quindi definiamo per prima cosa un sistema:
> Un *sistema* è una **collezione di entità** che interagiscono tra loro per raggiungere un **obiettivo**. 

Vediamo di capire cosa significa "collezione di entità" tramite un esempio: in un network abbiamo per esempio un PC, un Server, Router e Switch. Se l’obiettivo del nostro studio è, ad esempio, la comunicazione tra PC e Server allora le entità che compongono il nostro sistema sono proprio PC e Server e potremo dunque tralasciare la presenza di Switch e Router. 
Se invece consideriamo il punto di vista di un telecomunicazionista probabilmente per lui il sistema sarà prevalentemente composto da Switch e Router. 
Questo esempio serve per evidenziare il fatto che **la collezione di entità dipende dall’osservatore**.

Un sistema in un certo momento è caratterizzato da un determinato **stato**:
> Lo **stato** di un sistema è un *set di variabili* che, col proprio valore, rappresentano il sistema in un certo istante. Lo stato può cambiare nel tempo in modo *continuo* o *discreto*.

La procedura di performance evaluation va a valutare come lo stato di un sistema cambia nel tempo osservando le *statistiche* (ad esempio il valor medio) o cercando anche di capire quali sono i valori *limite* del sistema.

## Come facciamo valutiamo le performance di un sistema?

Ci sono essenzialmente 3 modi:
1. **Misurazioni**: prendiamo *un sistema reale* e misuriamo gli output dati degli input. Il problema di questo metodo è che molte volte il sistema non lo abbiamo a disposizione oppure, anche se lo avessimo, non potremmo testarlo con input particolari. Per risolvere il problema dell'assenza del sistema si può costruire un prototipo che però è un'operazione molto costosa.
2. **Modello analitico**: si scrivono delle *equazioni matematiche* che descrivono il sistema. In teoria è il metodo migliore, il problema è che fare un sistema analitico è molto difficile, a volte impossibile. Oppure, anche se fosse possibile trovare le equazioni magari ci vorrebbero anni per risolverle. Per evitare ciò, generalmente si semplifica il sistema col rischio di semplificare troppo per ottenere equazioni semplificate che però non descrivono più il sistema in modo soddisfacente.
3. **Simulazioni**: si crea un *software* che replica solo gli aspetti rilevanti del sistema reale (o quelli che in quel momento vogliamo studiare). È una soluzione intermedia tra le prime due. 

Quale dei tre metodi è il migliore? Dipende, vediamo la seguente tabella:

|                      | Misurazioni | Modello Analitico | Simulazioni |
| -------------------- | ----------- | ----------------- | ----------- |
| Livello di dettaglio |      ✅     |          ❌       |      ❓     |
| Costo                |      ❌     |          ✅       |      ❓     |

Questi metodi possono comunque essere combinati, un approccio non esclude l’altro. Ad esempio potremmo realizzare un software di simulazione e poi validare i risultati prodotti tramite il software con equazioni matematiche. 

Noi in questo corso parleremo di simulazione:
> Con il termine **simulazione** indichiamo *l'imitazione del comportamento* di un sistema reale tramite un *software*.

Ma come passiamo da un sistema ad un simulatore? 
Dobbiamo *modellare* sia gli input (semplificandoli) che il sistema, replicando gli elementi essenziali nel simulatore. 
Eseguendo il simulatore avrò *un’approssimazione* dell’output (è ovvio ma è bene specificarlo) e il motivo è che abbiamo semplificato sia il sistema che gli input. 
Il nostro obiettivo sarà quello di ottenere degli output che, seppur approssimati, sono statisticamente equivalenti a quelli reali.

**Vantaggi**
- Praticamente tutto può essere simulato.
- Ci permette di valutare design alternativi prima di effettuare la produzione.
- Si ha il controllo completo delle impostazioni del sistema. Per esempio, se il nostro obiettivo è il test di un’antenna wireless, nel mondo reale potrei avere dei dispositivi che, entrando nel sistema, vanno a falsare i risultati; se invece sono in un sistema simulato posso decidere esattamente quanti dispositivi saranno nel range dell’antenna. Inoltre, posso variare i parametri del sistema per vedere come variano gli output.
- La simulazione permette di modellare il sistema alla scala di tempo di cui abbiamo bisogno. Per esempio possiamo analizzare i nanosecondi. 

**Insidie**
- Dobbiamo eseguire la simulazione varie volte per avere un campione di dati attendibile.
- Spesso è costoso e richiede tempo.
- Bisogna essere esperti in materia perché questo lavoro non consiste solo nella scrittura del software ma anche nell'analisi dei dati quindi sono richieste competenze di statistica e teoria della probabilità.
- Se non si progetta il simulatore *bene* si ottengono risultati sbagliati (ed è facile sbagliare). Per esempio, posso avere giga di dati dati relativi ad una simulazione ma, se il modello non era corretto, posso cestinarli. Infatti, possiamo essere certi dei dati solo dopo la validazione ed il test sul sistema reale. 

Esistono 3 categorie principali di modello di simulazioni:
- Statico vs Dinamico
- Continuo vs Discreto
- Deterministico vs Stocastico

### Statiche vs Dinamiche

> Un modello di un sistema è **statico** se il *tempo* *non* è un fattore rilevante.

In questa tipologia di sistemi generalmente l'output si ottiene tramite un'equazione del tipo $y = f(x)$ dove $f$ è il modello e $x$ è l'input e, come si può notare da questa scrittura, il tempo non è un fattore. È un modello utile per capire la relazione tra input ed output in sistemi complessi.
Esempio: qual è il peso massimo che un ponte può sopportare? Statico, non dipende dal tempo.

> Un modello di un sistema è **dinamico** se il *tempo* è un fattore rilevante. 

In questo caso, vogliamo valutare come il modello del sistema cambia nel tempo.
Esempio: quali sono le caratteristiche del traffico? Dinamico perché il sistema si evolve.

### Continuo vs Discreto

> Un modello di un sistema è **continuo** se i valori assunti sono *continui*.

Esempio: qual è la velocità massima di una macchina? Continuo perché la velocità è un valore continuo.

> Un modello di un sistema è **discreto** se i valori assunti sono *discreti*.

Esempio: quante persone ci sono in coda alle poste? Discreto, per ovvie ragioni.

Si può modellare un sistema continuo in uno discreto e viceversa ma bisogna stare molto attenti perché:
- se si modella la velocità della macchina con un sistema discreto potrei avere risultati inaccurati.
- se si modella il numero di persone in fila come un valore continuo potrei avere 3.4 persone in fila.

### Deterministici vs Stocastici

> Un sistema è **deterministico** se *non* ci sono componenti *aleatorie*. 
> Un sistema è **stocastico** se ci sono componenti *aleatorie*. 

Esempio: se voglio prendere un mutuo e voglio sapere quando finirò di pagarlo, se ho un tasso di interesse fisso, sarò in una situazione deterministica e quindi so esattamente quando finirò le rate; mentre, se il tasso di interesse è variabile, non posso saperlo in anticipo. 

<div style="page-break-after: always;"></div>

### DES (Discrete Event Simulators)

Questi sono una categoria di simulatori che trattano sistemi *dinamici*, *discreti* e *stocastici*. Sono perfetti per noi informatici perché ad esempio se consideriamo un network avrò:
- un'evoluzione nel tempo -> modello dinamico
- i pacchetti arrivano ad intervalli di tempo discreti -> modello discreto
- l'intervallo di tempo tra un pacchetto e l'altro è casuale -> modello stocastico

# Come fare un BUON simulatore

**Buon Simulatore** significa che vogliamo ottenere:
- *accuratezza*, a tal punto che non si riescono a distinguere risultati simulati da risultati reali. 
- *efficienza*, ad esempio i risultati devono essere prodotti in tempi brevi.
- *flessibilità*, in modo da permetterci di cambiare parametri, scenari e di estendere il simulatore con nuove funzionalità.

## Quali strutture ci servono per fare un simulatore?

Le strutture di cui abbiamo bisogno per fare un simulatore sono:
1. Una **collezione di $N$ variabili** che descrivono lo *stato* del sistema in un determinato momento e che cambiano valore nel tempo.
2. Una variabile particolare chiamata **simulation clock** che rappresenta il *tempo di simulazione* e che contiene il valore dell'attuale tempo simulato. 
Quindi il numero totale di variabili è $N + 1$.

## Real time vs simulation time 

Se il sistema è *dinamico* allora il suo stato si evolverà nel tempo.
Quando nella simulazione avanziamo di 1 secondo *simulato*, in realtà stiamo eseguendo delle istruzioni sul calcolatore (ricordarsi che il simulatore è un software) e la CPU necessita di tempo *reale* per eseguire le istruzioni vere e proprie. Questo tempo si chiama **wall-clock time** ed è quello reale. 
In generale si ha wall-clock time $\not=$ simulation time e tra i due non c'è nessun tipo di proporzionalità.

Quale dei due è maggiore dell’altro? Dipende dallo scenario simulato, ad esempio: se si verificano tanti cambiamenti di stato in pochissimo tempo *simulato* avremo bisogno di molto tempo *reale* per eseguire le istruzioni.
### Esempio

1. Ipotizziamo di avere un computer in grado di eseguire *4 istruzioni al secondo*.
2. Il programma che voglio eseguire è composto da *4 istruzioni*.
3. Di quanto tempo reale ho bisogno per eseguire tutto il programma? 1 secondo.
4. Se invece il computer fosse in grado di eseguire *8 istruzioni al secondo* allora quanto tempo reale servirebbe per eseguire il programma composto da *4 istruzioni*? Considerando che ho raddoppiato le istruzioni al secondo allora avrò bisogno della metà del tempo per eseguire il programma, ovvero 0.5 secondi.
5. Ora mettiamoci in un ambiente simulato. Il computer è in grado di eseguire *4 istruzioni al secondo* e il simulatore è in grado di *eseguire 4 istruzioni al secondo*. Il programma è composto da *4 istruzioni*.
6. Di quanto tempo reale ho bisogno per eseguire il programma? 1 secondo perché sto simulando 4 istruzioni su un computer che è in grado di eseguire proprio 4 istruzioni al secondo.
7. E invece quanto tempo simulato è trascorso? 1 secondo perché anche il simulatore esegue 4 istruzioni al secondo.
8. Ora ipotizzo che il simulatore possa eseguire *8 istruzioni al secondo*. Il programma rimane sempre di *4 istruzioni* e il computer può eseguire sempre *4 istruzioni al secondo*.
9. Di quanto tempo reale avrò bisogno per eseguire il programma? Sempre di 1 secondo perché il fatto che il simulatore vada più veloce non cambia le prestazioni.
10. Infine, in quest'ultimo caso, quanto tempo simulato è trascorso? 0.5 secondi perché il simulatore sarebbe in grado di eseguire 8 istruzioni in un secondo mentre io ne sto eseguendo solo 4. 

Sì lo so, ti si rivolta il cervello. 

## Evento 

> Un **evento** è qualsiasi cosa che fa *cambiare* lo *stato* del sistema. 

Un esempio di evento potrebbe essere l’arrivo di una nuova richiesta ad un server perché lo stato del server cambia. 
Il tempo di simulazione, salvato nella variabile *simulation clock*, può avanzare in due modi:
1. **Next-event:** è di tipo event-based e avanzo solo quando arriva un evento. Quindi se ho molti eventi il tempo di simulazione avanzerà molto velocemente. 
2. **Fixed-increment**: divido il tempo in slot di dimensione fissa. Gli eventi arrivano in momenti casuali ma non vengono eseguiti immediatamente. Al tempo fissato $D$ verranno processati *tutti* gli eventi arrivati nello slot da $D - 1$ a $D$. Quindi, nella simulazione gli eventi appariranno come se fossero tutti arrivati nello stesso momento. In questo caso, la simulazione viene eseguita sempre a intervalli prefissati anche se non c’è un evento scatenante. 

La scelta dipende dal tipo di sistema, generalmente però il secondo tipo si utilizza con sistemi che sono intrinsicamente discreti perché in caso contrario questo tipo di avanzamento porta più problemi che benefici.
Potenzialmente, se ho un simulatore di tipo next-event, posso convertirlo in un fixed-increment semplicemente inviando eventi con un clock. 
Noi useremo l'avanzamento di tipo next-event.

Esistono però eventi che richiedono tempo per arrivare, per esempio il salvataggio di un file sul disco. Se nel simulatore vogliamo emulare un pc che salva dati sul disco possiamo:
- assumere che il salvataggio richieda zero secondi, questa tuttavia è una forte supposizione.
- trasformare questo evento in 2 eventi separati: un primo evento è il premere il tasto salva sul pc mentre l’altro è quando il il file è stato effettivamente salvato.
Ma quando aggiorniamo le variabili? Lo decidiamo noi, possiamo aggiornare alla fine di entrambi gli eventi o all’inizio del primo, dipende dall'implementazione che vogliamo usare.

E se invece due eventi arrivano nello stesso momento? Basta *serializzarli*, ovvero eseguirli uno alla volta. 

## Event queue

Diamo una seconda definizione di evento:
> Un **evento** è una *struttura dati* composta da un *firing time* che specifica quando quell’evento si verificherà, un *event type* che specifica il tipo di evento e *other data* come ad esempio la priorità.

Ogni evento viene aggiunto in una **coda degli eventi** ordinata per *firing time* (e, in caso di parimerito, in base alla priorità).
Con quale struttura dati che conosciamo possiamo rappresentare la coda degli eventi?
Un primo modo per farlo è tramite un’array ma ci possiamo rendere presto conto che non è efficiente considerando che non conosciamo a priori gli eventi (questo perché un evento può scatenarne altri). Quindi ogni volta che arriva un nuovo evento bisognerebbe rimaneggiare tutto il vettore. 
Un modo più efficiente sono le liste ma nel nostro non le useremo. 
Useremo infatti strutture ancora più efficienti che vedremo la prossima volta.

## Statistical Counters 

Lo scopo di un simulatore è quello di valutare le performance di un sistema quindi dovremo raccogliere i risultati delle simulazioni.
Per fare ciò si usano le statistical counters:
> Le **statistical counters** sono variabili o qualcosa di più complesso che racchiudono i *risultati* delle simulazioni. 

Queste possono contenere valori scalari come massimo, minimo o medie, oppure possono contenere una variazione nel tempo di un parametro oppure ancora possono contenere grafici complessi come istogrammi. 

## Componenti di un simulatore

Quindi, un simulatore è composto da:
- **Strutture dati**:
	- $N$ variabili di stato
	- 1 simulation clock
	- 1 coda degli eventi
	- $M$ statistical counters
- **Funzioni**:
	- di inizializzazione: per esempio per resettare tutti i contatori.
	- event scheduler: per estrarre l'evento in cima alla coda degli eventi.
	- event handlers: per eseguire l'evento schedulato aggiornando lo stato e le statistical variables. Essi possono inoltre inserire nuovi eventi nella coda.
	- per il calcolo delle statistiche e per la loro visualizzazione.

L'attività del simulatore consiste in un ciclo infinito in cui:
1. Si estrae l'evento in cima alla coda degli eventi e si avanza il simulation clock.
2. Si processa l'evento tramite l'event handler:
	1. Si aggiorna lo stato del simulatore
	2. Si aggiornano le statistical variables
	3. Eventualmente, si generano altri eventi che vengono aggiunti alla coda.
3. Alla fine di tutta la simulazione, si visualizzano le statistiche di interesse.

![[Azioni Simulatore.png|center|400]]