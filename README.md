# Raspberry-Pi-LCD-SPI-Display-2.4-ILI9341-

sudo raspi-config
Nun wechselt in das „Advanced Options“ Untermenü und wählt dort „SPI“ aus – dieses aktiviert ihr dann und beendet anschließend den Konfigurationsassistenten. Anschließend werden wir mit dem folgenden Befehl noch unser System auf den aktuellen Stand bringen (dies kann einige Minuten in Anspruch nehmen):

sudo rpi-update
Sobald das System erfolgreich aktualisiert wurde, müsst ihr das System noch mit dem folgenden Reboot-Befehl noch neu starten:

sudo reboot
Verbindet euch nun neu mit eurem Raspberry Pi, wir führen nun mit den nachfolgenden Befehlen einen ersten Funktionstest durch:

sudo modprobe fbtft_device name=tm022hdh26 rotate=90 speed=80000000 fps=60
sudo FRAMEBUFFER=/dev/fb1 startx
Nach einem kurzen Moment sollte euer Display nun die typische Raspbian Desktopoberfläche anzeigen, damit hätte unser Display den ersten Funktionstest problemlos bestanden. Unsere komplette Anleitung haben wir hier auch nochmal 


Autostart
Da wir das SPI-Display nicht jedesmal manuell über die Kommandozeile neu starten wollen, legen wir nun die entsprechende Befehle in den Autostart ab, dies funktioniert wie folgt: Zunächst müssen wir eine neue Datei im modules-load-Verzeichnis erstellen:

sudo nano /etc/modules-load.d/fbtft.conf
Dieser“fbtft.conf“-Datei geben wir folgenden Inhalt und speichern diese anschließend ab:

spi-bcm2835
fbtft_device
Nun erstellen wir noch eine weitere Datei im modprobe-Verzeichnis:

sudo nano /etc/modprobe.d/fbtft.conf
Dieser geben wir den folgenden Inhalt und speichern auch diese ab:

options fbtft_device name=tm022hdh26 rotate=90 speed=80000000 fps=60
Zuletzt müssen wir noch die bereits vorhandene rc.local Datei editieren:

sudo nano /etc/rc.local
Vor dem „exit 0“-Absatz erweitern wir die Datei mit folgendem Inhalt:

con2fbmap 1 1
sudo FRAMEBUFFER=/dev/fb1 startx
Speichert nun die gemachte Änderung und startet euren Raspberry Pi mit dem typischen Reboot-Befehl neu:

sudo reboot
Das Display sollte nun ohne zusätzliche manuelle Eingaben komplett automatisch starten.

Helligkeit regulieren
Da wir den LED-Pin unseres Displays an einen PWM-Port (Pulsweitenmodulation) unseres Raspberrys angeschlossen haben, können wir nach belieben die Helligkeit der Hintergrundbeleuchtung verändern. Dazu müsst ihr lediglich folgenden Befehl eingeben, dadurch wird das Display zunächst dunkel:

gpio -g mode 18 pwm
Um die Helligkeit auf die maximale Stufe zu setzen, gebt ihr den folgenden Befehl ein:

gpio -g pwm 18 1024
Im Bereich von 0 – 1024 könnt ihr beliebig die Helligkeit regulieren, verändert einfach den Befehl dementsprechend.
