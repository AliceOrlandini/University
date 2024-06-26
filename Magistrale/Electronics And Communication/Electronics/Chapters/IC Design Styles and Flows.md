# Astrazione elettronica a livello di design

Prendiamo ad esempio un inverter, esistono 4 livelli di astrazione:
1. **RTL** (Register Transfer Level): rappresentazione tramite linguaggio id programmazione, in Verilog ad esempio, *out <= NOT(in)*
2. **Gate**: rappresentazione con il simbolo circuitale dell’inverter.
3. **Transistor**: rappresentazione tramite una combinazione di [[IC-Manufactory Review#Transistore MOSFE|MOSFET]] di tipo nmos o pmos.
4. **Lay-out**: rappresentazione della geometria e dei diversi livelli, spesso fatta tramite software.
Man mano che si scende nel livello di astrazione aumenta il tempo di realizzazione poiché si scende sempre più nei dettagli.
# Stili di design di circuiti integrati

Gli stili di design in generale sono:

![[design_approach.png|center|400]]

e la scelta dipende dalle specifiche esigenze, per esempio se si ha bisogno di una grande programmabilità si opteà per un micro-controller come l'intel Core i7, se invece si hanno dei requisiti più a livello di throughput si scenderà più a livello hardware. 
## Full Custom vs Semicustom

Nel caso della metodologia **Full Custom**, si ha la *massima libertà* di design in termini di dimensione, posizionamento e interconnessione di ogni singolo transistor. Ciò permette di raggiungere il massimo potenziale del circuito in termini di area occupata e di velocità. D'altro canto però abbiamo maggiori costi, tempi di design e sono richieste capacità specifiche da parte degli sviluppatori.
Nel **Semicustom** invece, c’è una riduzione nella libertà di design perché si hanno componenti pre-disegnati e l'unico aspetto che il designer può decidere riguarda le interconnessioni tra le strutture logiche messe a disposizione. Questo secondo metodo facilita notevolmente il processo di realizzazione in termini di tempo, di costi e di abilità richieste rispetto all'approccio full-custom. Tuttavia, in termini di ottimizzazione, questo tipo di circuito è più limitato.

![[IC Design Styles and Flows.png|center|400]]

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

![[cost curve.png|center|300]]

Domanda: quale tra i design che abbiamo visto ha un costo $NRC$ maggiore? Il maggiore sarà sicuramente il Full-Custom, mentre gli approcci Semi-Custom come Gate-Array o Standard-Cell hanno tempi di design molto simili. Mentre per quanto riguarda costi fissi $RE$ l'approccio Full-Custom e Standard-Cell sono molto simili perché vengono realizzati da zero.
Invece, per quanto riguarda il volume di produzione? Se abbiamo volumi grandi è conveniente usare il Full-Custom o lo Standar-Cell. Se invece abbiamo volumi piccoli è meglio usare Gate-Array o FPGA. 

![[cost differences.png|center|300]]

Traducendo il grafico:
- se $n < N_{1}$ è conveniente usare il *gate-array*.
- se $N_{1} < n < N_{2}$ è conveniente usare lo *standard-cell*.
- se $n > N_{2}$ è conveniente usare il *full-custom*.
Il costo per unità non è l'unica metrica da considerare, può essere importante considerare parametri come velocità del circuito, dimensioni, consumi energetici. Nelle applicazioni con volumi medio/piccoli è conveniente mantenere i costi $NRE$ al minimo. 
Infine, i Full-Custom e Standard-Cell hanno un grande impatto sul *time-to-market*. 
# Programmig Metrics

I due principali metodi di programmazione per implementare una $f(x)$ su un FPGA (acronimo di "Field Programmable Gate Array" ed è costituito da un Gate Array con blocchi programmabili e un array di interconnessione che gli permettono di diventare una qualsiasi funzione logica) sono:
- **anti-fuse programming**: si applicano dei voltaggi differenti sull'FPGA che creerà una connessione tra i nodi A e B, questo sistema è detto OTP (one time programming)
- **SRAM based**: si memorizza nella memoria una configurazione di bit che permetterà di implementare la funzione. 
L'antifuse è più performante in termini di velocità ma l'SRAM è più compatibile con i vari FPGA presenti sul mercato. 
Vediamo come questi FPGA sono organizzati internamente.

# FPGA Architecture

La prima architettura di un FPGA che andiamo a considerare è quella della *ACTEL*: 
![[fpga_arch.png|center|300]]

Gli **I/O Pads** sono predefiniti in base alla board che andiamo ad acquistare e sono programmabili per ricevere input oppure essere output. 
Il **Core** è fatto di row di celle con spazi per il routing channels. Queste celle sono uguali per tutti (quei quadratini in blu) e le connessioni vengono fatte negli spazi (quelle linee bianche).

La **basic logic cell** è composta da *8 input*, *due multiplexer* e *una OR gate*. Se si vogliono implementare funzioni più complesse è necessario mettere insieme più basic logic cell, ad esempio se volessimo fare un D-Flip-Flop avremmo bisogno di due basic logic cell. 

![[basic_logic_cell.webp|center|200]]

L'interconnessione è una matrice di fili per indirizzare il segnale e implementare le differenti interconnection function. 

![[interconnection.webp|center|300]]

Un'altra architettura che andiamo ad analizzare è quella della *Xilinx* (che è quella che andremo ad usare in laboratorio). Anche qui abbiamo gli I/O Pads e basic blocks chiamati *Configurable Logic Block* con usando una struttura base che è più complessa rispetto alla ACTEL. 
Le interconnessioni sono chiamate *Programmable Interconnecting Point* (PIP) e sono invece molto semplici: 

![[xilinx_inter.webp|center|400]]

C'è una memoria connessa ad un pass gate. In questo caso il delay non cresce linearmente ($\tau = N\cdot \tau = N \cdot R \cdot C$) ma in modo quadratico ($\tau = N^{2}\cdot R \cdot C$). Questo significa che non si possono avere interconnessioni troppo lunghe perché altrimenti si deteriorerebbe la velocità del device complessivo.

![[xilinx_inter_2.webp|center|300]]

Questo è di fatto un limite per i segnali che vanno in tutto il circuito come ad esempio il *clock* perché non arriverebbe contemporaneamente a tutti gli elementi del circuito ma avrebbe un delay. 
Un modo per risolvere questo problema sono le *direct interconnections*.

Non si può dire quale dei due approcci tra ACTEL e Xilinx sia il migliore perché dipende dal circuito infatti per capire qual è il migliore ci si base sui risultati di performance. 

# Design Productivity

La **design productivity** è una metrica che rappresenta la capacità degli ingegneri di realizzare un circuito complesso in un determinato tempo. Tradotto potrebbe essere visto come: "il numero di transistor realizzati in una settimana". 
Ad esempio, se lavoriamo ad un livello di astrazione alto (ad esempio scrivendo in VHDL), si possono realizzare più transistor in meno tempo visto che poi il software tradurrà a basso livello. Questo per dire che questa metrica è influenzata dal livello di astrazione e dai software utilizzati. 
*If you go outside and you want to cut a tree with a MOTOSEGA*. cit.Fanu.

Negli anni la design productivity si è alzata per il grande sviluppo dei tool. 
Vediamo come cambia il numero di gate a seconda del livello di astrazione in una settimana:
- Transistor: 10 - 20 transistor
- Gate: 100 - 200 transistor
- RTL: 1k - 2k transistor
- Behavioral: 2k - 10k transistor
- Domain Specific: 8k -12k transistor
Inoltre, ad esempio per costruire 10M transistor servirebbero:
- Transistor: 62.500 ingegneri
- Gate: 6250 ingegneri
- RTL: 625 ingegneri
- Behavioral: 125 ingegneri
- Domain Specific: 62 ingegneri 
Poi, la cosa difficile è organizzare il lavoro più aumenta il numero di persone che lavorano al processo di progettazione. 
Bisogna anche analizzare il mercato andandolo a *targettizzare* in modo da capire cosa produrre e quanto tempo dedicarci.
Se non si riesce a raggiungere il **time to market** si perde una fetta di mercato che si tramuta in una perdita di soldi. 