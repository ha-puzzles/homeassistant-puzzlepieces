# Heimspeicher steuern

Erlaubt es den Heimspeicher zu steuern:
- Automatisches Laden und Entladen: Der Heimspeicher lädt und entlädt nach Bedarf.
- Nur Entladen: Der Heimspeicher entlädt nach Bedarf, aber eine Ladung wird blockiert.
- Nur Laden: Der Heimspeicher lädt sich bei PV Überschuss auf. Eine Entladung wird aber blockiert.
- Netzladen: Der Heimspeicher wird aus dem Netz aufgeladen.
- Aus: Der Speicher lädt und entlädt sich nicht.

## Helfer


Für die Automatisierungen und Skripte in den Unterordnern müssen folgende Helfer angelegt werden:

### Modus Dropdown


- Typ: Dropdown-Menü
- Name:  `helper_pv_battery_mode`
- Optionen:
  - Automatisches Laden und Entladen
  - Nur Entladen
  - Nur Laden
  - Netzladen
  - Aus

Nachdem er gespeichert angelegt wurde und die Entity ID vergeben wurde, empfehle ich den Helfer nochmals aufzumachen um ihm dann einen Benutzerfreundlicheren Namen und Icon zu geben:

![Battery Mode Helper](./img/battery-mode-helper.png)

### Zielladestand

- Typ: Zahlenwert-Eingabe
- Name:  `helper_pv_battery_target_soc`
- Minimalwert: 20
- Maximalwert: 100

Auch hier empfiehlt es sich nach dem ersten Anlegen den Helfer nochmals zu öffnen und Namen und Icon zu vergeben.


## Automatisierungen & Skripte

Automatisierungen mit Hilfe der YAML Dateien aus den entsprechenden Unterordnern für den passenden Wechselrichter anlegen.

Siehe die 'README.md' Datei dort für mehr Informationen.

## Dashboard Widget

Optional kann nun auch ein Widget zu einem Lovelace Widget hinzugefügt werden, mit dem der Speicher bequem gesteuert werden kann.

![Dashboard widget](./img/dashboard-widget.png)

```yaml
- type: entities
  entities:
    - entity: input_select.helper_pv_battery_mode
      name: Heimspeicher Modus
    - entity: input_number.helper_pv_battery_target_soc
      name: Ziel SOC
```

