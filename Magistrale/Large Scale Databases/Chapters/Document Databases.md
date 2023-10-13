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

# Vantaggi dell’XML

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
