# Home Assistant Puzzlestücke

Wir sammeln und teilen hier Puzzlestücke für Deine Homeautomation. Jedes Puzzelstück hilft Dir Dein Zuhause mit Hilfe von Home Assistant Stück für Stück smarter zu machen. Ein Puzzlestück kann sein...

- ... eine Automation
- ... eine Konfiguration
- ... eine Dokumentation
- ... ein Script
- ... ein Dashboard
- ... oder kurz: Alle mögliche über das wir selbst schon gestolpert sind und hier mit Euch teilen.


## Voraussetzungen

Wir setzen hier folgendes voraus:

- Bereitschaft sich etwas tiefer mit der Materie zu beschäftigen: Wir versuchen immer alles möglichst einfach und allgemein zur Verfügung zu stellen - das klappt aber nicht immer und Du wirst öfters mal etwas an Deine Situation anpassen müssen.

- Ein unterstütztes System auf dem Home Assistant laufen kann: https://www.home-assistant.io/installation/

- Einen unterstützten Wechselrichter. Derzeit bieten wir Puzzleteilchen für folgende Wechselrichter an:
  - Huawei (TBD: Genauer Typ)
  - Sofar Solar HYD x KTL
  
  Natürlich können viele unserer Puzzlestücke auch an andere Wechselrichter angepasst werden und dienen hier dann als Blaupausen. Falls Du weitere Wechelrichter unterstützen willst, melde Dich bei uns.

- Falls Du eine Wallbox einbinden willst, gehen wir davon aus, dass Du diese mit EVCC steuerst: https://evcc.io/.
  In EVCC steckt enorm viel Wissen und Know How. Wir sehen hier keinen Grund dieses Wissen in Home Assistant nachzubauen, erst recht bei der enormen Vielzahl der unterstützten Geräte von EVCC: https://docs.evcc.io/docs/devices/chargers

## Unterordner

### installation

Grundlegende Informationen und Tipps zur Installation Deines Homeassistant Systemes und Informationen, wie Du die Puzzleteile zusammenfügst.

Fange am besten mit der Dokumentation in [diesem Ordner](./installation/) an.

### use-cases

Anwendungsfälle für Automatisierungen. Nachdem Dein Home Assistant und EVCC aufgesetzt ist, findest Du [hier](./use-cases/) alles um Dein System smarter zu machen.


## Grundlagen

Ein paar Grundlagen zum Umgang mit den Puzzle Teilen

### Dateien von GitHub herunterladen

1. Öffne die Datei hier in GitHub indem Du auf ihren Dateinamen klickst.
2. Klicke oben rechts auf 'Download raw file'.

### Dateiinhalte von GitHub in die Zwischenablage kopieren.

1. Öffne die Datei hier in GitHub indem Du auf ihren Dateinamen klickst.
2. Klicke oben rechts auf 'Copy raw file'.
3. Füge den Inhalt der Datei wie beschrieben aus der Zwischenablage ein.

### Home Assistant Artefakte

Viele Artefakte von Home Assistant werden im UI angelegt. Nun können wir Euch jeden einzelnen Schritt erklären, wie ihr zum Beispiel eine Automation per UI Editor anlegen könnt. Einfacher geht es über die RAW Editoren, die sich bei (fast?) allen Einstellungen finden.

Um z.B. eine Automatisierung anzulegen mit unserer Konfigurationmacht folgendes:

1. Geht zu 'Einstellungen / Automatisierungen & Szenen'.
2. Klickt unten rechts auf 'AUTOMATISIERUNG ERSTELLEN' und wählt dann 'Neue Automatisierung erstellen' um mit einer leeren Automatisierung zu starten.
3. Geht oben rechts auf das Menü mit den drei Punkten und wählt 'Als YAML bearbeiten' aus.
4. Nun seht ihr einen Text Editor, in dem ihr unsere Automatisierung als YAML einfügen könnt.
5. Kopiert den Inhalt der Datei, die die Automatisierung beschreibt, wie [oben beschrieben](#dateiinhalte-von-github-in-die-zwischenablage-kopieren) und fügt sie in den Editor in Home Assistant ein.
6. Nun könnt ihr die Automaitiserung mit dem Knopf untern rechts speichern, oder ihr könnt sie Euch nochmal im visuellen Editor verständlicher anschauen, in dem ihr im Menü oben rechts nun 'Im visuellen Editor bearbeiten' auswählt.

Bei anderen Artefakten wie Dashboards, Skripten,... u.s.w. ist das Vorgehen hier ähnlich.

### Helfer

Einige unserer Automatisierungen und Skripte benutzten Helfer. Helfer sind Entitäten, die vom Benutzer erstellt und gesetzt werden können.

Um einen Helper zu erstellen...

1. Gehe zu 'Einstellungen / Geräte & Dienste'.
2. Öffne oben den Tab 'Helfer'.
3. Klicke unten rechts auf 'HELFER ERSTELLEN'
4. Wähle den entsprechenden Helfer Typ aus wie in der jeweiligen Dokumentation beschreiben.
5. Gebe den Namen **exakt** so ein, wie in der Dokumentation beschrieben.
6. Optional: Gebe ein beliebiges Symbol an
7. Gebe die restlichen Attribute, wie beschrieben an und klicke auf 'ERSTELLEN'.

---


![Home Assistant](./img/home-assistant.png)
![Evcc](./img/evcc.png)
