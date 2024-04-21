
## Design di applicazioni cloud

Le applicazioni cloud consistono in sistemi distribuiti composti da molteplici componenti che collaborano per fornire un servizio agli utenti. 
Tuttavia, l'implementazione e l'integrazione di tali componenti in un ambiente cloud possono risultare complesse, portando a costi più alti e tempi più lunghi. Per mitigare il rischio di prolungati tempi di sviluppo, e quindi di un ritardo nel lancio sul mercato (il cosiddetto "long time to market"), vengono adottate specifiche tecniche e modelli. 
Queste metodologie non solo facilitano l'implementazione iniziale, ma supportano anche la manutenzione a lungo termine dell'applicazione.

Un componente dell'applicazione deve essere progettato considerando i seguenti requisiti:
- **Interoperabilità**: il componente deve essere in grado di interagire e comunicare con altri componenti dell'applicazione o con sistemi esterni in modo efficace e senza problemi, tramite un'interfaccia standard che può essere facilmente invocata da altri componenti dell'applicazione.
- **Scalabilità**: il componente deve essere progettato per interagire dinamicamente con un numero crescente di componenti, sfruttando al massimo la scalabilità orizzontale messa a disposizione dal cloud.
- **Componibilità**: il componente deve essere progettato in modo modulare e indipendente, in modo da poter essere facilmente combinato e riutilizzato con altri componenti per creare funzionalità più complesse e personalizzate all'interno dell'applicazione.

## Service-Oriented Architecture

Negli ultimi anni, è emersa una metodologia specifica per il design del software delle applicazioni cloud, chiamata **Service-Oriented Architecture** (**SOA**). SOA non è un tool o un framework, ma piuttosto un approccio modulare al design del software.

In SOA, i componenti software sono definiti come *services*, che sono moduli autocontenuti che implementano funzionalità specifiche. Le applicazioni SOA sono costruite come una collezione di questi services, il che porta a una struttura fortemente modulare rispetto all'approccio tradizionale di avere un unico grande programma.

Questo approccio modulare non solo offre un elevato livello di flessibilità, ma permette anche l'indipendenza nello sviluppo dei services utilizzando linguaggi di programmazione diversi, rendendo SOA particolarmente adatto per ambienti eterogenei e distribuiti come le applicazioni cloud.

I *services* che costituiscono l'applicazione comunicano attraverso *message passing*. Ogni messaggio segue uno schema che definisce il formato per lo scambio di informazioni. 
Successivamente, il messaggio viene gestito internamente in modo "nascosto", il che significa che il mittente del messaggio non è a conoscenza dei dettagli implementativi del modulo destinatario. L'unico aspetto che deve essere pubblico è il formato del messaggio. 
Questo approccio garantisce un alto grado di disaccoppiamento tra i componenti del sistema e favorisce la modularità e la manutenibilità dell'applicazione.

## Ruoli

Quando si parla di SOA, i ruoli principali sono principalmente due: **consumer** e **provider**. Il consumer invia una richiesta di servizio a un provider tramite message passing, e il provider restituisce la risposta contenente i risultati richiesti. 
È importante notare che un service può svolgere sia il ruolo di consumer che di provider.

Di solito, i provider e i consumer non interagiscono direttamente l'uno con l'altro. Invece, viene utilizzato un modulo software addizionale chiamato **broker**. 
Il broker ha il compito di creare un service catalog, una lista che elenca tutti i servizi disponibili. Quando un consumer desidera richiedere un servizio, contatta il service broker, il quale fornisce tutte le informazioni necessarie sul servizio richiesto, inclusi il formato del messaggio. Una volta ricevute queste informazioni, il consumer può dialogare direttamente con il provider e viceversa. Questo approccio facilita la gestione e la scoperta dei servizi all'interno dell'architettura SOA.

## Vantaggi 

Come precedentemente menzionato, l'implementazione interna dei servizi rimane privata all'esterno dei confini del servizio stesso. Questo offre il vantaggio di semplificare la correzione di bug senza influenzare gli altri servizi, purché l'interfaccia per lo scambio di messaggi rimanga invariata.

Questa separazione consente una caratteristica chiamata **loose coupling** (accoppiamento debole), che permette di collegare e scollegare servizi specifici dall'applicazione senza sforzo. 
Questo significa che i servizi possono essere aggiunti, rimossi o sostituiti con facilità, senza causare impatti significativi sugli altri componenti dell'applicazione. 

Questa caratteristica rende il sistema flessibile ai cambiamenti ed espandibile. L'approccio SOA consente anche la composability, poiché un sistema può essere assemblato utilizzando una combinazione di servizi esistenti.

Il paradigma di comunicazione basato su messaggi permette infine che la comunicazione tra servizi avvenga in modo *stateless*: ogni modulo tratta ogni transazione in modo separato, indipendentemente da ogni altra transazione. Questo significa che ogni richiesta è gestita come un'entità isolata e non è necessario mantenere uno stato globale tra le transazioni. Ciò semplifica la gestione delle transazioni e favorisce la scalabilità del sistema, consentendo di gestire un alto volume di richieste in modo efficiente e affidabile.

