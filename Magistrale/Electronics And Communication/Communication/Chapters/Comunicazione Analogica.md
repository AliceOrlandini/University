
# Amplitude Modulation dual side band (AM-DSB)

La modulazione di ampiezza sfrutta la proprietà dei segnali *reali* di avere una trasformata di Fourier con *simmetria hermitiana*.
Ad esempio dato $m(t)$, esso avrà una $M(f)$ tale che: $$M(-f) = M^*(f)$$
Quello che possiamo fare è moltiplicare $m(t)$ per $cos(2\pi f_ct)$ al fine di ottenere il segnale modulato. Nel dominio della frequenza, questa operazione corrisponde a duplicare il segnale a destra e ha sinistra come in figura: 

![Modulazione | center | 500](https://electronicspost.com/wp-content/uploads/2020/05/1-4.png)

Il segnale modulato, nel dominio della frequenza sarà: $$S(f) = \frac{M(f - f_c)}{2} + \frac{M(f + f_c)}{2}$$
E la sua banda sarà il doppio della banda del segnale di partenza, quindi passerà da $w$ a $2w$.
Questo processo non è quindi efficiente in termini di consumo dello spettro ma ha il grande vantaggio di permetterci di realizzare il segnale in banda base (frequenze intorno allo zero) per poi andarlo a modulare alla frequenza che ci è stata assegnata subito prima dell’invio del segnale. 

Per quanto riguarda il processo inverso, ovvero la ricostruzione del segnale di partenza dato un segnale modulato ad una certa frequenza $f_c$; la prima cosa che facciamo è applicare un filtro passa banda alla frequenza $f_c$ al fine di eliminare i disturbi elettromagnetici. 
Poi, si moltiplica il segnale per $2cos(2\pi f_ct)$ per ottenere il segnale in banda base.
Infine, si applica un altro filtro, questa volta un bassa basso per estrarre il segnale interessato. 

#Attenzione Quando si esegue la modulazione deve essere rispettata la condizione $f_c >> w$ altrimenti non possiamo distinguere i due segnali perchè vanno in sovrapposizione l’uno con l’altro. 

# Analog Quadrature Amplitude Modulation (QAM)

La Amplitude Modulation ha il vantaggio di essere molto semplice da implementare ma non è particolarmente efficiente perché la banda del segnale raddoppia. Ciò costituisce uno spreco di risorse perché il segnale, essendo simmetrico, potrebbe essere inviato solo in parte (ad esempio solo la parte destra che tanto la sinistra è uguale). 

Vediamo allora un altro tipo di modulazione chiamato **QAM** (acronimo di *Analog Quadrature Amplitude Modulator*). Ipotizziamo di voler inviare 2 segnali $m_1(t)$ ed $m_2(t)$. Moltiplichiamo il primo segnale per il coseno di una frequenza $f_c$ (*canale in fase*) e il secondo segnale per un *meno* seno (capiremo presto il perché di questo meno) sempre con frequenza $f_c$ (*canale in quadratura*). Infine, sfruttando il fatto che il coseno e il seno sono ortogonali, sommiamo i risultati in un unico segnale: $$s(t) = m_1(t)cos(2\pi f_c t) - m_2(t)sin(2\pi f_c t)$$
da inviare attraverso il canale.
Lato ricevente l’operazione di demodulazione si effettua nel seguente modo: 
- per ricavare $m_1(t)$ avendo $s(t)$ moltiplico per $2cos(2\pi f_c t)$
- per ricavare $m_2(t)$ avendo $s(t)$ moltiplico per $-2sin(2\pi f_c t)$

Vediamo i calcoli del primo ricordando che: $cos(\alpha) = \frac{1 + cos^2(2\alpha)}{2}$
e che $cos(\alpha)*sin(\alpha) = \frac{sin(2\alpha)}{2}$:

$v_I(t) = s(t) 2cos(2\pi f_c t) =$
$= m_1(t)2cos^2(2 \pi f_c t) - m_2(t)sin(2 \pi f_c t)2cos(2\pi f_c t) =$
$= m_1(t) + m_1(t)cos(2*2\pi f_c t) - m_2(t) sin(2 * 2 \pi f_c t)$

Poi, applicando un filtro in banda base, semplifico il seno e il coseno in modo da isolare solo $m_1(t)$.
La stessa cosa si può fare per l’elemento in quadratura ricordando che $cos^2(\alpha) + sin^2(\alpha) = 1$:

$v_Q(t) = s(t)(-2)sin(2\pi f_c t)) =$
$= m_1(t)(-2)cos(2\pi f_c t)sin(2\pi f_c t) - m_2(t)(-2)sin^2(2\pi f_c t) =$
$= m_1(t) sin(2*2\pi f_c t) + 2m_2(t) - 2m_2(t)cos^2(2\pi f_c t) =$
$= m_1(t)sin(4\pi f_c t) + 2m_2(t) -m_2(t) -m_2(t)cos(2*2\pi f_c t)$
$= m_1(t)sin(4\pi f_c t) + m_2(t) - m_2(t)cos(4\pi f_c t)$

