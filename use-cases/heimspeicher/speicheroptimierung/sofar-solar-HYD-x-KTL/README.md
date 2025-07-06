# Speicheroptimierung (Sofar Solar HYD x KTL)

Alles wie im übergeordneten Ordner anlegen. Für Sofar spezifische Entities muss jedoch folgender Helfer angelegt werden. Dies wird gemacht um die Automatisierungen und Skripte der Speicheroptimierung im übergeordneten Ordner von den Wechselrichter-spezifischen Entities zu entkoppeln.

## Helfer

Folgender Sofar-spezifischer [Helfer](../../../../README.md#helfer) muss angelegt werden:

### Batterieladestand

Ein Helfer, der einfach nur den aktuellen Ladestand des Heimspeichers zurückgibt. 

- Typ: Template für einen Sensor
- Name beim Anlegen/Entitäts-ID:  `helper_speicheroptimierung_bat_soc`
- Zustandstemplate:
  ```
  {{ states('sensor.sofar_battery_capacity_total') | float }}
  ```
- Maßeinheit: %
- Geräteklasse: Batterie
- Zustandsklasse: Messwert