Hier geht es nicht nur um die Sprache C sondern auch das Programmieren an sich.

Es gibt eine riesige Anzahl an Büchern und sonstigen Dokumenten zum
Programmieren, in C oder ganz allgemein.  Es ist also im Grunde eine komplette
Verschwendung, noch eins zu schreiben.  Ein kleines folgt hier trotzdem.

# Was ist Programmieren?

Programmieren bedeutet das Schreiben eines Programms in einer bestimmten
Programmiersprache und im weiteren Sinne auch das Ausführen, Testen,
Untersuchen, Dokumentieren und Verbessern des Programms.
Ein Programm ist also etwas Ausführbares.  Beispiele:

 - Ein Kochrezept im Buch wird von Menschen ausgeführt.
   Die Programmiersprache ist hier nicht klar definiert, sie muß nur von den
   meisten Menschen im gleichen Sinne verstanden werden können.
   Menschliche Sprachen erfüllen diese Anforderung im Regelfall.
 - Ein Kochrezept für eine Küchenmaschine wird von dieser ausgeführt.
   Dazu muß das Rezept ganz genau im für diese Maschine spezifizierten Format
   digital eingegeben werden.
   Meist stellt der Hersteller Hilfsprogramme bereit, die mit einem
   menschlichen Nutzer interagieren und dann intern das verlangte Format erzeugen.
   Hier wird also etwas, das der Mensch malt oder schreibt, von einem gegebenen
   Programm in eine andere Form gebracht, die von der Maschine gelesen wird.

Noch in der Nachkriegszeit waren *Computer* oder *Berechner*
Berufsbezeichnungen für Menschen, die hand- oder schreibmaschinengeschriebene
Rechenprogramme abgearbeitet und die Ausgaben protokolliert haben.  Ein
Programm ist hier schlicht eine Serie von Definitionen, Aufgaben und
Anweisungen.

Die Analogie des Kochens nach Rezept ist unserem Programmieren verblüffend
analog.  Wir betrachten die folgenden Schritte:

 - Die Zutaten liegen im Schrank oder vielleicht auch im Vorratskeller.
   Es gibt also Anweisungen, eine bestimmte Zutat dort abzuholen und auf der
   Arbeitsplatte abzulegen.
 - Zutaten können ausgepackt und in eine kleine, feste Anzahl an Schalen gelegt
   werden.
 - Neben der Arbeitsplatte liegen ein Brett, Küchenmesser und andere Werkzeuge.
   Es gibt Anweisungen, Zutaten mit diesen Werkzeugen zu bearbeiten.
   Bearbeitet werden kann alles, was in einer der Schalen liegt, und das
   Ergebnis muß auch wieder in eine solche hinein.
 - Ein Kochtopf ist sozusagen ein Subsystem, das im Zeitverlauf beheizt werden
   muß, Eingaben aus Schalen oder direkt vom Brett bekommt und Ausgaben wieder
   in Schalen übergibt.
 - Das Endprodukt wird an den Menschen als weiteres Subsystem abgegeben.

Ein sehr primitives (oder sehr modernes) Programm unterscheidet zwischen dem
großen entfernten Speicher, in dem nicht direkt gearbeitet werden kann, und
einem kleinen lokalen Speicher, auf dessen Inhalt operiert wird.  Ersterer
heißt Hauptspeicher, letzter heißt Register.  Ein Programmteil sieht
beispielsweise so aus:

    mov Keller_Apfel_3 Register_Schale_1
    chop Register_Schale_1 5
    call Kochtopf (180, 10, Register_Schale_1, Register_Schale_2)

Hier ist der Parameter '5', die gewünschte Anzahl Apfelstücke, im Programm
*hardgecodet*.  Es gibt andere Anweisungen, die diesen Parameter aus einem
Notizzettel lesen, den man vorher aus dem Kochbuch herausgeschrieben hat.
Solche Notizen würden wir auch als Register bezeichnen.
Alle Ablageorte werden direkt mit Ihrer Adresse (im Keller oder auf dem
Tisch) bezeichnet, Variablennamen gibt es nicht.

