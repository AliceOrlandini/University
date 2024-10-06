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

Dato un indirizzo IPv6, esistono tre modalità principali di indirizzamento:
1. **Unicast**: Un indirizzo unicast identifica univocamente *un'interfaccia* su un nodo IPv6. Un pacchetto inviato a un indirizzo unicast viene recapitato direttamente al nodo associato a quell'indirizzo specifico.
2. **Multicast**: Un indirizzo multicast rappresenta un *gruppo di interfacce* IPv6, generalmente distribuite su più nodi. Un pacchetto inviato a un indirizzo multicast viene ricevuto e processato da tutti i nodi appartenenti al gruppo.
3. **Anycast**: Un indirizzo anycast viene assegnato a *più interfacce*, solitamente su nodi diversi. A differenza del multicast, un pacchetto inviato a un indirizzo anycast viene recapitato solo al nodo più vicino (in termini di routing) che condivide quell'indirizzo. È utile in sistemi distribuiti come DNS o DHCP, poiché garantisce che solo un nodo processi il pacchetto.

Infine, va notato che l'indirizzamento **broadcast**, presente in IPv4, non esiste più in IPv6. È stato sostituito dal concetto di multicast, che permette di raggiungere gruppi specifici di nodi.

### Quali sono gli scope di validità di IPv6?

Gli indirizzi IPv6 includono un **scope di validità**, che specifica l'ambito topologico all'interno del quale un indirizzo può essere utilizzato come identificatore unico. Questo ambito è codificato in una parte dell'indirizzo e determina dove l'indirizzo è valido. I principali scope sono:
- **Global scope**: L'indirizzo è valido in tutto Internet, ed è utilizzabile per comunicazioni globali.
- **Link-local**: L'indirizzo è valido solo all'interno di un singolo link. La comunicazione in questo ambito è più efficiente grazie alla "On Link Determination", che permette di identificare velocemente i nodi all'interno del link stesso.
- **Unique-local**: Questo indirizzo ha una visibilità più ampia rispetto ai link-local, ma è limitato a una rete privata, simile alle VPN. A differenza di IPv4, dove gli indirizzi privati potevano essere duplicati, gli indirizzi unique-local in IPv6 sono comunque unici, anche nelle reti private.

Il **link** rappresenta l'insieme di nodi che appartengono alla stessa rete e che possono comunicare direttamente tra loro. Le caratteristiche principali del link sono:
- **Stabilità nel tempo**: Il network address rimane costante per garantire connessioni affidabili.
- **Single link-layer broadcast domain**: Ogni nodo all'interno dello stesso dominio di broadcast non può avere un indirizzo IP duplicato. Questo semplifica il rilevamento di eventuali conflitti di indirizzo nella rete.
- **Transitività**: Se il nodo A può comunicare con il nodo B e B può comunicare con C, allora A può comunicare anche con C. La transitività è cruciale per garantire che tutti i nodi all'interno dello stesso network possano comunicare tra loro senza problemi.


### Quale notazione si utilizza per gli indirizzi IPv6?

Gli indirizzi IPv6 sono rappresentati con una notazione composta da 8 blocchi di 16 bit separati da due punti, nella forma **x:x:x:x:x:x:x:x**, dove ogni blocco è espresso in *4 cifre esadecimali*. 
Esistono delle regole di abbreviazione per rendere la notazione più compatta:
1. Gli zeri iniziali di ogni blocco possono essere omessi. Ad esempio, `0012` può essere scritto come `12`.
2. Blocchi consecutivi di zeri (anche più di due) possono essere sostituiti da `::`. Questa abbreviazione può però essere utilizzata solo una volta all'interno dell'indirizzo, altrimenti si creerebbero ambiguità. Ad esempio l'indirizzo `2001:0db8:0000:0000:0000:ff00:0000:0042` se venisse abbreviato diventerebbe `2001::ff00::42` ma non si può sapere quanti blocchi di zeri mancano nel primo `::` e nel secondo `::`, rendendo l'indirizzo ambiguo. Potrebbe essere un indirizzo completamente diverso perché non c'è un modo chiaro per determinare dove esattamente si trovano gli zeri omessi.

