# Diversity in Wireless Communications

Nel caso di flat fading channel c’è bisogno di molta energia per ridurre la BER ma non è sempre possibile per ragioni di batteria ad esempio.
Un modo per aumentare l’efficienza si chiama *diversity* ed è l’opportunità di aumentare la reliability di un segnale trasmettendolo in due o più canali di comunicazione con caratteristiche differenti. Quindi si trasmette lo stesso segnale ma ad una frequenza diversa $f_{c}^{’}$ tale che $f_{c}^{’}-f_{c}> B_{C}$.
Facendo questo per 4 canali differenti poi si seleziona il canale con l’attenuazione minore. (figura sulle slide)
Nella realtà è difficilmente applicabile perché ci servirebbe il quadruplo della banda del segnale.

Esistono vari modi di effettuare la diversity, i più efficienti sono:
- **Time Diversity**: relativo alla *coherence time*. 
- **Frequency Diversity**: relativo alla *coherence bandwidth*.
- **Spatial Diversity**: relativo alla *coherence distance*.

## Time Diversity

Il concetto di time diversity è stato formalizzato da Shannon nel 1948 ed legato alla teoria dei codici che consiste nell’aggiungere ridondanza al messaggio per rilevare ed eventualmente correggere gli errori di trasmissioni.
La ridondanza è misurata tramite il rate del codice $R = \frac{k}{n}<1$ con $k$ i bit entranti ed $n$ i bit uscenti dall’encoder. 

Per introdurre questi concetti bisogna studiare i **Campi di Galois**. Sono degli insiemi in cui sono permesse due operazioni: addizione e moltiplicazione che devono dare come risultato un elemento che fa ancora parte del campo, quindi il campo ha un numero di elementi finito.
In $GF(2)$ il campo è composto da $\{0,1\}$ possiamo fare l’operazione $xor$ e la $and$.
Diagramma a blocchi sulle slide con l’encoder e il decoder. 

Esistono due tipologie di codici: a **blocco** e **convoluzionali**.
Nei codici a blocco vengono aggiunti $n-k$ bit di ridondanza (chiamati anche *parity bits*). Questi codici vengono rappresentati tramite un prodotto algebrico tra il vettore di bit in ingresso e una matrice che rappresenta il codice: $$d = uG$$
Nell’error detection vogliamo riconoscere gli errori facendo la stessa operazione e vedendo il risultato.

Uno dei codici a blocco più importanti è il controllo di parità con rate $R = \frac{7}{8}$ ovvero $k = 7$ e $n = 8$.
Al ricevitore basta contare il numero di “uni” e se è dispari ho commesso errore. Se invece il risultato è pari potrei aver commesso comunque un errore. 

Vediamo come questo può essere applicato alla diversity time trasmission. Vogliamo incrementare la diversity solo quando troviamo un errore (**ARQ**).
$T_{ARQ}>T_{c}$ la seconda replica verrà ricevuto dopo il cohearence time per poi combinarli. 

Dato un canale di comunicazione con banda $B$ allora la **capacità del canale** può essere calcolata come: $$C = B\cdot log_{2}(1+SNR) \text{ bit/s}$$In una trasmissione con rate $R \le C$ allora per ogni piccolo $\epsilon$ è possibile trovare un codice tale che $P_{e}< \epsilon$.
Se invece $R > C$ non è possibile trovare un codice che rende la probabilità di errore sufficientemente piccola.
È quindi importante capire qual è la capacità del canale.

Nel codici convoluzionali invece il trasmettitore invia solamente i parity bits. 
L’encoder di un codice convoluzionale $(n,k,L)$ fa l’operazione di convoluzione in $GF(2)$.
Da notare che l’output al tempo $i$ dipende dai precedenti input, questo differenzia questa tipologia di codici da quelli a blocchi che invece dipendono solo dall’ingresso corrente.
$L-1$ è la memoria necessaria perché rappresenta la storia del sistema chiamata anche *stato*.
Esempio sulle slide.
È possibile disegnare anche il diagramma di stato visto che abbiamo della memoria. 
Un terzo modo per rappresentare l’output dell’encoder è il **Diagramma di Trellis** che ci permette di seguire il cambiamento dello stato in relazione all’ingresso.

Vediamo il decoder nel codici convoluzionali, ovvero la procedura per ricavare i bit del messaggio a partire da quelli ricevuti (che sono oggetto della convoluzione).
Devo trovare un path sul traliccio che ha distanza minima.
Visto che siamo in $GF(2)$ la distanza minima consiste nella distanza di Hamming quindi devo trovare il path sull traliccio che minimizza questa distanza.
**“AD ESEMPLE”**
Per trovare questo path con distanza minima un modo è trovare tutti i possibili path e calcolarne le distanze. Il problema di questo approccio è che il numero di path aumenta esponenzialmente col numero di bit nella sequenza. 
Per questo è stato inventato **l’Algoritmo di Viterbi** che permette di escludere dei path. 