Der Kochtopf ist ein Unterprogramm, das der Nutzer entweder selbst geschrieben
hat oder das schon mitgeliefert war.  Es ist hier nicht nötig zu wissen, wie
es funktioniert, nur die Parameter müssen dokumentiert sein: die Temperatur,
Kochdauer, und wohin das Ergebnis gespeichert werden soll.  Eine
Programmiersprache wie hier beschrieben heißt sinnigerweise Assembler.

# C für die AVR-Architektur

Eine Hochsprache wie C nimmt dem Programmierer eine ganze Menge Arbeit ab, zum
Beispiel versteckt sie komplett die Anzahl, Namen und Art der Register.  Es
werden lediglich bestimmte Typen von Variablen definiert, die beliebig benannt
werden dürfen, und auf denen direkt operiert wird.  Wichtig für uns sind:

 - Ein `int` speichert Ganzzahlen mit beliebigem Vorzeichen.
   Der Wertebereich ist hardwareabhängig.
   Die AVR-Architektur verwendet hier zwei Bytes, also 16 Bits für Werte von
   -32768 bis 32767.
 - Ein `unsigned int` hat die gleiche Speicherbreite wie der `int`, aber das
   Vorzeichenbit wird nicht benötigt:  Wertebereich 0 bis 65535.
 - `char` ist eine Ganzzahl mit nur einem Byte Speicher und Vorzeichen, der
   Wertebereich ist -128 bis 127.
 - `unsigned char` ist die nicht-negative Variante von einem Byte Speicher,
   der Wertebereich ist also 0 bis 255.
 - `float` ist eine Fließkommazahl mit 4 Bytes oder 32 Bits Speicherbreite.
   In diesen werden eine Ganzzahl und ein Exponent zur Basis 2 untergebracht.

