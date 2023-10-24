Ecco alcuni problemi:
- I document database molte volte non hanno supporto per le funzioni dei database relazionali come ad esempio SQL-like query language. 
- Il problema del key value me lo sono perso

All'inizio si cercò di lasciare il database relazionale ma includendo le operazioni di analisi, nel 2006 Google decise di sviluppare il suo database chiamato *Big Table*. 
I dati erano ancora memorizzati in tabelle ma con alcune caratteristiche:
- era organizzato in *colonne* sul disco
- i dati venivano indicizzati con: *riga*, *nome della colonna* e un *timestamp*.
- le operazioni di lettura e scrittura erano *atomiche*
- le righe venivano mantenute *ordinate*