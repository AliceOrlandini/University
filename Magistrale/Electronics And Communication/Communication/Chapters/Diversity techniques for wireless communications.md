Nel caso di un **canale a fading piatto** (*flat fading channel*), per ridurre il **Bit Error Rate** (**BER**) è spesso necessario aumentare significativamente la potenza del segnale trasmesso. Tuttavia, questo non è sempre praticabile, specialmente in dispositivi alimentati a batteria, come smartphone o sensori wireless, dove l'efficienza energetica è fondamentale.

Per migliorare l'efficienza e l'affidabilità della comunicazione senza aumentare la potenza, si utilizza una tecnica chiamata **diversity** (*diversità*). La diversità offre l'opportunità di aumentare la **affidabilità** (*reliability*) di un segnale trasmettendolo attraverso due o più canali di comunicazione con caratteristiche differenti. L'idea fondamentale è che, se uno dei canali è affetto da fading profondo (attenuazione severa), è probabile che gli altri canali non lo siano, permettendo al ricevitore di recuperare il segnale attraverso gli altri percorsi.

## Diversità in Frequenza

Un esempio di **diversità in frequenza** consiste nel trasmettere lo stesso segnale a una frequenza portante diversa $f_c'$, tale che la differenza tra le frequenze portanti sia maggiore della **coherence bandwidth** del canale $B_C$:
$$
|f_c' - f_c| > B_C
$$

In questo modo, i segnali trasmessi alle frequenze $f_c$ e $f_c'$ sperimentano fading indipendenti. Se si implementa questa strategia su quattro canali differenti, trasmettendo lo stesso segnale a quattro frequenze diverse, il ricevitore può **selezionare** il canale con l'attenuazione minore (questa tecnica è chiamata **selection diversity**).

Nonostante l'efficacia teorica, questa soluzione presenta delle **limitazioni pratiche**:
- **Occupazione di Banda**: Trasmettere lo stesso segnale su più frequenze richiede una banda totale maggiore. Nel caso di quattro canali, sarebbe necessaria una banda **quattro volte superiore** a quella del segnale originale.
- **Disponibilità Spettrale**: Spesso non è fattibile a causa delle restrizioni dello spettro di frequenze disponibili e delle regolamentazioni sulle assegnazioni di banda.

Per ovviare a queste limitazioni, sono state sviluppate diverse tecniche di diversità che non richiedono necessariamente una maggiore banda o potenza. Le più efficienti tra queste sono:
1. Diversità Temporale (**Time Diversity**)
- **Principio**: Sfrutta la **coherence time** del canale, che è l'intervallo di tempo durante il quale la risposta del canale può essere considerata costante.
- **Implementazione**: Il segnale viene trasmesso più volte in istanti di tempo separati da un intervallo maggiore della coherence time ($T_c$):
$$
  \Delta t > T_c
  $$
- **Vantaggio**: Ogni copia del segnale sperimenta condizioni di fading indipendenti, aumentando la probabilità che almeno una copia venga ricevuta correttamente.

2. Diversità in Frequenza (**Frequency Diversity**)
- **Principio**: Sfrutta la **coherence bandwidth** del canale, che è la banda di frequenze entro la quale la risposta del canale può essere considerata costante.
- **Implementazione**: Il segnale viene trasmesso su frequenze separate da un intervallo maggiore della coherence bandwidth ($B_C$):
$$
  \Delta f > B_C
  $$
- **Vantaggio**: Le copie del segnale trasmesse su frequenze diverse subiscono fading indipendenti, riducendo la probabilità che tutte le copie siano simultaneamente attenuate.

3. Diversità Spaziale (**Spatial Diversity**)
- **Principio**: Sfrutta la **coherence distance** del canale, che è la distanza oltre la quale le caratteristiche del fading diventano statisticamente indipendenti.
- **Implementazione**: Utilizza più antenne trasmittenti o riceventi separate spazialmente di una distanza superiore alla coherence distance ($d_c$):
$$
  \Delta d > d_c
  $$
- **Vantaggio**: Ciascuna antenna sperimenta fading indipendenti, permettendo al ricevitore di combinare i segnali per migliorare la qualità complessiva.

## Introduzione alla Teoria dei Codici

