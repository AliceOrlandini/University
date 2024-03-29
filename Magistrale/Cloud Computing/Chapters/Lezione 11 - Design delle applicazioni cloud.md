
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

I ruoli quando si parla di SOA sono principalmente 
## Broker


## Vantaggi 

