
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


# Interpretazione dell'OFDM

L'**Orthogonal Frequency Division Multiplexing (OFDM)** è una tecnica di modulazione che divide il flusso di dati ad alta velocità in flussi paralleli più lenti, ognuno dei quali modula una sottoportante diversa. Questa tecnica è efficace nel combattere l'**interferenza intersimbolica (ISI)** causata dal **frequency-selective fading** nei canali wireless.

## Generazione del Segnale OFDM nel Dominio del Tempo

Nel dominio del tempo, i campioni del segnale OFDM si ottengono applicando la **Trasformata di Fourier Inversa Discreta (IDFT)** ai simboli modulati in frequenza. Se $S(n)$ rappresenta il simbolo trasmesso sulla $n$-esima sottoportante, allora i campioni del segnale nel dominio del tempo sono dati da:
$$
s(k) = \frac{1}{\sqrt{N}} \sum_{n=0}^{N-1} S(n) e^{j 2\pi n \frac{k}{N}}
$$

per $k = 0, 1, \dots, N-1$, dove:
- $N$ è il numero totale di sottoportanti.
- $S(n)$ è il simbolo complesso modulato (ad esempio, QAM) sulla sottoportante $n$.
- $s(k)$ è il $k$-esimo campione nel dominio del tempo.

Possiamo anche scrivere:
$$
s(k) = \frac{1}{\sqrt{N}} \sum_{n=0}^{N-1} S_n(k)
$$

dove $S_n(k) = S(n) e^{j 2\pi n \frac{k}{N}}$ è il contributo del simbolo $S(n)$ al campione $s(k)$.

La **banda totale occupata** dal segnale OFDM è approssimativamente:
$$
B_s \approx \frac{1}{T_s}
$$

dove $T_s$ è la durata del simbolo su ciascuna sottoportante (anche chiamata durata del campione OFDM). Questo significa che:
$$
B_s \cdot T_s = 1
$$

La banda di ciascuna sottoportante è:
$$
\Delta f = \frac{B_s}{N} = \frac{1}{N T_s}
$$

Questo assicura che le sottoportanti siano ortogonali tra loro, poiché la separazione in frequenza $\Delta f$ è l'inverso della durata totale del simbolo OFDM $T = N T_s$.

### Forma d'Onda del Segnale OFDM

Il segnale associato alla $n$-esima sottoportante nel dominio del tempo è:
$$
s_n(t) = S(n) e^{j 2\pi n \Delta f t} \cdot \text{rect}\left(\frac{t}{T}\right)
$$

dove:
- $\Delta f = \frac{1}{T}$ è la separazione in frequenza tra le sottoportanti.
- $\text{rect}\left(\frac{t}{T}\right)$ è una funzione rettangolare di durata $T = N T_s$.

Il segnale totale è la somma dei segnali sulle singole sottoportanti:
$$
s(t) = \sum_{n=0}^{N-1} s_n(t)
$$

### Spettro del Segnale OFDM

Il segnale $s_n(t)$ ha uno spettro dato dalla convoluzione della trasformata di Fourier della funzione rettangolare (una sinc) e una delta centrata in $n \Delta f$:
$$
S_{s_n}(f) = S(n) T \cdot \text{sinc}\left[(f - n \Delta f) T\right]
$$

Le funzioni sinc associate alle diverse sottoportanti si sovrappongono in frequenza, ma grazie all'ortogonalità, non interferiscono tra loro nei punti di campionamento.

## Introduzione del Prefisso Ciclico (Cyclic Prefix)

Per combattere l'ISI causata dal **multipath**, si aggiunge un **prefisso ciclico** al segnale OFDM. Il prefisso ciclico è una copia degli ultimi $N_{\text{cp}}$ campioni del simbolo OFDM, aggiunti all'inizio del simbolo stesso:
- **Durata totale del simbolo trasmesso**: $T_{\text{tot}} = T + T_{\text{cp}}$, dove $T_{\text{cp}} = N_{\text{cp}} T_s$.
- **Scopo del prefisso ciclico**: Convertire la convoluzione lineare del canale in una convoluzione circolare, permettendo di utilizzare la trasformata di Fourier per la demodulazione senza interferenza tra simboli.

L'aggiunta del prefisso ciclico non modifica la periodicità del segnale in frequenza. Nel dominio del tempo, il segnale viene esteso, ma nel dominio della frequenza, questo si traduce in una leggera riduzione dell'efficienza spettrale a causa dell'overhead introdotto dal prefisso.