La **ridondanza** in un codice di correzione d'errore viene introdotta aggiungendo informazioni extra al messaggio originale. Questa ridondanza permette al ricevitore di rilevare e, in molti casi, correggere gli errori causati dal canale di comunicazione. La quantità di ridondanza è misurata tramite il **tasso del codice** ($R$), definito come:
$$
R = \frac{k}{n} < 1
$$

- $k$: numero di bit informativi in ingresso all'encoder.
- $n$: numero di bit totali in uscita dall'encoder (bit informativi + bit di ridondanza).

Un tasso del codice minore di 1 indica la presenza di ridondanza ($n > k$).

### Campi di Galois (GF)

Per comprendere e progettare codici di correzione d'errore, è essenziale avere familiarità con i **Campi di Galois** (**Galois Fields**), notati come $GF(q)$, dove $q$ è il numero di elementi del campo. Un campo di Galois è un insieme finito in cui sono definite due operazioni:

- **Addizione**.
- **Moltiplicazione**.

Entrambe le operazioni devono chiudersi all'interno del campo, ovvero il risultato di un'operazione tra due elementi del campo deve essere anch'esso un elemento del campo.

Ad esempio, il campo $GF(2)$ è composto da due elementi: $\{0, 1\}$. Le operazioni sono:
- **Addizione**: corrisponde all'operazione di **XOR**.
- **Moltiplicazione**: corrisponde all'operazione di **AND**.

Le tabelle di verità per queste operazioni sono:
**Addizione (XOR)**:

| $+$ | 0 | 1 |
|---------|---|---|
| **0**   | 0 | 1 |
| **1**   | 1 | 0 |

**Moltiplicazione (AND)**:

| $\times$ | 0 | 1 |
|--------------|---|---|
| **0**        | 0 | 0 |
| **1**        | 0 | 1 |

### Tipologie di Codici di Correzione d'Errore

Esistono principalmente due tipi di codici di correzione d'errore:
1. **Codici a Blocco**.
2. **Codici Convoluzionali**.

### 1. Codici a Blocco

Nei **codici a blocco**, il messaggio originale viene suddiviso in blocchi di $k$ bit, ai quali vengono aggiunti $n - k$ **bit di parità** o **bit di ridondanza** per formare un blocco codificato di $n$ bit.

#### Rappresentazione Matriciale

La codifica può essere espressa matematicamente come:
$$
\mathbf{d} = \mathbf{u} \cdot \mathbf{G}
$$

- $\mathbf{u}$: vettore dei bit informativi di lunghezza $k$.
- $\mathbf{G}$: matrice generatrice del codice di dimensione $k \times n$.
- $\mathbf{d}$: vettore codificato di lunghezza $n$.

#### Esempio: Codice di Parità Semplice

Un esempio comune è il **codice di controllo di parità** con tasso del codice $R = \frac{7}{8}$:
- $k = 7$ bit informativi.
- $n = 8$ bit codificati (7 bit informativi + 1 bit di parità).

**Procedura di Codifica**:
1. Calcolare il **bit di parità** come somma modulo 2 (XOR) dei 7 bit informativi.
2. Aggiungere il bit di parità al blocco per ottenere il vettore codificato.

**Rilevazione degli Errori al Ricevitore**:
- Il ricevitore calcola la parità del blocco ricevuto.
- Se la parità è **pari**, si assume che non ci siano errori.
- Se la parità è **dispari**, si rileva un errore.

**Limiti**:
- Non è possibile individuare quale bit sia errato.
- Se si verificano errori in un numero pari di bit, l'errore non viene rilevato.

### Applicazione nella Diversità Temporale

La diversità temporale può essere implementata trasmettendo nuovamente il messaggio quando viene rilevato un errore, come nel protocollo **ARQ** (**Automatic Repeat reQuest**). Per sfruttare efficacemente la diversità temporale:
- Il tempo tra le ritrasmissioni $T_{\text{ARQ}}$ deve essere maggiore del **tempo di coerenza** del canale $T_c$:
$$
  T_{\text{ARQ}} > T_c
  $$

In questo modo, le copie successive del messaggio attraversano condizioni di canale statisticamente indipendenti, aumentando la probabilità di una ricezione corretta.

### Capacità del Canale e Limiti Teorici

