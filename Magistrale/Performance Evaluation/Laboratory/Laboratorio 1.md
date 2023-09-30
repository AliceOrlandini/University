Ci sarà una prima parte teorica e solo dopo andremo a smanettare con OMNeT++. Solo verso il primo di novembre parleremo dei progetti.
## Introduzione alla simulazione

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

Come facciamo valutiamo le performance di un sistema?
1. **Misurazioni**: prendiamo *un sistema reale* e misuriamo gli output dati degli input. Il problema di questo metodo è che molte volte il sistema non lo abbiamo a disposizione oppure, anche se lo avessimo, non potremmo testarlo con input particolari. Per risolvere il problema dell'assenza del sistema si può costruire un prototipo che però è un'operazione molto costosa.
2. **Modello analitico**: si scrivono delle *equazioni matematiche* che descrivono il sistema. In teoria è il metodo migliore, il problema è che fare un sistema analitico è molto difficile, a volte impossibile. Oppure, anche se fosse possibile trovare le equazioni magari ci vorrebbero anni per risolverle. Per evitare ciò, generalmente si semplifica il sistema col rischio di semplificare troppo per ottenere equazioni semplificate che però non descrivono più il sistema in modo soddisfacente.
3. **Simulazioni**: si crea un *software* che replica solo gli aspetti rilevanti del sistema reale (o quelli che in quel momento vogliamo studiare). È una soluzione intermedia tra le prime due. 

Quale dei tre metodi è il migliore? Dipende, vediamo la seguente tabella:

|                      | Misurazioni | Modello Analitico | Simulazioni |
| -------------------- | ----------- | ----------------- | ----------- |
| Livello di dettaglio |      ✅     |          ❌       |      ❓     |
| Costo                |      ❌     |          ✅       |      ❓     |

Questi metodi possono comunque essere combinati, un approccio non esclude l’altro. Ad esempio potremmo realizzare un software di simulazione 

Noi parleremo di Simulazione:
> Simulazione significa imitare il comportamento di un sistema reale usando un computer.

Questo preclude che per fare una simulazione serve un software. Questo non è facile ma ce la faremo.

Come passiamo da un Sistema ad un Simulatore? Modellando sia gli input (semplificandoli, per esempio nel simulatore come input avrò una richiesta di pagine web basata su una media) che il sistema (replicando gli elementi essenziali essenziali del sistema nel simulatore). 
Eseguendo il simulatore avrò un’approssimazione dell’output, questo è ovvio perché ho semplificato il sistema e anche gli input. Il nostro obiettivo sarà quello di ottenere gli output approssimati il più vicini possibili a quelli reali (devono essere “statistically equivalent”).

**Vantaggi**
- Praticamente tutto può essere simulato.
- Ci permette di valutare design alternativi prima di effettuare la produzione.
- Ho il controllo completo delle impostazioni del sistema. Per esempio se testassi un’antenna wireless nel mondo reale avrei dispositivi che entrano nel sistema falsando i risultati. Se invece sono in un sistema simulato posso decidere esattamente quanti dispositivi saranno nel range dell’antenna. Inoltre, posso variare i parametri del sistema per vedere come variano gli output.
- La simulazione permette di modellare il sistema alla scala di tempo di cui abbiamo bisogno. Per esempio possiamo analizzare i nanosecondi. 

**Insidie**
- Dobbiamo eseguire la simulazione varie volte per avere un campione di dati.
- Spesso è costoso e time consuming.
- Bisogna essere esperti.
- Se non si progetta il simulatore *bene* si ottengono risultati sbagliati ed è facile sbagliare. Per esempio, posso avere giga di dati dati dalla simulazione ma se il modello non era corretto posso cestinarli. Possiamo essere certi dei dati solo dopo la validazione. 

Ci sono 3 categorie principali di simulazioni:
- Statiche vs Dinamiche
- Continue vs Discrete
- Deterministiche vs Stocastiche

Statiche vs Dinamiche
Qual è il peso massimo che un ponte può sopportare? Statico.
Quali sono le caratteristiche del traffico? Qui posso considerare il tempo, quindi è dinamico perché il sistema si evolve.

Continuo vs Discreto
Qual è la velocità massima di una macchina? La velocità è un valore continuo.
Quante persone ci sono in coda alle poste? 

Si può modellare un sistema continuo in uno discreto e viceversa ma i risultati saranno leggermente inaccurati.

Deterministici vs Stocastici
Qui consideriamo gli eventi randomici. Per esempio se voglio prendere un mutuo e voglio sapere quando finirò di pagarlo se ho un tasso di interesse fisso sarò in una situazione deterministica e so esattamente quando finirò mentre se il tasso di interesse è variabile non lo posso sapere in anticipo quindi userò random variables. 

