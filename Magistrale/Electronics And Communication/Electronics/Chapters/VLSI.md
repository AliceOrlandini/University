Oggi vedremo come si possono integrare su un’unico substrato un elevato numero di transistor con le relative interconnessioni. Poi, vedremo un’architettura di circuito integrato che ha ampliato la fama di questi dispositivi. Infine, introdurremo International Technology Roadmap for Semiconductors.

# Origini storiche

> Un circuito integrato un componente circuitale in cui è possibile, *su un singolo substrato*, includere componenti elettronici e interconnetterli. 

Il primo circuito integrato risale al 1959 : 

![US Patent|center|500](https://www.nutsvolts.com/uploads/wygwam/NV_0222_Steber_Figure10.jpg)

Nel corso degli anni si è sempre più ridotta la dimensione di questi componenti, adesso siamo ai 5nm.

Ora andiamo nel passato per vedere l’evoluzione dell’architettura dei computer:
- 1986-1989: Intel 486 DX CPU
	- 25 - 33 MHz: questa differenza di frequenza è dovuta al processo di produzione del circuito che non dava sempre gli stessi risultati in termini di caratteristiche elettroniche. Quindi, quei due numeri definiscono i range di funzionamento nel peggiore e nel migliore dei casi. (in questo modo potevano vendere tutti i dispositivi (mettevano le mani avanti))
	- 1.2 Mega Transistor
	- 1.0 micron: la distanza tra drain e source che definisce la dimensione del transistor
	- Diversi stili di design hanno colori diversi. 
	- 8 KByte sia per codice che per dati in forma di cache.
- 1989 - 1993: Pentium Processor
	- 60 - 66 MHz: si nota come a quel tempo l’obiettivo era quello di andare sempre più veloci.
	- 3.1 Mega Transistor
	- 0.8 micron
	- 8 KByte per le istruzioni e 8 KByte per i dati sotto forma di cache: si nota come hanno diviso le due cose e hanno raddoppiato la memoria, sappiamo infatti che molto spesso l’operazione più onerosa è quella di spostare i dati dalla memoria alla ram.
	- Anche qui i colori mettono in evidenza i materiali differenti e la diversa densità dei materiali
- 1995 - 1997: Pentium II Processor
	- 233 - 266 - 300 MHz
	- 7.5 Mega Transistor: più che raddoppiata rispetto al precedento
	- 0.35 micron
	- 16 KByte L1I, 16 KByte L1D, 512 KByte off-die L2: aumentata molto la memoria on chip.
- 1999: Pentium III Processor (Katmai)
	- 450 - 500 - 533 - 600 MHz:ci stiamo sempre più avvicinando al GHz
	- 9.5 Mega Transistor
	- 0.25 micron
	- 16 KByte L1I, 16 KByte L1D, 512 KByte off-chip L2
- 2002: Intel Itanium 2 Processor
	- 1 GHz
	- 221 Mega Transistor
	- 0.18 micron
	- 3 livelli di cache
	- Architettura EPIC

Ognuno di questi processori ha una **defect density**, cioé la densità di transistor difettosi per unità di area. Quindi è per questo motivo che non hanno raddoppiato l’area per porre più transistor ma le dimensioni sono rimaste ridotte. 
# Legge di Moore e Danner’s Scaling

> La **legge di Moore** sostiene che il numero di transistor per circuito integrato *raddoppia* ogni anno.

Il Denner’s Scaling è un paper che amplia la legge di Moore andando a studiare l’evoluzione delle dimensioni dei mosfet nei circuiti integrati: 
![Denner’s Scaling|center|500](https://www.researchgate.net/profile/Ojas-Parekh/publication/301879820/figure/fig24/AS:359784168607749@1462790638818/The-end-of-Dennard-Scaling-44.png)

Nel 2005 si provò ad aumentare per la prima volta il numero di Core e lo si fece per mantenere costante il consumo di energia. Prima infatti se il consumo di energia aumentava, di conseguenza cresceva anche la temperatura che diminuisce le performance del transistor. 

# Evoluzione del silicio

Inizialmente si utilizzava una struttura planare, poi si è passati ad una tecnologia FinFet che permette di raggiungere i 5nm.

Questo livello di tecnologia è stata raggiunta da poche compagnie:
- Intel 
- Samsung
- TSMC

# Interazione con l’uomo

Non dobbiamo dimenticare che questa tecnologia è fatta per essere utilizzata dall’uomo che non lavora a 5nm, quindi ci servono dei componenti il cui design è fatto a “misura d’uomo”.

Per fare ciò, si utilizza il **System in Package** (SiP) in cui internamenti si hanno più componenti interconnessi, di cui uno o più realizzati a 5nm, mentre altri di dimensioni molto più grandi. 

![SiP](https://anysilicon.com/wp-content/uploads/2022/03/System-in-Package-1024x700.jpg)

# VLSI

> **VLSI** è l’acronimo di *Very Large Scale Integration* ed è una denominazione generica che indica una elevata integrazione di transistor in un singolo chip. 

La International Technology Roadmap for Semiconductors è un gruppo di compagnie che studiano le diverse caratteristiche dei dispositivi prodotti e predicono il prossimo device. 
Questa compagnia si basa ovviamente sulla Legge di Moore. 

Non sempre sono accurati perché ad esempio nel 2001 la tecnologia è andata più avanti della predizione, mentre negli anni successivi la predizione era piuttosto accurata. 

| -                                            | 2001        | 2013        |
| -------------------------------------------- | ----------- | ----------- |
| Reticle size (mm)                            | 25 x 32     | 22 x 26     |
| Maximum die size ($mm^2$)                    | 800         | 572         |
| Feature size $F/\lambda$ ($\phi m$)          | 0.13        | 0.032       |
| Maximum die size ($\lambda ^2$)              | 47 billion  | 559 billion |
| 2-input NAND gates per die (300$\lambda ^2$) | 158 million | 1.8 billion |
| DRAM cells per die ($8F^2$)                  | 6Gb         | 70Gb            |

Bisonga considerare:
- la complessità 
- il consumo di energia
- la distribuzione di energia
- l’integrità del segale
- il tempo di fabbricazione
- i vari costi

Vediamo 3 modi per affrontare il problema della complessità:

| Design             | Pro                                                                       | Contro                                                         |
| ------------------ | ------------------------------------------------------------------------- | -------------------------------------------------------------- |
| Standard Processor | È un progessore general porpouse  cioè può soddisfare vari utenti e scopi | Le performance non sono eccezionali                            |
|                    | Grandi investimenti perché il costo può essere distribuito                | Consumo elevato di potenza                                     |
| FPGA               | Facile da programmare e riprogrammare per correggere errori               | Il consumo di energia triplica rispetto all’ASIC               |
|                    | Esistono configurazioni già programmate                                   | Non ideale per grandi consumi                                  |
| ASIC               | È una soluzione full custom                                               | Bisogna essere esperti perché è molto facile commettere errori |
|                    |                                                                           | Molto costoso                                                  |                                                                        |                                                                |                                                               |

Un po’ di acronimi che Fanucci usa: 
- **IP**: Intellectual Property, è il codice ad esempio che è di proprietà di chi lo ha sviluppato.
- **EDA**: Electronic Design Automation, software che automatizza il processo di realizzazione di un componente elettronico. 
- **CAD**: Computer Aided Design, software che aiuta il designer a sviluppare il suo prodotto. 