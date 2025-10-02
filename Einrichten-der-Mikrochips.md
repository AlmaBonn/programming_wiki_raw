# Installation der Arduino IDE 2.0

Folgen Sie den Anweisungen für Linux, Windows oder macOS auf der
[Arduino IDE 2 Download-Seite](https://docs.arduino.cc/software/ide-v2/tutorials/getting-started/ide-v2-downloading-and-installing/).

# Erste Schritte nach der Installation

1. Verbinden Sie den Mikrochip über einen USB-Port mit Ihrem Gerät.

2. In der Arduino IDE drücken Sie auf `Select Board`.
   Dort sollte nun `Arduino Leonardo` zur Auswahl stehen.
   Wählen Sie diese Option aus.

3. Öffnen Sie nun
   `File > Examples > 01.Basics > Blink`
   für ein Testprogramm.

4. Drücken Sie auf `Upload`, um das Programm zu kompilieren und auf den Arduino
   zu laden.
   Die Lampe an dem Mikrochip sollte nun in 1-Sekunden-Abständen blinken.
   Ist dies nicht der Fall, bietet eine eventuelle Fehlermeldungen im
   Output-Fenster einen Ansatzpunkt zur Fehlerbehebung.

# Beispiel Ausgangscode
Wenn Sie Code schreiben, implementieren Sie diesen in der `run`-Funktion in
dem untenstehenden Code. Sie dürfen natürlich gerne in der Funktion `run`
weitere von Ihnen implementierten Funktion aufrufen, aber implementieren Sie
unter keinen Umständen eine eigene `main`-Funktion!

```c
void run () {
  /* Schreiben Sie hier Ihren Code. Schreiben Sie keine main-Funktion! */
  Serial.println ("Hello World!");
}

/* Der Code in der setup-Funktion wird genau einmal ausgeführt. */
void setup () {
  /* Wir warten bis die USB-Verbindung besteht. */
  while (!Serial) { /* warten */ }
  /* Intialisiere Konsole mit der Datenrate von 9600 Bits pro Sekunde. */
  Serial.begin (9600);
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

# Probleme und Lösungen
Probleme beim Einrichten der Mikrochips und der IDE sowie passende Lösungen
sammeln wir in der folgenden Liste.

-
