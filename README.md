# bottom_ON-OFF_raspberry
Come aggiungere un pulsante on/off su Raspberry Pi
Tutti i modelli delle board Raspberry Pi non posseggono al loro interno un pulsante di accensione. Di fatto per accendere e utilizzare la board basta alimentarla, mentre  in presenza di una tastiera per spegnerla occorre impartire un comando di shutdown del sistema. Ma in tal caso la board sarà spenta ma per riaccenderla occorre togliere e ricollegare il cavo di alimentazione.

Tutto ciò costituisce un sistema non molto pratico (soprattutto per coloro i quali non possiedono una tastiera e sono costretti a sospendere da un momento all’altro l’alimentazione togliendo il cavo) e da tale pulpito gli utenti hanno trovato un escamotage per far fronte a questo grattacapo. Si tratta di aggiungere un interruttore tramite il quale sarà possibile accendere la board Raspberry Pi quando è spenta, e spegnerla quando è accesa.

Gli interruttori sono componenti molto semplici. Quando premi un bottone vengono connessi due contatti, in modo che l’elettricità possa fluire attraverso di essi. Il piccolo interruttore tattile che viene utilizzato in questa lezione ha quattro connessioni.

Al tasto andranno collegati (tramite saldatura) due fili, che andranno poi a intestarsi sui pin GPIO di Raspberry Pi. 

Vi è data la possibilità di accendere una board Raspberry Pi collegando tra loro i pin GPIO 3 e GND (pin fisici 5 e 6). Mettendo in continuità questi due piedini la board Raspberry Pi partirà con il boot. Quindi nel momento in cui i pin 5 e 6 sono messi in conduzione tramite pulsante sarà possibile accendere la board Raspberry Pi.

![alt text]([https://www.moreware.org/wp/blog/2020/07/06/come-aggiungere-un-pulsante-on-off-su-raspberry-pi/](https://i0.wp.com/www.moreware.org/wp/wp-content/uploads/2020/06/GpioPi4-1024x620-1.png?w=1024&ssl=1)

Non vi saranno inoltre reset improvvisi quando si preme erroneamente il tasto una volta che la board Raspberry Pi è accesa (tutto ciò non provoca danni eventuali alla scheda SD): infatti per spegnere il PI occorre eseguire il comando shutdown come spiegato più avanti.

Si è visto come si accendere la board. Ma come è possibile spegnere Raspberry Pi con lo stesso pulsante? Per effettuare questo step basta modificare il file /boot/config.txt . Digitiamo tale comando:

`sudo nano /boot/config.txt`

Aggiungiamo questa riga in fondo al file:

`dtoverlay=gpio-shutdown,active_low=1`

Ora premiamo CTRL+X per uscire, S per confermare e INVIO per mantenere lo stesso nome del file. Per finire, digitare quanto segue per spegnere il Raspberry Pi:

`sudo shutdown now`

Una volta che la board si è spente premere nuovamente il bottone per accendere Raspberry Pi . Una volta che il sistema si è avviato ripremere il bottone per spegnere la board.

Se tale tecnica non funziona non preoccupatevi, esiste un altro metodo per effettuare questo step.

Da una finestra di terminale digitate:

`nano /home/pi/spegnimento.py`

Il file non esiste e di conseguenza si aprirà un file vuoto nel quale saranno effettuate queste modifiche:

https://github.com/SimoneMoreWare/bottom_ON-OFF_raspberry/blob/main/spegnimento.py

Uscite con CTRL+X , poi S e INVIO.

Per verificare il coretto funzionamento apriamo lo script:

`Python /home/pi/spegnimento.py`

Ora una volta premuto il pulsante la board si spegnerà.

Lo script in questione si dovrà avviare in automatico ad ogni accensione della board. Da una finestra del terminale digitare:

sudo nano /etc/rc.local

`sudo nano /etc/rc.local`

Per modificare il file rc.local. In basso, prima della riga “exit 0“, mettete un’altra riga:

`python /home/pi/spegnimento.py &`

Ora lo script sarà avviato anche in background (la & finale serve proprio per questo scopo).

Ed ecco qui che ora potete utilizzare il proprio bottone per accendere e spegnere la board Raspberry Pi.
