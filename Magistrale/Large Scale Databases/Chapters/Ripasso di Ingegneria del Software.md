# Processo di progettazione

Il processo di progettazione si compone delle seguenti fasi di lavoro:
1. **Elenco dei requisiti**: si discute col cliente in merito ai requisiti che l'applicazione dovrà soddisfare.
2. **Definizione dei requisiti**: si definiscono i requisiti individuati in modo formale, tali requisiti si dividono in *funzionali* e *non funzionali*.
3. **Definizione dei casi d’uso**: si individuando gli attori e le azioni che intraprendono, spesso questa operazione viene effettuata tramite diagrammi.
4. **Analisi del Workflow e Data Modelling**: identificazione delle strutture dati principali e le relazioni tra loro. 
5. **Rifiniture**: si ampliano le strutture individuate al punto precedente, ad esempio si decide quale database utilizzare.
6. **Implementazione** e **test**: si sviluppa e si testa l'applicazione con i casi d'uso individuati al punto 3.
7. **Pubblicazione**: si mette in produzione l'applicativo.
8. **Manutenzione**: dopo la pubblicazione dell'applicativo è molto probabile che ci sia bisogno di manutenzione.

Tramite questa procedura formale, dalla definizione dei requisiti arriviamo all’applicazione finale. 
Il ruolo dell'ingegnere del software è quello di individuare:
- L'architettura software ed hardware
- I linguaggi di programmazione
- **Il sistema di gestione del database**
## Metodo AGILE

In alcune aziende vengono utilizzati approcci differenti come ad esempio l’**Agile**. Il suo funzionamento è riassunto dalla seguente immagine:

