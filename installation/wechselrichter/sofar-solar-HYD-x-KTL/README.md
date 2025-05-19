> [!WARNING]
> Muss aktualisiert werden. Es hat sich einiges getan

# Sofar HYD x KTL Einrichtung

## Wechselrichter in EVCC einbinden

Siehe die Dokumentation von EVCC: https://docs.evcc.io/docs/devices/meters#hyd-520k-3ph


## Wechselrichter in Home Assistant einbinden

Der Wechselrichter wird über die SolaX (ja richtig gelesen - die SolaX Integration unterstützt auch Sofar Wechselrichter) Integration eingebunden.

### Home Assistant Community Store 

Die notwendige Integration ist eine Community Integration. Daher muss erstmal der Community Store (HACS) in Home Assistant eingerichtet werden, falls noch nicht getan.

Installation wie hier: https://hacs.xyz/docs/setup/download/.

Im HomeAssistant sollte nun links ein neuer Menüpunkt 'HACS' auftauchen.

### SolaX Integration via HACS installieren

1. Öffne HomeAssistant in Deinem Browser und gehe auf 'HACS' im Menü.
2. Klicke auf 'Integrationen'.
3. Klicke unten rechts auf 'DURCHSUCHEN UND HERUNTERLADEN VON REPOSITORIES'.
4. Unter 'Suche nach Repository' gebe 'solax' ein.
5. Clicke auf 'SolaX Interter Modbus'
6. Unten rechts auf 'HERUNTERLADEN' klicken.

Danach einmal Home Assistant neu starten (am besten auf Entwicklerwerkzeuge und auf der ersten Seite findet ihr schon den roten Link zum Neustart).

### Den Wechselrichter verbinden

Details wie man den Wechselrichter mit EVCC und Home Assistant verbindet findet man hier: https://homeassistant-solax-modbus.readthedocs.io/en/latest/sofar-installation/

Wie empfehlen die Verbindung entweder über den Working Mode 'Transparency' wie dort beschrieben oder aber mit einem RS-485 Adapter.

> [!NOTE]
> Weise dem LSE-3 Loggerstick in Deinem Router unbedingt eine feste IP Adresse zu. Diese IP Adresse wirst Du mehrfach brauchen. Die IP Adresse muss unbedingt fest per DHCP vom Router zugewiesen werden. Eine festeingestellte IP Adresse im Loggerstick wird zu Problemen in der Kommunikation führen

> [!NOTE]
> Zu häufige Abfragen werden den Loggerstick überlasten und es wird ebenfalls zu Time Outs kommen. Getested wurde hier mit einem 30s Intervall bei EVCC und in HomeAssistant mit den Intervallen 60/30/15s. Wenn Du diese Intervalle unterschreitest kann es zu Problemen kommen.

**Erste Seite:**

- Name: "sofar" (Bei einem anderen Namen werden die Entitynamen aller Entitites des Wechselrichters anders lauten und müssen dann in unserem Automationen entsprechend angepasst werden - wir empfehlen also sehr diesen Namen zu verwenden, falls Du unsere Automationen nutzen willst)
- Schnittstelle: TCP/Ethernet (alternativ Seriell, wenn Du Dich direkt über einen RS-485 USB Adapter verbindest)
- Modbus Adresse: 1
- Wechselrichter Typ: sofar
- Abfragefrequenz: 60 Sekunden (oder länger, aber nicht kürzer)
- Medium polling interval: 30 Sekunden
- Fast polling interval: 15 Sekunden
- Namenszusatz für den Wechselrichter: Kann beliebig gewählt werden, sollte aber möglichst kurz sein oder leer.

Setze die Checkboxen je nachdem welche Features Dein Wechselrichter unterstützt - ist für unseren Fall aber alles optional.

![Erste Seite der Konfiguration](./img/setup-page1.png)

Auf 'Bestätigen' klicken. Nun muss die TCP/IP Schnittstelle für ModBus TCP konfiguriert werden:

**Zweite Seite:**

- Gebe die IP Adresse des Loggersticks oder eines Modbus TCP server an.
- Port: 8899
- Modbus TCP

![Zweite Seite der Konfiguration](./img/setup-page2.png)

**Dritte Seite:**

Nun kann man noch wählen ob man das Auslesen der Batteriemodule aktivieren will. Dies ist für manche Automatisierungen notwendig und sollte aktiviert bleiben. Nochmals auf 'Bestätigen' klicken.

Nun sollte unter der Integration 1 Gerät für den Wechselrichter und jeweils ein Gerät pro Batteriemodul auftauchen. Klicke mal darauf.

Wenn die Steuerelemente und Entities noch nicht mit Werten gefüllt sind, habe etwas Geduld. Das kann dauern. Weiter unten unter 'Sensoren' werden dann irgendwann auch mal jede Menge Werte des Wechselrichters angezeigt.

