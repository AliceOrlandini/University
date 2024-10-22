
I **segnali multiportante** rappresentano una tecnologia fondamentale per affrontare il problema del **frequency-selective fading** nei canali wireless. Questa tecnologia è alla base di sistemi di comunicazione avanzati come **LTE**, **5G** e **Wi-Fi**, dove sono richieste elevate velocità di trasmissione, affidabilità e efficienza spettrale.

I principali motivi per cui i segnali multiportante sono impiegati in queste applicazioni sono:
1. **Robustezza**: I segnali multiportante sono intrinsecamente più resistenti al fading selettivo in frequenza e all'interferenza intersimbolica (ISI). Dividendo la banda totale in molte sottoportanti strette, ciascuna affetta da flat fading, è possibile mitigare gli effetti negativi del canale.
2. **Efficienza Spettrale**: L'uso di segnali multiportante consente di utilizzare la banda di frequenza disponibile in modo più efficiente. Tecniche come l'**OFDM** (Orthogonal Frequency Division Multiplexing) permettono una sovrapposizione spettrale delle sottoportanti senza interferenza interportante grazie all'ortogonalità, massimizzando l'uso dello spettro.
3. **Flessibilità nell'Allocazione delle Risorse (OFDMA)**: La tecnologia multiportante permette un'allocazione dinamica e flessibile delle risorse di frequenza e tempo tra diversi utenti, caratteristica fondamentale nell'**OFDMA** (Orthogonal Frequency Division Multiple Access). Questo è particolarmente utile in sistemi cellulari come LTE e 5G, dove la banda deve essere condivisa efficacemente tra molti utenti con esigenze diverse.

# OFDM technology

Quando si ha a che fare con il **frequency selective fading**, la **probabilità di errore** $P_e$ diminuisce con l'aumento del rapporto segnale-rumore (SNR) fino a raggiungere un **asintoto orizzontale**, noto come **error floor**. Questo significa che, nonostante l'aumento della potenza del segnale, l'errore non può essere ridotto oltre un certo limite a causa dell'interferenza intersimbolica (ISI) introdotta dal canale.

Il **frequency selective fading** si verifica quando la **banda del segnale** $B_s$ è maggiore della **coherence bandwidth** $B_c$ del canale:
$$
B_s > B_c \approx \frac{1}{5 \cdot \sigma_{\tau}}
$$

dove $\sigma_{\tau}$ è il **delay spread** del canale.

Per risolvere questo problema, si può dividere la banda del segnale originale in $N$ sottobande, ciascuna delle quali occupa meno banda della coherence bandwidth:
$$
\frac{B_s}{N} < B_c
$$

In questo modo, ogni sottobanda sperimenta **flat fading** anziché frequency selective fading, riducendo significativamente l'ISI e migliorando le prestazioni del sistema.

Una possibile implementazione di questa divisione è l'uso di **filtri passa banda** per separare il segnale in diverse sottobande, per poi sommare i segnali generati. Tuttavia, questo approccio presenta diverse limitazioni:
- **Complessità**: La progettazione e la realizzazione di filtri passa banda precisi per ciascuna sottobanda aumentano la complessità del sistema.
- **Interferenza tra Sottobande**: I filtri reali non possono essere perfettamente ortogonali, causando **interferenza tra sottobande** dovuta alle imperfezioni dei filtri.
- **Efficienza Spettrale Ridotta**: Per evitare l'interferenza tra sottobande, è necessario introdurre dei **guard band** tra le sottobande, riducendo l'efficienza spettrale.

Per superare queste limitazioni, si utilizza la tecnica dell'**Orthogonal Frequency Division Multiplexing (OFDM)**. L'OFDM permette di:
- **Suddividere la banda del segnale** in numerose **sottoportanti ortogonali** tra loro, senza la necessità di filtri fisici separati.
- **Mantenere l'ortogonalità** tra le sottoportanti utilizzando trasformate di Fourier discrete (DFT e IDFT), evitando l'interferenza interportante.
- **Ridurre l'ISI** aumentando la durata del simbolo su ciascuna sottoportante, dato che la velocità di simbolo è ridotta.

## Modello del Canale di Propagazione Multipath

Facciamo un passo indietro e analizziamo il **canale di propagazione multipath**. Il canale può essere modellato come la somma delle repliche del segnale trasmesso, ciascuna con attenuazione, fase e ritardo differenti.

La risposta Impulsiva del canale è:
$$
h(t) = A_{LS} \sum_{l=0}^{N_c - 1} \alpha_{l} e^{j\phi_{l}} \delta(t - \tau_{l})
$$