## Propagazione del Segnale attraverso il Canale

Quando il segnale OFDM si propaga attraverso un canale multipath con risposta impulsiva $h(l)$, l'output ricevuto $y(k)$ è dato da:
$$
y(k) = \sum_{l=0}^{L-1} h(l) s(k - l) + n(k)
$$

dove:
- $L$ è la lunghezza del canale (numero di campioni non nulli in $h(l)$).
- $n(k)$ è il rumore additivo.

Grazie al prefisso ciclico, la convoluzione lineare diventa una convoluzione circolare, e possiamo esprimere $y(k)$ nel dominio della frequenza.

### Trasformazione nel Dominio della Frequenza

Applichiamo la Trasformata di Fourier Discreta (DFT) all'output ricevuto:
$$
Y(n) = \sum_{k=0}^{N-1} y(k) e^{-j 2\pi n \frac{k}{N}}
$$
Sostituendo $y(k)$:
$$
Y(n) = H(n) S(n) + N(n)
$$

dove:
- $H(n)$ è la risposta in frequenza del canale sulla $n$-esima sottoportante.
- $N(n)$ è il rumore nel dominio della frequenza.

Questo mostra che ogni sottoportante può essere trattata indipendentemente, e l'ISI è eliminata.

## Ortogonalità delle Sottoportanti

L'ortogonalità delle sottoportanti è mantenuta grazie alla scelta della separazione in frequenza $\Delta f = \frac{1}{T}$ e all'uso del prefisso ciclico. Questo assicura che, nonostante la sovrapposizione spettrale, le sottoportanti non interferiscano tra loro.

### Dimostrazione dell'Ortogonalità

Consideriamo l'espressione per il contributo della $n$-esima sottoportante dopo il passaggio attraverso il canale:
$$
\begin{align*}
y_n(k) &= \sum_{l=0}^{L-1} h(l) S(n) e^{j 2\pi n \frac{(k - l)}{N}} \\
&= S(n) e^{j 2\pi n \frac{k}{N}} \left( \sum_{l=0}^{L-1} h(l) e^{-j 2\pi n \frac{l}{N}} \right)
\end{align*}
$$

L'ultimo termine dipende solo dal canale e dalla sottoportante $n$, e l'esponenziale $e^{j 2\pi n \frac{k}{N}}$ rappresenta una sinusoide complessa a frequenza $n \Delta f$.

### Importanza del Prefisso Ciclico

Se non ci fosse il prefisso ciclico, la convoluzione lineare con il canale introdurrebbe interferenza tra i simboli, rompendo l'ortogonalità delle sottoportanti. Il prefisso ciclico garantisce che la convoluzione sia circolare, preservando l'ortogonalità.

## Chiarimenti sui passaggi matematici
### Passaggio sull'Ortogonalità

Quando abbiamo:
$$
y_n(k) = \sum_{l=0}^{L-1} h(l) S(n) e^{j 2\pi n \frac{k - l}{N}}
$$
Possiamo riscriverlo come:

$$
\begin{align*}
y_n(k) &= S(n) e^{j 2\pi n \frac{k}{N}} \sum_{l=0}^{L-1} h(l) e^{-j 2\pi n \frac{l}{N}} \\
&= S(n) e^{j 2\pi n \frac{k}{N}} H(n)
\end{align*}
$$
dove:

$$
H(n) = \sum_{l=0}^{L-1} h(l) e^{-j 2\pi n \frac{l}{N}}
$$

Questo mostra che l'effetto del canale su ciascuna sottoportante è un semplice fattore moltiplicativo $H(n)$.

### Importanza del Ciclo

Il fatto che $k - l$ possa essere negativo non è un problema grazie al prefisso ciclico, che estende il segnale in modo che la convoluzione lineare possa essere trattata come circolare.

# WiFi - IEEE 802.11

La tecnologia Wi-Fi basata sullo standard **IEEE 802.11** utilizza l'**OFDM** (Orthogonal Frequency Division Multiplexing) come tecnica di modulazione per migliorare l'efficienza spettrale e la robustezza contro il fading selettivo in frequenza.

I parametri di base sono:
- **Banda del segnale $B_s$**: 20 MHz.
- **Frequenza portante $f_c$**:
  - **2.4 GHz** (per le bande 802.11b/g/n).
  - **5 GHz** (per le bande 802.11a/n/ac).
