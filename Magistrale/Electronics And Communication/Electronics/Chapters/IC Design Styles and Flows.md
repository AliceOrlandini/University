# Astrazione elettronica a livello di design

Prendiamo ad esempio un inverter, esistono 4 livelli di astrazione:
1. **RTL** (Register Transfer Level): rappresentazione tramite linguaggio id programmazione, in Verilog ad esempio, *out <= NOT(in)*
2. **Gate**: rappresentazione con il simbolo circuitale dell’inverter.
3. **Transistor**: rappresentazione tramite una combinazione di [[IC-Manufactory Review#Transistore MOSFE|MOSFET]] di tipo nmos o pmos.
4. **Lay-out**: rappresentazione della geometria e dei diversi livelli, spesso fatta tramite software.
In queste rappresentazioni vado sempre più ad aggiungere dettagli quindi il processo di design richiede più tempo mano mano che si scende di livello di astrazione.
# Stili di design di circuiti integrati

Questo è lo schema dei diversi approcci nel design dei circuiti integrati: 

Il nostro approccio sarà Semicustom, Array Based e Pre-wired (FPGA) ma comunque andremo a studiarli tutti.

## Full Custom vs Semicustom

Nel caso Full Custom caso si ha la massima libertà di design riguarda alla dimensione, posizionamento e interconnessione di ogni singolo transistor.
Nel Semicustom invece c’è una riduzione nella libertà di design perché ci sono componenti pre-realizzati ma questo facilita notevolmente il processo di realizzazione sia in termini di tempo che di costi. 
# Metodi di design e flows

Per implementare dei circuiti digitali è possibile usare due tipi di approccio: *full-custom* o *semi-custom*. 
In linea generale, ciò che li caratterizza è: 
- Full-Custom: massima libertà di design in termini di dimensionamento, posizionamento e interconnessioni. Ciò permette di raggiungere il massimo potenziale del circuito in termini di area occupata e di velocità. D'altro canto però abbiamo maggiori costi, tempi di design e sono richieste capacità specifiche da parte degli sviluppatori.
- Semi-Custom: libertà limitata perché si usano circuiti logici pre-disegnati. Infatti, in questo caso vanno disegnate solo le interconnessioni tra le strutture logiche. Ciò comporta un minor costo, tempo di sviluppo e abilità inferiori rispetto al full-custom. Tuttavia, in termini di ottimizzazione, questo tipo di circuito è limitato.
![[IC Design Styles and Flows.png|center|700]]

I seguenti sono i livelli di design per passare da un livello astratto a un livello concreto:
1. **Specifiche**: si definisce che cosa deve fare il circuito.
2. **Architettura**
3. **Modello Funzionale**
4. **Logica**
5. **Circuiti**
6. **Poligoni**: da inviare alla fabbrica per produrre il chip.

## Full-custom 

Vediamo i passaggi di un design full custom consideriando la realizzazione di un inverter.
Nella fase di *specifica* si hanno indicazioni riguardo l’area massima occupata, il tempo di propagazione massimo o la potenza consumata. Poi bisogna anche definire la tecnologia, ad esempio consideriamo una tecnologia CMOS a 5nm. 
Definiamo poi la *topologia* del device a partire dalle tabelle di verità, attenzione che questa è un’operazione di sintesi.
Poi si calcola l’area dei transistor $W_{p}$ e $W_{n}$.
A questo punto si eseguono delle *simulazioni* sul funzionamento con ad esempio software come SPICE. 
Nella fase di *layout* si definisce ogni strato del dispositivo, a questo punto si conoscono esattamente le sue misure. Il DRC (“Design Rule Check”) è il software che si occuperà di verificare se le misure sono corrette.
Il LVS (“Layout vs Scheme”) controlla se il layout è uguale allo schema che avevamo fatto all’inizio. 
Infine, c’è l'LPE (“Layout Parasitic Extraction”) che è una simulazione che tiene in considerazione il modello dei transistor e il layout. 
Se qualsiasi cosa va storta si ritorna alle fasi precedenti. 
Tutti questi software si ottengono tramite licenza che ha un costo nell'ordine dei milioni di euro all'anno. Inoltre, gli sviluppatori devono avere abilità specifiche per utilizzarli. 
Per questi motivi, il full-custom flow noi non useremo ma era bene sapere come funziona. 
## ASIC semi-custom

Ora vediamo il semi-custom design style. 
L’azienda produttrice fornisce una serie di elementi da loro fabbricati che si possono assemblare per creare ciò di cui abbiamo bisogno. 
Abbiamo due possibilità: **Cell-based** e **Array-based (Gate-Array)**. 

Nel caso di gate array abbiamo una matrice fissa di gate tutti uguali con un certo numero di transistori quindi il designer si limita a stabilire le interconnessioni tra i gate della matrice. Le interconnessioni sono metalliche e posizionate in canali opposti (tendenzialmente viene usato meno del 50% dei transistori).
*At the end of the story* il cuore del gate array è composto da pmos ed nmos connessi opportunamente tra loro per implementare una certa funzione logica.

Nell'approccio standard cell invece si hanno celle di altezza fissa ma larghezza variabile. La dimensione dei canali di interconnessione non sono fisse ed inoltre le posizioni dei piedini di IO sono variabili. Quindi in questo caso si ha più libertà di design che comporta una maggior ottimizzazione dell'area e della velocità. Di contro invece si hanno maggiori costi in termini di tempo di sviluppo e di abilità. 

La tecnologia **FPGA** ha un'architettura di tipo *gate array* che però rispetto a questi ha molta più potenzialità in quanto può essere programmato sia dal punto di vista logico che dal punto di vista delle interconnessioni 

Noi lavoreremo al Register Transfer Level, come avveniva nel Verilog a reti logiche abbiamo dei registri e all'arrivo del clock lo stato del sistema cambierà, dovremo quindi implementare una certa logica tramite descrizioni dell'hardware.
Il **TapeOut** è la fase finale in cui si mandano i poligoni all'azienda produttrice che li usa per realizzare le maschere per il processo di produzione dei transistor. Le maschere sono uno dei costi più importanti da considerare. 
# Relazione costi performance

Vediamo ora i costi di un circuito integrato che hanno un'importanza fondamentale sulla scelta dello stile di desing. I costi si dividono in:
- **Costi fissi** $RE$: sono costi che si sostengono indipendentemente dal numero di pezzi realizzati. Esempi sono: 
	- Costi di design: licenze software, salario ingegneri che usano quei software
	- La macchina che produce le maschere e le macchine per i test
- **Non recurrent engineering cost** $NRE$: costi dei materiali e della manodopera che richiede la produzione di un singolo pezzo. Esempi sono:
	- Costo del Wafer: questo costo si riduce più l'area del chip diminuisce ($cost \propto A^{3}$).
Il costo per singola unità sarà quindi $C_{1}= \frac{NRE}{n} + RE$ mentre il costo totale sarà $C_{T}= NRE + (n \cdot RE)$.
La relazione tra $n$ e $C_{1}$ è di tipo iperbolico, nel senso che per $n$ piccoli il costo $C_{1}$ aumenta esponenzialmente, all'aumentare di $n$ la curva di attenua fino all'asintoto $RE$.

![[cost curve.png|center|500]]

Domanda: quale tra i design che abbiamo visto ha un costo $NRC$ maggiore? Il maggiore sarà il Full Custom, poi viene il Gate Array e infine lo Standard Cell.
Invece, per quanto riguarda il volume di produzione? Conviene se abbiamo volumi grandi usare il Full Custom o lo Standar Cell. Se invece abbiamo volumi piccoli è meglio usare Gate Array o addirittura FPGA. 

![[cost differences.png|center|500]]

# FPGA

**FPGA** è acronimo di "Field Programmable Gate Array" ed è costituito da un Gate Array con blocchi programmabili per diventare una qualsiasi funzione logica e un array di interconnessione. 
Ci sono due modi di programmarlo: 
- **SRAM based**: si inseriscono i dati nella ram, è riprogrammabile anche solo in parte. È però molto più sensibile al rumore (questo è quello che useremo)
- **Anti-Fuse**: si basa sulla connessione tra nodi a voltaggi differenti per creare una connessione permanente tra i due. Una volta programmato non è riprogrammabile.
L’anti-fuse è più efficiente in termini di velocità ma è meno compatibile con i dispositivi in commercio rispetto all’sram. 

Vediamo come questi due FPGA sono organizzati internamente:
La prima architettura è la Actel, basata su 3 componenti:
- Basic Logic Cell
- I/O Pads 
- Interconnection

La seconda opzione è l’architettura Xilinx che è molto più complessa quindi non la vedremo, ci basta sapere che può essere sia input che output ed è un’architettura più flessibile perché ci sono dei Configurable Logic Block (CLB).
Lo svantaggio di questi dispositivi è che non se ne possono mettere troppi in connessione in serie perché il ritardo $\tau _{TOT} = N^{2}\tau$ si deteriora quadraticamente. 
Ad esempio, il clock e il reset dovranno avere linee dedicate perché altrimenti il segnale di deteriorerebbe. 
# Design Productivity

La design productivity è la capacità degli ingegneri di realizzare un certo numero di transistor un tempo preciso.
Ad esempio se lavoriamo ad un livello di astrazione alto si possono realizzare più transistor in meno tempo visto che il software tradurrà a basso livello. 
Vediamo come cambia il numero di gate a seconda del livello di astrazione:
- Transistor: 10 - 20 transistor
- Gate: 100 - 200 transistor
- RTL: 1k - 2k transistor
- Behavioral: 2k - 10k transistor
- Domain Specific: 8k -12k transistor
Inoltre ad esempio per costruire 10M transistor servirebbero:
Transistor: 62.500 ingegneri
- Gate: 6250 ingegneri
- RTL: 625 ingegneri
- Behavioral: 125 ingegneri
- Domain Specific: 62 ingegneri 
Inoltre la cosa difficile è organizzare il lavoro più aumenta il numero di persone che lavorano al processo di progettazione. 

Bisogna anche analizzare il mercato andandolo a targettizzare.
Esempio della playstation che ritarda di 6 mesi e quindi la gente per natale al figliolo compra l’xbox.



