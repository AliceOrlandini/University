
## Full Virtualization

La **full virtualization** è un approccio tecnicamente obsoleto, ma lo esaminiamo attentamente per acquisire una comprensione più approfondita delle tecniche attuali. 
Precedentemente, abbiamo esaminato la multi-programmazione poiché le logiche coinvolte sono in gran parte simili a quelle impiegate nella virtualizzazione.

Similmente alla multi-programmazione, dove ogni processo percepisce di avere il completo controllo delle risorse fisiche grazie all'intermediazione del sistema operativo, **l'hypervisor** (noto anche come **Virtual Machine Monitor**) conferisce a ciascuna macchina virtuale l'illusione di avere il totale controllo delle risorse. 
Pertanto, il ruolo dell'hypervisor può essere paragonato a quello del sistema operativo, ma le sue funzionalità sono estese in modo da consentire l'esecuzione simultanea di diverse macchine virtuali, ognuna dotata del proprio sistema operativo.

## Hypervisor

> [!note] Hypervisor
> Un **hypervisor**, o Virtual Machine Monitor (VMM), è un software che *consente l'esecuzione simultanea di più sistemi operativi su una singola macchina fisica*, gestendo l'allocazione delle risorse e l'isolamento tra le macchine virtuali.

Se le architetture del sistema target e dell'host coincidono, è possibile implementare l'hypervisor riducendo al minimo l'utilizzo dell'emulazione. Come noto, l'emulazione aumenta l'overhead e di conseguenza il ritardo dovuto alla necessità di tradurre il codice in binario.

Nella maggior parte dei casi, il codice del sistema operativo o del software in esecuzione in una macchina virtuale può essere eseguito *direttamente sull'hardware* senza richiedere l'intervento dell'hypervisor.

## Virtualizzazione della CPU

Il primo passo coinvolge la creazione di una *rappresentazione virtuale della CPU* mediante tecniche simili a quelle impiegate nella multi-programmazione. 
In questo contesto, si utilizzano CPU virtuali realizzate attraverso strutture dati e scambiate mediante il meccanismo di cambio di contesto. Fondamentalmente, trattiamo il sistema operativo ospite (guest OS) come se fosse un'applicazione ordinaria. 
La differenza chiave risiede nel fatto che *il guest OS viene eseguito interamente in modalità utente*. Pertanto, sorge la domanda: come riesce a eseguire istruzioni privilegiate? 
La risposta è semplice

## Virtualizzazione della memoria

## Virtualizzazione dei device I/O

