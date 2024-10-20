# Amplitude Modulation dual side band (AM-DSB)

La modulazione di ampiezza sfrutta la proprietà dei segnali *reali* di avere una trasformata di Fourier con *simmetria hermitiana*.
Ad esempio dato $m(t)$, esso avrà una $M(f)$ tale che: $$M(-f) = M^*(f)$$Quello che possiamo fare è moltiplicare $m(t)$ per $cos(2\pi f_ct)$ al fine di ottenere il segnale modulato. Nel dominio della frequenza, questa operazione corrisponde a duplicare il segnale a destra e ha sinistra come in figura: 

![Modulazione | center | 500](https://i.sstatic.net/NURvo.png)

Il segnale modulato, nel dominio della frequenza sarà: $$S(f) = \frac{M(f - f_c)}{2} + \frac{M(f + f_c)}{2}$$E la sua banda sarà il doppio della banda del segnale di partenza, quindi passerà da $w$ a $2w$.
Questo processo non è quindi efficiente in termini di consumo dello spettro ma ha il grande vantaggio di permetterci di realizzare il segnale in banda base (frequenze intorno allo zero) per poi andarlo a modulare alla frequenza che ci è stata assegnata subito prima dell’invio del segnale. 

Per quanto riguarda il processo inverso, ovvero la ricostruzione del segnale di partenza dato un segnale modulato ad una certa frequenza $f_c$, la prima cosa che facciamo è applicare un filtro passa banda alla frequenza $f_c$ al fine di eliminare i disturbi elettromagnetici. 
Poi, si moltiplica il segnale per $2cos(2\pi f_ct)$ per ottenere il segnale in banda base.
Infine, si applica un altro filtro, questa volta un bassa basso, per estrarre il segnale interessato. 

#Attenzione Quando si esegue la modulazione deve essere rispettata la condizione $f_c >> w$ altrimenti non possiamo distinguere i due segnali perchè vanno in sovrapposizione l’uno con l’altro. 

# Analog Quadrature Amplitude Modulation (QAM)

La Amplitude Modulation ha il vantaggio di essere molto semplice da implementare ma non è particolarmente efficiente perché la banda del segnale raddoppia. Ciò costituisce uno spreco di risorse perché il segnale, essendo simmetrico, potrebbe essere inviato solo in parte (ad esempio solo la parte destra considerando che la parte sinistra è uguale). 

Vediamo allora un altro tipo di modulazione chiamato **QAM** (acronimo di *Analog Quadrature Amplitude Modulator*). Ipotizziamo di voler inviare 2 segnali $m_1(t)$ ed $m_2(t)$. Moltiplichiamo il primo segnale per il coseno di una frequenza $f_c$ (*canale in fase*) e il secondo segnale per un *meno* seno (capiremo presto il perché di questo meno) sempre con frequenza $f_c$ (*canale in quadratura*). Infine, sfruttando il fatto che il coseno e il seno sono ortogonali, sommiamo i risultati in un unico segnale: $$s(t) = m_1(t)cos(2\pi f_c t) - m_2(t)sin(2\pi f_c t)$$che invieremo attraverso il canale di comunicazione.
Lato ricevente l’operazione di demodulazione si effettua nel seguente modo: 
- per ricavare $m_1(t)$ avendo $s(t)$ moltiplico per $2cos(2\pi f_c t)$ e poi applico un filtro in banda base.
- per ricavare $m_2(t)$ avendo $s(t)$ moltiplico per $-2sin(2\pi f_c t)$ e poi applico un filtro in banda base.

Vediamo i calcoli per ricavare il primo segnale ricordando che: $cos(\alpha) = \frac{1 + cos^2(2\alpha)}{2}$
e che $cos(\alpha)*sin(\alpha) = \frac{sin(2\alpha)}{2}$:
$$\begin{align*} v_I(t) &= s(t) 2cos(2\pi f_c t) =\\[4pt]
&= m_1(t)2cos^2(2 \pi f_c t) - m_2(t)sin(2 \pi f_c t)2cos(2\pi f_{c t})=\\[4pt]
&= m_1(t) + m_1(t)cos(2*2\pi f_c t) - m_2(t) sin(2 * 2 \pi f_c t)

\end{align*}$$
Poi, applicando un filtro in banda base, semplifico il seno e il coseno in modo da isolare solo $m_1(t)$.
La stessa cosa si può fare per il segnale in quadratura ricordando che $cos^2(\alpha) + sin^2(\alpha) = 1$:
$$\begin{align*}
v_Q(t) &= s(t)(-2)sin(2\pi f_c t)) =\\[4pt]
&= m_1(t)(-2)cos(2\pi f_c t)sin(2\pi f_c t) - m_2(t)(-2)sin^2(2\pi f_c t) =\\[4pt]
&= m_1(t) sin(2*2\pi f_c t) + 2m_2(t) - 2m_2(t)cos^2(2\pi f_c t) =\\[4pt]
&= m_1(t)sin(4\pi f_c t) + 2m_2(t) -m_2(t) -m_2(t)cos(2*2\pi f_c t)\\[4pt]
&= m_1(t)sin(4\pi f_c t) + m_2(t) - m_2(t)cos(4\pi f_c t)\\[4pt]
\end{align*}$$
Anche qui con un filtro in banda base preleviamo solo $m_2(t)$.

