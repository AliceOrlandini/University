
# Design Metrics

Le metriche principali sono:
- Functionality
- Costi
- Reliability e robustezza
- Performance tra cui velocità e consumo di energia
- Time to market
## Costo

Innanzitutto il wafer ha un costo, poi alcuni dadi non funzionano e vanno buttati. 
Qui si ritorna al discorso dei non-recurring engineering cost e recurring costs già trattati [[IC Design Styles and Flows#Relazione costi performance|qui]].

## Reliability and Robustness

Ad esempio il componente deve essere il meno possibile sensibile al rumore (qualsiasi variazione alla tensione desiderata). Esiste il problema dei glitch che fanno sì che si sprechi energia. 
Il digitale è più robusto dell’analogico perché si definiscono i margini di rumore basso e alto. 

## Performance

Le performance si misurano principalmente in termini di velocità e consumo di energia. 
Ci sono anche i parametri Fan-Out(numero di gate controllabili da un solo gate) e Fan-In (numero di input di un gate) che influiscono sulla velocità.
Poi si considera il ritardo di propagazione.
Per quanto riguarda il consumo di energia consideriamo la Peak Power ovvero la potenza massima di un circuito, $P_{peak}= V_{dd}\cdot i_{peak}$
Un’altra potenza è quella media di dissipazione che influisce sulla batteria.
Infine, consideriamo il packaging and cooling requirements. 
Nel calcolo della potenza abbiamo componenti statici e dinamici. 
# Managing engineering trade-off

ASIP: Application Specific Istruction Processor
Basta non ne posso più.