![Agile | center | 300](https://blog.esker.it/wp-content/uploads/2020/04/agile-ita.png)

## Requisiti funzionali

I requisiti funzionali sono quelli che descrivono *le funzionalità principali* dell’applicazione ad alto livello. 
Sono ciò che l’applicazione fa, *non come lo fa* e dipendono dal tipo di software, utente, sistema. 
### Esempio
Ecco alcuni esempi di requisiti funzionali:
- Il sistema invia una mail di conferma quando un ordine viene completato.
- Il sistema consente ai visitatori per il blog di iscriversi alla newsletter.

## Requisiti non funzionali

I requisiti non funzionali descrivono le *proprietà* e i *vincoli* del sistema. Ad esempio il tempo di risposta o le caratteristiche dello storage ma possono anche riguardare il linguaggio di programmazione o il metodo di progettazione da utilizzare. 
#Attenzione I requisiti non funzionali possono essere più critici di quelli funzionali perché se non vengono rispettati il sistema potrebbe diventare inutilizzabile (e quindi inutile).

I requisiti non funzionali si dividono in:
1. **Requisiti di prodotto**: per esempio un vincolo in termini di latenza che deve essere rispettato. 
2. **Requisiti di organizzazione**: ad esempio l’utilizzo di alcuni standard di programmazione.
3. **Requisiti esterni**: ad esempio requisiti legislativi. 

## Casi d’uso

I casi d'uso rappresentano un metodo formale per descrivere come l’applicazione interagisce con il mondo esterno. 
Un singolo caso d'uso illustra le attività che un utente compie con il sistema.
Bisogna quindi identificare gli attori, le azioni che compiono e infine i passaggi che succedono tra attori e azioni. 
Tutto ciò va fatto in modo non troppo dettagliato, bisogna descrivere solo cosa si fa e non come lo si fa.

> Un attore è un *utente della piattaforma* o un *sistema esterno* che interagisce col nostro sistema al fine di ottenere valore.

Un attore può essere anche il tempo; ad esempio un’azione che si ripete ogni 10 minuti ha come attore proprio il tempo.

### Include ed Extend

Se si utilizza l'UML per rappresentare i casi d'uso, i collegamenti tra le azioni si dividono in:
- **include**: un’azione che *sicuramente* verrà effettuata.
- **extend**: un’azione che *può* essere effettuata oppure no. 
Entrambe *non sono esclusive*, ciò significa che un’azione può essere inclusa/estesa da altre azioni.
Per capire quale utente può/deve effettuare una certa azione si possono usare simboli o colori diversi.
Si definisce *caso d'uso base* quello che include o estende altri casi d'uso.
#Attenzione Non è un diagramma sequenziale, dice solo quali azioni devono/possono essere intraprese, ma non specifica in che ordine o come. In quest'ultimo caso si usano i diagrammi delle sequenze (che però non vedremo). 

## Data Modelling

I modelli forniscono delle *strutture formali*, *descrizioni dei dati* e delle *relazioni tra loro*. Ciò è indipendente dal tipo di database utilizzato. 
La creazione del modello dei dati comprende 3 passaggi:
1. **Analisi dei dati**: descrizione dei dati con cui abbiamo a che fare.
2. **Creazione del modello relazionale**: è un modello concettuale e indipendente dall’implementazione del database.
3. **Conversione**: si converte il modello in un database reale. 
In questo corso useremo il diagramma UML invece dell’ER visto a basi di dati.
#Attenzione Nel diagramma UML le cardinalità che sono invertire rispetto all’ER. 

Per creare questi diagrammi non ci sono regole precise, il professore però predilige modelli orientati alla manipolazione dei dati. Questo significa che quando si progetta bisogna pensare all'implementazione in termini di metodi java e query.
Per esempio, il professore preferisce un modello: 
- **browse**: per trovare tutti gli utenti.
- **find**: per trovare un utente specifico.
- **view**: per visualizzare i dettagli di quell'utente.

## Esercizio

Data la seguente descrizione da parte del cliente:
1. definire i [[#Requisiti funzionali|requisiti funzionali]] e [[#Requisiti non funzionali|requisiti non funzionali]].
2. disegnare il diagramma dei [[#Casi d’uso|casi d’uso]].
3. disegnare il [[#Data Modelling|modello dei dati]].

Il sistema consente all’azienda di vendere i propri prodotti online.
Possono essere venduti diversi tipi di prodotti e ciascun prodotto è descritto da alcuni attributi comuni, come prezzo, ID prodotto e numero di pezzi disponibili, e da alcuni attributi specifici. 
Il manager dell’azienda può aggiungere, rimuovere e aggiornare i prodotti. 
Gli utenti non registrati possono solo sfogliare i prodotti venduti nel sistema e visualizzarne le descrizioni.
Gli utenti registrati possono inoltre selezionare i prodotti ed inserirli nel carrello. 
I prodotti potrebbero essere rimossi dal carrello. 
Quando un utente registrato decide di acquistare i prodotti inseriti nel carrello, viene creato un ordine, viene richiesto all’utente di effettuare il pagamento finale e il carrello viene svuotato. 
Ogni utente registrato può visualizzare l'elenco dei propri ordini. 
Ogni ordine può avere uno stato diverso (bozza, pagato, in preparazione, spedito, consegnato, annullato). 
Il sistema può offrire alcuni servizi di analisi relativi a prodotti, ordini e utenti.

### Soluzione

I *requisiti funzionali* sono:
- Il sistema consente di visualizzare l’elenco dei prodotti venduti dall’azienda.
- Il sistema consente di visualizzare le descrizioni, i prezzi e altri attributi specifici dei prodotti venduti dall’azienda. 
- Il sistema consente di aggiungere al carrello i prodotti.
- Il sistema consente di rimuovere dal carrello i prodotti.
- Il sistema consente di comprare i prodotti presenti nel carrello.
- Il sistema consente di visualizzare l’elenco dei propri ordini.
- Il sistema consente di visualizzare lo stato di un ordine.
- Il sistema consente di aggiungere prodotti.
- Il sistema consente di rimuovere prodotti.
- Il sistema consente di aggiornare i prodotti. 
- Il sistema consente di visualizzare analisi sui prodotti, ordini e utenti. 

I *requisiti non funzionali* sono: 
- Un utente per aggiungere un prodotto al carrello deve essere registrato. 
- Un utente per rimuovere un prodotto dal carrello deve essere registrato. 
- Un utente per comprare un prodotto presente nel carrello deve essere registrato. 
- Un utente per visualizzare l’elenco degli ordini deve essere registrato. 
- Un utente per visualizzare lo stato di un suo ordine deve essere registrato.
- Per aggiungere i prodotti bisogna essere manager.
- Per rimuovere i prodotti bisogna essere manager.
- Per aggiornare i prodotti bisogna essere manager.
- Per visualizzare le analisi su prodotti, ordini e utenti bisogna essere manager. 

Il diagramma dei casi d’uso e il modello dei dati sono sulle slide.