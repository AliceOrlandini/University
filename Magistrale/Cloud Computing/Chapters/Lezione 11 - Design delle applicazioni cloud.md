
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