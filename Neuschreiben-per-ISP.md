
Sollten Sie die mitgelieferte Main-Funktion oder die USB-Logik Ihres Chips
überschrieben haben, ist nicht aller Tage Abend.
Der Chip reagiert zwar nicht mehr richtig am USB-Anschluß, ist aber noch voll
funktionsfähig.  Wir müssen lediglich wieder ein Programm mit funktionierender
USB-Ansteuerung hineinschreiben.

# Reset der Chips per Hardware

Die Mikrochips verfügen über die eingebaute Funktionalität, den internen
Programmspeicher neu zu beschreiben.  Dies geschieht über elektrische
Ansteuerung nach der ISP-Methode.  Eine zweite Methode, die High-Voltage
Programmierung mit 12V, steht uns nur im Prinzip zur Verfügung, da wir dafür
fast alle 32 Beinchen des Chips verdrahten müßten, was sich dann wirklich nicht
mehr lohnt.  Die hier vorgestellte Methode ist dagegen machbar und lehrreich.

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

## Erstellen sinnvoller Programmierdaten

Vor dem Beschreiben müssen wir natürlich ein funktionierendes Programm von
einem normal funktionierenden Chip herunterbekommen, damit wir es auf den
scheintoten Chip aufspielen können.  Hierzu schalten wir in den
Arduino-Optionen "verbose" Output beim Upload an und sehen die
Konsolen-Nachrichten durch.  Dort erscheint beim Upload ein Aufruf des
Programms `avrdude` mit kompletter (sehr langer) Kommandozeile.  Diesen Aufruf
kopieren wir heraus in ein Skript, lesen die Dokumentation zu den Argumenten
von `avrdude` und ersetzen einige Argumente des Aufrufs so, daß wir das Flash
und die drei Fuses aus dem funktionierenden Chip lesen und lokal abspeichern.
Achtung auch hier, da man mit falschen Optionen an `avrdude` den Chip
*wirklich* permanent stillegen kann (Stichwort Reset-Pin).

## Beschreiben eines scheintoten Chips

Nun stecken wir den ISP-Programmer in unseren Computer ein und ändern in den
Optionen der Arduino-Umgebung den Programmer entsprechend auf das Modell des
ISP-Programmers.  Wenn man keinen ISP-Programmer kaufen möchte, sondern einen
anderen Mikrokontroller mit dem ISP-Programmier-Programm beschreibt, ist die
Option "Arduino as ISP".
Wir verbinden nun die sechs Anschlüsse des Programmers nach dem Bild auf
Wikipedia für den sechspoligen ISP-Header mit den Anschlüssen der Platine, die
wir wiederbeleben wollen.  Die Verbindung ist genau 1:1 (es werden keine
Leitungen überkreuzt):
Das genutzte Protokoll ist
[SPI](https://de.wikipedia.org/wiki/Serial_Peripheral_Interface),
wobei die *SCK*-Leitung das Taktsignal vermittelt, *MO* steht für *MOSI*, also
*Master Out Slave In*, somit wird dieses Signal direkt verbunden, denn ein Chip
hat die eine Rolle und der andere die andere.  Gleiches gilt für *MI*, kurz für
*MISO*.  Die anderen drei Leitungen verbinden Plus, Minus (*GND*) und den
Reset-Pin.

Die geänderte Programmiermethode (von USB auf ISP) führt zu anderen Argumenten
von `avrdude`.  Wir öffnen also ein simples Programm in der Arduino-Umgebung und
uploaden dieses.  Der Chip sollte jetzt bereits wieder funktionieren (und zum
Beispiel blinken, wenn es das Blink-Beispielprogramm war).  Im Konsole-Output
suchen wir wieder die `avrdude`-Kommandozeile und kopieren sie in ein Skript.

Nun werden im Skript die Kommandozeilen-Optionen von `avrdude` so geändert, daß
die vorher gesicherten lokalen Dateien in den Chip geschrieben werden.
Aufrufen, fertig.  Der Chip funktioniert jetzt wieder über USB.

Es kann sein, daß dieser letzte Schritt gar nicht nötig ist und der USB-Code
bereits im ersten Upload wieder an seinen Platz kommt.  Das würde bedeuten, das
vorherige Lesen von Flash und Fuses wäre gar nicht nötig.  YMMV.
