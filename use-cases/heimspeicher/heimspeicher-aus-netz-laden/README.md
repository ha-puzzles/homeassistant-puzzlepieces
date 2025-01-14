# Heimspeicher: Aus Netz laden

Started das Laden des Heimspeichers aus dem Netz bis auf einen Ladestand von 80%.

## Helfer

Für die Automatisierungen in den Unterordnern muss folgender Helper angelegt werden:

- Typ: Schalter
- Name:  `helper_home_battery_manual_charging`

## Automatisierungen

Automatisierungen mit Hilfe der YAML Dateien aus den entsprechenden Unterordnern für den passenden Wechselrichter anlegen.

## Test

Nachdem untenstehender Helfer angelegt wurde und die Automatisierungen für den entsprechenden Wechselrichter aus den entsprechenden Unterordnern eingerichtet worden sind, kann der Speicher schon manuell ein- und ausgeschaltet werden.

1. Unter `Geräte und Dienste` auf den Helfer Tab gehen
2. Auf obigen Helfer klicken
3. Schalter umlegen so, dass er auf An geht.
4. Nach einer gewissen Wartezeit sollte der Speicher anfangen aus dem Netz zu laden.
3. Schalter auf Aus umlegen.
4. Nach einer gewissen Wartezeit sollte der Speicher nicht mehr aus dem Netz geladen werden.