- **Numero totale di sottoportanti ($N$)**: 64.

La spaziatura in frequenza tra le sottoportanti ($\Delta f$) è data da:
$$
\Delta f = \frac{B_s}{N} = \frac{20 \times 10^6 \text{ Hz}}{64} = 312.5 \times 10^3 \text{ Hz} = 312.5 \text{ kHz}
$$

Questo significa che ogni sottoportante è separata in frequenza di 312.5 kHz dalla successiva.

## Sottoportanti Utilizzate

- **Sottoportanti totali**: 64.
- **Sottoportanti utilizzate per la trasmissione dei dati**: 52.
  - **48 sottoportanti** per i dati effettivi.
  - **4 sottoportanti** per i piloti (**pilot subcarriers**), utilizzati per la sincronizzazione e la stima del canale.
- **Sottoportanti nulle**: 12.
  - **DC Subcarrier**: Una sottoportante centrale (n° 0) è nulla per evitare problemi di leakage e per facilitare la conversione da banda base a banda passante.
  - **Guard Bands**: Le restanti 11 sottoportanti nulle sono posizionate ai bordi dello spettro OFDM per fungere da bande di guardia e ridurre l'interferenza con canali adiacenti.

## Motivo delle Sottoportanti Nulle

Le sottoportanti nulle sono utilizzate per:
1. **DC Subcarrier (Sottoportante a Frequenza Zero)**:
   - Evitare problemi dovuti a offset DC nel convertitore analogico-digitale (ADC).
   - Ridurre l'interferenza tra le componenti in banda base e quelle in banda passante.
2. **Guard Bands (Bande di Guardia)**:
   - Ridurre l'interferenza con canali adiacenti.
   - Permettere l'uso di filtri pratici con roll-off non ideale senza causare interferenza tra canali.

## Calcolo del Data Rate

Il **data rate** dipende dalla modulazione utilizzata e dal numero di bit per simbolo $n$.

Le modulazioni utilizzate sono:
- **BPSK**: $n = 1$ bit per simbolo.
- **QPSK**: $n = 2$ bit per simbolo.
- **16-QAM**: $n = 4$ bit per simbolo.
- **64-QAM**: $n = 6$ bit per simbolo.

### Esempio con 16-QAM

Supponiamo di utilizzare la modulazione **16-QAM**, dove $n = 4$ bit per simbolo:

- **Tempo utile del simbolo ($T_u$)**:
$$
  T_u = \frac{1}{\Delta f} = \frac{1}{312.5 \times 10^3 \text{ Hz}} = 3.2 \ \mu s
  $$
- **Cyclic Prefix ($T_{cp}$)**:
  - Tipicamente, il prefisso ciclico è il 25% del tempo utile:
$$
    T_{cp} = 0.25 \times T_u = 0.8 \ \mu s
    $$
- **Tempo totale del simbolo OFDM ($T_{\text{sym}}$)**:
$$
  T_{\text{sym}} = T_u + T_{cp} = 3.2 \ \mu s + 0.8 \ \mu s = 4 \ \mu s
  $$

#### Calcolo del Data Rate

- **Data rate per sottoportante**:
$$
  R_{\text{subcarrier}} = \frac{n}{T_{\text{sym}}} = \frac{4 \ \text{bit}}{4 \ \mu s} = 1 \ \text{Mbps}
  $$
- **Data rate totale** (considerando le 48 sottoportanti dati):
$$
  R = R_{\text{subcarrier}} \times \text{numero di sottoportanti dati} = 1 \ \text{Mbps} \times 48 = 48 \ \text{Mbps}
  $$

Questo calcolo mostra come si ottiene un data rate di **48 Mbps** utilizzando 16-QAM in un sistema Wi-Fi con $B_s = 20$ MHz.

## Calcolo del Data Rate per Diverse Modulazioni

### BPSK (n = 1)

- **Data rate per sottoportante**:
$$
  R_{\text{subcarrier}} = \frac{1}{4 \ \mu s} = 0.25 \ \text{Mbps}
  $$
- **Data rate totale**:
$$
  R = 0.25 \ \text{Mbps} \times 48 = 12 \ \text{Mbps}
  $$

### QPSK (n = 2)

