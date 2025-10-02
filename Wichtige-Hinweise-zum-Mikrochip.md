Es gibt Programmierfehler, die dazu führen
können, dass die Mikrochips dauerhaft nicht mehr funktionieren.
Achten Sie bitte darauf diese Fehler zu vermeiden!

## Überschreiben der Main-Funktion

Wie Sie wahrscheinlich wissen hat jedes C-Programm eine main-Funktion.
Auf den Mikrochips dürfen wir aber unter keinen Umständen eine main-Funktion
implementieren, da diese bereits intern vorliegt und das Überschreiben dieser
main-Funktion mit einer eigenen main-Funktion dazu führen kann, dass der
Mirochip dauerhaft nicht mehr funkioniert.
Sie können einfach die run-Funktion in dem Beispiel-Code
[hier](Einrichten-der-Mikrochips#beispiel-für-ausgangscode) als Ausgangspunkt
verwenden.


## Allokieren von zu viel Speicher

Die Mikrochips haben 32kB Arbeitsspeicher und Sie dürfen nicht mehr Speicher
allokieren, da andernfalls der Mikrochip möglicherweise dauerhaft nicht mehr
verwendet werden kann. Speicher kann per `malloc` und `calloc` auf dem Heap oder
auch auf dem Stack per `DATATYPE VAR_NAME[SIZE];` allokiert werden.
`SIZE` beschreibt die Anzahl von `DATATYPE` allokierten Elementen deren Größe
für ein einzelnes Element in Bytes sie per `sizeof (DATATYPE)` erhalten können.
