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
Se siamo in un'organizzazione a righe, devo fare accessi a tutti i blocchi del disco, se invece uso un'organizzazione a colonna devo leggere un solo blocco del disco.
È meglio che le informazioni siano il più vicini possibile nel disco, questo migliora tutte le performance. 
Un altro vantaggio è che è più facile effettuare delle compressioni (a seconda del tipo di algoritmo) al fine di ridurre la ridondanza.
Se invece vogliamo effettuare un'operazione di questo tipo:
```sql
INSERT INTO person
VALUES("Alice", "Orlandini", 3000)
```
Avremo dei problemi, le operazioni di inserimento o cancellazione sono molto onerose perché bisogna accedere a tutti i blocchi del disco. Ma se si vogliono eseguire delle transazioni non si sceglie questa architettura a basso livello. 

# Delta Store

Un delta store è un'area aggiuntiva del database *in memory* (una cache) ottimizzata per gestire informazioni che vengono aggiornate e usate frequentemente. Le informazioni contenute nel delta store sono poi unite periodicamente al database principale che avrà una struttura colonnare. 
Le query potrebbero richiedere di accedere a entrambi i database per ritornare informazioni complete e accurate. 
All'esame potrebbe essere chiesto di disegnarlo, è sulle slide.
# Projections

Quando si fanno analisi spesso si utilizzano più campi insieme, per migliorare le performance si utilizzano le **projections** in cui si vanno a mettere fisicamente vicine delle parti del database nel disco.
In questo caso creano delle tabelle che contengono dati aggruppati facendo query del tipo:
```sql
SELECT Region, Product, SUM(Sales)
FROM Sales
GROUP BY Region, Product;
```
```sql
SELECT Customer, SUM(Sales)
FROM Sales
GROUP BY Customer;
```
