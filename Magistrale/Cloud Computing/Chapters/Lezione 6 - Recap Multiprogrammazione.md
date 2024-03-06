
I requisiti chiave per la virtualizzazione sono:
1. **Equivalenza**: il sistema operativo in esecuzione su una macchina virtuale sopra un hypervisor deve comportarsi in modo equivalente a come lo farebbe se eseguito direttamente sulla macchina fisica. Questo assicura la coerenza delle operazioni tra le due modalità di esecuzione.
2. **Controllo delle Risorse**: l'hypervisor deve avere il completo controllo delle risorse fisiche del sistema, mentre il sistema operativo guest deve gestire le risorse virtuali fornite dall'hypervisor. Questo divisione di controllo è essenziale per garantire la separazione e l'isolamento tra le macchine virtuali.
3. **Efficienza**: affinché la virtualizzazione sia efficiente, una parte delle istruzioni deve essere eseguita direttamente sull'hardware senza passare attraverso l'hypervisor. Ciò assicura una maggiore performance ed evita un eccessivo overhead dovuto alla traduzione delle istruzioni.

Ricordare: il set di istruzioni usato nel sistema virtuale è lo stesso utilizzato nell'hardware effettivo.

## Multiprogrammazione

I requisiti che abbiamo elencato sono simili a quelli necessari per la multiprogrammazione cioè la capacità di un sistema di eseguire più processi "contemporaneamente". 

## VCPU

## Cambio di Contesto

## Supporto alla multiprogrammazione

