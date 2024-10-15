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

# Heterodyne receiver architecture 

Quando studiamo il ricevitore analogico, lo immaginiamo come questo (*Direct Conversion Receiver*) (parte a destra con il LPF):

![[Modello con inviluppo complesso.png]]

In realtà, l'architettura reale è un po' diversa da questa che invece viene utilizzata solo a scopi scolastici per comprendere la comunicazione analogica. Il vero ricevitore sarà fatto in questo modo (disegno incompleto, sulle slide c'è completo): 

![receiver|center|600](https://rahsoft.com/wp-content/uploads/2021/08/Screenshot-2021-08-17-at-16.45.09-600x318.png)

La prima cosa che troviamo nello schema a blocchi di un ricevitore è sempre **l'antenna** che cattura il segnale elettromagnetico dall'aria. 
Subito dopo, troviamo un **filtro passa banda** utile per rimuovere le interferenze date dalla presenza del rumore, poi **amplifico** il segnale ottenuto. 
Da questo punto l'architettura cambia rispetto a come lo abbiamo visto prima perché quello che faremo è passare dalla frequenza RF a quella base in 2 passaggi e non in uno solo come avveniva prima. 
La prima demodulazione la facciamo spostandoci da RF ad una frequenza chiamata *intermediate frequency IF* per fare in modo che qualsiasi frequenza sia RF, che sappiamo essere variabile, dopo la modulazione ci troveremo sempre un segnale centrato in IF. 
Quindi, il segnale ricevuto è centrato in $f_c$ e sarà: 
$$r(t) = Re\{\tilde{r}(t)e^{j2\pi f_{c}t}\} = r_{I}(t)cos(2\pi f_{c}t) - r_{Q}(t)sin(2\pi f_{c}t)$$
E vogliamo spostare $r(t)$ in $f_{IF}$ moltiplicandolo per un $cos(2\pi f_{LO}t)$ con $f_{LO}= f_{c}- f_{IF}$ acronimo di *local oscillator*.
Assumiamo per esempio che $r(t) = m(t)cos(2\pi f_{c}t)$, un segnale modulato DSB, ed eseguiamo i conti ricordando che $cos(\alpha)cos(\beta) = \frac{1}{2} cos(\alpha + \beta) + \frac{1}{2}cos(\alpha - \beta)$:
$$
\begin{align*}
r(t)cos(2\pi f_{LO}t) &= m(t)cos(2\pi f_{c}t)cos(2\pi(f_{c}-f_{IF})t) =\\[4pt]
&=\frac{1}{2}m(t)cos(2\pi(2f_{c}-f_{IF})t)+\frac{1}{2}m(t)cos(2\pi f_{IF}t)\\[4pt]
\end{align*}$$

Il primo termine è un componente ad alta frequenza perché $f_c$ è molto più grande di $f_{IF}$, il secondo termine invece notiamo che contiene il segnale trasmesso $m(t)$ ed è modulato alla frequenza $f_{IF}$. Quindi, il primo termine lo eliminiamo con un filtro centrato in $f_{IF}$ e ci sembrerebbe di aver finito. 
In realtà c'è un problema: sappiamo che $cos(\alpha - \beta) = cos(\beta - \alpha)$ quindi potrebbe accadere che alcuni componenti frequenziali simmetrici ad $f_{IF}$ vadano a interferire col secondo termine. (qui Moretti a parole l'ha spiegato malissimo ma dal disegno su OneNote si capisce che intende dire che se ho un segnale nell'asse negativo, anche lui verrà demodulato e può succedere che si sovrapponga al segnale in $f_{IF}$, almeno questo ho capito io). Questo segnale si chiama *segnale immagine*, un segnale simmetrico ad $f_{IF}$ che si pone esattamente in sua corrispondenza. 
Per risolvere questo problema, *prima* della modulazione si rimuovono tutte le potenziali immagini con un filtro chiamato per l'appunto **image filter**. 
Una domanda potrebbe essere: "eh ma non bastava il primo filtro?" no. perché il primo è un filtro molto più largo, questo invece è molto più selettivo. 
Dopo il filtro si esegue la **modulazione** vista precedentemente e poi si pone un altro **filtro** in corrispondenza di $f_{IF}$ per estrarre il segnale, quest'ultimo è l'unico filtro "fisso" ed è il più selettivo di tutti. 
Infine, si **amplifica** il segnale e lo si **demodula**. 
Il segnale che otteniamo in uscita sarà quindi: 
$$r(t) = Re\{\tilde{r}(t) e^{j2\pi f_{IF}t}\}$$
Sì, sembra di essere al punto di partenza perché è cambiato "solo" $f_{IF}$ ma il grande vantaggio è che a questo punto posso usare una frequenza di campionamento fissa perché questa frequenza è scelta in modo tale che $2(f_{IF} + B) \le f_s$ con $f_{s} = 28.8 MHz$ la frequenza di campionamento. 
Di conseguenza, $(f_{IF}+ B) \le 14.4 MHz$ e posso trattare il segnale come un segnale in banda base con una banda che oscilla tra -14.4 e +14.4 MHz (se la relazione scritta sopra è soddisfatta). 

Ecco il modello completo:

![[RF to baseband.png|center|700]]

Proviamo a dare alcuni numeri, nel *super-heterodyne receiver* si ha un ricevitore con le seguenti caratteristiche:
1. i segnali ricevuti vengono modulati ad $f_{IF}= 3.57MHz$.
2. il segnale in $f_{IF}$ viene campionato con una $f_{s}= 28.8$ MS/s = $1.4MHz$ (mega-samples per second) in un convertitore ADC ad 8 bit. 
3. il segnale digitale viene inviato con un certo rate, il massimo è circa $2.8$ MS/s.
4. infine, i campioni vengono passati al computer tramite un collegamento USB.

Due parole sul segnale digitale, il segnale verrà campionato ad una frequenza $f_{s}= \frac{1}{T_{s}}$ quindi avrà la seguente forma:
$$
\begin{align*}
r[n] &= Re\{\tilde{r}[n]cos(2\pi f_{IF}nT_{s})\} =\\[4pt]
&= Re\{\tilde{r}[n]cos(2\pi \frac{f_{IF}}{f_{s}}n)\}\\[4pt]
\end{align*}$$
In cui $\frac{f_{IF}}{f_{s}} = \frac{3.57}{28.8} = 0.123 MHz$.

Momento sfogo: ho dovuto riascoltare tutta la lezione mettendo in pausa e tornando indietro, non smetterò mai di dire che l’inglese di Moretti rende la lezione particolarmente ostica, menomale che almeno registra. 
Correzione: Moretti ha appena detto che siccome a lezione non c’è nessuno (siamo tipo 10) toglierà le registrazioni. Bro, fatti due domande se a lezione non c’è nessuno…

# Implementazione pratica dell'FM Receiver

![[Ricevitore FM.png|center]]

Il primo elemento che troviamo nel ricevitore FM è l'antenna che riceve il segnale e lo passa all'SDR che lo campiona a 228 kS/s. 
Poi, i campioni vengono processati in MATLAB.
Il segnale FM è attualmente trasmesso in stereo ma originariamente veniva trasmesso in mono cioè: $m(t) = L(t) + R(t)$ normalizzato ad 1 e con $k_{f} = 75$ KHz/V in modo che $\Delta f = k_{f}max|m(t)| = 75$ kHZ con $m_{f} = \frac{\Delta f}{B} = \frac{75}{15} = 5$.

Come abbiamo visto nel capitolo del [[Comunicazione Analogica#Frequency Modulation (FM)|ricevitore FM]], l'inviluppo complesso del segnale ricevuto sarà (tralasciando il rumore): 
$$\tilde{v}(t) = e^{j 2\pi f_{k} \int_{-\infty}^{t} m(\tau) d\tau}$$
Poi, la fase sarà: 
$$\angle \tilde{v}(t) = 2 \pi k_{f} \int_{-\infty}^{t} m(\tau) d\tau$$
E la sua derivata:
$$\hat{m}(t) = \frac{1}{2\pi k_{f}} \frac{d}{dt} \angle \tilde{v}(t)$$
Nel ricevitore reale in realtà si fa anche qualcosa in più, vediamo cosa.
L'SDR restituisce un inviluppo complesso campionato con una frequenza $f_{s}= \frac{1}{T_{s}}$:
$$
\begin{align*}
\tilde{v}(k) &= \tilde{v}(t)|_{t = kT_{s}} \\[4pt]
&= e^{j2 \pi k_{f}\int_{-\infty}^{kT_{s}}m(\tau) d\tau } =\\[4pt]
&\approx e^{j2\pi k_{f}\sum\limits_{l=-\infty}^{k}m(l)T_{s}}\\[4pt]
\end{align*}$$
L'approssimazione è valida perché la frequenza di campionamento è molto alta ($f_{m} = 2\cdot (1+m_{f}) \cdot B_{m}$).
Scriviamo ora la formula del campione precedente: 
$$\tilde{v}(k-1) = e^{j2 \pi k_{f}\sum\limits_{l=-\infty}^{k-1} m(l)T_{s}}$$
E facciamo la seguente operazione: 
$$
\begin{align*}
\tilde{v}(k) \cdot \tilde{v}^{*}(k-1) & \approx e^{j2 \pi k_{f}\sum\limits_{l = -\infty}^{k}m(l)T_{s}} \cdot e^{-j2 \pi k_{f}\sum\limits_{l = -\infty}^{k-1}m(l)T_{s}}\\[4pt]
&= e^{j2\pi k_{f}m(k) T_{s}}
\end{align*}
$$
Quindi, moltiplicando due campioni vicini si ottiene un esponenziale che ha nell'argomento il campione che si sta cercando. Perciò, invece di svolgere la derivata si può semplicemente eseguire una moltiplicazione nel seguente modo:
$$m(k) = \frac{1}{2\pi k_{f}T_{s}}\angle [\tilde{v}(k) \tilde{v}^*(k-1)]$$
$k_{f}$ può essere sostituito con $\Delta f$ perché abbiamo normalizzato $m(f)$ quindi sono uguali. 
In MATLAB il ricevitore è implementato così e non con la derivata. 

# Effetti degli errori di sincronizzazione

L'errore di fase può essere scritto come $e^{j(2\pi \Delta f t + \Delta \varphi)}$ che viene moltiplicato a $\tilde{s}_{FM}(t)$. 
Quando si considera la fase dell'inviluppo complesso con l'errore di fase otteniamo:
$$\angle \tilde{s}_{FM}(t)e^{j(2\pi \Delta f t + \Delta \varphi)} =  2\pi \Delta f t + 2\pi k_{f}\int_{-\infty}^{t}m(\tau) d\tau + \Delta \varphi$$

Poi si effettua l'operazione di derivazione seguita da una moltiplicazione per $\frac{1}{2\pi k_{f}}$ per ottenere il segnale ed eliminare lo sfasamento $\Delta \varphi$ in modo da ottenere:
$$\frac{\Delta f}{k_{f}} + m(t)$$
L'effetto dell'errore di sincronizzazione sarà solo una costante, ma $k_{f}$ è molto più grande di $\Delta f$ quindi il termine è trascurabile. 

# Programmare un RTL-SDR con MATLAB

MATLAB è un linguaggio di programmazione orientato agli oggetti, permette infatti di creare classi per strutture dati complesse con un set di operazioni che si possono effettuare su tali strutture dati. 
Esiste ad esempio una classe che rappresenta il RTL-SDR e si installa andando nella sezione *add-ons* e, per alcuni PC (Windows più che altro), per usarla bisogna installare i driver. 

La classe si instanzia in questo modo *radio = comm.SDRRTLSReciver*.
Uno degli attributi della classe è *CenterFrequency*
Per impostare la BandWidth del segnale non abbiamo un attributo, per impostarlo si sfrutta il  il *SampleRate* e il teorema di Nyquist. 

L'SDR manda l’inviluppo complesso al computer, per prendere il segnale l’FM bisogna campionarlo almeno alla frequenza di campionamento minima, la FM manda il segale a 50kHz, qual è la frequenza minima? Nessuno risponde perché nessuno ha capito la domanda. Bisogna considerare la regola di Carson:
$B_{FM} \simeq 2(m_{f} + 1)B= 6 \cdot 2 \cdot 50 = 600$ kHz
