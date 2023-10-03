L’obiettivo di un DBMS è quello di memorizzare dati permanentemente (persistently). Poi, se si vogliono usare i dati questi devono essere corretti e integri (consistency) infine i dati devono essere disponibili in qualsiasi momento (availability).

In questo corso lavoreremo con sistemi distribuiti. Un sistema distribuito è un sistema deployato non in un singolo server ma in una classe di database organizzati opportunamente per comunicare tra loro tramite un network.

All’esame quando si risponde a una domanda specificare che il sistema che si sta considerando è distribuito. 

Aspetti positivi:
1. Possiamo garantire una grande scalabilità con costi moderati.
2. Tolleranza ai guasti perché anche se uno si guasta abbiamo altra unità sostitutive. 
3. Sono anche flessibili perché possiamo disattivare le risorse se in quel momento non sono necessarie. 

Problemi:
1. Se si hanno delle repliche, bisogna comunque garantire la consistenza. Ad esempio, se devo aggiornare un’informazione bisognerà farlo per tutte le repliche (availability). Questo genera lavoro extra da parte del server. 
2. Se si hanno interi parti di network fuori uso potrebbero esserci interi nodi isolati. 

## Data Persistency

I dati devono essere memorizzati in modo da non perdere quelle informazioni. Si possono usare Dischi o Memorie Flash (SSD) o altri.

## Data Consistency

Bob controlla il suo conto e, se ha abbastanza soldi compra un prodotto per la compagnia. Di conseguenza si aggiorneranno il conto di bob e il conto della compagnia. 
Bob deve aspettare che entrambe le informazioni vengano aggiornate, altrimenti ci sarebbe inconsistenza. 
Ora ipotizziamo di avere la stessa situazione ma con più repliche:
1. Scrivere sul server principale
2. Copiare le informazioni sul server di backup 
3. Ack di quando la copia è terminata
4. Finire l’operazione di scrittura
Questa si chiama two-phase transition. Queste sono tutte transazioni ACID quindi richiederebbero del tempo per venire eseguite. 

ACID però è troppo lento, non è accettabile aspettare troppo tempo per avere consistenza.

La disponibilità deve essere messa al primo posto. Esempio dell’e-commerce: 
- Il cliente prende dei prodotti e li mette nel carrello 
- Viene fatta una scrittura del server principale
- Viene aggiornato il server secondario
- ACK quando l’aggiornamento del server secondario è stato effettuato
- Risposta all’utente
Si perde troppo tempo, ad esempio per amazon anche un “insignificante” decimo di secondo nella risposta alle richieste reduce il profitto dell’1%. Quindi vogliamo una risposta veloce alle richieste. 

Dobbiamo considerare anche la partizione del network
Ci sono insiemi di computer collegati in un network, se il network non fosse attivo e dati e repliche si trovano in partizioni diverse allora non potrei aggiornare le repliche. Quindi avrei Avaliability ma non Concistency.
Quale devo preferire? Dipende. Posso decidere di mantenere attivo il servizio ma senza garantire la consistenza. 
Se però fosse un’applicazione bancaria la consistenza deve essere garantita quindi il sistema deve essere irraggiungibile per alcuni clienti (quelli coinvolti nel guasto) il sistema comunque per qualcuno funziona.

## Triangolo CAP

Dato un database distribuito non possiamo assicurare contemporaneamente:
- **Consistency C**
- **Availability A**
- **Partition Protection P**
Al massimo due delle precedenti funzioni possono essere trovare in un database distribuito. 

## Soluzioni CA

Vogliamo consistenza e disponibilità, ad esempio le banche.
Non possiamo garantire la tolleranza alle partizioni. 
Posso assicurare la consistenza solo fino a un certo numero di repliche (ad esempio 2 su 3).
Il problema è che la consistenza può inficiare sulle performance (la latenza) e la scalabilità.
In questo caso se una partizione smette di funzionare tutto il sistema va giù.

## Soluzioni AP

Qui consideriamo i social network. Se accedo ad un dato non ho la certezza di star accedendo alla replica più aggiornata, ma noi questo lo accettiamo per avere disponibilità.


## Soluzioni CP

Voglio consistenza ma se una parte del sistema non funziona voglio che il sistema continui a funzionare nelle partizioni funzionanti. Quindi tecnicamente la disponibilità non è garantita per tutti.

# BASE

Basically Avaiable: utilizzando la replicazione dei dati il sistema è sempre disponibile. (?)
Soft State: se faccio un’operazione di lettura su qualsiasi server posso ricevere una risposta che non corrisponde al valore più recente. 
Eventually Consistent: da qualche parte nel futuro tutte le copie saranno allineate. 

Queste proprietà si trovano nei database NoSQL. Nel triangolo sarei nel lato degli AP. 

Disponibilità -> Repliche -> ma vanno aggiornate quindi serve un sistema che garantisca la consistenza -> Latenza. 

La consistenza deve essere fatta il più velocemente possibile. 
Il 20% delle domande del test scritto è estratto da questa parte, non bisogna ricopiare il trafiletto ma argomentare (ricordarsi di scrivere che abbiamo un database distribuito e in che contesto siamo, perché dobbiamo occuparci di questa cosa):
Tipi di eventual consistenza:
- read-your-writes consistency
- session consistency 
- monotonic read consistency
- monotonic write consistency
- casual consistency: la causalità è una dipendenza di due azioni, non è una sequenza di azioni. se io faccio un’azione un’altra può partire, anche tardi ma partirà. C’è una relazione causale tra le due. Se un’azione ha una relazione causale con un’altra allora sono in questo caso. (il prof ha detto che questa domanda la sbaglia il 50% della gente che fa l’esame, sarà magari che la spiega lui da cane? Nooo mi dissocio)
	Se Mike risponde a un post di Pietro allora ho una relazione causare tra le due azioni. Non è una causa temporale ma causale. La memorizzazione deve rispettare la causalità.
	Attenzione! Non è un ordinamento temporale ma causale!
	Lui vuole vedere la causalità, sennò da metà punti.