dove:
- $A_{LS}$ è l'attenuazione dovuta al **large scale fading**.
- $\alpha_{l}$ è l'attenuazione del percorso $l$.
- $\phi_{l}$ è la fase associata al percorso $l$.
- $\tau_{l}$ è il ritardo del percorso $l$.
- $N_c$ è il numero di percorsi multipli considerati.
- $\delta(\cdot)$ è la funzione delta di Dirac.

Applicando la trasformata di Fourier alla risposta impulsiva, otteniamo la **risposta in frequenza** del canale:
$$
H(f) = \sum_{l=0}^{N_c - 1} \alpha_{l} e^{j\phi_{l}} e^{-j2\pi f \tau_{l}}
$$

Il segnale ricevuto nel dominio della frequenza è dato da:
$$
Y(f) = H(f) \cdot S(f)
$$

dove $S(f)$ è la trasformata di Fourier del segnale trasmesso $s(t)$.

### Filtraggio del Segnale

Nel sistema di comunicazione, il segnale passa attraverso un filtro che limita la banda del segnale. La **banda del filtro** è approssimativamente:
$$
B_s \approx \frac{1}{T}
$$

dove $T$ è la durata del simbolo.

Poiché il filtro attenua le componenti al di fuori della sua banda passante, siamo interessati solo alla parte del segnale compresa in questa banda. Questo ci permette di semplificare l'analisi concentrandoci sul segnale filtrato.

Per semplificare ulteriormente l'analisi, si porta il segnale in **banda base**. La conversione in banda base permette di rappresentare il segnale come un segnale complesso a frequenza zero, facilitando il trattamento matematico.
Il **tempo di campionamento** necessario per rappresentare completamente il segnale in banda base è:
$$
T_s = T
$$

cioè, il segnale deve essere campionato almeno una volta ogni $T$ secondi per soddisfare il criterio di Nyquist.
Il canale in banda base equivalente può essere rappresentato come:
$$
h_{\text{eq}}(t) = \sum_{l=0}^{L - 1} h(l) \delta(t - lT)
$$

dove:
- $h(l)$ è il coefficiente del canale al tempo $lT$.
- $L$ è il numero di campioni del canale considerati.

Il segnale ricevuto in banda base è dato da:

$$
y(t) = \sum_{k} s(t - kT) h_{\text{eq}}(kT) + n(t)
$$
dove:
- $s(t)$ è il segnale trasmesso in banda base.
- $h_{\text{eq}}(kT)$ sono i coefficienti del canale campionati.
- $n(t)$ è il rumore additivo.

I vantaggi della rappresentazione in banda base sono:
- **Semplificazione del Modello**: Campionando il canale e il segnale a intervalli di $T$ secondi, si semplifica l'analisi e la simulazione del sistema.
- **Riduzione della Complessità Computazionale**: Lavorare in banda base permette di utilizzare segnali a bassa frequenza, riducendo la complessità del processamento digitale.
- **Facilità di Implementazione**: Molti algoritmi di elaborazione del segnale, come l'equalizzazione e la demodulazione, sono più facilmente implementabili in banda base.

## Analisi del Canale e della Trasmissione OFDM

Quando trasmettiamo un segnale attraverso un canale di comunicazione affetto da **multipath**, il segnale ricevuto è una convoluzione del segnale trasmesso con la risposta impulsiva del canale. Questa convoluzione introduce **interferenza intersimbolica** (ISI), che può degradare significativamente le prestazioni del sistema.

### Segnale Ricevuto e Convoluzione Discreta

Consideriamo un segnale discreto $s(k)$ composto da $N$ campioni, con $k$ che varia da 0 a $N - 1$. Il segnale ricevuto $y(k)$ può essere espresso come:
$$
y(k) = \sum_{l=0}^{L-1} h(l) s(k - l)
$$

dove:
- $h(l)$ è la risposta impulsiva discreta del canale, con $L$ campioni.
- $s(k - l)$ è il segnale trasmesso ritardato di $l$ campioni.

Questa equazione rappresenta la convoluzione discreta tra il segnale trasmesso e la risposta del canale.

Un esempio di di calcolo è:
- **Per $k = 0$:**
$$
  \begin{align*}
  y(0) &= h(0) s(0) + h(1) s(-1) + h(2) s(-2) + \dots + h(L-1) s(-(L-1)) \\
       &= h(0) s(0)
  \end{align*}
  $$

  Poiché i termini $s(-1), s(-2), \dots$ sono tutti zero (non esistono campioni precedenti a $s(0)$), l'espressione si semplifica.