Anche qui con un filtro preleviamo solo $m_2(t)$.

Siamo quindi riusciti, con la stessa quantità di banda, ad inviare due segnali invece che uno.
## Inviluppo Complesso

La notazione che abbiamo utilizzato può essere semplificata. Diamo delle definizioni:
> Un segnale si dice *segnale passa banda* se la sua *energia* è concentrata in una banda pari a *2B* centrata in un intorno di $f_c$ diversa da zero e tale che $f_c >> 2B$.

Una proprietà importante di un segnale passa banda è che possiamo sempre rappresentarlo tramite il suo *inviluppo complesso*. 
> *L’inviluppo complesso* è una rappresentazione puramente matematica del segnale in banda base. 

Qui ci sono dei passaggi che fanno vedere come dall’inviluppo complesso si ricava la modulazione QAM.

Dai passaggi vediamo il perché di quel meno che mettevamo nella modulazione QAM. 
Per ogni segnale in passa banda esiste l’inviluppo complesso e quindi posso scriverlo in termini di $m_1(t)$ e $m_2(t)$.
$s_{QAM}$ è un numero reale perché $m_1(t)$ ed $m_2(t)$ sono reali.
$\tilde{s} _{QAM}(t)$ però non è reale ma complesso e non è simmetrico rispetto all’origine. (disegno sulle slide).
Ricordarsi: lavoriamo con astrazioni matematiche che possono essere complesse. Nel mondo fisico non esistono segnali complessi ma è molto utile per noi scrivere ciò per rappresentare i segnali in banda passante. 
L’inviluppo complesso ha il vantaggio di racchiudere tutte le informazioni che ci servono. 

Qual è l’inviluppo complesso della modulazione DSB? $s_{DSB}(t) = m(t)cos(2 \pi f_c t)$ il suo inviluppo complesso sarà esattamente $m(t)$.

Vediamo perché abbiamo speso tutto sto tempo per una notazione. 
1. Questa rappresentazione è fondamentale per ? non ho capito. (15:57) software define ratio.
2. Rimuove gli effetti della carrier frequency
3. Si si vuole simulare un segnale passa banda è meno computazionalmente impegnativo perché il sample rate è minore. Perché per Nyquist $\frac{1}{T} > 2B_s$ con $B_s = f_c + W$. Possiamo usare l’inviluppo complesso perché così demodulando vado nell’origine (in banda base) dove T sarà molto più piccolo perché $\frac{1}{T} > 2W$. 

Nel disegno si vede che usare l’inviluppo complesso semplifica i disegni quindi lo useremo.

# Frequency Modulation (FM)

Nelle modulazioni che abbiamo visto prima della trasmissione bisogna amplificare il segnale. 
L’amplificatore deve essere lineare (sennò mi sminchia il segnale) e il punto più alto è la saturazione. Se va in saturazione il segnale viene disturbato dall’amplificatore quindi avrò un Back Off ma se BakeOffo avrò un amplificatore che già di suo costa e nemmeno lo uso al massimo. Invece di Baking Off il segnale non amplifico in Ampiezza ma in Fase!


In questo tipo di modulazione $s_{FM}(t)cos(2\pi f_c t + 2 \pi k_f \int_{-\infty}^{t}m(\tau) d\tau)$
Che tipo di segnale è? È un segnale passa banda perché non c’è la tilde (lol) e perché compare $f_c$.
Frequenza è la derivata della fase in fisica quindi svolgo la derivata trovando $f_d(t)$ sulle slide.
Ricaviamo anche l’inviluppo complesso (su OneNote).
Voglio studiare la banda occupata dal segnale mudulato in FM: non si può calcolare con Fourier perché non ha soluzione (non ha una forma chiusa) quindi ne cercheremo un’approssimazione. 
but, there is a but. 

Definiamo la frequency deviation: $f_d(t) = f_i(t) - f_c = k_f m(t)$
#Attenzione da ora in poi avremo 2 tipi di frequenze, la prima sarà la frequenza istantanea $f_i(t)$ e la seconda la $f_d(t)$, sono ovviamente correlate ma diverse. 
Definiamo la frequenza massima della frequenza deviation: $\Delta f = max{|f_d(t)|} = k_f max{|m(t)|}$
 definiamo poi l’indice di modulazione come: $m_f = \frac{\Delta f}{B_m}$
 
Quindi il massimo fratto la banda del segnale modulato. 
Ora voglio sapere la banda occupata dal segnale modulato, quella del segnale trasmesso la conosco ma essendo dentro un integrale che è dentro un coseno è difficile da calcolare (via ci siam capiti).
Approssimazione: concentro tutta l’energia in un punto quindi invio un segnale che è un impulso di potenza. $V_m \delta(f)$ che nel tempo diventa solo $V_m$.
Calcolo $s_{FM}$ applicando la definizione e risolvendo l’integrale.
Calcoli su OneNote. 
Di conseguenza trovo anche l’inviluppo complesso. 

 