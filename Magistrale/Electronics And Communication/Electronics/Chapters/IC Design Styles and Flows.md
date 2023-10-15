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

![[IC Design Styles and Flows.png|center|700]]

Vediamo i livelli di design per passare da un livello astratto a un livello concreto:
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
Abbiamo due possibilità: Cell-based e Array-based. 
La processazione viene fatta dal core e ci sono una serie di piedini per l'input/output.
Il core nel gate array è organizzato in gate tutti uguali con i channels che interconnettono tra loro i gate. *At the end of the story* il core è composto da pmos ed nmos connessi opportunamente tra loro per implementare una certa funzione logica. La connessione viene fatta tramite materiali metallici.
Nella standard cell invece si hanno blocchi di dimensioni diverse ed è il programmatore che decide quale blocco utilizzare (??? boh sinceramente non ho capito nulla). 
Noi lavoreremo al Register Transfer Level, come avveniva nel Verilog a reti logiche abbiamo dei registri e all'arrivo del clock lo stato del sistema cambierà, dovremo quindi implementare una certa logica tramite descrizioni e successivamente sintesi.

# Relazione costi performance

# FPGA

# Design Productivity

# Evoluzione dell’EAD industry

# Come mappare un algoritmo su un sistema on chip