- **Data rate per sottoportante**:
$$
  R_{\text{subcarrier}} = \frac{2}{4 \ \mu s} = 0.5 \ \text{Mbps}
  $$
- **Data rate totale**:
$$
  R = 0.5 \ \text{Mbps} \times 48 = 24 \ \text{Mbps}
  $$

## Percentuali e Overhead

### Efficienza Spettrale

- **Sottoportanti per i dati**: 48 su 64, cioè il **75%** delle sottoportanti totali.
- **Efficienza temporale**:
  - A causa del prefisso ciclico, l'efficienza temporale è:
$$
    \frac{T_u}{T_{\text{sym}}} = \frac{3.2 \ \mu s}{4 \ \mu s} = 0.8 \ (\text{80\%})
    $$
- **Efficienza complessiva**:
$$
  \text{Efficienza} = \frac{\text{sottoportanti dati}}{\text{sottoportanti totali}} \times \frac{T_u}{T_{\text{sym}}} = 0.75 \times 0.8 = 0.6 \ (\text{60\%})
  $$

Questo significa che l'efficienza spettrale complessiva è del **60%**, tenendo conto sia delle sottoportanti nulle che dell'overhead del prefisso ciclico.

## Note Aggiuntive

- **Modulazione Adattiva**: I sistemi Wi-Fi possono adattare la modulazione in base alle condizioni del canale. In ambienti con elevato rapporto segnale-rumore (SNR), possono utilizzare modulazioni ad alto ordine come 64-QAM per massimizzare il throughput. In condizioni di SNR basso, utilizzano modulazioni più robuste come BPSK o QPSK.
- **Codifica di Canale (FEC)**: Viene utilizzata la codifica convoluzionale o altri schemi di codifica per la correzione degli errori, che introducono un ulteriore overhead ma migliorano l'affidabilità della comunicazione.
- **Domande Sulle Percentuali**: Potrebbero essere poste domande sugli esami riguardo alle percentuali di efficienza, l'overhead introdotto dalle sottoportanti nulle e dal prefisso ciclico, e come questi influenzano il data rate.

## Riassunto dei Parametri Chiave

- **Banda del segnale ($B_s$)**: 20 MHz.
- **Frequenza portante ($f_c$)**:
  - 2.4 GHz o 5 GHz.
- **Numero totale di sottoportanti ($N$)**: 64.
- **Spaziatura tra le sottoportanti ($\Delta f$)**: 312.5 kHz.
- **Sottoportanti utilizzate per i dati**: 48.
- **Sottoportanti pilota**: 4.
- **Sottoportanti nulle**: 12 (DC Subcarrier + Guard Bands).
- **Tempo utile del simbolo ($T_u$)**: 3.2 μs.
- **Cyclic Prefix ($T_{cp}$)**: 0.8 μs.
- **Tempo totale del simbolo OFDM ($T_{\text{sym}}$)**: 4 μs.

# Probabilità di Errore dell'OFDM

La **probabilità di errore** in un sistema OFDM può essere considerata **uguale a quella di un sistema PAM** per ciascuna sottoportante, a condizione che il canale sia ideale o che gli effetti del fading e del rumore siano adeguatamente compensati.

La **probabilità di errore di simbolo** per una modulazione PAM su un canale AWGN è data da:
$$
P_e = 2 \left(1 - \frac{1}{M}\right) Q\left( \sqrt{\frac{6 \log_2 M}{M^2 - 1} \cdot \frac{E_b}{N_0}} \right)
$$

Dove:
- $M$ è l'ordine della modulazione (ad esempio, $M = 4$ per 4-PAM).
- $Q(\cdot)$ è la funzione Q di Gauss.
- $E_b/N_0$ è il rapporto tra l'energia per bit e la densità spettrale di rumore.

In un sistema OFDM, questa formula può essere applicata a ciascuna sottoportante, considerando l'SNR effettivo su quella sottoportante.


## Esempio Pratico

Supponiamo di utilizzare una modulazione 16-PAM su ciascuna sottoportante in un sistema OFDM. La probabilità di errore per ciascuna sottoportante sarà:
$$
P_e = 2 \left(1 - \frac{1}{16}\right) Q\left( \sqrt{\frac{6 \log_2 16}{16^2 - 1} \cdot \frac{E_b}{N_0}} \right)
$$

Questo calcolo è identico a quello che faremmo per un sistema 16-PAM su un canale AWGN.