# ESP32-WLED-Strips — Sternwarte Höfingen Kuppelraum

WLED-basierte LED-Steuerung für den Kuppelraum der Sternwarte Höfingen.
ESP32 + 3× WS2812B LED-Strips (5V, adressierbar, auf 721 LEDs gekuerzt) + 4 Hardware-Buttons.

---

## Hardware

| Komponente      | Details                                                      |
|-----------------|--------------------------------------------------------------|
| Controller      | ESP32 NodeMCU Dev Kit C                                      |
| LED-Strips      | 3× WS2812B ECO, 5 m, 300 LEDs, DC 5V (einer abgeschnitten, in Serie → 721 LEDs) |
| LED-Daten-Pin   | GPIO 16                                                      |
| Einspeisung     | 3-Punkt-Einspeisung: Anfang, nach 5 m, nach 10 m            |
| Versorgungskabel| 2-adrig, 0,75 mm2 Lautsprecherkabel                         |
| Button 1        | GPIO 17 — Rot                                                |
| Button 2        | GPIO 18 — Weiß                                               |
| Button 3        | GPIO 19 — Aus                                                |
| Button 4        | GPIO 21 — Funky Mode                                         |

Pinout-Diagramm: [docs/pinout.md](docs/pinout.md)
Einkaufsliste:   [docs/shopping.md](docs/shopping.md)

---

## Button-Aktionen (Kurzübersicht)

| Button | Short Press | Long Press | Double Press |
|--------|-------------|-----------|--------------|
| **Rot** 🔴 | Rot 50 % | Jede zehnte rot | Rot 100 % |
| **Weiß** ⚪ | Weiß 50 % | Weiß 10 % | Weiß 100 % |
| **Aus** ⚫ | Alle aus | Jede zehnte | Jeder zweite |
| **Fun** 🎨 | Rainbow | Theater Chase | Knight Rider |

---

## Button-Belegung

> WLED Macro-Reihenfolge: **[Short press, Long press, Double press]**

| Button    | GPIO | Short press            | Long press              | Double press           |
|-----------|------|------------------------|-------------------------|------------------------|
| **Rot**   | 17   | Rot 50 % (Preset 20)  | Jede zehnte rot (Preset 22) | Rot 100 % (Preset 21) |
| **Weiß**  | 18   | Weiß 50 % (Preset 23) | Weiß 10 % (Preset 25)  | Weiß 100 % (Preset 24)|
| **Aus**   | 19   | Alle aus (Preset 26)   | Jede zehnte (Preset 28) | Jede zweite (Preset 27)|
| **Funky** | 21   | Rainbow (Preset 29)    | Theater Chase (Preset 31)| Knight Rider (Preset 30) |

---

## Presets

### Standard-Presets (1–14)

| ID | Name              | Effekt                          | Helligkeit |
|----|-------------------|---------------------------------|-----------|
| 1  | Red               | Solid Rot                       | 100 %     |
| 2  | White             | Solid Weiß                      | 50 %      |
| 3  | Aus               | LEDs aus                        | —         |
| 4  | Rainbow           | Rainbow (fx 9)                  | 100 %     |
| 5  | Roter Glitter     | Red Glitter (fx 103)            | 100 %     |
| 6  | Fairytwinkle      | Fairytwinkle (fx 51)            | 100 %     |
| 7  | Red Halfbright    | Solid Rot                       | 0,4 %     |
| 8  | White Halfbright  | Solid Weiß (300 LEDs)           | 2,4 %     |
| 9  | Fun               | Playlist: Red + Glitter         | variabel  |
| 10 | I see you         | Red Telecast (fx 58)            | 100 %     |
| 11 | Knightrider       | Larson Scanner (fx 40)          | 100 %     |
| 12 | Fireworks 1D      | Fireworks 1D (fx 90)            | 100 %     |
| 13 | Theater Ants      | Theater Chase (fx 13)           | 100 %     |
| 14 | Running Colors    | Running Colors (fx 15)          | 100 %     |

### Button-Presets (20–31)

| ID | Name                | Effekt                        | Helligkeit |
|----|---------------------|-------------------------------|-----------|
| 20 | Rot 50 %            | Solid Rot                     | 50 %      |
| 21 | Rot 100 %           | Solid Rot                     | 100 %     |
| 22 | Jede zehnte rot     | Solid Rot, grp:1 spc:9        | 100 %     |
| 23 | Weiß 50 %           | Solid Weiß                    | 50 %      |
| 24 | Weiß 100 %          | Solid Weiß                    | 100 %     |
| 25 | Weiß 10 %           | Solid Weiß                    | 10 %      |
| 26 | Aus                 | `on: false` — LEDs komplett aus | —        |
| 27 | Jeder zweite        | Solid Weiß, grp:1 spc:1       | 100 %     |
| 28 | Jede zehnte         | Solid Weiß, grp:1 spc:9       | 100 %     |
| 29 | Rainbow 100 %       | Rainbow (fx 9)                | 100 %     |
| 30 | Knight Rider 100 %  | Knight Rider (fx 40)          | 100 %     |
| 31 | Theater Chase 100 % | Theater Chase (fx 13)         | 100 %     |

---

## Backup / Restore

1. WLED auf den ESP32 flashen ([Web-Installer](https://install.wled.me/)).
2. In der WLED-Weboberfläche unter **Config → Security & Updates → Restore** importieren:
   - `config/wled_cfg_v3.json` — Gerätekonfiguration
   - `config/wled_presets_v3.json` — Presets & Button-Macros
3. Neu starten.

> [!ACHTUNG]
> **VIN und USB niemals gleichzeitig anschließen!**
>
> Auf diesem ESP32 DevKit sind USB-VBUS und VIN direkt verbunden (keine Schutzdiode).
> Bei gleichzeitigem Anschluss fließt Strom rückwärts — das kann den USB-Controller
> des PCs oder das Netzteil dauerhaft beschädigen.
>
> | Modus | Vorgehen |
> |---|---|
> | **Normalbetrieb**  | Nur VIN vom Netzteil — USB-Kabel abstecken |
> | **Flashen / Konfigurieren** | Nur USB — VIN-Leitung abstecken |
> | **Beides gleichzeitig nötig** | USB-Kabel ohne VBUS verwenden (Data-only) |

---

## Repo-Struktur

```
config/
  wled_cfg_v3.json       <- aktuelle Gerätekonfiguration
  wled_presets_v3.json   <- aktuelle Presets & Button-Macros
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
