# ESP32-WLED-Strips — Sternwarte Höfingen Kuppelraum

WLED-basierte LED-Steuerung für den Kuppelraum der Sternwarte Höfingen.
ESP32 + 2× WS2812B LED-Strips (5V, adressierbar) + 4 Hardware-Buttons.

---

## Hardware

| Komponente      | Details                                                      |
|-----------------|--------------------------------------------------------------|
| Controller      | ESP32 NodeMCU Dev Kit C                                      |
| LED-Strips      | 2× WS2812B ECO, 5 m, 300 LEDs, DC 5V (in Serie → 600 LEDs) |
| LED-Daten-Pin   | GPIO 16                                                      |
| Button 1        | GPIO 17 — Rot                                                |
| Button 2        | GPIO 18 — Weiß                                               |
| Button 3        | GPIO 19 — Aus                                                |
| Button 4        | GPIO 21 — Funky Mode                                         |

Pinout-Diagramm: [docs/pinout.md](docs/pinout.md)
Einkaufsliste:   [docs/shopping.md](docs/shopping.md)

---

## Button-Belegung

> WLED Macro-Reihenfolge: **[Short press, Long press, Double press]**

| Button    | GPIO | Short press            | Long press              | Double press           |
|-----------|------|------------------------|-------------------------|------------------------|
| **Rot**   | 17   | Rot 50 % (Preset 20)  | Rot 10 % (Preset 22)   | Rot 100 % (Preset 21) |
| **Weiß**  | 18   | Weiß 50 % (Preset 23) | Weiß 10 % (Preset 25)  | Weiß 100 % (Preset 24)|
| **Aus**   | 19   | Alle aus (Preset 26)   | Jede zehnte (Preset 28) | Jede zweite (Preset 27)|
| **Funky** | 21   | Rainbow (Preset 29)    | Theater Chase (Preset 31)| Larson (Preset 30)    |

---

## Presets

| ID | Name                | Effekt                        | Helligkeit |
|----|---------------------|-------------------------------|-----------|
| 20 | Rot 50 %            | Solid Rot                     | 50 %      |
| 21 | Rot 100 %           | Solid Rot                     | 100 %     |
| 22 | Rot 10 %            | Solid Rot                     | 10 %      |
| 23 | Weiß 50 %           | Solid Weiß                    | 50 %      |
| 24 | Weiß 100 %          | Solid Weiß                    | 100 %     |
| 25 | Weiß 10 %           | Solid Weiß                    | 10 %      |
| 26 | Aus                 | `on: false` — LEDs komplett aus | —        |
| 27 | Jeder zweite        | Solid Weiß, grp:1 spc:1       | 100 %     |
| 28 | Jede zehnte         | Solid Weiß, grp:1 spc:9       | 100 %     |
| 29 | Rainbow 100 %       | Rainbow (fx 9)                | 100 %     |
| 30 | Larson 100 %        | Larson Scanner (fx 40)        | 100 %     |
| 31 | Theater Chase 100 % | Theater Chase (fx 13)         | 100 %     |

---

## Backup / Restore

1. WLED auf den ESP32 flashen ([Web-Installer](https://install.wled.me/)).
2. In der WLED-Weboberfläche unter **Config → Security & Updates → Restore** importieren:
   - `config/wled_cfg_v2.json` — Gerätekonfiguration
   - `config/wled_presets_v2.json` — Presets & Button-Macros
3. Neu starten.

---

## Repo-Struktur

```
config/
  wled_cfg_v2.json       <- aktuelle Gerätekonfiguration
  wled_presets_v2.json   <- aktuelle Presets & Button-Macros
docs/
  pinout.md              <- GPIO-Pinout & Verdrahtung
  shopping.md            <- Einkaufsliste mit Links
CHANGELOG.md             <- Änderungshistorie
README.md
```

---

## Lizenz

Konfiguration, Dokumentation und eigene Skripte in diesem Repository: **MIT License**.
**WLED** selbst steht unter der [EUPL-1.2](https://github.com/Aircoookie/WLED/blob/main/LICENSE).
