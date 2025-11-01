
Sollten Sie die mitgelieferte main-Funktion oder die USB-Logik Ihres Chips
überschrieben haben, ist nicht aller Tage Abend.
Der Chip reagiert zwar nicht mehr wie erwartet am USB-Anschluß, ist aber
noch voll funktionsfähig.  Wir müssen lediglich wieder ein Programm mit
funktionierender USB-Ansteuerung hineinschreiben.

# Reset der Chips per Hardware

Die Mikrochips verfügen über die eingebaute Funktionalität, den internen
Programmspeicher (das *Flash*) neu zu beschreiben.  Dies geschieht über
elektrische Ansteuerung sechs bestimmter Kontakte nach der ISP-Methode.
Eine zweite Methode, die High-Voltage (HV) Programmierung mit 12V, steht uns
nur im Prinzip zur Verfügung, da wir dafür fast alle 32 Beinchen des Chips
verdrahten müßten, was sich dann wirklich nicht mehr lohnt.  Die hier
vorgestellte Methode ist dagegen machbar und lehrreich.

## Ohne Gewähr

Die folgenden Anweisungen sind nur beispielhaft, und Sie handeln auf eigene
Verantwortung.  Ihr Risiko ist allerdings vertretbar und soll Sie nicht davon
abhalten, hier viel auszuprobieren:  Vier Dollar (oder acht, oder zwölf...)
sind ein guter Preis für die neu erworbenen Fähigkeiten.

## Das ISP Konzept

Das Programm im Chip läßt sich durch elektrische Ansteuerung sechs bestimmter
Beinchen nach bestimmten Protokollen neu aufspielen.  Wir verwenden hierzu die
ISP-Konvention, kurz für
[In-System Programmierung](https://de.wikipedia.org/wiki/In-System-Programmierung).
Dazu brauchen wir

 - elektrischen Zugriff auf die sechs Anschlüsse von +5V bis MI auf der einen
   Seite der Platine,
 - einen ISP-Programmer (oder einen Mikrokontroller, der entsprechend
   programmiert ist),
 - eine sechspolige Verbindung zwischen den Anschlüssen des Programmers und den
   namensgleichen der Platine.

Im Prinzip koennten Sie sogar einen weiteren unserer Chips als
ISP-Programmer programmieren, um einen (oder mehrere) andere zu retten.
Am Schluss muss dieser letzte nur wieder USB-programmierbar gemacht werden.

Den elektrischen Zugriff konstruieren wir beispielsweise wie folgt.

![Board anfangs](/pics/chip_board.jpg)
![Buchsenleiste](/pics/buchsenleiste.jpg)
![Board geloetet](/pics/chip_geloetet.jpg)

## Flashen eines Chips per ISP

Wir stecken den betroffenen Chip aus und den ISP-Programmer in unseren
Computer ein.  Dann ändern wir in den Optionen der Arduino-Umgebung den
Programmer auf das entsprechende Programmer-Modell.  Wenn man keinen
ISP-Programmer kaufen möchte, sondern einen anderen Mikrokontroller mit dem
ISP-Programmier-Programm beschrieben hat, ist die Option "Arduino as ISP".
Es ist nicht schwer, im Quellcode des ISP-Programmers (als Beispiel in der
Arduino-Entwicklungsumgebung mitgeliefert) die Nummern der zu verwendenden
Beinchen anzupassen.

Wir verbinden nun die sechs Anschlüsse des Programmers mit den gleichnamigen
Anschlüssen der Platine, die wir wiederbeleben wollen.  Es bietet sich an,
hier eine Buchsenleiste auf die seitliche Sechserreihe zu löten, dann kann
man dort Drähte einstecken und nach dem Programmieren wieder abziehen.  Wenn
man ein Arduino-Board als Programmer nutzt, sucht man sich die richtigen
sechs Positionen in seinen Buchsenleisten zusammen.  Die Verdrahtung ist
logisch 1:1 (es werden keine Leitungen überkreuzt).

Das genutzte Protokoll ist
[SPI](https://de.wikipedia.org/wiki/Serial_Peripheral_Interface),
wobei die *SCK*-Leitung das Taktsignal vermittelt, *MO* steht für *MOSI*, also
*Master Out Slave In*, somit wird dieses Signal zwischen beiden Seiten
verbunden, denn ein Chip hat die eine Rolle und der andere die andere.
Gleiches gilt für *MI*, kurz für *MISO*.  Die anderen drei Leitungen
verbinden Plus (*+5V*), Minus (*GND*) und den Reset-Pin (*RST*).
Bei letzterem ist eine Besonderheit, dass der Reset-Ausgang des Programmers
*nicht* dem eigenen Reset-Eingang entspricht, sondern nach dem Reset-Eingang
des Ziel-Chips benannt ist, mit dem er verbunden wird.

![Programmer](/pics/ISP_Programmer.jpg)
![Multikabel](/pics/multikabel.jpg)
![Verbindung](/pics/chip_connect.jpg)

Nun klickt man in der Arduino-Umgebung auf *Burn Bootloader*.  Dies schreibt
über ISP eine funktionierende USB-Logik und ein Blink-Programm in den Chip.
Danach kann man den Programmer und die Verkabelung wegpacken und der Chip
funktioniert wieder am USB-Anschluß, als wäre nichts gewesen.
