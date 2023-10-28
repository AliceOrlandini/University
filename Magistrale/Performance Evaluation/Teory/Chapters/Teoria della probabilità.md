# Spazio campione ed Eventi

## Definizioni

Il concetto di **Probabilità** può essere definito in diversi modi, quello che useremo noi è quello di *frequenza relativa*:

> [!note] Probabilità di un evento
> Ripetiamo un esperimento *un numero elevato di volte* $N$ in *condizioni indipendenti* (ovvero che il risultato di un esperimento non influenza il risultato degli esperimenti successivi) allora, definito $k$ il numero di *realizzazioni* di un certo evento $E$, la probabilità di tale evento sarà: $$P(E) = \lim_{N \to \infty} \frac{k}{N}$$

Questa definizione di probabilità è puramente matematica poiché nella realtà è impossibile effettuare un numero infinito di esperimenti per cui la probabilità di un esperimento è generalmente determinato in base ad una *conoscenza a priori*. 
Ad esempio l’esperimento “lancio di una moneta” per motivi di simmetria darà una probabilità del 50% di ottenere “testa”.

> [!note] Esperimento Casuale
> Definiamo **esperimento casuale** un esperimento il cui risultato (*l’outcome*) non è predicibile a priori.

#Attenzione L’outcome di un esperimento è definito solo nella mente dell’osservatore.
Infatti, quando ci troviamo davanti ad un esperimento, la prima cosa da fare è definire lo *spazio dei risultati*. Questo aspetto è importante perché dato un esperimento possiamo avere più spazi a seconda di ciò che vogliamo analizzare in quel momento.

> [!note] Spazio Campione
> Definiamo **sample space** o spazio campione $S$ il set di *tutti i possibili outcome* di un esperimento casuale.

Ad esempio nel caso dell’esperimento “lancio di un dado” avrò $S = \{1,2,3,4,5,6\}$ visto che il dato ha 6 facce che numeriamo da 1 a 6. 

> [!note] Evento
> Definiamo **evento** $E$ un *sottoinsieme* (subset) dello spazio campione (sample space) $S$.

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
# Assiomi della probabilità

> [!note] Probabilità
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

### Esercizio 1

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
1. Ha una **cardinalità finita** $N = |S|$.
2. Ha **outcomes ugualmente equiprobabili** (*equally likely outcoes*).

Ad esempio quando si lancia una moneta la cardinalità di $S$ è 2 (testa o croce) e i risultati sono equiprobabili (50% testa e 50% croce) ovvero $p = \frac{1}{N}$.

Per essere più precisi dovremmo dire che $N*p = 1$ considerando che tutti gli eventi sono mutuamente esclusivi e l’unione di tutti gli eventi è il sample space  che ha probabilità pari a 1.

Se siamo in un modello *a probabilità uniforme* (vedremo più avanti esempi di modelli) allora si parla di sample space con *equally likely outcomes* e la probabilità di un evento $E$ sarà: 
> [!danger] Probabilità di un evento
> $$P(E) = \frac{|E|}{N}$$ con $N = |S|$.

Quindi per definire la probabilità di un evento ci basta *contare* il numero di outcomes inclusi in quell’evento e dividerli per la cardinalità del sample space.

## Basic Principle of Counting

Visto che, come visto al punto precedente, per trovare la probabilità di un evento $E$ quando il sample space ha cardinalità finita e outcomes ugualmente equiprobabili dobbiamo contare il numero di outcome e dividere per la cardinalità del sample space, allora ci conviene introdurre un *principio* che ci permette di contare più facilmente gli outcome di un evento.

> [!note] Principio base del conteggio
Dato un *esperimento* $C$ composto da due *sotto-esperimenti* $C_1$ e $C_2$, aventi rispettivamente $N_1$ e $N_2$ possibili outcome, allora il numero di possibili outcome dell’esperimento $C$ è uguale a $N_1*N_2$.
Questa definizione può essere generalizzata da 2 a $k$ esperimenti: $$C = \prod_{i = 1}^{k}N_i$$
### Esercizio 2

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

### Esercizio 3

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

> [!danger] Numero di permutazioni di $k$ elementi in un set di $n$ elementi
> $$S_{k,n} = n * (n-1)* ... * (n - (k-1)) = \frac{n!}{(n-k)!}$$permutazioni di $k$ elementi dato un set di $n$ elementi.

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
Quindi la risposta alla seconda domanda è: 

> [!danger] Coefficiente Binomiale
$$\frac{S_{k,n}}{k!} = \frac{n!}{k!(n-k)!} = \binom{n}{k}$$
### Proprietà del coefficiente binomiale 

1. $\binom{n}{k} = 0$ se $k > n$.
2. $\binom{n}{k} = \binom{n}{n-k}$ con $n\ge k$ perché se trovi il modo di estrarre $k$ elementi da un set di $n$ allora hai trovato il modo anche di estrarre i rimanenti $n-k$ elementi. Quindi i due numeri saranno uguali.
3. $\binom{n}{0} = \binom{n}{n} = 1$ deriva dalla definizione che 0! = 1.
4. $\binom{n}{k} = \binom{n-1}{k-1} + \binom{n-1}{k}$ con $n \ge k$ cioè posso calcolare il coefficiente binomiale come somma di binomi più piccoli. È comodo quando si utilizzano programmi ricorsivi. 

### Esercizio 4

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

### Esercizio 5

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

### Esercizio 6

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
Posso quindi definire: 

> [!danger] Probabilità Condizionata
> $$P(E|F) = \frac{P(EF)}{P(F)}$$

Che, in questo caso, sarà uguale a: $P(E|F) = \frac{2/36}{1/6} = \frac{1}{3}$.
Notiamo inoltre che $P(E|F) > P(E)$ come avevamo intuito. 

### Esercizio 7

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

> [!note] Probabilità di un evento data la probabilità condizionata
> Dati $F_1,...,F_N$ insiemi tali che $\bigcup_{i=1}^{N}F_i = S$ e $F_i \cap F_j = \emptyset$ se $i \not= j$ allora possiamo calcolare la probabilità di $E$ come:
> $$P(E) = \sum_{i=1}^{N}P(EF_i) = \sum_{i=1}^{N}P(E|F_i)*P(F_i)$$
![[Bayes 2.png|center|400]]
### Esercizio 8

Ci sono due tipi di persone, coloro che sono inclini agli incidenti e quelli che non lo sono. Una compagnia di assicurazioni sa che:
- le persone inclini agli incidenti che ha una probabilità di avere un incidente del 40%
- le persone non inclini agli incidenti hanno una probabilità di avere un incidente è del 20%.
- il 30% dei guidatori sono inclini agli incidenti. 
Qual è la probabilità che un nuovo cliente della compagnia avrà un incidente?

Chiamiamo A = { avere un incidente } e B = { inclini agli incidenti }. Vogliamo trovare P(A). Conosciamo P(B), P(A|B) e anche P(A|$B^\complement$) per cui possiamo applicare la legge della probabilità totale: $P(A) = P(A|B)*P(B) + P(A|B^\complement) * P(B^\complement) = 0.26$.

## Teorema di Bayes

> [!note] Teorema di Bayes
> Dati $F_1,...,F_N$ tali che $\bigcup_{i=1}^{N}F_i = S$ e $F_i \cap F_j = \emptyset$ se $i \not= j$. Ipotizziamo di conoscere *la probabilità a priori* $P(F_j)$. Se si verifica l'evento $E$ allora la probabilità di $F_j$ si modificherà nel seguente modo (*probabilità a posteriori*): $$P(F_j|E) = \frac{P(EF_j)}{P(E)} = \frac{P(E|F_j)*P(F_j)}{\sum_{i=1}^{N}P(E|F_i)*P(F_i)}$$

