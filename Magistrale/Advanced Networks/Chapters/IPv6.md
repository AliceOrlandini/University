### Quali sono le motivazioni che hanno portato ad adottare IPv6?

Il principale motivo che ha portato alla migrazione da IPv4 a IPv6 è stato il *limitato spazio di indirizzamento* di IPv4. Nel 1994, l'IETF (Internet Engineering Task Force) aveva previsto l'esaurimento degli indirizzi IPv4 tra il 2005 e il 2011, cosa che effettivamente si è verificata. Per risolvere questo problema, è stato sviluppato un nuovo standard, **IPv6**, che utilizza indirizzi di *128 bit*, garantendo così uno spazio di indirizzamento praticamente inesauribile per il futuro.

Oltre a risolvere il problema dello spazio di indirizzi, IPv6 ha affrontato alcune criticità presenti in IPv4. Tra queste, la rimozione della lunghezza variabile dell'header, che semplifica il processamento dei pacchetti. Inoltre, con IPv6 non è più necessario l'uso del NAT (Network Address Translation), poiché è possibile assegnare a ogni utente un blocco di indirizzi /64, garantendo un numero sufficiente di indirizzi per ogni dispositivo connesso.

Un'altra innovazione di IPv6 è stata l'eliminazione del protocollo ARP (Address Resolution Protocol), sostituito dal Neighbor Discovery Protocol (NDP), che è più efficiente. Inoltre, IPv6 offre un miglior supporto per le opzioni e le estensioni, consentendo una maggiore flessibilità e scalabilità per nuove funzionalità.

IPv6 è diventato uno standard ufficiale nel 2017 e, sebbene la sua adozione sia ancora in corso, sta gradualmente diffondendosi in tutto il mondo. Attualmente, circa il 45% degli utenti globali utilizza IPv6 per accedere ai servizi Google, segno di una crescente adozione di questo nuovo protocollo.

### Da quali campi è composto il Base Header di IPv6?

Il base header di IPv6 è composto dai seguenti campi:
1. **Version** (4 bit): Indica la versione del protocollo Internet, che per IPv6 è 6.
2. **Traffic Class** (8 bit): Campo usato per indicare la priorità e la gestione del traffico (QoS).
3. **Flow Label** (20 bit): Usato per identificare un flusso di pacchetti appartenenti a una stessa sessione o flusso.
4. **Payload Length** (16 bit): Indica la dimensione del payload (dati) che segue l'header.
5. **Next Header** (8 bit): Specifica il tipo di header successivo, che può essere un header di trasporto (es. TCP, UDP) o un header di estensione.
6. **Hop Limit** (8 bit): Indica il numero massimo di salti che il pacchetto può attraversare prima di essere scartato.
7. **Source Address** (128 bit): L'indirizzo IPv6 del mittente.
8. **Destination Address** (128 bit): L'indirizzo IPv6 del destinatario.

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



### A cosa serve il fragment header?