La **capacità del canale** $C$ definisce il massimo tasso al quale le informazioni possono essere trasmesse con una probabilità di errore arbitrariamente piccola:
$$
C = B \cdot \log_2(1 + \text{SNR}) \quad \text{[bit/s]}
$$

- $B$: banda del canale.
- $\text{SNR}$: rapporto segnale-rumore.

**Teorema di Shannon**:
- Se il tasso di trasmissione $R \leq C$, esiste un codice che permette di raggiungere una probabilità di errore $P_e$ arbitrariamente piccola.
- Se $R > C$, non è possibile trovare un codice che renda $P_e$ sufficientemente piccolo.

**Importanza**:
- Conoscere la capacità del canale è fondamentale per progettare sistemi di comunicazione efficienti.
- Permette di determinare il limite teorico delle prestazioni raggiungibili.

## Codici Convoluzionali

I **codici convoluzionali** introducono ridondanza distribuendo i bit di parità lungo la sequenza di dati, utilizzando una struttura a stati che tiene conto della storia dei bit trasmessi.

### Caratteristiche

- L'output al tempo $i$ dipende non solo dal bit di input corrente $u_i$, ma anche dai **precedenti $L - 1$ bit**.
- $L$: lunghezza del registro di memoria (profondità di memoria).
- **Stato**: rappresentato dai contenuti del registro di memoria.

### Encoder di un Codice Convoluzionale

- L'encoder esegue un'operazione di **convoluzione** in $GF(2)$ tra la sequenza di input e le funzioni di generazione.
- Genera $n$ bit di output per ogni $k$ bit di input, spesso con $k = 1$.

**Esempio**:
Un codice convoluzionale con parametri $(n, k, L) = (2, 1, 3)$:
- **Input**: sequenza di bit $u_i$.
- **Output**: per ogni bit di input, vengono generati 2 bit di output $d_i = [d_i^{(1)}, d_i^{(2)}]$.
- **Memoria**: il codice tiene traccia dei precedenti $L - 1 = 2$ bit.

Per rappresentare il codice si possono usare due diagrammi: 
1. **Diagramma a Stati**: mostra le transizioni tra gli stati dell'encoder in funzione dell'input.
2. **Diagramma di Trellis**: rappresenta l'evoluzione degli stati nel tempo, facilitando l'analisi delle possibili sequenze di stati e transizioni.

### Decodifica dei Codici Convoluzionali: Algoritmo di Viterbi

La decodifica dei codici convoluzionali mira a trovare la sequenza di bit trasmessi che più probabilmente ha generato la sequenza ricevuta. Questo si traduce nel trovare il **percorso più probabile** nel trellis.

#### Distanza di Hamming

- In $GF(2)$, la **distanza di Hamming** $d_H$ è il numero di bit in cui due sequenze differiscono.
- Obiettivo: minimizzare la distanza di Hamming tra la sequenza ricevuta $\mathbf{x}$ e le possibili sequenze codificate $\mathbf{d}$.

#### Branch Metric

- **Branch Metric** $\lambda_{s_{j-1} \rightarrow s_j}$: distanza di Hamming tra l'output previsto per una transizione di stato $s_{j-1} \rightarrow s_j$ e il segmento corrispondente della sequenza ricevuta.
  $$
  \lambda_{s_{j-1} \rightarrow s_j} = d_H\left(d_{s_{j-1} \rightarrow s_j}, x_j\right)
  $$

### Metriche Cumulative

- **Path Metric** $\Lambda_m$: somma cumulativa delle branch metric lungo un percorso fino allo stadio $m$:
  $$
  \Lambda_m = \sum_{j=1}^{m} \lambda_{s_{j-1} \rightarrow s_j}
  $$

### Algoritmo di Viterbi

L'**Algoritmo di Viterbi** è un metodo efficiente per trovare il percorso con la minima distanza nel trellis, evitando l'esplosione combinatoria dei percorsi possibili.

#### Principi Chiave

1. **Sovrapposizione dei Percorsi**:
   - Se due percorsi arrivano allo stesso stato allo stadio $m$, solo il percorso con la metrica cumulativa minore viene conservato.
   - Questo perché i percorsi futuri da quello stato condivideranno le stesse transizioni, e il percorso con metrica maggiore non potrà superare quello con metrica minore.
