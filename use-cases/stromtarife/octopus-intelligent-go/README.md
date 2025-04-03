# Octopus Intelligent Go mit EVCC

Mit diesen Satz von Automatisierungen kann die aktive Steuerung des angemeldeten Fahrzeuges per Octopus App bei Octopus Intelligent Go einfach eingeschaltet bleiben. Die Automatisierungen verhindern, dass Octopus der Steuerung von EVCC in the Quere kommt und umgekehrt.

Ein paar grundlegende Eigenschaften und Einschränkungen:
- Dies hier funktioniert nur für von Octopus getriggerte, nächtliche Ladevorgänge zwischen 0:00 und 5:00 Uhr. Daher kann es sein, dass das Auto nicht ganz vollgeladen wird, wenn Octopus nach 5:00 noch einen Ladeslot plant. Lösung dafür: Abfahrtzeit in der Octopus App auf 5:00 Uhr einstellen.
  - Die Automatisierung weiß nicht, wann der Ladevorgang von Octopus außerhalb dieser Zeiten gestartet wird (eine bessere [Octopus Integration in EVCC](https://github.com/evcc-io/evcc/discussions/17766#discussioncomment-12276408) ist schon in Arbeit).
- PV Überschuss wird immer dem Laden per Octopus Go vorgezogen.
- Hier wird der Fall abgehandelt, dass Octopus Intelligent Go das Fahrzeug steuert, nicht eine von Octopus Intelligent Go unterstützte Wallbox.
- Die Automatisierungen benötigen Zugriff auf die fahrzeugseitige Steuerung und sind beispielhaft für einen Tesla implementiert. Diese fahrzeugabhängigen Teile sind aber in Scripte ausgelagert, die leicht durch eigene Scripte ersetzt werden können. Beispiele der Scripte für [Tesla: Ladung starten/stoppen](../../fahrzeuge/Tesla/ladung-starten-stoppen/).
- Da bei Tesla die API Aufrufe ab Februar 2025 kostenpflichtig geworden sind wird versucht - wann immer möglich - überflüssige Aufrufe zu vermeiden. Bei anderen Fahrzeugen gibt es eventuell Limits für die Anzahl der Aufrufe pro Tag.

## Helfer

Für die Automatisierungen in den Unterordnern müssen folgende Helper für die jeweiligen Ladepunkts, die man steuern will, angelegt werden:

- Für Ladepunkt 1
  - Typ: Schalter
  - Name:  `helper_lp1_octopus_intelligent_go_active`
  - Icon: `mdi:battery-clock-outline`
- Für Ladepunkt 2
  - Typ: Schalter
  - Name:  `helper_lp2_octopus_intelligent_go_active`
  - Icon: `mdi:battery-clock-outline`

Mit diesen Schaltern kann man steuern ob man Octopus erlaubt nachts einen Ladevorgang zu starten: Ein: Intelligent Go darf nachts einen Ladevorgang starten. Aus: Ladevorgänge nur per PV, oder wenn man EVCC auf den Modus 'Schnell' umschaltet.

Da man die Schalter später sicher auf einem Dashboard anzeigen will, empfiehlt es sich auch gleich schöne Icons und nach dem Anlegen, wenn die Entitäts ID angelegt wurde, einen benutzerfreundlichen Namen zu vergeben.

## Abhängigkeiten
- Unsere [EVCC MQTT Integration](../../../installation/evcc-mqtt-integration/), damit die Entities passen.
- Für Tesla Fahrzeuge:
  - Eine funktionsfähig konfigurierte [Tesla Fleet Integration](https://www.home-assistant.io/integrations/tesla_fleet/#configuration) mit der Befehle ausgeführt werden können.
  - Empfehlung: [Manuelle Aktualisierung der Tesla Fleet Integration](../../fahrzeuge/Tesla/entities-manuell-aktualisieren/)
  - [Tesla: Ladung starten/stoppen](../../fahrzeuge/Tesla/ladung-starten-stoppen/)
- Für alle anderen Fahrzeuge: Jeweils ein Script, das die Ladung auf Fahrzeugseite stoppt, und ein Script, das die Ladung auf Fahrzeugseite startet. 
  - Contributions für Scripte anderer Hersteller sind sehr willkommen (YAML Dateien der Scripte und ein kleines README.md mit einer kurzen Beschreibung, idealerweise als Pull Request im [`fahrzeuge` Ordner](../../fahrzeuge/)).

## Automatisierungen

Es gibt jeweils ein Satz für Ladepunkt 1 (YAML Datei startet mit `lp1_`) und Ladepunkt 2 (YAML Datei startet mit `lp2_`). Entweder die Automatisierungen für beide Ladepunkts anlegen oder nur für den Ladepunkt an dem das von Octopus Intelligent Go gesteuerte Fahrzeug normalerweise hängt.

Folgende Automatisierungen sind mit Hilfe der YAML Dateien anlegen. 

### Ladevorgang vorbereiten

[`lp1-igo-ladevorgang-vorbereiten.yaml`](./lp1-igo-ladevorgang-vorbereiten.yaml) / [`lp2-igo-ladevorgang-vorbereiten.yaml`](./lp2-igo-ladevorgang-vorbereiten.yaml)

*Scriptaufruf zum Stoppen des fahrzeugseitigen Ladevorgangs muss ggf. angepasst werden.*

Bereited EVCC und das Fahrzeug kurz vor 0:00 Uhr so vor, dass Octopus den Ladevorgang starten kann. Der Lademodus von EVCC wird auf 'schnell' geschaltet und das Laden im Fahrzeug gestoppt, damit Octopus später wieder mittels Fahrzeugsteuerung eine Ladung starten kann.

> [!NOTE]
> Ein Ladevorgang im Tesla lässt sich leider erst abschalten während die Wallbox schon einen Ladevorgang gestartet hat. Wird der Ladevorgang im Auto gestoppt bevor die Wallbox das Laden beginnt, denkt der Tesla, dass ein neuer Ladvorgang beginnt, sobald die Wallbox Strom liefert und startet die Ladung im Fahrzeug neu. Daher wird in der Automation erst der Ladevorgang gestartet und gewartet bis er gestartet wird. Erst danach wird die Ladung im Tesla pausiert.
> 
> Andere Fahrzeuge benötigen diesen Aufwand vielleicht nicht und können schon gestoppt werden, bevor man den Lademodus von EVCC auf 'schnell' schaltet.


### Ladevorgang zurücksetzen

[`lp1-igo-ladevorgang-zuruecksetzen.yaml`](./lp1-igo-ladevorgang-zuruecksetzen.yaml) / [`lp2-igo-ladevorgang-zuruecksetzen.yaml`](./lp2-igo-ladevorgang-zuruecksetzen.yaml)

Setzt um 5:00 Uhr den Lademodus von EVCC zurück auf 'PV'.


### EVCC auf 'Schnell': Sofort starten

[`lp1-igo-schnell-laden.yaml`](./lp1-igo-schnell-laden.yaml) / [`lp2-igo-schnell-laden.yaml`](./lp2-igo-schnell-laden.yaml)

*Scriptaufruf zum Starten des fahrzeugseitigen Ladevorgangs muss ggf. angepasst werden.*

Wenn außerhalb der nächtlichen Zeit das Auto schnell geladen wird und dazu der Lademodus des Auto auf 'schnell' gestellt wird, stelle sicher, dass - falls das Laden des Autos fahrzeugseitig von Octopus deaktiviert wurde - die Ladung sofort ohne Zeitverzögerung gestartet wird. 

Diese Automatisierung ist optional, da die nächste Automatisierung ebenfalls sicher stellt, dass der Ladevorgang startet - jedoch ggf. mit einer Zeitverzögerung von etwa 5 Minuten.


### Sonstige Zeit: Überprüfen ob Ladevorgang gestartet wurde.

[`lp1-igo-sonst-ladung-starten.yaml`](./lp1-igo-sonst-ladung-starten.yaml) / [`lp2-igo-sonst-ladung-starten.yaml`](./lp2-igo-sonst-ladung-starten.yaml)

*Scriptaufruf zum Starten des fahrzeugseitigen Ladevorgangs muss ggf. angepasst werden.*

Wenn außerhalb der nächtlichen Zeit ein Ladepunkt für mindestens 5 Minuten aktiviert wird (egal ob im Modus 'PV' oder 'schnell') und dann immer noch kein aktiver Ladevorgang erkannt worden ist, gehen wir davon aus, dass das Laden vermutlich Fahrzeugseitig gestoppt wurde. Dann wird fahrzeugseitig der Ladevorgang gestartet.

# Sonstige Zeit: Ladevorgang unterbrochen

[`./lp1-igo-sonst-ladung-unterbrochen.yaml`](./lp1-igo-sonst-ladung-unterbrochen.yaml) / [`./lp2-igo-sonst-ladung-unterbrochen.yaml`](./lp2-igo-sonst-ladung-unterbrochen.yaml)

*Scriptaufruf zum Starten des fahrzeugseitigen Ladevorgangs muss ggf. angepasst werden.*

Wenn außerhalb der nächtlichen Zeit der Ladevorgang an einem Ladepunkt unterbrochen wird, aber der Ladepunkt immer noch aktiviert ist, gehen wir davon aus, dass die Ladevorgang fahrzeugseitig von Octopus Intelligent Go unterbrochen worden ist. Dann wird fahrzeugseitig der Ladevorgang gestartet.