- **Per $k = 1$:**
$$
  \begin{align*}
  y(1) &= h(0) s(1) + h(1) s(0) + h(2) s(-1) + \dots + h(L-1) s(-(L-2)) \\
       &= h(0) s(1) + h(1) s(0)
  \end{align*}
  $$

  Ancora una volta, i termini con indici negativi di $s$ sono zero.

Possiamo rappresentare l'operazione di convoluzione come una moltiplicazione matriciale:
$$
\mathbf{y} = \mathcal{H} \mathbf{s}
$$

dove:
- $\mathbf{y}$ è il vettore dei campioni ricevuti.
- $\mathbf{s}$ è il vettore dei campioni trasmessi.
- $\mathcal{H}$ è una matrice $N \times N$ **di Toeplitz**, costruita in modo tale che ogni diagonale parallela alla principale contiene lo stesso elemento $h(l)$.

**Esempio di Matrice di Toeplitz:**

$$
\mathcal{H} = \begin{bmatrix}
h(0) & 0 & \dots & 0 \\
h(1) & h(0) & \dots & 0 \\
\vdots & \ddots & \ddots & \vdots \\
h(L-1) & \dots & h(1) & h(0) \\
0 & \dots & h(L-1) & h(1) \\
\vdots & \ddots & \ddots & \vdots \\
0 & \dots & 0 & h(L-1)
\end{bmatrix}
$$

Notiamo che la matrice $\mathcal{H}$ è **non circolare**, poiché i termini fuori dai limiti (con indici negativi) vengono trattati come zero.

Per trasformare la matrice $\mathcal{H}$ in una **matrice circolante**, introduciamo un **prefisso ciclico** (Cyclic Prefix, CP) al segnale trasmesso.
La procedura è:
1. **Aggiunta del Prefisso Ciclico:**
   - Prendiamo gli ultimi $N_{\text{cp}}$ campioni del segnale $\mathbf{s}$ e li aggiungiamo all'inizio del vettore, creando un nuovo vettore $\overline{\mathbf{s}}$ di lunghezza $N + N_{\text{cp}}$.
   - Questo implica che i campioni con indici negativi sono definiti come $\overline{s}(-l) = s(N - l)$ per $l = 1, 2, \dots, L - 1$.

2. **Nuova Espressione del Segnale Ricevuto:**
   - Per $k = 0$:
$$
     \begin{align*}
     y(0) &= h(0) \overline{s}(0) + h(1) \overline{s}(-1) + \dots + h(L-1) \overline{s}(-L+1) \\
          &= h(0) s(0) + h(1) s(N - 1) + \dots + h(L-1) s(N - L + 1)
     \end{align*}
     $$

3. **Matrice Circolante $\overline{\mathcal{H}}$:**
   - La nuova matrice $\overline{\mathcal{H}}$ è ora **circolante**, poiché ogni riga è una versione ciclicamente shiftata della precedente.
   - Le proprietà delle matrici circolanti consentono una diagonalizzazione efficiente tramite trasformate di Fourier.

**Nota:** L'introduzione del prefisso ciclico comporta un **overhead** in termini di banda e potenza, poiché si trasmettono campioni aggiuntivi che non contengono nuove informazioni.

### Diagonalizzazione della Matrice Circolante

Le matrici circolanti possono essere diagonalizzate utilizzando la **Trasformata di Fourier Discreta (DFT)**.
Le sue proprietà sono:
- **Matrice di Fourier $F$:**
$$
  [F]_{k,n} = \frac{1}{\sqrt{N}} e^{-j \frac{2\pi k n}{N}}
  $$

  - $F$ è una matrice unitaria normalizzata, cioè $F^{H} F = I$.

- **Diagonalizzazione:**
$$
  \overline{\mathcal{H}} = F^{H} H F
  $$

  dove $H$ è una matrice diagonale i cui elementi sono le componenti spettrali della risposta del canale.

La diagonalizzazione della matrice del canale ha effetti benefici significativi:

- **Eliminazione dell'ISI:**
  - Dopo la trasformazione, l'operazione tra il segnale e il canale diventa una moltiplicazione elemento per elemento nel dominio della frequenza:
$$
    Y = F y = F \overline{\mathcal{H}} \mathbf{s} = F F^{H} H F \mathbf{s} = H S
    $$

    dove $S = F \mathbf{s}$ è la DFT del segnale trasmesso e $Y$ è la DFT del segnale ricevuto.
  - Poiché $H$ è diagonale, non c'è interferenza tra i simboli (niente ISI), e ogni sottoportante può essere trattata indipendentemente.