2. **Eliminazione dei Percorsi Subottimali**:
   - Riduce il numero di percorsi considerati, mantenendo il numero di percorsi proporzionale al numero di stati, evitando la crescita esponenziale.

#### Procedura dell'Algoritmo

1. **Inizializzazione**:
   - Impostare la metrica cumulativa iniziale per lo stato iniziale (spesso zero) e infinita per gli altri stati.

2. **Ricorsione**:
   - Per ogni stadio $m$:
     - Per ogni stato $s_j$:
       - Calcolare la metrica cumulativa per ciascun percorso che porta a $s_j$ dalle possibili transizioni.
       - Conservare solo il percorso con la metrica minima per $s_j$.
  
3. **Terminazione**:
   - Alla fine della sequenza, selezionare il percorso che termina con la metrica cumulativa minima.

4. **Backtracking**:
   - Risalire il trellis a ritroso seguendo le decisioni prese per ricostruire la sequenza di bit trasmessi.

I vantaggi sono:
- **Efficienza Computazionale**: Riduce drasticamente la complessità rispetto alla ricerca esaustiva.
- **Ottimalità**: Fornisce la soluzione esatta per la sequenza di stati più probabile.

# Interleaving

I **codici convoluzionali** sono altamente efficaci nel correggere errori quando gli errori sono **distribuiti uniformemente e incorrelati** lungo la sequenza di dati. Questo scenario è tipico di un **canale AWGN** (Additive White Gaussian Noise) senza memoria, dove gli errori avvengono in modo casuale e indipendente.

Tuttavia, nei canali wireless reali, specialmente in presenza di **flat fading** che varia nel tempo, gli errori tendono a verificarsi in **burst** (grappoli) durante i periodi di attenuazione del segnale. Ciò significa che gli errori non sono più indipendenti e uniformemente distribuiti, ma si concentrano in determinate porzioni della sequenza di dati. In queste condizioni, le prestazioni dei codici convoluzionali possono degradare significativamente, poiché l'algoritmo di decodifica (come l'algoritmo di Viterbi) potrebbe iniziare a seguire un percorso errato nel trellis, portando a errori di decodifica.

Per risolvere questo problema, si utilizza la tecnica dell'**interleaving**. L'interleaving ha lo scopo di **distribuire gli errori correlati** lungo la sequenza di dati, trasformando errori in burst in errori singoli e sparsi, che possono essere meglio gestiti dai codici di correzione d'errore.

Ecco come funziona l'Interleaving:
- **Interleaver**: Prima della trasmissione, i dati codificati vengono passati attraverso un interleaver che riorganizza la sequenza dei simboli in modo predeterminato o pseudo-casuale.
- **Deinterleaver**: Al ricevitore, un deinterleaver ripristina l'ordine originale dei simboli prima della decodifica.

**Schema a Blocchi**:

```
Trasmettitore:
Dati → Codificatore → Interleaver → Modulatore → Canale

Ricevitore:
Canale → Demodulatore → Deinterleaver → Decodificatore → Dati
```

### Effetto dell'Interleaving

- **Distribuzione degli Errori**: Se due bit erano adiacenti prima dell'interleaving, dopo l'interleaving saranno separati da una certa distanza nella sequenza trasmessa.
- **Correzione Migliorata**: Gli errori che si verificano in burst nel canale (ad esempio, a causa di un periodo di fading) vengono sparsi nel tempo dopo il deinterleaving, apparendo come errori singoli e non correlati al decodificatore.
- **Prestazioni dei Codici Convoluzionali**: Con errori distribuiti, i codici convoluzionali possono correggere efficacemente gli errori utilizzando l'algoritmo di Viterbi.

## Costi e Trade-off dell'Interleaving

### Ritardo (Delay)

- **Introduzione di Ritardo**: L'interleaving richiede che un blocco di dati venga memorizzato prima della trasmissione, il che introduce un ritardo proporzionale alla profondità dell'interleaver.  
- **Impatto su Applicazioni in Tempo Reale**: Per applicazioni sensibili al ritardo, come la trasmissione vocale o video in tempo reale, un ritardo elevato può essere inaccettabile.

### Dimensione dell'Interleaver

