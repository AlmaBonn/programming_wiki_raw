Hier stellen wir einige Grundlagen zur Verwendung des Terminals vor.

# Erste Erklärung

Ein Terminal ist das interaktive Textfenster, das man direkt nach dem Einloggen
sieht.  Möglicherweise sieht man nach dem Einloggen auch zunächst einen leeren
Desktop, dann kann man über das Kontextmenü oder ein Symbol ein Terminal
starten.  Das Terminal an sich ist ein Programm, das Eingaben (durch die
Tastatur) und Ausgaben (zum Bildschirm) eines zweiten Programms durch das
Betriebssystem leitet (und bei manchen Anwendungen auch über das Netzwerk, so
daß Computer aus der Ferne gesteuert werden können).  In unserem Fall ist das
zweite Programm eine sogenannte "Shell," meistens die `bash`.  Mit dieser
interagieren wir in der Praxis des Programmierens fast ausschließlich.

Die Shell erwartet Eingaben des Nutzers (also Ihre) über die Tastatur.  Wenn
man also ein oder mehrere Worte tippt und dann die Entertaste drückt (die große
mit dem eckigen Pfeil nach links), interpretiert die Shell das erste Wort als
Programm und die weiteren als Argumente, die diesem Programm übergeben werden
sollen.  Argumente haben oft die spezielle Form `-a` oder `--bcd` und werden
vom Programm als spezielle Schalter interpretiert.  Jedes Programm macht das
anders; idealerweise steht die Konvention auf seiner man-Page (s. u.).  Die
Shell sucht dann das Programm dieses Namens im Dateisystem und führt es aus.

 - Wenn man wissen möchte, wo im Dateisystem die Shell ein bestimmtes
   Programm findet, tippt man z. B. `which python`.

 - Ein wichtiges Programm ist zum Beispiel `man`.  Es greift auf eine Datenbank
   an Hilfen zu den meisten Programmen zu und zeigt diese an.  Das gesuchte
   Programm ist als Argument anzugeben.  Probieren Sie also einmal `man man`,
   oder `man bash`.  Man verläßt den Hilfeviewer mit der Taste `q`.

 - Ein weiteres Programm ist `echo`.  Es gibt alle Argumente auf den Bildschirm
   aus, probieren Sie `echo abc`.

 - Ein Pager zeigt eine Datei am Bildschirm an und ermöglicht das Scrollen
   und Suchen darin.  Ein beliebter Pager ist `less`, der z. B. implizit von
   `man` aufgerufen wird.  Probieren Sie einmal

       find .
       find /usr/share/doc | less

   Navigieren Sie per Tastatur und quittieren sie den Pager wieder mit `q`.

Die Shell versteht tatsächlich eine ganze Programmiersprache, da ist ein
Programmaufruf nur ein Spezialfall.  Variablen werden ohne Leerzeichen
gesetzt.  Leerzeichen können mit Anführungszeichen zum Teil eines Wortes
gemacht werden.  Ein Kommando in backtics wird ausgeführt und dessen Ergebnis
an Ort und Stelle eingesetzt.  Versuchen Sie, die Ergebnisse folgender Sitzung
zu verstehen:

    ABC="eine Variable      mit Leerzeichen"
    echo $ABC
    echo "$ABC"
    DEF=`echo $ABC`
    echo $DEF
    echo "$DEF"
    GHI=`echo "$ABC"`
    echo $GHI
    echo "$GHI"

Der Kern der Erkenntnis ist der, daß "a b   c" ein einzelnes Wort bleibt und
daher auch ein einzelnes Argument.  Die Leerzeichen zwischen Worten werden als
Trenner registriert und dann sofort vergessen.

Die Shell kann weiterhin die Ausgabe eines Programms direkt als Eingabe eines
zweiten verwenden.  Der Aufruf 'wc -w' zählt beispielsweise Worte, was macht
also:

    echo $ABC | wc -w

