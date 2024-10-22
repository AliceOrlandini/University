## Tipi di propagazione basati sulla frequenza

A seconda della carrier frequency cambia il tipo di propagazione:
- **Ground wave (onde di terra)**: $f_{c}<2\text{ MHz}$, il segnale si propaga seguendo la superficie terrestre, che agisce come un conduttore. È usata principalmente in ambito militare e può coprire lunghe distanze, anche centinaia di chilometri. Tuttavia, siccome 2 MHz è una frequenza relativamente bassa, le applicazioni pratiche sono limitate a contesti specifici.
- **Sky wave (onde ionosferiche)**: $f_{c} \approx 10 \text{ MHz},$ le onde radio colpiscono la ionosfera e rimbalzano verso la terra, permettendo al segnale di coprire grandi distanze. Tuttavia, la ionosfera è influenzata dal plasma e, quindi, dalla radiazione solare, che può alterare la propagazione del segnale in momenti diversi della giornata o dell’anno.
- **Space wave (onde spaziali)**: $f_{c} > 30 \text{ MHz}$, qui il segnale viaggia nell’aria senza rimbalzare sulla superficie terrestre o sulla ionosfera. Il principale limite è che il segnale ha un raggio d'azione più ristretto, poiché è necessario che non ci siano ostacoli tra trasmettitore e ricevitore per una propagazione efficace.

## Fenomeni che influenzano la propagazione del segnale

- **Rifrazione**: Avviene quando un’onda elettromagnetica colpisce una superficie che ha dimensioni simili alla lunghezza d'onda del segnale, modificando la direzione di propagazione.
- **Diffrazione**: Si verifica quando il segnale incontra un ostacolo con bordi, che piega il segnale oltre l'ostacolo, permettendone la propagazione anche in presenza di barriere fisiche.
- **Scattering (scattering diffuso)**: Accade quando il segnale colpisce tanti piccoli ostacoli, come le foglie di un albero, e il segnale viene sparpagliato in più direzioni.

Studiare la propagazione del segnale in un ambiente è complesso, perché ogni situazione richiede la considerazione di molte variabili, anche se il trasmettitore e il ricevitore sono identici.
Un metodo dettagliato per studiare la propagazione è il _Ray Tracing_, che simula il percorso preciso delle onde. Tuttavia, è utilizzato solo in contesti particolari a causa della sua complessità.
Spesso si preferiscono modelli semplificati che adottano approcci probabilistici per descrivere la propagazione del segnale, rendendo più gestibile lo studio dell’ambiente.

## Large Scale Fading

Il **large scale fading** è un modello che serve a descrivere come un segnale radio si indebolisce (o viene attenuato) quando viaggia dal trasmettitore al ricevitore, tenendo in considerazione fenomeni su larga scala come la distanza e gli ostacoli tra i due dispositivi.

I due modelli principali sono:
1. **Path-loss (Perdita di percorso)**: Descrive come la potenza del segnale diminuisce quando il segnale si allontana dal trasmettitore. È legato principalmente alla distanza.
2. **Shadowing (Ombreggiamento)**: Tiene conto degli ostacoli che il segnale incontra lungo il suo percorso, come edifici, colline o altre strutture, che fanno "ombra" al segnale, riducendo la potenza che arriva al ricevitore.

Quando combiniamo questi due modelli, otteniamo un’idea generale della media della potenza del segnale ricevuto.

### Propagazione sferica del segnale

Immagina il segnale come una luce che si propaga in tutte le direzioni a partire dall’antenna del trasmettitore, formando una **sfera** che si espande con il crescere della distanza. Man mano che la sfera si allarga, la potenza del segnale si distribuisce su una superficie sempre più grande, il che significa che la potenza percepita a una certa distanza diminuisce.

La superficie di una sfera è data dalla formula $S = 4\pi r^{2}$ dove:
- $r$ è il *raggio* della sfera, cioè la distanza dal trasmettitore.
- $S$ rappresenta *l’area* su cui si distribuisce la potenza del segnale.

La **potenza ricevuta** dal segnale $P_{RX}$ dipende dalla **potenza trasmessa** $P_{TX}$ e dalla **distanza** $d$ tra il trasmettitore e il ricevitore. Dato che il segnale si distribuisce su una superficie sferica, più grande è la superficie (cioè più lontano è il ricevitore), meno potenza arriverà.

