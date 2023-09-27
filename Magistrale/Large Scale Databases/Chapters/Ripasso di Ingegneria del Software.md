# Processo di progettazione

Il processo di progettazione si compone delle seguenti fasi di lavoro:
1. **Elenco dei requisiti** dal cliente.
2. **Definizione dei requisiti** in modo formale: i requisiti possono essere funzionali e non funzionali.
3. **Definizione dei casi d’uso**: spesso effettuato tramite diagrammi individuando gli attori e le azioni che intraprendono.
4. **Analisi del Workflow e Data Modelling**: identificazione delle strutture principali e le relazioni tra loro, solo qui si decide quale database utilizzare. 
5. **Rifiniture**: incluso il design del database.
6. **Implementazione** e **test**
7. **Pubblicazione**
8. **Manutenzione**

In questo modo dalla definizione dei requisiti arriviamo all’applicazione finale. 

## Metodo AGILE

In alcune aziende vengono utilizzati approcci differenti come ad esempio l’Agile.

[Agile]()

## Requisiti funzionali

Descrivono le funzionalità principali dell’applicazione. Sono ciò che l’applicazione fa, non come lo fa. dipendono dal tipo di software, utente, sistema. 
Descrivono la relazione tra input ed output ma non come.

### Esempio
Ecco alcuni esempi di requisiti funzionali:
- Il sistema invia una mail di conferma quando un ordine viene completato.
- Il sistema consente ai visitatori per il blog di iscriversi alla newsletters

## Requisiti non funzionali

Descrivono le proprietà e i vincoli del sistema come ad esempio il tempo di risposta o le caratteristiche dello storage. 
Possono anche riguardare il linguaggio di programmazione o il metodo di progettazione da utilizzare. 
#Attenzione! I requisiti non funzionali possono essere più critici di quelli funzionali perché se non vengono rispettati il sistema diventa inutilizzabile (e quindi inutile).

I requisiti non funzionali si dividono in:
1. **Requisiti di prodotto**: per esempio un limite di latenza che deve essere rispettato. 
2. **Requisiti di organizzazione**: ad esempio l’utilizzo di alcuni standard di programmazione.
3. **Requisiti esterni**: ad esempio requisiti legislativi. 

## Casi d’uso

Sono metodi formali per descrivere come l’applicazione interagisce con il mondo esterno.
Bisogna identificare gli attori, le azioni che compiono e infine i passaggi che succedono tra attori e azioni, non in modo dettagliato però.

> Un attore è un *utente della piattaforma* o un *sistema esterno* che interagisce col nostro che interagisce col sistema al fine di ottenere valore.

Un attore può essere anche il tempo se ad esempio se si ha un’azione che si ripete ogni 10 minuti.

### Include ed Extend

I collegamenti tra le azioni si dividono in:
- **include**: un’azione che *sicuramente* verrà effettuata.
- **extend**: un’azione che *può* essere effettuata oppure no. 
Entrambe *non sono esclusive*, ciò significa che un’azione può essere inclusa/estesa da altre azioni.
Si possono usare colori diversi per indicare quale attore ha il permesso di eseguire un’azione.
Lo use case base è quello che include o estende altri use case
#Attenzione! Non è un diagramma sequenziale, dice solo che quelle azioni devono/possono essere intraprese ma non in che ordine o come. In questo caso si usano i diagramma delle sequenze. 

## Data Modelling

I modelli forniscono delle *strutture formali*, *descrizioni dei dati* e delle *relazioni tra loro*. Ciò è indipendente dal tipo di database utilizzato. 
La creazione del modello dei dati comprende 3 passaggi:
1. **Data analysis**: descrizione dei dati con cui abbiamo a che fare.
2. **Creazione del modello relazionale**: è un modello concettuale e indipendente dall’implementazione del database.
3. **Conversione**: si converte il modello in un database reale. 
In questo corso useremo il diagramma UML invece dell’ER visto a basi di dati.

Sulle slide si trovano alcuni esempi.
#Attenzione! le cardinalità che sono invertire rispetto all’ER. 

