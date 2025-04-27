> [!NOTE]  
> Neues Update, das vorherige Probleme hoffentlich löst. Es muss ein anderer Helper angelegt werden. Die vorherigen können gelöscht werden. Außerdem müssen die existierenden Automatisierungen aktualisiert werden und es gibt weitere Automatisierungen.

# Octopus Intelligent Go mit EVCC

Mit diesen Satz von Automatisierungen kann die aktive Steuerung des angemeldeten Fahrzeuges per Octopus App bei Octopus Intelligent Go einfach eingeschaltet bleiben. Die Automatisierungen verhindern, dass Octopus der Steuerung von EVCC in the Quere kommt und umgekehrt.

Ein paar grundlegende Eigenschaften und Einschränkungen:
- PV Überschuss wird immer dem Laden per Octopus Go vorgezogen. Daher wird tagsüber Smartes Laden von Octopus deaktiviert und erst nach Sonnenuntergang wieder eingeschaltet. Smarte Ladezeiten tagsüber werden mit diesen Automatisierungen ignoriert.
- Hier wird der Fall abgehandelt, dass Octopus Intelligent Go das Fahrzeug steuert, nicht eine von Octopus Intelligent Go unterstützte Wallbox.
- Die Automatisierungen benötigen Zugriff auf die fahrzeugseitige Steuerung und sind beispielhaft für einen Tesla implementiert. Diese fahrzeugabhängigen Teile sind aber in Scripte ausgelagert, die leicht durch eigene Scripte ersetzt werden können. Beispiele der Scripte für [Tesla: Ladung starten/stoppen](../../fahrzeuge/Tesla/ladung-starten-stoppen/).
- Da bei Tesla die API Aufrufe ab Februar 2025 kostenpflichtig geworden sind wird versucht - wann immer möglich - überflüssige Aufrufe zu vermeiden. Bei anderen Fahrzeugen gibt es eventuell andere Limits für die Anzahl der Aufrufe pro Tag.


