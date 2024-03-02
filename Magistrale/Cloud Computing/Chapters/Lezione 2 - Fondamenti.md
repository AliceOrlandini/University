Il prof inizia la lezione dicendo "who cares about history".
Io che ieri ho passato un'ora a sistemare gli appunti sulla storia:

![meme|center|200](https://i.kym-cdn.com/photos/images/newsfeed/002/621/521/c0f.gif)

La parte che viene invece anche se può sembrare scontata all'esame la chiede quindi non saltatela. 
## Definizione di Cloud Computing

Il termine Cloud Computing oggigiorno si riferisce a molti ambiti quindi può significare tutto o niente. Per noi avrà due significati:
1. Cloud Services: software che sono in esecuzione su architetture distribuite.
2. Cloud Infrastructure: l’hardware ed il software che permette di erogare i servizi. 
La definizione che useremo per il Cloud è:

> [!note] Cloud Computing
> Il **Cloud Computing** è un *ambiente IT* progettato con lo scopo di fornire risorse IT che siano *scalabili*, *misurabili* e *accessibili via Internet*.

Le risorse IT hardware messe a disposizione sono quelle di base per il funzionamento di un calcolatore, cioè: CPU, RAM e dispositivi di memorizzazione di massa. 
Queste devono essere accessibili dall’utente esclusivamente via Internet. 

Tali risorse vengono utilizzate per fornire servizi, anch’essi accessibili via Internet, dall’utente finale tramite un set di programming interfaces (API). 

Quindi, con Cloud Computing si può indicare sia l’infrastruttura fisica, sia i servizi che vengono erogati tramite tali infrastrutture agli utenti finali. 
Il paradigma utilizzato si chiama XaaS, cioè Everything as a Service, cioè un modello di business in cui si erogano servizi sotto abbonamento.

## Ruoli 

I ruoli principali che individuiamo quando si parla di Cloud Computing sono:
- Cloud Provider: è il proprietario dell’infrastruttura cloud, cioè colui che investe risorse per acquistare l’hardware necessario per realizzare l’infrastruttura al fine di metterla a disposizione per implementare servizi su di essa. 
- Cloud Service Owner: è colui che utilizza l’infrastruttura per erogare il proprio servizio, un esempio potrebbe essere Netflix che eroga il servizio di video streaming sull’infrastruttura cloud di Amazon (AWS).
- Cloud Service User: è l’utente finale che, tramite l’Internet, accede alle API del servizio ospitato sull’infrastruttura cloud. 
Ad esempio se consideriamo Gmail e Google, il cloud provider è anche il cloud service owner perché utilizza la sua stessa infrastruttura per erogare un servizio. 
I ruoli secondari ma comunque presenti invece sono:
- Cloud Resource Administrator: è colui che is occupa della manutenzione dell’infrastruttura, è separato dal cloud provider perché quest’ultimo è solo colui che investe e vuole avere un ritorno economico mentre il primo gestisce la piattaforma.
- Cloud Costumer: è una sorta di service owner solo che invece che erogare servizi agli utenti finali usa l’infrastruttura per ospitare dei servizi personali, un esempio può essere il gestionale interno all’azienda. 
- Cloud Auditor: è un’ente esterno che si occupa di certificare la qualità del cloud, in termini di sicurezza, performance, e del rispetto delle norme contrattuali.
- Cloud Carrier: è l’Internet Service Provider, cioè colui che mette a disposizione l’infrastruttura Internet per accedere al Cloud. 

## Il Modello Cloud

Come riportato nella definizione di Cloud Computing, ci sono due parole molto importanti che lo caratterizzano: “infrastrutture IT **scalabili** e **misurabili**”. 
Infatti il cloud adotta un modello di business di tipo utility-based, cioè un pay-as-you-go, questo significa che le risorse effettivamente utilizzate del Cloud Service Owner devono essere misurate in modo da fargli pagare solo ciò che ha utilizzato. 
Per esempio, si può scegliere di far pagare la banda utilizzata oppure l’energia consumata o ancora lo spazio di storage occupato. 
Il secondo aspetto riguarda l’allocazione delle risorse che deve essere di tipo dinamico (on-demand) e senza l’intervento diretto dell’uomo, quindi il sistema deve essere scalabile dinamicamente.
Le risorse devono poter essere allocate non solo in seguito alla richiesta dell’uomo ma anche dalle macchine (tale macchina è chiamata orchestrator).

Il cloud ha quindi permesso di adottare un nuovo modello di business: 
- Dal punto di vista del Cloud Provider, esso mettere a disposizione delle risorse hardware per coloro che vogliono erogare servizi e farsi pagare queste risorse in base all’effettivo utilizzo.
- Dal punto di vista del Cloud Service Owner, esso fornisce un servizio ai Cloud User sotto abbonamento e fare margine pagando le risorse utilizzate al Cloud Provider. 
- Il Cloud User utilizza l’Internet messo a disposizione dal Cloud Carrier per accedere all’infrastruttura pagando un abbonamento al Cloud Service Owner. 

L’infrastruttura è realizzata tramite Datacenter, cioè degli spazi dedicati per contenere un grande numero di server e le reti LAN per connetterli tra loro. Tale hardware fornisce risorse IT di tra tipologie: 
- Computazionali
- Storage
- Networking

## Multi-tenancy

Le architetture tradizionali sono abilitate all’accesso multi-user, cioè più utenti che accedono contemporaneamente ad un unica macchina con un amministratore che coordina le operazioni. 
Un esempio potrebbe essere un server che riceve le richieste dei client, sarà lui ad occuparsi di servire tutte le richieste. Ciò equivale ad esempio alla visita di un appartamento, la visita è la richiesta del client e l’appartamento è il server. 

Quando si utilizza un’infrastruttura cloud però si vuole sfruttare al 100% tutte le risorse che si hanno a disposizione come i GB di RAM o i secondi di CPU. 
Quindi ciò che si vorrebbe fare è fare in modo che più persone abitino lo stesso appartamento e che ognuno di loro serva le richieste di competenza. 
Questo approccio è chiamato multi-tenancy.
Ogni amministratore del servizio ha la completa facoltà di scegliere la propria configurazione in termini di sistema operativo e avrà l’impressione di essere l’unico ad utilizzare le risorse hardware. Invece, quello che accade è che più servizi le stanno utilizzando, ognuno con le proprie scelte in termini di configurazione. 
*“From the point of view of the cloud service owner it seams an INCULATA” cit. Vallati*

Questo è possibile tramite la Virtualizzazione: una tecnica che consente di creare una versione virtuale di qualsiasi risorsa IT (ad esempio una versione virtuale della RAM, della CPU, dell’intero server…).
Con l’utilizzo della virtualizzazione si possono creare delle copie virtuali dell’hardware e fare interagire il service owner con tali copie in modo che abbia l’impressione di averne il completo controllo, ma senza che conosca come vengono poi gestire realmente. 

Generalmente si crea una versione virtuale di ogni componente hardware (Hardware Virtualization), poi si crea l’ambiente virtuale sopra il quale è installato un sistema operativo che nel complesso viene chiamato Macchina Virtuale. 
In questo modo si possono installare più sistemi operativi pur usando un unico hardware perché ognuno di essi avrà l’impressione di avere il proprio hardware dedicato. 

## Hypervisor

L’hypervisor è il cuore del virtualization layer ed è un componente software che permette di amministrare e controllare l’hardware virtuale. 
La vera macchina sulla quale è in esecuzione l’hypervisor si chiama host. Sopra l’host c’è l’hypervisor che gestisce l’interazione tra l’hardware reale e quello virtuale. Sopra di esso ci sono tutti i sistemi operativi che vengono chiamati guest. 

Oggigiorno, a seconda dei requisiti da soddisfare, sono disponibili diverse tipologie di virtualizzazione: 
- Full virtualization: colui di cui abbiamo parlato finora
- Para virtualization
- Operating System virtualization
Gli ultimi due li vedremo più avanti, comunque sono utili ai fini della scalabilità perché utilizzano diversi livelli di astrazione. 
La differenza principale tra i tre riguarda le diverse risorse necessarie per creare l’ambiente virtuale. 

## Cloud Virtualized Infrastructure

Nel cloud si ha una grande quantità di server, connessi tra loro tramite LAN e sopra il quale è in esecuzione un Hypervisor per l’esecuzione delle diverse macchine virtuali. 
Tali macchine virtuali possono essere create dinamicamente dai Cloud Costumers. 

Tramite il network LAN ogni server può comunicare o con altri server oppure con il mondo esterno. Un Cloud User che interagisce con l’infrastruttura accederà solo ad una macchina virtuale sarà il software di gestione dell’infrastruttura che si occuparà del load balancing delle richieste e di scalare se necessario. 

## Scaling 

 Un grande vantaggio della virtualizzazione consiste nel fatto che è possibile aggiungere on-demand nuove instanze della stessa macchina virtuale oppure rimuoverne alcune, in seguito al cambiamento delle condizioni di carico che il sistema deve gestire.
 Le due possibilità per lo scaling sono:
 - Orizzontale: si allocano nuove macchine virtuali su server dello stesso tipo, per esempio se il traffico aumenta si installa lo stesso ambiente su un altro server e lo si affianca al primo per gestire le richieste. 
 - Verticale: non si aggiungono nuove macchine virtuali ma si aggiungono nuove risorse all’hardware esistente. Ad esempio questo equivale ad aggiungere nuovi banchi di RAM ad un server già operativo.

Nell’ambito Cloud lo scaling orizzontale è quello più utilizzato.

| Scaling Orizzontale                            | Scaling Verticale                                        |
| ---------------------------------------------- | -------------------------------------------------------- |
| Meno costoso                                   | Più costoso                                              |
| Nuove risorse immediatamente disponibili       | Nuove risorse più lente ad essere allocate               |
| Permette la replicazione e scaling automatico  | Richiede l’intervento dell’uomo e maggiori step di setup |
| Necessarie nuove risorse IT                    | Non sono necessarie nuove risorse IT                     |
| Non è limitato dal limite fisico dell’hardware | È limitato dal limite fisico dell’hardware                                                         |

## Misurazione (Metering)

La virtualizzazione ha un altro grandissimo vantaggio cioè quello di permettere di misurare quanto una risorsa è stata effettivamente utilizzata, cosa impossibile senza questo meccanismo.
Sono infatti sufficienti giusto qualce linea di codice sull’Hypervisor per capire a quanto ammonta il consumo giornaliero di una specifica risorsa hardware.