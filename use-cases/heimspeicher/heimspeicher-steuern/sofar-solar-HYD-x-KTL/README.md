# Heimspeicher steuern

Der Sofar Solar Wechselrichter hat verschiedene [Energy Storage Modes](https://homeassistant-solax-modbus.readthedocs.io/en/latest/sofar-energy-storage-modes/). Für unsere Zwecke schalten die Skripte den Wechselrichter in den Passive Mode. In diesem kann der Wechselrichter beliebig gesteuert werden. Es wird dann auch nicht in den Eingenutzungsmodus (Self-Use) zurückgeschaltet. Diesen Modus emulieren wir auch im Passive Mode. Der Vorteil hierbei: Das Umsetzen des Energy Storage Mode wird jedesmal ins EEPROM des Wechselrichters geschrieben, welcher eine begrenzte Anzahl von Schreibzugriffen hat. Daher beschränken wir uns auf das setzen der Leistungswerte, welche nur im Speicher vorgehalten werden.

## Helfer

Die Helfer wie im [übergeordneten Ordner beschrieben](../README.md) anlegen.


## Skript 

### Passive Mode Werte Setzen

[`passive-mode-werte-setzen.yaml`](./passive-mode-werte-setzen.yaml)

Ein paramtrisiertes Script, dass sicherstellt, dass der Passive Mode erfolgreich gesetzt wird und dann die entsprechenden Werte im Wechselrichter setzt.

Dieses Script wird von der 'Modus Änderung' Automatisierung verwendet und sollte in der Regel nicht direkt benutzt werden.

#### Notwendige Anpassungen
- `<companion_app_device_name>` Der Gerätename Deines Smartphones auf dem die Companion App läuft um Benachrichtigungen zu erhalten.


## Automatisierungen

### Modus Änderung

[`modus-änderung.yaml`](./modus-änderung.yaml)

Reagiert auf die Änderungen des Heimspeicher Modus Dropdown Helfers und setzt die Werte mit Hilfe von obigen Script entsprechend.

### Ladung abgeschlossen

[`ladung-abgeschlossen.yaml`](./ladung-abgeschlossen.yaml)

Beendet den Modus 'Netzladen', wenn der per Helfer eingestellte, gewünschte Ziel-Ladestand erreicht worden ist und schaltet zurück auf 'Automatisches Laden und Entladen'.

#### Notwendige Anpassungen
- `<companion_app_device_name>` Der Gerätename Deines Smartphones auf dem die Companion App läuft um Benachrichtigungen zu erhalten.