Siamo quindi riusciti, con la stessa quantità di banda, ad inviare due segnali invece che uno.
## Inviluppo Complesso

La notazione che abbiamo utilizzato può essere semplificata. Diamo delle definizioni:

> [!note] Segnale Passa Banda
> Un segnale si dice *segnale passa banda* se la sua *energia* è concentrata in una banda pari a $2B$ centrata in un intorno di $f_c \not= 0$ tale che $f_c >> 2B$.

Una proprietà importante di un segnale passa banda è che possiamo sempre rappresentarlo tramite il suo *inviluppo complesso* nel seguente modo: 
$$s(t) = Re\{\tilde{s}(t)e^{j2\pi f_c t}\}$$
> [!note] Inviluppo Complesso
> *L’inviluppo complesso* è una rappresentazione puramente matematica del segnale in banda passante e si scrive come $\tilde{s}(t)=s_I(t) + js_{Q(t)}= \tilde{s}_{Re}(t) + j\tilde{s}_{Im}(t)$. 

Sostituiamo la definizione di inviluppo complesso nella definizione di segnale passa banda ricordando che $e^{j2\pi f_c t} = cos(2\pi f_c t) + j sin(2\pi f_c t)$:
$$
\begin{align*}
s(t) &= Re\{\tilde{s}(t)e^{j2\pi f_c t}\} =\\[4pt]
&= Re\{(s_I(t) + js_Q(t))(cos(2\pi f_c t)+jsin(2\pi f_c t))\} =\\[4pt]
&= s_I(t)cos(2\pi f_c t) - s_Q(t)sin(2\pi f_c t)\\[4pt]
\end{align*}$$
Da questi calcoli abbiamo ottenuto esattamente la modulazione QAM e notiamo immediatamente il motivo per cui nella modulazione QAM mettevamo un meno davanti al seno. 

Per ogni segnale in banda passante esiste l’inviluppo complesso e quindi posso scriverlo in termini di $m_1(t)$ e $m_2(t)$.
#Attenzione 
$s_{QAM}(t)$ è un segnale *reale* perché $m_1(t)$ ed $m_2(t)$ sono reali e la somma di segnali reali è un segnale reale.
$\tilde{s} _{QAM}(t)$ non è un segnale reale ma *complesso* e quindi non è detto che goda della proprietà Hermitiana che gli garantirebbe la simmetria rispetto all’origine. (disegno sulle slide).
Quindi dobbiamo sempre ricordare che lavoriamo con astrazioni matematiche che, in quanto tali, possono essere complesse. Tuttavia, nel mondo fisico non esistono segnali complessi per cui questa per noi sarà solo una semplificazione di notazione col fine di agevolarci con la trattazione di segnali in banda passante.
In questo modo, utilizzando l’inviluppo complesso abbiamo il vantaggio di racchiudere tutte le informazioni che ci servono (i due segnali) in un'unica espressione. 

Gli inviluppi complessi delle modulazioni che abbiamo visto precedentemente sono:
- AM-DSB: 
	$s_{DSB}(t) = m(t)cos(2 \pi f_c t)$ quindi il suo inviluppo complesso sarà esattamente $\tilde{s}_{DSB}(t) = m(t)$ da cui ricavo: 
	$s_I(t) = m(t)$ 
	$s_Q(t) = 0$.
- QAM: 
		$s_{QAM}(t) = m_1(t)cos(2 \pi f_c t) - m_2(t)sin(2\pi f_c t)$ quindi il suo inviluppo complesso sarà esattamente $\tilde{s}_{QAM}(t) = m_1(t) + jm_2(t)$ da cui ricavo:
		$s_I(t) = Re\{\tilde{s}_{QAM}(t)\} = m_1(t)$
		$s_Q(t) = Im\{\tilde{s}_{QAM}(t)\}= m_2(t)$.

