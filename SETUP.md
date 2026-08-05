# SETUP-Howto (einsteigerfreundlich)

Dieses Setup ist für Leute gedacht, die mit ESP32, WLED und LED-Strips zum ersten Mal arbeiten.
Wenn du die Schritte in der Reihenfolge durchgehst, solltest du am Ende ein sauber laufendes System haben.

Was wir machen:
- Hardware aufbauen (ESP32, Strips, Buttons, Netzteil)
- WLED auf den ESP32 flashen
- Projekt-Konfiguration importieren
- Kurz testen, ob alles wie erwartet funktioniert

## 1) Vorher kurz checken

Für Teile und Verdrahtung bitte diese beiden Dateien offen haben:
- Einkaufsliste: [docs/shopping.md](docs/shopping.md)
- Pinout + Verdrahtung: [docs/pinout.md](docs/pinout.md)

Minimum an Hardware:
- 1x ESP32 DevKit C
- 3x WS2812B 5V Strips
- 5V-Netzteil mit genug Leistung
- 4x Taster
- Kabel, Klemmen, Lötzubehör

## 2) Das Grundprinzip in 30 Sekunden

- Die Daten für die LEDs kommen vom ESP32 auf GPIO16.
- Die vier Buttons hängen an GPIO17, GPIO18, GPIO19 und GPIO21 (jeweils gegen GND).
- Die Strips bekommen ihren Strom direkt vom Netzteil.
- Alle GND-Verbindungen müssen zusammenhängen.

Wenn das neu für dich ist: Kein Stress. Einfach den Verdrahtungsplan aus [docs/pinout.md](docs/pinout.md) genau nachbauen.

## 3) Hardware aufbauen

### 3.1 Datenleitung zu den LEDs

1. GPIO16 vom ESP32 auf D-IN vom ersten Strip.
2. In die Datenleitung einen kleinen Serienwiderstand setzen (typisch 10 bis 100 Ohm), möglichst nah am ESP32.
3. Die Strips sauber hintereinander verbinden: D-OUT vom ersten auf D-IN vom zweiten usw.

### 3.2 Stromversorgung

1. +5V vom Netzteil auf die Einspeisepunkte der Strips legen.
2. GND vom Netzteil mit Strip-GND und ESP32-GND verbinden.
3. Den Pufferkondensator laut [docs/pinout.md](docs/pinout.md) nahe am Strip einbauen.

### 3.3 Buttons anschließen

1. Je Taster eine Seite auf den GPIO-Pin:
   - Rot -> GPIO17
   - Weiß -> GPIO18
   - Aus -> GPIO19
   - Funky -> GPIO21
2. Die andere Seite jedes Tasters auf GND.
3. Keine externen Pull-Ups nötig (intern in WLED aktiv).

## 4) WLED flashen

Installer: [https://install.wled.me/](https://install.wled.me/)

### 4.1 Je nach Betriebssystem

- macOS: Chrome oder Edge, ESP32 per USB anschließen.
- Windows: Chrome oder Edge, ggf. Treiber installieren wenn kein COM-Port auftaucht.
- Linux: Chrome oder Edge, ggf. serielle Rechte setzen (z. B. dialout).

### 4.2 Flash-Ablauf

1. Installer im Browser öffnen.
2. Install klicken.
3. Den richtigen seriellen Port vom ESP32 auswählen.
4. Flashen starten und durchlaufen lassen.
5. Warten, bis der ESP32 neu startet.

## 5) Erstes Mal in WLED einloggen

1. Nach dem Flashen startet WLED meist mit eigenem WLAN (Access Point).
2. Mit Handy oder Notebook damit verbinden.
3. WLED im Browser aufrufen.
4. Dein normales WLAN eintragen, damit der ESP32 dauerhaft im Netzwerk erreichbar ist.

Beispielwerte (bitte ersetzen):
- SSID: Sternwarte-WLAN
- Passwort: dein-passwort
- spätere URL zum Gerät: z. B. http://192.168.178.60

## 6) Projektdateien importieren

Wenn WLED erreichbar ist:

1. In WLED auf Config -> Security & Updates -> Restore.
2. Erst [config/wled_cfg_v3.json](config/wled_cfg_v3.json) importieren.
3. Danach [config/wled_presets_v3.json](config/wled_presets_v3.json) importieren.
4. Neustarten.

Kurzer Hinweis: Es gibt im Repo auch ältere Dateien ohne _v3. Für dieses Setup bitte die _v3-Dateien nutzen.

## 7) Kurzer Funktionstest

Nach dem Neustart einmal prüfen:

1. LEDs gehen an?
2. Buttons reagieren?
3. Presets verhalten sich plausibel (Rot, Weiß, Aus, Funky)?
4. Keine auffälligen Resets oder starkes Flackern?

Wenn alles passt: fertig.

## 8) Wenn etwas nicht klappt

### Kein Port beim Flashen sichtbar

- Anderes USB-Kabel testen (viele Kabel können nur laden).
- Anderen USB-Port nehmen.
- Unter Windows ggf. USB-Seriell-Treiber nachinstallieren.

### WLED-Weboberfläche nicht erreichbar

- 1 bis 2 Minuten warten, dann nochmal suchen.
- ESP32 kurz stromlos machen und neu starten.
- Mit dem Handy testen, falls der Rechner im Netz blockt.

### Strip bleibt dunkel

- DIN/DOUT verwechselt?
- GND wirklich gemeinsam?
- Datenleitung auf GPIO16?
- Widerstand im sinnvollen Bereich (ca. 10 bis 100 Ohm)?
- Lötstellen sauber oder evtl. kalt?

### LEDs flackern

- Einspeisung verbessern (mehrere Einspeisepunkte, bessere Kontakte).
- Lange oder lose Kabel prüfen.
- Datenleitung und Lötstellen nacharbeiten.

### Buttons tun nichts

- Gegen GND verschaltet?
- Richtiger GPIO erwischt?
- Testweise Presets direkt in WLED schalten, um LED-Seite auszuschließen.

## 9) Sicherheit

- Offene 5V-Hochstromstellen immer sauber isolieren.
- Netzteil nur mit ordentlicher Befestigung und Zugentlastung betreiben.
- Bei Hitze, Geruch oder instabiler Versorgung sofort abschalten und Verkabelung prüfen.
- Den Warnhinweis zu USB/VIN unbedingt beachten: [README.md](README.md).

Viel Erfolg beim Aufbau.
