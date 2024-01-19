# Programmi Metrics

I due principali metodi di programmazione per implementare una $f(x)$ su un FPGA sono:
- **anti-fuse programming**: si applicano dei voltaggi differenti sull'FPGA che creerà una connessione tra i nodi A e B, questo sistema è detto OTP (one time programming)
- **SRAM based**: si memorizza nella memoria una configurazione di bit che permetterà di implementare la funzione. 
L'antifuse è più performante in termini di velocità ma l'SRAM è più compatibile con i vari FPGA presenti sul mercato. 
Vediamo come questi FPGA sono organizzati internamente.

# FPGA Architecture

L'architettura di un FPGA è la seguente: 
![[fpga_arch.png|center|500]]

Gli **I/O Pads** sono predefiniti in base alla board che andiamo ad acquistare e sono programmabili per ricevere input oppure essere output. 
Il **Core** è fatto di row di celle con spazi per il routing channels. Queste celle sono uguali per tutti (quei quadratini in blu) e le connessioni vengono fatte negli spazi (quelle linee bianche).

La **basic logic cell** è composta da *8 input*, *due multiplexer* e *una OR gate*. Se si vogliono implementare funzioni più complesse è necessario mettere insieme più basic logic cell, ad esempio se volessimo fare un D-Flip-Flop avremmo bisogno di due basic logic cell. 

![[basic_logic_cell.webp|center|300]]

L'interconnessione è una matrice di fili per indirizzare il segnale e implementare le differenti interconnection function. 

![[interconnection.webp|center|400]]

Un'altra architettura che andiamo ad analizzare è quella della Xilinx (che è quella che andremo ad usare in laboratorio). Anche qui abbiamo gli I/O Pads usando una strutt

# Design Metrics

Le metriche principali per la valutazione di un FPGA sono:
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
