## 1. Quali sono gli attori principali nel global Internet?

I principali attori coinvolti nel funzionamento di Internet globale sono:
1. **Global ISP (Internet Service Providers)**: Fornitori di servizi che offrono connettività Internet a livello globale.
2. **Content Providers**: Aziende che producono e distribuiscono contenuti su Internet, come Netflix, Google e Amazon.
3. **Data Center Networks**: Reti che ospitano e gestiscono i server per grandi quantità di dati, spesso utilizzate dai cloud provider e altre grandi aziende.
4. **Transit Providers**: Fornitori che permettono il transito di dati tra differenti reti e ISP.
5. **Regional ISP**: ISP locali o regionali che collegano le reti degli utenti finali a Internet.
6. **Cloud Providers**: Aziende che offrono servizi di cloud computing, come AWS, Google Cloud e Microsoft Azure.
7. **Enterprise Networks**: Reti interne alle aziende utilizzate per connettere i dispositivi aziendali.

## 2. Qual è il protocollo alla base del core di Internet? Qual è il suo limite maggiore e come lo si può risolvere?

Il protocollo alla base del core di Internet è l'**Internet Protocol (IP)**, che gestisce l'instradamento dei pacchetti di dati tra i vari dispositivi connessi a Internet. Esistono due versioni principali di IP: **IPv4** e **IPv6**.

### Limite maggiore di IP: Scalabilità
Il maggiore limite dell'IP, in particolare con **IPv4**, è la **scalabilità**. Questo si manifesta principalmente a causa della complessità crescente delle **tabelle di routing**, che aumentano man mano che la rete si espande (si stimano nel 2024 circa 1 milione di destinazioni). Questo porta a:
- **Routing Matrix molto grandi**: Con l'espansione della rete, le tabelle di routing diventano enormi, richiedendo molto spazio in memoria, soprattutto in reti più grandi.
- **Alte latenze**: Il processo di ricerca nei grandi spazi di indirizzi rallenta il routing, aumentando la latenza.
Quindi il protocollo IP in teoria è perfetto ma nella pratica non scala, cosa che lo rende inutilizzabile.

### Soluzione al problema: MPLS e SR
La soluzione principale a questo problema è quello di instradare non usando l'indirizzo di destinazione, abbiamo due opzioni:
1. **MPLS (Multi-Protocol Label Switching)**: Utilizza etichette (label) per instradare i pacchetti in modo più efficiente, riducendo la latenza e la complessità delle tabelle di routing nei nodi centrali della rete. In questo protocollo il pacchetto IP viene incapsulato in uno MPLS.
2. **SR (Segment Routing)**: Il percorso che un pacchetto deve seguire attraverso la rete è codificato direttamente nel pacchetto stesso sotto forma di una sequenza di segmenti (una lista di identificatori che rappresentano nodi o percorsi specifici). In questo caso non si incapsula ma si aggiungono informazioni al pacchetto IP.

## 3. Come si raggiunge flessibilità nelle reti? Qual è la differenza con la scalabilità?

Il termine **flexibility** si riferisce alla **capacità del protocollo di adattarsi a diverse esigenze di rete**, come l'ingegneria del traffico e l'ottimizzazione dei percorsi. Il difetto principale dell'IP, riguardo alla flessibilità, è la **rigidità** con cui gestisce il routing e l'instradamento dei pacchetti. L'IP tradizionale non consente un controllo fine dei percorsi che i pacchetti devono seguire attraverso la rete; i pacchetti vengono generalmente instradati seguendo i percorsi predefiniti dalle tabelle di routing, senza possibilità di modifiche dinamiche o granulari basate su esigenze specifiche di traffico.
### Differenza tra Flexibility e Scalability:
- **Scalability** riguarda la **capacità del protocollo di gestire una rete in crescita**, ovvero la capacità di IP di funzionare bene anche in reti di grandi dimensioni. Il difetto di scalabilità in IP si manifesta quando le tabelle di routing diventano troppo grandi.
- **Flexibility**, invece, si riferisce alla **capacità del protocollo di adattarsi a diverse esigenze operative** e scenari dinamici, come l'instradamento personalizzato o l'ottimizzazione del traffico. Il difetto di flessibilità riguarda l'incapacità di IP di deviare dinamicamente il traffico o di ottimizzare percorsi senza modifiche statiche alle tabelle di routing.
### Soluzioni per la mancanza di flessibilità:
- **Segment Routing (SR)**: SR permette di avere percorsi specifici inclusi nei pacchetti, rendendo l'instradamento molto più flessibile senza dipendere da tabelle di routing complesse.
- **Software-Defined Networking (SDN)**: SDN separa il controllo della rete dal piano dati, permettendo una gestione più flessibile e dinamica dei percorsi e delle risorse di rete.

Inoltre, nelle reti moderne bisogna garantire QoS (Quality of Services) legata alla velocità della rete globale e QoE (Quality of Experience) più legata alle reti domestiche. 

## 4. Come sono composte le architetture delle reti dei data center?

Le architetture delle reti dei data center utilizzano il modello **Leaf-Spine**, un'architettura di rete utilizzata per migliorare la scalabilità, le prestazioni e la ridondanza. È composta da due livelli principali:
1. **Spine (dorsale)**: Questo livello è composto da switch ad alta capacità che agiscono come il core della rete. Ogni switch spine è collegato a tutti gli switch leaf, garantendo che ogni host connesso possa comunicare con qualsiasi altro host con la stessa latenza.
2. **Leaf (foglia)**: Questo livello è composto da switch che si collegano direttamente ai server, storage e altre risorse del data center. Ogni switch leaf è collegato a tutti gli switch spine, il che permette una bassa latenza e un percorso breve tra qualsiasi punto della rete.

Inoltre, viene utilizzata la virtualizzazione delle risorse per fornire un servizio ottimale a tutti i client che utilizzano il datacenter. 

## 5. Che cos'è il Software Defined Networking?

Il **Software Defined Networking (SDN)** è un'architettura di rete che separa il **piano di controllo** (control plane) dal **piano di inoltro** (data plane), rendendo la rete più flessibile e programmabile. 
### Caratteristiche principali:
1. **Centralizzazione del controllo**: In SDN, le decisioni di instradamento e gestione della rete sono centralizzate in un controller software, che ha una visione globale della rete e può configurarla dinamicamente.
2. **Programmabilità**: Le reti SDN possono essere programmate e configurate attraverso interfacce software (API), rendendo più semplice l'automazione della gestione della rete e la creazione di nuovi servizi.
Uno dei protocolli più famosi per l'SDN si chiama OpenFlow.