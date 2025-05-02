# Erhaltungsladung des Stromspeichers

Viele Wechselrichter haben für den Heimspeicher einen Schutz gegen Tiefentladung. Ab einem bestimmten Ladestand wird der Speicher aus dem Netz aufgeladen.

Mit dieser Automation kann dies flexibler gemacht werden. Der Batteriestand wann diese Ladung beginnt und bis zu welchem Stand aufgeladen wird kann frei gewählt werden. Zudem verlässt sich diese Automation nicht auf den Gesamtspeicherstand, sondern liest die einzelnen Module aus und reagiert schon, wenn nur ein Modul zu tief entladen wird.

Eine bereits im Wechselrichter eingebaut Schutzfunktion muss eventuell deaktiviert werden, wenn diese den Speicher schon früher auflädt.


## Helfer

Für die Automatisierungen muss folgender Helper angelegt werden, welcher nur als internes Flag für die Logik der Automatisierungen dient:

- Typ: Schalter
- Name:  `helper_pv_battery_mincharge_active`


## Abhängigkeiten

- [Heimspeicher steuernn](../heimspeicher-steuern)

Außerdem muss das Auslesen der Batteriemodule in der homeassistant-solax-modbus Integration aktiviert sein. Die einzelnen Batteriemodule tauchen dann als einzelne Geräte auf.

## Automatisierungen

Automatisierungen mit Hilfe der YAML Dateien für die nächtliche Deaktivierung aus den entsprechenden Unterordnern für den passenden Wechselrichter und aus diesem Ordner für die morgendliche Aktivierung anlegen.


## Anpassungen

- `<companion_app_device_name>` Der Gerätename Deines Smartphones auf dem die Companion App läuft um Benachrichtigungen zu erhalten.
- Aktuell wird der Speicher aufgeladen, sobald eines der Speichermodule unter 8% fällt. In [erhaltungsladung-starten.yaml](./sofar-solar-HYD-x-KTL/erhaltungsladung-starten.yaml) ist die untere Entladegrenze anzupassen, in [erhaltungsladung-stoppen.yaml](./sofar-solar-HYD-x-KTL/erhaltungsladung-stoppen.yaml) der Zielladestand. 
- Die Automationen sind für 2 Speichermodule geschrieben. Bei weniger oder mehr Speichermodulen, müssen diese entsprechend in den Bedingungen der Automationen erweitert werden.
