### Quali sono le motivazioni che hanno portato ad adottare IPv6?

Il principale motivo che ha portato alla migrazione da IPv4 a IPv6 è stato il *limitato spazio di indirizzamento* di IPv4. Nel 1994, l'IETF (Internet Engineering Task Force) aveva previsto l'esaurimento degli indirizzi IPv4 tra il 2005 e il 2011, cosa che effettivamente si è verificata. Per risolvere questo problema, è stato sviluppato un nuovo standard, **IPv6**, che utilizza indirizzi di *128 bit*, garantendo così uno spazio di indirizzamento praticamente inesauribile per il futuro.

Oltre a risolvere il problema dello spazio di indirizzi, IPv6 ha affrontato alcune criticità presenti in IPv4. Tra queste, la rimozione della lunghezza variabile dell'header, che semplifica il processamento dei pacchetti. Inoltre, con IPv6 non è più necessario l'uso del NAT (Network Address Translation), poiché è possibile assegnare a ogni utente un blocco di indirizzi /64, garantendo un numero sufficiente di indirizzi per ogni dispositivo connesso.

Un'altra innovazione di IPv6 è stata l'eliminazione del protocollo ARP (Address Resolution Protocol), sostituito dal Neighbor Discovery Protocol (NDP), che è più efficiente. Inoltre, IPv6 offre un miglior supporto per le opzioni e le estensioni, consentendo una maggiore flessibilità e scalabilità per nuove funzionalità.

IPv6 è diventato uno standard ufficiale nel 2017 e, sebbene la sua adozione sia ancora in corso, sta gradualmente diffondendosi in tutto il mondo. Attualmente, circa il 45% degli utenti globali utilizza IPv6 per accedere ai servizi Google, segno di una crescente adozione di questo nuovo protocollo.

### Da quali campi è composto il Base Header di IPv6?

Ecco i cambiamenti più significativi nel passaggio da IPv4 a IPv6:
1. **Indirizzamento esteso**: la dimensione dell'indirizzo IP è passata da 32 bit a 128 bit, diventando praticamente inesauribili.
2. **Intestazione ottimizzata di 40 byte**: alcuni campi di IPv4 erano opzionali e ciò rendeva l'intestazione di dimensione variabile. In IPv6 invece l'intestazione è fissa a 40 byte consentendo una più rapida elaborazione da parte dei router. Le opzioni sono state codificate in un modo più flessibile usando gli extension header.
3. **Etichettatura dei flussi**: ad esempio la trasmissione audio e video potrebbe essere trattata come un flusso, in questo modo in IPv6 si può differenziare il traffico di rete. 

Il base header di IPv6 è composto dai seguenti campi:
1. **Versione** (4 bit): Indica la versione del protocollo Internet, che per IPv6 è 6.
2. **Classe di Traffico** (8 bit):  Simile al campo TOS di IPv4, viene usato per indicare la priorità e la gestione del traffico (QoS).
3. **Flow Label (etichetta di flusso)** (20 bit): Si tratta di un numero generato casualmente usato per identificare un flusso di datagrammi appartenenti a una stessa sessione o flusso. Senza questo campo i pacchetti venivano identificati come appartenenti allo stesso flusso in base all'indirizzo sorgente e destinazione, ora abbiamo un modo più esplicito e veloce per fare ciò. Da notare inoltre che il vecchio metodo con il NAT non era praticabile in quanto l'indirizzo sorgente viene mascherato dal NAT.
4. **Lunghezza del Payload** (16 bit): Indica la dimensione del payload (dati) che segue l'header.
5. **Next Header** (8 bit): Specifica il tipo di header successivo, che può essere un header di trasporto (es. TCP, UDP) o un header di estensione.
6. **Hop Limit** (8 bit): Indica il numero massimo di salti che il pacchetto può attraversare prima di essere scartato. Ogni volta che il datagramma raggiunge un router, il valore di questo campo viene decrementato di 1 fino a quando non arriva a zero, momento in cui viene scartato. Lo scopo è quello di non lasciare datagrammi che vagano senza meta per l'Internet.
7. **Source Address** (128 bit): L'indirizzo IPv6 del mittente.
8. **Destination Address** (128 bit): L'indirizzo IPv6 del destinatario.

Confrontando il formato IPv6 con IPv4 notiamo che sono stati tolti alcuni campi: 
- **Frammentazione/Riassemblaggio**: IPv6 non consente frammentazione né riassemblaggio nei router intermedi, queste operazioni devono essere effettuate solo dal sorgente e solo dal destinatario. Il motivo di tale scelta è che le operazioni di frammentazione e riassemblaggio consumano tempo e demandarle ai router dei sistemi periferici è molto più efficiente. In IPv6 infatti un pacchetto che supera l'MTU (maximum trasmission unit) del router viene scartato.
- **Checksum di intestazione**: Considerando che i protocolli a livello di trasporto (come TCP) calcolano il loro checksum, i progettisti di IPv6 hanno ritenuto ridondante il checksum sull'header del datagramma. Ancora una volta la principale preoccupazione è stata la veloce elaborazione del datagramma nei router. 
- **Opzioni**: Si è trovato un modo più efficiente di aggiungere opzioni usando gli extension header. 

