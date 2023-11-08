# Teoria dei Grafi

Un grafo è una collezione di vertici e archi. 
Ogni vertice rappresenta **un’entità**, ovvero *istanze* di classi (oggetti). 
Le relazioni quindi si effettuano tra istanze di entità (non necessariamente tra lo stesso tipo di entità).
Questo non è modellazione ma implementazione.
Gli archi possono avere una direzione o meno.
Per trovare lo shortest path possiamo usare gli algoritmi come quello di Dijkstra.

Succede che alcune istanze interagiscano con loro stesse, in questo caso si parla di *loop*.

Ci sono alcune applicazioni in cui abbiamo più grafi che interagiscono tra loro, questi possono essere compattati tramite *intersezioni* o *unioni*. 

Il graph traversal è il processo di visita di tutti i vertici del grafo. 
Lo scopo di solito è quello di poter effettuare operazioni id lettura o di scrittura nei valori del grafo. Questa operazione di visita di tutti i vertici deve essere effettuata raramente perché è molto onerosa.

Due grafi sono isomorfi se:
- per ogni vertice del primo grafo esiste una corrispondenza nel secondo.
- per ogni arco tra un paio di vertici esiste un arco corrispondente tra vertici del secondo grafo.
L’identificazione di sotto-grafi isomorfi permette di identificare pattern utili. 

Un po’ di nomenclatura:
- **Ordine**: numero di vertici.
- **Size**: numero di archi.
- **Degree**: numero di archi connessi ad un vertice.
- **Closeness**: la misura di quanto lontano è il vertice da tutti gli altri vertici nel grafo.
- **Betweenness**: è la misura di quanto un arco fa da collo di bottiglia tra due vertico.

Un tipo di grafo, spesso usato in logistica, è il **Flow Network**. È un grafo direzionato caratterizzato dal fatto che in ogni nodo la somma dei pesi sugli archi entranti deve essere minore o uguale alla somma dei pesi degli archi uscenti.
Il **Bipartite Graph** non è direzionato e abbiamo 2 set di vertici, Tra vertici dello stesso set non si possono effettuare relazioni, le uniche relazioni possono essere effettuate tra vertici appartenenti a set diversi. Può essere usato per rappresentare la relazione tra studenti e insegnanti. 
Infine, il **Multigraph** è un grafo in cui possiamo definire differenti tipi di archi tra vertici. Per esempio da Lucca a Firenze posso andare in macchina e quindi avrò un certo tipo di relazione, oppure in treno e quindi avrò una seconda tipologia di relazione. 

# Graph Databases

Questa tipologia di database viene spesso usata per monitorare lo stato tra due entità. 
L’app del covid immuni era fatta così.