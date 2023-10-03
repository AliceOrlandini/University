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