La potenza ricevuta si calcola con la seguente formula:
$$P_{RX} = P_{TX}\cdot \frac{1}{S}\cdot A$$
dove:
- $P_{TX}$ è la potenza trasmessa.
- $S = 4\pi r^{2}$ è la superficie della sfera $d$.
- $A = (\frac{\lambda}{2})^{2}\cdot \frac{1}{\pi}$ rappresenta l’area efficace dell’antenna ricevente (una misura di quanto segnale può raccogliere l'antenna).

Sostituendo l'espressione di $A$ e di $S$, otteniamo:
$$P_{RX}= P_{TX}\cdot \frac{1}{4\pi d^{2}} \frac{\lambda^{2}}{4}\cdot \frac{1}{\pi} = P_{TX}\cdot \left({\frac{\lambda}{4\pi d}}\right)^{2}$$
Questa espressione esprime quanto la potenza si riduce in base alla distanza $d$.

La lunghezza d'onda $\lambda$ è la distanza tra due creste successive dell’onda elettromagnetica. Si può calcolare con la formula $\lambda = \cfrac{c}{f_{0}}$, dove $c$ è la *velocità della luce* (circa $3 \cdot 10^{8}$ metri al secondo) e $f_{0}$ è la *frequenza del segnale*.
Se $f_{0}$​ è ad esempio 3 GHz (una frequenza tipica per comunicazioni wireless), la lunghezza d'onda sarà: 
$$
\lambda = \cfrac{3\cdot 10^{8}}{3 \cdot 10^{9}} = 0.1 \text{ metri}
$$

L'attenuazione del segnale è la riduzione della potenza del segnale mentre viaggia. Nell'esempio, si parla di **un'attenuazione** di 60 dB.
Per avere un'idea di cosa significhi: un’attenuazione di 60 dB corrisponde a una riduzione della potenza di un fattore di un milione (1 milione di volte più debole).

In parole semplici, quando un segnale radio viene trasmesso, *la sua potenza diminuisce* man mano che si allontana dal trasmettitore, sia per via della distanza sia a causa degli ostacoli che può incontrare lungo il percorso. Usare formule come quelle descritte sopra permette di stimare quanto segnale rimarrà una volta che raggiunge il ricevitore.

## Path Loss (perdita di percorso)

Il **path loss** descrive quanto la potenza di un segnale diminuisce con la distanza quando si propaga attraverso uno spazio libero.

La formula per il path loss nel caso della propagazione in spazio libero è:
$$\Gamma(f_{0},d_{0}) \approx \left(\frac{\lambda}{4\pi d_{0}}\right)^{2}$$
Questa equazione mostra che il path loss dipende dalla frequenza portante $f_{0}$​, tramite la lunghezza d'onda $\lambda$, e dalla distanza $d_{0}$ che è una distanza di riferimento tra il trasmettitore e il ricevitore. $\lambda$ è legato alla frequenza attraverso $\lambda = \cfrac{c}{f_{0}}$ dove $c$ è la velocità della luce. 

Un altro termine importante nel calcolo del path loss è:
$$\left(\frac{d_{0}}{d}\right)^{n}$$dove $n > 2$ è chiamato **path-loss exponent** e descrive quanto velocemente la potenza del segnale diminuisce in funzione della distanza $d$. Più alto è $n$, più velocemente il segnale perde potenza con l'aumentare della distanza.

### Rapporto di potenza tra trasmettitore e ricevitore

Il path loss è spesso espresso come il **rapporto** tra la **potenza trasmessa** e la **potenza ricevuta**:
$$A_{PL}= \frac{P_{Tx}}{P_{Rx}}$$
Tabella sulle slide.
Ci sono due principali termini legati all’ambiente che influenzano il path loss:
1. **La frequenza portante $f_{0}$**: Se la frequenza aumenta, anche l’attenuazione del canale aumenta. In altre parole, a frequenze più alte, il segnale si attenua più velocemente.
2. **La distanza $d$**: Maggiore è la distanza tra trasmettitore e ricevitore, maggiore sarà l’attenuazione.

Man mano che la frequenza del segnale aumenta, aumenta anche l’attenuazione. Questo significa che *a frequenze più alte*, *il segnale oscilla di più*, e ci vuole più tempo per raggiungere il ricevitore a causa di una maggiore attenuazione lungo il percorso.

Intorno ai 60 GHz, esiste una "finestra" in cui l’attenuazione diventa particolarmente elevata. Questo è dovuto alla presenza di *molecole d'acqua* e ossigeno nell’atmosfera, che assorbono il segnale, causando una forte attenuazione. In pratica, queste frequenze agiscono come dei "filtri" naturali, limitando l'efficacia del segnale in queste bande.

## Shadowing

Immaginiamo di avere una cella con due ricevitori posti alla stessa distanza dal trasmettitore. Teoricamente, dato che $d_{A}= d_{B}$​, ci aspetteremmo che l'attenuazione del segnale sia identica per entrambi i ricevitori. In realtà, non è così a causa dello **shadowing**, che si verifica in presenza di ostacoli specifici e produce effetti importanti sulla propagazione del segnale.

Lo shadowing è descritto come una variabile aleatoria Gaussiana misurata in dB, con una **distribuzione log-normale**.

L'equazione per la potenza ricevuta è:
$$P_{Rx}[dBm] = P_{Tx}[dBm] + A_{PL}[dB] + A_{S}[dB]+ A_{SS}[dB]$$
con:
- $A_{PL}$ un valore deterministico che dipende dalla distanza $d$.
- $A_{S}$ una variabile aleatoria con distribuzione log-normale.
- $A_{SS}$ rappresenta l'attenuazione dovuta al small scale fading.

## Small Scale Fading

Per descrivere l'effetto dello **small scale fading**, è necessario considerare lo schema a blocchi del sistema di comunicazione. Lo small scale fading rappresenta la risposta all'impulso $h(t)$ del canale. Mentre il **large scale fading** era modellato come un fattore moltiplicativo che attenua il segnale complessivamente, lo small scale fading è descritto come un *sistema lineare tempo-invariante* (LTI) che altera il segnale attraverso una convoluzione.

L'uscita del canale è data da:
$$
y(t) = s(t) \circledast h(t)
$$
dove $s(t)$ è il segnale trasmesso.

Lo small scale fading è causato dal fenomeno del **multipath**: il segnale trasmesso arriva al ricevitore attraverso molteplici percorsi, ognuno dei quali introduce un ritardo e un'attenuazione differenti dovuti alle riflessioni, diffrazioni e scattering con gli ostacoli nell'ambiente. Il segnale ricevuto è la somma di queste repliche, che possono interferire tra loro e causare **interferenza intersimbolica** (ISI).

Le caratteristiche principali di ciascun percorso multiplo sono:
- **Attenuazione $a_{l}$**: dipende dal percorso specifico e può variare nel tempo. È una variabile aleatoria a causa della natura imprevedibile dell'ambiente.
- **Ritardo $\tau_{l}$**: dato da  $\tau_{l} = \cfrac{d_{l}}{c}$, dove  $d_{l}$ è la lunghezza del percorso $l$ e $c$ è la velocità della luce. Anche se le distanze possono variare, per piccole variazioni il ritardo può essere considerato quasi deterministico.
- **Fase $\phi_{l}$**: variazione di fase introdotta dal percorso, anch'essa una variabile aleatoria.

La risposta del canale può quindi essere espressa come:
$$
h(t) = \sum_{l=0}^{N_{c}-1} a_{l} e^{j\phi_{l}} \delta(t - \tau_{l})
$$
e il segnale ricevuto diventa:
$$
y(t) = A_{LS} \sum_{l=0}^{N_{c}-1} a_{l} e^{j\phi_{l}} s(t - \tau_{l})
$$
dove $A_{LS}$ rappresenta l'attenuazione dovuta al large scale fading e $N_{c}$ è il numero totale di percorsi multipli considerati.

Il tempo intercorso tra l'arrivo delle diverse repliche è chiamato **dispersione temporale** (*Time Dispersion*).

Consideriamo il segnale trasmesso come una sequenza di simboli modulati:

$$
x(t) = \sum_{i} c_{i} g(t - iT)
$$
dove:
- $c_{i}$: simboli trasmessi.
- $g(t)$: forma d'onda del simbolo.
- $T$: periodo del simbolo.

La forma d'onda complessiva al ricevitore, tenendo conto del canale, è:
$$
g(t) = g_{T}(t) \circledast h(t) \circledast g_{R}(t) = g_{N}(t) \circledast h(t)
$$
dove:

- $g_{T}(t)$: risposta del filtro trasmettitore.
- $g_{R}(t)$: risposta del filtro ricevitore.
- $g_{N}(t)$: forma d'onda complessiva del sistema trasmettitore-ricevitore senza canale.

Espandendo $g(t)$ tenendo conto dei percorsi multipli, otteniamo:
$$
g(t) = \sum_{l=0}^{N_{c}-1} a_{l} e^{j\phi_{l}} g_{N}(t - \tau_{l})
$$
Il segnale ricevuto campionato all'istante $t = mT$ è:
$$
x(m) = x(t)\big|_{t = mT} = \sum_{k} c_{m - k} g(kT)
$$
Dove:

$$
g(kT) = g(t)\big|_{t = kT} = \sum_{l=0}^{N_{c}-1} a_{l} e^{j\phi_{l}} g_{N}(kT - \tau_{l})
$$
Sostituendo, otteniamo:

$$
x(m) = c_{m} g(0) + \sum_{k \neq 0} c_{m - k} g(kT)
$$

Il termine $c_{m} g(0)$ rappresenta il contributo del simbolo desiderato, mentre la somma $\sum_{k \neq 0} c_{m - k} g(kT)$ rappresenta l'interferenza intersimbolica causata dai simboli adiacenti e dai ritardi introdotti dal canale.

Poiché i ritardi $\tau_{l}$ non sono controllabili e variano in modo aleatorio, l'ISI è inevitabile. Dobbiamo quindi chiederci quanta interferenza intersimbolica abbiamo nel nostro sistema e come essa influisce sulle prestazioni del sistema di comunicazione. Possiamo mitigare l'effetto dell'ISI attraverso l'uso di equalizzatori o tecniche di modulazione appropriate.
## Delay Spread

Il **Delay Spread** è un parametro fondamentale che misura la dispersione temporale del canale e si indica con $\sigma_{\tau}$. Rappresenta la deviazione standard dei ritardi dei diversi percorsi multipli ponderati in base alla loro potenza. In altre parole, quantifica quanto il segnale si "allarga" nel tempo a causa dei ritardi introdotti dai vari percorsi che il segnale può prendere a causa di riflessioni, diffrazioni e scattering.

È preferibile che la dispersione temporale sia **piccola**, perché una grande dispersione implica che le repliche del segnale non arrivano al ricevitore nello stesso istante. Questo può causare **interferenza intersimbolica** (ISI), in quanto i simboli trasmessi possono sovrapporsi temporalmente, rendendo difficile per il ricevitore distinguere tra simboli successivi. Una dispersione temporale ridotta minimizza l'ISI e migliora la qualità della comunicazione.

Nel caso ideale in cui c'è solo una replica del segnale (cioè, un canale senza multipath), la funzione di trasferimento del canale è data da:
$$
H(f) = \alpha_{0} e^{j\phi_{0}}
$$

Dove:
- $\alpha_{0}$ è l'attenuazione del segnale lungo il percorso diretto.
- $\phi_{0}$ è la fase associata al percorso diretto.

Prendendo il modulo della funzione di trasferimento, otteniamo:
$$
|H(f)| = \alpha_{0}
$$

Questo indica che, in assenza di multipath e quindi di dispersione temporale, il canale si comporta come un semplice fattore di attenuazione e fase costante. Non introduce distorsioni o ritardi variabili con la frequenza, e il segnale mantiene la sua forma originale al ricevitore (a parte un'attenuazione e un possibile shift di fase).

In presenza di multipath, tuttavia, la funzione di trasferimento del canale diventa più complessa:
$$
H(f) = \sum_{l=0}^{N_{c}-1} a_{l} e^{j(\phi_{l} - 2\pi f \tau_{l})}
$$

Dove:
- $N_{c}$ è il numero di percorsi multipli considerati.
- $a_{l}$ è l'attenuazione del percorso $l$.
- $\phi_{l}$ è la fase iniziale del percorso $l$.
- $\tau_{l}$ è il ritardo del percorso $l$.
- $f$ è la frequenza.

La presenza di $\tau_{l}$ nel termine esponenziale indica che il canale introduce una distorsione di fase che varia con la frequenza, causando dispersione del segnale nel dominio temporale. Questo effetto può degradare significativamente le prestazioni del sistema se non viene adeguatamente compensato.

Per mitigare gli effetti del delay spread, si possono adottare diverse tecniche:
- **Equalizzazione**: utilizzo di filtri al ricevitore per invertire parzialmente gli effetti del canale.
- **Spread Spectrum**: tecniche come CDMA che distribuiscono il segnale su una banda larga per renderlo meno sensibile al multipath.
- **Utilizzo di simboli più lunghi**: aumentando la durata del simbolo, si riduce l'effetto relativo della dispersione temporale.

In sintesi, il delay spread è un parametro critico nella progettazione di sistemi di comunicazione wireless, poiché influisce direttamente sulla quantità di ISI e, di conseguenza, sulla qualità e affidabilità della trasmissione.

## Coherence Bandwidth

La **Coherence Bandwidth** $B_{c}$ del canale è l'intervallo di frequenze entro il quale la risposta in frequenza del canale $H(f)$ (e di conseguenza la sua densità spettrale di potenza $S_{h}(f)$) può essere considerata costante. In altre parole, è la banda di frequenza su cui il canale non introduce distorsioni significative al segnale trasmesso.

La Coherence Bandwidth è *inversamente proporzionale al delay spread* $\sigma_{\tau}$ del canale. Una relazione approssimativa comunemente utilizzata è:
$$
B_{c} \approx \frac{1}{5\sigma_{\tau}}
$$

Ovvero:
- Se $\sigma_{\tau}$ è **piccolo**, allora $B_{c}$ è **grande**, il che significa che il canale ha una risposta in frequenza piatta su una larga banda.
- Se $\sigma_{\tau}$ è **grande**, allora $B_{c}$ è **piccolo**, indicando che il canale è selettivo in frequenza.

Si può effettuare una classificazione del canale in base a $\sigma_{\tau}$ e $B_{c}$:
- **Caso 1:** Se $\sigma_{\tau} \ll T$ (dove $T$ è la durata del simbolo), allora $B_{c} > B_{s}$ (con $B_{s}$ banda del segnale) e il canale è detto **flat fading**. In questo scenario, il canale non introduce significative distorsioni in frequenza, e l'interferenza intersimbolica (ISI) è minima. In questo caso avremo meno ISI, poiché le repliche del segnale arrivano quasi simultaneamente.
- **Caso 2:** Se $\sigma_{\tau} > T$, allora $B_{c} < B_{s}$ e il canale è **frequency-selective** (multipath). In questo caso avremo **più ISI**, poiché le repliche del segnale arrivano in tempi diversi, causando sovrapposizione tra simboli consecutivi.

Il delay spread è difficile da calcolare esattamente poiché dipende dalle specifiche condizioni del canale, come gli ostacoli e l'ambiente circostante. Pertanto, spesso ci si concentra sul calcolo del suo valore medio anziché sulla distribuzione completa (PDF).
Iniziamo con alcuni valori: 
- **Valore medio del ritardo:**
$$
  E[\tau] \approx \sum_{l=0}^{L-1} p_{l} \tau_{l} = \sum_{l=0}^{L-1} \left( \frac{\alpha_{l}^{2}}{\sum_{i=0}^{L-1} \alpha_{i}^{2}} \right) \tau_{l}
  $$

  Dove:

  - $p_{l}$ è la probabilità associata al percorso $l$.
  - $\alpha_{l}$ è l'ampiezza (attenuazione) del percorso $l$.
  - $\tau_{l}$ è il ritardo del percorso $l$.
  - $L$ è il numero totale di percorsi multipli considerati.

- **Valore medio del quadrato del ritardo:**
$$
  E[\tau^{2}] = \sum_{l=0}^{L-1} p_{l} \tau_{l}^{2}
  $$

- **Calcolo del delay spread:**
$$
  \sigma_{\tau} = \sqrt{E[\tau^{2}] - \left( E[\tau] \right)^{2}}
  $$

Tabella riassuntiva:

| Condizione                   | Relazione             | Tipo di Canale             | Effetto sull'ISI     |
|------------------------------|-----------------------|----------------------------|----------------------|
| $\sigma_{\tau} \ll T$        | $B_{c} > B_{s}$       | Flat Fading                | ISI minima           |
| $\sigma_{\tau} > T$          | $B_{c} < B_{s}$       | Frequency-Selective Fading | ISI significativa    |

Nota:
- **Flat Fading:** Il canale ha una risposta in frequenza piatta su tutta la banda del segnale. Non introduce distorsioni frequenza-dipendenti, quindi il segnale subisce solo un'attenuazione e uno shift di fase.
- **Frequency-Selective Fading:** Il canale introduce distorsioni che variano con la frequenza all'interno della banda del segnale, causando dispersione temporale e ISI.


## Esempio

Consideriamo un esempio per illustrare l'effetto dei ritardi nei percorsi multipli e l'utilizzo degli equalizzatori nel sistema di comunicazione.
Le probabilità associate ai due percorsi multipli sono calcolate come:
$$
p_{0} = \frac{\alpha_{0}^{2}}{\alpha_{0}^{2} + \alpha_{1}^{2}}
$$
$$
p_{1} = \frac{\alpha_{1}^{2}}{\alpha_{0}^{2} + \alpha_{1}^{2}}
$$

dove:
- $\alpha_{0}$ e $\alpha_{1}$ sono le attenuazioni (ampiezze) dei due percorsi.
- $p_{0}$ e $p_{1}$ rappresentano le frazioni di potenza ricevuta attraverso ciascun percorso.

Analizziamo quattro casi distinti in base al ritardo $\tau$ del secondo percorso rispetto al primo:
1. **Caso 1: $\tau = 0.1T$**
   - **Situazione:** Il ritardo del secondo percorso è molto piccolo rispetto alla durata del simbolo $T$.
   - **Effetto:** **Non si ha interferenza intersimbolica (ISI)** significativa.
   - **Motivazione:** Le repliche del segnale arrivano quasi simultaneamente, quindi l'effetto del multipath è trascurabile sul simbolo corrente e non disturba i simboli successivi.
   - **Azioni Necessarie:** Non è necessario l'uso di un equalizzatore; il sistema può operare correttamente senza interventi aggiuntivi.
2. **Caso 2: $\tau = T$**
   - **Situazione:** Il ritardo del secondo percorso è uguale alla durata del simbolo.
   - **Effetto:** **Si ha molta interferenza intersimbolica**.
   - **Motivazione:** La replica ritardata del simbolo precedente arriva contemporaneamente al simbolo successivo, causando sovrapposizione e distorsione.
   - **Azioni Necessarie:** È possibile utilizzare un **equalizzatore** per compensare l'ISI, correggendo le distorsioni introdotte dal canale e recuperando il segnale originale.
3. **Caso 3: $\tau = 1.5T$**
   - **Situazione:** Il ritardo del secondo percorso è maggiore della durata del simbolo, ma non eccessivamente.
   - **Effetto:** **Si ha ancora interferenza intersimbolica**, poiché la replica ritardata interferisce con il simbolo successivo.
   - **Motivazione:** L'ISI è presente e può degradare le prestazioni del sistema.
   - **Azioni Necessarie:** Anche in questo scenario, l'uso di un **equalizzatore** può aiutare a mitigare gli effetti dell'ISI e migliorare la qualità della ricezione.
4. **Caso 4: $\tau = 4T$**
   - **Situazione:** Il ritardo del secondo percorso è molto maggiore della durata del simbolo.
   - **Effetto:** **Si ha interferenza intersimbolica significativa** che coinvolge più simboli successivi.
   - **Motivazione:** La replica ritardata del segnale interferisce con diversi simboli futuri, rendendo l'ISI troppo estesa per essere gestita efficacemente.
   - **Azioni Necessarie:** In questo caso, **non si può usare un equalizzatore** tradizionale per correggere l'ISI. Il ritardo è troppo grande, e l'equalizzatore non è in grado di compensare interferenze che si estendono su molti simboli.

In conclusione:
- **Equalizzatori:** Sono efficaci quando il ritardo dei percorsi multipli è comparabile o leggermente superiore alla durata del simbolo $T$. Possono correggere l'ISI introdotta dal canale e migliorare le prestazioni del sistema.
- **Limiti degli Equalizzatori:** Quando il ritardo è molto grande ($\tau \gg T$), come nel caso 4, l'ISI coinvolge troppi simboli e l'equalizzatore non è più efficace. In tali situazioni, è necessario adottare altre strategie, come:
  - **Tecniche di diversità temporale o spaziale.**
  - **Modulazioni resistenti all'ISI (es. OFDM).**
  - **Inserimento di intervalli di guardia tra i simboli.**

## Flat Fading Channel: BER in AWGN

In questa sezione analizziamo la **Bit Error Rate (BER)** di un canale affetto da **Additive White Gaussian Noise (AWGN)**, considerando sia il caso ideale sia le complicazioni introdotte dal fading e dal multipath.

### Canale AWGN Ideale

Un canale AWGN ideale si verifica quando il ricevitore e il trasmettitore sono molto vicini o quando esiste una linea di comunicazione diretta e libera da ostacoli, con l'unica fonte di degrado rappresentata dal rumore termico. Un esempio tipico è la comunicazione satellitare, dove il segnale viaggia attraverso lo spazio libero senza interferenze significative.

L'impulso risposta del canale in questo scenario è:
$$
h(t) = \delta(t)
$$

dove $\delta(t)$ è la funzione delta di Dirac. Questo indica che il canale non introduce distorsioni o ritardi al segnale trasmesso; il segnale viene semplicemente attenuato e affetto da rumore gaussiano bianco additivo.

La **probabilità di errore** per un tale canale può essere calcolata utilizzando le formule standard per un canale AWGN. Aumentare la potenza del segnale trasmesso in questo caso riduce efficacemente la BER, poiché migliora il **rapporto segnale-rumore (SNR)** al ricevitore.

### Canale Flat Fading

Quando il canale è affetto da **flat fading**, l'impulso risposta non è più una delta di Dirac pura. Il canale introduce variazioni casuali nell'ampiezza e nella fase del segnale, pur mantenendo una risposta in frequenza piatta sull'intera banda del segnale. L'impulso risposta può essere espresso come:
$$
h(t) = a \, e^{j\phi} \delta(t)
$$

dove:
- $a$ è un fattore di attenuazione aleatorio.
- $\phi$ è uno shift di fase aleatorio.

Per calcolare la **probabilità di errore** in un canale flat fading, è necessario considerare la variabilità introdotta dal fading. La BER condizionale al valore di $a$ e $\phi$ viene integrata sulla distribuzione statistica di questi parametri:

$$
\text{BER} = \int_{0}^{\infty} \text{BER}(a) \, f_{a}(a) \, da
$$

dove $f_{a}(a)$ è la funzione di densità di probabilità dell'attenuazione $a$.

In questo caso, aumentando la potenza del segnale trasmesso si ottiene una diminuzione della BER, simile al caso AWGN, ma l'effetto è meno pronunciato a causa delle fluttuazioni introdotte dal fading.

### Canale Multipath con ISI

Nel caso di un canale affetto da **multipath**, la situazione si complica ulteriormente. Il segnale trasmesso arriva al ricevitore attraverso molteplici percorsi, ognuno con diversi ritardi e attenuazioni dovuti a riflessioni, diffrazioni e scattering causati dagli ostacoli nell'ambiente.

L'impulso risposta del canale multipath è:
$$
h(t) = \sum_{l=0}^{L-1} a_{l} e^{j\phi_{l}} \delta(t - \tau_{l})
$$

dove:
- $L$ è il numero di percorsi multipli.
- $a_{l}$ è l'attenuazione del percorso $l$.
- $\phi_{l}$ è la fase associata al percorso $l$.
- $\tau_{l}$ è il ritardo del percorso $l$.

Questo causa **interferenza intersimbolica (ISI)**, poiché le repliche ritardate del segnale interferiscono con i simboli successivi. In presenza di ISI, la BER aumenta significativamente e non può essere ridotta efficacemente semplicemente aumentando la potenza del segnale trasmesso.

### Effetto dell'Aumento di Potenza nel Canale Multipath

Aumentare la potenza del segnale in un canale multipath con ISI può addirittura peggiorare le prestazioni:
- **Incremento dell'ISI:** L'ISI non dipende dalla potenza del segnale, ma dall'ampiezza relativa delle repliche ritardate. Aumentando la potenza, si amplificano sia il segnale desiderato sia le repliche interferenti, mantenendo inalterato il rapporto tra loro.
- **Limitazioni dell'SNR:** L'aumento della potenza migliora l'SNR, ma l'ISI introduce un **errore deterministico** che non può essere compensato semplicemente migliorando l'SNR.

Per questi motivi, l'aumento della potenza non offre vantaggi significativi nel ridurre la BER in presenza di multipath con ISI.

### Grafico della BER

Il grafico della BER in funzione dell'SNR per diversi scenari di canale (come illustrato nelle slide) mostra:
- **Canale AWGN Ideale:** La BER diminuisce esponenzialmente con l'aumento dell'SNR.
- **Canale Flat Fading:** La diminuzione della BER con l'aumento dell'SNR è meno rapida rispetto al canale AWGN, a causa delle variazioni di ampiezza introdotte dal fading.
- **Canale Multipath con ISI:** La BER si mantiene elevata nonostante l'aumento dell'SNR, evidenziando che l'ISI limita le prestazioni e che l'aumento della potenza non è efficace.

## BER per 2-PAM in un canale flat

Calcoliamo la probabilità di errore per un canale flat di una 2-PAM. Dal grafico si osserva che è una linea dritta e vogliamo capire perché.

Il segnale di arrivo è il seguente: $x(m) = \alpha \cdot a_{m}+n(m)$ ma per motivi di notazione possiamo trascurare il timing, quindi possiamo scrivere: $x = \alpha \cdot a + n$ con $n$ e $\alpha$ variabili aleatorie  $\alpha \in \{-1,+1\}$ (distribuita di Rayleigh) e $n \in \mathcal{N}(0,N_{0})$.

Per calcolare la probabilità di errore dobbiamo calcolare il valor medio fissando $\alpha$ in modo che $\alpha \cdot a \in \{-\alpha, +\alpha\}$, quindi la probabilità di errore si può calcolare in questo modo: 
$$
P(e|a) = \frac{1}{2} \cdot((P_{e}|_{a=-1,\alpha})+(P_{e}|_{a=1,\alpha}))
$$
Cosa possiamo dire di $x$ dato $(P_{e}|_{a=-1,\alpha})$? $$x = -\alpha + n$$ e quindi $x \in \mathcal{N}(-\alpha, N_{0})$ perché il rumore è gaussiano. Quindi otteniamo che $$P_{e}|_{a=-1,\alpha} = Q\left(\frac{\alpha}{\sqrt{N_{0}}}\right)$$trovato calcolando la distanza tra il valor medio (-$\alpha$) e il treshold ($0$) dividendo per la deviazione standard ($\sqrt{N_{0}}$). Svolgendo i calcoli: 
$$
\begin{align*}
P_{e}|_{a=-1,\alpha} &= Q\left(\frac{\alpha}{\sqrt{N_{0}}}\right) =\\[4pt]
&= Q\left(\alpha\cdot \sqrt{\frac{1}{N_{0}}}\right) =\\[4pt]
&= Q\left(\alpha\cdot \sqrt{\frac{2E_{s}}{N_{0}}}\right)
\end{align*}
$$
Perché siamo in una 2-PAM quindi $E_{s}= \frac{A}{2}= \frac{1}{2}\Rightarrow 2E_{s} = 1$.
Lo stesso vale anche per $P_{e}|_{a=1,\alpha} = Q\left(\alpha\cdot \sqrt{\frac{2E_{s}}{N_{0}}}\right)$.
Sostituiamo nella formula iniziale: $$P(e|a) = \frac{1}{2} \cdot Q\left(\alpha \cdot \sqrt{\frac{2E_{s}}{N_{0}}}\right)$$
Per trovare la probabilità di errore basta integrare:
$$
\begin{align*}
P(e) &= \int_{0}^{+\infty}P(e|\alpha)\cdot p(\alpha) d\alpha =\\[4pt]
&= \int_{0}^{+\infty}2\alpha e^{-\alpha^{2}}Q\left(\alpha \cdot \sqrt{\frac{2E_{s}}{N_{0}}}\right) d\alpha
\end{align*}
$$
Ci basta tra zero e infinito perché la distribuzione è di Rayleigh ed è quindi sempre positiva. 
La potenza del small scale fading dipende dal path loss e normalizziamo la potenza per fare in modo che sia uguale ad 1: $$E\{\alpha^{2}\} = E\{(\sqrt{h_{R}^{2}+ h_{I}^{2}})^{2}\} = E\{h_{R}^{2}+h_{I}^{2}\} = E\{h_{R}^{2}\} +E\{h_{I}^{2}\} = 2E\{h_{R}^{2}\} = 2\sigma^{2} = 1$$perché sono indipendenti. Inoltre, $\alpha = ||h||$ dove $h\in C(0,2\sigma^{2}\}$. Dovrò quindi scegliere $\sigma^{2} = \frac{1}{2}$ per far in modo che il valor medio sia uguale ad 1.
Posso quindi scrivere $p(\alpha) = 2\alpha e^{-\alpha^{2}}$ e sostituirlo nell'integrale.
Questo integrale si risolve per parti $\int_{a}^{b}f'(x)g(x) = f(x)g(x)|_{a}^{b}-\int_{a}^{b}f(x)g'(x) dx$:
- $f(x) = -e^{-\alpha^{2}}$
- $f'(x) = 2\alpha e^{-\alpha^{2}}$
- $g(x) = Q\left(\alpha \cdot \sqrt{\frac{2E_{s}}{N_{0}}}\right)$
Dobbiamo calcolare la derivata della Q-function, essa per definizione è pari a:
$$Q(x) = \frac{1}{2\pi}\int_{x}^{+\infty}e^{\frac{-t^{2}}{2}}dt = \frac{1}{2\pi}\left[1-\int_{-\infty}^{x} e^{\frac{-t^{2}}{2}}dt\right]$$
da cui si ricava che: $$\frac{\partial Q(x)}{\partial x} = \frac{-1}{2\pi}e^{\frac{-x^{2}}{2}}$$
Quindi, indicando con $\mu = \sqrt{\frac{2E_{2}}{N_{0}}}$: 
$$g'(\alpha) = \frac{\partial}{\partial\alpha}Q\left(\alpha \cdot \sqrt{\frac{2E_{s}}{N_{0}}}\right) = \frac{\partial}{\partial\alpha}Q(\alpha\mu) = -\frac{1}{\sqrt{2\pi}}e^{-\frac{\alpha^{2}\mu^{2}}{2}}\cdot \mu$$

Torniamo quindi all'integrale iniziale:
$$
\begin{align*}
P(e) &= -e^{-\alpha^{2}}Q(\alpha\mu)|_{0}^{+\infty}-\int_{0}^{+\infty}(-e^{-\alpha^{2}})\left(-\frac{\mu}{\sqrt{2\pi}}e^{-\frac{\alpha^{2}\mu^{2}}{2}}\right) d\alpha=\\[4pt]
&= \frac{\frac{1}{2}-\mu}{\sqrt{2\pi}}\int_{0}^{+\infty}e^{-\frac{\alpha^{2}(2+\mu^{2})}{2}} d\alpha
\end{align*}
$$
ora integriamo per sostituzione $\beta = \alpha\sqrt{(2+\mu^{2})}$ da cui $d\alpha = \frac{1}{\sqrt{2+\mu^{2}}}d\beta$:
$$
\begin{align*}
P(e) &= \frac{1}{2}-\frac{\mu}{\sqrt{2+\mu^{2}}} \int_{0}^{+\infty}e^{\frac{-\beta^{2}}{2}}d\beta = \\
&= \frac{1}{2}\left(1-\frac{\mu}{\sqrt{2+\mu^{2}}}\right)
\end{align*}
$$
In questa forma ancora non si capisce la forma della curva quindi sostituiamo di nuovo $\mu = \sqrt{\frac{2E_{s}}{N_{0}}}$:
$$
P(e) = \frac{1}{2}\left(1-\sqrt{\frac{\frac{E_{s}}{N_{0}}}{1-\frac{E_{s}}{N_{0}}}}\right)
$$
e quando $x$ è grande si ha $\sqrt\frac{x}{1+x}\approx 1-\frac{1}{2x}$ (per Taylor, questa è l'unica approssimazione fatta) per cui per valori grandi di SNR la probabilità di errore può essere approssimata come: $$P(e) \approx \frac{1}{4\frac{E_{s}}{N_{0}}}$$
E in decibel ha un andamento lineare decrescente: $$10log_{10}\left(\frac{1}{4\frac{E_{s}}{N_{0}}}\right) = -10log_{10}4-10log_{10}\frac{E_{s}}{N_{0}} = -6 -\left(\frac{E_{s}}{N_{0}}\right)_{db}$$
## Canale Variabile nel Tempo

In precedenza, abbiamo assunto che il canale avesse la seguente forma:
$$
h(t) = \sum_{l=0}^{L-1} \alpha_{l} e^{j\phi_{l}} \delta(t - \tau_{l})
$$

Questa espressione è valida solo se il trasmettitore e il ricevitore sono **stazionari** e l'ambiente circostante rimane immutabile nel tempo. In altre parole, il canale è considerato **lineare tempo-invariante** (LTI). Tuttavia, nella realtà, specialmente nelle comunicazioni mobili, sia il trasmettitore che il ricevitore possono essere in movimento, e l'ambiente può variare (ad esempio, a causa di oggetti in movimento o cambiamenti nelle condizioni atmosferiche). In questi casi, il canale diventa **variabile nel tempo**.

Quando il canale è variabile nel tempo, le caratteristiche dei percorsi multipli, come le ampiezze $\alpha_{l}$, le fasi $\phi_{l}$ e i ritardi $\tau_{l}$, possono cambiare nel tempo. Tuttavia, spesso si può assumere che i ritardi $\tau_{l}$ cambino in modo trascurabile (nell'ordine di nanosecondi) rispetto alla durata del simbolo, e quindi possono essere considerati costanti in prima approssimazione.

L'espressione generale del canale variabile nel tempo dipende sia dal tempo assoluto $t$ sia dal ritardo relativo $\tau$:
$$
h(t, \tau) = A_{LS} \sum_{l=0}^{L-1} \alpha_{l}(t) e^{j\phi_{l}(t)} \delta(\tau - \tau_{l})
$$

oppure, considerando che anche i ritardi possono variare leggermente nel tempo:
$$
h(t, \tau) = A_{LS} \sum_{l=0}^{L-1} \alpha_{l}(t) e^{j\phi_{l}(t)} \delta(\tau - \tau_{l}(t))
$$

Dove:
- $h(t, \tau)$ è la **risposta impulsiva del canale variabile nel tempo**.
- $A_{LS}$ rappresenta l'attenuazione dovuta al **large scale fading**.
- $\alpha_{l}(t)$ è l'ampiezza del percorso $l$ al tempo $t$.
- $\phi_{l}(t)$ è la fase del percorso $l$ al tempo $t$.
- $\tau_{l}(t)$ è il ritardo del percorso $l$ al tempo $t$.
- $\delta(\cdot)$ è la funzione delta di Dirac.

Questa espressione riflette il fatto che le caratteristiche del canale cambiano nel tempo a causa del movimento dei terminali o delle variazioni ambientali. Matematicamente, studiare un canale di questo tipo è complesso perché il sistema non è più **lineare e tempo-invariante (LTI)**, ma diventa un sistema **lineare tempo-variabile (LTV)**.

### Approccio di Analisi Semplificato

Per rendere l'analisi trattabile, si adotta spesso l'approccio di considerare il canale come **quasi stazionario** per intervalli di tempo molto brevi. In pratica, si "congela" il canale al tempo $t_0$, assumendo che le sue caratteristiche rimangano costanti in un piccolo intorno di $t_0$. Questo permette di trattare il canale come se fosse **momentaneamente lineare e tempo-invariante**, applicando le tecniche di analisi LTI per quell'intervallo di tempo.

Questo metodo è giustificato quando le variazioni del canale sono lente rispetto alla durata del simbolo o al tempo di osservazione. L'ipotesi è nota come **Wide-Sense Stationary Uncorrelated Scattering (WSSUS)**, che implica che:
- Il canale è stazionario in senso lato: le sue statistiche di primo e secondo ordine non cambiano significativamente nel tempo.
- I percorsi multipli sono non correlati tra loro.

### Implicazioni del Canale Variabile nel Tempo

Quando il canale è variabile nel tempo, emergono nuovi fenomeni che influenzano le prestazioni del sistema di comunicazione:
- **Doppler Spread:** A causa del movimento relativo tra trasmettitore e ricevitore, le frequenze dei segnali ricevuti subiscono uno shift doppler, che causa l'allargamento dello spettro in frequenza. Questo può portare a **fading selettivo nel tempo**.
- **Coherence Time ($T_c$)**: È l'intervallo di tempo durante il quale il canale può essere considerato stazionario. È inversamente proporzionale al Doppler Spread ($B_d$):
$$
  T_c \approx \frac{1}{B_d}
  $$

- **Fast Fading vs Slow Fading:**
  - **Fast Fading:** Le variazioni del canale sono rapide rispetto alla durata del simbolo ($T_c \ll T$). Il canale cambia all'interno di un simbolo, complicando la demodulazione.
  - **Slow Fading:** Le variazioni del canale sono lente rispetto alla durata del simbolo ($T_c \gg T$). Il canale può essere considerato costante durante la trasmissione di un simbolo o di un blocco di simboli.

## Effetto Doppler

L'**Effetto Doppler** è un fenomeno fisico che si manifesta come una variazione apparente della frequenza di un'onda percepita da un osservatore in movimento relativo rispetto alla sorgente dell'onda. Questo effetto si applica a vari tipi di onde, tra cui le onde sonore, luminose ed elettromagnetiche.
- **Avvicinamento della sorgente**: Se la sorgente dell'onda si muove verso l'osservatore, le onde vengono "compresse", risultando in un aumento della frequenza percepita. Nel caso del suono, il tono appare più acuto.
- **Allontanamento della sorgente**: Se la sorgente si allontana dall'osservatore, le onde vengono "allungate", portando a una diminuzione della frequenza percepita e a un tono più grave.

Consideriamo un esempio per illustrare l'effetto Doppler nel caso di un'onda sonora:
- Due osservatori, **A** e **B**, sono distanti esattamente 2 km l'uno dall'altro.
- Un'ambulanza si muove in linea retta da **A** verso **B** a una velocità costante di $\mathcal{v} = 120$ km/h.
- La velocità del suono nell'aria è $c = 1200$ km/h, cioè 10 volte la velocità dell'ambulanza.

Tempistiche degli eventi:
1. **Istante $t_0$**: L'ambulanza si trova presso l'osservatore **A** e inizia a suonare la sirena.
2. **Tempo impiegato dall'ambulanza per raggiungere B**:
$$
   \Delta t = \frac{d}{\mathcal{v}} = \frac{2 \text{ km}}{120 \text{ km/h}} = 60 \text{ secondi}
   $$
3. **Tempo impiegato dal suono per viaggiare da A a B**:
$$
   \Delta \tau = \frac{d}{c} = \frac{2 \text{ km}}{1200 \text{ km/h}} = 6 \text{ secondi}
   $$

Eventi chiave:
- **A $t = t_0 + 6 \text{ s}$**: L'osservatore **B** inizia a sentire la sirena dell'ambulanza.
- **A $t = t_0 + 60 \text{ s}$**: L'ambulanza raggiunge l'osservatore **B** e spegne la sirena.
- **A $t = t_0 + 66 \text{ s}$**: L'osservatore **A** smette di sentire la sirena.

Durata totale durante la quale gli osservatori sentono la sirena:
- **Osservatore A**: 66 secondi (da $t_0$ a $t_0 + 66 \text{ s}$).
- **Osservatore B**: 54 secondi (da $t_0 + 6 \text{ s}$ a $t_0 + 60 \text{ s}$).
- **Autista dell'ambulanza**: 60 secondi (da $t_0$ a $t_0 + 60 \text{ s}$).

Questo esempio illustra come il movimento della sorgente sonora rispetto agli osservatori influenzi la percezione della durata e della frequenza del suono a causa dell'effetto Doppler. In particolare, la frequenza percepita dal suono aumenta per l'osservatore **B** mentre l'ambulanza si avvicina e diminuisce per l'osservatore **A** mentre l'ambulanza si allontana.

### Calcolo del Doppler Shift nelle Comunicazioni Radio

Consideriamo ora l'effetto Doppler nel contesto delle comunicazioni radio:
- **Sorgente**: Una stazione radio trasmette un segnale sinusoidale:
$$
  s(t) = \sin(2\pi f_{c} t)
  $$
  dove $f_{c}$ è la frequenza portante.
- **Veicolo**: Un'auto si muove con velocità costante $\mathcal{v}$ allontanandosi dalla sorgente.
- **Velocità della luce**: $c$, la velocità delle onde elettromagnetiche (approssimata a $3 \times 10^8$ m/s).

Al tempo $t = t_{0}$, l'auto si trova a una distanza:
$$
d = \mathcal{v} \cdot t_{0}
$$
Il segnale trasmesso al tempo $t = t'$ arriva all'auto dopo un ritardo:
$$
\Delta \tau = \frac{d}{c} = \frac{\mathcal{v} t_{0}}{c}
$$
Il segnale ricevuto al tempo $t_{0}$ sarà:
$$
\begin{align*}
y(t_{0}) &= s(t_{0} - \Delta \tau) = \sin\left(2\pi f_{c} (t_{0} - \Delta \tau)\right) \\
&= \sin\left(2\pi f_{c} t_{0} - 2\pi f_{c} \frac{\mathcal{v} t_{0}}{c}\right) \\
&= \sin\left(2\pi \left(f_{c} - f_{c} \frac{\mathcal{v}}{c}\right) t_{0}\right)
\end{align*}
$$

Definiamo lo **Shift Doppler** $f_{D}$ come:
$$
f_{D} = - f_{c} \frac{\mathcal{v}}{c}
$$
Il segnale ricevuto diventa:
$$
y(t_{0}) = \sin\left(2\pi (f_{c} + f_{D}) t_{0}\right)
$$
Notiamo che:
- Se $\mathcal{v} > 0$ (l'auto si allontana dalla sorgente), allora $f_{D} < 0$ e la frequenza percepita diminuisce.
- Se $\mathcal{v} < 0$ (l'auto si avvicina alla sorgente), allora $f_{D} > 0$ e la frequenza percepita aumenta.

Poiché $c = f_{c} \lambda$, dove $\lambda$ è la lunghezza d'onda, possiamo scrivere:
$$
f_{D} = - \frac{\mathcal{v}}{\lambda}
$$
Questo esprime lo shift Doppler in termini della velocità relativa $\mathcal{v}$ e della lunghezza d'onda $\lambda$.

### Effetto Doppler in Presenza di Multipath

Nelle comunicazioni wireless reali, il segnale trasmesso arriva al ricevitore attraverso molteplici percorsi a causa di riflessioni, diffrazioni e scattering. Ogni percorso ha una diversa componente di velocità relativa e angolo di arrivo, causando differenti shift Doppler per ciascuna replica del segnale.

**Doppler Spread:**
- Il risultato è una **dispersione in frequenza** nota come **Doppler Spread**.
- Questo spread è un processo stocastico che descrive la distribuzione degli shift Doppler causati dai vari percorsi multipli.

**Densità Spettrale di Potenza del Canale:**
La densità spettrale di potenza del canale in presenza di Doppler Spread è:
$$
S_{D}(f) = \frac{1}{\pi f_{D}} \frac{1}{\sqrt{1 - \left( \frac{f}{f_{D}} \right)^{2}}}
$$
dove $f_{D}$ è il massimo shift Doppler, corrispondente al percorso con la massima velocità relativa.

**Funzione di Autocorrelazione del Canale:**
La funzione di autocorrelazione temporale del canale è data da:
$$
p(\Delta t) = J_{0}(2\pi f_{D} \Delta t)
$$
dove:

- $J_{0}$ è la funzione di Bessel di prima specie e ordine zero.
- $\Delta t$ è la differenza di tempo.

### Modello del Segnale Ricevuto

**Segnale ricevuto in presenza di fading:**
- **Canale Flat Fading:**
  $$
  y(t) = \alpha(t) e^{j\phi(t)} s(t - \tau)
  $$
  - $\alpha(t)$ e $\phi(t)$ variano nel tempo a causa del fading e dell'effetto Doppler.
  - $\tau$ è il ritardo medio (assunto costante).

- **Canale Frequency-Selective Fading:**
  $$
  y(t) = \sum_{l=0}^{L-1} \alpha_{l} e^{j\phi_{l}(t)} s(t - \tau_{l})
  $$
  - $L$ è il numero di percorsi multipli.
  - $\alpha_{l}$, $\phi_{l}(t)$ e $\tau_{l}$ sono rispettivamente l'ampiezza, la fase (variabile nel tempo) e il ritardo associati al percorso $l$.

**Effetto sul Segnale Ricevuto:**
- Il Doppler Spread causa una variazione nel tempo delle ampiezze e delle fasi dei segnali ricevuti.
- Il segnale ricevuto è affetto da **fast fading**, caratterizzato da rapide fluttuazioni dell'ampiezza del segnale.

### Coherence Time del Canale

Per semplificare l'analisi, è utile definire il **Coherence Time** $T_{c}$ del canale, che rappresenta l'intervallo di tempo durante il quale il canale può essere considerato statisticamente costante.

Il coherence time può essere approssimato trovando il valore di $\Delta t$ per cui la funzione di autocorrelazione scende a un determinato valore (ad esempio, quando $p(\Delta t) = 0$).

Assumendo che $J_{0}(2\pi f_{D} T_{c}) \approx 0$ per $2\pi f_{D} T_{c} = 2\pi \times \frac{1}{2}$, otteniamo:
$$
2\pi f_{D} T_{c} = 2\pi \times \frac{1}{2} \quad \Rightarrow \quad f_{D} T_{c} = \frac{1}{2}
$$
Da cui:
$$
T_{c} = \frac{1}{2 f_{D}}
$$

La **Coherence Distance** $d_{c}$ è la distanza percorsa dal ricevitore durante il coherence time:
$$
d_{c} = \mathcal{v} \cdot T_{c} = \mathcal{v} \cdot \frac{1}{2 f_{D}}
$$
Poiché $f_{D} = \frac{\mathcal{v}}{\lambda}$, sostituendo otteniamo:
$$
d_{c} = \mathcal{v} \cdot \frac{1}{2} \cdot \frac{\lambda}{\mathcal{v}} = \frac{\lambda}{2}
$$
Quindi, il canale può essere considerato statisticamente costante per distanze inferiori a metà lunghezza d'onda.

### Implicazioni dell'Effetto Doppler sui Sistemi di Comunicazione

- **Fast Fading:** Quando il coherence time $T_{c}$ è minore della durata del simbolo $T$, il canale varia significativamente durante la trasmissione di un simbolo, causando fast fading.
- **Slow Fading:** Se $T_{c} \gg T$, il canale rimane costante durante la trasmissione di più simboli.

**Mitigazione degli Effetti del Doppler:**
- **Diversità Temporale e Frequenziale:** Utilizzo di tecniche che sfruttano la diversità nel tempo e nella frequenza per combattere il fading.
- **Equalizzazione Adattativa:** Implementazione di equalizzatori che si adattano alle variazioni del canale.
- **Codifica e Interleaving:** Utilizzo di codici di correzione d'errore e interleaving per distribuire gli errori causati dal fading.

### Conclusioni

L'effetto Doppler ha un impatto significativo sui sistemi di comunicazione wireless, soprattutto in scenari mobili. La variazione della frequenza dovuta al movimento relativo tra trasmettitore e ricevitore non solo altera la frequenza percepita del segnale, ma, in presenza di multipath, introduce una dispersione in frequenza nota come **Doppler Spread**. Questa dispersione causa variazioni rapide dell'ampiezza e della fase del segnale ricevuto, note come **fast fading**.

La comprensione e la modellazione dell'effetto Doppler sono essenziali per la progettazione di sistemi di comunicazione robusti ed efficienti. Attraverso la caratterizzazione del canale mediante parametri come il **Coherence Time** e la **Coherence Bandwidth**, è possibile implementare tecniche avanzate per mitigare gli effetti negativi del fading e migliorare le prestazioni complessive del sistema.