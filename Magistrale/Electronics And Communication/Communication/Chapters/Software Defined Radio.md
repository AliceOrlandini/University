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
$2(f_{IF} + B) \le 28.8 MHz$
$f_{IF} + B \le 14.4 MHz$
Il segnale starà tra -14.4 e +14.4 MHz. 

Mi sa che questa lezione va riascoltata, non smetterò mai di dire che l’inglese di Moretti rende la lezione particolarmente ostica. 

# Programmare un RTL-SDR con MATLAB

MATLAB è un linguaggio di programmazione orientato agli oggetti, permette infatti di creare classi per strutture dati complesse con un set di operazioni che si possono effettuare su tali strutture dati. 
Esiste ad esempio una classe che rappresenta il RTL-SDR e si installa andando nella sezione *add-ons* e, per alcuni PC (Windows più che altro), per usarla bisogna installare i driver. 

La classe si chiama *radio = comm.SDRRTLSReciver*
uno degli attributi della classe è CenterFrequency, un altro è la Band Width del segnale, per impostarlo sfrutto il SampleRate e il teorema di Nyquist. 
SDR manda l’inviluppo complesso al computer, per prendere l’FM signal bisogna campionarlo almeno alla frequenza di campionamento minima, la fm manda il segale a 50kHz signal, qual è la frequenza minima? 