- **Semplificazione del Ricevitore:**
  - Il ricevitore può effettuare una semplice equalizzazione dividendo ogni componente di $Y$ per il corrispondente elemento di $H$.

### Schema a Blocchi del Sistema OFDM

Il sistema OFDM può essere rappresentato come segue:

1. **Trasmettitore:**
   - **Mappatura dei Simboli:**
     - I dati binari vengono mappati in simboli complessi utilizzando tecniche di modulazione digitale (es. QAM, PSK), seguendo la **mappatura di Gray** per minimizzare gli errori.
   - **Trasformata Inversa di Fourier (IFFT):**
     - I simboli vengono raggruppati in vettori di lunghezza $N$ e trasformati nel dominio del tempo tramite IFFT.
   - **Aggiunta del Prefisso Ciclico:**
     - Si aggiunge il prefisso ciclico per rendere il canale circolante.
2. **Canale:**
   - Il segnale attraversa il canale multipath, modellato dalla matrice circolante $\overline{\mathcal{H}}$, e viene aggiunto rumore $N(k)$.
3. **Ricevitore:**
   - **Rimozione del Prefisso Ciclico:**
     - Si elimina il prefisso ciclico per ripristinare il vettore originale di lunghezza $N$.
   - **Trasformata di Fourier (FFT):**
     - Si applica la FFT per trasformare il segnale nel dominio della frequenza.
   - **Equalizzazione:**
     - Si divide ogni componente del vettore ricevuto per il corrispondente elemento di $H$ per compensare l'effetto del canale.
   - **Demodulazione e Decodifica:**
     - Si demodulano i simboli ottenuti e si decodificano i dati binari.

### Vantaggi dell'OFDM

- **Riduzione dell'ISI:** Aumentando la durata del simbolo (grazie alla suddivisione in sottoportanti), il sistema diventa meno sensibile al delay spread del canale ($T > \sigma_{\tau}$).
- **Efficienza Spettrale:** Le sottoportanti sono ortogonali e possono sovrapporsi in frequenza senza interferire tra loro, ottimizzando l'uso dello spettro disponibile.

- **Semplicità di Equalizzazione:** L'equalizzazione si riduce a una semplice divisione nel dominio della frequenza, senza la necessità di complessi equalizzatori nel dominio del tempo.

### Considerazioni sul Tasso di Trasmissione

- **Tasso di Simbolo:** Ogni vettore di $N$ simboli viene trasmesso in $N T$ secondi, dove $T$ è la durata del simbolo su ciascuna sottoportante.

- **Vantaggio sul Delay Spread:** Poiché la durata del simbolo è aumentata, l'effetto del delay spread del canale è ridotto, minimizzando l'ISI.

### Utilizzo di Repliche per il Controllo degli Errori

Nel ricevitore, possiamo utilizzare repliche per il controllo degli errori?
Nel contesto dell'OFDM, l'uso di repliche del segnale per il controllo degli errori non è una pratica comune. Tuttavia, esistono tecniche per migliorare l'affidabilità del sistema:

1. **Codici di Correzione d'Errore (FEC):**
   - **Codici a Ridondanza:** Si aggiungono dati ridondanti al flusso di dati tramite codici come Reed-Solomon, Turbo Codes o LDPC, permettendo al ricevitore di rilevare e correggere errori senza la necessità di ritrasmissioni.

2. **Diversità in Frequenza:**
   - **Pilot Symbols:**
     - Si inseriscono simboli noti (piloti) all'interno delle sottoportanti per stimare e tracciare le variazioni del canale.
   - **Interleaving:**
     - I simboli vengono distribuiti su diverse sottoportanti e intervalli di tempo, in modo che gli errori causati dal fading selettivo siano resi più indipendenti e correggibili dai codici FEC.

3. **Diversità in Tempo e Spazio:**
   - **Trasmissioni Ripetute:**
     - In alcuni sistemi, si può decidere di trasmettere lo stesso simbolo più volte (repliche) in momenti diversi per combattere il fading rapido.
   - **MIMO (Multiple Input Multiple Output):**
     - L'utilizzo di più antenne trasmittenti e riceventi introduce diversità spaziale, migliorando la robustezza del sistema.


# OFDM interpretation

formule e grafici sulle slide.

