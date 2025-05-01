# Hyundai Ioniq 5: Fahrzeugladegrenze bei Bedarf erhöhen

Script um die Fahrzeugladegrenze im Bedarfsfall zu erhöhen.

Wenn man die Ladegrenze per EVCC steuern lässt, könnte man eigentlich die Ladegrenze im Fahrzeug immer bei 100% lassen. Das hat jedoch einen entscheidenden Nachteil: Taucht in EVCC ein Problem auf, dann kann es sein, dass die Wallbox das Auto stur weiter auf 100% lädt. Das kam bei mir zum Beispiel schon einmal vor, als die Cloud API meiner Wallbox im falschen Moment Probleme machte. Oder EVCC kann auch mal aus diversen Gründen abstürzen. Und schon stand das Auto bei 100%, obwohl ich es gar nicht gebraucht habe. Sehr schlecht für die Batterie.

Daher sollte man normalerweise im Fahrzeug eine Ladegrenze unter 100% eingestellt haben. Ich habe so zum Beispiel Fahrzeugseitig 85% eingestellt. Das Problem dabei jedoch: Nun will ich wirklich mal auf 100% laden. Wehe aber ich vergesse nun im Auto den Ladelimit auch zu erhöhen. Dieses Script erledigt das:

- Ist die gewünschte Ladegrenze höher als die derzeitige Ladegrenze im Fahrzeug, dann wird diese im Fahrzeug erhöht.
- Ist die gewünschte Ladegrenze niedriger als die zur Sicherheit eingestellte Grenze, dann wird die Sicherheitsgrenze im Fahrzeug gesetzt.

Um diesen Aufruf zu automatisieren gibt es hier eine [Automatisierung für EVCC](../../../evcc/fahrzeugladegrenze-erhöhen/).

## Abhängigkeiten

- [Tesla Entities manuell aktualisieren](../entities-manuell-aktualisieren/)

## Script

[`tesla-ladegrenze-erhöhen.yaml`](./tesla-ladegrenze-erhöhen.yaml)

Beim Aufruf muss die Variable `desired_limit_soc` übergeben werden, welche den gewünschten Ladelimit setzt.

Im Script den Wert der Variablen `default_car_limit_soc` auf den von Euch gewünschten Standardwert des Ladelimits setzen, der mindestens gesetzt wird.

### Notwendige Anpassungen
Ersetze `<fahrzeug_id>` durch den Namen, den die Tesla Fleet Integration für Euren Tesla vergeben hat.