Dies soll nur der Anfang sein.
Es gibt sehr viele sehr gute Dokumente zur Unix-Shell.
Eine gute Übersicht zu diesem Thema gibt es auch in den Abschnitten 1-4 sowie
der Sektion *Wichtige Tipps für den Anfang* in dieser
[Anleitung](https://deployn.de/blog/linux-terminal/).

# Erste Schritte

## Ordnerstruktur

1. Öffnen Sie ein Terminal mit der Tastenkombination `Strg + Alt + T`, über ein
   Icon oder über die Suchfunktion des Anwendungsmenüs.

2. Zunächst können Sie zur Orientierung als ersten Befehl

       ls

   in das Terminal eingeben und mit Enter bestätigen.
   Dieser Befehl listet alle Dateien auf, die sich im aktuellen Ordner befinden.
   Wann immer Sie auf einen Befehl stoßen, dessen Funktionsweise Sie nicht
   kennen, können Sie sich mit dem Befehl `man` eine Übersicht angeben lassen.
   In diesem Fall geben Sie also

       man ls

   ein. Unter anderem listet `man` alle Parameter auf, mit denen man `ls`
   konfigurieren kann. Zum Beispiel gibt es die Option die Dateien nach ihrem
   letzten Änderungsdatum zu sortieren. Neben `man` ist auch die help-Option
   nützlich, die Sie mit `BEFEHL --help`, also zum Beispiel `ls --help`
   aufrufen können.  Der Buchstabe `q` schließt den Hilfeviewer.
   Eine gängige Konfiguration von `ls` sind die Optionen

       ls -l -a -F

   die oft mit

       ll

   zu einem eigenen Befehl definiert werden.
   Dieser Befehl liefert eine Liste, die neben den Dateinamen auch das Datum
   der letzten Änderung, die Dateigröße und die Zugriffsrechte anzeigt.
   Für weitere Informationen zu Zugriffsrechten verweisen wir auf
   diese [Anleitung](https://www.howtoforge.de/anleitung/was-ist-umask-unter-linux/).

   Wer mag, definiert sich eigene Befehle mit z. B.

       alias lt="ls -ltr"

   Der Befehl `alias` ist kein Programm, sondern in die bash eingebaut.  Dies
   läßt sich nachlesen, indem nach `man bash` mit `/alias` und Enter eine
   Suche gestartet wird.  Durch folgende und vorherige Fundstellen iteriert man
   mit `n` und `N`.

3. Mit dem Befehl

       pwd

   wird der absolute Pfad des Ordners ausgegeben, in welchem Sie sich
   aktuell befinden.

4. Um den Ordner zu wechseln gibt es den `cd` Befehl.
   Wählen Sie einen der Ordner, der Ihnen mit `ls` angezeigt wird (Ordner-Namen
   enden im Unterschied zu Dateinamen in einem `/`), z.B. `Downloads`.
   Um nun in den Downloads-Ordner zu wechseln kann der Befehl

       cd Downloads

   verwendet werden.
   Die geänderte Position spiegelt sich auch darin wieder, dass `pwd` nun eine
   andere Ausgabe liefert.
   Um zum Ursprungsordner zurückzukehren können Sie nun den Befehl

       cd ..

   verwenden.
   Die Punkte sind unter Unix keine Syntax sondern echte Dateien (in diesem
   Fall Verzeichnisse), `.` ist das aktuelle und `..` das nächsthöhere.

5. Erstellen Sie als nächstes einen neuen Ordner namens example-directory mit

       mkdir example-directory

   Wechseln Sie mit `cd` in den Ordner.

6. Legen Sie in dem Ordner eine neue Datei namens example-file.txt an.
   Dafür öffnen Sie diese einfach mit einem Texteditor, zum Beispiel mit

       nano example-file.txt

   oder

       vim example-file.txt

   Zum Bearbeiten verweisen wir auf die
   [Nano-Anleitung](https://www.howtoforge.de/anleitung/linux-nano-editor-fuer-anfaenger-erklaert-10-beispiele/) und die
   [vim-Anleitung](https://ieee.uni-passau.de/uploads/2014/02/vim-basics.pdf)
   sowie den Kommandozeilenbefehl `vimtutor`.
   Während `vim` der umfassendere Editor ist, ist `nano`
   einsteigerfreundlicher.  Die anfängliche Mühe beim Lernen eines Editors
   amortisiert sich bei regelmäßiger Nutzung innerhalb von Tagen.
   Wenn Sie in `vim` per Tastenkombination und auswendig ein beliebiges
   Rechteck markieren, löschen und einfügen können, haben Sie es geschafft.

   Wenn es nur um das Anzeigen von (Text-)Dateien geht, können Sie auch einen
   sogenannten Pager statt eine Editor verwenden.
   Ein solches Programm ist zum Beispiel `less`.
   Es hat den Vorteil, dass große Dateien nicht komplett in den Speicher geladen
   werden.
   Sie finden per `man less` oder in dieser
   [deutschen Erklärung](https://wiki.ubuntuusers.de/less/) mehr Informationen
   zu `less`.

7. Benennen Sie die Datei nun in file-with-a-new-name.txt um. Dafür verwenden
   Sie

       mv example-file.txt file-with-a-new-name.txt

   Mit dem gleichen Befehl können Sie auch Ordner umbenennen oder sogar an
   andere Stellen verschieben.

8. Löschen Sie die Datei mit dem Befehl

       rm file-with-a-new-name.txt

  > [!CAUTION]
  > Der im folgenden beschriebene Befehl zum Löschen von Ordnern
  > sollte nur in Kombination mit einem validen Ordner-Namen und mit Vorsicht
  > genutzt werden.
  > Der Befehl `rm -rf /` kann zum Beispiel je nach vorhandenen Berechtigungen
  > unwiderruflich das gesamte Dateisystem löschen.

   Anschließend können Sie wieder in den Ursprungsordner zurückkehren und auch
   den neu erstellten Ordner example-directory mit

       rm -r example-directory

   löschen.

9. Wenn wir Dateien suchen, können wir den Befehl `find` verwenden.
   Wir legen uns hierfür einen kleinen Übungsordner an:

       mkdir -p find-playground/{docs,logs}
       touch find-playground/docs/readme.txt \
             find-playground/docs/todo.txt \
             find-playground/logs/app.log

    wobei `-p` es uns ermöglicht das Eltern-Verzeichnis automatisch
    mitzuerstellen – nutzen Sie `mkdir --help`, um mehr über die Option `-p`
    zu erfahren.
    Außerdem ist `{docs,logs}` eine sogenannte *brace expression*.
    Sie lässt die Shell zwei Aufrufe mit `find-playground/docs` und
    `find-playground/logs` erzeugen.
    Den Befehl `touch` nutzen wir hier, um leere Dateien zu erstellen.
    Schauen Sie sich den Befehl gerne mit `man touch` genauer an.

    Nun können wir `find` in unserem Übungsordner verwenden.

   a) Alle `.txt`-Dateien unterhalb von `find-playground` auflisten:

          find find-playground -type f -name "*.txt"

      wobei `-type f` nur nötig ist, wenn Sie die Ergebnisse auf reguläre
      Dateien beschränken möchte.
      Das heißt zum Beispiele keine Verlinkungen (symlink) oder Verzeichnisse
      in den Suchresultaten zu haben.
      Wenn Sie mehr Suchresultate nicht stören oder
      Sie sich sicher sind, mit dem Suchmuster nur reguläre Dateien zu finden,
      ist die Option nicht nötig.
      In unserem Beispiel macht diese Option keinen Unterschied.

   b) Dateien der letzten 5 Minuten finden:

          find find-playground -type f -mmin -5

   c) Aufräumen:

          rm -r find-playground

   Es gibt noch deutlich mehr Optionen und Funktionalitäten von `find`.
   Hierfür können Sie sich `man find` oder auch diese
   [deutsche Erklärung](https://wiki.ubuntuusers.de/find/#Aufruf) anschauen.

## Tipps und Tricks

 - Um lange Befehle nicht mehrmals eingeben zu müssen, kann man mit den
   Pfeiltasten auch die zuletzt genutzten Befehle durchsuchen (Pfeiltaste nach
   oben für den nächstälteren und nach unten für den nächstjüngeren Befehl) und
   erneut mit Enter bestätigen.

   Für Befehle, die schon etwas länger zurückliegen, bietet sich die
   Rückwärtssuche mit `Strg + R` an.

 - Um lange Namen, zum Beispiel von Ordnern oder Dateien, nicht aufwendig und
   vor allem fehlerfrei abtippen zu müssen, kann die Autovervollständigung mit
   `Tab` verwendet werden, siehe
   [Anleitung](https://de.wikipedia.org/wiki/Befehlszeilenerg%C3%A4nzung).
   Gibt es also in einem Ordner nur den Unterordner Downloads und den
   Unterordner Programme, reicht statt der Eingabe `cd Downloads` auch die
   Eingabe `cd D` gefolgt von `Tab`, da sich der restliche Name eindeutig
   erschließt.

   Ist es bei der Eingabe nicht eindeutig, welcher Ordner gemeint ist listet
   `Tab Tab` alle Ordner auf, die in Frage kommen (ähnlich zum `ls` Befehl).

  - Um im Terminal langes hochscrollen zu vermeiden, können Sie auch mit der
    Tastenkombination `Shift + PageUp` sukzessive durch den Terminalverlauf
    durchgehen.

  - Mit der Maus kann man beliebigen Text markieren und direkt am Ort des
    Mauszeigers mit der Mitteltaste pasten.  Ein Umständliches `Ctrl-V` etc.
    ist nicht nötig, funktioniert aber mit separatem Textpuffer auch.  Wenn
    der Editor dies blockiert, kann man das freischalten, in `vim` z. B. mit
    `:set mouse=`.

## Die Manual Page

 Wir schauen uns nun die manual page von `ls` etwas genauer an, um an einem
 Beispiel zu sehen wie wir passende Optionen finden können und gegebenen
 Optionen verstehen können.

 Rufen Sie erneut die manual page von `ls` auf. Das erste Ziel ist es nun
 Optionen zu finden, die eine umgekehrte Sortierung nach Zeitstempel
 (Änderungsdatum) bietet.

 Nutzen Sie hierfür die Möglichkeit in der manual page zu suchen in dem Sie

      /by time

eingeben und mit Enter bestätigen. Hierbei hat `/` die Funktion in den
Suchmodus zu kommen. Wenn Sie die Option für das Sortieren nach den Zeitstempeln
gefunden haben, können Sie nun per

     /reverse

nach der Option suchen, um die Sortierungsreihenfolge umzukehren.
Probieren Sie den gefundenen Befehl einmal aus. In welcher Reihenfolge zeigt
`ls` die Resultate ohne die übergebenen Optionen an? Sie können für diese Frage
auch wieder auf `man ls` nachschauen.

Mithilfe der manual page von `ls` können Sie nun auch herausfinden was jeweils
die einzelnen Optionen in dem oben erwähnten `ls -l -a -F` bedeuten.
