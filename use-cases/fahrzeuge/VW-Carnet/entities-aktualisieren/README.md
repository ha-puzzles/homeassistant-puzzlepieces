# VW Carnet: Entities manuell aktualisieren

Manuelles Aktualisieren der VW Carnet Entities um sicher zu stellen, dass man den neuesten Wert hat.

## Abhängigkeiten

- [VW Carnet Integration](https://github.com/robinostlund/homeassistant-volkswagencarnet)

## Update Entities Script

[`update-entities.yaml`](./update-entities.yaml)

### Notwendige Anpassungen
- Im Script sind generell alle Entities durch den Namen Eures VW anzupassen. Ersetzt `<fahrzeug_id>` durch den Namen, den die VW Carnet Fleet Integration für Euren VW vergeben hat.
- Fügt die Entities der VW Carnet Integration hinzu, die ihr nach Aufruf abfragen wollt.

## Vielen Dank

Vielen Dank an [@Joeknx](https://github.com/Joeknx) für das Beisteuern der Skripte für VW.