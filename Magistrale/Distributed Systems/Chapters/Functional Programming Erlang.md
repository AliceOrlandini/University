### Principi e concorrenza nella programmazione funzionale

Un paradigma di programmazione è un insieme di principi e strategie che definiscono un *approccio* per risolvere problemi informatici. In pratica, è una modalità di pensiero che guida il modo in cui sviluppiamo e organizziamo il codice per raggiungere un determinato obiettivo. La scelta del paradigma influisce non solo sulla struttura del programma, ma anche sulle sue *performance* e sulla sua *manutenibilità*.

I principali paradigmi si collocano su un ampio spettro, con due estremi significativi:
- **Paradigma imperativo**: il più comune nella programmazione tradizionale, si basa su una sequenza di istruzioni che modificano lo *stato del programma* e controllano il flusso di esecuzione. In questo approccio, il programmatore indica esplicitamente come il calcolo deve essere eseguito, passo dopo passo.

```python
if(x > 0): 
	result = 15/3
else:
	result = 2*3
result = result - 1
```

- **Paradigma funzionale**: in questo modello, non esiste uno stato mutabile; le variabili, una volta assegnate, restano costanti. L'elaborazione avviene tramite la valutazione di espressioni piuttosto che attraverso l'esecuzione di comandi. Questo approccio riduce la possibilità di *effetti collaterali*, rendendo il codice più *prevedibile* e spesso più adatto alla parallelizzazione.

```erlang
(15/3 if x > 0 else 2*3) - 1
(5 if 7 > 0 else 6) - 1
5 - 1
4
```

Altri paradigmi, come quello logico o orientato agli oggetti, si trovano tra questi due estremi, ciascuno con caratteristiche specifiche per affrontare problemi in modo diverso.

Vediamo ora alcuni concetti della programmazione funzionale che non sono comuni in quella imperativa.

#### Expression & Referential Transparency

Le computazioni sono valutazioni di espressioni.
Un'espressione è detta referentually transparent se può essere sostituita con il suo valore senza cambiare il comportamento del programma, in questo caso tale espressione non ha side effects.

Una funzione è detta pura se la sua chiamata è referentially transparent e quindi non ha side effects.

Se vale la transparency referential è possibile formalizzare il programma come un rewriting system (oggetti + regole per modificarli). Ciò è utile per automatic verification, ottimizzazioni e parallelizzazione.
Si può inoltre utilizzare memo-ization: ièpsizzia

### Introduzione ad Erlang

### Il modello Actor

### Going concurrent & distributed actually