# OLTP vs OLAP

1. **OLTP (Online Transaction Processing)**:  sono architetture software orientate alla gestione di transazioni ACID.
2. **OLAP (Online Analytics Processing)**: sono architetture software orientate all'analisi veloce dei dati.

Con i database relazionali si usava principalmente l'architettura OLTP perché è un'architettura orientata ai record.
Se invece bisogna elaborare i dati tramite creazioni, update o cancellazioni si è passati all'OLAP.

# Data Warehouse

È un database relazionale in cui le informazioni sono organizzate per favorire l'analisi. Di solito si divide in *business processes* in cui si effettuano le operazioni ed è di tipo OLTP e *business data warehouse* in cui si estraggono informazioni per l'analisi e viene realizzato con OLAP.
Per realizzare un Data Warehouse si utilizza uno **schema a stella** in modo che query aggregate vengano eseguite velocemente. Questa tipologia di schemi non rappresenta uno schema relazionale completamente normalizzato perché la ridondanza è sesso accettata e le informazioni dipendono dalla chiave primaria. 
Questa tipologia di schemi hanno dei problemi:
- La processazione dei dati sono operazioni CPU e IO intensive
- I volumi dei dati crescono con le richieste degli utenti per le operazioni interattive.

# The Columnar Storage

Nel caso in cui non siamo interessati a estrarre la singola riga per estrarre le informazioni ma ci basta una rappresentazione aggregata per fare analisi allora è conveniente usare un'architettura a colonna. 
Esempio:
```sql
SELECT SUM(salary)
FROM person;
```
Se siamo in un'organizzazione a righe, devo fare accessi a tutti i blocch