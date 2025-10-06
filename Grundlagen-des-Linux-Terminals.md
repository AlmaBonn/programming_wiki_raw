Hier stellen wir einige Grundlagen zur Verwendung des Linux-Terminals vor.

Eine gute Übersicht zu diesem Thema gibt es auch in den Abschnitten 1-4 sowie
der Sektion `Wichtige Tipps für den Anfang` in dieser
[Anleitung](https://deployn.de/blog/linux-terminal/).

# Erste Schritte

## Ordnerstruktur
1. Öffnen Sie ein Terminal mit der Tastenkombination `Strg + Alt + T` oder über
   die Suchfunktion des Anwendungsmenüs.

2. Zunächst können Sie zur Orientierung als ersten Befehl

       ls

   in das Terminal eingeben und mit `Enter` bestätigen.
   Dieser Befehl listet alle Dateien auf, die sich im aktuellen Ordner befinden.
   Wann immer Sie auf einen Befehl stoßen, dessen Funktionsweise Sie nicht
   kennen, können Sie sich mit dem Befehl `man` eine Übersicht angeben lassen.
   In diesem Fall geben Sie also

       man ls

   ein. Unter anderem listet `man` alle Parameter auf, mit denen man `ls`
   konfigurieren kann. Zum Beispiel gibt es die Option die Dateien nach ihrem
   letzten Änderungsdatum zu sortieren. Neben `man` ist auch die help-Option
   nützlich, die Sie mit `BEFEHL --help`, also zum Beispiel `ls --help`
   aufrufen können.
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
   enden im Unterschied zu Dateinamen in einem `/`), z.B. `Downloads`.
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

   Ist es bei der Eingabe nicht eindeutig, welcher Ordner gemeint ist listet
   `Tab Tab` alle Ordner auf, die in Frage kommen (ähnlich zum `ls` Befehl).

  - Um im Terminal langes hochscrollen zu vermeiden, können Sie auch mit der
    Tastenkombination `Shift + PageUp` sukzessive durch den Terminalverlauf
    durchgehen.

## Die Manual Page
 Wir schauen uns nun die manual page von `ls` etwas genauer an, um an einem
 Beispiel zu sehen wie wir passende Optionen finden können und gegebenen
 Optionen verstehen können.

 Rufen Sie erneut die manual page von `ls` auf. Das erste Ziel ist es nun
 Optionen zu finden, die eine umgekehrte Sortierung nach Zeitstempel
 (Änderungsdatum) bietet.

 Nutzen Sie hierfür die Möglichkeit in der manual page zu suchen in dem Sie

      /by time

eingeben und mit `Enter` bestätigen. Hierbei hat `/` die Funktion in den
Suchmodus zu kommen. Wenn Sie die Option für das Sortieren nach den Zeitstempeln
gefunden haben, können Sie nun per

     /reverse

nach der Option suchen, um die Sortierungsreihenfolge umzukehren.
Probieren Sie den gefundenen Befehl einmal aus. In welcher Reihenfolge zeigt
`ls` die Resultate ohne die übergebenen Optionen an? Sie können für diese Frage
auch wieder auf `man ls` nachschauen.

Mithilfe der manual page von `ls` können Sie nun auch herausfinden was jeweils
die einzelnen Optionen in dem oben erwähnten `ls -l -a -F` bedeuten.
