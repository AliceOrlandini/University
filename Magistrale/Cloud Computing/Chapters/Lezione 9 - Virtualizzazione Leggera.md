
Esistono due tecniche per implementare la virtualizzazione leggera: la para-virtualizzazione e la virtualizzazione a livello di sistema operativo (Operating System Level Virtualization). Iniziamo con la para-virtualizzazione.
## Paravirtualizzazione

Come discusso in precedenza, la full virtualization richiede la virtualizzazione completa dell'infrastruttura, consentendo al sistema operativo guest di funzionare senza modifiche. 
Tuttavia, questo approccio comporta un elevato overhead in quanto le istruzioni che non possono essere eseguite direttamente sull'host devono essere emulate dall'hypervisor. In assenza di supporto hardware, si verifica anche un overhead dovuto ai cambi di contesto tra le VM e l'hypervisor.

Per affrontare queste problematiche, è stato introdotto l'approccio della para-virtualizzazione. In questi sistemi, è presente comunque un hypervisor, ma è progettato considerando che il sistema operativo guest può essere modificato per riconoscere di essere in esecuzione come macchina virtuale. 
Le modifiche al sistema operativo guest si concentrano generalmente sul kernel e non coinvolgono le applicazioni eseguite al suo interno, che rimangono inconsapevoli della virtualizzazione.
## Architettura ed implementazione

L'obiettivo della para-virtualizzazione è introdurre modifiche al sistema operativo guest in modo che eviti di eseguire istruzioni per le quali non ha i privilegi necessari. A tale scopo, l'hypervisor espone un set di API tramite un'interfaccia chiamata Virtualization Layer.

L'hypervisor è responsabile dell'implementazione di tutte le istruzioni e della creazione della rappresentazione virtuale dell'hardware, e possibilmente anche della gestione dell'hardware reale. Pur continuando a verificarsi cambi di contesto, non saranno multipli poiché una volta che il controllo viene passato all'hypervisor, sarà quest'ultimo a occuparsi di completare la procedura.

Per evitare l'emulazione, si modifica il kernel del sistema operativo guest in modo che chiami le **Hypercalls**, ovvero le funzioni esposte dall'hypervisor che sostituiscono le istruzioni non virtualizzabili. 
Le Hypercalls vengono eseguite in modalità system, quindi, avendo i privilegi necessari, possono essere eseguite direttamente sull'hardware.

I vantaggi della para-virtualizzazione includono:
- Significativo aumento delle prestazioni poiché si evita l'emulazione.
- Non richiede il supporto hardware dedicato.

Tuttavia, ci sono anche degli svantaggi:
- Supporta solo versioni modificate del sistema operativo guest.
- Alcuni sistemi operativi non consentono modifiche (ad esempio, Windows...).

Con l'avvento del supporto hardware per la virtualizzazione, si è notato che la para-virtualizzazione non offre vantaggi significativi in termini di prestazioni. Di conseguenza, nel tempo è diventata sempre meno popolare.

## Xen

Xen, rilasciato nel 2003, è uno degli hypervisor più diffusi per quanto riguarda la para-virtualizzazione. Questo software cerca di trovare un equilibrio tra la riscrittura del kernel del sistema operativo guest e l'emulazione. Inoltre, può essere eseguito su un kernel Linux, fornendo così un ambiente flessibile e adattabile.

Dal punto di vista dell'architettura, Xen è caratterizzato da una struttura snella e leggera. Al di sopra del layer costituito da Xen, a un livello di privilegio inferiore, sono presenti i cosiddetti "domini". 
Il numero di domini può variare a seconda delle esigenze, e all'interno di ciascun dominio viene eseguito un sistema operativo con le relative applicazioni. I domini sono isolati l'uno dall'altro e vengono utilizzati per implementare le macchine virtuali.

Esiste un dominio speciale chiamato "Dom0" che dispone dell'accesso alle API di Xen, consentendo la creazione e la distruzione di altri domini. All'interno di Dom0 è possibile installare qualsiasi sistema operativo Linux, dotandolo di tool specifici utilizzati per gestire gli altri domini. 
I domini possono avere accesso diretto a determinati dispositivi di I/O oppure possono utilizzare versioni virtualizzate di tali dispositivi.

## Virtualizzazione Leggera

Ci sono situazioni in cui è necessario solo un ambiente isolato per distribuire le applicazioni, senza la necessità di un intero sistema operativo. 
Utilizzando la virtualizzazione classica per lo sviluppo delle applicazioni, l'overhead non è trascurabile, sia in termini di consumo di memoria che di tempo di elaborazione. Inoltre, è comune che diverse macchine virtuali condividano lo stesso sistema operativo guest, portando a istanze multiple dello stesso sistema operativo in esecuzione sull'hardware.

Per evitare questo scenario, sono state recentemente introdotte nuove tecnologie di virtualizzazione chiamate **lightweight virtualization**, ideali per servizi o applicazioni che non richiedono la virtualizzazione dell'intero sistema, pur garantendo gli stessi vantaggi della virtualizzazione, quali:
- Isolamento
- Installazione dinamica
- Ambiente autocontenuto


## Container

## Docker


