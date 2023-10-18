Dopo essere certi che i nostri numeri sono uniformemente distribuiti dobbiamo chiederci anche se sono indipendenti gli uni dagli altri. Per testare questa condizione possiamo 
- effettuare un’ispezione visuale con ad esempio *lag plots*
- numericamente, tramite la funzione di autocorrelazione (non scenderemo nei dettagli).

Per verificare la bontà di un generatore di numeri casuali tramite la cosa migliore è basarsi sulla letteratura. 
#Attenzione non usare mai generatori non testati. 

Abbiamo visto come generare numeri uniformemente distribuiti ma ora proviamo a trasformarli in un altro tipo di distribuzione, ad esempio quella esponenziale. 
I metodi per fare ciò sono:
- inverse transform 
- convolution

## Inverse transform

Supponiamo di volere un sample $x$ da una variabile aleatoria data la sua CDF $F(x)$. Il codominio della CDF è tra 0 e 1, estraiamo usando un generatore che genera numeri uniformemente distribuiti un valore $u$ e computiamo la funzione inversa della CDF $x = F^{-1}(u)$. Quindi $x$ è un sample originato dalla distribuzione $F(x)$. 
Il problema di questo metodo è che c’è bisogno di invertire la CDF che molte volte non si può fare.

Nel caso di funzioni a gradino se conosco la lunghezza di ogni segmento non ho bisogno di calcolare l’inversa perché uno gradino corrisponde a un valore della PMF. 
Per fare ciò avrò bisogno di memoria per contenere una struttura dati che contiene per ogni gradino il valore della PDF.

Ad esempio la distribuzione normale non ha una forma chiusa. Per risolvere questo problema abbiamo un altro metodo quello della convoluzione con il Central Limit Theorem. Generiamo una serie di numeri indipendenti e uniformemente distribuiti tra 0 e 1 e poi li sommiamo tutti. Troviamo X che avrà un certo valor medio e varianza dato dal teorema. 

Il problema qui è il periodo, per generare questi numeri avremo bisogno di tanti numeri casuali e quindi raggiungeremo prima il periodo.

# How to do a simulation analysis

Ora che abbiamo visto tutto sui simulatori occupiamoci delle analisi. Il grande problema è che non c’è una procedura standard quindi Nardini ci darà solo delle linee guida che noi dovremo usare a seconda delle e