Vediamo perché abbiamo speso tutto sto tempo per una notazione:
1. Questa rappresentazione è fondamentale semplice da studiare visto che elimina l'effetto della carrier frequency (tradotto: frequenza portante ovvero $f_c$).
2. La simulazione di un segnale passa banda è meno computazionalmente impegnativa perché il sample rate è minore. Infatti, per Nyquist $\frac{1}{T} \ge 2B_s$ e nel caso di segnale passa banda avrei $B_s = f_c + W$ per cui il sample rate sarebbe molto elevato. Invece se si utilizza l’inviluppo complesso avrò $B_s = W$ e quindi $\frac{1}{T} > 2W$ molto più piccolo rispetto a prima. 

Nel disegno si vede che usare l’inviluppo complesso semplifica i disegni quindi lo useremo:

![[Modello con inviluppo complesso.png]]

# Frequency Modulation (FM)

Nelle modulazioni che abbiamo visto, prima della trasmissione vera e propria, è necessario *amplificare* il segnale. 
Come abbiamo visto ad esempio ad elettronica digitale l’amplificatore amplifica il segnale in modo *lineare* fino a raggiungere la *saturazione*. Se l'amplificatore va in saturazione il segnale viene disturbato (credo che il prof intendesse distorto). Per evitare ciò, si impone che il segnale non raggiunga mai il valore tale per cui l'amplificatore va in saturazione. La distanza tra il punto in cui inizia la saturazione e il valore massimo del segnale da amplificare si chiama *BackOff* (IBO). 
Il problema è che se il BackOff è grande avrò un amplificatore che non viene usato al massimo (ed è uno spreco perché l'amplificatore è uno degli elementi del sistema di comunicazione più costosi quindi andrebbe utilizzato al massimo delle sue potenzialità). 
![Amplificatore|center|300](https://www.researchgate.net/publication/337266039/figure/fig4/AS:1095929276968961@1638301309865/Amplitude-amplitude-transfer-characteristic-of-a-basic-nonlinear-high-power-amplifier.png)

Per risolvere questo problema, introduciamo un tipo di modulazione che invece di modulare in ampiezza lo fa *in fase*.

In questo tipo di modulazione il segnale $m(t)$ viene modulato nel seguente modo: $$s_{FM}(t) = cos\left(2\pi f_c t + 2 \pi k_f \int_{-\infty}^{t}m(\tau) d\tau\right)$$Domanda: che tipo di segnale è? 
È un segnale *passa banda* perché compare $f_c$ quindi ha un inviluppo complesso che vale: $$\tilde{s}_{FM}(t) = e^{j2\pi k_f \int_{-\infty}^{t}m(\tau)d\tau}$$Facciamo una riprova sostituendo l'inviluppo complesso nella definizione di segnale passa banda, se l'inviluppo che abbiamo scritto è corretto dovremmo trovare la definizione di $s_{FM}(t)$:
$$
\begin{align*}
s(t) &= Re\{\tilde{s}_{FM}(t) e^{j2\pi f_c t}\} = \\[4pt]
&= Re\{e^{j2\pi f_c t}e^{j2\pi k_f \int_{-\infty}^{t}m(\tau)d\tau}\} =\\[4pt]
&= Re\{e^{j(2\pi f_c t+ 2\pi k_f \int_{-\infty}^{t}m(\tau)d\tau)}\} =\\[4pt]
&= cos\left(2\pi f_c t + 2\pi k_f \int_{-infty}^{t}m(\tau)d\tau\right) = s_{FM}(t)\\[4pt]
\end{align*}$$
Proviamo ora a studiare la banda occupata dal segnale modulato in FM, *but, there is a but (cit. Moretti)* questa non si può calcolare con la trasformata di Fourier perché quest'ultima non ha una forma chiusa. Quindi ne cercheremo un’approssimazione. 

L'inviluppo complesso lo possiamo scrivere anche in questo modo: $$\tilde{s}_{FM}(t) = e^{j2\pi k_f \int_{-\infty}^{t}m(\tau)d\tau} = e^{j\tilde{\phi}(t)}$$Dalla fisica sappiamo che la frequenza è la derivata della fase quindi calcolo la derivata di $\phi(t)$ trovando: 
$$
\begin{align*}f_d(t) &= \frac{1}{2\pi}\frac{d}{dt}\phi(t) -f_c=\\[4pt]
&= \frac{1}{2\pi}\frac{d}{dt}\tilde{\phi}(t)=\\[4pt]
&=k_fm(t)\\[4pt]
\end{align*}$$

Definiamo la *frequency deviation* di un segnale come: $$f_d(t) = f_i(t) - f_c = k_f m(t)$$
#Attenzione da ora in poi avremo 2 tipi di frequenze, la prima sarà la frequenza istantanea $f_i(t)$ e la seconda sarà la frequency deviation $f_d(t)$, sono ovviamente correlate ma diverse. 
Definiamo la *frequenza massima* della frequency deviation come: $$\Delta f = max\{|f_d(t)|\} = k_f max\{|m(t)|\}$$definiamo infine *l'indice di modulazione* come: $$m_f = \frac{\Delta f}{B_m}$$Torniamo al calcolo della banda occupata dal segnale modulato, quella del segnale trasmesso $m(t)$ la conosco. 
L'approssimazione consiste nel concentrare tutta l’energia del segnale in un punto e considerare quindi $m(t)$ come un impulso di valore pari al valore massimo assunto dal segnale. 
Nel caso in cui $m(t) = V_mcos(2\pi f_m t)$ ho che $max\{|m(t)|\} = V_m$ e quindi ipotizzo che tutto il segnale sia concentrato in $V_m \delta(f)$ in corrispondenza di $f_m$ (la banda del segnale è $B_m = f_m$), che nel dominio del tempo diventa pari a $V_m$.
Calcolo $s_{FM}(t)$ applicando la definizione e risolvendo l’integrale: 
$$
\begin{align*}
s_{FM}(t) &= cos(2\pi f_c t +2\pi k_f\int_{-\infty}^{t}V_mcos(2\pi f_m \tau) d\tau)=\\[4pt]
&= cos(2\pi f_c t + 2\pi k_f V_m \frac{1}{2\pi f_m}sin(2\pi f_m t))=\\[4pt]
&= cos(2\pi f_c t + \frac{k_fV_m}{f_m}sin(2\pi f_m t)) =\\[4pt]
&= cos(2\pi f_c t + m_fsin(2\pi f_mt))\\[4pt]
\end{align*}$$
L’integrale lo risolvo solo per $t$ perché il termine in $t$ è l’unico che ci interessa. 
E ricavo facilmente anche l’inviluppo complesso come (i passaggi non ci sono perché sono identici a quelli che abbiamo fatto nell’inviluppo complesso di prima): $$\tilde{s}_{FM}(t) = e^{jm_fsin(2\pi f_m t)}$$Nell'immagine è rappresentato il risultato di questa tipologia di modulazione:

![[FM Modulation.png]]

Il primo segnale è un $cos(2\pi f_ct)$. Il secondo è $m(t)$ che quando è positivo allora $f_m$ *cresce* e quindi il terzo segnale che è quello modulato $s_{FM}(t)$ sarà più "denso". L'opposto accade per $m(t)$ negativo.
Si vede bene perchè si chiama **“frequency modulation”**, la fase cambia ma l’ampiezza rimane la stessa.

Ma torniamo alla domanda iniziale: quanto vale la banda del segnale trasmesso?
Ripartiamo dall'espressione che abbiamo trovato risolvendo l'integrale: $$s_{FM}(t) = cos(2\pi f_c t + m_fsin(2\pi f_mt))$$e dal suo inviluppo complesso: $$\tilde{s}_{FM}(t) = e^{jm_fsin(2\pi f_m t)}$$La caratteristica principale di questo segnale è che è *periodico* di periodo $T_m = \frac{1}{f_m}$. 

> [!note] Segnale Periodico
> Un segnale è **periodico** se $s(t) = s(t + kT_m)$ $\forall k \in Z$.

Quindi riscriviamo l'inviluppo complesso nel seguente modo:
$$\tilde{s}_{FM}(t+kT_m) =e^{jm_fsin(2\pi f_m (t+kT_m))}$$
Essendo il segnale periodico, possiamo usare la **trasformata serie di Fourier** per rappresentarlo: 
$$\tilde{s}_{FM}(t) = \sum_{n} \delta _n e^{j2\pi nf_m t}$$
In cui:
$$
\begin{align*}
\delta _n &= \frac{1}{T_m} \int_{-T_m/2}^{T_m/2} \tilde{s}_{FM}(t) e^{j2\pi nf_m t} dt =\\[4pt]
&= \frac{1}{T_m}\int_{-T_m/2}^{T_m/2} e^{jm_fsin(2\pi f_m t) e^{j2\pi n f_m t}} dt =\\[4pt]
&= \frac{1}{T_m}\int_{-T_m/2}^{T_m/2} e^{j(m_fsin(2\pi f_m t)-2\pi nf_mt)}dt\\[4pt]
&=\frac{1}{2\pi}\int_{-\pi}^{\pi}e^{jm_fsin(\vartheta)-n\vartheta}d\vartheta\\[4pt]
&= J_n(m_f)\\[4pt]
\end{align*}$$
Questo integrale non ha una forma chiusa ma può essere scritto come una **funzione Bessel** del primo tipo ordine (una funzione tabulata) fatta in questo modo:

![Bessel|center|500](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5d/Bessel_Functions_%281st_Kind%2C_n%3D0%2C1%2C2%29.svg/1920px-Bessel_Functions_%281st_Kind%2C_n%3D0%2C1%2C2%29.svg.png)

Più grande è $X$ e più devo considerare il contributo delle funzioni. 
Se $X=0$ allora posso considerare solo la funzione di ordine zero, cioé $J_0(X)$ (quella rossa) ma se $X \gg 0$ devo considerarle tutti i contributi dati dagli altri ordini (non ho capito molto bene questa parte).
Alla fine comunque mi basta sapere che $\delta _n = J_n(m_f)$ ovvero il coefficiente è pari alla funzione Bessel calcolata in $m_f$.

Il segnale sarà quindi: 
$$
\begin{align*}
s_{FM}(t) &= Re\{\tilde{s}_{FM}(t)e^{j2\pi f_c t}\} =\\[4pt]
&= \sum_{n}J_n(m_f)cos(2\pi(f_c+nf_m)t)\\[4pt]
\end{align*}$$
Che, nel dominio della frequenza, sarà $S_{FM}(f)$ fatta in questo modo:

![FM|center|500](https://rolandkuit.com/FM%20Tutorial/Pictures/FM%20Sidebands.jpg)

Noto che più termini ho e più lo spettro è esteso, quindi la banda dipenderà da $m_f$ e dalla distanza tra i picchi.
La distanza tra i picchi è pari a $\frac{1}{T_m} = f_m$ (questo lo sapevamo già da Comunicazioni Numeriche).
Quindi lo spettro dipenderà da $f_m$ e da $m_f$ il primo per la distanza tra i picchi e il secondo per il numero di picchi. 

Una buona approssimazione della banda si ottiene applicando la **regola di Carson** (non scenderemo nei dettagli):
$B_{FM} \simeq 2(m_f + 1)B = 2(\Delta f + B)$
Inoltre, si può dimostrare che la maggior parte dell’energia (il 98% circa) è concentrata nella banda trovata con la regola di Carson quindi questa è una buona approssimazione della banda.

In ambito commerciale si ha una $B_{FM} \approx 180kHz$ perché:
- $B = 2(2+5)= 15 kHz$ (non lo so da dove escono questi numeri)
- $\Delta f = 75kHz$
- $m_f = \frac{\Delta f}{B} = 5$
Quindi $B_{FM} = 2\cdot(1 + 5) \cdot 15 = 2 \cdot 6 \cdot 15 = 180 kHz$ (all’esame questi numeri li chiede).

In AM la banda occupata era $B_{AM} = 2B$, quindi in FM tecnicamente la banda occupata è maggiore. Il vantaggio di questa modulazione però sono due:
1. posso usare l’amplificatore molto vicino alla zona di saturazione perché amplificando in fase non ci sarà mai il rischio di entrare nella zona di saturazione dell'amplificatore. 
2. questa modulazione è più resistente al rumore. Vediamo perché andando a studiare la demodulazione lato ricevente. 

Lato ricevente il nostro obiettivo è quello di estrarre $m(t)$ dal segnale ricevuto:
$$\tilde{v}(t) = e^{j2\pi k_f \int_{-\infty}^{t}m(\tau)d\tau}$$
In generale, lato ricevente si effettua sempre l'operazione opposta rispetto a quella del trasmettitore quindi in questo caso dovrò effettuare *un'operazione di derivazione*:
$$\widehat{m}(t) = \frac{1}{2\pi k_f}\frac{d}{dt} \angle \tilde{v}(t)$$
Se ci fosse stata un'extra fase in questo modo: $$\tilde{v}(t) = e^{j2\pi k_f \int_{-\infty}^{t}m(\tau)d\tau + \varphi_0}$$l’operazione di derivata la andrebbe ad eliminare, ecco perché la modulazione FM è meno sensibile al rumore. 
Quest'ultima operazione, darebbe un fattore $2\pi k_f$ a causa dell'esponenziale, per eliminarlo ed ottenere solo $\widehat{m}(t)$ moltiplico per $\frac{1}{2\pi k_f}$.
Lo schema finale del ricevitore è il seguente: 

![[Ricevitore FM.png|center|600]]
