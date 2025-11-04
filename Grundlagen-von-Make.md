Das Programm `make` ist auch für kleine Projekte schon hilfreich.
Es dient dazu, das Aktualisieren der erzeugten ausführbaren Dateien zu
automatisieren und die Entwicklung nicht-redundant und effizient zu gestalten.

# Effizientes Entwickeln

Viele Programmiersprachen, so unter Anderem C und C++, verlangen das
manuelle Übersetzen des Programms vom Quellcode (also den
menschgeschriebenen Dateien) in Maschinencode (das ausführbare
Programm als Datei).  Dies läßt sich vereinfachen mit `make`.

Die Nützlichkeit von Make beschränkt sich *nicht* aufs Programmieren,
sondern erstreckt sich auf alle möglichen dateibasierten Abläufe, zum
Beispiel das Erzeugen einer PDF-Datei aus LaTeX-Quellen oder das Erstellen
einer Grafik aus Dateien mit Meßwerten.

## Problematik in der Praxis

Betrachten wir das Kommando zum Übersetzen (Compilieren) des
[ohnechip-Progamms](/beispiele/ohnechip/ohnechip.c):

    gcc -Wall -Wextra -o ohnechip ohnechip.c

Es gibt hier zwei im Prinzip vermeidbare Fehlerquellen:

 - man ändert das C-Programm und vergißt, es neu zu übersetzen.
 - Man tippt andauernd die gesamte Zeile neu ein.

Bei größeren Projekten, die aus mehreren Dateien bestehen, sind zum
Übersetzen mehrere Aufrufe erforderlich, die sich niemand merken kann.

## Automatisierung mit Make

Das `make`-Programm liest eine Beschreibungsdatei, das sogenannte
`Makefile`, eine Datei genau dieses Namens, die nach einem Satz von Regeln
vom Nutzer geschrieben wird.

Man kann Variablen definieren, zum Beispiel für den Namen der
ausführbaren Zieldatei, und Abhängigkeiten, also von welchen
Quelldateien ein bestimmtes Ziel abhängt, und welcher Aufruf
nötig ist, um dieses Ziel zu erzeugen.

Make liest bei seinem Aufruf durch `make` auf der Kommandozeile das
`Makefile` und untersucht die Modifikationszeiten aller Dateien.
Es führt dann genau die Aufrufe in der richtigen Reihenfolge aus, die
benötigt werden, um alle definierten Zieldateien zu aktualisieren.
Wenn die Quelldateien in der Zwischenzeit nicht geändert wurden,
passiert also (sinnvollerweise) gar nichts.

Das Makefile ist Teil der Entwicklungsarbeit und gehört sozusagen zum
restlichen Code, wird also idealerweise mit gebackupt, weitergegeben etc.

Ein einfaches [Makefile für das ohnechip-Programm](/beispiele/ohnechip/Makefile)
kann so aussehen:

    ohnechip: ohnechip.c
    <TAB>gcc -Wall -Wextra $< -o $@

Hier muß statt `<TAB>` tatsächlich ein TAB-Zeichen im Makefile stehen.
Nun kann man einfach `make` tippen, und wenn das ausführbare Programm
älter ist als seine Quelldatei, wird es neu compiliert.
