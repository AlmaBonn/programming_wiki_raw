Auf dem Mikrochip unterstützen die `printf`-Funktion und ihre Varianten
standardmäßig nicht das Ausgeben von `float` und `double` Variablen.
Die Funktion gibt ohne weitere Anpassungen auch nicht nach `Serial` aus.
Dabei ist die `printf`-Funktion sehr nützlich, wenn Sie Variablenwerte
ausgeben lassen wollen.
Wir zeigen auf dieser Wiki-Seite, wie Sie die Funktion `printf` wie in C
üblich verwenden können.

# Anpassen der Linker-Optionen

Wenn wir Code mit der Arduino IDE kompilieren, werden intern der Compiler
(siehe
auch [hier](Grundlagen-der-C-Programmierung)) und Linker aufgerufen. Der Linker
ist dafür verantwortlich, verschiedene Compiler-Ergebnisse miteinander in
dieselbe ausfuehrbare Datei zu packen.
So sorgt er beispielsweise dafür, dass wir Funktionen aus
Softwarebibliotheken aufrufen können.
Für `printf` müssen wir nun Linker-Optionen hinzufügen, um sicherzustellen,
dass eine Version der Funktion `printf` eingebunden wird, die auch
Fließkommazahlen ausgeben kann.
Dies ist nicht ab Werk so, da standardmäßig für Mikrochips aufgrund ihres
limitierten Speicherplatzs das kompilierte Programm so klein wie möglich
gehalten wird.

## Finden von `platform.txt`

Um die Linker-Optionen anzupassen, müssen wir die Datei finden, aus der die
Arduino IDE die Optionen lädt. Diese Datei heißt `platform.txt`.
Sie können die Datei finden, indem Sie in Ihrem Home-Verzeichnis nach ihr
suchen, oder Sie finden den Ort heraus, in dem Sie die ausführliche
Compiler-Ausgabe in der Arduino IDE aktivieren und aufmerksam lesen.

Das Suchen können Sie unter Linux zum Beispiel mit dem Befehl `find` (siehe
auch
[hier](Grundlagen-des-Linux-Terminals#erste-schritte)) per

       find ~ -name 'platform.txt'

realisieren. Dieser Befehl sucht in Ihrem Home-Verzeichnis nach
`platform.txt`, was je nach der Menge Ihrer Daten etwas dauern kann.

Als Alternative können Sie die ausführliche Compiler-Ausgabe in der Arduino IDE
aktivieren, indem Sie auf **File** und dann dort auf **Preferences** klicken.
In dem sich dann öffnenden Fenster können Sie bei "Show verbose output during"
den Haken vor "compile" setzen. Wenn Sie dann ein Programm kompilieren, indem
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

Zur Erklärung:  Die `-W`-Option weist den Compiler an, ihr Argument intern
an den Linker weiterzugeben.  Der Linker sieht also `-u vfprintf` und
verarbeitet danach die Programmlibrary `printf_flt`, die den übersetzten
und float-fähigen Code der Funktion `vfprintf` enthaelt.

# Beispiel mit `printf` and friends

Bitte beachten Sie, dass Sie den Serial Monitor in der Arduino IDE geöffnet haben
müssen, um die Programmausgabe sehen zu können (siehe auch
[hier](Einrichten-der-Mikrochips#erste-schritte-nach-der-installation)).

## Ein einfaches Beispiel mit `snprintf`

Die Standardfunktion `Serial.write` akzeptiert Strings (Zeichenketten),
die sie in das Konsolenfenster der Arduino-Umgebung schickt.
Wir können eine Variante von `printf` nutzen, die diesen String
beschreibt, bevor wir ihn an `Serial.write` weitergeben.

```c
#include <stdio.h> /* fuer die printf-Funktionen */
#define BUF_LENGTH 72 /* Maximal erwartete Anzahl Zeichen pro Zeile */

void run () {
  char write_buf[BUF_LENGTH];

  int i = 199;
  float a = 1.7;
  double b = 3233.8;

  snprintf (write_buf, BUF_LENGTH, "Wir haben %d, %f und %f\n",
            i, a, b);
  Serial.write (write_buf);
}
```

Hierzu einige Bemerkungen:

 - Damit der Code kompiliert, muss er mit dem
   [Beispiel für Ausgangscode](Einrichten-der-Mikrochips#beispiel-für-ausgangscode)
   kombiniert werden.
 - Die `sprintf` Funktion kopiert kommentarlos auch mehr Zeichen, als
   im Puffer reserviert sind.  Dies kann das Programm (und auch
   den ganzen Chip) abstuerzen lassen.   Daher bitte *nur* die abgesicherte
   Variante `snprintf` nutzen.
 - Wenn man `man 3 snprintf` ins Terminal tippt und durchscrollt,
   erfaehrt man sehr viel ueber die vielen Moeglichkeiten dieser
   Funktionenfamilie.  Dort wird fuer die Formatierung `%f` beschrieben,
   dass die
   Option ein `double` Argument erwartet.  In unserem Programm
   geben wir aber auch eine `float`- Variable (mit kleinerem
   Wertebereich als `double`) aus.  Der Compiler konvertiert
   in diesem Fall automatisch den schmaleren in den breiteren Typ.
 - Konvertieren Sie Variablen derselben Sorte (also entweder
   Ganz- oder Kommazahlen untereinander) *nicht* von einem
   Typ mit groesserem Wertebereich in einen mit kleinerem.
   Damit aendert man je nach Groesse den urspruenglichen Wert in einen
   anderen, zur Laufzeit unvorhersagbaren.

## Ein trickreicheres Beispiel mit `printf`

Um nun die Funktion `printf` verwenden zu können, müssen wir noch den Ort ändern
an dem `printf` seine Ausgabe liefert. Dies ist nämlich `stdout` und nicht
das auf dem Mikrochip genutzte `Serial`.
Zu diesem Zweck führen wir die Funktion `serial_putchar` in dem untenstehenden
Code ein. Es gilt wie in dem Ausgangscode
[hier](Einrichten-der-Mikrochips#beispiel-für-ausgangscode), dass Sie Ihren Code
ausschließlich in der Funktion `run` schreiben. Der einzige Unterschied ist nur,
dass wir aufgrund unserer angepassten Linker-Optionen und der Umleitung von
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
