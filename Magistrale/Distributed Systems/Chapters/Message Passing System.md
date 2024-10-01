### Modelli per sistemi distribuiti

#### Shared Memory e Message Passing

I principali modelli di **scambio di messaggi** nei sistemi distribuiti sono:
- **Shared Memory**: in questo modello, ogni CPU ha la propria *cache locale*, ma tutte le unità condividono una memoria centrale comune. La comunicazione tra le unità avviene indirettamente attraverso questa memoria condivisa. Le CPU accedono alla memoria condivisa tramite un **bus** comune, permettendo lo scambio di messaggi e dati tra i processi. Tuttavia, è necessario gestire la sincronizzazione per evitare problemi come i data races.
- **Message Passing**: in questo modello, ogni nodo è indipendente e ha la propria *memoria locale*. La comunicazione tra le unità avviene inviando e ricevendo messaggi attraverso un **network di interconnessione**. Questo modello non richiede memoria condivisa e ogni unità comunica esplicitamente con le altre attraverso messaggi, il che riduce i problemi di sincronizzazione e offre maggiore scalabilità, soprattutto in sistemi distribuiti su larga scala.

![[Shared Memory to Msg Passing.webp|center|400]]

#### Canali di interconnessione

Concentrandoci sul secondo tipo di **passaggio di messaggi**, possiamo realizzarlo attraverso due tipi di reti di comunicazione:
- **Grafo completamente connesso**: in questo modello, ogni unità è direttamente collegata a tutte le altre tramite archi. Ciò significa che ogni nodo può comunicare direttamente con ogni altro nodo, senza passare attraverso intermediari.
- **Grafo parzialmente connesso**: in questo caso, non tutte le unità sono collegate direttamente tra loro. Alcune comunicazioni devono passare attraverso altri nodi intermedi, il che può ridurre il numero di connessioni fisiche necessarie, ma potrebbe introdurre latenze e richiedere meccanismi più complessi per instradare i messaggi.

![[Channels.png|center|400]]

In entrambi i modelli, ogni arco può essere **unidirezionale** o **bidirezionale**, a seconda che i messaggi possano viaggiare in una sola direzione o in entrambe.

Un altro aspetto da considerare è l'ordine in cui vengono recapitati i messaggi. In alcuni sistemi, i messaggi seguono un ordine **FIFO**, cioè i messaggi inviati per primi vengono ricevuti per primi. In altri sistemi, questo ordine potrebbe non essere garantito, e i messaggi potrebbero arrivare fuori sequenza, richiedendo ulteriori meccanismi per garantire la coerenza della comunicazione.

#### 

### Addressing dei Processi

### Semantica della Send e Receive