- **Profondità dell'Interleaver**: Una maggiore profondità dell'interleaver offre una migliore dispersione degli errori, ma aumenta il ritardo.
- **Compromesso**: È necessario trovare un equilibrio tra la necessità di disperdere gli errori e il ritardo massimo tollerabile dall'applicazione.

I tipi di interleaving sono due: 
- **Interleaving a Blocchi**: I dati vengono scritti in una matrice per righe e letti per colonne (o viceversa), permettendo una semplice implementazione.
- **Interleaving Convoluzionale**: Utilizza una serie di registri di ritardo con differenti latenze, offrendo una dispersione più complessa e flessibile.

In conclusione l'interleaving è una tecnica fondamentale per migliorare le prestazioni dei sistemi di comunicazione in presenza di canali affetti da errori correlati nel tempo, come nei casi di fading lento. Tuttavia, l'uso dell'interleaving introduce un compromesso tra **robustezza** e **ritardo**:

- **Quando Utilizzarlo**: In applicazioni dove l'integrità dei dati è essenziale e il ritardo è accettabile (es. trasferimento di file, messaggistica non in tempo reale).
- **Quando Evitarlo**: In applicazioni sensibili al ritardo, come la voce e il video in tempo reale, dove la latenza deve essere minimizzata.

È importante progettare il sistema di comunicazione tenendo conto delle esigenze specifiche dell'applicazione, bilanciando l'efficacia della correzione degli errori con i requisiti di ritardo e latenza.

# Turbo Codes e LDPC

I **Turbo Codes** e i **codici LDPC (Low-Density Parity-Check)** sono due classi di codici di correzione d'errore che hanno rivoluzionato le comunicazioni digitali, avvicinandosi notevolmente al limite teorico di capacità del canale stabilito da Claude Shannon nel 1948. Grazie alle loro eccezionali prestazioni in termini di **Bit Error Rate (BER)**, questi codici sono ampiamente utilizzati nei moderni sistemi di comunicazione, come le reti cellulari (3G, 4G, 5G), i sistemi satellitari e le reti Wi-Fi.

I **Turbo Codes** sono stati introdotti nel 1993 da Berrou, Glavieux e Thitimajshima. La loro innovazione principale risiede nell'uso di **codificatori convoluzionali paralleli** e di un **processo di decodifica iterativa** che sfrutta il **feedback positivo** per migliorare progressivamente la stima dei dati trasmessi.

**Analogia con il Motore Turbo**: L'analogia con un motore turbo è appropriata. In un motore turbo, i gas di scarico vengono riutilizzati per azionare una turbina che comprime l'aria in ingresso, migliorando l'efficienza del motore. Allo stesso modo, nei Turbo Codes, le informazioni già elaborate vengono riutilizzate nel processo di decodifica per migliorare la stima dei dati originali, attraverso un feedback positivo.

### Struttura del Codificatore Turbo

Un codificatore Turbo tipicamente ha un **tasso di codice $R = \frac{k}{n}$**, dove $k$ è il numero di bit informativi e $n$ è il numero di bit trasmessi. Nel caso comune di un tasso $R = \frac{1}{3}$, per ogni bit informativo vengono trasmessi tre bit.

**Componenti principali del codificatore Turbo**:
1. **Informazioni Sistematiche ($d^{(0)}$)**: Si trasmette il bit informativo originale senza alterazioni. Questo è il bit "sistematico".
2. **Prima Parità ($d^{(1)} = p_1$)**: Il bit informativo originale viene passato attraverso un **codificatore convoluzionale** per generare un bit di parità.
3. **Seconda Parità ($d^{(2)} = p_2$)**: Il bit informativo originale viene prima passato attraverso un **interleaver** (che riordina i bit in modo pseudo-casuale) e poi attraverso un **secondo codificatore convoluzionale** per generare un secondo bit di parità.

**Schema del Codificatore Turbo**:

```
            ┌───────────────┐
Input u ───►│               │───► d^(0) (sistematico)
            │               │
            │ Codificatore  │───► d^(1) (parità 1)
            │ Convoluzionale│
            │               │
            └───────────────┘
                   │
                   ▼
             Interleaver
                   │
                   ▼
            ┌───────────────┐
            │               │
            │ Codificatore  │───► d^(2) (parità 2)
            │ Convoluzionale│
            │               │
            └───────────────┘
```


