Ecco alcuni problemi:
- I document database molte volte non hanno supporto per le funzioni dei database relazionali come ad esempio SQL-like query language. 
- Il problema del key value me lo sono perso

All'inizio si cercò di lasciare il database relazionale ma includendo le operazioni di analisi, nel 2006 Google decise di sviluppare il suo database chiamato **Big Table**. 
I dati erano ancora memorizzati in tabelle ma con alcune caratteristiche:
- era organizzato in *colonne* sul disco
- i dati venivano indicizzati con: *riga*, *nome della colonna* e un *timestamp*.
- le operazioni di lettura e scrittura erano *atomiche*
- le righe venivano mantenute *ordinate*
È una sorta di key based ma sempre relazionale perché dal *row id* si identifica un'informazione (non una riga, perché in una riga possiamo mettere più informazioni).

![Google Big Table|center|700](https://cloud.google.com/bigtable/img/storage-model.svg)

Ad alto livello ci saranno molti valori null ma a basso livello il database sarà ottimizzato a colonna. 
Una volta definita la lista delle column family il sistema si occuperà di allocarle sul disco in modo ottimizzato.