![IPv6|center|400](https://www.networkacademy.io/sites/default/files/inline-images/comparing-ipv4-and-ipv6-headers.png)


### Oltre al base header da cosa è composto un datagramma IPv6?

Un datagramma IPv6 è costituito da un header di base di 40 byte, seguito da zero o più header di estensione di dimensione variabile. Gli header di estensione vengono elaborati nell'ordine in cui appaiono all'interno del datagramma.

![Extension Header|center|400](https://lh4.googleusercontent.com/proxy/hxQ2VNajIhduFByP3T3G14Ekf0c8BB1eyIm1exQXBBIQhiyodxSZqpgq5Iy6AWGSN_IjeHjYZnTTz5P8_LSymL9o3qDcwwnIrpr_YzU)



### Quali sono i possibili valori del campo next header?

Il campo **Next Header** del base header di IPv6 contiene al suo interno le informazioni riguardo al tipo di dati contenuti nel prossimo header. 
Esso può assumere diversi valori, a seconda del protocollo o del tipo di header che segue l'header IPv6 di base. 
Ecco alcuni dei valori più comuni:
1. **6**: TCP (Transmission Control Protocol)
2. **17**: UDP (User Datagram Protocol)
3. **1**: ICMPv4 (Internet Control Message Protocol for IPv4)
4. **58**: ICMPv6 (Internet Control Message Protocol for IPv6)
5. **43**: Routing Header (per indirizzare il pacchetto attraverso uno specifico percorso)
6. **44**: Fragment Header (per la frammentazione dei pacchetti IPv6)
7. **50**: ESP (Encapsulating Security Payload) - usato per IPsec
8. **51**: AH (Authentication Header) - usato per IPsec
9. **59**: No Next Header (indica che non ci sono altri header successivi)
10. **60**: Destination Options Header (usato per informazioni aggiuntive specifiche del destinatario)

Oltre al campo che specifica il tipo del prossimo header, ogni header di estensione include un campo denominato **Header Extension Length**. Questo campo indica la lunghezza dell'header di estensione corrente, permettendo di determinare la posizione del prossimo header da processare.

Nel caso in cui il campo Next Header indichi che il prossimo header è un altro pacchetto IPv6, si parla di *"tunneling"*. Il tunneling consente di incapsulare un datagramma IPv6 all'interno di un altro datagramma IPv6.

### A cosa serve l'hop-by-hop options header?

L'**Hop-by-Hop Options Header** di IPv6 serve per trasportare informazioni che devono essere elaborate da ogni router lungo il percorso del pacchetto, non solo dal destinatario finale. Se il datagramma ha più header questo dovrà essere il primo, questo perché visto che deve essere elaborato da ogni router in questo modo è più facile trovarlo e processarlo.

Questo header è composto da due sezioni principali: la parte *Option Type* e la parte *Option Data*.

La sezione *Option Type* si suddivide in tre campi:
1. **Action**: Definisce come i nodi che non riconoscono l'opzione devono trattare il pacchetto. (ad esempio: 01 scarta il pacchetto)
2. **C**: Indica se l'opzione può essere modificata durante il transito del pacchetto.
3. **Type**: Specifica il tipo di opzione presente nell'header.

Esempi di tipi di opzione includono:
- **Jumbo Payload (type = 194)**: Consente di inviare pacchetti con un payload superiore a 65.535 byte, utile per trasferimenti di dati molto grandi. Quando viene impostato il campo payload del pacchetto IPv6 è impostato a zero byte ed il payload viene messo nel payload dell'header. 
- **Router Alert (type = 5)**: Informa i router lungo il percorso che devono ispezionare il pacchetto, utilizzato da protocolli come RSVP (Resource Reservation Protocol) per la gestione delle risorse di rete.

Poiché ogni router deve elaborare questo header, il suo uso può introdurre un overhead e potenzialmente rallentare il routing, per cui viene utilizzato con parsimonia e solo per casi specifici.

### A cosa serve il routing header?

Il **Routing Header** in IPv6 è utilizzato per specificare un percorso preciso che un pacchetto deve seguire per raggiungere la destinazione. In pratica, consente di indicare una o più tappe (nodi intermedi) che il pacchetto deve attraversare lungo il suo percorso verso la destinazione finale. Questo processo è chiamato **source routing**.

Il **Routing Header** è composto dai seguenti campi:
- **Routing type**: Specifica il tipo di routing da utilizzare (0: default, 2: Mobile IPv6, 3: RPL).
- **Segment left**: Indica il numero di nodi intermedi rimanenti che il pacchetto deve ancora attraversare.
- **Address**: Elenca gli indirizzi IPv6 dei nodi intermedi da attraversare.

Durante l'elaborazione del pacchetto, i router precedenti non vengono rimossi dall'header, consentendo al destinatario finale di ricostruire il percorso seguito dal pacchetto.

Tuttavia, l'utilizzo di questo header presenta alcuni svantaggi:
- Non tiene conto dello stato di congestione dei router lungo il percorso. Una soluzione adottata è il *"loose control"*, dove si specifica solo un nodo obbligatorio da attraversare, lasciando più flessibilità per il percorso.
- Alcuni nodi intermedi potrebbero essere malevoli, introducendo rischi per la sicurezza.
- Può essere sfruttato per attacchi DoS (Denial of Service), in cui tutti i pacchetti vengono forzati a passare attraverso un singolo router, causando sovraccarico.

A causa di questi problemi, il **Routing Header** è stato deprecato nelle implementazioni moderne di IPv6.
### A cosa serve il fragment header?

Il **Fragment Header** in IPv6 è utilizzato per gestire la **frammentazione dei pacchetti**. A differenza di IPv4, in cui la frammentazione può essere eseguita da qualsiasi router lungo il percorso, in IPv6 la frammentazione può essere fatta solo dal **mittente**. Il Fragment Header viene quindi inserito dal nodo sorgente quando un pacchetto supera la dimensione massima supportata dalla rete di destinazione, nota come **MTU (Maximum Transmission Unit)**.

Un aspetto cruciale che ora ricade sulla responsabilità del mittente è determinare l'MTU (Maximum Transmission Unit) lungo il percorso del pacchetto. Esistono due modi per conoscerla:
1. È stato impostato dallo standard un valore fisso per la **Minimum Transmission Unit** pari a 1028 byte, che garantisce una dimensione di pacchetto accettabile per la maggior parte delle reti. È stato impostato a 1028 byte e non ad esempio a 2500 byte perché siamo in una fase di transizione in cui spesso i pacchetti IPv6 vengono incapsulati in pacchetti IPv4 e quindi la loro dimensione può aumentare durante il percorso. 
2. Un metodo più dinamico consiste nell'usare i pacchetti ICMPv6 con il messaggio di errore "Packet Too Big". Questo fa parte del protocollo noto come **Path MTU Discovery**, utilizzato principalmente prima di inviare pacchetti di grandi dimensioni, poiché può rallentare la trasmissione. Il funzionamento si basa sull'invio di pacchetti di dimensioni elevate: se questi raggiungono la destinazione senza problemi, significa che l'MTU è sufficiente. In caso contrario, il mittente riceve un pacchetto ICMPv6 di errore contenente l'informazione sull'MTU del router che ha bloccato il pacchetto, permettendo così di adattare la dimensione dei pacchetti successivi.

Il Fragment Header contiene i seguenti campi principali:
- **Fragment Offset**: Indica la posizione del frammento all'interno del pacchetto originale.
- **Identification**: Identifica in modo univoco i frammenti che appartengono allo stesso pacchetto originale.
- **More Fragments Flag (M flag)**: Indica se il frammento è l'ultimo o se ci sono altri frammenti successivi.
Grazie a questo header i router non sanno di star trasmettendo un frammento intermedio e sarà il destinatario a riassemblarli.

### Come avviene la processazione degli header IPv6?

Per l'elaborazione degli header IPv6, esistono diversi metodi. 
Se è presente un header **hop-by-hop**, questo deve essere *processato a livello software*, poiché ogni nodo lungo il percorso deve esaminare questo tipo di header. 

![With Hop-by-Hop|center|500](https://www.cisco.com/en/US/technologies/tk648/tk872/images/technologies_white_paper0900aecd8054d37d-07.jpg)


In assenza dell'header hop-by-hop, l'intero pacchetto può essere elaborato *direttamente dall'hardware* di rete, senza coinvolgere il software. Questo è il motivo per cui l'uso dell'header hop-by-hop dovrebbe essere limitato e usato solo quando strettamente necessario, poiché aumenta il carico di elaborazione sui router lungo il percorso.

![Without Hop-by-Hop|center|500](https://www.cisco.com/en/US/technologies/tk648/tk872/images/technologies_white_paper0900aecd8054d37d-10.jpg)

Quando è presente un **Access Control List (ACL)**, ogni pacchetto viene confrontato con una serie di regole definite nell'ACL. Queste regole specificano quali pacchetti devono essere permessi o bloccati in base a criteri come indirizzi IP, protocolli o porte. 

Gli **ACL basati sull'header type** controllano il tipo di header IPv6 (come hop-by-hop, routing header, ecc.) per decidere se consentire o bloccare un pacchetto. Sono quindi focalizzati sui campi specifici degli header IPv6.

![ACL type|center|500](https://www.cisco.com/en/US/technologies/tk648/tk872/images/technologies_white_paper0900aecd8054d37d-12.jpg)

Gli **ACL basati sull'upper layer** invece guardano oltre l'header IPv6 e controllano le informazioni presenti nei protocolli di livello superiore, come TCP o UDP. Ad esempio, potrebbero esaminare le porte TCP/UDP, il numero di sequenza o altri dati del livello di trasporto o applicazione.

![ACL upper layers|center|500](https://www.cisco.com/en/US/technologies/tk648/tk872/images/technologies_white_paper0900aecd8054d37d-14.jpg)

### Quali sono le categorie alle quali può appartenere un IPv6?

Dato un indirizzo IPv6 abbiamo 3 possibilità:
1. **Unicast**: un indirizzo 
2. **Multicast**:
3. **Anycast**:
