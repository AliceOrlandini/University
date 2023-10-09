Guglielmo Marconi
Da analogico si è passato a digitale.
L’idea del Software Defined Radio è quello di processare un segnale digitale e farci tutto ciò che ci si deve fare via software. 

È nato in ambiente militare, all’inizio ogni sistema aveva una radio a se mentre ora le cose sono cambiate centralizzando via software.
Esempio: nel telefono c’è il GPS, FM radio, 5G, 4G, Bluetooth, Wi-Fi e chissà quanti altri.
Il campionamento la fa un’entità sola. 

Origini storiche.

![Servizi di telecomunicazione in base alla banda|center|500](https://digitalregulation.org/wp-content/uploads/word-image-141.png)

Ci interessano 3 parti: 
1. Analog processing of the signal 
2. Clock, oscilla a 28 MHz: *Clock Crystal 28.8MHz*: oscillatore
3. Baseband processing unit: *RTL2832U COFDM Demodulator*

Un sistema SDR si compone dei seguenti blocchi: 

![SDR diagramma a blocchi](https://www.researchgate.net/profile/Stephen-Ugwuanyi/publication/328164022/figure/fig1/AS:701214731796481@1544194028671/Simple-SDR-Architecture.ppm)

RF part of the reciver,

Da design si ha che $2f_{IF} + B \le f_s$ con $f_s$ la sampling frequency. 
28.8MHz è la sampling frequency.
$f_{IF} + B \le 28’800 Hz$