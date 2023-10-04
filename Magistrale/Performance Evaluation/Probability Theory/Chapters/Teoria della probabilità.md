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
3. Se $E_i$ ed $E_j$ sono due eventi tali che $E_iE_j = \emptyset$ se $i \not= j$ (ovvero sono eventi disgiunti, cioè la cui intersezione è nulla) allora $P(\bigcup_iE_i) = \sum_{i} P(E_i)$

Esempio dell’ultimo assioma:
$P(E_i \cup E_j) = P(E_i) + P(E_j)$
se $E_i \cap E_j = \emptyset$
cioè la probabilità dell’unione di eventi disgiunti è la somma delle probabilità dei singoli eventi.

## Proprietà derivanti dagli assiomi

Ecco alcune proprietà derivanti dagli assiomi:
1. $P(E^\complement) = 1 - P(E)$ poiché $E$ ed il suo complemento sono sicuramente disgiunti.
2. $P(E_1 \cup E_2) = P(E_1) + P(E_2) - P(E_1E_2)$ si può dimostrare facilmente guardando i diagrammi di Venn.

### Esercizio 2

Il 28% degli americani fuma sigarette.
Il 7% degli americani fuma sigari.
Il 5% degli americani fuma sia sigari che sigarette.
Qual è la percentuale di non fumatori?

1. Definire gli eventi:
	- E = { fumatori di sigarette } P(E) = 0.28
	- F = { fumatori di sigari } P(F) = 0.07
	- EF = { fumatori sia di sigari che di sigarette } P(EF) = 0.05
	- G = { non fumatori }
	- S = { americani } P(S) = 1
2. Scrivere l'equazione: P(G) = P($(E\cup F)^\complement$) = 1 - P($E\cup F$) = 1 - (P(E) + P(F) - P(EF)) = 0.7
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

Consideriamo un'urna contenente 6 palline nere e 5 bianche. Qual è la probabilità che, estraendo due palline (senza reinserimento), si ottengano una pallina bianca ed una nera? (l'ordine non è importante).

1. Definisco il sample space (ovvero la struttura degli outcomes):
	Numero le palline: 
	- Da 1 a 6 saranno palline nere
	- Da 7 a 11 saranno palline bianche
	L'outcome sarà una coppia $(b_i;b_j)$ tali che $0 \le i \le 11$ e $0 \le j \le 11$ con $i \not= j$ (perché non c'è il reinserimento).
1. Definisco l'evento favorevole:
$E = \{(b_i;b_j), 1 \le i \le 6; 7 \le j \le 11\} \cup \{(b_i;b_j) 1 \le j \le 6; 7 \le i \le 11\}$ 
3. L'insieme S ha cardinalità finita? Sì
4. Gli eventi sono ugualmente equiprobabili? Sì, proprio per come abbiamo definito l'outcome, infatti, *la probabilità che esca una qualsiasi coppia è uguale a quella delle altre coppie*.
5. Determino la cardinalità di S e di E utilizzando il **principio base del conteggio**:
	- Inizio da S, divido in 2 esperimenti e poi calcolo $|S| = N_1 * N_2$:
		1. $C_1$ prima estrazione, quanti elementi ho? $N_1 = 6 + 5 = 11$.
		2. $C_2$ seconda estrazione, quanti elementi ho? Ho una pallina in meno quindi $N_2 = 5 + 5 = 10$ oppure $N_2 = 6 + 4 = 10$.
	- I due eventi che compongono E sono disgiunti? Sì perché non possono verificarsi entrambi, quindi considero i due eventi come separati e poi vado a sommarli $|E| = |E_1| + |E_2|$. Per ogni evento posso applicare il principio base del conteggio:
		1. $|E_1| = N_{11} * N_{12} = 6 * 5 = 30$ 
		2. $|E_2| = N_{21} * N_{22} = 5 * 6 = 30$
6. Infine, calcolo $P(E) = \frac{|E|}{|S|} = \frac{60}{110} = \frac{6}{11}$

### Esercizio 4