Wir können also eine ganze Menge an Zahlen darstellen.  Buchstaben werden nach
dem 7-Bit
[ASCII-Code](https://de.wikipedia.org/wiki/American_Standard_Code_for_Information_Interchange)
als Zahlen abgespeichert, und ein Programmierer muß sich merken, welche
`char`-Variablen tatsächlich Zahlen speichern und welche Buchstaben kodieren.
Wenn man in C Zeichenketten in doppelten (!) Anführungszeichen definiert, wird
jeder Buchstabe automatisch in seine Zahl umgesetzt.  Sonderzeichen erzeugt man
durch sogenannte Escape-Sequenzen, wie zum Beispiel `\n` für eine neue Zeile.
Zeichen wie Umlaute, Smileys etc. werden kodiert als Obermenge von 7-Bit ASCII
über
[UTF-8](https://de.wikipedia.org/wiki/Unicode)
durch Sequenzen von bis zu vier Bytes.

Es empfiehlt sich, immer den kleinstmöglichen Wertetyp zu wählen, und immer
`unsigned` sofern die Zahlen nicht negativ werden können oder sollen.
Speicher ist knapp und vor allem langsam und sollte daher nicht verschwendet
werden.

Variablen müssen vor der ersten Verwendung mit ihrem Typ deklariert werden.
Das Zuweisen von Speicherplatz heißt dagegen ihre Definition.
In der Praxis wird oft beides in einer Anweisung erledigt.
Die unkomplizierteste Form ist diese:

    unsigned int i = 5;

Die Sprache C kennzeichnet sich durch eine große Vielfalt an Operatoren aus.
Diese Operatoren haben alle einen Rückgabewert.
Dieser kann einer weiteren Variablen zugewiesen oder auch vergessen werden.
Beispielsweise gibt es die folgenden, vorausgesetzt a und b sind definiert:

 - `a ++` addiert 1 zur Variablen a, Rückgabewert wird vergessen.
 - `b = a ++` addiert 1 zu a und weist den vorherigen Wert b zu.
   Gemäß der Sprachgrammatik bindet `++` stärker als `=` und greift zuerst.
   Der Rückgabewert der Zuweisung `=` wird vergessen.
 - `c = b = ++ a` addiert 1 zu a und weist das Ergebnis b und auch c zu.
   Der Rückgabewert der Zuweisung `=` ganz links wird vergessen.

Um mehrere Operatoren in derselben Anweisung zu verwenden, muß ihre Präzedenz
und Assoziativität definiert werden.  Dies kann man
[in dieser Tabelle](https://www.eecs.northwestern.edu/~wkliao/op-prec.htm)
nachlesen.  Der Operator `=` assoziiert beispielsweise von rechts nach links,
also weist

    a = b = c

erst b den Wert von c zu und dann a den gerade berechneten Wert von (b = c),
also b nach der Operation, gleich c sowohl vorher als auch nachher.
Andersherum assoziieren die Grundrechenarten von links nach rechts, daher
berechnet

    a + b - 9

erst a + b und subtrahiert dann 9 vom Ergebnis.  Hier werden a und b nicht
verändert.

Logische Operatoren betrachten 0 als falsch und alles andere als wahr.
Das Ergebnis ist definierterweise entweder 0 oder 1.  Die Ausdrücke

    a = 5, (a == 5 || a == 6)
    a = 5, a || a == 6

ergeben also beide als Wert 1 (bitte den Komma-Operator nachschlagen) und
weisen als Nebeneffekt vorher a den Wert 5 zu.
Aus der Präzedenz der Operatoren ergibt sich, daß die runden Klammern im ersten
Beispiel überflüssig sind.

# Funktionen

Eine Funktion ist in C auch nur eine Variable, die aber zusätzlich ausgeführt
werden kann.  Jede Funktion hat einen Rückgabewert mit Typ, wenn er
grundsätzlich vergessen werden soll, ist dieser Typ `void`.  Die Argumentliste
enthält entweder nur `void` oder eine oder mehrere Parameternamen mit Typ.
Zwei vollständige Funktionsdeklarationen sind zum Beispiel

    void machnichts (void) {}
    int summe (int a, int b) { return a + b; }

Wir können eine zweite Variable mit der Funktion `machnichts` belegen, dabei
wird nicht der Körper dupliziert sondern nur ihre Adresse.  Rückgabewert und
Übergabeparameter müssen übereinstimmen, sind also Teil des Typs dieser
Funktionen.

    void (*machkopie) (void) = machnichts;

Wir sehen hier ein Semikolon, dieses macht aus dem Ausdruck davor (mit
vergessenem Rückgabewert der Zuweisung `=`) eine Anweisung (einen Schritt im
Programm), die keinen Wert hat.

Globale Variablen werden außerhalb einer Funktion definiert und sind überall im
Programm sichtbar, lokale Variablen sind innerhalb eines `{ ... }` Blocks
definiert, also zum Beispiel eines Funktionskörpers.
So wird mit jedem Aufruf der beiden folgenden Funktionen eine bestimmte globale
Variable verändert:

    int a = 6;
    void addtoa (int was) { a += was; }
    void addloop (int eingabe) {
        unsigned char i;
        int hilfswert = eingabe + 4;
        for (i = 0; i < 3; ++i) { a += hilfswert; hilfswert *= eingabe; }
        /* die zweite Multiplikation im letzten Durchlauf ist unnuetz! */
    }

Wichtig ist hier, daß keine dieser Funktionen bereits aufgerufen wird!
Dies geschieht direkt oder indirekt aus dem Hauptprogramm.
Die Arduino-Umgebung gibt das Hauptprogramm vor und erwartet, daß sämtliche
Aufrufe des Anwenders in der `setup()`- oder `loop()`-Funktion geschehen.
Der Standard-C-Compiler der Kommandozeile hingegen erwartet alle Aufrufe des
Anwenders innerhalb einer Main-Funktion, die noch geschrieben werden muß:

    #include <stdlib.h>
    int main (void) {
        addtoa (5);
        return EXIT_SUCCESS;
    }

Nochmal, in einem Arduino-Programm darf die main-Funktion *auf keinen Fall*
geschrieben werden, da sie von der Entwicklungsumgebung vordefiniert wird.
Die sogenannte Include-Direktive bindet übrigens den Inhalt der sogenannten
Header-Datei `stdlib.h` ein, sonst wäre `EXIT_SUCCESS` nicht definiert.

Es gibt noch einige Feinheiten, die relevant werden, wenn ein Programm in
Funktionen zerlegt wird, die in mehreren verschiedenen Code-Dateien stehen.
Eine Funktion oder Variable kann `static` sein, wenn sie nur in ihrer eigenen
Datei aufrufbar sein soll.  Dies ist eine sinnvolle Standardannahme.
Eine lokale Variable, die static deklariert wird, behält ihren Wert über
mehrere Aufrufe der Funktion hinweg - ein Feature, das sich klug und unklug
einsetzen läßt und das wir deswegen normalerweise nicht nutzen.

# Compilieren und Ausführen

Die Sprache C wurde vor über fünfzig Jahren menschendefiniert, in Unkenntnis
künftiger Hardware und daher bewußt auch unabhängig von damaliger Hardware.
Für jede gegebene Hardware ist daher ein Mechanismus notwendig, um ein
C-Programm in den passenden Assembly-Code zu übersetzen und diesen dann in eine
äquivalente Binärdarstellung umzuformatieren, die das Betriebssystem dieser
Hardware in den Programmspeicher des Prozessors laden kann, so daß dieser es
lesen und direkt ausführen kann.  Dieser Mechanismus wird Compilieren genannt
und von einem Programm, dem Compiler, durchgeführt, der notwendigerweise sowohl
Hardware- als auch betriebssystemabhängig arbeitet.

Wir schreiben nun ein Beispielprogramm in eine Datei first_compile.c:

    #include <stdio.h>
    #include <stdlib.h>
    static int verdoppeln (int a) { return 2 * a; }
    int main (void) {
        int wert = 5;
        wert = verdoppeln (wert);
        wert = verdoppeln (3 * wert);
        printf ("Der Wert ist am Ende %d\n", wert);
        return EXIT_SUCCESS;
    }

Die printf-Anweisung ersetzt die %-Codes der Reihe nach mit dem formatierten
Wert der folgenden Parameter.  Näheres steht unter `man 3 printf`.

Diese Datei übersetzen wir in Assembler mit

    gcc -Wall -S first_compile.c

Der Schalter `-Wall` aktiviert sehr sinnvolle Warnungen.
Das Ergebnis ist eine Datei `first_compile.s`, in der wir vielleicht nicht
allzuviel erkennen aber immerhin unsere beiden Funktionsnamen.  Wir können
zumindest erahnen, daß im Programm gar nicht multipliziert wird, sondern
verdoppelt wird durch Addition zu sich selbst, und mal drei genommen durch
Addieren dieses Ergebnisses.
(Was passiert aber, wenn wir die 2 und 3 durch andere Zahlen ersetzen?)

Den Assembly-Code können wir jetzt in eine ausführbare Datei wandeln mit

    gcc -Wall -o first_compile first_compile.s

Der Name der neuen ausführbaren Datei wird nach dem `-o` Argument angegeben.
Diese können wir aufrufen mit

    ./first_compile

Hier erinnern wir uns daran, daß `.` das aktuelle Verzeichnis ist, über das die
Shell die ausführbare Datei lokalisiert, und freuen uns über eine Ausgabe des
berechneten Werts von 60.

Wir können den Weg über die Assembly-Datei natürlich abkürzen und direkt
übersetzen mit

    gcc -Wall -o first_compile first_compile.c
