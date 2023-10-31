# Sommario

**Titolo:** *L'Importanza della Privacy nell'Uso dei Sensori per le Sperimentazioni*

**Introduzione**

Nell'era digitale, l'uso sempre più diffuso di sensori in sperimentazioni scientifiche e applicazioni quotidiane ha aperto nuove frontiere per l'acquisizione di dati. Sebbene questa evoluzione tecnologica abbia portato numerosi vantaggi, è anche emersa una preoccupazione cruciale: la privacy. L'uso di sensori in contesti di sperimentazione richiede una riflessione approfondita sull'equilibrio tra l'innovazione tecnologica e la tutela della privacy individuale.

**Il Ruolo dei Sensori nelle Sperimentazioni**

I sensori sono strumenti essenziali nelle sperimentazioni scientifiche, consentendo la raccolta di dati in tempo reale, l'automazione dei processi e la precisione delle misurazioni. Sono impiegati in una vasta gamma di settori, dall'ambito medico alla ricerca ambientale, dall'automazione industriale all'Internet delle cose (IoT).

**La Privacy come Priorità**

L'importanza della privacy nell'uso dei sensori non può essere sottovalutata. La crescente quantità di dati personali raccolti dai sensori solleva questioni etiche e legali che richiedono una risposta ponderata. Gli individui hanno il diritto fondamentale alla privacy, e questo diritto non dovrebbe mai essere sacrificato sull'altare dell'innovazione tecnologica.

**Le Minacce alla Privacy**

Le sperimentazioni che coinvolgono sensori presentano alcune minacce alla privacy che devono essere affrontate con attenzione. Queste minacce includono:

1. **Raccolta di Dati Sensibili:** I sensori possono raccogliere dati altamente sensibili, come informazioni mediche o comportamentali, che potrebbero essere utilizzate in modo improprio se non gestiti correttamente.

2. **Vulnerabilità dei Dati:** La trasmissione e l'archiviazione dei dati dai sensori possono essere vulnerabili agli attacchi informatici, mettendo a rischio la sicurezza e la privacy.

3. **Tracking Costante:** Alcuni sensori, come quelli nell'ambito dell'IoT, consentono un monitoraggio costante delle persone, sollevando preoccupazioni sulla sorveglianza invasiva.

**Soluzioni per Proteggere la Privacy**

Per affrontare queste sfide, è necessario adottare misure proattive per proteggere la privacy nell'uso dei sensori nelle sperimentazioni. Queste misure includono:

1. **Consentimento Informato:** Gli individui devono essere adeguatamente informati sui dati che verranno raccolti e devono dare il loro consenso esplicito prima dell'uso dei sensori.

2. **Anonimizzazione dei Dati:** I dati raccolti dovrebbero essere anonimizzati o pseudonimizzati per proteggere l'identità degli individui.

3. **Sicurezza dei Dati:** Dovrebbero essere implementate robuste misure di sicurezza per proteggere i dati dai sensori da accessi non autorizzati.

4. **Leggi e Normative:** Le leggi sulla privacy dovrebbero essere aggiornate per affrontare le sfide poste dai sensori e garantire la responsabilità delle organizzazioni che li utilizzano.

**Conclusioni**

La privacy nell'uso dei sensori per le sperimentazioni è una questione di fondamentale importanza. Mentre i sensori continuano a migliorare la nostra comprensione del mondo, è cruciale che la privacy individuale sia protetta. Solo attraverso un equilibrio tra l'innovazione tecnologica e la tutela della privacy possiamo sfruttare appieno il potenziale dei sensori senza compromettere i diritti fondamentali degli individui.