L'**interleaver** è cruciale perché riordina i bit in ingresso al secondo codificatore convoluzionale, rendendo le due sequenze di parità ($p_1$ e $p_2$) **parzialmente indipendenti**. Ciò significa che gli errori che potrebbero verificarsi in una sequenza potrebbero non verificarsi nell'altra, aumentando la probabilità che almeno una delle parità fornisca informazioni utili per la correzione degli errori.

### Decodifica Iterativa con Feedback Positivo

Il **processo di decodifica** nei Turbo Codes è ciò che li rende così potenti. Utilizza una **decodifica iterativa** basata su un **feedback positivo**, che migliora progressivamente la stima dei dati trasmessi.

**Passaggi della Decodifica**:

1. **Primo Decoder**:
   - Riceve il bit sistematico $d^{(0)}$ e la prima parità $d^{(1)}$.
   - Applica un **algoritmo di decodifica soft**, come l'algoritmo MAP (Maximum A Posteriori) o l'algoritmo SOVA (Soft Output Viterbi Algorithm).
   - Produce una stima probabilistica (**informazione soft**) dei bit originali, chiamata **extrinsic information** o **reliability**.

2. **Interleaving della Stima**:
   - L'informazione soft prodotta dal primo decoder viene interleavata (riordinata) per allinearsi con l'ordine dei bit nel secondo decoder.

3. **Secondo Decoder**:
   - Riceve l'informazione soft interleavata e la seconda parità $d^{(2)}$.
   - Applica lo stesso algoritmo di decodifica soft.
   - Produce una nuova stima dei bit originali.

