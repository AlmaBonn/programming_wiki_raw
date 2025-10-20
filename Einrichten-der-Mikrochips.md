# Installation der Arduino IDE 2.0

Fuer Ihre Privatrechner folgen Sie den Anweisungen für Linux, Windows oder
macOS auf der
[Arduino IDE 2 Download-Seite](https://docs.arduino.cc/software/ide-v2/tutorials/getting-started/ide-v2-downloading-and-installing/).

Während der Installation auf Windows werden Sie gefragt, ob Sie verschiedene
Programme in Bezug zum USB-Port installieren moechten. Diese Programme sind
notwendig, damit der Mikrochip von der Arduino IDE erkannt und bearbeitet
werden kann.

Auf Linux müssen Sie wahrscheinlich die Datei
`/etc/udev/rules.d/99-arduino.rules` neu anlegen. Hierfür benötigen Sie für
gewöhnlich Admin-Rechte. Zum Beispiel können Sie

       sudo vim /etc/udev/rules.d/99-arduino.rules

aufrufen und in die Datei wenn nicht vorhanden die Zeile

       SUBSYSTEMS=="usb", ATTRS{idVendor}=="2341", GROUP="plugdev", MODE="0666"

einfügen.  Sie können natürlich Modifikationen vornehmen, z. B. den Mode auf
`0660` setzen, wenn Ihr Nutzer in der `plugdev`-Systemgruppe enthalten ist.

Wenn es Probleme mit der seriellen Schnittstelle über den USB-Anschluß gibt,
kann es helfen, das Linux-Paket `modemmanager` zu deinstallieren.

# Erste Schritte nach der Installation

1. Verbinden Sie den Mikrochip über einen USB-Port mit Ihrem Gerät.
   Er wird in einen USB-A-Port eingesteckt!  Es ist kein Kabel nötig.
   Falls Ihr Geraet nur USB-C-Ports besitzt, können Sie ein
   Adapterkabel oder einen USB-Hub zwischenschalten.

2. In der Arduino IDE drücken Sie auf `Select Board`.
   Dort sollte nun `Arduino Leonardo` zur Auswahl stehen.
   Je nach Chip-Charge und Betriebssytem kann auch `Arduino Micro` angezeigt
   werden.
   Wählen Sie die Option entsprechend aus.

3. Öffnen Sie nun
   `File > Examples > 01.Basics > Blink`
   für ein Testprogramm.

4. Drücken Sie auf `Upload`, um das Programm zu kompilieren und auf den Arduino
   zu laden.
   Die Lampe an dem Mikrochip sollte nun in 1-Sekunden-Abständen blinken.
   Ist dies nicht der Fall, bietet eine eventuelle Fehlermeldungen im
   Output-Fenster einen Ansatzpunkt zur Fehlerbehebung.

5. Drücken Sie nun auf Serial Monitor. Im unteren Fenster sollte sich nun ein
   weiteres Fenster mit der Überschrift Serial Monitor geöffnet haben.
   In diesem Fenster können Sie dann Ausgaben von Ihren Programmen sehen,
   falls Ihr Programm etwas ausgibt (siehe z. B.
   [hier](Nutzung-von-Printf#ein-trickreicheres-beispiel-mit-printf)).

## Verwenden eines eigenen Editors

Der eingebaute Editor in der Arduino-IDE ("Integrated Development Environment":
Entwicklungsumgebung) funktioniert wie auf Windows üblich, was zunächst einmal
das Einfachste ist.  Verglichen mit den meisten Unix-Editoren fehlen ihm jedoch
(unserer Meinung nach) viele Features für effizientes, professionelles und
angenehmes Arbeiten.  Eine Alterantive ist zum Beispiel `vim` (siehe auch
[hier](Grundlagen-des-Linux-Terminals#erste-schritte)).

Daher öffnen Sie die Programmdatei gerne parallel mit Ihrem Lieblings-Editor.
Wann immer Sie abspeichern, registriert die IDE das und lädt den Quelltext neu.

# Beispiel für Ausgangscode

Wenn Sie Code schreiben, implementieren Sie diesen in der `run`-Funktion in
dem untenstehenden Code. Sie dürfen natürlich gerne in der Funktion `run`
weitere von Ihnen implementierte Funktionen aufrufen, aber implementieren Sie
unter keinen Umständen eine eigene `main`-Funktion!
> [!CAUTION]
> Bitte beachten Sie auch unsere Hinweise auf dieser
> [Seite](Wichtige-Hinweise-zum-Mikrochip).

```c
void run () {
  /* Schreiben Sie hier Ihren Code. Schreiben Sie keine main-Funktion! */
  Serial.println ("Hello World!");
}

/* Der Code in der setup-Funktion wird genau einmal ausgeführt. */
void setup () {
  /* Initialisiere Konsole mit der Datenrate von 9600 Bits pro Sekunde. */
  Serial.begin (9600);
  /* Wir warten bis die USB-Verbindung besteht. */
  while (!Serial) { /* warten */ }
  run ();
}

/* Der Code in der loop-Funktion wird wiederholt ausgeführt. */
void loop () {
  /* Wir führen nichts in dieser Funktion aus.
   * Wenn wir Code wiederholt ausführen möchten, verwenden wir
   * eine Schleife (for oder while) in der run-Funktion.
   */
   return;
}
```

Um die in C übliche `printf`-Funktion zu verwenden können Sie dieser
[Anleitung](Nutzung-von-Printf) folgen.

# Probleme und Lösungen
Probleme beim Einrichten der Mikrochips und der IDE sowie passende Lösungen
sammeln wir in der folgenden Liste.  Bitte melden Sie uns alles, was Sie
finden!

-
