Studiamo le regole di temporizzazione che sono fondamentali da capire quando utilizziamo il tool di sintesi. 
Abbiamo già detto che tutti i sistemi che andremo ad implementare saranno formati da un registro, della logica ed un altro registro (R-L-R) con un clock, un input ed un output. 

Prima di vedere le timing issues ripassiamo la differenza tra Latch e Flip-Flop: 
- i **Latch** sono level sensitive, ovvero sono sensibili all'ingresso quando il clock è pari ad 1. (*transparent mode*)
- i **Flip-Flop** invece sono sensibili all'ingresso solo in corrispondenza del fronte di salita del clock.
Noi ci concentreremo sui Flip-Flop Positive Edge Triggered che, a differenza del Latch, *non* è trasparente, infatti nel progetto evitare di fare Latch. 

In generale, in un circuito integrato ciò che si vorrebbe è che il fronte di salita del clock arrivi a tutti gli elementi del circuito nello stesso istante. Invece, c’è sempre un ritardo $\tau_{i}$ tra il fronte di salita che arriva al componente $i$ e il ritardo $\tau_{j}$ che arriva al componente $j$, e la differenza tra questi due ritardi è chiamato **clock skew** $\delta_{ij}$.
Ad esempio nell’FPGA avevamo detto che c’era una linea solo per il clock, lo scopo era proprio quello di ridurre al massimo il clock skew. Questo diventa un problema quando ci sono di aree del circuito che interagiscono tra loro. 

Per ridurre lo skew, spesso si utilizza un approccio **GALS** (Global Asyncronous Local Syncronous) in cui abbiamo delle *zone localmente sincronizzate*. Ad esempio 4 zone in cui i dispositivi sono sincronizzati con rispettivamente $clock_{1}$, $clock_{2}$, $clock_{3}$ e $clock_{4}$. Questo approccio riduce il clock skew nelle sottoaree identificate. 

# Timing Classifications

Vediamo come si analizza il timing di un sistema digitale sincronizzato formato da R-L-R ipotizzando che non ci sia skew.
La prima regola da identificare è capire quali sono i vincoli di tempo in modo da avere una corretta funzione di uscita. Quindi dobbiamo ricordare le regole di pilotaggio di un Flip-Flop. 

La regola di pilotaggio fondamentale è che il dato in input sia *stabile* per almeno $t_{setup}$ (lui lo chiama $t_{su}$) prima del fronte di salita del clock, e $t_{hold}$ dopo il fronte di salita. Seguendo questa regola, l'output del registro, si aggiornerà dopo un tempo clock-to-Q: $t_{c-q}$ che rappresenta la differenza tra il fronte di salita del clock e il momento in cui l’output si adegua e diventa stabile.

![[timing_class.webp|center|400]]

Ora inseriamo nel nostro circuito della *logica*, definiamo la $t_{plogic}$ il propagation delay della logica. Questo valore è pari a peggiore tra $t_{pHL}$ e $t_{pLH}$ date tutte le possibili configurazioni in input.

Sappiamo che molte volte prima di ottenere l'output finale della logica, ci possono essere dei piccoli glitch prima che esso diventi stabile. Definiamo quindi il **contamination delay** come il delay che si ha dal cambiamento dell'input al primo cambiamento dell'output: $t_{\text{cd logic}}$. Come detto, questo output può non essere quello definitivo. In questo caso dobbiamo considerare il tempo più corto. 
Il contamination delay non è solo della logica ma anche dei registri, in questo caso consideriamo $t_{\text{cd register}}$.

# The two Timing Rules

Studiamo un circuito R1-L-R2 con lo stesso clock.

![[timing_basics.webp|center|400]]

