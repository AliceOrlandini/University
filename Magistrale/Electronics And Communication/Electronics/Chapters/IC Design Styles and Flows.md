# Astrazione elettronica a livello di design

Prendiamo ad esempio un inverter, esistono 4 livelli di astrazione:
1. **RTL** (Register Transfer Level): rappresentazione tramite linguaggio id programmazione, in Verilog ad esempio, *out <= NOT(in)*
2. **Gate**: rappresentazione con il simbolo circuitale dell’inverter.
3. **Transistor**: rappresentazione tramite una combinazione di [[IC-Manufactory Review#Transistore MOSFE|MOSFET]] di tipo nmos o pmos.
4. **Lay-out**: rappresentazione della geometria e dei diversi livelli, spesso fatta tramite software.
Man mano che si scende nel livello di astrazione aumenta il tempo di realizzazione poiché si scende sempre più nei dettagli.
# Stili di design di circuiti integrati

## Full Custom vs Semicustom

Nel caso della metodologia **Full Custom**, si ha la *massima libertà* di design in termini di dimensione, posizionamento e interconnessione di ogni singolo transistor. Ciò permette di raggiungere il massimo potenziale del circuito in termini di area occupata e di velocità. D'altro canto però abbiamo maggiori costi, tempi di design e sono richieste capacità specifiche da parte degli sviluppatori.
Nel **Semicustom** invece, c’è una riduzione nella libertà di design perché si hanno componenti pre-disegnati e l'unico aspetto che il designer può decidere riguarda le interconnessioni tra le strutture logiche messe a disposizione. Questo secondo metodo facilita notevolmente il processo di realizzazione in termini di tempo, di costi e di abilità richieste rispetto all'approccio full-custom. Tuttavia, in termini di ottimizzazione, questo tipo di circuito è più limitato.

![[IC Design Styles and Flows.png|center|700]]

I livelli di design per passare dal livello *astratto* a un livello *concreto* sono:
1. **Specifiche**: si definisce che cosa deve fare il circuito.
2. **Architettura**
3. **Modello Funzionale**
4. **Logica**
5. **Circuiti**
6. **Poligoni**: questi sono inviati alla fabbrica che produrrà il chip.

## Full-custom 

Vediamo i passaggi di un design full custom considerando la realizzazione di un inverter.
1. Nella fase di **specifica** si hanno indicazioni riguardo ad esempio l’*area massima* occupata, il *tempo di propagazione* massimo o la *potenza consumata*. In questa fase si definisce anche la tecnologia, ad esempio, potremmo scegliere una tecnologia CMOS a 5nm. 
2. Definiamo poi la **topologia** del circuito a partire dalle tabelle di verità, questa è già un’operazione di *sintesi*.
3. Dopo aver definito la topologia, si calcola l'**area dei transistor** $W_{p}$ e $W_{n}$.
4. A questo punto si eseguono delle **simulazioni** sul funzionamento con, ad esempio, software come SPICE. 
5. Nella fase di **layout** si definisce ogni strato del dispositivo, a questo punto si conoscono esattamente le sue misure. Il *DRC* (“Design Rule Check”) è il software che si occuperà di verificare se le misure sono corrette.
6. Poi, **controlliamo** che le regole specificate all'inizio in fase di specifica sono corrette, questa operazione generalmente si compie utilizzando software *LVS* (“Layout vs Scheme”) che automaticamente controlla se il layout è uguale allo schema che avevamo fatto all’inizio. 
7. Infine, c’è l'*LPE* (“Layout Parasitic Extraction”) che è una **simulazione** che tiene in considerazione il modello dei transistor e il layout. 

Se qualsiasi cosa va storta si ritorna alle fasi precedenti. 
Bisogna considerare che tutti questi software si ottengono tramite *licenza* che ha un costo nell'ordine dei milioni di euro all'anno. Inoltre, gli sviluppatori devono avere abilità specifiche per utilizzarli. 
Per questi motivi, noi non useremo il full-custom flow ma era bene sapere come funziona. 
## ASIC semi-custom

Ora vediamo il semi-custom design style. 
In questo caso, l'azienda produttrice fornisce una serie di elementi logici da loro fabbricati che si possono assemblare per creare ciò di cui abbiamo bisogno. 
Un ASIC può essere ottenuto utilizzando due metodi: **Standard-Cell** e **Array-based (Gate-Array)**. 
La metodologia di progettazione delle standard-cell utilizza celle di una libreria pre-progettate. In altre parole, mentre si designa il circuito, le celle vengono raccolte da una libreria. L'array gate, come suggerisce il nome, ha un array di gate (possono essere porte AND o NAND, ecc..) che possono essere collegati opportunamente per produrre la funzionalità desiderata.
Le interconnessioni sono metalliche e posizionate in canali opposti (tendenzialmente viene usato meno del 50% dei transistori).
*At the end of the story* il cuore del gate array è composto da PMOS ed NMOS connessi opportunamente tra loro per implementare una certa funzione logica.

Nell'approccio standard cell invece, si hanno celle di altezza fissa ma larghezza variabile. La dimensione dei canali di interconnessione non sono fisse ed anche le posizioni dei piedini di IO sono variabili. 
Quindi, in questo caso, si ha più libertà di design che comporta una maggior ottimizzazione dell'area e della velocità. Di contro invece si hanno maggiori costi in termini di tempo di sviluppo e di abilità richieste per progettare questi circuiti. 

La tecnologia **FPGA** ha un'architettura di tipo *gate array* che però, rispetto a questi, ha molta più potenzialità in quanto può essere programmato sia dal punto di vista logico che dal punto di vista delle interconnessioni.

Noi lavoreremo al **Register Transfer Level**. Infatti, come avveniva nel Verilog a reti logiche abbiamo dei registri e all'arrivo del clock lo stato del sistema cambierà, dovremo quindi implementare una certa logica tramite descrizioni dell'hardware.

Il **TapeOut** è la fase finale in cui si mandano i poligoni all'azienda produttrice che li usa per realizzare le maschere per il processo di produzione dei transistor. Le maschere sono uno dei costi più importanti da considerare. 
# Relazione costi performance

Vediamo ora i costi di un circuito integrato che hanno un'importanza fondamentale sulla scelta dello stile di desing. I costi si dividono in:
- **Costi fissi** $RE$: sono costi che si sostengono indipendentemente dal numero di pezzi realizzati. Esempi sono: 
	- Costi di design: licenze software, salario ingegneri, ricerca e sviluppo.
	- La macchina che produce le maschere e le macchine per i test.
- **Non Recurrent Engineering Cost** $NRE$: costi dei materiali e della manodopera che richiede la produzione di un singolo pezzo. Un esempio è il costo del wafer: questo costo diminuisce al diminuire dell'area del chip ($cost \propto A^{3}$).
Il costo per singola unità sarà: $$C_{1}= \frac{NRE}{n} + RE$$mentre il costo totale sarà: $$C_{T}= NRE + (n \cdot RE)$$La relazione tra $n$ e $C_{1}$ è di tipo iperbolico, infatti, per $n$ piccoli il costo $C_{1}$ aumenta esponenzialmente mentre all'aumentare di $n$ la curva si attenua fino all'asintoto $RE$.

![[cost curve.png|center|500]]

Domanda: quale tra i design che abbiamo visto ha un costo $NRC$ maggiore? Il maggiore sarà sicuramente il Full-Custom, mentre gli approcci Semi-Custom come Gate-Array o Standard-Cell hanno tempi di design molto simili. Mentre per quanto riguarda costi fissi $RE$ l'approccio Full-Custom e Standard-Cell sono molto simili perché vengono realizzati da zero.
Invece, per quanto riguarda il volume di produzione? Se abbiamo volumi grandi è conveniente usare il Full-Custom o lo Standar-Cell. Se invece abbiamo volumi piccoli è meglio usare Gate-Array o FPGA. 

![[cost differences.png|center|500]]

Traducendo il grafico:
- se $n < N_{1}$ è conveniente usare il *gate-array*.
- se $N_{1} < n < N_{2}$ è conveniente usare lo *standard-cell*.
- se $n > N_{2}$ è conveniente usare il *full-custom*.
Il costo per unità non è l'unica metrica da considerare, può essere importante considerare parametri come velocità del circuito, dimensioni, consumi energetici. Nelle applicazioni con volumi medio/piccoli è conveniente mantenere i costi $NRE$ al minimo. 
Infine, i Full-Custom e Standard-Cell hanno un grande impatto sul *time-to-market*. 
# FPGA

**FPGA** è acronimo di "Field Programmable Gate Array" ed è costituito da un Gate Array con blocchi programmabili e un array di interconnessione che gli permettono di diventare una qualsiasi funzione logica. 
Ci sono due modi di programmarlo: 
- **SRAM based**: si inseriscono i dati nella RAM, è quindi riprogrammabile anche solo in parte. È però molto più sensibile al rumore (questo è quello che useremo).
- **Anti-Fuse**: si basa sulla connessione tra nodi a voltaggi differenti per creare una connessione permanente tra i due. Una volta programmato non è riprogrammabile.
L’anti-fuse è più efficiente in termini di velocità ma è meno compatibile con i dispositivi in commercio rispetto all’SRAM. 

Vediamo come questi due FPGA sono organizzati internamente. Le architetture più comuni sono:
- **Actel**, basata su 3 componenti:
	- Basic Logic Cell
	- I/O Pads 
	- Interconnection
- **Xilinx** che è molto più complessa quindi non la vedremo, ci basta sapere che è un’architettura più flessibile perché ci sono dei "Configurable Logic Block" (CLB).
Lo svantaggio di questi dispositivi è che non se ne possono mettere troppi in connessione in serie perché il ritardo $\tau _{TOT} = N^{2}\cdot \tau$ con $\tau = RC$ si deteriora quadraticamente. 
Ad esempio, il clock e il reset dovranno avere linee dedicate per evitare che ad alcuni componenti arrivino linee di segnale deteriorate. 
# Design Productivity

La **design productivity** è la capacità degli ingegneri di realizzare un certo numero di transistor un determinato tempo.
Ad esempio, se lavoriamo ad un livello di astrazione alto, si possono realizzare più transistor in meno tempo visto che il software tradurrà a basso livello. 
Vediamo come cambia il numero di gate a seconda del livello di astrazione:
- Transistor: 10 - 20 transistor
- Gate: 100 - 200 transistor
- RTL: 1k - 2k transistor
- Behavioral: 2k - 10k transistor
- Domain Specific: 8k -12k transistor
Inoltre ad esempio per costruire 10M transistor servirebbero:
- Transistor: 62.500 ingegneri
- Gate: 6250 ingegneri
- RTL: 625 ingegneri
- Behavioral: 125 ingegneri
- Domain Specific: 62 ingegneri 
Inoltre, la cosa difficile è organizzare il lavoro più aumenta il numero di persone che lavorano al processo di progettazione. 
Bisogna anche analizzare il mercato andandolo a targettizzare.