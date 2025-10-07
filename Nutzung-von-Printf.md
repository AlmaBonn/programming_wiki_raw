Auf dem Mikrochip wird standardmäßig die `printf`-Funktion nicht unterstützt.
Diese Funktion ist sehr nützlich, wenn Sie Variablenwerte in einem String
ausgeben lassen wollen. Daher zeigen wir auf dieser Wiki-Seite wie Sie die
Funktion `printf` wie in C üblich verwenden zu können.

# Anpassen der Linking-Optionen

Wenn wir Code mit der Arduino IDE kompilieren wird intern der Compiler (siehe
auch [hier](Grundlagen-der-C-Programmierung)) und Linker aufgerufen. Der Linker
ist dafür verantwortlich verschiedene Compiler-Ergebnisse miteinander zu
*verlinken*. So sorgt er beispielsweise dafür, dass wir Funktionen aus
Softwarebibliotheken aufrufen können.
Für `printf` müssen wir nun Linking-Befehle hinzufügen, um sicherzustellen, dass
die Funktion `printf` funktioniert. Dies ist nötig, da standardmäßig für
Mikrochips aufgrund ihres limitierten Speicherplatzs das kompilierte
Programm so klein wie möglich gehalten wird.

## Finden von `platform.txt`

Um die Linking-Optionen anzupassen, müssen wir die Datei finden aus der die
Arduino IDE die Optionen lädt. Diese Datei heißt `platform.txt`.
Sie können die Datei finden in dem Sie in Ihrem Home-Verzeichnis nach ihr suchen
oder Sie finden den Ort heraus in dem Sie die ausführliche Compiler-Ausgabe
in der Arduino IDE aktivieren.

Das Suchen können Sie unter Linux zum Beispiel mit dem Befehl `find` (siehe auch
[hier](Grundlagen-des-Linux-Terminals#erste-schritte)) per

       find ~ -type f -name 'platform.txt'

realisieren. Dieser Befehl sucht in Ihrem Home-Verzeichnis nach Dateien mit dem
Namen `platform.txt`.

Als Alternative können Sie die ausführliche Compiler-Ausgabe in der Arduino IDE
aktivieren in dem Sie auf **File** und dann dort auf **Preferences** klicken.
In dem sich dann öffnenden Fenster können Sie bei "Show verbose output during"
den Haken vor "compile" setzen. Wenn Sie dann ein Programm kompilieren in dem
Sie auf "Verify" oder "Upload" klicken, finden Sie nun unter Output deutlich
mehr Informationen. Suchen Sie dort
`Using board 'leonardo' from platform in folder:`, was für gewöhnlich bereits
in der zweiten Zeile der Ausgabe stehen sollte.
Das dort angegebene Verzeichnis sollte `platform.txt` enthalten.

## Ändern von `platform.txt`

Nachdem wir die Datei `platform.txt` gefunden haben, öffnen wir sie mit einem
Text-Editor unserer Wahl (siehe auch
[Grundlagen des Linux Terminals](Grundlagen-des-Linux-Terminals#erste-schritte)).
Dort fügen wir bei `compiler.c.elf.flags` vor den bereits vorhandenen Optionen
`-Wl,-gc-sections` die neuen Optionen

       -Wl,-u,vfprintf -lprintf_flt -lm

ein und speichern die Datei.

# Ein Beispiel mit `printf`

Um nun die Funktion `printf` verwenden zu können, müssen wir noch den Ort ändern
an dem `printf` seine Ausgabe liefert. Dies ist nämlich `stdout` und nicht
das auf dem Mikrochip genutzte `Serial`.
Zu diesem Zweck führen wir die Funktion `serial_putchar` in dem untenstehenden
Code ein. Es gilt wie in dem Ausgangscode
[hier](Einrichten-der-Mikrochips#beispiel-für-ausgangscode), dass Sie Ihren Code
ausschließlich in der Funktion `run` schreiben. Der einzige Unterschied ist nur,
dass wir aufgrund unserer angepassten Linking-Optionen und der Umleitung von
`stdout` nach `Serial` in der Funktion `setup`, nun auch `printf` verwenden
können.

```c
#include <stdio.h> /* für FILE und fdev_setup_stream, um stdout umzuleiten */

void run () {
  /* Schreiben Sie hier Ihren Code. Schreiben Sie keine main-Funktion! */
  printf ("Approximated pi: %lf\n", 3.14159265);
}

/* Die Funktion wird verwendet um stdout zu Serial umzuleiten. */
static int serial_putchar (char c, FILE *stream) {
  /* Wandle '\n' in '\r\n' zwecks Portierbarkeit um. */
  if (c == '\n') {
    Serial.write('\r');
  }
  Serial.write(c);
  return 0;
}

/* unser neuer Output-Stream */
static FILE serial_stdout;

/* Der Code in der setup-Funktion wird genau einmal ausgeführt. */
void setup () {
  /* Initialisiere Konsole mit der Datenrate von 9600 Bits pro Sekunde. */
  Serial.begin (9600);
  /* Wir warten bis die USB-Verbindung besteht. */
  while (!Serial) { /* warten */ }

  /* Setze unsere eigene putchar-Funktion auf die neue stdout-Variable. */
  fdev_setup_stream (&serial_stdout, serial_putchar, NULL, _FDEV_SETUP_WRITE);
  /* Setze den stdout-Zeiger auf die Adresse des neuen Streams. */
  stdout = &serial_stdout;

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
