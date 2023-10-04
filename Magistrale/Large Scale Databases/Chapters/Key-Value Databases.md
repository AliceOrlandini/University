
Questa è una prima struttura di NoSQL database. 
Iniziamo da un array: una lista ordinata di valori tutti dello stesso tipo indicizzati da un indice. I limiti sono che l’indice può essere solo un intero e che i valori devono avere tutti lo stesso tipo. 
Gli array associativi (dizionari in python) superano questi limiti perché il contenuto dell’array può variare di tipo e l’index può essere qualsiasi cosa come una stringa ad esempio. 

I dati devono essere salvati permanentemente nel database, in questo caso non c’è bisogno di tabelle ma ci basta salvare ogni valore con un identificatore unico. Possiamo usare questo tipo di database quando non dobbiamo mantenere relazioni specifiche tra entità. 
Esempio: e-commerce Prodotti, Ordini, Utenti ha senso usare i relazionali.

namespace: collezione di nomi usata nel nostro database. Il namespace è specificato nei bucket. Ad esempio se uso il bucket1 allora avrò un set di chiavi e il corrispettivo set di valori. Una chiave deve essere unica in bucket. 
Una entry in un bucket rappresenta una singola informazione. 

Motivi per usare questi db:
- semplicità
- velocità
- scalabilità

## Semplicità

Perché non dobbiamo definire lo schema del database né il tipo di dato per ogni attributo. Dobbiamo solo definire il bucket e le sue chiavi. 
È molto facile modificare attributi senza cambiare la struttura del db. 
È flessibile e forgiving: nessuno controllerà che venga inserito il valore corretto, ad esempio se un campo rappresenta un numero di telefono il DBMS non ci avvertirà se viene inserito un indirizzo di casa. 
Se il data model è semplice non ci dovrebbero essere problemi.

Dipende tutto da ciò che si deve fare: se devo scrivere la lista della spesa uso le note del telefono, se devo scrivere la tesi uso Latex. 

## Velocità

La maggior parte di questi database vengono copiati interamente in cache quindi le operazioni di lettura e scrittura sono molto veloci. Poi a un certo punto verranno effettuate le operazioni sul DB allocato sul disco. 

Anche nei DB relazionali c’è un sistema di caching ma qui i database sono stati ottimizzati per fare ciò.


