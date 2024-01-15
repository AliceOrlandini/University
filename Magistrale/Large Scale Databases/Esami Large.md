# Primo progetto

Fa provare l’applicazione con lo schermo condiviso.

Fa uno stress test ad esempio un utente registrato che non ha amici, la funzione che mostra gli amici consigliati cosa mostra? A loro non mostrava nulla, Ducange voleva che ci fosse un errore.

Ora ha chiesto di usare la funzione di seguire un utente. Non vuole tante parole, se chiede di scegliere l’opzione 8 non bisogna parlare tanto ma solo premere 8. 
Usando l’azione segui utente, l’applicazione chiede l’username, Ducange giustamente chiede, come faccio io, appena registrato, a conoscere i nomi degli utenti?
Il ragazzo risponde che c’è un comando per la lista degli utenti e Ducange ribatte che quella lista doveva essere mostrata quando si sceglieva di seguire un utente. Inoltre, controllano in diretta gli use cases nella documentazione che non sono coerenti con il comportamento dell’applicazione. 

Come trovo i generi preferiti dell’utente? Non si può. 

Mostrano i primi 5 libri suggeriti ad un utente in base ai suoi amici, però hanno le review su Mongo mentre le Recommendation su Neo, Ducage allora chiede se hanno fatto un join tra due db. Perché o tutte le informazioni sono su Neo (e non è questo caso) quindi per forza hanno fatto anche una query su Mongo. Non si fa, voleva che solo se necessario venisse fatta una seconda query su Mongo ad esempio tramite una seconda interazione dell’utente.
Quando si fa una recommendation bisogna usare i dati nel grafo e basta, punto.

Ora vedono i book statistics, Ducange non vuole troppe informazioni perché loro mostrano oltre che ai 5 book più letti anche le review. Ducange vuole che un’operazione faccia solo quello quindi se poi un utente vuole le review ci sarà la funzione per farlo (alla fine è lo stesso discorso di prima). I ragazzi rispondono che hanno mostrato solo 5 libri con le review e lui Ducange ribatte “e menomale che non ne avete mostrati di più”.

Ora si passa alla presentazione, praticamente come quella fatta in aula, mi sa infatti che potremo riutilizzarla in qualche modo. 

Schiavo fa notare che il diagramma UML sulle slide non è coerente con quello sulla documentazione. 

Per quanto riguarda il dataset nella presentazione hanno spiegato come lo hanno processato. 

Hanno elencato i requirements e le considerazioni sul CAP.

Alcuni dati li hanno messi sia su Mongo che su Neo. Hanno usato anche document embedding per mettere le ultime 5 recensioni nella collection dei libri. 
Hanno messo un elenco delle query.

Hanno spiegato come avviene la consistency tra Mongo e Neo. Schiavo chiede perché c’è una strict consistency tra Mongo e Neo quando prima hanno detto che in generale hanno usato eventual consistency. 
Hanno sbagliato, non dovevano garantire la consistenza anche su Mongo che tanto sono solo recommendation quindi è importante se siano consistenti. 
Praticamente non hanno implementato una eventual consistency completa perché un pezzo del database ha una strinc consistency (si avvalora la tesi che per Duca un db meno consistente è meglio è ahahahah)

Ora chiede come hanno messo le impostazioni del db per l’aggiornamento delle repliche (W1 R1 quelle lì)

Ora parlano dello Sharding, hanno usato un Consistent Hasing. La loro soluzione non va bene ma loro dicono che se avessero fatto lo sharging in un altro modo avrebbero avuto problemi con le analytics. Allora Ducange dice che lo sharding doveva essere fatto su time intervals (mesi, un paio d’anni) oppure bastava proprio non farlo.

Ora ci sono gli Indexes. Ne hanno 2, uno per gli user e uno per i book.

Fine, Ducange ha detto che l’errore più grosso è stata la strict consistency tra i database. 
Il codice non è stato visto e Schiavo si è limitato a dire “code well organized”. 
Voto tra il primo e il secondo quartile quindi avranno preso tra 18 e 24. 

# Secondo progetto 

Si parte dalla demo del progetto, questi hanno un sito web probabilmente fatto con Bootstrap, fanno un tour dell’applicazione. 

Schiavo chiede di aggiungere una Review e l’applicazione esplode (palese Schiavo sapeva di questa cosa e gliel’ha fatta provare di proposito). La loro applicazione è piena di bug, la nostra andrà stress testata malamente.

Ora ha aperto MongoCompass e gli fa cercare i nuovi film aggiunti. 

Ducange chiede dove sono le aggregation nell’applicazione e loro rispondono “la lista degli attori” e Ducange parecchio innervosito “queste sono find, non aggregation!”
Vuole vedere il pezzo di codice dove fanno la ricerca dei film per nome dell’attore, fa mettere quindi mettere un breackpoint per vedere quel pezzo runnare. 

Ducange fa ”voglio sentire la voce di Lucas” quindi vuole che tutti si parli. Il class diagram lo stava descrivendo quell’altro e Duca ripete “voglio sentire Lucas”. Quindi per non rischiare tutti devono sapere tutto. 

Nel class diagram mancano pezzi, tipo i commenti dove sono?

Vuole sapere esattamente la grandezza del dataset.

Non hanno fatto sharding.
L’index l’hanno messo sull’user su neo.
Hanno fatto una slide sull’architettura dell’applicazione con spiegato il MVC che hanno usato.

<<<<<<< HEAD
Momento del voto, ha detto solo che hanno ottenuto il minimo ed ha aggiunto che vedranno cosa faranno domani allo scritto. 
=======
Momento del voto, ha solo detto che hanno ottenuto il minimo ed ha aggiunto che vedranno cosa faranno domani allo scritto. 
>>>>>>> origin/main
