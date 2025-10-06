Auf dem Mikrochip wird standarmäßig die `printf`-Funktion nicht unterstützt.
Diese Funktion ist sehr nützlich, wenn Sie Variablenwerte in einem String
ausgeben lassen wollen. Daher zeigen wir auf dieser Wiki-Seite wie Sie leicht
eine Softwarebibliothek hinzufügen, um die Funktion `printf` wie in C
üblich verwenden zu können.

# Installation von LibPrintf

Wir können die Softwarebibliothek LibPrintf installieren in dem wir in der
Arduino IDE in dem Menü ganz oben auf **Sketch** klicken und dann in dem sich
öffnenden Menü **Inlcude Libraries** und dort wiederum **Manage Libraries...**
anklicken.
In dem sich dann öffenden Library Manager können Sie in die Suche LibPrintf
eingeben und dort auf **INSTALL** klicken.
Eine allgemeine Erklärung zum Softwarebibliotheken in der Arduino IDE finden
Sie
[hier](https://docs.arduino.cc/software/ide-v1/tutorials/installing-libraries/).

# Einbinden von Printf
Sobald wir LibPrintf installiert haben, können wir die Softwarebibliothek
einbinden in dem wir wieder auf **Sketch** klicken und dann in dem Menü unter
**Include Library** LibPrintf auswählen. Hierdurch sollte dann automatisch am
Anfang der gerade geöffneten Datei `#include <LibPrintf.h>` eingefügt werden.

# Ein Beispiel mit LibPrintf
Nachdem wir LibPrintf installiert und eingebunden haben, können wir nun unser
[Beispiel](Einrichten-der-Mikrochips#beispiel-für-ausgangscode) anpassen und
erhalten beispielsweise den folgenden Code, der eine Approximation an Pi
ausgibt.

```c
#include <LibPrintf.h>

void run () {
  /* Schreiben Sie hier Ihren Code. Schreiben Sie keine main-Funktion! */
  printf ("Approximated pi: %lf\n", 3.14159265);
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