Questa formula descrive come *la fiducia* su un'ipotesi $F_j$ si modifica in base al fatto che un evento $E$ si è verificato, considerando che $E$ era influenzato da tali ipotesi. 

### Esercizio 9

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

> [!note] Eventi Indipendenti
> Due eventi $E$ ed $F$ sono **indipendenti** *se e solo se* $P(EF) = P(E) \cdot P(F)$

In questo caso, il verificarsi dell'evento F non ha alcuna influenza sull'evento E, quindi i due sono **indipendenti**. 
Scriviamo la definizione in termini di probabilità condizionata:
$$P(E|F)= \frac{P(EF)}{P(F)} = \frac{P(E)\cdot P(F)}{P(F)} = P(E)$$
Di seguito alcune proprietà:
- Se l'evento E è indipendente dall'evento F, allora E sarà indipendente anche da $F^\complement$.
- L'indipendenza è **simmetrica**: se E è indipendente da F allora anche F sarà indipendente da E.
- Se E è indipendente sia da F che da G *non* è necessariamente vero che sia indipendente anche da FG.
## Prove Ripetute

L'esperimento delle **prove ripetute** è un tipico caso in cui si assume che gli esperimenti siano indipendenti l'uno dall'altro. "Condizioni indipendenti" in questo caso significa che il risultato dell'esperimento j-esimo non influenza i risultati degli esperimenti precedenti o successivi.

### Esercizio 10

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

### Esercizio 11

Il signor Rossi ha un mazzo di $n > 1$ chiavi. Da finire
## Sistemi Paralleli

> [!note] Sistema Parallelo
> Un sistema è **parallelo** se è composto da $n$ sottosistemi e funziona se *almeno uno* di questi sottosistemi funziona. 

Per visualizzare il modello possiamo pensare alla corrente che scorre tra due terminali e i vari sottosistemi come degli switch. La corrente scorrerà solo se almeno uno degli switch è chiuso.

