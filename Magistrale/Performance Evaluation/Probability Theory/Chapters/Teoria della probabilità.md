# Spazio campione ed Eventi

## Definizioni

### Probabilità

Il concetto di **Probabilità** può essere definito in diversi modi, quello che useremo noi è quello di *frequenza relativa*:

> Ripetiamo un esperimento *un numero elevato di volte* $N$ in *condizioni indipendenti* (ovvero che il risultato di un esperimento non influenza il risultato degli esperimenti successivi) allora, definito $k$ il numero di *realizzazioni* di un certo evento $E$, *la probabilità di tale evento sarà*: $$P(E) = \lim_{N \to \infty} \frac{k}{N}$$

Questa definizione di probabilità è puramente matematica poiché nella realtà è impossibile effettuare un numero infinito di esperimenti per cui la probabilità di un esperimento è generalmente determinato in base ad una *conoscenza a priori*. 
Ad esempio l’esperimento “lancio di una moneta” per motivi di simmetria darà una probabilità del 50% di ottenere “testa”.

### Esperimento Casuale 

> Definiamo *esperimento casuale* un esperimento il cui risultato (*l’outcome*) non è predicibile a priori.

Attenzione! L’outcome di un esperimento è definito solo nella mente dell’osservatore.
Infatti, quando ci troviamo davanti ad un esperimento, la prima cosa da fare è definire lo *spazio dei risultati*. Questo aspetto è importante perché dato un esperimento possiamo avere più spazi a seconda di ciò che vogliamo analizzare in quel momento.

### Sample Space

> Definiamo *sample space* o spazio campione $S$ il set di *tutti i possibili outcome* di un esperimento.

Ad esempio nel caso dell’esperimento “lancio di un dado” avrò $S = \{1,2,3,4,5,6\}$ visto che il dato ha 6 facce che numeriamo da 1 a 6. 

### Evento

> Definiamo *evento* $E$ un *sottoinsieme* (subset) dello spazio campione (sample space) $S$.

Nell'esempio “lancio di un dado” un possibile evento può essere $E = \{1,2,5\}$ cioè l’evento in cui il risultato del lancio del dato è 1 o 2 oppure 5.

Diremo che un evento $E$ *si è verificato* se l’outcome dell’esperimento è incluso in $E$.

Due eventi particolari sono:
- L’evento **nullo**: rappresentato dal set vuoto $\emptyset$, questo evento non ha outcome per cui è impossibile che si verifichi.
- L’evento **certo**: rappresentato dal sample space $S$, questo evento include tutti i possibili outcome per cui si verificherà sempre. 

## Operazioni tra set 

Definiamo due eventi $E$ ed $F$ appartenenti al sample space $S$, è possibile eseguire tre *operazioni algebriche* tra i due set (set ed eventi verranno usati come sinonimi):
- **Unione**: $E \cup F$ il risultato è un set che include *tutti* gli outcome di $E$ e tutti gli outcome id $F$.
- **Intersezione**: $E \cap F = EF$ il risultato è un set che include *solo gli outcome in comune* tra $E$ ed $F$.
- **Complemento**: $E^\complement = S/E$ il risultato è il set di outcome che non sono in $E$ infatti può essere visto come il sample space al quale vengono tolti gli outcome di $E$.

Le proprietà di questi operatori sono:
- Unione e intersezione sono *associative* e *commutative*.
- Il complemento è *involutivo*, ovvero: $(E^\complement)^\complement = E$
- Leggi di *De Morgan*: $$ (A \cup B)^\complement = A^\complement \cap B^\complement$$ $$(A \cap B)^\complement = A^\complement \cup B^\complement$$

Attenzione! Non confondersi tra *evento* ed *outcome*. 
Un outcome è un **elemento** del sample space. 
Un evento è un **subset** del sample space. 
Si può ovviamente associare un evento a ciascun outcome ma bisogna mantenere i due concetti separati.

### Esercizio 1

# Assiomi della probabilità

> La *probabilità* è un *numero* associato ad un *evento* che descrive la *frequenza* con cui si verifica tale evento.

Gli assiomi su cui si basa la definizione di probabilità sono:
1. $0 \le P(E) \le 1$ 
2. $P(S) = 1$
	1. Se $E_i$ ed $E_j$ sono due eventi tali che $E_iE_j = \emptyset$ se $i \not= j$ (ovvero sono eventi disgiunti, cioè la cui intersezione è nulla) allora $P(\bigcup_iE_i) = \sum_{i} P(E_i)$

Esempio dell’ultimo assioma:
$P(E_i \cup E_j) = P(E_i) + P(E_j)$
se $E_i \cap E_j = \emptyset$
cioè la probabilità dell’unione di eventi disgiunti è la somma delle probabilità dei singoli eventi.

## Proprietà derivanti dagli assiomi

Ecco alcune proprietà derivanti dagli assiomi:
1. $P(E^\complement) = 1 - P(E)$ poiché $E$ ed il suo complemento sono sicuramente disgiunti.
2. $P(E_1 \cup E_2) = P(E_1) + P(E_2) - P(E_1E_2)$ si può dimostrare facilmente guardando i diagrammi di Venn.

### Esercizio 2

# Sample space con eventi ugualmente equiprobabili

In alcuni casi il sample space di un esperimento casuale ha due proprietà:
1. Ha una *cardinalità finita* $N = |S|$.
2. Ha *outcomes ugualmente equiprobabili* (*equally likely outcoes*).

Ad esempio quando si lancia una moneta la cardinalità di $S$ è 2 (testa o croce) e i risultati sono equiprobabili (50% testa e 50% croce) ovvero $p = \frac{1}{N}$.

Per essere più precisi dovremmo dire che $N*p = 1$ considerando che tutti gli eventi sono mutuamente esclusivi e l’unione di tutti gli eventi è il sample space  che ha probabilità pari a 1.

Se siamo in un modello *a probabilità uniforme* (vedremo più avanti esempi di modelli) allora si parla di sample space con *equally likely outcomes* e la probabilità di un evento $E$ sarà: $$P(E) = \frac{|E|}{N}$$ con $N = |S|$.

Quindi per definire la probabilità di un evento ci basta *contare* il numero di outcomes inclusi in quell’evento e dividerli per la cardinalità del sample space.

## Basic Principle of Counting

Visto che, come visto al punto precedente, per trovare la probabilità di un evento $E$ quando il sample space ha cardinalità finita e outcomes ugualmente equiprobabili dobbiamo contare il numero di outcome e dividere per la cardinalità del sample space, allora ci conviene introdurre un *principio* che ci permette di contare più facilmente gli outcome di un evento.

Il *principio base del conteggio* afferma che: 

> Dato un *esperimento* $C$ composto da due *sotto-esperimenti* $C_1$ e $C_2$, aventi rispettivamente $N_1$ e $N_2$ possibili outcome, allora il numero di possibili outcome dell’esperimento $C$ è uguale a $N_1*N_2$.

Questa definizione può essere generalizzata da 2 a $k$ esperimenti: $$C = \prod_{i = 1}^{k}N_i$$
### Esercizio 3

