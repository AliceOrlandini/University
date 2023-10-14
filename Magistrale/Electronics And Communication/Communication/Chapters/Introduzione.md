Iniziamo il corso parlando di alcuni dati statistici generati dall’ITU (International Telecomunication Union) che evidenziano come il numero di comunicazioni wireless, con l’approdo della telefonia mobile, abbia superato di gran lunga il numero di comunicazioni via cavo. 
Per questo motivo, questo corso si focalizzerà sulle comunicazioni Wireless, studieremo il funzionamento del canale di comunicazione e dei modi in cui si possono migliorare le sue performance.

## Lo spettro radio

> Lo *spettro radio* è una parte dello *spettro elettromagnetico* utilizzata per le comunicazioni wireless (senza fili), come, per esempio, la trasmissione di dati attraverso reti *Wi-Fi* o in *banda larga mobile* tramite reti cellulari.

Lo spettro negli anni è stato partizionato da degli enti per evitare sovrapposizioni nelle trasmissioni. Per esempio, lo spettro Americano è stato diviso in blocchi di frequenze secondo il seguente schema: 

![Spettro USA|center|700](https://www.beautifulpublicdata.com/content/images/size/w1600/2023/02/january_2016_spectrum_wall_chart_0.jpg)

Si nota che più le frequenze aumentano in termini di unità di misura più si possono aggiungere blocchi di banda perché più si aumenta più si ha disponibilità di allocazione delle risorse.
Potremmo chiederci quale sia il limite maggiore di frequenze assegnabili e la risposta è che questo limite è imposto solo dalla tecnologia di cui siamo in possesso. 
Per esempio, 50 anni fa era impensabile che avremmo raggiunto i GHz ma, tramite il progresso tecnologico, siamo riusciti ad arrivare a queste frequenze. 
Dobbiamo comunque sempre ricordarci che lo spettro più è limitato e più è costoso per cui dobbiamo usarlo al meglio sfruttandolo al massimo.

A seconda del range frequenziale nel quale ci troviamo, è stato assegnato un nome:

| Banda | Range        | Lunghezza d’onda        | Servizi               | Propagazione     |
| ----- | ------------ | ----------------------- | --------------------- | ---------------- |
| LF (Low)    | 30 - 300 kHz | $10^4$ - $10^3$ m       | Per usi militari      | Via Terra        |
| MF (Medium)   | 0.3 - 3 MHz  | $10^3$ - $10^2$ m       | AM radio              | Via Terra e Aria |
| HF (High)   | 3 - 30 MHz   | $10^2$ - 10 m           | Aviazione             | Via Aria         |
| VHF (Very High)  | 30 - 300 MHz | $10$ - $1$ m            | FM radio              | Via Spazio       |
| UHF (Ultra High)  | 0.3 - 3 GHz  | $1$ - $10^{-1}$ m       | Wi-Fi, GPS, Cellulare | Via Spazio       |
| SHF (Super High)  | 3 - 30 GHz   | $10^{-1}$ - $10^{-2}$ m | Wi-Fi, 5G             | Via Spazio       |
| EHF (Extremely High)  | 30 - 300 GHz | $10^{-2}$ - $10^{-3}$ m | Wi-Fi, 5G, Radar      | Via Spazio       |

![Servizi di telecomunicazione in base alla banda|center|700](https://digitalregulation.org/wp-content/uploads/word-image-141.png)

Questa tabella è bene ricordarsela perché succede spesso che all’esame venga chiesto quale range frequenziale stiamo trattando.

Notiamo che c’è una proporzionalità tra la frequenza e la lunghezza d’onda, in particolare: $$\lambda = \frac{c}{f}$$con $c = 3 * 10^8$ m/s la velocità della luce. Ed ecco perché la banda è stata divisa per multipli di 3, in questo modo infatti si semplificano i calcoli. 