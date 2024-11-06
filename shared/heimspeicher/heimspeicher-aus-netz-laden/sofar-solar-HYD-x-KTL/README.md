Die Ladestromstärke ist an den verbauten Speicher anzupassen.

In dem Beispiel hier wird ein BTS Speicher mit 10 kWh angenommen, der eine maximal Ladeleistung von 5 kW hat. Da wir bei der Ladeleistung nicht ganz an die Grenze gehen, laden wir hier mit 4.5 kW.

Aus der Datei `speicher-netz-ladung-starten.yaml`:
```yaml
  - data:
      value: "4500"
    target:
      entity_id:
        - number.sofar_passive_mode_battery_power_min
    action: number.set_value
  - data:
      value: "5000"
    target:
      entity_id:
        - number.sofar_passive_mode_battery_power_max
    action: number.set_value
```

Batter Power Min wird daher auf 4500 W gesetzt und die maximale Leistung auf 5000 W.