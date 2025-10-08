Es gibt Programmierfehler, die dazu führen
können, dass die Mikrochips dauerhaft nicht mehr funktionieren.
Achten Sie bitte darauf, diese Fehler zu vermeiden!

  > [!CAUTION]
  > Wer seinen Chip kaputtmacht, kauft bitte selbständig einen neuen.

Das kostet etwa vier Dollar im Internet, dauert aber leider etwas.

## Überschreiben der Main-Funktion

Wie Sie wahrscheinlich wissen, hat jedes C-Programm eine main-Funktion
(siehe auch [hier](Grundlagen-der-C-Programmierung#funktionen)).
Auf den Mikrochips dürfen wir aber unter keinen Umständen eine main-Funktion
implementieren, da diese bereits intern vorliegt und das Überschreiben dieser
main-Funktion mit einer eigenen main-Funktion dazu führen kann, dass der
Mikrochip dauerhaft nicht mehr mit neuer Software beschrieben werden kann,
da in der ursprünglichen main-Funktion die Behandlung des USB-Interfaces
implementiert war.
Sie können stattdessen die run-Funktion in diesem
[Beispiel-Code](Einrichten-der-Mikrochips#beispiel-für-ausgangscode) als Ausgangspunkt
verwenden.

## Überschreiben des Betriebs-Speichers

Die Mikrochips haben 2,5kB Arbeitsspeicher, von dem ein Teil für interne
Aufgaben wie das Lauschen am USB-Interface belegt werden.  Sie dürfen also
keinen Speicher allozieren, der diesen Bereich überschreibt, da andernfalls
der Mikrochip möglicherweise dauerhaft nicht mehr verwendet werden kann.

Speicher kann per `malloc` und `calloc` auf dem Heap oder
auch auf dem Stack per `DATATYPE VAR_NAME[SIZE];` alloziert werden.
`SIZE` beschreibt die Anzahl von `DATATYPE` allokierten Elementen, deren Größe
für ein einzelnes Element in Bytes Sie per `sizeof (DATATYPE)` erhalten können.
Auf den Chips können Heap und Stack so weit wachsen, daß sie in den Bereich
der Betriebsvariablen reichen, wodurch man dann Fehlfunktionen erzeugt.
Daher ist dieser Ansatz für unsere Chips *nicht* empfohlen.

Arbeiten Sie sicherheitshalber lieber mit Allokationen, deren Größe zum Zeitpunkt
des Compilerens bekannt sind wie folgt.

```c
/* Die Aufgabe muß auch für Größe 0 wohldefiniert sein. */
void aufgabe_der_groesse_M(int r[], unsigned char M) {
    unsigned char j;
    unsigned long microseconds = micros();

    /* loese die Aufgabe der Groesse M */
    for (j = 0; j < M; ++j) {
        r[j] = j + 17;
    }

    microseconds = micros() - microseconds;
    /* Berichte über die Ergebnisse der Aufgabe der Größe M inkl. Laufzeit */
}

/* Definiere die globale Variable r mit bekannter Speichergröße */
#define N 8
int ri[N];

/* Aufsetzen der Testreihe beispielsweise eingefügt am Ende von setup() */
unsigned char Mi;
for (Mi = 0; Mi <= N; ++Mi) {
    aufgabe_der_groesse_M(ri, Mi);
}
```
