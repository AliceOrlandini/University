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

La lezione di oggi parte male perché mi fa stra male la pancia e mi sono dimenticata a casa l’oki. Vabbè ci proviamo. 

Ora vediamo i passaggi di un design full custom:
consideriamo la realizzazione di un inverter quindi la specifica si hanno indicazioni riguardo l’area massima occupata, un tempo di propagazione massimo o un requisito di potenza consumata. Poi bisogna anche definire la tecnologia, ad esempio consideriamo un nmos a 5nm. 
Definiamo poi la topologia del device tramite un disegno, questa è un’operazione di sintesi.
Poi si calcola l’area dei transistor $W_{p}$ e $W_{n}$.
Ora si eseguono delle simulazioni sul funzionamento. 
Nella fase di layout si definisce ogni strato del dispositivo, a questo punto si conoscono esattamente le misure del dispositivo. Il DRC (“Design Rule Check”) è un software che si occuperà di verificare se le misure sono corrette.
Il LVS (“Layout vs Scheme”) controlla se il layout è uguale allo schema che avevamo fatto all’inizio. 
Infine c’è il LPE “Layout Parasitic Extraction” che è una simulazione che tiene in considerazione il modello dei transistor e il layout. 
Se qualsiasi cosa va storta si ritorna alle fasi precedenti. 
Questo è il full-custom flow che però noi non useremo. 
## ASIC semi-custom

Ora vediamo il semi-custom design style. 

# Relazione costi performance

# FPGA

# Design Productivity

# Evoluzione dell’EAD industry

# Come mappare un algoritmo su un sistema on chip