Il periodo del clock è pari a $T$.
Assumiamo che D1 (l'input al registro 1) sia pilotato in modo corretto ovvero sarà stabile per $t_{setup1}$ e $t_{hold1}$.
L'output Q2 del registro 1 cambierà e sarà stabile dopo un periodo clock-to-queue chiamato $t_{cq1}$.
Se guardo D2 (l'input al registro 2), questo si adeguerà dopo $t_{plogic}$. Il sampling di questo nuovo valore avverrà in corrispondenza del fronte di salita del clock e il dato dovrà essere stabile per $t_{setup2}$ e $t_{hold2}$.
La prima regola da considerare, detta **setup violation**, deriva dal fatto che bisogna essere sicuri che:
$$T \ge t_{cq1}+t_{p logic}+t_{setup2}$$

La seconda regola è invece legata al tempo di contaminazione perché se ho la contaminazione di Q2, avrò anche la contaminazione della logica, dopo un periodo $t_{\text{cd logic}}$. 
Il setup qui non è un problema, il problema è che D2 deve rimanere stabile fino a $t_{hold2}$ perché stiamo analizzando D2 rispetto al primo fronte di salita del clock.
La seconda regola, detta **old time violation**, sarà quindi: 
$$t_{\text{cd logic}}+t_{\text{cd reg1}} \ge t_{hold2}$$
*TODO: disegni da mettere*

Poniamo caso che ho un circuito e mi accorgo che c'è una violazione di timing, devo essere preoccupato per una violazione di setup o di una di old time? 
In entrambi i casi l'output non è quello aspettato, però nel caso della setup posso semplicemente aumentare il periodo di clock $T$ mentre per la old time violation non posso farci nulla. 
Quindi tra le due quella più importante da rispettare è la *seconda* perché una volta programmato il dispositivo non c’è modo di risolvere il problema mentre nella prima posso aumentare $T$ che è un parametro esterno.

# Logic synthesis tool 

Vediamo un esempio di logic synthesis tool, non è una cosa che dovremo fare noi però è importante capire come funziona. 
Consideriamo dei registri e della logica tra loro:

![[logic_sintesi.webp|center|300]]

Vediamo che ci possono essere molti percorsi (path) tra l'input e l'output: 

![[logic_sintesi_path.webp|center|300]]

Quindi per ogni path il tool andrà a calcolare la formula della prima regola. 
Per questo esercizio si possono prendere i delay specificati negli sheet, fatti in questo modo:

| Load | 0.015 | 0.020 | 0.035 |
| ---- | ---- | ---- | ---- |
| Delay C$\rightarrow$ Q | 0.75 | 0.80 | 1.0 |
| Delay C$\rightarrow$ QN | 0.74 | 0.79 | 0.99 |

|  | Setup | Hold |
| ---- | ---- | ---- |
| Delay D$\rightarrow$ C | 0.17 | 0.04 |
Si noti che abbiamo tre valori per ogni delay perché esso dipende dalla capacità che si va caricare. 
A questo punto, si cercano i valori delle capacità di uscita ad ogni componente (e in ingresso al successivo) sempre negli sheet, ad esempio all'ingresso della porta AND ci sarà una capacità $C_{A}$ di un certo valore che troviamo nello sheet. 
Se ho due capacità in parallelo farò la somma tra le due. 
Questo ci serve per capire quale valore andare a guardare nella tabella sopra.
Poi faccio la somma dei tre valori per ogni path, il risultato è il seguente: 

![[sintesi_tool_table.webp|center|400]]

Si dice **critical path** il percorso con il delay maggiore tra tutti i path nel circuito. 

# Sources of Clock Skew and Jitter in Clock Network

**PLL**: Phase Lock Loop, da un clock con una determinata frequenza dato dal quarzo si vuole ottenere un clock ad un’altra frequenza. (? non ho capito bene)

$I_{M}= C_{tot}\cdot \frac{\partial V_{ck}}{\partial t}|_{MAX} =C \frac{V_{dd}}{t_{r}} \approx 2.5A$

Due casi:
1. H-Tree Clock Network
2. Clock Grid Network

