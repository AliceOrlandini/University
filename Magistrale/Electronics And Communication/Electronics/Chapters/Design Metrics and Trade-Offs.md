# Design Metrics

Le metriche sono utili per scegliere opportunamente la tecnologia sulla quale implementare i nostri circuiti in modo da ottenere l'ottimo. 
Le metriche principali per la valutazione di un FPGA sono:
- **Funzionalità**: è la più importante perché se la funzionalità non è raggiunta, il circuito non fa quello che dovrebbe fare. 
- **Costi**: come abbiamo visto i [[IC Design Styles and Flows#Relazione costi performance|costi]] si dividono in fissi e variabili. 
- **Affidabilità** e **Robustezza**: si misurano in termini di *margini di rumore* e *immunità al rumore*.
- **Performance**: tra cui *velocità* (o delay) e *consumo di energia*.
- **Time-to-Market**
## Costo

Il costo dipende molto dal costo del wafer sulla quale verrà implementato il silicio, ad esempio può costare 10k. Se ad esempio il wafer è stato diviso in 100 dadi, il costo di ogni dado sarà 10k/100dadi.
Tuttavia, alcuni dadi probabilmente *non funzioneranno* e verranno scoperti in fase di test, questi vengono eliminati.
Poi bisogna includere il dado nel packaging e infine applicare il marchio e la classificazione.
![[cost_manufact.webp|center|400]]

Quindi **l'area** influisce sul costo del componente.
I non-recurrent cost sono:
- il costo di design
- la verification, ad esempio se si hanno 10 ingegneri: 7 saranno per la verification e 3 per il design. 
- la creazione della maschera
Mentre i costi ricorrenti saranno proporzionali all'area del chip e sono:
- la processazione del silicio
- l'assemblaggio
- i test
Alla fine otteniamo: 
$$\text{costo per chip} = \text{costi variabili per chip} + \frac{\text{costi fissi}}{\text{volume}}$$
in cui:
$$\text{costi variabili} = \frac{\text{costo del dado}+\text{costo dei test}+\text{costo del packaging}}{\text{test finale del prodotto}}$$
$$\text{costo del dado} = \frac{\text{costo del wafer}}{\text{dadi per wafer} \cdot \text{dadi prodotti}}$$
$$\text{dadi per wafer} = \frac{\pi \cdot (\text{diametro del wafer}/2)^{2}}{\text{area del dado}}-\frac{\pi \cdot \text{diametro del wafer}}{\sqrt{2} \cdot \text{area del dado}}$$
La **defect density** è il numero di defect che si hanno per unità di area, quindi la grandezza dell'area influisce su questo parametro.
Alla fine il numero di dadi prodotti saranno:
$$\text{dadi prodotti} = \left(1+\frac{\text{defects per unità di area}\cdot \text{dadi nell'area}}{\alpha}\right)^{-\alpha}$$

con $\alpha$ dipende dal processo di produzione come ad esempio dalla complessità delle maschere. 
## Reliability and Robustness

Il **rumore** è una variazione *non voluta* dei voltaggi e/o delle correnti presenti nei nodi logici. 
Ad esempio può capitare all'arrivo del clock poiché in quel frangente si scaricano o caricano le capacità quindi il voltaggio avrà una variazione. 
Per calcolarlo, bisogna considerare le capacità parassite presenti tra due componenti. 
Il livello di *"glitch"* dipende dalla tecnologia che si sta considerando, per esempio una tecnologia a 350nm CMOS avrà un glitch veramente basso, mentre quella a 120nm CMOS molto più evidente. 

![[reliability.webp|center|300]]
Il glitch è un problema perché è di fatto un *consumo di energia* (l'area sottostante alla curva è l'energia persa). 
Per quanto riguarda il problema di interpretazione del valore logico, esistono dei voltaggi soglia chiamati $V_{OH}$ e $V_{IH}$ per il valore logico 1, e $V_{IL}$ e $V_{OL}$ per il valore logico 0. L'area tra i due valori è chiamata: **margine di rumore alto o basso**. In quest'area è consentito avere rumore, mentre se il rumore eccede l'area, ci potrebbero essere dei problemi. 

![[noise_margin.webp|center|500]]

Si definisce **immunità al rumore** la capacità del sistema di trasmettere informazioni corrette in presenza di rumore.  

## Performance

Le performance si misurano principalmente in termini di *velocità* (delay) e *consumo di potenza* e riguardano soprattutto le reti combinatorie.
Queste riguardano ad esempio il *timing*, ad esempio nel caso di un inverter è il cosiddetto **propagation delay** $t_{p}$ cioè il tempo che impiega l'uscita ad adeguarsi quando vi è un cambiamento dell'ingresso. Esso si divide in High-to-Low ($t_{pHL}$) e Low-to-High ($t_{pLH}$).

![[delay.webp|center|500]]
Abbiamo già visto che:
$$t_{pHL} = \frac{K\cdot C}{\beta_{n}} \cdot \frac{1}{V_{DD}-V_{Tn}}$$
e:
$$t_{pLH} = \frac{K\cdot C}{\beta_{p}} \cdot \frac{1}{V_{DD}-|V_{Tp}|}$$

Ci sono anche i parametri:
- **Fan-Out**: numero di gate controllabili da un solo gate
- **Fan-In** numero di input di un gate 
che influiscono sulla velocità.

Per quanto riguarda il consumo di energia consideriamo la **Peak Power** ovvero la potenza massima di un circuito: 
$$P_{peak}= V_{dd}\cdot i_{peak}$$
Questa potenza è importante perché conoscendola posso dimensionare correttamente la *supply line* ovvero la linea per la fornitura di energia ai componenti. 

Un’altra potenza è quella media di dissipazione che influisce sulla batteria.
Infine, consideriamo il packaging and cooling requirements. 
Nel calcolo della potenza abbiamo componenti statici e dinamici. 
# Managing engineering trade-off

ASIP: Application Specific Istruction Processor
