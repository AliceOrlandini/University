I due tipi di modelli per lo scambio di messaggi nei sistemi concorrenti sono:
1. **Memoria Condivisa (Shared Memory)**: I processi comunicano condividendo variabili in una memoria comune accessibile a tutti.
2. **Passaggio di Messaggi (Message Passing)**: I processi comunicano inviando e ricevendo messaggi attraverso canali di comunicazione definiti.
Si può dimostrare che questi due modelli sono equivalenti ma noi per evidenziare lo scambio di messaggi tra i nodi useremo il secondo metodo.

La differenza principale tra **processi** e **thread** riguarda la condivisione delle risorse e lo spazio di indirizzamento:
- **Processi**: Sono entità indipendenti con il proprio spazio di indirizzamento in memoria. Ogni processo ha le sue risorse allocate dal sistema operativo e comunica con altri processi tramite meccanismi come la memoria condivisa o il passaggio di messaggi.
- **Thread**: Sono flussi di esecuzione all'interno dello stesso processo. Condividono lo spazio di indirizzamento e le risorse del processo padre, il che permette una comunicazione e sincronizzazione più efficienti tra thread.

