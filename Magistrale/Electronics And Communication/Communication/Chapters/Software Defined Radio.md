# RTL-SDR

L’idea del **Software Defined Radio** è quella di processare un segnale per farlo diventare un segnale digitale ed elaborarlo via software utilizzando strumenti come MATLAB. 
È nato in ambiente militare, all’inizio ogni sistema aveva una radio specifica, mentre ora con una sola antenna si possono prelevare più segnali da processare via software.
Ad esempio, nel telefono abbiamo: GPS, FM radio, 5G, 4G, Bluetooth, Wi-Fi e chissà quanti altri.
La cosa importante è che l'operazione di campionamento la fa una sola entità.

L'RTL-SDR è fatto nel seguente modo: 

![SDR|center|700](https://d3i71xaburhd42.cloudfront.net/69780c5100d9ac6ed2afcb33399fe40fb749fe57/2-Figure4-1.png)

Le parti sul quale ci concentreremo maggiormente sono 3: 
1. **R820T DTV Tuner**: per la processazione analogica del segnale.
2. **28.8MHz Clock Crystal**: oscillatore a 28 MHz.
3. **RTL2832U COFDM Demodulator**: "baseband processing unit". (non ha detto altro) 

L'idea principale è quella di avere una frequenza radio (RF) programmabile, convertire il segnale analogico in uno digitale per poi demodularlo in banda base e infine elaborare il tutto via software. 
Quindi, i blocchi che compongono il SDR sono i seguenti: 

![SDR diagramma a blocchi](https://www.researchgate.net/profile/Stephen-Ugwuanyi/publication/328164022/figure/fig1/AS:701214731796481@1544194028671/Simple-SDR-Architecture.ppm)

# Heterodyne architecture 

Quando studiamo il ricevitore analogico, lo immaginiamo come questo (*Direct Conversion Receiver*) (parte a destra con il LPF):
![[Modello con inviluppo complesso.png]]

In realtà, l'architettura reale è un po' diversa da questa che invece viene utilizzata solo a scopi scolastici per comprendere la comunicazione analogica. Il vero ricevitore sarà fatto in questo modo: 

![receiver|center|600](https://rahsoft.com/wp-content/uploads/2021/08/Screenshot-2021-08-17-at-16.45.09-600x318.png)

La prima cosa che troviamo nello schema a blocchi di un ricevitore è sempre **l'antenna** che cattura il segnale elettromagnetico dall'aria. 
Subito dopo, troviamo un **filtro passa banda** utile per rimuovere le interferenze date dalla presenza del rumore, poi **amplifico** il segnale ottenuto. 
Da questo punto l'architettura cambia rispetto a come lo abbiamo visto prima perché quello che faremo è passare dalla frequenza RF a quella base in 2 passaggi e non in uno solo come avveniva prima. 
La prima demodulazione la facciamo spostandoci da RF ad una frequenza chiamata *intermediate frequency IF* per fare in modo che qualsiasi frequenza sia RF, che sappiamo essere variabile, dopo la modulazione ci troveremo sempre un segnale centrato in IF. 
Quindi, il segnale ricevuto è centrato in $f_c$ e sarà: 
$$r(t) = Re\{\tilde{r}(t)e^{j2\pi f_{c}t}\} = r_{I}(t)cos(2\pi f_{c}t) - r_{Q}(t)sin(2\pi f_{c}t)$$
E vogliamo spostare $r(t)$ in $f_{IF}$ moltiplicandolo per un $cos(2\pi f_{LO}t)$ con $f_{LO}= f_{c}- f_{IF}$ acronimo di *local oscillator*.
Assumiamo per esempio che $r(t) = m(t)cos(2\pi f_{c}t)$, un segnale modulato DSB, ed eseguiamo i conti:
$$r(t)cos(2\pi f_{LO}t) =$$
$$ = m(t)cos(2\pi f_{c}t)cos(2\pi(f_{c}-f_{IF})t) =$$
Ricordiamo che $cos(\alpha)cos(\beta) = \frac{1}{2} cos(\alpha + \beta) + \frac{1}{2}cos(\alpha - \beta)$:
$$= \frac{1}{2}m(t)cos(2\pi(2f_{c}-f_{IF})t)+\frac{1}{2}m(t)cos(2\pi f_{IF}t)$$
Il primo termine è un componente ad alta frequenza perché $f_c$ è molto più grande di $f_{IF}$, il secondo termine invece notiamo che contiene il segnale trasmesso $m(t)$ ed è modulato alla frequenza $f_{IF}$. Quindi, il primo termine lo eliminiamo con un filtro centrato in $f_{IF}$ e ci sembrerebbe di aver finito. 
In realtà c'è un problema: sappiamo che $cos(\alpha - \beta) = cos(\beta - \alpha)$ quindi potrebbe accadere che alcuni componenti frequenziali simmetrici ad $f_{IF}$ vadano a interferire col secondo termine. (qui Moretti a parole l'ha spiegato malissimo ma dal disegno su OneNote si capisce che intende dire che se ho un segnale nell'asse negativo, anche lui verrà demodulato e può succedere che si sovrapponga al segnale in $f_{IF}$, almeno questo ho capito io). Questo segnale si chiama *segnale immagine*, un segnale simmetrico ad $f_{IF}$ che si pone esattamente in sua corrispondenza. 
Per risolvere questo problema, *prima* della modulazione si rimuovono tutte le potenziali immagini con un filtro chiamato per l'appunto **image filter**. 
Dopo il filtro si esegue la **modulazione** vista precedentemente e poi si pone un altro filtro in corrispondenza di $f_{IF}$ per estrarre il segnale. 

Da design si ha che $2f_{IF} + B \le f_s$ con $f_s$ la sampling frequency. 
28.8MHz è la sampling frequency.
$2(f_{IF} + B) \le 28.8 MHz$
$f_{IF} + B \le 14.4 MHz$
Il segnale starà tra -14.4 e +14.4 MHz. 

Mi sa che questa lezione va riascoltata, non smetterò mai di dire che l’inglese di Moretti rende la lezione particolarmente ostica. 

# Programmare un RTL-SDR con MATLAB

MATLAB è un linguaggio di programmazione orientato agli oggetti, permette infatti di creare classi per strutture dati complesse con un set di operazioni che si possono effettuare su tali strutture dati. 
Esiste ad esempio una classe che rappresenta il RTL-SDR e si installa andando nella sezione *add-ons* e, per alcuni PC (Windows più che altro), per usarla bisogna installare i driver. 

La classe si chiama *radio = comm.SDRRTLSReciver*
uno degli attributi della classe è CenterFrequency, un altro è la Band Width del segnale, per impostarlo sfrutto il SampleRate e il teorema di Nyquist. 
SDR manda l’inviluppo complesso al computer, per prendere l’FM signal bisogna campionarlo almeno alla frequenza di campionamento minima, la fm manda il segale a 50kHz signal, qual è la frequenza minima? Nessuno risponde perché nessuno ha capito la domanda. Bisogna considerare la regola di Carson:
$B_{FM} \simeq 2(m_f + 1)B$
= 6 2 50 = 600