Ogni sequenza $d$ corrisponde univocamente ad una sequenza di stati $s_{0}, s_{1}, …$.
L’output dell’encoder $d_{j}$ dipende dalla transizione tra gli stati $s_{j-1}$ e $s_{j}$. 
La distanza $d_{H}(d,x)$ può essere calcolata sul traliccio come:$$d_{H}(d,x) = \sum\limits_{i=1}^{N}d_{H}(d_{s_{j-1}\rightarrow s_{j}},x_{j}) = \sum\limits_{j=1}^{N}\lambda_{s_{j-1}\rightarrow s_{j}}$$dove $\lambda_{s_{j-1}\rightarrow s_{j}} = d_{H}(d_{s_{j-1}\rightarrow s_{j}},x_{j})$ è definita come *branch metric*.
La distanza finale è quindi solo la somma di tutte le branch metric $\lambda_{s_{j-1}\rightarrow s_{j}}$.
Sapendo la sequenza di stati, la distanza tra la sequenza ricevuta ed i bit ricavati può essere calcolata iterativamente (step by step): $$\Lambda_{m}=\sum\limits_{j=1}^{m}\lambda_{s_{j-1}\rightarrow s_{j}}$$Viterbi osservò che se due sequenze $s^{(1)}$ e $s^{(2)}$ arrivano allo stesso punto allora la metrica comulativa può essere calcolata come: $$\Lambda_{N}^{(1)} = \Lambda_{m}^{(1)}+\sum\limits$$e osservò che se $\Lambda_{m}^{(1)}< \Lambda_{m}^{(2)}$ allora la seconda sequenza sarà sempre peggiore della prima perché è ottenuta sommando un altro termine. 
Quindi quando due sequenze si uniscono posso mantenerne solo una. Il numero di path aumenta quindi linearmente. 

# Interleaving

I codici convoluzionali sono utili quando si hanno canali memoryless e AWGN con random error events. 
I codici a correzione sono performanti quando gli errori sono uniformemente distribuiti e incorrelati. 
Ma se abbiamo flat fading channel che cambia nel tempo, ho più probabilità di commettere errore quando ci sono le attenuazioni, ovvero quando l’SNR è basso e quindi il segnale è “basso”.
Quindi anche se in media avrei pochi errori, quando il canale è attenuato non ho più errori uniformemente e incorrelati. Gli errori si possono correggere quando sono molto distanti tra loro, ad esempio Viterbi inizierebbe a seguire un path totalmente sbagliato.

L’interliving prende il canale e lo mixa in modo da avere errori incorrelati. 
Per fare ciò prima del decoder metto un Deinterleaver (schema a blocchi sulle slide).
Si mescolano i messaggi da inviare in modo che siano pseudo-random distribuiti. 
L’effetto è che se ho 2 bit trasmessi adiacentemente, dopo l’interliving non sono adiacenti e quindi il codice convoluzionale torna performante perché gli errori vengono sparsi. 

L’interliving ha un costo perché se ho una sequenza molto lunga devo aspettare tanto tempo, soprattutto se uso la combo interliving e Vitermi. Si usa quando si vuole una comunicazione robusta, ad esempio per il trasferimento di un file, per la voce non si usa perché il delay e la latenza sono fondamentali. 

# Turbo codes and LDPC

Questi sono due nuovi tipi di codici che raggiungono performance incredibili. 
Shannon ha dato un limite ben preciso di bit per simbolo inviabili nel canale, i turbo codes ci si avvicinano infatti vengono usate in tutti i sistemi di comunicazioni moderni. 
Nei turbo code ci sono i feedback, funziona come i motori: il risultato dell’esplosione della benzina va in una turbina e la attiva generando energia. La pressione diventa così elevata che diventa caldo e c’è bisogno di un sistema di raffreddamento dato dall’acqua. 
Alla fine si estrae energia dall’esplosione. 
L’idea è quella del *positive feedback*.

I turbo code sono un codice sistematico con un rate $R = \frac{1}{3}$ 
Immagine sulle slide.
Ho 3 repliche:
- $d^{(1)} = p_{1}$ va attraverso ad un codice convoluzionale.
- $d^{(0)}= u$ non ha modifiche.
- $d^{(2)} = p_{2}$ va attraverso ad un interleaver e ad un altro codice convoluzionale.

Il decoder decodifica per prima cosa i bit non alterati e si fanno delle decisioni che non sono sì o no ma sono più un feedback, ad esempio “questo è probabilmente uno zero” chiamata relaiability.
Poi andiamo a fare l’interleaving, per poi fare un altra decisione col parity 2.
L’output del secondo decoder è ancora una relaiability, poi la porto in retroazione e riprovo la parity1 e la relaiability che è questa volta molto meglio del sistematic.
Ogni volta che passo attraverso il chain c’è un positive feedback perché aumento la relaiability.  
Non ho capito nulla, schema sulle slide. 
Quindi è come se avessimo degli stream dei dati diversi, e l’interleaving li rende incorrelati. 

Nel grafico vediamo la BER della 2-PAM, e il turbo code con varie iterazioni. 