Si vogliono allineare su uno scaffale 10 libri di 4 diverse materie. Qual è la probabilità che ponendoli a caso sullo scaffale lo si faccia raggruppandoli per materia (ovvero, prima tutti quelli di matematica, poi tutti quelli di chimica e così via)? (l'ordine delle materie non è importante).
I libri sono:
- 4 di matematica
- 3 di fisica
- 2 di informatica
- 1 di chimica

Anche in questo caso assegno numeri ai libri da 1 a 10, l'outcome sarà una permutazione di libri (non scriviamo gli insiemi come prima perché è troppo lungo).
Ogni permutazione ha la stessa probabilità di uscire rispetto alle altre quindi sono nel modello ad egual probabilità.
1. Contiamo gli outcome del sample space, possiamo considerare 10 sotto-esperimenti, ognuno dei quali ha cardinalità inferiore al precedente (perché la prima volta ho 10 libri, la seconda 9 e così via):
	$|S| = 10 * 9 * 8 * 7 * 6 * 5 * 4 * 3 * 2 * 1 = 10!$
2. Ora calcoliamo la cardinalità di E:
	I libri di matematica possono essere disposti in 4! modi, i libri di fisica in 3! e così via. Poi, devo considerare che l'ordine delle materie non è importante, ovvero per ogni materia avrò 4! possibili modi di ordinarle. Quindi la cardinalità di E sarà:
	$|E| = (4! * 3! * 2! * 1!) * 4!$ 
3. Infine, $P(E) = \frac{|E|}{|S|} = \frac{(4! * 3! * 2! * 1!) * 4!}{10!}$

## Permutazioni e subset di cardinalità nota

Il principio base del conteggio può anche essere usato per risolvere un problema di questo tipo: 
"quante **permutazioni** di $k$ elementi posso fare partendo da un set di $n$ elementi (con $n \ge k$)?".
Come al solito dividiamo in sotto-esperimenti:
1. Per il primo elemento avremo $n$ possibilità quindi $N_1 = n$
2. Per il secondo elemento avremo $N_2 = n - 1$
3. ...
4. Per il $k$-esimo esperimento avremo $N_k = n - (k - 1)$.
Per cui potrò fare: $$S_{k,n} = n * (n-1)* ... * (n - (k-1)) = \frac{n!}{(n-k)!}$$permutazioni di $k$ elementi dato un set di $n$ elementi.
Notare che se fosse $k = n$ otterrei il risultato del precedente esercizio.

Ora proviamo a rispondere alla seguente domanda, leggermente differente:
"quanti **subset** di $k$ elementi posso fare un un set di $n$ elementi (con $n \ge k$)?"
Questa domanda è differente dalla precedente perché in questo caso l'ordine degli elementi non ha importanza. Infatti, se un set è composto da 3 elementi, non importa come questi sono permutati perché costituiranno comunque un unico set. 
Questo significa che l'espressione $S_{k,n}$ conta gli stessi set più volte, quindi per rispondere a questa domanda dovremo dividere l'espressione per qualcosa. Arriviamoci tramite un esempio:
Consideriamo il set delle lettere dell'alfabeto, le lettere sono $n = 26$ e ipotizziamo di voler contare:
1. il numero di permutazioni di $k = 3$ lettere.
2. il numero di set con $k = 3$ lettere.
La prima è facile perché basta applicare $S_{k,n} = \frac{n!}{(n-k)!}$.
Ora vediamo la seconda, facciamolo con un esempio:
Se ad esempio le lettere sono $\{ABC\}$ avrò le seguenti permutazioni:
$\langle ABC \rangle, \langle ACB \rangle, \langle BAC \rangle, \langle BCA \rangle, \langle CAB \rangle, \langle CBA \rangle$  
*Quante permutazioni sono relative allo stesso subset?* In questo caso 6 cioè 3!.
Generalizzando troviamo che **il numero di permutazioni che producono lo stesso subset è esattamente il numero di permutazioni di $k$ elementi** ovvero $k*(k-1)*...*2*1 = k!$.
Quindi la risposta alla seconda domanda è: $$\frac{S_{k,n}}{k!} = \frac{n!}{k!(n-k)!} = \binom{n}{k}$$
Chiamato **coefficiente binomiale**.

### Proprietà del coefficiente binomiale 

1. $\binom{n}{k} = 0$ se $k > n$ 
2. $\binom{n}{k} = \binom{n}{n-k}$ con $n\ge k$ perché se trovi il modo di estrarre $k$ elementi da un set di $n$ allora hai trovato il modo anche di estrarre i rimanenti $n-k$ elementi. Quindi i due numeri saranno uguali.
3. $\binom{n}{0} = \binom{n}{n} = 1$ deriva dalla definizione che 0! = 1.
4. $\binom{n}{k} = \binom{n-1}{k-1} + \binom{n-1}{k}$ con $n \ge k$ cioè posso calcolare il coefficiente binomiale come somma di binomi più piccoli. È comodo quando si utilizzano programmi ricorsivi. 

### Esercizio 5

Un gruppo di 5 maschi e di 10 femmine sono allineati in ordine casuale.
1. Qual è la probabilità che *la persona in quarta posizione sia un maschio*?
	Intuitivamente potremmo rispondere $P(E) = \frac{5}{15}$ perché il fatto che la posizione sia la quarta non influisce (se il testo avesse detto "dodicesima posizione" sarebbe cambiato qualcosa? No) ma proviamo a farlo in modo analitico. 
	Siamo in un modello ad ugual probabilità? Sì quindi come al solito calcoliamo le cardinalità di S e di E.
	$|S| = 15!$ perché il numero di persone è 15 e quindi dobbiamo considerare 15! possibili permutazioni di esse.
	La cardinalità di E la troviamo dividendo in due sotto-esperimenti:
	1. $C_1$ maschio in quarta posizione, quante possibilità ho? Considerando che i maschi sono 5 avrò $N_1$ = 5.
	2. $C_2$ tutte le altre possibili combinazioni ovvero $N_2 = 14!$ perché ho tolto quella in quarta posizione.
	Quindi $|E| = 5 * 14!$, infine posso calcolare: $P(E) = \frac{15!}{5*14!} = \frac{5}{15}$ come avevamo intuito.
2. Qual è la probabilità che *la persona in dodicesima posizione sia un maschio*? 
	Come visto prima, la posizione specifica non ha importanza quindi la risposta a questa domanda è la stessa della precedente.
3. Qual è la probabilità *che Adam* (un maschio) *sia in terza posizione*?
	Ora, riflettiamo. Cos'è cambiato rispetto a prima? Sicuramente la cardinalità di E diminuirà perché non mi va più bene un maschio a caso tra i 5 presenti ma voglio proprio Adam. Quindi: 
	1. $C_1$ Adam in terza posizione, quante possibilità ho? $N_1 = 1$.
	2. $C_2$ tutti gli altri ovvero $N_2 = 14!$
	Per cui $|E| = 1 * 14!$ e il risultato finale sarà $P(E) = \frac{14!}{15!} = \frac{1}{15}$ anche questo risultato è ragionevole. 

Quando si fanno gli esercizi ricordarsi sempre questo schema: 
![[Modellazione.svg|center|500]]

Spesso gli studenti si dimenticano di fare l'ultimo passaggio, l'interpretazione dei risultati ottenuti.

### Esercizio 6

Supponiamo di avere un insieme di $n$ persone e si vuole creare una fila di $k$ persone (con $k \le n$) prendendole a caso dal set di $n$ persone. Rispondere alle seguenti domande:
1. qual è la probabilità che Adam sia in prima posizione?
	La cardinalità di S sarà $|S| = S_{k,n}$ per quanto detto precedentemente.
	La cardinalità di E sarà $|E| = N_1 * N_2 = 1 * S_{k-1,n-1}$ sarebbe il problema di prima ma generalizzato con $n$.
	Da cui la soluzione è: $P(E) = \frac{|E|}{|S|} = 1*\frac{(n-1)!}{((n-1)-(k-1))!}*\frac{(n-k)!}{n!} = \frac{1}{n}$
	
	Notiamo che la soluzione non dipende da $k$ e il motivo è che si chiedeva specificatamente la probabilità che Adam si trovi in prima posizione che non dipende da quanto è lunga la fila. 
1. è la stessa probabilità che qualsiasi persona si trovi in una qualsiasi posizione? Sì.
2. qual è la probabilità che una persona (facciamo per esempio Adam) entri a far parte della coda?
	Ecco, qui la situazione cambia perché la lunghezza della coda è importante visto che più la coda è lunga e più ci saranno probabilità di farne parte. Poi notiamo anche il fatto che se $n = k$ allora la probabilità di farne parte sarà 1, mentre se $k = 0$ allora la probabilità di farne parte sarà nulla. Queste sono tutte riprove che possiamo fare quando avremo trovato la soluzione.
	
	Procediamo come al solito trovando le cardinalità:
	$|S| = S_{k,n}$ una permutazione di $k$ persone prese da un set di $n$.
	$|E| = k * S_{k-1,n-1}$ perché $k$ sono tutte le posizioni possibili in cui Adam può stare mentre $S_{k-1,n-1}$ sono tutte le permutazioni degli altri membri della coda una volta escluso Adam.
	
	Quindi $P(E) = \frac{|E|}{|S|} = \frac{k * S_{k-1,n-1}}{S_{k,n}} = ... = \frac{k}{n}$.
	Facciamo in fine le riprove: se $n = k$ allora $P(E) = 1$ mentre se $k = 0$ allora $P(E) = 0$, tutto torna. 

### Esercizio 7

Una squadra di basket è composta da 6 giocatori neri e da 6 bianchi. Durante una trasferta alloggiano in un hotel e vengono assegnati nelle stanze a coppie di 2 giocatori.
1. Qual è la probabilità che tutte le stanze abbiano giocatori dello stesso colore?
	In questo caso, l'outcome non è una sequenza ma un *set* perché l'ordine non cambia il risultato, ad esempio l'outcome composto da $\{3,12\}$ è uguale a $\{12,3\}$, quindi l'outcome avrà questa forma: $\{\{3,12\};\{1,9\};...;\{...\}\}$.
	Siamo in un modello UPM? Sì.
	Troviamo le cardinalità:
	- Per quanto riguarda S divido in sotto-esperimenti del tipo: numero di permutazioni per la prima stanza, numero di permutazioni per la seconda stanza e così via. Quindi potremmo scrivere una cosa del genere $|S| = \binom{12}{2}*\binom{10}{2}*...*\binom{2}{2}$. Questa scrittura è corretta? NO. Perché scrivendo così stiamo considerando anche l'ordine. Dividiamo quindi per la permutazione delle stanze per togliere tutti i duplicati. Il risultato corretto è dunque $|S| = \frac{\binom{12}{2}*\binom{10}{2}*...*\binom{2}{2}}{6!} = \frac{12!}{2^6*6!}$.
	- Ora vediamo la cardinalità di E:
		- $C_1$ tutti i possibili modi di porre 6 giocatori neri in 3 stanze. Quanti modi ci sono? $N_1 = \frac{\binom{6}{3}*\binom{3}{3}}{3!} = \frac{6!}{2^3*3!}$
		- $C_2$ tutti i possibili modi di porre 6 giocatori bianchi in 3 stanze. In questo caso, $N_2 = N_1$
	 Infine, $P(E) = \frac{|E|}{|S|} = \frac{5}{231}$.

# Probabilità Condizionata

In molti casi è utile trovare la probabilità di un evento E *sapendo che* un altro evento F si è verificato. In questo caso si parla di **probabilità condizionata** $P(E|F)$.

Ad esempio, lanciamo due dadi, qual è la probabilità che la somma di due facce sia maggiore o uguale a 10? 
In questo caso, $P(E) = \frac{|E|}{|S|} = \frac{|\{(5,5),(5,6),(6,5),(6,6),(6,4)(4,6)\}|}{|\{(x,y): 1 \le x \le 6, 1 \le y \le 6\}|} = \frac{1}{6}$.
Ora, supponiamo che il risultato del primo lancio sia 2, qual è la probabilità che la somma delle facce sia maggiore o uguale a 10? Zero. 
Oppure, supponiamo che il risultato del primo lancio sia 5, in questo caso la probabilità andrà ad aumentare, ma di quanto? 
Dovrò calcolare gli outcome presenti nell'intersezione tra l'evento 
E = { somma delle facce maggiore di 9 } e l'evento 
F = { Primo dato uguale a 5 } perché sicuramente possiamo escludere tutti gli outcome di $F^\complement$.
Posso quindi definire: $$P(E|F) = \frac{P(EF)}{P(F)}$$
Che, in questo caso, sarà uguale a: $P(E|F) = \frac{2/36}{1/6} = \frac{1}{3}$.
Notiamo che $P(E|F) > P(E)$ come avevamo intuito. 

### Esercizio 8

In un'Università abbiamo:
- 52% di studentesse
- 5% di studenti di Ingegneria Informatica
- 2% di studentesse che fanno Ingegneria Informatica
Trovare:
1. la probabilità che, preso a caso uno studente, considerando che fa Ingegneria Informatica, sia una studentessa.
	Definisco:
	- F = { studentesse dell'Università } -> P(F) = 0.52
	- C = { studenti di Ingegneria Informatica } -> P(F) = 0.05
	- FC = { studentesse dell'università che fanno Ingegneria Informatica } -> P(F) = 0.02
	Basta quindi applicare la formula, $P(F|C) = \frac{P(FC)}{P(C)} = \frac{2}{5}$.
2. la probabilità che, preso a caso uno studente, considerando che è una studentessa, faccia Ingegneria Informatica. 
	Anche qui basta applicare la formula $P(C|F) = \frac{P(CF)}{P(F)} = \frac{1}{26}$.

## Legge della probabilità totale

Un evento E può sempre essere scritto in termini di un altro evento F nel seguente modo: $$E = (E\cap F)\cup(E\cap F^\complement)$$
come si può vedere dalla seguente immagine: 

![[Bayes.png|center|400]]

I due eventi che compongono l'unione sono disgiunti quindi la probabilità di E può essere scritta come: $$P(E) = P(EF) + P(EF^\complement)$$Ora, sostituendo la formula della probabilità condizionata troviamo: $$P(E) = P(E|F)*P(F) + P(E|F^\complement)*P(F^\complement) =$$$$= P(E|F)*P(F) + P(E|F^\complement)*(1-P(F))$$
Questa espressione può essere generalizzata nel seguente modo:
> Dati $F_1,...,F_N$ insiemi tali che $\bigcup_{i=1}^{N}F_i = S$ e $F_i \cap F_j = \emptyset$ se $i \not= j$ allora possiamo calcolare la probabilità di $E$ come:
> $$P(E) = \sum_{i=1}^{N}P(EF_i) = \sum_{i=1}^{N}P(E|F_i)*P(F_i)$$
![[Bayes 2.png|center|400]]
### Esercizio 9

Ci sono due tipi di persone, coloro che sono inclini agli incidenti e quelli che non lo sono. Una compagnia di assicurazioni sa che:
- le persone inclini agli incidenti che ha una probabilità di avere un incidente del 40%
- le persone non inclini agli incidenti hanno una probabilità di avere un incidente è del 20%.
- il 30% dei guidatori sono inclini agli incidenti. 
Qual è la probabilità che un nuovo cliente della compagnia avrà un incidente?

Chiamiamo A = { avere un incidente } e B = { inclini agli incidenti }. Vogliamo trovare P(A). Conosciamo P(B), P(A|B) e anche P(A|$B^\complement$) per cui possiamo applicare la legge della probabilità totale: $P(A) = P(A|B)*P(B) + P(A|B^\complement) * P(B^\complement) = 0.26$.

## Teorema di Bayes

> Dati $F_1,...,F_N$ tali che $\bigcup_{i=1}^{N}F_i = S$ e $F_i \cap F_j = \emptyset$ se $i \not= j$. Ipotizziamo di conoscere *la probabilità a priori* $P(F_j)$. Se si verifica l'evento $E$ allora la probabilità di $F_j$ si modificherà nel seguente modo (*probabilità a posteriori*): $$P(F_j|E) = \frac{P(EF_j)}{P(E)} = \frac{P(E|F_j)*P(F_j)}{\sum_{i=1}^{N}P(E|F_i)*P(F_i)}$$

Questa formula descrive come *la fiducia* su un'ipotesi $F_j$ si modifica in base al fatto che un evento $E$ si è verificato, considerando che $E$ era influenzato da tali ipotesi. 

### Esercizio 10

Un laboratorio effettua dei test per una malattia rara. Se una persona ha la malattia allora il test lo rileva nel 99% dei casi, invece per l'1% il test da falsi positivi (dice che hai la malattia ma non ce l'hai).
Il 5% della popolazione ha quella malattia. 
Considerando che il test è risultato positivo, qual è la probabilità che si sia effettivamente malati? 
	Definisco:
	- P(D) = probabilità di essere malati = 0.005
	- P(P) = probabilità di risultare positivi al test
	- P(P|D) = probabilità di risultare positivi al test considerando che si ha la malattia = 0.99
	- P(P|$D^\complement$) = probabilità di risultare positivi al test considerando che non si ha la malattia = 0.01
	Una volta definiti i parametri posso calcolare:
	P(P) = P(P|D) P(D) + P(P|$D^\complement$) (1-P(D)) = 0.332
	Sembrerebbe un risultato sorprendente ma ha del tutto senso se poniamo attenzione al fatto che la probabilità di avere effettivamente la malattia è molto bassa, questo indipendentemente dal test.
	Il test è comunque un buono strumento di misurazione perché da un risultato di 0.332 che è molto maggiore di 0.005. 
	
## Eventi Indipendenti

> Due eventi $E$ ed $F$ sono indipendenti *se e solo se* $P(EF) = P(E) \cdot P(F)$

In questo caso, il verificarsi dell'evento F non ha alcuna influenza sull'evento E, quindi i due sono **indipendenti**. 
Scriviamo la definizione in termini di probabilità condizionata:
$$P(E|F)= \frac{P(EF)}{P(F)} = \frac{P(E)\cdot P(F)}{P(F)} = P(E)$$
Di seguito alcune proprietà:
- Se l'evento E è indipendente dall'evento F, allora E sarà indipendente anche da $F^\complement$.
- L'indipendenza è **simmetrica**: se E è indipendente da F allora anche F sarà indipendente da E.
- Se E è indipendente sia da F che da G *non* è necessariamente vero che sia indipendente anche da FG.
## Prove Ripetute

L'esperimento delle prove ripetute è un tipico caso in cui si assume che gli esperimenti siano indipendenti l'uno dall'altro. "Condizioni indipendenti" in questo caso significa che il risultato dell'esperimento j-esimo non influenza i risultati degli esperimenti precedenti o successivi.

### Esercizio 11

Una moneta viene lanciata 5 volte in condizioni indipendenti. Trovare la probabilità che:
1. I primi 3 lanci diano lo stesso risultato.
	Consideriamo ogni lancio come un sottoesperimento: 
	- lanciamo la prima volta, la probabilità di ottenere un outcome è p = 1.
	- lanciamo la seconda volta, la probabilità di ottenere un outcome uguale al primo lancio è pari a p = 1/2.
	- lanciamo la terza volta, la probabilità di ottenere un outcome uguale ai primi due è sempre p = 1/2.
	Questo perché i tre eventi sono indipendenti. La probabilità finale sarà quindi P = 1 * 1/2 * 1/2 = 1/4. 
2. O i primi 3 o gli ultimi 3 lanci diano lo stesso risultato (anche se si verificano entrambi gli eventi va bene).
	Con l'indipendenza devo calcolare la probabilità dell'evento $F\cup L$ dove F = {3 lanci uguali all'inizio} ed L = {3 lanci uguali alla fine}. 
	Quindi $FL$ = {5 risultati uguali} e posso trovare: 
	$P(F\cup L) = P(F) + P(L) - P(FL) = 1/4 + 1/4 - 1/16 = 7/16$ 
	Usando UPM avremmo ottenuto lo stesso risultato ma con un percorso più tortuoso. 
3. Ci siano almeno 2 teste nei primi 3 lanci e 2 croci negli ultimi 3. 
	In questo caso, è preferibile usare UPM perché possiamo scrivere tutti i possibili outcomes favorevoli che sono: HHxTT, HxHTT, xHHTT, HHTxT, HHTTx e, per ognuno di questi capiamo cosa può assumere x in modo da evitare i duplicati: 
	1.  x può assumere T
	2. x può assumere T
	3. x può assumere T o H
	4. x può assumere H
	5. x può assumere H
	Quindi avrò 6 outcomes favorevoli e 32 outcomes totali ($2^5$) da cui P = 6/32 = 3/16.

Notare che se la moneta fosse stata truccata il metodo UPM sarebbe stato inutilizzabile e avremmo dovuto per forza usare l'indipendenza.

### Esercizio 12

Il signor Rossi ha un mazzo di $n > 1$ chiavi.
## Sistemi Paralleli

> Un sistema è **parallelo** se è composto da $n$ sottosistemi e funziona se *almeno uno* di questi sottosistemi funziona. 

Per visualizzare il modello possiamo pensare alla corrente che scorre tra due terminali e i vari sottosistemi come degli switch. La corrente scorrerà solo se almeno uno degli switch è chiuso.

![Sistema Parallelo|center|400](https://cdnintech.com/media/chapter/41421/1512345123/media/image2.png)

Quanto un problema viene posto come *"almeno uno"* ci deve scattare un trigger che ci suggerisce che forse è meglio usare il **complemento**, infatti: 
P(corrente scorre) = 1 - P(corrente non scorre) = 1 - P(tutti gli switch aperti)
e la probabilità che tutti gli switch siano aperti si calcola abbastanza facilmente. 

### Esercizio 13

Consideriamo il seguente sistema parallelo: 

![[Sistemi paralleli 1.svg|center|700]]

Ogni switch $i$ ha una probabilità di essere chiuso pari a $p_i$. 
Trovare la probabilità che scorra corrente da A a B.

Definisco l'evento $A_i$ come l'evento "switch i-esimo chiuso" che avrà $P(A_i) = p_i$ e calcolo: $$P(scorre corrente) = 1 - P(tutti aperti) =$$
$$= 1 - P(A_1^\complement \cdot A_2^\complement ... A_n^\complement ) = $$
$$= 1 - \prod_{i = 1}^{n} P(A_i^\complement)$$
$$= 1 - \prod_{i = 1}^{n} (1-p_i)$$
### Esercizio 14

Consideriamo il seguente sistema parallelo: 

![[Sistemi paralleli 2.svg|center|500]]

Il nodo che ci da problemi è il 3 quindi semplifichiamo utilizzando la probabilità condizionata: 
- 3 aperto -> stesso problema di prima: [[#Sistemi Paralleli#Esercizio 13|esercizio 13]]
- 3 chiuso -> il circuito si semplifica con un cortocircuito su 3.
In fine: $$P(scorre corrente) = P(scorre|3 aperto)P(3aperto) + P(scorre|3chiuso)P(3chiuso) =$$
$$= (1-(1-p_1 p_4)(1- p_2 p_5))(1-p_3) + \{(1- (1-p_1)(1-p_2))(1-(1-p_4)(1-p_5))p_3\}$$
# Variabili Aleatorie 

> Dato un esperimento casuale il cui sample space è $S$, si dice che $X$ è una **variabile aleatoria** di $S$ se è una funzione reale tale che $X:S\rightarrow \mathbb{R}$

Notiamo che la funzione $X$ è deterministica, è il risultato dell'esperimento casuale ad essere aleatorio. 

![variabile aleatoria|center|500](https://miro.medium.com/v2/resize:fit:720/format:webp/1*AxzRBWY2L-WRUlJrtyil_A.png)

La probabilità associata alla variabile aleatoria si indica in questo modo: $P\{X = 1\} = P\{X = 0\} = 1/2$.

### Esercizio 15

Consideriamo l'esperimento lancio di due dadi, il sample space è $S = \{(d_1, d_2)|1 \le d_1 d_2 \le 6\}$ e definiamo le seguenti variabili aleatorie:
- $X$ somma dei valori usciti: $X:S \rightarrow \mathbb{R}, X((d_1,d_2)) = d_1 + d_2$.
- $Y$ valore massimo uscito: $Y:S \rightarrow \mathbb{R}, Y((d_1,d_2)) = max\{d_1, d_2\}$.

$X$ assumerà i seguenti valori: {2,3,...,11,12} mentre $Y$ i valori {1,2,...,6}.
Proviamo a calcolare alcune probabilità: 
- $P\{Y = 1\} = \frac{1}{36}$ perché l'unico outcome favorevole è (1,1)
- $P\{Y = 2\} = \frac{3}{36}$ perché gli outcome favorevoli sono (1,2),(2,1),(2,2)
- $P\{Y = 3\} = \frac{5}{36}$

## Comulative Distribution Function

Una variabile aleatoria $X$ (discreta o continua) è completamente caratterizzata dalla sua **Comulative Distribution Function (CDF)**, detta anche semplicemente *distribuzione*, definita come segue: $$F(\omega) = P\{X \le \omega\}$$
Notare che $\omega$ è scritta in minuscolo ad indicare il fatto che essa è un valore.
