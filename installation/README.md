# Installation

Grundsätzlich wollen wir hier nicht die Dokumentationen anderer wiederholen. Aber oft führen viele Wege zum Erfolg und einige unserer Puzzlestücke vertrauen darauf, dass manche Dinge entsprechend installiert sind. Falls ihr also von dieser Anleitung abweicht, könnte es sein, dass ihr an manchen Stellen ein paar Dokumentationen oder Konfigurationen anpassen müsst.

## Software

Es wird benötigt:

- [Home Assistant](https://www.home-assistant.io/)
- [EVCC](https://evcc.io/)
- [Mosquitto](https://mosquitto.org/) MQTT Broker

### Installation mit HAOS

[Home Assistant Operation System (HAOS)](https://developers.home-assistant.io/docs/operating-system/) ist ein eigenes Operating System, basierend auf Linux, dass es sehr einfach macht Home Assistant und viele Komponenten - idealerweise auf einem Raspberry PI - zu installieren.

TBD

### Installation auf einem Linux System

Vielleicht habt ihr schonen einen Raspberry PI am laufen und wollt nun nicht alles neu mit HAOS aufsetzen. Oder ihr bevorzugt einfach ein normales Linux auf dem ihr Euch frei entfalten könnt, da HAOS doch auf die Möglichkeiten seiner Add Ons beschränkt ist.

#### Linux: Raspberry Pi OS

Da das System die ganze Zeit laufen muss bietet sichfür ein stromsparender [Raspberry PI](https://www.raspberrypi.com/products/) an. Auf diesem installiert man dann mit Raspberry Pi OS ein Linux System.

1. [Raspberry Pi Imager](https://www.raspberrypi.com/software/) herunterladen und das image für die SD Karte erstellen. Als Image reicht das 'Raspberry Pi OS Lite' ohne Desktop völlig aus.
2. Nachdem das Image auf der SD Karte eingerichtet ist, die Karte im Raspberry PI eingesteckt ist und der Raspberry gebootet ist, solltet ihr mit SSH drauf kommen. Details dazu: https://www.raspberrypi.com/do…ters/getting-started.html
3. Nach der Installation einmal alle Pakete aktualisieren: `sudo apt-get update && sudo apt-get upgrade`


#### Home Assistant

Ich habe Home Assistant via Docker Container installiert.

Zunächst mal Docker installieren via apt-get install docker docker-compose.

Legt ein Verzeichnis für die Installation an, zum Beispiel ~/docker/homeassistant.

Legt eine Datei compose.yml in diesem Verzeichnis an mit folgendem Inhalt:

```yaml
version: '3'
services:
  homeassistant:
    container_name: homeassistant
    image: "ghcr.io/home-assistant/home-assistant:stable"
    volumes:
      - /home/<user>/docker/homeassistant/config:/config
      - /etc/localtime:/etc/localtime:ro
    restart: unless-stopped
    privileged: true
    network_mode: host
```
\<user\> muss hier durch Euren Usernamen ersetzt werden. Dann folgendes ausführen:

```
sudo docker-compose pull
sudo docker-compose up -d
```

#### EVCC

Installation wie hier beschrieben: https://docs.evcc.io/docs/installation/linux

Dann mit evcc configure konfigurieren. Alles weitere dazu entnehmt ihr bitte der EVCC Dokumentation.


#### Mosquitto

Damit EVCC mit HomeAssistant kommuniziert braucht ihr noch einen MQTT Broker - also am einfachsten Mosquitto. Die Installation ist einfach:

```
sudo apt-get install mosquitto
sudo systemctl enable mosquitto
```

Nun einen Benutzer 'evcc' für Mosquitto anlegen:

```
sudo mosquitto_passwd -c /etc/mosquitto/credentials evcc
```

Sollte man bereits Nutzer angelegt haben, kann mit folgendem Befehl weitere User angelegt werden:

```
sudo mosquitto_passwd -b /etc/mosquitto/credentials USERNAME PASSWORD
```

Jetzt legt man als root die Datei `/etc/mosquitto/conf.d/local.conf` an mit folgendem Inhalt:

```
listener 1883
allow_anonymous false
password_file /etc/mosquitto/credentials
```

Zu guter letzt wird noch Mosquitto neu gestartet:
```
sudo systemctl restart mosquitto
```


## Konfiguration

### EVCC an Mosquitto anbinden

Damit EVCC MQTT Topics an Mosquitto schickt muss die Konfigurationsdatei von EVCC `evcc.yaml` mit folgendem Teil ergänzt werden:

```
mqtt:
  broker: localhost:1883
  topic: evcc
  user: evcc
  password: PASSWORD
```

PASSWORD ist hierbei das Mosquitto Password für den EVCC user, wie ihr es oben anlegt habt.

Nun empfehlen wir mal kurz zu überprüfen, ob EVCC schon erfolgreich seine Daten an MQTT sendet. Am besten geht das mit dem [MQTT Explorer](https://mqtt-explorer.com/).


### EVCC an Home Assistant via MQTT anbinden

Siehe Unterordner [`evcc-mqtt-integration`](./evcc-mqtt-integration/)

