# Funzioni Principali

Questa è un’altra architettura di database noSQL. 
Il problema dei key-value databases era che se si volevano fare query un po’ più complesse bisognava scriverle dal codice (e scrivere codice significa commettere errori). 
Vediamo quindi un altro paradigma che garantisce una grande flessibilità ma fornisce una struttura dati precisa che va in contro alla OOP. 
Nei documenti possiamo definire attributi come e quando vogliamo, *schema-less*.

# XML Documents

XML è acronimo di **eXtensible Markup Language**, è un linguaggio di markup ovvere un linguaggio in cui si va a specificare la struttura e il contenuto del documento.
Si basa su tags (come l’html) per gli attributi che possono contenere tutti i tipi di informazioni. 

In passato veniva usato anche per il protocollo SOAP (Simple Object Access Protocol) un protocollo in cui si vanno a incapsulare le informazioni in un file xml (???).

Questo linguaggio viene usato dalla Microsoft in *docx*, *xlsx*, *pptx*.

## Vantaggi dell’XML

Per prima cosa un file xml è facile da trasmettere, poi un file può essere visualizzato con vari software.
Infine, questi documenti possono essere uniti facilmente quindi è favorita la modularizzazione in file più piccoli.
Inoltre, dentro al tag si possono inserire delle proprietà.
Esiste infine un intero ecosistema per lavorare con questi file:
- **XPath**: utile per navigare negli elementi di un file XML.
- **XQuery**: è il linguaggio utilizzato per effettuare query su file XML.
- **XML Schema**: è un file XML “speciale” che descrive quali requisiti (la struttura) deve avere un file XML.
- **XSLT**: un linguaggio per trasformare il file XML in altri formati come ad esempio l’HTML.
- **DOM**: per renderizzare visivamente il file 

Un file XML può essere rappresentato come un albero (sia logicamente che in memoria).

## XML Databases

Un Database XML è una piattaforma che implementa degli standard XML come *XQuery* e *XSLT*.
Deve fornire servizi per l’immagazzinamento, l’indicizzazione e la sicurezza dei file XML. 
È importante notare che questa tipologia di database non rappresentano un alternativa ai database relazionali. 
I dati all’interno del database sono byte, quindi il risultato di una query non sarà l’immagine ma i byte dell’immagine che poi con l’opportuno software devono essere interpretati. 

## Inconvenienti 

Un primo inconveniente è che è un linguaggio estremamente verboso e ripetitivo (un tag va ripetuto tutte le volte e almeno 2 volte per dato). Per questo motivo, un file occupa tantissimo spazio e ha bisogno di molto tempo per essere elaborato. 

Per ovviare a ciò, si è passati ai JSON Documents.

# JSON Documents

JSON è l’acronimo di **JavaScript Object Notation** e viene usato per formattare i dati. Nel nome c’è (la bestia nera) *JavaScript* ma (stiamo tranquilli perché) JSON non è dipendente dal linguaggio usato. 
È una collezione di name-value information in cui i value possono essere molto complicati. 

Un po’ di nomenclatura:
- Un **oggetto** è un insieme di coppie di name-value non ordinate (l’ordine non importa).
- Un **array** è una collezione di values ordinati. 
- Un **value** può essere una stringa, un numero, booleano, null, oggetto o un array. 
- Le **stringhe** devono essere specificate tra doppie virgolette.
- I **numeri** sono decimali (non sono accettati octali o esadecimali).
- Un **oggetto** può contenere altri oggetti. 

## Confronto tra JSON e XML 

**Similitudini**:
- facili da leggere dagli umani
- sintassi molto semplice
- gerarchici
- indipendenti dal linguaggio utilizzato
- supportati da API nei principali linguaggi di programmazione

**Differenze**:
- la sintassi dei due è differente
- JSON è meno verboso
- JSON include gli array
- i nomi in JSON non possono essere uguali alle parole riservate per JavaScript. 

## JSON Database

In questo caso i dati sono memorizzati in formato JSON e un *documento* è l’unità base che viene memorizzata in questo tipo di database.
Gli array possono contenere documenti quindi è facile ottenere una struttura gerarchica di documenti.
Una **collezione** non è un set di documenti dello stesso tipo ma una collezione di documenti che condividono uno scopo comune. Infatti, possiamo avere schemi polimorfici ovvere documenti che contengono altri documenti completamente diversi in termini di contenuto. 
Anch’esso è *schema-less*. 
MongoDB usa questo tipo di database. 

I vantaggi di usare questo tipo di database sono che ho una grande flessibilità per gestire strutture dati molto differenti.

Lo svantaggio principale è il fatto che non ci sono regole per la struttura dei dati (ad esempio se un valore deve rispettare un certo tipo o un range, nessuno controlla queste proprietà).

#Attenzione non usare una struttura che si basa sulle foreign key perché all’esame si viene bocciati instant. 