![Sistema Parallelo|center|400](https://cdnintech.com/media/chapter/41421/1512345123/media/image2.png)

Quanto un problema viene posto come *"almeno uno"* ci deve scattare un trigger che ci suggerisce che forse è meglio usare il **complemento**, infatti: 
P(corrente scorre) = 1 - P(corrente non scorre) = 1 - P(tutti gli switch aperti)
e la probabilità che tutti gli switch siano aperti si calcola abbastanza facilmente. 

### Esercizio 12

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

> [!note] Variabile Aleatoria
> Dato un esperimento casuale il cui sample space è $S$, si dice che $X$ è una **variabile aleatoria** di $S$ se è una funzione reale tale che $X:S\rightarrow \mathbb{R}$

Notiamo che la funzione $X$ è deterministica, è il risultato dell'esperimento casuale ad essere aleatorio. 

![variabile aleatoria|center|500](https://miro.medium.com/v2/resize:fit:720/format:webp/1*AxzRBWY2L-WRUlJrtyil_A.png)

La probabilità associata alla variabile aleatoria si indica in questo modo: $P\{X = 1\} = P\{X = 0\} = 1/2$.

### Esercizio 14

Consideriamo l'esperimento lancio di due dadi, il sample space è $S = \{(d_1, d_2)|1 \le d_1 d_2 \le 6\}$ e definiamo le seguenti variabili aleatorie:
- $X$ somma dei valori usciti: $X:S \rightarrow \mathbb{R}, X((d_1,d_2)) = d_1 + d_2$.
- $Y$ valore massimo uscito: $Y:S \rightarrow \mathbb{R}, Y((d_1,d_2)) = max\{d_1, d_2\}$.

$X$ assumerà i seguenti valori: {2,3,...,11,12} mentre $Y$ i valori {1,2,...,6}.
Proviamo a calcolare alcune probabilità: 
- $P\{Y = 1\} = \frac{1}{36}$ perché l'unico outcome favorevole è (1,1)
- $P\{Y = 2\} = \frac{3}{36}$ perché gli outcome favorevoli sono (1,2),(2,1),(2,2)
- $P\{Y = 3\} = \frac{5}{36}$

## Comulative Distribution Function

Una variabile aleatoria $X$ (discreta o continua) è completamente caratterizzata dalla sua **Comulative Distribution Function (CDF)**, detta anche semplicemente *distribuzione*, definita come segue: 

> [!note] CDF
> La **Comulative Distribution Function** è un funzione tale che, data la variabile aleatoria $X$ e il numero $\omega$, vale $$F(\omega) = P\{X \le \omega\}$$

Notare che $\omega$ è scritta in minuscolo ad indicare il fatto che essa è un valore.
La CDF essendo una probabilità assumerà valori compresi tra 0 e 1, inoltre è *monotona non decrescente* (può solo salire o rimanere costante).
Inoltre, $\lim_{\omega \rightarrow -\infty} F(\omega) = 0$ e $\lim_{\omega \rightarrow +\infty} F(\omega) = 1$ che vale per qualsiasi variabile aleatoria. 
### Esercizio 15

Considerare una variabile aleatoria $X$ con la seguente CDF: 
$F(\omega) = 1- e^{-w^2}$ per $\omega > 0$ e pari a zero per $\omega$ negativi. 

![[CDF.png|center|600]]

Calcolare P{X > 1}:
per risolvere questo esercizio bisogna prima di tutto disegnare la funzione e assicurarsi che rispetti le proprietà di una CDF.
Poi, notiamo che P{ X > 1 } = 1 - P{ X $\le$ 1 } = 1 - F(1) = 1 - (1 - $e^{-1}$) = 1/e.

Calcolare P{ 1 < X $\le$ 2 }:
si nota che: P{ X $\le$ 2 } = P{X $\le$ 1 } + P{1 < X $\le$ 2 } 
da cui si ricava che: P{1 < X $\le$ 2 } = F(2) - F(1) = 1/e - 1/$e^4$
## Probability Mass Function

> [!note] PMF
> Per variabili aleatorie *discrete*, si può definire la **Probability Mass Function** (PMF) nel seguente modo: $p(a) = P\{X = a\}$.

È sempre valida la *condizione di normalizzazione* che sostiene che $\sum_{-\infty}^{+\infty} p(a) = 1$

Dalla PMF si può passare alla CDF nel seguente modo: $$\sum_{x\le a} p(x)$$
Ma si può fare anche il contrario, dalla CDF si ricava la PMF nel seguente modo: $$p(a) = F(a)-F(a^-)$$
## Probability Density Function

Per variabili aleatorie continua non ha alcun senso definire la probability mass function perché è impossibile che una variabile di questo tipo assuma *esattamente* un valore (con infinita precisione) in uno spazio continuo.

> [!note] PDF
> Per variabili aleatorie *continue*, si può definire la **Probability Density Function** (PDF) $f(x)$ che è una funzione *non negativa* con la seguente proprietà: $f(x)$ è una PDF se, dato un set $B$ di numeri reali, allora $$P\{X \in B\} = \int_{B}f(x) dx$$

Anche per questa funzione è valida la condizione di *normalizzazione*: $$P\{X \in (-\infty, +\infty)\} = \int_{-\infty}^{+\infty} f(x) dx = 1$$
Se $B$ è un intervallo $[a,b]$ allora si ottiene: $$P\{a \le X \le b\} = \int_{a}^{b} f(x) dx = F(b) - F(a)$$
Questa funzione dal punto di vista fisico misura la probabilità che la variabile aleatoria $X$ assuma un valore *intorno* ad $[a,b]$. Ma non possiamo sapere la probabilità che $X$ sia esattamente $a$ o esattamente $b$ perché ci troviamo nel dominio continuo. Facciamo una riprova: $$P\{X = a\} = P\{a \le X \le a\} = \int_{a}^{a} f(x) dx = F(a) - F(a) = 0$$
Come abbiamo visto, integrando la PDF si ottiene la CDF nel seguente modo: $$F(a) = P\{X \le a\} = P\{-\infty \le X \le a\}= \int_{-\infty}^{a}f(x) dx$$
La proprietà impostante è che si può ottenere anche PDF a partire dalla CDF semplicemente facendo un'operazione di derivazione: $$f(a)= \frac{\partial}{\partial a} F(a)$$
Questa operazione presuppone che la CDF sia differenziabile, per i nostri scopi lo sarà sempre. 
## Jointly distributed random variables

Spesso siamo interessati all'interazione tra due o più variabili aleatorie. Per esempio se consideriamo l'esperimento "lancio di una freccia su un bersaglio" e applichiamo un piano cartesiano in corrispondenza del centro del bersaglio, non è sufficiente conoscere la CDF di $X$ o la CDF di $Y$ ma dovremo studiarle congiuntamente l'una all'altra. 

> [!note] JCDF
> Date due variabili aleatorie (*discrete* o *continue*), la loro **Joint Comulative Distribution Function** (JCDF) è definita come: $$$F(x,y) = P\{X \le z, Y \le y\}$$

Dove la virgola sta per *and* logico, infatti si sta considerando l'intersezione degli eventi $X \le x$ e $Y \le y$.

A partire dalla JCDF si possono calcolare le rispettive CDFs, basta osservare che: $$F_{X}(x)= P\{X \le x\} = P\{X \le x, Y \le +\infty\} = F(x,+\infty)$$
È importante ricordare che non vale il contrario, infatti, in generale, non si possono ottenere informazioni su una JCDF a partire dalle singole distribuzioni. 

> [!note] JPMF
> Date due variabili aleatorie *discrete*, $X$ e $Y$ si può definire la **Joint Probability Mass Function** (JPMF) come $$p(x,y) = P\{X = x, Y = y\}$$

Anche in questo caso, possiamo ottenere le PMFs a partire dalla JPMF: $$p(x) = P\{X = x\} = P\{\bigcup_{i} (X = x, Y = y_{i})\}$$
Questi eventi sono *mutuamente disgiunti* quindi dall'unione posso passare alla somma di probabilità:
$$= \sum_{i} P(X = x, Y = y_{i)}= \sum\limits_{i} p(x, y_i)$$
Dalla JCDF si può ottenere la JPMF nel seguente modo: $$F(x,y) = P\{X \le x, Y \le y\} = \sum\limits_{x_{i}\le x} \sum\limits_{y_{j} \le y} p(x_{i}, y_{j})$$
Le probabilità $P\{X = x\}$ e $P\{Y = y\}$ vengono anche chiamate *probabilità marginali*.

> [!note] JPDF
> Date due variabili aleatorie *continue*, per ogni set $C \subseteq \mathbb{R}^{2}$ di numeri reali $(x,y)$ si può definire la **Joint Probability Density Function** (JPDF) nel seguente modo: $$P\{(X,Y) \in C\} = \int_{(x,y) \in C} \int f(x,y) dx dy$$

Inoltre, quando $C$ può essere separato in due set di numeri reali $C = \{(x,y)|x \in A, y \in B\}$ allora possiamo riscrivere il precedente integrale nel seguente modo: $$P\{(X,Y) \in C\} = P\{X \in A, Y \in B\} = \int_{A} \int_{B} f(x,y) dy dx$$
Non spaventarsi per la presenza degli integrali, la maggior parte dei conti che bisognerà fare saranno con esponenziali o equazioni semplici, inoltre ricordarsi che gli integrali possono essere risolti in qualsiasi ordine quindi è fondamentale scegliere quello che semplifica di più i calcoli. 

Dalla definizione di JCDF, possiamo ottenere: $$F(a,b) = P\{X \le a, Y \le b\} = $$
$$P\{X \in (-\infty, a], Y \in (-\infty, b]\} = $$
$$= \int_{-\infty}^{a} \int_{-\infty}^{b} f(x,y) dy dx$$
abbiamo quindi dimostrato che si può ottenere la JCDF a partire dalla JPDF tramite un'operazione di integrazione.
Possiamo ottenere la relazione inversa derivando: $$f(a,b) = \frac{\partial ^{2}}{\partial a \partial b} F(a,b)$$
la JCDF per noi sarà sempre differenziabile. 

Se esiste la JPDF possiamo anche ottenere le singole PDFs di $X$ e di $Y$ nel seguente modo: $$P\{X \in A\} = P\{X \in A, Y \in (-\infty, + \infty]\} = $$
$$= \int_{A} \int_{-\infty}^{+\infty} f(x,y) dy dx$$
Sapendo che $P\{X \in A\} = \int_{A} f_{X}(x) dx$ allora: $$f_{X}(x) = \int_{-\infty}^{+\infty} f(x,y) dy$$
### TODO schema di come passare da una all'altra

Tutte le definizioni precedenti sono state introdotte per 2 variabili aleatorie, possono essere ovviamente estese ad $n$ variabili aleatorie $X_{1},X_{2},...,X_{n}$ definendo ad esempio $$F(x_{1},x_{2},...,x_{n}) = P\{X_{1} \le x_{1},...,X_{n} \le x_{n}\}$$ da cui si ottiene la CDF della singola variabile $X_{i}$ come $$F_{X_{i}}(x_{i}) = F(+\infty, ..., +\infty, x_{i}, +\infty, +\infty)$$
## Variabili aleatorie indipendenti

> [!note] Variabili Aleatorie Indipendenti
> Due variabili aleatorie $X$ e$Y$ sono **indipendenti** *se e solo se* $$F(x,y) = F_{X}(x)\cdot F_{Y}(y)$$

Che significa che la loro JCDF è ottenuta facendo il prodotto delle singole CDF. È come dire che $\{X \le x\}$ e $\{Y \le y\}$ sono eventi indipendenti per qualsiasi valore di x e y. Infatti: 
$$
\begin{align*}
F(x,y) &= P\{X \le x, Y \le y\} =\\
&= P\{X \le x | Y \le y\}\cdot P\{Y \le y\} =\\
&= P\{X \le x\}\cdot P\{Y \le y\} =\\
&= F_{X}(x)\cdot F_{Y}(y)
\end{align*}
$$
Se due variabili aleatorie sono indipendenti allora segue che:
- se sono **discrete** $p(x,y) = p_{X}(x)\cdot p_{Y}(y)$
- se sono **continue** $f(x,y) = f_{X}(x)\cdot f_{Y}(y)$
E si può ovviamente estendere anche ad $n$ variabili aleatorie. 
(Praticamente quando c'è l'indipendenza possiamo fare moltiplicazioni)

### Esercizio

## Valor Medio

> [!note] Valor Medio
> Il **valor medio** (o valore atteso) di una variabile aleatoria $X$ è denominato come $E[X]$ e si calcola nel seguente modo:
> - variabili aleatorie *discrete*: $E[X] = \sum\limits_{i} x_{i}p(x_{i})$
> - variabili aleatorie *continue*: $E[X] = \int_{-\infty}^{+\infty} xf(x) dx$

Nel caso discreto il valor medio è praticamente la somma pesata di ogni valore assunto dalla variabile moltiplicato per la probabilità di quel valore. 
Fare attenzione al fatto che il valor medio nel caso discreto potrebbe non essere un valore ammissibile. 

Consideriamo una proprietà importante:
data una variabile aleatoria discreta $X$, proviamo ad assumere che $Y$ sia in funzione di $X$ in questo modo: $Y = g(X)$ con $g()$ *iniettiva* (questa assunzione poi la toglieremo). 
In questo caso, $p_{Y}(y_{i}) = p_{X}(x_{j})$ per qualisasi valore $x_{j},y_{i}$ tali che $y_{i}= g(x_{j})$. Calcoliamo quindi il valor medio: $$E[Y] = \sum\limits_{i}y_{i}p_{Y}(y_{i}) = \sum\limits_{i}g(x_{j})p_{X}(x_{j}) = E[g(X)]$$
Se rimuoviamo l'ipotesi che $g()$ sia iniettiva allora otteniamo che la probabilità che una variabile aleatoria $Y$ assuma il valore $y_{i}$ è *la somma della probabilità di tutti i valori $x$ che tramite la funzione $g()$ vengono mappati in $y_{i}$* ovvero $$p_{Y}(y_{i}) = \sum\limits_{j\cdot g(x_{j}) = y_{i}}p_{X}(x_{j})$$
Quindi posso calcolare il valor medio usando la stessa formula di prima:
$$E[Y] = \sum\limits_{i}y_{i}p_{Y}(y_{i}) = \sum\limits_{i}y_{i}\left[\sum\limits_{j\cdot g(x_{j})=y_{i}}p_{X}(x_{j})\right]= \sum\limits_{j}g(x_{j})p_{X}(x_{j}) = E[g(X)]$$
La stessa cosa vale per variabili aleatorie *continue*: $$E[g(x)] = \int_{-\infty}^{+\infty} g(x)f(x) dx$$
Vediamo ora il **valor medio della somma di variabili aleatorie**:
$$
\begin{equation}
E[g(X,Y)]= \begin{cases}\sum\limits\sum\limits g(x,y)\cdot p(x,y) & \text{VA discrete} \\
\int\int g(x,y)\cdot f(x,y)dxdy & \text{VA continue}\end{cases}
\end{equation}
$$
Nel caso in cui $g(X,Y) = X + Y$ allora si può dimostrare che $E[X+Y] = E[X] + E[Y]$ e notare che non abbiamo supposto l'indipendenza!
## Indicator Variable

> [!note] Indicator Variable
> La variabile aleatoria discreta chiamata indicator variable relativa ad un evento $A$ è definita come segue: 
>$$
>\begin{equation}
>I_{A}= \begin{cases}1 & \text{se A si verifica} \\
0 & \text{se A non si verifica}\end{cases}
>\end{equation}
$$

Si deduce quindi che $p(1) = P(A)$ e $p(0) = 1-P(A)$ di conseguenza il suo valor medio sarà $E[I_{A}] = 1\cdot p(1) + 0 \cdot p(0) = p(1) = p(A)$
Questa variabile aleatoria è molto importante perchè trasforma un evento in qualcosa di binario.

## Varianza

Come abbiamo visto il valor medio ci permette di riassumere il contenuto informativo di una determinata distribuzione. Il valor medio però è spesso insufficiente. 
Siamo infatti interessati a determinare **quanto è dispersa** una certa variabile aleatoria intorno al suo valor medio. Questo valore è chiamato Varianza: 
> [!note] Varianza
> La misura di quanto una variabile aleatoria $X$ è *dispersa* intorno al suo valor medio si chiama **varianza** ed è definita nel seguente modo: $$Var(X) = E[(X-\mu )^{2}]$$

Molto spesso la varianza si calcola nel seguente modo: 
$$
\begin{align*}
Var(X) &= E[(X-\mu)^{2}] = \\
&= E[X^{2}-2X\mu + \mu ^{2}] =\\
&= E[X^{2}] - 2\mu E[X] + \mu^{2}\\
&= E[X^{2}] - 2\mu^{2}+ \mu^{2}\\
&= E[X^{2}] - \mu^{2}\\
&= E[X^{2}] - E[X]^{2}\\
&= \sigma^{2}
\end{align*}
$$
Cioè il valor medio della variabile al quadrato meno il quadrato del valor medio.

> [!note] Deviazione Standard
> Definiamo la **deviazione standard** come: $$StDev(X) = \sigma = \sqrt{Var(X)}$$

Per il valor medio avevamo visto che $E[aX + b] = aE[X] + b$ essendo il valor medio un'operazione lineare. La varianza invece è un'operazione quadratica quindi non possiamo usare la stessa proprietà, vediamo come cambia: 
$$
\begin{align*}
Var(aX+b) &= E[(aX+b -E[aX+b])^{2}] =\\
&= E[(aX +\not{b} -aE[X] -\not{b})^{2}] = \\
&= E[a^{2}\cdot (X-\mu)^{2}]= \\
&= a^{2}Var(X)
\end{align*}
$$
Da notare che:
1. **l'offset non ha effetto**: questo ha senso perché anche se si sposta la distribuzione sull'asse x il livello di dispersione rimane lo stesso, al contrario del valor medio che aumenta/diminuisce a seconda dell'offset.
2. **la scala ha effetto**: ha senso perché se una variabile aleatoria viene scalata di un certo fattore sto *compressando* se $a<1$ oppure *diffondendo* se $a > 1$.

Vediamo allora cosa accade per la somma di due variabili aleatorie in generale partendo dalla definizione:
$$
\begin{align*}
Var(X+Y) &= E[(X+Y - (\mu_{X}+\mu_{Y})^{2})] = \\
&= E[(X - \mu_{X}+ Y - \mu_{Y})^{2}] =\\
&= E[(X-\mu_{X})^{2}+ (Y-\mu_{Y})^{2} + 2(X-\mu_{X})(Y-\mu_{Y})] =\\
&= E[(X-\mu_{X})^{2}] + E[(Y-\mu_{Y})^{2}] +2E[(X-\mu_{X})(Y-\mu_{Y})] =\\
&= Var(X) + Var(Y) + 2E[(X-\mu_{X})(Y-\mu_{Y})]
\end{align*}
$$
che non è esattamente la somma delle due ma c'è un ulteriore termine chiamato **covarianza**.
> [!note] Covarianza
> La **covarianza** è definita come: $$Cov(X,Y) = E[(X-\mu_{X})(Y-\mu_{Y})]$$

Notiamo che questo valore può essere sia positivo che negativo, sviluppando la definizione troviamo:
$$
\begin{align*}
Cov(X,Y) &= E[(X-\mu_{X})(Y-\mu_{Y})] =\\
&= E[X\cdot Y + \mu_{X} \cdot \mu_{Y}-Y\cdot \mu_{X}-X\cdot \mu_{Y}] = \\
&= E[X\cdot Y] + E[\mu_{X}\cdot \mu_{Y}] - E[Y\cdot \mu_{X}]-E[X \cdot \mu_{Y}] =\\
&= E[X\cdot Y] + \mu_{X}\cdot \mu_{Y} - \mu_{X}\cdot \mu_{Y} -\mu_{X}\cdot \mu_{Y} =\\
&= E[X\cdot Y]- \mu_{X}\cdot \mu_{Y}
\end{align*}
$$
cioè il valor medio del prodotto delle due variabili aleatorie meno il prodotto dei valor medi delle due. 
Osserviamo che:
1. La covarianza è **commutativa**: $Cov(X,Y) = Cov(Y,X)$ perché il prodotto è commutativo.
2. $Cov(X,X) = Var(X)$ per dimostrarlo basta sostituire.
3. Se $X$ e $Y$ sono variabili aleatorie **indipendenti** allora $E[X\cdot Y] = \mu_{X}\cdot \mu_{Y}$ quindi $Cov(X,Y) = 0$.
Da quest'ultima proprietà si deduce che se $X$ e $Y$ sono variabili aleatorie **indipendenti** allora **la varianza è la somma delle varianze** $Var(X,Y) = Var(X)+Var(Y)$.
Notare che non vale il contrario, se la covarianza è nulla non è detto che le variabili siano indipendenti. 
Ovviamente tutto ciò vale per $N$ variabili aleatorie: $$
\begin{align*}
Var\left(\sum\limits_{i}X_{i}\right) &= \sum\limits_{i}Var(X_{i}) + \sum\limits_{i}\sum\limits_{j\not=i} Cov(X_{i},X_{j}) =\\
&= \sum\limits_{i}\sum\limits_{j}Cov(X_{i},X_{j}) 
\end{align*}$$
## Covarianza e Correlazione

La covarianza di due variabili aleatorie può essere positiva, negativa o nulla e ciò ha un significato fisico. Definiamo due eventi $A$ e $B$ e le relative indicator variables $I_{A}$ e $I_{B}$.
La variabile $I_{A}\cdot I_{B}$ è pari ad 1 solo se l'evento $A\cap B = AB$ si verifica. Calcoliamo $Cov(I_{A},I_{B})$ applicando la definizione:
$$
\begin{align*}
Cov(I_{A},I_{B}) &= E[I_{A}\cdot I_{B}] - E[I_{A}]\cdot E[I_{B}] =\\
&= P(AB) - P(A)\cdot P(B)
\end{align*}
$$
quindi:
- se $Cov(I_{A},I_{B}) > 0$ allora $P(AB) > P(A)\cdot P(B)$ e quindi $P(A|B) = \frac{P(AB)}{P(B)} > P(A)$
- se $Cov(I_{A},I_{B}) < 0$ allora $P(AB) < P(A)\cdot P(B)$ e quindi $P(A|B) = \frac{P(AB)}{P(B)} < P(A)$
Lo stesso vale anche per $P(B|A)$.
Questo significa che:
- se la covarianza è positiva allora l'evento $A$ è più probabile che si verifichi se $B$ si verifica.
- se la covarianza è negativa allora l'evento $A$ è meno probabile che si verifichi se $B$ si verifica.
- se i due eventi sono indipendenti allora la covarianza è nulla e quindi non si influenzano l'un l'altro. 
Una misura *normalizzata* di questo effetto si chiama **correlazione**:
> [!note] Correlazione
> La correlazione tra due variabili aleatorie $X$ e $Y$ è: $$Corr(X,Y) = \frac{Cov(X,Y)}{\sqrt{Var(X)\cdot Var(Y)}}$$

che è definita solo tra -1 e +1, per questo si dice che è normalizzata. 
# Variabili aleatorie Speciali

Alcune variabili aleatorie possono essere dei buoni modelli per alcuni aspetti della realtà. Vediamo le principali.
## Distribuzioni Discrete

### Distribuzione di Bernoulli

Una variabile aleatoria discrata di **Bernoulli** vale 1 con una probabilità $p$ e vale 0 con una probabilità $1-p$. È praticamente una indicator variable di un evento con probabilità $p$.
Questa tipologia di distribuzione viene usata *quando si ha una certa probabilità di successo e una di insuccesso* (un risultato binario).
I suoi parametri sono:
- **Valor Medio**: $E[X] = p$
- **Varianza**: $Var(X) = p\cdot (1-p)$
Il valore di massima incertezza si raggiunge se $p = 1/2$.
### Distribuzione Binomiale

Questa distribuzione si può ricavare a partire dalla distribuzione di Bernoulli e rappresenta *il numero di successi in $n$ tentativi ripetuti in condizioni indipendenti* dove ogni tentativo ha una probabilità di successo pari a $p$.
Questa distribuzione si chiama **Binomiale** è caratterizzata da due parametri: $n$ e $p$ e si indica con $X \thicksim Bi(n,p)$.
Calcoliamo la sua probability mass function PMF:
$$p(i) = P\{X=i\} = \binom{n}{i}p^{i}(1-p)^{n-i}$$
il prodotto di probabilità si può fare perché i tentativi sono indipendenti.
Controlliamo che sia verificata la proprietà di normalizzazione: 
$$\sum\limits_{i=0}^{n}p(i) = \sum\limits_{i=0}^{n}\binom{n}{i}p^{i}(1-p)^{n-i} = [p+(1-p)]^{n} = 1$$
La PMF assume valori positivi tra 0 e $n$, è simmetrica solo se $p = 0.5$ e il suo valore massimo è *intorno* a $n\cdot p$ (intorno perché è discreta).
I suoi  parametri sono:
- **Valor Medio**: $E[X] = E\left[\sum\limits_{i=1}^{n} E[X_{i}]\right]= n\cdot p$
- **Varianza**: visto che i tentativi sono indipendenti posso sommare la varianze delle singole variabili aleatorie di Bernoulli: $Var(X) = Var(\sum\limits_{i=1}^{n}X_{i})=\sum\limits_{i=1}^{n} Var(X_{i}) = n\cdot [p \cdot (1-p)]$

Infine, date $X_{1} \thicksim Bi(n_{1},p)$ e $X_{2} \thicksim Bi(n_{2},p)$ due variabili aleatorie **indipendenti** allora $X_{1}+X_{2} \thicksim Bi(n_{1} + n_{2},p)$.
### Distribuzione di Poisson

Una variabile aleatoria discreta è di **Poisson** con un parametro $\lambda > 0$ se la sua PMF è la seguente: $$p(i) = P\{X=i\} = e^{-\lambda}\cdot \frac{\lambda^{i}}{i!}$$per ogni $i \ge 0$ e si indica con $X \thicksim Poisson(\lambda)$.
Come sempre, verifichiamo la normalizzazione: 
$$\sum\limits_{i=0}^{+\infty} p(i) = \sum\limits_{i=0}^{+\infty} e^{-\lambda}\cdot \frac{\lambda^{i}}{i!} = e^{-\lambda} \sum\limits_{i=0}^{+\infty} \frac{\lambda^{i}}{i!} = e^{-\lambda} e^{\lambda} = 1$$
Si noti che questa sommatoria per convergere necessita che $\lim_{i\rightarrow \infty} \frac{\lambda^{i}}{i!} = 0$ e quindi la PMF di Poisson tende a zero solo se $i$ cresce. 
I suoi  parametri sono:
- **Valor Medio**: $E[X] = \lambda$ perché: $$
\begin{align*}
E[X] &= \sum\limits_{i=0}^{+\infty} i \cdot \left(e^{-\lambda}\cdot \frac{\lambda^{i}}{i!}\right)=\\
&= e^{-\lambda} \cdot \lambda \sum\limits_{i=1}^{+\infty} \not{i} \left(\frac{\lambda^{i-1}}{\not{i}\cdot (i-1)!}\right)=\\
&= e^{-\lambda}\cdot \lambda \sum\limits_{i=1}^{+\infty} \left(\frac{\lambda^{i}}{i!}\right)=\\
&= e^{-\lambda}\cdot \lambda e^{\lambda}= \\
&= \lambda
\end{align*}
$$
- **Varianza**: $Var(X) = (\lambda^{2}+\lambda)-\lambda^{2} = \lambda$. Dimostrazione sulle dispense.
Quindi $\lambda$ è sia la varianza che il valor medio. 

La distribuzione di Poisson ha la caratteristica di *approssimare* abbastanza bene una variabile Binomiale quando si ha:
- Un gran numero di esperimenti $n$
- Una bassa probabilità di successo $p$
- $\lambda = n\cdot p$
Quindi quando siamo in questo caso è conveniente da un punto di vista computazionale usare la distribuzione di Poisson invece che la Binomiale.
Dimostriamo questo legame partendo dalla PMF della binomiale assumendo $\lambda = n\cdot p$:
$$
\begin{align*}
P\{X=i\} &= \binom{n}{i}p^{i}(1-p)^{n-i} =\\
&= \frac{n\cdot (n-1) \cdot ... \cdot (n-i+1)}{i!} (\frac{\lambda}{n})^{i}\cdot \frac{(1-\frac{\lambda}{n})^{n}}{(1-\frac{\lambda}{n})^{i}} =\\
&= \frac{n\cdot (n-1) \cdot ... \cdot (n-i+1)}{n^{i}} \frac{\lambda^{i}}{i!}\cdot \frac{(1-\frac{\lambda}{n})^{n}}{(1-\frac{\lambda}{n})^{i}}
\end{align*}
$$
Se $n$ è grande allora posso trascurare $(1-\frac{\lambda}{n})^{i}$ e si ha $\frac{n\cdot (n-1) \cdot ... \cdot (n-i+1)}{n^{i}} \approx 1$ per cui: 
$$
\begin{align*}
\lim_{n\rightarrow \infty} (1-\frac{\lambda}{n})^{n} &\approx e^{-\lambda}
\end{align*}
$$
e: 
$$
\begin{align*}
\lim_{n\rightarrow \infty} (1-\frac{\lambda}{n})^{i} &\approx 1
\end{align*}
$$
e sostituendo si ottiene $P\{X = i\} \approx \frac{\lambda^{i}}{i!}\cdot e^{-\lambda}$ che è la PMF della distribuzione di Poisson.

Un'importante proprietà della distribuzione di Poisson è la seguente: 
date due variabili aleatorie di Poisson **indipendenti** $X_{1} \thicksim Poisson(\lambda_{1})$ e $X_{2} \thicksim Poisson(\lambda_{2})$ allora $X_{1}+X_{2} \thicksim Poisson(\lambda_{1} + \lambda_{2})$.
Dimostrazione sulle dispense.
### Distribuzione Geometrica

La distribuzione **geometrica** misura *il numero di fallimenti prima del primo successo in un esperimento di prove ripetute*. Il dominio è $[0,+\infty)$ e le sue caratteristiche (valor medio e varianza) dipenderanno dalla probabilità di successo $p$ del singolo esperimento. 
Ha anche una seconda definizione che è: tentativi prima del primo successo, in questo caso il dominio diventa $[1,+\infty)$ perché servirà almeno un tentativo.
Comunque, se consideriamo la prima definizione, è facile capire che $P\{X = 0\} = p$ perché bisogna avere successo nel primo esperimento per avere zero insuccessi. Possiamo poi continuare sfruttando l'indipendenza degli esperimenti:
$P\{X = 1\} = (1-p)\cdot p$ un insuccesso e un successo
$P\{X = 2\} = (1-p)^{2} \cdot p$ due insuccessi e un successo
e così via.
La formula generale sarà quindi $P\{X = k\} = (1-p)^{k}p$ con $k\ge 0$.
La corrispondente PMF è una sequenza decrescente esponenzialmente, possiamo osservare che: 
$$
\begin{align*}
P(X \le k) &= \sum\limits_{i=0}^{k}(1-p)^{k}\cdot p =\\
&= p\cdot \frac{1-(1-p)^{k+1}}{1-(1-p)} =\\
&= 1-(1-p)^{k+1}
\end{align*}
$$
che somiglia molto ad un'operazione di complemento con $P\{X \le k\} = 1-P\{X >k\}$.
I suoi  parametri sono:
- **Valor Medio**: $E[X] = \frac{1-p}{p}$ perché: $$
\begin{align*}
E[X] &= \sum\limits_{k=0}^{+\infty}k\cdot (1-p)^{k}\cdot p =\\
&= p \cdot (1-p) \sum\limits_{k=1}^{+\infty} \frac{\partial}{\partial p}-[(1-p)^{k}] =\\
&= -p \cdot (1-p) \cdot \frac{\partial}{\partial p} \sum\limits_{k=1}^{+\infty} (1-p)^{k} =\\
&= -p \cdot (1-p)\cdot \frac{\partial}{\partial p}\left[\frac{1}{p}-1\right]=\\
&= \frac{1-p}{p}
\end{align*}
$$
- **Varianza**: $\sigma^{2} = \frac{1-p}{p^{2}}$ questa la dimostreremo più avanti.
La distribuzione geometrica ha un'importante proprietà: **memoryless**; ed è l'unica distribuzione discreta ad averla. 
> [!note] Memoryless
> La proprietà di **memoryless** sostiene che: $$P\{X \ge n+m|X\ge n\} = P\{X \ge m\}$$ cioè l'osservazione degli esperimenti non aumenta la probabilità di successo.

Per capire, consideriamo l'esperimento lancio di una moneta e ipotizziamo di aver perso per $n$ volte. Ci chiediamo qual è la probabilità di perdere per ulteriori $m$ volte prima di ottenere un successo. Ovviamente, visto che i lanci sono tutti *indipendenti* non otteniamo alcuna informazione avendo osservato i primi $n$ lanci. Quindi la probabilità è indipendente da quante volte nel passato l'esperimento ha avuto esito di insuccesso.
Vediamo la dimostrazione formale:
$$
\begin{align*}
P\{X \ge n+m|X\ge n\} &= \frac{P\{X \ge n+m,X \ge n\}}{P\{X \ge n\}} =\\[4pt]
&= \frac{P\{X \ge n+m\}}{P\{X \ge n\}} =\\[4pt]
&= \frac{1-P\{X \le n+m-1,X \ge n\}}{1-P\{X \le n-1\}} =\\[4pt]
&= \frac{1-[1-(1-p)^{n+m}]}{1-[1-(1-p)^{n}]} =\\[4pt]
&= (1-p)^{m} =\\[4pt]
&= P\{X \ge m\}
\end{align*}
$$

Se si usa l'altra definizione, la proprietà di memoryless continua a valere ma con maggiore invece che maggiore e uguale.
#Attenzione un errore che gli studenti fanno è confondere la memoryless con: $$P\{X \ge n+m|X\ge n\} = P\{X \ge n+m\}$$che vale *solo* se $\{X \ge n+m\}$ e $\{X\ge n\}$ sono *indipendenti* che in generale non è vero. Infatti: $$P\{X \ge n+m|X\ge n\} = \frac{P\{X\ge n+m\}}{P\{X \ge n\}} \not = P\{X \ge n+m\}$$
## Probability Generating Functions PGF

Per variabili aleatorie discrete e non negative (tutte quelle che facciamo noi lo sono) si può scrivere la *z-trasformata* detta anche **Probability Generating Function PGF**.

> [!note] Probability Generating Function
> Per una variabile aleatoria $X$ si può definire la **Probability Generating Function**: $$G(z) = E[z^{X}] = \sum\limits_{n=0}^{+\infty}p_{n}\cdot z^{n}$$ dove $z$ è un numero complesso.

La somma converge se $|z| \le 1$.
Proprietà:
1. **normalizzazione**: $$G(1) = \sum\limits_{n=0}^{+\infty} p_{n}1^{n}=1$$
2. **valor medio**: $$E[X] =\sum\limits_{n=0}^{+\infty} p_{n}\cdot n = \frac{\partial}{\partial z}[\sum\limits_{n=0}^{+\infty} p_{n} \cdot z^{n}]_{z=1} = G^{'}(1)$$
3. **derivata seconda**: $$E[X(X-1)] =\sum\limits_{n=0}^{+\infty} p_{n}\cdot n \cdot (n-1) = \frac{\partial^{2}}{\partial z^{2}}[\sum\limits_{n=0}^{+\infty} p_{n} \cdot z^{n}]_{z=1} = G^{''}(1)$$
4. **varianza**: $$Var(X) = E[X^{2}]-E[X]^{2} = E[X(X-1)]+E[X]-E[X]^{2}= G^{''}(1) + G^{'}(1) +G^{'}(1)^{2}$$
5. **univocità**: se due variabili aleatorie $X$ e $Y$ hanno la stessa PGF allora hanno anche la stessa PMF e viceversa. Più formalmente, $$\forall z, G_{X}(z) = G_{Y}(z) \Leftrightarrow \forall n, P\{X = n\} = P\{Y = n\}$$quindi la PGF ha lo stesso contenuto informativo della PMF che caratterizza completamente $X$ e $Y$.
6. **convoluzione**: date $n$ variabili aleatorie *indipendenti* $X_{1},X_{2},...,X_{n}$ di cui si conoscono le PGFs $G_{1}(z),G_{2}(z),..G_{n}(z)$, allora *la PGF della loro somma* $S=X_{1}+X_{2}+\dots +X_{n}$ è: $$\begin{align*} G_{S}(z) &= E[z^{X_{1}+X_{2}+\dots +X_{n}}] =\\[4pt]
&= E[z^{X_{1}}\cdot z^{X_{2}} \cdot ...\cdot z^{X_{n}}] =\\[4pt]
&= E[z^{X_{1}}]\cdot E[z^{X_{2}}] \cdot ...\cdot E[z^{X_{n}}] =\\[4pt]
&= G_{1}(z)\cdot G_{2}(z)\cdot ... \cdot G_{n}(z)\end{align*}$$
Calcoliamo le PGF delle distribuzioni che abbiamo visto:
- **Bernoulli**: 
	$p(0) = 1-p$
	$p(1) = p$
	$G(z) = (1-p)\cdot z^{0}+p\cdot z^{1} = 1-p+p\cdot z$
- **Binomiale**:
	$p(i) = \binom{n}{i} p^{i}\cdot (1-p)^{n-i}$
	$G(z) = \sum\limits_{i=0}^{n}\binom{n}{i} p^{i}\cdot (1-p)^{n-i} \cdot z^{i} = [p\cdot z+(1-p)]^{n}$
	questa si poteva ricavare anche applicando la proprietà di convoluzione. 
- **Poisson**:
	$p(i) = e^{-\lambda}\cdot \frac{\lambda^{i}}{i!}$
	$G(z) = \sum\limits_{i=0}^{+\infty} e^{-\lambda}\cdot \frac{\lambda^{i}}{i!} \cdot z^{i} =\frac{e^{-\lambda}}{e^{-\lambda z}}\cdot \sum\limits_{i=0}^{+\infty} e^{-\lambda z} \cdot \frac{(\lambda z)^{i}}{i!} = e^{-\lambda +\lambda z}$
- **Geometrica**:
	$p(i) = (1-p)^{i}\cdot p$
	$G(z) = \sum\limits_{i=0}^{n} (1-p)^{i}\cdot p \cdot z^{i} = \frac{p}{1-z\cdot (1-p)}$

Per la distribuzione di Poisson, il calcolo della varianza si semplifica notevolmente:
$G(z) = e^{-\lambda +\lambda z}$
$G^{'}(z) = \lambda \cdot e^{-\lambda}\cdot e^{\lambda z}$ da cui $G^{'}(1) = \lambda$ che è il valor medio
$G^{''}(z) = \lambda^{2} \cdot e^{-\lambda}\cdot e^{\lambda z}$ da cui $G^{'}(1) = \lambda^{2}$
da cui: $\sigma^{2} = G^{''}(1)+G^{'}(1) -G^{'}(z)^{2} = \lambda^{2}+\lambda -\lambda^{2} = \lambda$

Per la distribuzione Geometrica, troviamo la varianza che non avevamo dimostrato:
$G(z) = \frac{p}{1-z\cdot (1-p)}$

$G^{'}(z) = \frac{p\cdot (1-p)}{[1-z\cdot (1-p)]^{2}}$ da cui $G^{'}(1) = \frac{1-p}{p}$ che è il valor medio

$G^{''}(z) = \frac{2\cdot p \cdot (1-p)^{2}}{[1-z\cdot (1-p)]^{3}}$ da cui $G^{''}(1) = \frac{2\cdot (1-p)^{2}}{p^{2}}$

da cui: $\sigma^{2} = G^{''}(1)+G^{'}(1) -G^{'}(z)^{2} = \frac{1-p}{p^{2}}$
## Distribuzioni Continue

Definiamo le più importanti distribuzioni *continue*.
### Distribuzione uniforme

Una variabile aleatoria è **uniformemente distribuita** se la sua PDF è constante nell'intervallo $[a,b]$: 
$$
\begin{equation}
f(x)= \begin{cases}\frac{1}{b-a} & a \le x \le b \\
0 & \text{altrimenti}\end{cases}
\end{equation}
$$
e si indica con $X \thicksim U(a,b)$.

![Uniformemente Distribuita|central|400](https://analystprep.com/cfa-level-1-exam/wp-content/uploads/2021/09/cfa-level-1-continuous-uniform-random-variable-1.jpg)

I suoi parametri sono:
- **Valor Medio**: $E[X] = \int_{a}^{b} \frac{1}{b-a} \cdot x \cdot dx = \frac{b+a}{2}$
- **Varianza**: $Var(X) = E[X^{2}]-E[X]^{2}= \frac{(b-a)^{2}}{12}$
La CDF ha la seguente forma: $F(X) = \int_{a}^{x} \frac{1}{b-a} dx = \frac{x-a}{b-a}$ con $a \le x \le b$.
### Distribuzione esponenziale

Una variabile aleatoria continua **distribuita esponenzialmente** con un *rate* $\lambda > 0$ se la sua PDF ha la seguente forma: 
$$
\begin{equation}
f(x)= \begin{cases}\lambda\cdot e^{-\lambda x} & x \ge 0 \\
0 & x < 0\end{cases}
\end{equation}
$$
e si indica con $X \thicksim exp(\lambda)$.
Il dominio è $[0,+\infty)$ e si può facilmente ottenere la CDF integrando la PDF: $F(x) = \int_{0}^{x}\lambda \cdot e^{-\lambda y}dy = \lambda [-\frac{1}{\lambda}\cdot e^{-\lambda y}]_{0}^{x}=1-e^{-\lambda x}$ con $x \ge 0$, si vede che tende ad 1 esponenzialmente.

![Esponenziale|center|400](https://upload.wikimedia.org/wikipedia/commons/thumb/f/f5/Exponential_distribution_pdf_-_public_domain.svg/1200px-Exponential_distribution_pdf_-_public_domain.svg.png)

Questo tipo di distribuzione, assume valori molto grandi con valori molto piccoli di probabilità infatti sono buoi modelli per misurare il tempo che trascorre tra due eventi che si verificano casualmente (ad esempio terremoti).

I suoi parametri sono (calcolati entrambi integrando per parti):
- **Valor Medio**: $E[X] = \frac{2}{\lambda^{2}}$
- **Varianza**: $Var(X) = \frac{1}{\lambda^{2}}$

Due importanti caratteristiche della distribuzione esponenziale sono:
1. Dati $n$ variabili aleatorie *indipendenti* distribuite esponenzialmente $X_{1},X_{2},...,X_{n}$ con i rispettivi rate $\lambda_{1},\lambda_{2},...,\lambda_{n}$, la variabile aleatoria $Y = min\{X_{1},X_{2},...,X_{n}\}$ è anch'essa esponenziale con un rate $\lambda = \sum\limits_{i=1}^{n}\lambda_{i}$.
2. Data $X$ distribuita esponenzialmente con un rate $\lambda >0$, allora $P\{X>s+t |X>t\} = P\{X>s\}$ detta **memoryless**.
	Per capire, assumiamo di stare studiando la vita di un dispositivo rappresentata da una variabile aleatoria esponenzialmente distribuita. La proprietà di memoryless ci permette di dire che il momento in cui il dispositivo smetterà di funzionare è indipendente da quanto tempo nel passato ha funzionato. Proviamolo formalmente:
	$$
\begin{align*}
P\{X>s+t |X>t\} &= \frac{P\{X>s+t, X>t\}}{P\{X>t\}} =\\
&= \frac{P\{X>s+t\}}{P\{X>t\}} =\\
&= \frac{e^{-\lambda(s+t)}}{e^{-\lambda t}} =\\
&= e^{-\lambda s} =\\
&= P\{X > s\}
\end{align*}
$$
3. Data $X$ distribuita esponenzialmente con un rate $\lambda >0$, allora $Y = \lfloor X \rfloor$ è geometrica con probabilità $p = 1-e^{-\lambda}$.
	$$
\begin{align*}
p_{Y}(k) &= P\{Y = k\} =\\[4pt]
&= P\{k \le X \le k+1\} =\\[4pt]
&= F(k+1) - F(k) =\\[4pt]
&= (1-e^{-\lambda(k+1)}) - (1-e^{-\lambda k}) =\\[4pt]
&= (e^{-\lambda})^{k}\cdot (1-e^{-\lambda}) =\\[4pt]
&= (1-p)^{k}\cdot p
\end{align*}
$$
	che è la distribuzione geometrica.
## Laplace-Stieltjes Transform LS

La trasformazione Laplace-Stieltjes rispecchia il concetto di PGF per variabili aleatorie *continue e non negative*.
> [!note] Laplace-Stieltjes
> Data una PDF $f(t)$ si può definire LS come: $$L(s) = E[e^{-st}] = \int_{0}^{+\infty}e^{-st}\cdot f(t) dt$$ con $s$ complessa.

L'integrale converge se $Re(s)\ge 0$. Vediamo le sue proprietà:
1. **Normalizzazione**: $L(0) =1$
2. **Central Moments**: $\sigma^{2}= L^{''}(0)+[L^{'}(0)]^{2}$
3. **Univocità**: se due variabili aleatorie $X$ e $Y$ hanno la stessa LST allora hanno anche le stesse PDFs e viceversa. Più formalmente, $\forall x, f_{X}(x) = f_{Y}(y) \Leftrightarrow \forall s, L_{X}(s) = L_{Y}(s)$  
4. **Convoluzione**: date $n$ variabili aleatorie *indipendenti* $X_{1},X_{2},...,X_{n}$ e sapendo le loro LST $LS_{1},LS_{2},...,LS_{n}$, allora si può ottenere la LST della loro somma facendo: $$
	\begin{align*}
L_{S}(s) &= E[e^{-s(X_{1}+X_{2}+\dots+X_{n})}] =\\[4pt]
&= E[e^{-s\cdot X_{1}}] + E[e^{-s\cdot X_{2}}] + \dots + E[e^{-s\cdot X_{n}}] =\\[4pt]
&= L_{1}(s)\cdot L_{2}(s) \cdot ... \cdot L_{n}(s)
\end{align*}
$$
Calcoliamo la LST della distribuzione esponenziale:
$$
\begin{align*}
L(s) &= E[e^{-st}] =\\
&= \int_{0}^{+\infty}e^{-(s+\lambda)\cdot t} dt =\\
&= \frac{\lambda}{-(s-\lambda)} \cdot [e^{-(s+\lambda)\cdot t}]_{0}^{+\infty} =\\
&= \frac{\lambda}{s+\lambda}
\end{align*}
$$
da cui si ricavano:
$L^{'}(s) = -1\cdot \frac{\lambda}{(s+\lambda)^{2}}$
$L^{''}(s) = 2\cdot \frac{\lambda}{(s+\lambda)^{3}}$
e quindi:
$E[X] = -L^{'}(0) = \frac{1}{\lambda}$
$E[X^{2}] = L^{''}(0) = \frac{2}{\lambda^{2}}$
$Var(X) = E[X^{2}] - E[X]^{2}= \frac{1}{\lambda^{2}}$
### Distribuzione normale

Una variabile aleatoria continua è Gaussiana se la sua PDF ha la seguente forma:
$$f(x) = \frac{1}{\sqrt{2\pi}\cdot\sigma}e^{\frac{-(x-\mu)^{2}}{2\sigma^{2}}}$$
con $\sigma > 0$ e si indica con $X \thicksim \mathcal{N}(\mu,\sigma^{2})$.
Il dominio della PDF è $(-\infty,+\infty)$ e assume il valore massimo in $x=\mu$ e quel valore massimo è pari a: $\frac{1}{\sqrt{2\pi}\cdot\sigma} \thicksim \frac{0.4}{\sigma}$. È ovviamente simmetrica rispetto a $\mu$ che è anche il suo valor medio.
Inoltre, più è grande $\sigma$ più bassa è la PDF e più alte saranno le code (l'area deve essere 1).

![Normale|center|400](https://upload.wikimedia.org/wikipedia/commons/thumb/7/74/Normal_Distribution_PDF.svg/325px-Normal_Distribution_PDF.svg.png)

Supponiamo di voler calcolare $F(a) = P\{X \le a\}$ con $X \thicksim N(\mu,\sigma^{2})$, dovrei risolvere il seguente integrale: 
$$
\begin{align*}
F(a) &= \int_{-\infty}^{a}\frac{1}{\sqrt{2\pi}\cdot\sigma}e^{\frac{-(x-\mu)^{2}}{2\sigma^{2}}} dx =\\[4pt]
&= \frac{1}{\sqrt{2\pi}\cdot\sigma}\int_{-\infty}^{\frac{a-\mu}{\sigma}} \sigma \cdot e^{\frac{-z}{2}} dz =\\[4pt]
&= \Phi(\frac{a-\mu}{\sigma})
\end{align*}
$$
dove la $\Phi(z)$ è la CDF della **distribuzione normale standard**, scritta in questo modo $Z \thicksim \mathcal{N}(0,1)$. Questa funzione è tale che:
- $\Phi(0) = \frac{1}{2}$ perché è metà dell'area ed è simmetrica.
- $\Phi(-a) = P\{Z \le -a\} = P\{Z \ge a\} = 1 -\Phi(a)$
Quindi per risolvere l'integrale dobbiamo calcolare numericamente i valori della $\Phi(z)$ ma ci basta farlo solo per i valori positivi. C'è un altro fattore che restringe il numero di calcoli da effettuare ovvero che questo tipo di distribuzione decresce molto rapidamente. 
L'unità di misura che utilizziamo per valutare la PDF è la *deviazione standard* $\sigma$ e osserviamo che solo lo 0.3% dei valori sono a più di 3 deviazioni standard dal punto medio. Quindi possiamo assumere che i valori oltre le 5 deviazioni standard siano praticamente zero e tabulare $Z \thicksim \mathcal{N}(0,1)$ solo per valori $(0,5]$. 

Una proprietà della distribuzione normale è la seguente:
date $n$ variabili aleatorie *indipendenti e normalmente distribuite* $X_{1},X_{2},...,X_{n}$ e una variabile $Y = \sum\limits_{i=1}^{n} \pm X_{i}$ è anch'essa normalmente distribuita con parametri $\mu = \sum\limits_{i=1}^{n}\pm \mu_{i}$ e $\sigma^{2}= \sum\limits_{i=1}^{n}\sigma_{i}^{2}$.
## Teorema del Limite Centrale

La distribuzione normale ha una grande importanza perché in molti problemi moderni reali è richiesto di sommare un grande numero di eventi indipendenti e, con la distribuzione normale si può applicare il teorema del limite centrale.
> [!note] Teorema del limite centrale
> Date $n$ variabili aleatorie *indipendenti e identicamente distribuite* che hanno:
> - stesso valor medio $\mu$ *finito* 
> - stessa varianza $\sigma^{2}$ *finita*
> Definita la variabile aleatoria $S = \sum\limits_{i=1}^{n}X_{i}$ essa ha $\mu_{S}=n\cdot \mu$ e $\sigma^{2}_{S}=n\cdot \sigma^{2}$
> Inoltre, per valori grandi di $n$ ($n\ge 30$) può essere approssimata come una distribuzione normale, in altre parole: $S \thicksim \mathcal{N}(n\cdot \mu,n\cdot \sigma^{2})$


## Percentile

### Distribuzione Chi-Square 

### Distribuzione Student

## Distribuzioni heavy tailed