La banda del singolo segnale sarà $\Delta f = \frac{B_{s}}{N}$.
23/11
Nel dominio del tempo, i samples del segnale sono: $$s = F^{H}S$$
generato tramite il CP insertion. 
Il sample $k$-esimo sarà: $$s(k) = \frac{1}{\sqrt{N}}\sum\limits_{n=0}^{N-1}S(n)e^{j2\pi n \frac{k}{N}} = \frac{1}{\sqrt{N}}\sum\limits_{n=0}^{N-1} S_{n}(k)$$
$B_{s}$ è la banda occupata dal segnale OFDM e si approssima con $B_{S}\approx \frac{1}{T_{s}}$ per cui $B_{s}\cdot T_{s} = 1$. Quindi, sostituendo nella formula posso scrivere: 
$$
\begin{align*}
s_{n}(k) &= S(n)e^{\frac{j2\pi nk}{N}} =\\
&= 
\end{align*}
$$
Grafico di cosa si vede nel tempo. 

Quando facciamo il *cycling prefix* si prende la prima parte e la si ritrasmette. In frequenza questa aggiunta non inficia sulla periodicità del segnale perché rimane una sinusoide. 

Quando il segnale si propaga nel canale avremo: $$y(k) = \sum\limits_{l=0}^{L-1}h(l)\cdot s(k-l) = \frac{1}{\sqrt{N}}\sum\limits Y_{n}(k)$$proprio perché essendo il canale lineare tempo invariante non cambia la frequenza. 
Formule su one note.

$k-l$ $= \sum\limits_{l=0}^{L-1}h(l)e^{\frac{j2\pi (k-l)}{N}}$ all'esponente possiamo scriverlo perché stiamo applicando un ciclo quindi non sarà mai negativo perché se $k-l$ fosse negativo e non ci fosse cycling prefix allora: $$y^{b}_{n}(k) = \sum\limits_{l=0}^{k}h(l)S^{b}(n)e^{j2\pi \frac{k-l}{N}}+\sum\limits_{l=k+1}^{L-1} h(l)S^{(b-1)}(n)e^{j2\pi \frac{k-l}{N}}$$perché è come se fossero due blocchi uniti. Questa è la motivazione per cui OFDM è ortogonale. 
Io davvero non sto capendo nulla, quando parte con le formule non gli sto dietro. 

Altre proprietà:
$$
\begin{align*}
y_{n}(k) &= \sum\limits_{l=0}^{L-1}h(l)S(n)e^{j2\pi n \frac{k-l}{N}} =\\
&= \sum\limits_{l=0}^{L-1}h(l)S(n)e^{j2\pi n \frac{k}{N}}e^{-j2\pi n \frac{l}{N}} =\\
&= e^{j2\pi n \frac{k}{N}} \sum\limits_{l=0}^{L-1}h(l)S(n)e^{-j2\pi n \frac{l}{N}}???
\end{align*} 
$$

Relazione tra il sampling time e la bandwidth.
$T = NT_{s}$
Per essere precisi nel campionamento bisogna che sia:
$s_{n}(t) = S(n)e^{j2\pi n \Delta f t} rect(\frac{t}{NT_{s}})$
è un processo stocastico perché $S(n)$ è una VA, poi c'è un prodotto quindi una convoluzione e infine l'esponenziale è una delta e la rect una sinc. Quindi il risultato è una sinc shiftata nel tempo: 
$S_{s_{n}} = A sinc^{2}((f-n\Delta f)NT_{s})(NT_{s})^{2}$
tutte le sinc delle repliche sono ortogonali quindi per ogni campionamento la sinc sarà zero (perché sono tutte sovrapposte)
La distanza tra due repliche sarà $\Delta f$ quindi la banda può essere approssimata sapendo che: boh non ho capito, ha fatto tutto un discorso e poi ha concluso che è pari a $B_{OFDM}\approx N\cdot \frac{1}{NT_{s}} = \frac{1}{T_{s}}$.
Da questa si ricava anche che $B_{s}= 1$. (?)

$T_{s}$ in OFDM system is $T$ in PAM system. (invio dei simboli)

## WiFi - IEEE 802.11

Lo chiede all'esame.
In questo WiFi si ha $B_{s} = 20$ MHz con $f_{c}= 2.4\cdot 10^{3}$ Hz oppure $f_{c}= 5\cdot 10^{3}$ Hz.
$N = 64$, se si fanno i calcoli si ottiene $\Delta f = \frac{20\cdot 10^{6}}{64} = 312.5\cdot 10^{3}$ Hz.

12 sono null subcarriers. vanno sacrificate per un motivo che non ho capito. oggi non è giornata via. 

domande varie sulle percentuali. 
calcolo del data rate: $R = B\cdot n$, in una Q-PAM $n=4$ e $R = 48$ Mb/s.

## Error Probability of OFDM

È uguale a quella della PAM.





