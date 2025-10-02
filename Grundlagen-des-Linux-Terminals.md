Hier stellen wir einige Grundlagen zur Verwendung des Linux-Terminals vor.

Eine gute Übersicht zu diesem Thema gibt es auch in den Abschnitten 1-4 sowie
der Sektion `Wichtige Tipps für den Anfang` in dieser
[Anleitung](https://deployn.de/blog/linux-terminal/).

# Erste Schritte

## Ordnerstruktur
1. Öffnen Sie ein Terminal mit der Tastenkombination `Strg + Alt + T` oder über
   die Suchfunktion des Anwendungsmenü.

2. Zunächst können Sie zur Orientierung als ersten Befehl

       ls

   in das Terminal eingeben und mit `Enter` bestätigen.
   Dieser Befehl listet alle Dateien auf, die sich im aktuellen Ordner befinden.
   Wann immer Sie auf einen Befehl stoßen, dessen Funktionsweise sie nicht
   kennen, können Sie sich mit dem Befehl `man` eine Übersicht angeben lassen.
   In diesem Fall geben Sie also

       man ls

   ein. Unter anderem listet `man` alle Parameter auf, mit denen man `ls`
   konfigurieren kann. Zum Beispiel gibt es die Option die Datein nach ihrem
   letzten Änderungsdatum zu sortieren.
   Eine gängige Konfiguration von `ls` sind die Optionen

       ls -l -a -F

   die mit

       ll

   sogar einen eigenen Befehl erhalten hat.
   Dieser Befehl liefert eine Liste, die neben den Dateinamen auch das Datum
   der letzten Änderung, die Dateigröße und die Zugriffsrechte anzeigt.
   Für weitere Informationen zu Zugriffsrechten verweisen wir auf
   diese [Anleitung](https://www.howtoforge.de/anleitung/was-ist-umask-unter-linux/).

3. Mit dem Befehl

       pwd

   wird der absolute Pfad des Ordners ausgegeben, in welchem Sie sich
   aktuell befinden.

4. Um den Ordner zu wechseln gibt es den `cd` Befehl.
   Wählen Sie einen der Ordner, der Ihnen mit `ls` angezeigt wird (Ordner-Namen
   enden im Unterschied zu Datei-Namen in einem `/`), z.B. `Downloads`.
   Um nun in den Downloads-Ordner zu wechseln kann der Befehl

       cd Downloads

   verwendet werden.
   Die geänderte Position spiegelt sich auch darin wieder, dass `pwd` nun eine
   andere Ausgabe liefert.
   Um zum Ursprungsordner zurückzukehren können Sie nun den Befehl

       cd ..

   verwenden.

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
   Während `vim` der umfassendere Editor ist, ist `nano` einsteigerfreundlicher.

7. Benennen Sie die Datei nun in file-with-a-new-name.txt um. Dafür verwenden
   Sie

       mv example-file.txt file-with-a-new-name.txt

   Mit dem gleichen Befehl können Sie auch Ordner umbenennen oder sogar an
   andere Stellen verschieben.

8. Löschen Sie die Datei mit dem Befehl

       rm file-with-a-new-name.txt

   **Warnung: Der im folgenden beschriebene Befehl zum Löschen von Ordnern
     sollte nur in Kombination mit einem validen Ordner-Namen und mit Vorsicht
     genutzt werden.
     Der Befehl `rm -rf /` löscht zum Beispiel unwiderruflich das gesamte
     Dateisystem**

   Anschließend können Sie wieder in den Ursprungsordner zurückkehren und auch
   den neu erstellten Order example-directory mit

       rm -r example-directory

   löschen.

## Tipps und Tricks

 - Um lange Befehle nicht mehrmals eingeben zu müssen, kann man mit den
   Pfeiltasten auch die zuletzt genutzten Befehle durchsuchen (Pfeiltaste nach
   oben für den nächstälteren und nach unten für den nächstjüngeren Befehl) und
   erneut mit `Enter` bestätigen.

   Für Befehle, die schon etwas länger zurückliegen bietet sich die
   Rückwärtssuche mit `Strg + R` an.

 - Um lange Namen, zum Beispiel von Ordnern oder Dateien, nicht aufwendig und
   vor allem fehlerfrei abtippen zu müssen, kann die Autovervollständigung mit
   `Tab` verwendet werden, siehe
   [Anleitung](https://de.wikipedia.org/wiki/Befehlszeilenerg%C3%A4nzung).
   Gibt es also in einem Ordner nur den Unterordner Downloads und den
   Unterordner Programme, reicht statt der Eingabe `cd Downloads` auch die
   Eingabe `cd D` gefolgt von `Tab`, da sich der restliche Name eindeutig
   erschließt.

   Ist es bei der Eingabe nich eindeutig, welcher Ordner gemeint ist listet
   `Tab Tab` alle Ordern auf, die in Frage kommen (ähnlich zum `ls` Befehl).