DES: Discrete Event Simulators
Questi simulatori trattano sistemi dinamici, discreti e stocastici. Sono perfetti per noi informatici. 

# Come fare un BUON simulatore

Buon significa che vogliamo ottenere:
- risultati accurati del tipo che non si riescono a distinguere risultati simulati da risultati reali. 
- efficienza
- flessibilità in modo da permetterci di cambiare parametri, scenari e di estendere il simulatore con nuove funzionalità.

Quali strutture ci servono per fare un simulatore?
1. Una collezione di variabili che descrivono il sistema in un determinato momento e che cambiano valore nel tempo.
2. Una variabile particolare chiamata *simulation clock* che rappresenta il **tempo di simulazione**. Per esempio questa variabile può valere 10 secondi. A seconda del tempo le variabili cambiano valore. 
Quindi il numero totale di variabili è $N + 1$.

Real time vs simulation time 
Se il sistema si evolve nel tempo anche lo stato dovrà evolversi.
Quando nella simulazione avanziamo di tempo (per esempio 1 secondo) in realtà eseguiamo delle istruzioni sul PC (ricordarsi che il simulatore è un software) e la CPU necessita di tempo per eseguire le istruzioni di 1 secondo di simulazione. Questo tempo NON è quello di simulazione ma è quello dell’orologio che ho al polso e si chiama wall-clock time. Si ha:
wall-clock time $\not=$ simulation time
e non c’è una costante di proporzionalità tra le due. 
Quale dei due è maggiore dell’altro? Dipende dallo scenario simulato.

Gli eventi fanno cambiare lo stato del sistema. 
Ad esempio l’arrivo di una nuova richiesta ad un server è un evento perché lo stato del server cambia. Gli eventi esistono nel tempo.
Abbiamo due opzioni: 
1. Next-event time advance: se ho molti eventi il tempo di simulazione avanzerà molto velocemente perché ad ogni evento si avanza di $t$. (?)
2. Fixed-increment time advance: divido il tempo in fixed slot. Gli eventi continueranno ad arrivare in momenti casuali. Al tempo $D$ processo *tutti* gli eventi arrivati nello slot precedente. Se non ci sono eventi non faccio nulla. Quindi nella simulazione degli eventi appariranno come se fossero arrivati nello stesso momento.
Se gli eventi sono frequenti e arrivano molto vicini l’uno all’altro allora a 3D posso processarli tutti in una volta. Però se non arrivano eventi perdo tempo perché ad esempio a 2D perdo tempo.
La scelta dipenda dal tipo di sistema, se è intrinsicamente time slotted userò il secondo, altrimenti userò il primo. 
Se ho un next-event time advance volendo ricreare un time slotted time advance semplicemente creando eventi con un clock. 
La simulazione verrà eseguita solo quando arrivano eventi nel primo tipo. Nel secondo tipo la simulazione viene eseguita sempre a intervalli prefissati anche se non c’è un evento scatenante. 
Ma scusa non basta un if(ci sono eventi) {eseguo}?

Esistono però eventi che richiedono tempo per arrivare, per esempio il salvataggio di un file sul disco. Se nel simulatore vogliamo emulare un pc che salva dati sul disco possiamo:
- assumere che il salvataggio richieda zero secondi. (è una forte supposizione però)
- si trasforma in 2 eventi: un primo evento è il premere il tasto salva sul pc mentre l’altro è quando il il file è effettivamente salvato.
Ma quando aggiorniamo le variabili? Dipende da noi, possiamo aggiornare alla file di entrambi gli eventi o all’inizio del primo. L’handler dell’evento è colui che fa l’upgrade del sistema.
Come gestire eventi multipli dipende dall’implementazione dell’handler.

Event queue
Prima di tutto bisogna definire che cos’è un evento nel simulatore:
> Un evento è una struttura dati con un *firing time* che specifica quando quell’evento si verificherà

Avranno anche un event type per differenziarli l’uno dall’altro.
Tra gli altri dati possiamo avere ad esempio la priorità dell’evento. 

Poi aggiungeremo l’evento ad una coda degli eventi ordinata per firing time (in caso di parimerito in base alla priorità).
Per fare la coda possiamo usare un’array ma non è efficiente. (non conosciamo a priori gli eventi che abbiamo ma lo vedremo in futuro).
Un modo più efficiente sono le liste. 
Non useremo nemmeno le liste ma strutture ancora più efficienti che vedremo la prossima volta. (l’efficienza è fondamentale)

Ora ci mancano solo le statistical counters ovvero variabili o qualcosa di più complesso che racchiudono i risultati delle simulazioni. 