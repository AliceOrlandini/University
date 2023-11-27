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