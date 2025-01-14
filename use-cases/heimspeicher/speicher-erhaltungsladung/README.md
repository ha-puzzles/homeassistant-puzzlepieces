# Erhaltungsladung des Stromspeichers

Viele Wechselrichter haben für den Heimspeicher einen Schutz gegen Tiefentladung. Ab einem bestimmten Ladestand wird der Speicher aus dem Netz aufgeladen.

Mit dieser Automation kann dies flexibler gemacht werden. Der Batteriestand wann diese Ladung beginnt und bis zu welchem Stand aufgeladen wird kann frei gewählt werden.

Eine bereits im Wechselrichter eingebaut Schutzfunktion muss eventuell deaktiviert werden, wenn diese den Speicher schon früher auflädt.


## Helfer

Für die Automatisierungen muss folgender Helper angelegt werden:

- Typ: Schalter
- Name:  `helper_pv_battery_mincharge_active`


## Abhängigkeiten

- [Heimspeicher: Aus Netz laden](../heimspeicher-aus-netz-laden/)
- 

## Automatisierungen

Automatisierungen mit Hilfe der YAML Dateien für die nächtliche Deaktivierung aus den entsprechenden Unterordnern für den passenden Wechselrichter und aus diesem Ordner für die morgendliche Aktivierung anlegen.


## Anpassungen

Aktuell wird der Speicher ab unter 10% auf 20% aufgeladen. In [erhaltungsladung-starten.yaml](./sofar-solar-HYD-x-KTL/erhaltungsladung-starten.yaml) ist die untere Entladegrenze anzupassen, in [erhaltungsladung-stoppen.yaml](./sofar-solar-HYD-x-KTL/erhaltungsladung-stoppen.yaml) der Zielladestand.
