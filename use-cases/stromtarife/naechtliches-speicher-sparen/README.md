# Dynamischer Stromtarif: Nächtliches Speicher sparen

Warum sollte sich der Speicher Nachts in den günstigen Stunden ganz entleeren und morgens, wenn es wieder teurer wird muss man dann den teuren Strom kaufen?

Diese Automatisierung verhindert nach 0:00 Uhr die weitere Speicherentladung, wenn der Speicher unter 35% fällt, und aktiviert ihn morgens wieder rechtzeitig zu den teuren Stunden, so dass man möglichst die Zeit bis die Sonne scheint wieder überbrücken kann.

Die Entladeschwelle von 35% sowie die Zeiten in den Automatisierungen sind an die jeweilige Speicherkapazität und den individuell zu erwartenden morgendlichen Stromverbrauch anzupassen.

## Abhängigkeiten

- [Heimspeicher: Entladung Verhindern](../../heimspeicher/heimspeicher-entladung-deaktivieren/)

## Automatisierungen

Automatisierungen mit Hilfe der YAML Dateien für die nächtliche Deaktivierung aus den entsprechenden Unterordnern für den passenden Wechselrichter und aus diesem Ordner für die morgendliche Aktivierung anlegen.
