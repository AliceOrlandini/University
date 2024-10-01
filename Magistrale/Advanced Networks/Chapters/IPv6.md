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
3. **Flow Label (etichetta di flusso)** (20 bit): Usato per identificare un flusso di datagrammi appartenenti a una stessa sessione o flusso.
4. **Lunghezza del Payload** (16 bit): Indica la dimensione del payload (dati) che segue l'header.
5. **Next Header** (8 bit): Specifica il tipo di header successivo, che può essere un header di trasporto (es. TCP, UDP) o un header di estensione.
6. **Hop Limit** (8 bit): Indica il numero massimo di salti che il pacchetto può attraversare prima di essere scartato. Ogni volta che il datagramma raggiunge un router, il valore di questo campo viene decrementato di 1. Lo scopo è quello di non lasciare datagrammi che vagano senza meta per l'Internet.
7. **Source Address** (128 bit): L'indirizzo IPv6 del mittente.
8. **Destination Address** (128 bit): L'indirizzo IPv6 del destinatario.

Confrontando il formato IPv6 con IPv4 notiamo che sono stati tolti alcuni campi: 
- **Frammentazione/Riassemblaggio**: IPv6 non consente frammentazione né riassemblaggio nei router intermedi, queste operazioni devono essere effettuate solo dal sorgente e solo dal destinatario. Il motivo di tale scelta è che le operazioni di frammentazione e riassemblaggio consumano tempo e demandarle ai router dei sistemi periferici è molto più efficiente. 
- **Checksum di intestazione**: considerando che i protocolli a livello di tra

### Quali sono i possibili valori del campo next header?

Il campo **Next Header** dell'header IPv6 può assumere diversi valori, a seconda del protocollo o del tipo di header che segue l'header IPv6 di base. Ecco alcuni dei valori più comuni:
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

### A cosa serve l'hop-by-hop options header?

L'**Hop-by-Hop Options Header** di IPv6 serve per trasportare informazioni che devono essere elaborate da ogni router lungo il percorso del pacchetto, non solo dal destinatario finale. È utilizzato per opzioni che richiedono una gestione particolare a ogni hop (salto) che il pacchetto compie sulla rete.

Alcuni esempi di utilizzo del Hop-by-Hop Options Header includono:
- **Jumbo Payload**: Permette di inviare pacchetti con payload di dimensioni superiori a 65.535 byte.
- **Router Alert**: Segnala ai router lungo il percorso che devono ispezionare il pacchetto, utile per protocolli come RSVP (Resource Reservation Protocol).

Poiché ogni router deve elaborare questo header, il suo uso può introdurre un overhead e potenzialmente rallentare il routing, per cui viene utilizzato con parsimonia e solo per casi specifici.

### A cosa serve il routing header?

Il **Routing Header** in IPv6 è utilizzato per specificare un percorso alternativo che un pacchetto deve seguire per raggiungere la destinazione. In pratica, consente di indicare una o più tappe (nodi intermedi) che il pacchetto deve attraversare lungo il suo percorso verso la destinazione finale. Questo processo è chiamato **source routing**.

Il campo "Next Header" dell'header IPv6 di base indica la presenza di un **Routing Header**, e il **Routing Header** stesso contiene una lista di indirizzi IPv6 (nodi intermedi) che il pacchetto dovrà attraversare.

Esistono vari tipi di Routing Header (RH), i più comuni sono:
- **RH0 (obsoleto)**: Permetteva di specificare più tappe intermedie. È stato deprecato a causa di problemi di sicurezza (vulnerabilità a "routing loops").
- **RH2**: Usato principalmente con il protocollo **Mobile IPv6**, consente a un pacchetto di raggiungere un dispositivo mobile tramite un nodo intermedio (come il suo "home agent").

### A cosa serve il fragment header?

Il **Fragment Header** in IPv6 è utilizzato per gestire la **frammentazione dei pacchetti**. A differenza di IPv4, in cui la frammentazione può essere eseguita da qualsiasi router lungo il percorso, in IPv6 la frammentazione può essere fatta solo dal **mittente**. Il Fragment Header viene quindi inserito dal nodo sorgente quando un pacchetto supera la dimensione massima supportata dalla rete di destinazione, nota come **MTU (Maximum Transmission Unit)**.

Il Fragment Header permette di:
1. **Frammentare pacchetti** troppo grandi per essere trasmessi in una singola unità sulla rete, rispettando l'MTU.
2. **Ricostruire i frammenti** una volta che raggiungono il destinatario, che riassembla il pacchetto completo.

Il Fragment Header contiene i seguenti campi principali:
- **Fragment Offset**: Indica la posizione del frammento all'interno del pacchetto originale.
- **Identification**: Identifica in modo univoco i frammenti che appartengono allo stesso pacchetto originale.
- **More Fragments Flag (M flag)**: Indica se il frammento è l'ultimo o se ci sono altri frammenti successivi.
