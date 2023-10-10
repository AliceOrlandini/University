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

Vediamo i livelli di design per passare da un livello astratto a un livello concreto:
1. **Specifiche**: si definisce che cosa deve fare il circuito.
2. **Architettura**
3. **Modello Funzionale**
4. **Logica**
5. **Circuiti**
6. **Poligoni**: da inviare alla fabbrica per produrre il chip.

# Relazione costi performance

# FPGA

# Design Productivity

# Evoluzione dell’EAD industry

# Come mappare un algoritmo su un sistema on chip
