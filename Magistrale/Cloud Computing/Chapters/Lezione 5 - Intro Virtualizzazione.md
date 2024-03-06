
> [!note] Virtualizzazione
> Per **virtualizzazione** si intende la rappresentazione di una risorsa fisica in forma *simulata*, tramite un layer software. 

Il software installato sulla macchina fisica (host) viene anche chiamato “virtualization layer” ed è composto da varie tecniche di virtualizzazione. Il meccanismo della virtualizzazione permette di separare le risorse computazionali fisiche dall’accesso diretto degli utenti, per una migliore gestione delle risorse. 

Ogni tipo di risorsa può essere virtualizzata, generalmente si dividono in due categorie:
- obbligatorie: come CPU, RAM e hard disk, necessarie per far funzionare un server
- opzionali: come tutte le periferiche, non necessariamente necessarie per il funzionamento del server
Per quanto riguarda le risorse obbligatorie è importante notare che è possibile virtualizzare solo se c’è almeno una corrispondenza fisica che empwers la rappresentazione fisica. 



Ad esempio un processore virtuale non può essere virtualizzato senza un processore fisico. 
Diverso è per le periferiche, dove non è necessario avere una corrispondenza fisica in hardware. 
In generale, il virtualization layer trasforma le risorse fisiche in una versione virtuale ma la copia virtuale può anche non essere una copia fedele di quella reale, questa è una feature in quanto si può per esempio creare un processore a 32 bit a partire da uno fisico a 64 bit. 