Esistono anche due indirizzi IPv6 particolari:
- **0:0:0:0:0:0:0:0** o semplicemente **::**, è l'indirizzo non specificato, utilizzato ad esempio quando un dispositivo si connette per la prima volta a una rete e richiede un indirizzo tramite DHCP.
- **0:0:0:0:0:0:0:1** o **::1**, è l'indirizzo di loopback, usato per riferirsi a sé stessi (localhost).

Come in IPv4, anche in IPv6 gli indirizzi sono suddivisi in una parte di rete (prefisso) e una parte dedicata agli host all'interno della rete. La parte di rete viene indicata usando la notazione **/lunghezza_prefisso** (ad esempio, **/64** per indicare un prefisso di rete lungo 64 bit).

Alcuni prefissi permettono di capire la tipologia di indirizzo:

| Allocation            | Prefisso in binario | Prefisso in esadecimale | Frazione dello spazio di indirizzamento |
| --------------------- | ------------------- | ----------------------- | --------------------------------------- |
| Non assegnato         | 0000 0000           | ::0/8                   | 1/256                                   |
| Riservato             | 0000 001            | ::1/7                   | 1/128                                   |
| Global Unicast (GUA)  | 001                 | 2000::/3                | 1/8                                     |
| Link-Local Unicast    | 1111 1110 10        | FE80::/10               | 1/1024                                  |
| Riservato             | 1111 1110 11        | FEC0::/10* deprecato    | 1/1024                                  |
| Unique Local (ULA)    | 1111 110            | FC00::/7                |                                         |
| Amministratore Locale | 1111 1101           | FD00::/8                |                                         |
| Multicast             | 1111 1111           | FF00::/8                | 1/256                                   |

### Da quali campi è composto il Global Unicast Address?

![[Global Unicast Address.webp|center|400]]

Come accennato, il prefisso degli **indirizzi Global Unicast** in IPv6 è **2000::/3**, il che significa che qualsiasi indirizzo che inizia con **2000** a **3fff** è un indirizzo unicast globale. Per esempio, un indirizzo che inizia con **2001** è coerente con questo prefisso.
Le sezioni successive dell'indirizzo IPv6 permettono di identificare diversi elementi:
1. **Registro e prefisso dell'ISP**: Questi campi indicano l'autorità di registrazione e l'Internet Service Provider (ISP) che ha assegnato l'indirizzo.
2. **Prefisso del sito**: Un "sito" in questo contesto è una rete o organizzazione che ha un insieme di indirizzi IPv6 assegnati dall'ISP.
3. **Subnet**: Sono dedicati 16 bit per definire eventuali subnet interne al sito, facilitando la suddivisione logica delle reti.
4. **Interface ID**: Questa parte dell'indirizzo, composta dagli ultimi 64 bit, corrisponde all'**host** di IPv4 e identifica un'interfaccia univoca all'interno della rete.

Per motivi di ottimizzazione e prestazioni, è stato stabilito che il prefisso degli indirizzi IPv6 sarà di *64 bit*. Questo permette ai router di gestire più velocemente il routing degli indirizzi, dato che possono utilizzare l'hardware per processare il prefisso senza coinvolgere la CPU.

### Come si ricava l'interface ID?

Ogni host all'interno di un link IPv6 deve avere un **interface ID** univoco, e ci sono tre metodi principali per assegnarlo:
1. **Manuale**: L'interface ID viene configurato manualmente dall'amministratore di rete.
2. **Dinamico**: L'ID viene assegnato tramite **DHCPv6**.
3. **Stateless**: L'ID viene generato automaticamente dall'host.

