Für das Algorithmische Mathematik I Wiki verwenden wir Github.
Um zum Wiki beizutragen, muss einmalig das Setup durchgeführt werden, bevor man zu einzelnen Beiträgen übergehen kann.

# Setup

Ein git Repository ist eine identisch replizierbare Sammlung von Inhalten
mit Historie.  Die Historie ist durch kryptographische Hash-Funktionen
zurückzuverfolgen und nicht fälschbar.  Jede Kopie eines Repositories ist
vollständig, und welches Repository momentan offiziell ist, ist lediglich
eine soziale Konvention.

1. Erstellen Sie einen kostenlosen [Github Account](https://github.com/signup).

2. Zum Bearbeiten des Wiki-Inhalts gibt es ein separates Repository.
   Erstellen Sie von diesem Repository einen eigenen [Fork](https://github.com/AlmaBonn/programming_wiki_raw/fork).
   Falls nicht bereits automatisch geschehen, tragen Sie als Owner den eigenen Github-Account ein.

# Einen Beitrag erstellen

Es gibt verschiedene Wege, die Inhalte zu bearbeiten.
Sie arbeiten in der Regel an einer eigenen Kopie des Repositories und
beantragen dann per Web-Interface, daß Ihre Änderungen in das offizielle
Repository übernommen werden.  Änderungen am offiziellen Repository (z. B.
durch andere) pflegen Sie regelmäßig in ihrer eigenen Kopie ein.

## Bearbeiten per Web-Interface

1. Melden Sie sich bei Github an und öffnen Sie Ihren Fork des Daten-Repository.

2. Drücken Sie auf `Sync fork`, um sicherzustellen, dass Sie mit der neuesten
   Version des Wikis arbeiten.

3. Klicken Sie auf den Namen der Datei, welche Sie bearbeiten wollen.

4. Klicken Sie in der Ansicht der Datei auf das `Edit`-Symbol (Stift, oben rechts).

5. Bearbeiten Sie die Datei wie gewünscht.
   Alle Dateien sind im Markdown-Format geschrieben, welches grundlegende
   Datei-Formatierungen ermöglicht.
   Um einen Ausblick auf das formatierte Ergebnis zu bekommen kann zwischen der
   `Edit`- und der `Preview`-Ansicht gewechselt werden.

6. Sobald Sie mit den Änderungen zufrieden sind klicken Sie auf
   `Commit changes...`.

7. Tragen Sie zunächst eine Commit-Message ein, die die vorgenommenen Änderungen
   knapp und präzise zusammenfasst (nicht mehr als ~10 Worte).

8. Drücken Sie auf `Commit changes`.

9. Drücken Sie auf `Code` um zurück zur Übersicht über das Repository zu
   gelangen.

10. Drücken Sie auf `Contribute` und anschließend auf `Open pull request`.

11. Es öffnet sich ein neues Fenster mit der Überschrift `Open a pull request`.
   Tragen Sie dort einen passenden Titel ein, zum Beispiel den Inhalt der zuvor
   gewählten Commit-Message. Falls nötig können Sie Ihre Änderungen auch im
   `Description`-Feld ausführlicher beschreiben.
12. Drücken Sie schließlich auf `Create pull request`.
    Ihre vorgeschlagenen Änderungen sind nun zum Review bei den
    Vorlesungs-Assistenten eingereicht. Falls Rückfragen aufkommen, werden Sie
    über Ihre bei Github hinterlegte E-Mail-Adresse benachrichtigt. Diese können
    Sie dann direkt per Mail oder über den enthaltenen Github-Link beantworten.
    Sobald alle Fragen geklärt sind werden die Änderungen bald in das Wiki
    eingebunden.

## Bearbeiten im eigenen Dateisystem

Sie können von unserem offiziellen Repository auf
github per Kommandozeile eine lokale Kopie erstellen und auch die Inhalte
Ihres Forks hinzuziehen.  Siehe hierzu `man git-clone` und `mat git-fetch`.
In der Verzeichnisstruktur liegen verschiedene Markdown-Dateien mit Endung
`.md`.  Diese editieren Sie mit einem Editor Ihrer Wahl und erstellen so
eine lokale git-Historie in einer sinnvoll benannten branch (z.  B.
update-wiki-beitragen).  Diese laden Sie dann auf Ihren Fork bei github hoch
(siehe `man git-push`), worauf das Web-Interface Ihnen die Möglichkeit
anbietet, einen Pull-Request zu erstellen wie oben.

Eine Vorschau in `html` generieren Sie z. B. per

    pandoc Zum-Wiki-beitragen.md -o test.htm
    firefox test.htm &

Damit von `git status` nicht die generierte html-Datei angezeigt wird, und
damit Sie diese nicht aus Versehen ins Repository hinzufügen, empfiehlt es
sich, in der Datei in Ihrem Homeverzeichnis `~/.git/info/exclude` den
Eintrag

    *.htm

hinzuzufügen.  Je nachdem was Sie bearbeiten, sollten auch Hilfsdateien für
LaTeX, generierter Object-Code, etc. *nicht* im Repository gesichert werden.
Damit Ihr Name und Ihre Email-Adresse von `git commit` korrekt eingetragen
werden, empfiehlt es sich, auf jedem Arbeitsrechner die Datei `~/.gitconfig`
entsprechend (und gleichlautend) zu befüllen.

Das lokale Repository dient gleichzeitig als Backup und als Plattform zur
Offline-Arbeit.  Wenn Sie mehrere Rechner benutzen, auf denen jeweils ein
halbwegs aktuelles lokales Repository liegt, haben Sie mehrere Backups zur
Verfügung und sind recht sicher geschützt vor Datenverlust.  Außerdem sind
alle Inhalte identisch bis zum jüngsten `git fetch` unabhängig vom aktuell
genutzten Arbeitsrechner.

# Markdown

Markdown ist eine Sprache, die einfache Textformattierung ermöglicht.
Im Folgenden listen wir die wichtigsten Funktionalitäten auf, orientieren Sie
sich im Allgemeinen auch an der Formatierung der Datei, welche Sie bearbeiten.
Für weitere Informationen empfehlen wir den
[Markdown-Guide](https://www.markdownguide.org/basic-syntax/#overview).

- Überschriften können mittels `# ` vor der Überschrift erstellt werden.
  Unterüberschriften werden analog mit `##`, `###`, usw, erstellt.

- Listen können mit einem `- ` vor jedem Stichpunkt erstellt werden.
  Aufzählungen funktionieren analog.

      1. Erster Schritt
      2. Zweiter Schritt

   wird zu

1. Erster Schritt
2. Zweiter Schritt

- Um eine Sektion des Wikis zu verlinken wird zunächst in eckigen Klammern der
  Link-Text festgelegt, der blau unterlegt erscheint. Dahinter wird in runden
  Klammern der Name der Datei ohne Datei-Endung angegeben, gefolgt von `#` und
  dem Namen der Sektion in Kleinbuchstaben.

      [Anleitung](Zum-Wiki-Beitragen#setup)

  wird zu

  [Anleitung](Zum-Wiki-Beitragen#setup)


- Um Wörter hervorzuheben, zum Beispiel Code oder Felder auf einer Webseite,
  kann das Wort mit `` ` `` eingeklammert werden.

      Dieses `Wort` wird hervorgehoben.

   wird zu

   Dieses `Wort` wird hervorgehoben.
- Mathematische Ausdrücke können mit `$` eingeklammert werden.

      $5^2$

   wird zu

   $5^2$
