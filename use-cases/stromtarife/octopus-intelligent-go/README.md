> [!WARNING]  
> Es scheint ein Problem zu geben, wenn Octopus tagsüber - während PV Überschusszeiten - eine Niedrigtarifphase hat. Mehr dazu hier: https://github.com/ha-puzzles/homeassistant-puzzlepieces/discussions/30

# Octopus Intelligent Go mit EVCC

Mit diesen Satz von Automatisierungen kann die aktive Steuerung des angemeldeten Fahrzeuges per Octopus App bei Octopus Intelligent Go einfach eingeschaltet bleiben. Die Automatisierungen verhindern, dass Octopus der Steuerung von EVCC in the Quere kommt und umgekehrt.

Ein paar grundlegende Eigenschaften und Einschränkungen:
- PV Überschuss wird immer dem Laden per Octopus Go vorgezogen.
- Hier wird der Fall abgehandelt, dass Octopus Intelligent Go das Fahrzeug steuert, nicht eine von Octopus Intelligent Go unterstützte Wallbox.
- Die Automatisierungen benötigen Zugriff auf die fahrzeugseitige Steuerung und sind beispielhaft für einen Tesla implementiert. Diese fahrzeugabhängigen Teile sind aber in Scripte ausgelagert, die leicht durch eigene Scripte ersetzt werden können. Beispiele der Scripte für [Tesla: Ladung starten/stoppen](../../fahrzeuge/Tesla/ladung-starten-stoppen/).
- Da bei Tesla die API Aufrufe ab Februar 2025 kostenpflichtig geworden sind wird versucht - wann immer möglich - überflüssige Aufrufe zu vermeiden. Bei anderen Fahrzeugen gibt es eventuell Limits für die Anzahl der Aufrufe pro Tag.



