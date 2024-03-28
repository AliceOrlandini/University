
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

Un approccio di tipo lightweight, noto come "Operating System Virtualization" o "Shared Kernel Approaches", è stato proposto di recente. 
Il suo obiettivo principale è quello di minimizzare l'overhead che si verifica quando vengono eseguite più istanze di macchine virtuali con lo stesso sistema operativo installato.

Con questo approccio, l'hypervisor non è più necessario. Invece, dei server virtuali sono abilitati dal kernel della macchina fisica, che è condiviso da tutti i server virtuali. Poiché il kernel è condiviso, tutte le VM hanno lo stesso sistema operativo, ma devono implementare istanze di spazio utente logicamente distinte.

Un esempio di questo approccio sono i Linux Containers. Questi consentono di creare ambienti virtualizzati isolati, utilizzando il kernel Linux sottostante e condividendo le risorse del sistema in modo efficiente, riducendo così l'overhead rispetto alla virtualizzazione tradizionale.

| Pro                                                                                                                                      | Contro                                                                                                               |
| ---------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| La virtualizzazione del sistema operativo è più leggera in termini di overhead poiché tutti virtual server condividono lo stesso kernel. | Tutte le macchine virtuali devono condividere lo stesso kernel che può portare a problemi di sicurezza e isolamento. |
| Un singolo sisteme con lo stesso quantitativo di risorse può supportare più macchine virtuali rispetto alla full virtualization.         | Non tutti i sistemi operativi offrono la possibilità di condividere il kernel (Windows...)                           |

## Container

I container sono un metodo per isolare un insieme di processi e far loro credere di essere gli unici in esecuzione sulla macchina. 
La rappresentazione della macchina fornita ai container può offrire solo un sottoinsieme delle risorse effettivamente disponibili sull'hardware fisico, ad esempio meno memoria, meno spazio su disco o meno capacità di elaborazione.

È importante notare che i container non sono macchine virtuali. I processi in esecuzione all'interno di un container sono processi normali che sono eseguiti nel kernel dell'host. Di conseguenza, non c'è un kernel ospite in esecuzione nel container. 
A causa di questa caratteristica, non è possibile eseguire un sistema operativo diverso dall'host all'interno di un container. Poiché il kernel è condiviso con l'host, il sistema operativo all'interno del container deve coincidere con quello dell'host.

Il principale vantaggio dei container rispetto alle macchine virtuali risiede nelle prestazioni. Non c'è alcuna penalità significativa sulle prestazioni nell'esecuzione di un'applicazione all'interno di un container, poiché condividono il kernel dell'host. Al contrario, l'uso delle macchine virtuali comporta una perdita di prestazioni.

## Linux Containers

I Linux containers sono tra i più famosi e ampiamente adottati. Sono implementati utilizzando due funzionalità del kernel: i **namespaces** e i **control groups**. Queste funzionalità garantiscono che ogni container abbia un ambiente di esecuzione isolato e che possano accedere solo alle risorse per le quali hanno i permessi.

I namespace forniscono un mezzo per segregare le risorse di sistema in modo che possano essere nascoste da processi selezionati. 
Si tratta di un'estensione di una vecchia funzionalità Unix chiamata chroot(), che consentiva di limitare l'accesso a un sottoinsieme di cartelle del sistema. 
Grazie ai **namespace**, i processi possono essere isolati per operare solo in una parte del sistema, garantendo che accedano solo ai file di cui hanno bisogno. 
Sono state definite diverse tipologie di namespace, come ad esempio i network namespaces, utilizzati per nascondere le interfacce di rete.

I namespace, tuttavia, non sono da soli sufficienti per isolare un insieme di processi, poiché esiste un secondo modo per interferire con gli altri: prendere il controllo delle risorse per un lungo periodo, sfruttando le risorse del sistema in modo abusivo.

I **control groups**, noti anche come cgroups, sono gruppi di processi in cui il consumo di risorse è controllato ed applicato. Questi gruppi vengono creati dagli utenti con privilegi di root e ciascun processo deve appartenere a un gruppo, dal quale non può fuggire. Ad esempio, se un processo genera un processo figlio, quest'ultimo continuerà comunque a far parte del gruppo del padre.

