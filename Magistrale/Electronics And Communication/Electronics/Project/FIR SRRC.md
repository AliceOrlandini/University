Progettare un circuito che realizzi un filtro FIR (Finite Impulse Response) con risposta all'impulso SRRC (Square Root Raised Cosine)  con le seguenti caratteristiche: 
- Ordine del filtro $N = 22$
- Campioni per simbolo $= 4$
- Fattore di roll-off $\alpha = 0.5$
$$y[n] = \sum\limits_{i=0}^{N}c_{i}\cdot x[n - i]$$
Per ingressi, uscite e coefficienti utilizzare una rappresentazione a 16 bit. 
Per i coefficienti $c_{k}$ si possono utilizzare i seguenti valori: 
- $c_{0} = c_{22} = -0.0165$
- $c_{1} = c_{21} = -0.0150$
- $c_{2} = c_{20} =0.0155$
- $c_{3} =c_{19} =0.0424$
- $c_{4} =c_{18} =0.0155$
- $c_{5} =c_{17} =-0.0750$
- $c_{6} =c_{16} = -0.1568$
- $c_{7} =c_{15} = -0.1061$
- $c_{8} =c_{14} =0.1568$
- $c_{9} =c_{13}=0.5786$
- $c_{10} =c_{12}=0.9745$
- $c_{11}=1.1366$

L'interfaccia del circuito da progettare è la seguente:

![[circuito_progetto.webp | center]]

Per implementare il filtro FIR nel tuo circuito, puoi utilizzare un'architettura di tipo sommatore - moltiplicatore. L'equazione di uscita sarà la somma pesata degli ultimi $N+1$ campioni di ingresso, moltiplicati per i rispettivi coefficienti.

# ChatGPT

L'implementazione può essere realizzata con una struttura di tipo pipeline, dove i campioni vengono spostati attraverso i vari stadi del filtro. Ogni stadio eseguirà la moltiplicazione per il coefficiente corrispondente e sommerà il risultato parziale con la somma totale accumulata finora.

# Link Utili

[Progetto Studente UniPi](https://github.com/brunocasu/uart-receiver-circuit/tree/master)
[FIR SRRC 1](https://github.com/BBN-Q/VHDL-FIR-filters/tree/master)
[FIR SRRC 2](https://github.com/paulrox/SRRC_FIR/tree/master)
