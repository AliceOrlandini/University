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

Uno dei codici 