> [!NOTE]  
> 2025-06-15: Neu: Maximale Lade- und Entladeströme festlegen. Zu aktualisieren sind alle Scripte und Automationen mit Ausnahme von `werte-überwachen-neu-setzen.yaml`
>
> 2025-05-14: Berücksichtigung der EVCC Speichersteuerung.
> 
> 2025-05-05: Da sich bei mir der Passive Mode immer wieder geändert hat - warum auch immer, habe ich nun eine Automatisierung geschrieben, die die Werte überwacht und sicherstellt, dass die aktuellen Werte immer zur aktuellen Einstellung des Helfers passen. Dies zog weitreichende Änderungen nach sich. Bitte alle Skripte und Automatisierungen aktualisieren.

# Heimspeicher steuern (Sofar Solar HYD x KTL)

Der Sofar Solar Wechselrichter hat verschiedene [Energy Storage Modes](https://homeassistant-solax-modbus.readthedocs.io/en/latest/sofar-energy-storage-modes/). Für unsere Zwecke schalten die Skripte den Wechselrichter in den Passive Mode. In diesem kann der Wechselrichter beliebig gesteuert werden. Es wird dann auch nicht in den Eingenutzungsmodus (Self-Use) zurückgeschaltet. Diesen Modus emulieren wir auch im Passive Mode. Der Vorteil hierbei: Das Umsetzen des Energy Storage Mode wird jedesmal ins EEPROM des Wechselrichters geschrieben, welcher eine begrenzte Anzahl von Schreibzugriffen hat. Daher beschränken wir uns auf das setzen der Leistungswerte, welche nur im Speicher vorgehalten werden.

## Helfer

Die Helfer wie im [übergeordneten Ordner beschrieben](../README.md) anlegen.


## Skripte

### Passive Mode Werte Setzen

[`passive-mode-werte-setzen.yaml`](./scripte/passive-mode-werte-setzen.yaml)

Ein parametrisiertes Script, dass sicherstellt, dass der Passive Mode erfolgreich gesetzt wird und dann die entsprechenden Werte im Wechselrichter setzt.

#### Notwendige Anpassungen
- `<companion_app_device_name>` Der Gerätename Deines Smartphones auf dem die Companion App läuft um Benachrichtigungen zu erhalten.

### Modus setzen

[`modus-setzen.yaml`](./scripte/modus-setzen.yaml)

Nutzt das obige Script, um den aktuell im Helfer eingestellten Modus zu setzen.

## Automatisierungen

### Modus Änderung

[`modus-änderung.yaml`](./automatisierungen/modus-änderung.yaml)

Reagiert auf die Änderungen des Heimspeicher Modus Dropdown Helfers und setzt die Werte mit Hilfe der obigen Scripte entsprechend.

### Passive Mode überwachen und ggf. Werte neu setzen

[`./automatisierungen/werte-überwachen-neu-setzen.yaml`](./automatisierungen/werte-überwachen-neu-setzen.yaml)

Sollten sich die durch den Helfer eingestellten Werte durch irgendwelche anderen Einflüße ändern und entsprechen sie nicht mehr den zu erwartenden Werten, dann werden die Werte neu gesetzt und eine Benachrichtigung verschickt.

#### Notwendige Anpassungen
- `<companion_app_device_name>` Der Gerätename Deines Smartphones auf dem die Companion App läuft um Benachrichtigungen zu erhalten.

### Ladung abgeschlossen

[`ladung-abgeschlossen.yaml`](./automatisierungen/ladung-abgeschlossen.yaml)

Beendet den Modus 'Netzladen', wenn der per Helfer eingestellte, gewünschte Ziel-Ladestand erreicht worden ist und schaltet zurück auf 'Automatisches Laden und Entladen'.

#### Notwendige Anpassungen
- `<companion_app_device_name>` Der Gerätename Deines Smartphones auf dem die Companion App läuft um Benachrichtigungen zu erhalten.