[Garante della provacy](https://www.garanteprivacy.it/home/docweb/-/docweb-display/docweb/3616387#:~:text=L´idea%20di%20privacy,forse%20assumere%20decisioni%20più%20consapevoli.|Garante della privacy)

Nel primo capitolo della tesi, verrà presentata una panoramica approfondita sulla storia e sul funzionamento tecnico del sensore adottato negli esperimenti, noto come LiDAR. Il LiDAR, acronimo di "Light Detection and Ranging," ha una storia che affonda le radici nel XX secolo e ha attraversato un'evoluzione significativa, estendendo il suo utilizzo dalle applicazioni militari e aeronautiche a un vasto spettro di campi, tra cui geoscienze, topografia, ingegneria e persino l'industria automobilistica per sistemi di guida autonoma.

Per quanto riguarda il funzionamento tecnico del LiDAR, il sistema è composto principalmente da un laser che emette impulsi di luce, un ricevitore che misura il tempo di ritorno di tali impulsi e un sistema di posizionamento che consente di registrare la posizione spaziale dei dati raccolti. Questi dati vengono elaborati per creare una nuvola di punti tridimensionale che rappresenta in modo dettagliato la superficie o gli oggetti rilevati.

Infine, va sottolineato che la decisione di impiegare il LiDAR in questa ricerca è motivata da diverse ragioni. Tra queste, l'alta precisione e risoluzione spaziale offerte dalla tecnologia, nonché la sua capacità di acquisire dati rapidamente, sono fondamentali per il successo della ricerca. Inoltre, la capacità del LiDAR di fornire dati tridimensionali dettagliati è cruciale per gli obiettivi specifici della tesi, offrendo un livello di precisione che altre tecnologie potrebbero non garantire.

# LiDAR

## Domanda 1

Il LiDAR, acronimo di "Light Detection and Ranging" (o "Laser Imaging, Detection, and Ranging"), è una tecnologia di rilevamento remoto che utilizza impulsi laser per misurare la distanza tra un sensore LiDAR e un oggetto o una superficie bersaglio. Questa tecnologia è ampiamente utilizzata in una varietà di applicazioni, tra cui la cartografia, la geologia, l'automazione industriale, la robotica, la guida autonoma e molti altri campi. Ecco come funziona il LiDAR in dettaglio:

1. **Generazione del laser**: Il sistema LiDAR inizia emettendo impulsi laser invisibili nella regione infrarossa o vicino infrarossa. Questi laser generano un fascio di luce coerente e direzionato verso il bersaglio.

2. **Emissione del laser**: Il laser emette impulsi di luce in rapida successione verso l'ambiente circostante. La frequenza di emissione degli impulsi è molto elevata, spesso nell'ordine dei milioni di impulsi al secondo.

3. **Riflessione del laser**: Quando il fascio laser colpisce un oggetto o una superficie, una parte della luce viene riflessa indietro verso il sensore LiDAR.

4. **Rilevamento della luce riflessa**: Il sensore LiDAR rileva i ritorni luminosi (o echi) dalla luce riflessa e misura il tempo impiegato dall'impulso laser per tornare al sensore. Questo tempo di volo (time-of-flight) è una misura diretta della distanza tra il sensore e l'oggetto.

5. **Calcolo della distanza**: Utilizzando la velocità della luce come costante, il sistema LiDAR calcola la distanza tra il sensore e l'oggetto bersaglio. Poiché gli impulsi laser sono emessi in continuazione, il sistema può generare una serie di punti di dati 3D che rappresentano la forma e la distanza degli oggetti nell'ambiente circostante.

6. **Scansione dell'ambiente**: Per ottenere una visione completa dell'ambiente, i sistemi LiDAR eseguono una scansione dell'area circostante, ruotando o inclinando il sensore o utilizzando un dispositivo a specchio mobile per coprire un'ampia area. Questa scansione crea un "nuvola di punti" 3D, che è un insieme di coordinate XYZ che rappresentano la geometria degli oggetti circostanti.

7. **Elaborazione dei dati**: I dati raccolti vengono poi elaborati da un software dedicato, che può essere utilizzato per creare mappe 3D dell'ambiente, riconoscere oggetti, calcolare distanze, estrarre informazioni topografiche o alimentare algoritmi di controllo per veicoli autonomi.

Le applicazioni del LiDAR sono varie, dall'automotive, in cui viene utilizzato per la percezione ambientale e la guida autonoma, all'ingegneria civile, in cui può essere utilizzato per la progettazione e il monitoraggio delle infrastrutture. La precisione e la velocità del LiDAR lo rendono uno strumento prezioso in molti settori in cui la comprensione dettagliata dell'ambiente è fondamentale.
## Medium

[The LiDAR Revolution](https://medium.com/predict/the-lidar-revolution-52e888a0ce41)
[3D scanning with iPhone LiDAR using Dot3D](https://medium.com/@KentaItakura/3d-scanning-with-iphone-lidar-using-dot3d-65445674c280)
[Creating a Point Cloud Dataset for 3D Deep Learning](https://medium.com/@kidargueta/creating-a-point-cloud-lidar-data-dataset-for-3d-deep-learning-61684b1fc043)
[LiDAR-Camera Fusion: A Beginner’s Guide](https://medium.com/@shashankag14/lidar-camera-fusion-a-short-guide-34115a3055da)
[Technical discussions about Point Cloud Data](https://medium.com/@OttoYu/technical-discussions-about-point-cloud-data-63ef3a285982)
[What I’ve learned after 100 days of 3D Scans with the iPhone 12 Pro LiDAR](https://manuvision.medium.com/what-ive-learned-after-100-days-of-3d-scans-with-the-iphone-12-pro-lidar-c6bd31038f4d)
[How to Voxelize Meshes and Point Clouds in Python](https://towardsdatascience.com/how-to-voxelize-meshes-and-point-clouds-in-python-ca94d403f81d)

## Introduzione

[What is LiDAR](https://www.synopsys.com/glossary/what-is-lidar.html)
[LiDAR Story](https://www.blickfeld.com/blog/the-beginnings-of-lidar/)
[IoT e GDPR](https://www.cybersecurity360.it/legal/privacy-dati-personali/iot-e-gdpr-come-conciliare-linnovazione-con-la-sicurezza-dei-dati/)
# Brain Storming

- Mettere anche gli screen dell'app per mostrare il funzionamento
- Commentare i pezzi di codice
- Slide:
	- “Introduzione al problema”
	- “Soluzioni”
	- “Risultati”


- **ABSTRACT**: e' un riassunto di tutto il tuo lavoro in una singola pagina, con focus sul problema affrontato e sulle motivazioni per l'approccio scelto e le considerazioni sui risultati ottenuti (e' stato efficace e se si quanto?).
- **INTRODUZIONE E MOTIVAZIONE**: qui devi spiegare a che serve una analisi come quella svolta, che vantaggi offre, perché farla, a chi interessa. 
- **RELATED WORKS**: qui devi riferire qualche lavoro in letteratura per spiegare come mai usi un determinato approccio. In questo caso ad esempio una possibile ragione e' il machine learning, al contrario un approccio classico (statistiche con soglie dettate dan un esperto del dominio applicativo) e' difficilmente generalizzabile e scalabile: ogni caso di studio o applicazione serve un nuovo esperto e un nuovo studio. A fine di questo capitolo devono essere evidenti delle ragioni per cercare un approccio diverso o migliore (che e' praticamente quello che proponi nella tua tesi) e che vantaggi ipotizzi che possa offrire rispetto a quanto sopra descritto.
- **DESIGN E IMPLEMENTAZIONE**: sulla base delle carenze degli approcci in letteratura evidenziati nei related works devi proporre una tua soluzione e la sua implementazione, presentando tutte le esigenze a cui questa deve rispondere. Qui puoi inserire prima una ricca descrizione del tuo approccio e del core della sua implementazione, poi gli USE CASE del tuo software (senza pero' dei riferimenti alle funzioni che chiamano). Potrebbe seguire una descrizione delle classi implementate...meglio se con diagramma delle classi, guarda le slide in allegato con particolare riferimento a come impostare la presentazione di uno USE CAS.
- **CASE STUDY**: qui presenti i dati che hai utilizzato nel tuo studio, di cosa trattano (es. persone che salgono/scendono dai taxi nell'hotspot D di Manhattan), dove si trova l'hotspot, che divisione in comportamenti che hai adottato (es. cluster 0=lavorativi, 1=prefestivi e 2=festivi) e perche proprio questa divisione, come si distinguono, in cosa cambiano e perche'.
- **RISULTATI SPERIMENTALI**: qui dobbiamo accompagnare il lettore in un "viaggio" nell'analisi che hai svolto. Parti dalla spiegazione di che metriche/grafici hai usato per misurare l'efficacia del tuo sistema. A questo seguiranno i risultati ottenuti con le varie metriche/grafici presentati prima, variare dei suoi parametri e dei csi di studio (es. dei cluster).
- **CONCLUSIONI**: riassumi cio' che hai fatto, metti in evidenza l'efficacia del tuo sistema con la parametrizzazione migliore (se necessario rimetti anche il risultato stesso relativo solo a questo caso). E' importante che qui mostri che quanto avevamo ipotizzato nei primi capitoli e' stato dimostrato sperimentalmente. Infine (ma e' la parte piu importante), devi interpretare i tuoi risultati: come mai possiamo (o non possiamo) ritenere soddisfatte le ipotesi presentate nei primi capitoli sulla base dei nostri esperimenti? che valore offrono questi risultati se forniti ad uno (*).
- **APPENDICE A**: manuale d'uso del software con la descrizione di tutti i metodi, e il codice prodotto