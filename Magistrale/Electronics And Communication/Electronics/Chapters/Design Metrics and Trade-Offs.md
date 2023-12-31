# Design Metrics

Le metriche principali sono:
- **Funzionalità**
- **Costi**: come abbiamo visto i [[IC Design Styles and Flows#Relazione costi performance|costi]] si dividono in fissi e variabili. 
- **Affidabilità** e **Robustezza**: si misurano in termini di *margini di rumore* e *immunità al rumore*.
- **Performance**: tra cui *velocità* (o delay) e *consumo di energia*.
- **Time-to-Market**
## Costo

Ecco alcune formule per il calcolo dei costi ricorrenti:
$$\text{costi variabili} = \frac{\text{costo del dado}+\text{costo dei test}+\text{costo del packaging}}{\text{test finale del prodotto}}$$
$$\text{costo del dado} = \frac{\text{costo del wafer}}{\text{dadi per wafer} \cdot \text{dadi prodotti}}$$
$$\text{dadi per wafer} = \frac{\pi \cdot (\text{diametro del wafer}/2)^{2}}{\text{area del dado}}-\frac{\pi \cdot \text{diametro del wafer}}{\sqrt{2} \cdot \text{area del dado}}$$
Qui Fanucci ha mostrato un botto di tabelle coi vari costi che boh io salto. 
## Reliability and Robustness

Il rumore è una variazione non voluta dei voltaggi e delle correnti presenti nei nodi logici. Per calcolarlo, bisogna considerare le capacità parassite presenti tra due componenti. 
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