4. **Feedback Positivo**:
   - La nuova stima viene deinterleavata (riportata all'ordine originale) e utilizzata come input aggiuntivo nel primo decoder per la successiva iterazione.

5. **Iterazioni Multiple**:
   - Il processo viene ripetuto per un certo numero di iterazioni (tipicamente da 4 a 10), con ogni iterazione che migliora la stima dei bit originali.

**Schema del Processo di Decodifica**:

```
           ┌───────────────┐
           │               │
           │  Decoder 1    │───► Informazione soft
           │               │
           └───────────────┘
                   │
                   ▼
             Interleaver
                   │
                   ▼
           ┌───────────────┐
           │               │
           │  Decoder 2    │───► Nuova informazione soft
           │               │
           └───────────────┘
                   │
                   ▼
             Deinterleaver
                   │
                   └───────────────► Feedback al Decoder 1
```

### Perché Funziona il Feedback Positivo?

Ogni decoder migliora la stima dei bit basandosi non solo sulle parità ricevute, ma anche sull'informazione proveniente dall'altro decoder. Questo scambio di informazioni permette ai decoder di convergere progressivamente verso la sequenza di bit corretta.

**Effetto del Feedback Positivo**:
- **Miglioramento Progressivo**: Ogni iterazione aumenta la **affidabilità** (reliability) della stima dei bit.
- **Riduzione degli Errori**: Gli errori residui diminuiscono con il numero di iterazioni, avvicinando le prestazioni al limite teorico di Shannon.

### Prestazioni dei Turbo Codes

I Turbo Codes hanno dimostrato di poter raggiungere prestazioni molto vicine al **limite di Shannon**, ovvero il massimo tasso di trasmissione teorico senza errori per un dato canale.

**Grafico della BER**:
- Il grafico tipico mostra la BER in funzione dell'SNR (Signal-to-Noise Ratio).
- Le curve relative ai Turbo Codes mostrano una **rapida diminuzione** della BER con l'aumentare dell'SNR, specialmente dopo poche iterazioni.
- Confrontando con la 2-PAM (modulazione senza codifica), si nota come i Turbo Codes offrano un miglioramento significativo delle prestazioni.

I vantaggi dei Turbo Codes sono:
- **Elevata Efficienza**: Si avvicinano al limite di Shannon, consentendo comunicazioni affidabili a tassi di trasmissione elevati.
- **Flessibilità**: Possono essere progettati con diversi tassi di codice e lunghezze, adattandosi a varie applicazioni.
- **Applicazioni Pratiche**: Utilizzati in molteplici standard di comunicazione, come 3G, 4G, DVB-RCS, e sistemi satellitari.

# LDPC (Low-Density Parity-Check Codes)

I **codici LDPC** furono introdotti da Robert Gallager nel 1962, ma rimasero in gran parte sconosciuti fino agli anni '90, quando furono riscoperti grazie alla crescente potenza di calcolo disponibile. Come i Turbo Codes, gli LDPC sono capaci di avvicinarsi al limite di Shannon utilizzando tecniche di decodifica iterativa.

La truttura di questi codici è:
- **Matrice di Parità Sparsa**: Gli LDPC sono definiti da una **matrice di parità** a bassa densità (pochi 1 rispetto agli 0), che descrive le relazioni tra i bit di informazione e i bit di parità.
- **Grafi Bipartiti**: La struttura del codice può essere rappresentata mediante un **grafo fattoriale** (o grafo di Tanner), con due tipi di nodi:
  - **Nodi Variabile**: Rappresentano i bit di dati.
  - **Nodi Controllo**: Rappresentano le equazioni di parità.

**Esempio di Matrice di Parità**:

Una matrice $H$ di dimensioni $m \times n$, dove $n$ è il numero totale di bit e $m$ il numero di equazioni di parità, con una densità di 1 molto bassa.

### Decodifica Iterativa negli LDPC

La decodifica degli LDPC avviene attraverso un **algoritmo di passaggio di messaggi** (message-passing), tipicamente l'**algoritmo di Belief Propagation** o l'**algoritmo Sum-Product**.

**Processo di Decodifica**:
1. **Inizializzazione**:
   - Ogni nodo variabile inizia con una stima iniziale basata sul segnale ricevuto.

2. **Passaggio di Messaggi**:
   - I nodi variabile e i nodi controllo scambiano informazioni sulle probabilità dei bit.
   - Ogni nodo controllo calcola una stima della probabilità che la sua equazione di parità sia soddisfatta, basandosi sulle informazioni ricevute dai nodi variabile.
   - Ogni nodo variabile aggiorna la propria stima basandosi sulle informazioni provenienti dai nodi controllo.

3. **Iterazioni**:
   - Il processo viene ripetuto per un numero fissato di iterazioni o fino a quando le equazioni di parità sono soddisfatte.

I vantaggi degli LDPC sono:
- **Prestazioni Vicine al Limite di Shannon**: Come i Turbo Codes, anche gli LDPC possono avvicinarsi molto al limite teorico di capacità del canale.
- **Scalabilità**: Possono essere progettati con lunghezze molto elevate, migliorando le prestazioni.
- **Efficienza di Decodifica**: Gli algoritmi di decodifica sono altamente paralleli, rendendo gli LDPC adatti per implementazioni hardware efficienti.

### Applicazioni degli LDPC

- **Standard Wi-Fi (802.11n/ac/ax)**: Utilizzati per migliorare l'efficienza delle comunicazioni wireless.
- **Standard DVB-S2 (Digital Video Broadcasting - Satellite - Second Generation)**: Per la trasmissione satellitare di segnali video ad alta definizione.
- **Sistemi di Comunicazione di Massa**: Come nelle reti cellulari 5G.

## Confronto tra Turbo Codes e LDPC

### Similitudini

- **Decodifica Iterativa**: Entrambi utilizzano processi di decodifica iterativa per migliorare progressivamente la stima dei dati.
- **Prestazioni Elevate**: Capaci di avvicinarsi al limite di Shannon, offrendo prestazioni eccezionali in termini di BER.
- **Utilizzo in Standard Moderni**: Entrambi sono integrati in vari standard di comunicazione.

### Differenze

1. **Struttura del Codice**:
  - **Turbo Codes**: Basati su codificatori convoluzionali paralleli e interleaving.
  - **LDPC**: Basati su matrici di parità sparse e grafi bipartiti.
2. **Implementazione Hardware**:
  - **Turbo Codes**: Richiedono decoder più complessi, con processi iterativi meno adatti alla parallelizzazione.
  - **LDPC**: Decodifica altamente parallela, rendendo più semplice l'implementazione in hardware ad alte prestazioni.
3. **Prestazioni su Diversi Canali**:
  - **Turbo Codes**: Prestazioni migliori su canali con rumore Gaussiano bianco additivo.
  - **LDPC**: Prestazioni superiori su canali con rumore impulsivo o interferenze.


