Il prof inizia la lezione dicendo "who cares about history".
Io che ieri ho passato un'ora a sistemare gli appunti sulla storia:

![meme|center|200](https://i.kym-cdn.com/photos/images/newsfeed/002/621/521/c0f.gif)

## Definizione di Cloud Computing

Il termine Cloud Computing oggigiorno si riferisce a molti ambiti quindi può significare tutto o niente. Per noi avrà due significati:
1. Cloud Services: software che sono in esecuzione su architetture distribuite.
2. Cloud Infrastructure: l’hardware ed il software che permette di erogare i servizi. 
La definizione che useremo per il Cloud è:
> [!note] Cloud Computing
> Il Cloud Computing è un ambiente IT progettato con lo scopo di fornire in modo remoto risorse IT che siano scalabili, misurabili e accessibili via Internet.

Le risorse IT hardware messe a disposizione sono quelle di base per il funzionamento di un calcolatore, cioè: CPU, RAM e dispositivi di memorizzazione di massa. 
Queste devono essere accessibili dall’utente esclusivamente via Internet. 

Tali risorse vengono utilizzate per fornire servizi, anch’essi accessibili via Internet, dall’utente finale tramite un set di programmi interfaces (API). 

Quindi con Cloud Computing si può indicare sia l’infrastruttura fisica, sia i servizi che vengono erogati tramite tali infrastrutture agli utenti finali. 
Il paradigma utilizzato si chiama XaaS, cioè Everything as a Service, cioè un modello di business in cui si erogano servizi sotto abbonamento.

## Ruoli 

I ruoli principali che individuiamo quando si parla di Cloud Computing sono:
- Cloud Provider: è il proprietario dell’infrastruttura cloud, cioè colui che investe risorse per acquistare l’hardware necessario per realizzare l’infrastruttura al fine di metterla a disposizione per implementare servizi su di essa. 
- Cloud Service Owner: è colui che utilizza l’infrastruttura per erogare il proprio servizio, un esempio potrebbe essere Netflix che eroga il servizio di video streaming sull’infrastruttura cloud di Amazon (AWS).
- Cloud Service User: è l’utente finale che, tramite l’Internet, accede alle API del servizio ospitato sull’infrastruttura cloud. 
Ad esempio se consideriamo Gmail e Google, il cloud provider è anche il cloud service owner perché utilizza la sua stessa infrastruttura per erogare un servizio. 
I ruoli secondari ma comunque presenti invece sono:
- Cloud Resource Administrator: è colui che is occupa della manutenzione dell’infrastruttura, è separato dal cloud provider perché quest’ultimo è solo colui che investe e vuole avere un ritorno economico.
- Cloud Costumer: è una sorta di service owner solo che invece che erogare servizi agli utenti finali usa l’infrastruttura per ospitare dei servizi personali, un esempio può essere il gestionale interno all’azienda. 
- Cloud Auditor: è un’ente esterno che si occupa di certificare la qualità del cloud, in termini di sicurezza e performance, e il rispetto delle norme contrattuali.
- Cloud Carrier: è l’Internet Service Provider, cioè colui che mette a disposizione l’infrastruttura Internet per accedere al Cloud. 

## Il Modello Cloud

Come riportato nella definizione di Cloud Computing, ci sono due parole molto importanti che lo caratterizzano che sono: “infrastrutture IT **scalabili** e **misurabili**”. 
Infatti il cloud adotta un modello di business di tipo utility-based, cioè un pay-as-you-go, questo significa che le risorse effettivamente utilizzate del Cloud Service Owner devono essere misurate in modo da fargli pagare solo ciò che ha utilizzato. 
Per esempio si può scegliere di far pagare la banda utilizzata oppure l’energia consumata o ancora lo spazio di storage occupato. 
Il secondo aspetto riguarda l’allocazione delle risorse che deve essere di tipo dinamico, on-demand e senza l’intervento diretto dell’uomo, quindi il sistema deve essere scalabile dinamicamente.
Le risorse devono poter essere allocate non solo in seguito alla richiesta dell’uomo ma anche dalle macchine (tale macchina è chiamata orchestrator).

Il cloud ha quindi permesso di adottare un nuovo modello di business che consiste nel: 
- Dal punto di vista del Cloud Provider, mettere a disposizione delle risorse hardware per coloro che vogliono erogare servizi e farsi pagare queste risorse in base all’effettivo utilizzo.
- Dal punto di vista del Cloud Service Owner, fornire un servizio ai Cloud User sotto abbonamento e fare margine pagando le risorse utilizzate al Cloud Provider. 
- Il Cloud User utilizza l’Internet messo a disposizione dal Cloud Carrier per accedere all’infrastruttura pagando un abbonamento al Cloud Service Owner. 

L’infrastruttura è realizzata tramite Datacenter cioè degli spazi dedicati per contenere un grande numero di server e le reti LAN per connetterli tra loro. Tale hardware fornisce risorse IT di tra tipologie: 
- Computazionali
- Storage
- Networking

## Multi-tenancy

Le architetture tradizionali sono abilitate all’accesso multi-user, cioè più utenti che accedono contemporaneamente ad un unica macchina con un amministratore che coordina le operazioni. 
Un esempio potrebbe essere un server che riceve le richieste dei client, sarà lui ad occuparsi di servire tutte le richiesta. Ciò equivale ad esempio alla visita di un appartamento, la visita è la richiesta del client e l’appartamento è il server. 

Quando si utilizza un’infrastruttura cloud però si vuole sfruttare al 100% tutte le risorse che si hanno a disposizione come i GB di RAM o i secondi di CPU. 
Quindi ciò che si vorrebbe fare è fare in modo che più persone abitino lo stesso appartamento e che ognuno di loro serva le richieste di competenza. 
Questo approccio è chiamato multi-tenancy.
Ogni amministratore del servizio ha la completa facoltà di scegliere la propria configurazione in termini di sistema operativo e avrà l’impressione di essere l’unico ad utilizzare le risorse hardware. Invece, quello che accade è che più amministratori le stanno utilizzando, ognuno con le proprie scelte in termini di configurazione. 
*“From the point of view of the cloud service owner it seams an INCULATA” cit. Vallati*

Questo è possibile tramite la Virtualizzazione: una tecnica che consente di creare una versione virtuale di qualsiasi risorsa IT (ad esempio una versione virtuale della RAM, della CPU, dell’intero server…).
Con l’utilizzo della virtualizzazione si possono creare delle copie virtuali dell’hardware e fare interagire il service owner con tali copie in modo che abbia l’impressione di averne il completo controllo, ma senza che conosca come vengono poi gestire realmente. 

Generalmente si Ogni ambiente virtuale sopra il quale è installato un sistema operativo si chiama Macchina Virtuale. 