## Web Services

Il concetto di Web Service (WS) si riferisce a un'applicazione modulare auto-contenuta e auto-descrittiva progettata per essere accessibile tramite il web. 
In sostanza, i Web Service rappresentano un'implementazione dell'Architettura Orientata ai Servizi (SOA). Il gruppo di lavoro W3C ha definito un Web Service come un sistema software progettato per facilitare l'interazione *interoperabile* e *automatica* tra macchine all'interno di una rete.

Il web offre una serie di meccanismi fondamentali:
1. **Identificare le risorse nella rete**: attraverso gli URI (in particolare gli URL), è possibile individuare in modo univoco le risorse su Internet, consentendo agli utenti di accedervi facilmente.
2. **Trasferire dati**: grazie ai protocolli di comunicazione come HTTP e HTTPS, è possibile trasferire dati da un punto all'altro della rete in modo sicuro ed efficiente.
3. **Linguaggio di markup per codificare le informazion**i: HTML è il linguaggio di markup principale utilizzato per strutturare e visualizzare le informazioni sul web.

## XML

XML, acronimo di eXtensible Markup Language, è un linguaggio di markup basato su testo che stabilisce un insieme di regole per codificare una vasta gamma di informazioni in un formato che può essere letto sia dagli esseri umani che dai computer. 
Definisce una sintassi e specifica come le informazioni devono essere strutturate all'interno di un documento. Sebbene esistano regole opzionali, non sono obbligatorie per la validità di un documento XML.

Ciò che distingue XML è la sua natura estensibile: il set di tag non è predefinito, il che significa che gli utenti possono creare i propri tag in base alle esigenze specifiche dell'applicazione o del contesto.

La struttura di un documento XML è *gerarchica* e si compone di elementi che sono delimitati da un tag di apertura e uno di chiusura. Tipicamente, il documento inizia con un elemento root, chiamato XML declaration, che fornisce informazioni sulle proprietà del documento.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<GeocodeResponse>
	<status>OK</status>
	<result>
	<type>establishment</type>
	<type>point_of_interest</type>
	<geometry>
		<location type="GPS">
			<lat>43.7210349</lat>
			<lng>10.3898890</lng>
		</location>
	</geometry>
	</result>
</GeocodeResponse>
```


La rigorosa sintassi XML consente di determinare se un documento è "well-formed", ma è importante anche verificare che il documento sia valido, ovvero che abbia un significato coerente con la sua rappresentazione. Per garantire la validità, si utilizza un secondo documento chiamato XML Schema.
Nel caso dell'esempio precedente, la latitudine e la longitudine sono obbligatorie, questo requisito può essere specificato nell'XML Schema, che definirebbe la struttura e le regole che il documento XML deve seguire affinché sia considerato valido. Inoltre, è ampiamente impiegato per favorire l'interoperabilità tra sistemi e applicazioni.

## Web Services Interaction

Un web service possiede un'interfaccia descritta in un formato eseguibile dalle macchine, chiamato Web Services Description Language (WSDL).
Questo linguaggio è utilizzato per comunicare con il broker, un componente che funge da registro globale per la pubblicizzazione e la scoperta dei web service. 
Attraverso WSDL, ogni provider di servizi si registra presso il broker, mentre il consumatore di servizi lo utilizza per individuare un servizio specifico di interesse. 
Infine, il consumatore e il provider interagiscono tramite il protocollo SOAP (Simple Object Access Protocol), che stabilisce uno standard per lo scambio di messaggi tra loro.

## SOAP

SOAP fornisce uno standard per la struttura di impacchettamento per la trasmissione di documenti XML attraverso diversi protocolli Internet. 
Un messaggio SOAP è composto da un elemento root chiamato *envelope*, che contiene l'header con informazioni come quelle relative alla sicurezza, e un body che contiene il vero payload, che può includere informazioni di request/response, oppure una sezione di fault che contiene errori o eccezioni, se presenti.

La trasmissione effettiva del messaggio avviene attraverso il binding XML, che consente di inviare i messaggi tramite il protocollo HTTP. È possibile utilizzare i metodi HTTP GET o POST per trasmettere i dati.

## WSDL

Il WSDL descrive in un formato standard l'interfaccia e l'insieme di operazioni supportate da un Web Service. Standardizza anche i parametri di input e output per ciascuna operazione del servizio. 
Attraverso il WSDL, i client possono comprendere automaticamente:
- Le funzionalità offerte dal servizio
- La sua posizione
- Il metodo per invocarlo

Pertanto, ogni volta che un client desidera utilizzare un servizio di cui non ha conoscenza, richiede e analizza il documento WSDL e solo successivamente invoca il servizio tramite SOAP.

L'architettura di un messaggio WSDL non è complessa perché deve gestire una grande varietà di servizi. Il messaggio è un documento XML composto da 2 sezioni: 
1. **Service Interface Definition**:
2. **Service Implementation Definition**:

## REST

