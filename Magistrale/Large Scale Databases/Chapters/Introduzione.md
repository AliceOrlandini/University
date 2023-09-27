# Big Data

Big Data è una parola utilizzata per descrivere molte cose ma innanzitutto rappresenta un insieme di dati e solo successivamente la tecnologia con cui vengono immagazzinati.
I servizi su internet generano dati *continuamente* ad *alta velocità* per cui i problemi ai quali andiamo incontro sono la raccolta (*fetch*), l'immagazzinamento (*store*) e l'elaborazione (*elaboration* ) dei dati stessi.

I dati vengono prodotti tramite varie fonti come:
- Social media e Networks
- Strumenti scientifici come quelli che raccolgono dati spaziali (satelliti)
- Transazioni (bancarie ad esempio)
- Sensori
Quindi conoscere le caratteristiche di diverse architetture di database è diventato fondamentale per molti ambiti.

## Definizione di Big Data: le 5V

Si definiscono Big Data i database aventi le seguenti caratteristiche:
1. **Volume**: grandi quantità di dati.
2. **Varietà**: formati diversi di dati provenienti o dalla stessa sorgente o da sorgenti diverse. Tali dati andranno "normalizzati".
3. **Velocità**: i dati possono essere generati con velocità diverse. In qualsiasi applicazione avremo infatti momenti di elevata e momenti di ridotta attività.
4. **Value**: i dati devono avere un certo valore (l'estrazione di tale valore può permetterci ad esempio di farci soldi sopra).
5. **Veracity**: veridicità, i dati acquisiti devono essere validati al fine di evitare inconsistenze, dati mancanti o dati malevoli. 

Col passare del tempo si sono aggiunte altre V alla definizione di Big Data:
6. **Validity**: qualità del dato.
7. **Variability**: eterogeneità.
8. **Venue**: provenienza da più sorgenti.
9. **Vocabulary**: dati accompagnati da documentazione.
10. **Vagueness**: evitare dati confusi.

Il tema del Big Data riguarda vari aspetti:
- **Acquisizione dei dati**: esempio con le API.
- **Sicurezza e Privacy**: dobbiamo stare ad esempio attenti a non infettare i sistemi.
- **Visualizzazione dei dati**: per le GUI o anche per l’analisi.
- **Analisi o Mining**: manipolazione dei dati per estrarre informazioni nascoste.
- **Storage**: come memorizzare al meglio i dati.
- **Querying**: estrazione dei dati.

## Come lavoriamo con una grande quantità di dati?

La prima cosa che dobbiamo chiederci è come possiamo lavorare con i big data, un primo modo è usare una CPU molto potente (*scale-up*) ma abbiamo diversi problemi:
- **costi**: utilizzare una CPU potente ha un costo molto elevato.
- **single point of failure**: se la CPU ha un malfunzionamento l'intero sistema collassa.
- **limite tecnologico**: le CPU non possono superare un certo limite superiore di prestazione a causa del limite imposto dalla tecnologia.
Un’altra soluzione è la *scale-out* ovvero la parallelizzazione di più CPU aventi le stesse capacità in modo da distribuire la computazione tutti i calcolatori. Tuttavia, un fattore da considerare che differenzia questa soluzione dalla precedente è che si avranno dati sparsi tra i calcolatori appartenenti all’infrastruttura. 
È infatti fondamentale è evitare trasferimenti tra nodi ma mantenere i dati in una sola memoria (*data locality*).
Ci possono essere tuttavia delle repliche del dato in diversi nodi (ne parleremo a Cloud Computing).

## MapReduce paradigma

> Muovere il risultato di una computazione è più economico che muovere computazione e dati allo stesso tempo.

Il MapReduce è un metodo di programmazione delle basi di dati introdotto da Google nel 2012.
Si dividono i dati in partizioni (*map*) che verranno elaborate in parallelo, poi si combinano i risultati della computazione in risultati aggregati (*reduce*). È quindi una specie di “divide et impera”.
### Esempio: 
Abbiamo 100 blocchi e vogliamo contare quanti blocchi esistono dello stesso colore, per effettuare la computazione abbiamo a disposizione 5 calcolatori. 
Dividiamo i blocchi in 5 gruppi da 20 blocchi ciascuno (uno per calcolatore) e compiamo la computazione. 
Infine, quando tutte le CPU hanno finito uniamo i loro risultati. 

Il formato dei dati è generalmente di tipo *“key”:”value”*, sia per gli input che per gli output.
Il vantaggio di questo paradigma risiede nel fatto che possiamo distribuire il carico di lavoro tra tutti i nodi della rete di calcolatori che saranno in numero sufficiente per la grandezza dei dati con cui si ha a che fare. Inoltre, i costi della scalabilità nel caso in cui il carico di lavoro aumentasse sono notevolmente ridotti perché è sufficiente aggiungere nuovi calcolatori alla rete. 

## Hadoop

Hadoop è un framework open source distribuito da Apache scritto in Java e composto da 4 moduli:
1. **Hadoop Common**: un insieme di funzioni di utilità per interagire con gli altri moduli.
2. **Hadoop Distributed File System**: HDFS, è un file system distribuito in cui ogni nodo è un pezzetto del sistema completo.
3. **Hadoop YARN**: un framework per la gestione delle risorse
4. **Hadoop MapReduce**: il modello di programmazione visto precedentemente ([[#MapReduce paradigma]])

### HDFS

Per la gestione del file system, molto efficiente per operazioni di lettura e scrittura, progettato per grandi quantità di dati.
È ottimizzato solo per **one-pass batch processing**.
La *multiple-pass batch* è invece la ripetizione di un ciclo produttivo. Essenzialmente nel momento in cui viene generato il risultato della computazione in questo modello si ricomincia dall’input. 
Hadoop non è ottimizzato per questo, il motivo è da ricercare proprio nel file system perché i dati, una volta finita la computazione, vengono rimossi dalla memoria. Se vogliamo quindi ripetere la computazione in loop bisogna ricaricare i dati in memoria e questa operazione è ovviamente inefficiente.

## Spark

Spark è un'alternativa ad Hadoop più veloce in caso di applicazioni online e iterative. Infatti, con Spark i dati vengono spostati dal filesystem ad una cache in cui vi permangono anche per più loop di elaborazione al fine di ridurre la latenza di accesso. 

Questo è lo schema di funzionamento di Spark:

![Spark_funzionamento](https://spark.apache.org/docs/latest/img/cluster-overview.png)

- **Driver Program**: prepara il task da eseguire, chiede al cluster manager un worker al quale assegnare tale task. Dopo che il cluster manager assegna un worker al task, il driver program comunicherà direttamente col worker.
- **Worker**: esegue i task con i dati presenti in cache, può comunicare anche con altri Workers.
- **Cluster Manager**: ha il compito di assegnare task agli worker.

Spark è compatibile con tutti i principali framework:

![Spark](https://www.zdnet.com/a/img/resize/d26dbe5b8b669db1ff5e77a09668c92b558e2757/2017/11/01/d17912b7-7717-4a71-9b09-573460970f94/sparkecosystem.png?auto=webp&width=1280)
### Resilient Distributed Dataset (RDD)

È l’astrazione principale offerta da Spark.

# Storia dei database

- 1950 - 1972: pre-relazionali
- 1972 - 2005: modello relazionale
- 2005 - 2015: prossima generazione di database (relazionali + noSQL)
> Un database è un’organizzazione ordinata di dati.

Il primo database mai inventato è l’enciclopedia in quanto i dati erano organizzati in ordine alfabetico. Oppure anche una libreria tecnicamente è un database. 

Sinceramente mi sembra inutile. Gli darò una lettura sulle slide e via.

### ACID Transaction

Una transazione ACID ha le seguenti caratteristiche:
1. **Atomic**: la transazione è indivisibile.
2. **Consistente**: il database deve rimanere consistente prima e dopo la transizione, non ci deve essere perdita di informazione in caso di errore nella transazione.
3. **Isolata**: la transazione non deve influenzare altre transazioni.
4. **Durable**: una volta che la transazione è stata salvata nel database i suoi effetti devono rimanere salvati anche in presenza di errori al sistema operativo o hardware.

> eventually: prima o poi
> possibly: eventualmente