## Abhängigkeiten
- Die [octopus-germany](https://github.com/thecem/octopus_germany) Integration von @thecem.
- Unsere [EVCC MQTT Integration](../../../installation/evcc-mqtt-integration/), damit die Entities passen.
- Für Tesla Fahrzeuge:
  - Eine funktionsfähig konfigurierte [Tesla Fleet Integration](https://www.home-assistant.io/integrations/tesla_fleet/#configuration) mit der Befehle ausgeführt werden können.
  - Empfehlung: [Manuelle Aktualisierung der Tesla Fleet Integration](../../fahrzeuge/Tesla/entities-manuell-aktualisieren/)
  - [Tesla: Ladung starten/stoppen](../../fahrzeuge/Tesla/ladung-starten-stoppen/)
- Für alle anderen Fahrzeuge: Jeweils ein Script, das die Ladung auf Fahrzeugseite stoppt, und ein Script, das die Ladung auf Fahrzeugseite startet. 
  - Contributions für Scripte anderer Hersteller sind sehr willkommen (YAML Dateien der Scripte und ein kleines README.md mit einer kurzen Beschreibung, idealerweise als Pull Request im [`fahrzeuge` Ordner](../../fahrzeuge/)).

## Helfer

Für die Automatisierungen in den Unterordnern müssen folgende Helper für die jeweiligen Ladepunkte, die man steuern will, angelegt werden:

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

## Automatisierungen

Es gibt jeweils ein Satz für Ladepunkt 1 (YAML Datei startet mit `lp1_`) und Ladepunkt 2 (YAML Datei startet mit `lp2_`). Entweder die Automatisierungen für beide Ladepunkts anlegen oder nur für den Ladepunkt an dem das von Octopus Intelligent Go gesteuerte Fahrzeug normalerweise hängt.

Folgende Automatisierungen sind mit Hilfe der YAML Dateien anlegen. 

### Smartcharging startet

[`lp1-igo-smartcharging-startet.yaml`](./lp1-igo-smartcharging-startet.yaml) / [`lp2-igo-smartcharging-startet.yaml`](./lp2-igo-smartcharging-startet.yaml)

Wenn Octopus eine Niedrigtarifphase startet, wird EVCC auf den Modus 'Schnell' geschaltet, wenn der entsprechende Helper für den Loadpoint eingeschaltet ist.

#### Notwendige Anpassungen
- `<octopus_kunden_nummer>` ist durch Deine Kundennummer ohne "A " zu ersetzen. Lautet Deine Kundennummer zum Beispiel "A 123456", dann muss hier "123456" stehen.
- `<companion_app_device_name>` Der Gerätename Deines Smartphones auf dem die Companion App läuft um Benachrichtigungen zu erhalten.

### Ladevorgang zurücksetzen

[`lp1-igo-smartcharging-endet.yaml`](./lp1-igo-smartcharging-endet.yaml) / [`lp2-igo-smartcharging-endet.yaml`](./lp2-igo-smartcharging-endet.yaml)

Setzt den Lademodus von EVCC zurück auf 'PV', wenn die Niedrigtarifphase endet.

#### Notwendige Anpassungen
- `<octopus_kunden_nummer>` ist durch Deine Kundennummer ohne "A " zu ersetzen. Lautet Deine Kundennummer zum Beispiel "A 123456", dann muss hier "123456" stehen.
- `<companion_app_device_name>` Der Gerätename Deines Smartphones auf dem die Companion App läuft um Benachrichtigungen zu erhalten.


### EVCC auf 'Schnell': Sofort starten

[`lp1-igo-schnell-laden.yaml`](./lp1-igo-schnell-laden.yaml) / [`lp2-igo-schnell-laden.yaml`](./lp2-igo-schnell-laden.yaml)

*Scriptaufruf zum Starten des fahrzeugseitigen Ladevorgangs muss ggf. angepasst werden.*

Wenn außerhalb der nächtlichen Zeit das Auto schnell geladen wird und dazu der Lademodus des Auto auf 'schnell' gestellt wird, stelle sicher, dass - falls das Laden des Autos fahrzeugseitig von Octopus deaktiviert wurde - die Ladung sofort ohne Zeitverzögerung gestartet wird. 

Diese Automatisierung ist optional, da die nächste Automatisierung ebenfalls sicher stellt, dass der Ladevorgang startet - jedoch ggf. mit einer Zeitverzögerung von etwa 5 Minuten.

#### Notwendige Anpassungen
- `<octopus_kunden_nummer>` ist durch Deine Kundennummer ohne "A " zu ersetzen. Lautet Deine Kundennummer zum Beispiel "A 123456", dann muss hier "123456" stehen.

### Sonstige Zeit: Überprüfen ob Ladevorgang gestartet wurde.

[`lp1-igo-sonst-ladung-starten.yaml`](./lp1-igo-sonst-ladung-starten.yaml) / [`lp2-igo-sonst-ladung-starten.yaml`](./lp2-igo-sonst-ladung-starten.yaml)

Wenn außerhalb der nächtlichen Zeit ein Ladepunkt für mindestens 5 Minuten aktiviert wird (egal ob im Modus 'PV' oder 'schnell') und dann immer noch kein aktiver Ladevorgang erkannt worden ist, gehen wir davon aus, dass das Laden vermutlich Fahrzeugseitig gestoppt wurde. Dann wird fahrzeugseitig der Ladevorgang gestartet.

#### Notwendige Anpassungen
- Scriptaufruf zum Starten des fahrzeugseitigen Ladevorgangs
- `<octopus_kunden_nummer>` ist durch Deine Kundennummer ohne "A " zu ersetzen. Lautet Deine Kundennummer zum Beispiel "A 123456", dann muss hier "123456" stehen.
- `<companion_app_device_name>` Der Gerätename Deines Smartphones auf dem die Companion App läuft um Benachrichtigungen zu erhalten.

# Sonstige Zeit: Ladevorgang unterbrochen

[`./lp1-igo-sonst-ladung-unterbrochen.yaml`](./lp1-igo-sonst-ladung-unterbrochen.yaml) / [`./lp2-igo-sonst-ladung-unterbrochen.yaml`](./lp2-igo-sonst-ladung-unterbrochen.yaml)

*Scriptaufruf zum Starten des fahrzeugseitigen Ladevorgangs muss ggf. angepasst werden.*

Wenn außerhalb der nächtlichen Zeit der Ladevorgang an einem Ladepunkt unterbrochen wird, aber der Ladepunkt immer noch aktiviert ist, gehen wir davon aus, dass die Ladevorgang fahrzeugseitig von Octopus Intelligent Go unterbrochen worden ist. Dann wird fahrzeugseitig der Ladevorgang gestartet.

#### Notwendige Anpassungen
- Scriptaufruf zum Starten des fahrzeugseitigen Ladevorgangs
- `<octopus_kunden_nummer>` ist durch Deine Kundennummer ohne "A " zu ersetzen. Lautet Deine Kundennummer zum Beispiel "A 123456", dann muss hier "123456" stehen.
- `<companion_app_device_name>` Der Gerätename Deines Smartphones auf dem die Companion App läuft um Benachrichtigungen zu erhalten.