Vediamo in particolare come si ottiene un interface ID in modo *stateless*.
Quando si genera un **interface ID** in modo stateless, il metodo più comune è utilizzare il **MAC Address** dell'interfaccia di rete, che è generalmente unico (salvo errori di produzione) e ha una lunghezza di 48 bit. Per ottenere i 64 bit richiesti per l'**interface ID**, si combinano i bit del MAC Address con alcuni bit fissi. L'indirizzo così generato è noto come **SLAAC (Stateless Address Autoconfiguration) Address**.

![[Stateless Interface ID.png|center|400]]

Un problema significativo con questo metodo è legato alla privacy. Poiché l'**interface ID** è derivato dal MAC Address, rimane invariato anche quando l'host si sposta tra reti diverse. Ciò significa che, anche se il prefisso della rete cambia (per esempio passando dalla rete dell'università a quella di casa), l'**interface ID** rimane lo stesso. Questo consente a servizi come Google di tracciare gli spostamenti di un utente, associando l'ID univoco ai vari link.

Per risolvere questo problema, sono stati introdotti due nuovi metodi:
- **Temporary Transient Addresses**: Questi indirizzi sono generati in modo casuale e si aggiornano periodicamente. Il vantaggio è che l'ID cambia regolarmente, rendendo più difficile il tracciamento. Tuttavia, un lato negativo è che, cambiando l'ID, l'host non è più riconoscibile all'interno dello stesso link, il che può creare problemi di identificazione locale.
- **Stable Privacy Addresses**: In questo metodo, l'interface ID non è basato su un identificatore hardware come il MAC Address, ma viene generato in modo stabile per ogni link al quale ci si connette. Ciò significa che, all'interno di uno stesso link, l'interface ID rimane costante, ma cambia quando l'host si sposta su una rete diversa. In questo modo, l'host è riconoscibile all'interno del link senza esporre la sua identità su reti differenti, proteggendo la privacy dell'utente.

### Da quali campi sono composti il Link-Local Address e l'Unique Local?

Gli **indirizzi link-local** in IPv6 vengono assegnati automaticamente tramite auto-configurazione. Per questi indirizzi, 54 bit sono fissati a zero, lasciando i restanti 64 bit per l'**interface ID**.

Per quanto riguarda invece gli **indirizzi unique-local**, essi includono un flag **L** che indica se l'indirizzo è stato assegnato ufficialmente o se è ancora in fase di generazione. 
Questi indirizzi, pur essendo locali, devono essere unici all'interno della rete. Per garantire questa univocità senza dover ricorrere a un'autorità centrale, vengono generati casualmente 40 bit, che costituiscono il **Global ID**. Questo meccanismo permette di assegnare indirizzi unici senza richiedere un registro globale, poiché i 40 bit generati casualmente hanno una bassa probabilità di duplicazione.

Infine, l'indirizzo include un **Subnet ID**, che consente di suddividere ulteriormente la rete interna.

![[Link Local and Local.png|center|500]]


### Com'è strutturato l'anycast address?

Un indirizzo **Anycast** è assegnato a più interfacce, solitamente su nodi diversi. Quando un pacchetto viene inviato in modalità anycast, esso è destinato a più interfacce, ma viene recapitato a una sola di esse, generalmente quella più vicina in termini di routing.

Questa modalità è stata progettata per garantire **load balancing** e **ridondanza**, poiché più nodi o router possono fornire lo stesso servizio. Se uno di questi nodi è sovraccarico o non disponibile, un altro può rispondere alla richiesta, migliorando l'affidabilità e l'efficienza del servizio. Nel contesto del **CAP theorem**, questa tipologia di invio rientra nel modello **AP** (Availability e Partition Tolerance), garantendo che un servizio sia sempre disponibile, anche in caso di partizioni di rete.

L'implementazione del routing anycast avviene a livello di rete: il mittente non ha controllo su quale interfaccia o nodo riceverà effettivamente il pacchetto, e generalmente non è interessato a saperlo, poiché il comportamento desiderato è ottenere una risposta dal nodo più appropriato (di solito il più vicino).

L'implementazione degli indirizzi **Anycast** si basa sul protocollo di routing IP. È sufficiente inserire la destinazione nella tabella di routing e il pacchetto verrà instradato verso il nodo più vicino che condivide quell'indirizzo anycast. Questo meccanismo funziona sia con **IPv6** che con **IPv4**, rendendo l'uso di indirizzi anycast flessibile e compatibile con entrambe le versioni del protocollo.

Uno dei principali utilizzi degli indirizzi anycast è nei **CDN (Content Delivery Networks)**. I CDN replicano contenuti, come video o file multimediali, su diversi nodi distribuiti in tutto il mondo. Quando un utente richiede un contenuto, il pacchetto viene instradato al nodo più vicino geograficamente o topologicamente, migliorando la velocità di accesso e riducendo la latenza. Questo meccanismo è simile a una cache distribuita su Internet.

Quando un indirizzo è di tipo anycast, la procedura di **Duplicate Address Detection (DAD)** non viene eseguita. Questo perché la duplicazione degli indirizzi è intenzionale e voluta: più nodi devono condividere lo stesso indirizzo anycast per garantire la distribuzione del servizio.

Un indirizzo **Anycast** è composto da **n bit di prefisso** che identificano la sottorete, mentre i restanti **128 - n bit** sono impostati a zero. Questo garantisce che l'indirizzo sia interpretabile correttamente e facilmente riconoscibile all'interno delle tabelle di routing.

![[Anycast address.png|center|500]]


### Com'è strutturato il multicast address?

Un pacchetto inviato a un **multicast address** viene ricevuto e processato da tutti i nodi che fanno parte del gruppo multicast associato a quell'indirizzo. Un singolo nodo può appartenere a più gruppi multicast contemporaneamente.

Un **multicast address** inizia con il prefisso **ff00::/8**, dove i primi 8 bit indicano che si tratta di un indirizzo multicast. La struttura di un indirizzo multicast è composta da:
- **8 bit** di prefisso fisso (ff),
- **4 bit** di flag che indicano il tipo di indirizzo multicast,
- **4 bit** per specificare l'ambito (scope) del multicast, che indica la portata del gruppo multicast (ad esempio, link-local, site-local, o globale),
- **112 bit** utilizzati per identificare il gruppo multicast specifico.

![[Global address.webp|center|700]]

Esistono alcuni multicast address predefiniti, utilizzati per scopi specifici. Ecco alcuni esempi:
- **ff02::1**: Tutti i nodi sul link locale. 
- **ff02::2**: Tutti i router sul link locale.
- **ff02::5** e **ff02::6**: Utilizzati rispettivamente da tutti i router OSPF (Open Shortest Path First) e OSPF-designated routers.
- **ff02::1:2**: Utilizzato dai nodi che inviano richieste DHCPv6 per trovare un server DHCPv6 sulla rete locale.

### Che cos'è ICMPv6?

**ICMPv6** (Internet Control Message Protocol for IPv6) è un protocollo utilizzato per gestire errori e fornire funzioni di diagnostica nelle reti IPv6. Con l'introduzione di IPv6, ICMPv6 è stato esteso per includere funzioni che, in IPv4, venivano gestite da protocolli separati o esterni. Questo ha reso ICMPv6 più completo e integrato nella gestione delle reti.

Tra queste nuove funzioni troviamo: 
- **Trovare tutti i router in un link**: ICMPv6 permette di scoprire i router presenti sulla rete locale, facilitando la configurazione e il routing.
- **Address Resolution Protocol (ARP)**: In IPv6, la funzione ARP è integrata in ICMPv6 attraverso il Neighbor Discovery Protocol (NDP), che risolve gli indirizzi IP in indirizzi MAC.
- **Tenere traccia dei vicini raggiungibili**: Simile alla ARP cache di IPv4, ICMPv6 gestisce le informazioni sui nodi vicini e i loro stati di raggiungibilità attraverso il NDP.
- **Controllare IP duplicati**: ICMPv6 include il processo di **Duplicate Address Detection (DAD)**, che verifica se un indirizzo IP è già in uso all'interno della rete, evitando conflitti.

Un pacchetto ICMPv6 ha la seguente struttura:
1. **Type (8 bit)**: Specifica il tipo di messaggio ICMPv6 (ad esempio, messaggi di errore o di informazione). Alcuni esempi di tipi includono:
	- Tipo 1: Messaggio di destinazione irraggiungibile.
	- Tipo 2: Packet Too Big (utile per l'MTU).
	- Tipo 3: Time Exceeded (usato nell'hop by hop).
	- Tipo 4: Parameter Problem (il formato del pacchetto è errato).
1. **Code (8 bit)**: Specifica ulteriori dettagli sul tipo di messaggio, indicando la sottocategoria dell'errore o dell'informazione.
2. **Checksum (16 bit)**: Controlla l'integrità del pacchetto ICMPv6, verificando eventuali errori nei dati. Questo campo è necessario perché ora il checksum è responsabilità dei layer superiori visto che è stato rimosso da IPv6.
3. **Message-specific data**: Ogni tipo di messaggio può avere dati specifici.

![ICMPv6|center|400](https://notes.shichao.io/tcpv1/figure_8-10.png)

### Come funziona il Neighbour Discovery Protocol?

Il **Neighbour Discovery Protocol (NDP)** è un protocollo in IPv6, utilizzato per gestire la comunicazione tra nodi sulla stessa rete locale (link-local) e per sostituire alcune delle funzionalità che, in IPv4, erano gestite da altri protocolli.

Le sue funzioni principali sono:
1. **Risoluzione degli indirizzi**: NDP risolve gli indirizzi IPv6 in indirizzi MAC, simile a come ARP funziona in IPv4. Quando un nodo vuole comunicare con un altro nodo all'interno della stessa rete, invia una richiesta di **Neighbor Solicitation** per ottenere l'indirizzo MAC corrispondente all'indirizzo IPv6.
2. **Raggiungibilità dei vicini**: NDP tiene traccia della raggiungibilità dei vicini utilizzando una tabella di vicini, che registra lo stato di ciascun nodo nella rete. Quando un nodo vuole comunicare con un vicino, invia periodicamente messaggi di **Neighbor Solicitation** per verificare che il vicino sia ancora raggiungibile.
3. **Autoconfigurazione degli indirizzi**: Utilizzando **Router Solicitation** e **Router Advertisement**, NDP consente ai nodi di ottenere le informazioni di configurazione della rete, come il prefisso della rete e l'indirizzo del router predefinito. Questo permette agli host di configurare i propri indirizzi IPv6 in modo autonomo tramite **SLAAC (Stateless Address Autoconfiguration)**.
4. **Rilevamento di router**: NDP consente agli host di scoprire i router disponibili nella rete tramite messaggi di **Router Solicitation** (RS) inviati dall'host ai router locali. I router rispondono con messaggi di **Router Advertisement** (RA), fornendo informazioni di configurazione come prefissi di rete, MTU e gateway predefinito.
5. **Redirect**: Quando un router scopre che esiste un percorso più ottimale per raggiungere una destinazione, invia un messaggio di **Redirect** al nodo, informandolo del nuovo router da utilizzare per quel percorso.
6. **Duplicate Address Detection (DAD)**: Prima di assegnare un indirizzo IPv6 a una rete, NDP verifica che tale indirizzo non sia già in uso nella rete locale. Questo processo di controllo avviene tramite messaggi di **Neighbor Solicitation** e **Neighbor Advertisement**.