## Abhängigkeiten
- Die [octopus-germany](https://github.com/thecem/octopus_germany) Integration von @thecem in mindestens der Version 0.0.33.
- Die [Sonne](https://www.home-assistant.io/integrations/sun) Integration von Home Assistant muss konfiguriert sein über den Standort des Wohnortes.
- Unsere [EVCC MQTT Entities](../../../installation/evcc-mqtt-integration/), damit die Entities von EVCC passen.
- Für Tesla Fahrzeuge:
  - Eine funktionsfähig konfigurierte [Tesla Fleet Integration](https://www.home-assistant.io/integrations/tesla_fleet/#configuration) mit der Befehle ausgeführt werden können.
  - [Tesla: Entities manuell aktualisieren](../../fahrzeuge/Tesla/entities-manuell-aktualisieren/)
  - [Tesla: Ladung starten/stoppen](../../fahrzeuge/Tesla/ladung-starten-stoppen/)
- Für alle anderen Fahrzeuge: Jeweils ein Script, das die Ladung auf Fahrzeugseite stoppt, und ein Script, das die Ladung auf Fahrzeugseite startet. 
  - Contributions für Scripte anderer Hersteller sind sehr willkommen (YAML Dateien der Scripte und ein kleines README.md mit einer kurzen Beschreibung, idealerweise als Pull Request im [`fahrzeuge` Ordner](../../fahrzeuge/)).

## Helfer

Für die Automatisierungen muss der folgende [Helper](../../../README.md#helfer) angelegt werden:

- Typ: Schalter
- Name:  `helper_intelligent_go`
- Icon: `mdi:battery-clock-outline`

Mit diesen Schaltern kann man steuern ob man Octopus erlaubt nachts einen Ladevorgang zu starten: Ein: Intelligent Go darf nachts einen Ladevorgang starten. Aus: Ladevorgänge nur über EVCC gesteuert.

Da man die Schalter später sicher auf einem Dashboard anzeigen will, empfiehlt es sich auch gleich schöne Icons und nach dem Anlegen, wenn die Entitäts ID angelegt wurde, einen benutzerfreundlichen Namen zu vergeben.

## Skripte

Die folgenden Skripte werden von den unten stehenden Automatisierungen aufgerufen.

### Smartes Laden einschalten

[`igo-smartes-laden-einschalten.yaml`](./scripte/igo-smartes-laden-einschalten.yaml)

Schaltet das Smarte Laden ein und überprüft ob dies erfolgreich erfolgt ist.

#### Notwendige Anpassungen
- Scriptaufruf zum aufwecken des Fahrzeuges.
- `<octopus_id>` ist durch die von der Integration vergebene ID zu ersetzen. Du findest diese im Name der Entitäten der Octopus Germany Integration.
- `<companion_app_device_name>` Der Gerätename Deines Smartphones auf dem die Companion App läuft um Benachrichtigungen zu erhalten.

### Smartes Laden ausschalten

[`igo-smartes-laden-ausschalten.yaml`](./scripte/igo-smartes-laden-ausschalten.yaml)

Schaltet das Smarte Laden aus und überprüft ob dies erfolgreich erfolgt ist.

#### Notwendige Anpassungen
- `<octopus_id>` ist durch die von der Integration vergebene ID zu ersetzen. Du findest diese im Name der Entitäten der Octopus Germany Integration.
- `<companion_app_device_name>` Der Gerätename Deines Smartphones auf dem die Companion App läuft um Benachrichtigungen zu erhalten.



## Automatisierungen

Es gibt allgemeine [Automatisierungen](../../../README.md#grundlagen), die ihr immer braucht (YAML Datei beginnt mit `igo-`) und jeweils ein Satz für Ladepunkt 1 (YAML Datei beginnt mit `lp1-igo-`) und Ladepunkt 2 (YAML Datei startet mit `lp2-igo-`). Entweder die Automatisierungen für beide Ladepunkte anlegen oder nur für den Ladepunkt an dem das von Octopus Intelligent Go gesteuerte Fahrzeug normalerweise hängt.

Folgende Automatisierungen sind mit Hilfe der YAML Dateien anlegen. 

### Intelligent Go Helper aus

[`igo-helper-aus.yaml`](./automatisierungen/igo-helper-aus.yaml)

Wenn der Helper ausgeschaltet wird, wird Smartes Laden bei Octopus ausgeschaltet.

### Intelligent Go Helper ein

[`igo-helper-ein.yaml`](./automatisierungen/igo-helper-ein.yaml)

Wenn der Helper eingeschaltet und wenn es gerade Nacht ist, wird Smartes Laden bei Octopus sofort eingeschaltet. Sonst wird das später bei Sonnenuntergang gemacht.

### Sonnenaufgang

[`igo-sonnenaufgang.yaml`](./automatisierungen/igo-sonnenaufgang.yaml)

Bei Sonnenaufgang wird Smartes Laden bei Octopus ausgeschaltet und die Kontrolle wieder an EVCC übergegben.

### Sonnenuntergang

[`igo-sonnenuntergang.yaml`](./automatisierungen/igo-sonnenuntergang.yaml)

Bei Sonnenuntergang wird - falls der Helper eingeschaltet ist - Smartes Laden wieder eingeschaltet, so dass Octopus wieder die Kontrolle über den Ladevorgang übernehmen kann.

### Smartes Laden startet

[`lp1-igo-smartcharging-startet.yaml`](./automatisierungen/lp1-igo-smartcharging-startet.yaml) / [`lp2-igo-smartcharging-startet.yaml`](./automatisierungen/lp2-igo-smartcharging-startet.yaml)

Wenn Octopus eine Niedrigtarifphase von Octopus startet, wird EVCC auf den Modus 'Schnell' geschaltet, wenn der entsprechende Helper eingeschaltet ist.

#### Notwendige Anpassungen
- `<octopus_id>` ist durch die von der Integration vergebene ID zu ersetzen. Du findest diese im Name der Entitäten der Octopus Germany Integration.
- `<companion_app_device_name>` Der Gerätename Deines Smartphones auf dem die Companion App läuft um Benachrichtigungen zu erhalten.

### Smartes Laden endet

[`lp1-igo-smartcharging-endet.yaml`](./automatisierungen/lp1-igo-smartcharging-endet.yaml) / [`lp2-igo-smartcharging-endet.yaml`](./automatisierungen/lp2-igo-smartcharging-endet.yaml)

Setzt den Lademodus von EVCC zurück auf 'PV', wenn die Niedrigtarifphase von Octopus endet.

#### Notwendige Anpassungen
- `<octopus_id>` ist durch die von der Integration vergebene ID zu ersetzen. Du findest diese im Name der Entitäten der Octopus Germany Integration.
- `<companion_app_device_name>` Der Gerätename Deines Smartphones auf dem die Companion App läuft um Benachrichtigungen zu erhalten.


### EVCC auf 'Schnell': Sofort starten

[`lp1-igo-schnell-laden.yaml`](./automatisierungen/lp1-igo-schnell-laden.yaml) / [`lp2-igo-schnell-laden.yaml`](./automatisierungen/lp2-igo-schnell-laden.yaml)

Wenn außerhalb der nächtlichen Zeit das Auto schnell geladen wird und dazu der Lademodus des Auto auf 'schnell' gestellt wird, stelle sicher, dass - falls das Laden des Autos fahrzeugseitig von Octopus deaktiviert wurde - die Ladung sofort ohne Zeitverzögerung gestartet wird. 

Diese Automatisierung ist optional, da die nächste Automatisierung ebenfalls sicher stellt, dass der Ladevorgang startet - jedoch ggf. mit einer Zeitverzögerung von etwa 5 Minuten.

#### Notwendige Anpassungen
- Scriptaufruf zum Starten des fahrzeugseitigen Ladevorgangs

### Sonstige Zeit: Überprüfen ob Ladevorgang gestartet wurde.

[`lp1-igo-sonst-ladung-starten.yaml`](./automatisierungen/lp1-igo-sonst-ladung-starten.yaml) / [`lp2-igo-sonst-ladung-starten.yaml`](./automatisierungen/lp2-igo-sonst-ladung-starten.yaml)

Wenn außerhalb der nächtlichen Zeit ein Ladepunkt für mindestens 5 Minuten aktiviert wird (egal ob im Modus 'PV' oder 'schnell') und dann immer noch kein aktiver Ladevorgang erkannt worden ist, gehen wir davon aus, dass das Laden vermutlich fahrzeugseitig gestoppt wurde. Dann wird fahrzeugseitig der Ladevorgang gestartet.

#### Notwendige Anpassungen
- Scriptaufruf zum Starten des fahrzeugseitigen Ladevorgangs
- `<companion_app_device_name>` Der Gerätename Deines Smartphones auf dem die Companion App läuft um Benachrichtigungen zu erhalten.

# Sonstige Zeit: Ladevorgang unterbrochen

[`./lp1-igo-sonst-ladung-unterbrochen.yaml`](./automatisierungen/lp1-igo-sonst-ladung-unterbrochen.yaml) / [`./lp2-igo-sonst-ladung-unterbrochen.yaml`](./automatisierungen/lp2-igo-sonst-ladung-unterbrochen.yaml)

Wenn außerhalb der nächtlichen Zeit der Ladevorgang an einem Ladepunkt unterbrochen wird, aber der Ladepunkt immer noch aktiviert ist, gehen wir davon aus, dass die Ladevorgang fahrzeugseitig von Octopus Intelligent Go unterbrochen worden ist. Dann wird fahrzeugseitig der Ladevorgang gestartet.

#### Notwendige Anpassungen
- Scriptaufruf zum Starten des fahrzeugseitigen Ladevorgangs
- `<companion_app_device_name>` Der Gerätename Deines Smartphones auf dem die Companion App läuft um Benachrichtigungen zu erhalten.
