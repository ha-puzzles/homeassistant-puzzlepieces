# EVCC: Fahrzeugladegrenze bei Bedarf erhöhen

Sollte in EVCC eine höhere Ladegrenze eingstellt werden, als derzeit im Fahrzeug eingestellt, wird die Ladegrenze im Fahrzeug automatisch erhöht.

Wenn man die Ladegrenze per EVCC steuern lässt, könnte man eigentlich die Ladegrenze im Fahrzeug immer bei 100% lassen. Das hat jedoch einen entscheidenden Nachteil: Taucht in EVCC ein Problem auf, dann kann es sein, dass die Wallbox das Auto stur weiter auf 100% lädt. Das kam bei mir zum Beispiel schon einmal vor, als die Cloud API meiner Wallbox im falschen Moment Probleme machte. Oder EVCC kann auch mal aus diversen Gründen abstürzen. Und schon stand das Auto bei 100%, obwohl ich es gar nicht gebraucht habe. Sehr schlecht für die Batterie.

Daher sollte man normalerweise im Fahrzeug eine Ladegrenze unter 100% eingestellt haben. Ich habe so zum Beispiel Fahrzeugseitig 90% eingestellt. Das Problem dabei jedoch: Nun will ich wirklich mal auf 100% laden. Wehe aber ich vergesse nun im Auto den Ladelimit auch zu erhöhen. Dann hat man am Ende ein nicht ganz vollgeladenes Auto.

## Abhängigkeiten

- Skripte für die Fahrzeuge zur Erhöhung der Ladegrenzen. Hier Beispielsweise für die Fahrzeuge
  - [Tesla: Ladegrenze erhöhen](../../fahrzeuge/Tesla/ladegrenze-erhöhen/) (Vehicle 1 in meiner EVCC Konfiguration)
  - [Hyundai Ioniq 5: Ladegrenze erhöhen](../../fahrzeuge/Hyundai-Ioniq5/ladegrenze-erhöhen/) (Vehicle 2 in meiner EVCC Konfiguration)

## Automatisierungen

[`lp1-fahrzeugladegrenze-erhöhen.yaml`](./lp1-fahrzeugladegrenze-erhöhen.yaml) | [`lp2-fahrzeugladegrenze-erhöhen.yaml`](./lp2-fahrzeugladegrenze-erhöhen.yaml)

Automatisierung jeweils für die einzelnen Ladepunkte anlegen.

### Notwendige Anpassungen

- Gegebenenfalls Skript Aufrufe für andere Fahrzeugtyppen anpassen. Beispielskripte siehe unter Abhängigkeiten oben.
- In der Auswahl muss der EVCC interne Fahrzeugname passend verglichen werden. Hier wird der Tesla durch die Entity `sensor.evcc_vehicle_1_name` identifiziert und der Hyundai Ioniq 5 durch die Entity `sensor.evcc_vehicle_2_name`. Dies ist gegebenenfalls in den Bedingungen der Auswahl anzupassen.