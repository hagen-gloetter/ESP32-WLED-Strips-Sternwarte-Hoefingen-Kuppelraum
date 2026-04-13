# Changelog

Alle relevanten Änderungen an Konfiguration und Dokumentation.

Format angelehnt an [Keep a Changelog](https://keepachangelog.com/de/1.0.0/).

---

## [v4] — 2026-04-14

### Hinzugefügt
- `docs/pinout.md`: Vollständiges ASCII-Pinout aller 38 Pins des ESP32 DevKit C V4
- `docs/pinout.md`: Verdrahtungsplan (physikalischer Anschlussplan mit Box-Diagramm)
- `docs/pinout.md`: Schaltplan (elektrischer Circuit-Plan mit Bauteilwerten R1/C1/SW1–SW4)

### Pinout-Entscheidung dokumentiert
GPIO16–19, 21 liegen als kompakter Block auf einer Seite des DevKit C —
optimale Wahl für Lochrasterplatine. Strapping-Pins (GPIO0/2/12/15) und
UART0 (GPIO1/3) korrekt gemieden.

---

## [v3] — 2026-04-14

### Geändert
- `docs/pinout.md`: Stromtabelle vollständig überarbeitet mit realen WS2812B-Werten
  (Rot ~20 mA/LED, Weiß ~60 mA/LED) und WLED-Limiter-Verhalten dokumentiert
- `docs/shopping.md`: Netzteil-Empfehlung konkretisiert
  → **Meanwell LRS-50-5 (5V/10A)** als Primärempfehlung
- `config/wled_cfg_v2.json`: `maxpwr: 9000` (9 A Software-Limit) bestätigt — passend zu 5V/10A-Netzteil

### Power-Budget-Übersicht (600 LEDs, maxpwr: 9000 mA)

| Preset            | Verbrauch       | Limiter aktiv? |
|-------------------|-----------------|----------------|
| Rot 10 %          | ~1,2 A          | Nein           |
| Rot 50 %          | ~6,0 A          | Nein           |
| Rot 100 %         | ~12,0 A → 9,0 A | Ja (→ ~75 %)   |
| Weiß 10 %         | ~3,6 A          | Nein           |
| Weiß 50 %         | ~18,0 A → 9,0 A | Ja (→ ~25 %)   |
| Jede zehnte       | ~3,6 A          | Nein           |

---

## [v2] — 2026-04-14

### Hinzugefügt
- Neue Presets 20–31 für vollständige Button-Belegung mit drei Helligkeitsstufen
- Preset 27 „Jeder zweite" (`grp:1, spc:1`)
- Preset 28 „Jede zehnte" (`grp:1, spc:9`)
- Preset 29 „Rainbow 100 %", 30 „Larson 100 %", 31 „Theater Chase 100 %"
- `config/wled_cfg_v2.json` — neue Gerätekonfiguration
- `config/wled_presets_v2.json` — neue Preset-Datei
- `docs/pinout.md` — GPIO-Belegung, Verdrahtungsdiagramme, Stromverbrauch
- `CHANGELOG.md`

### Geändert
- LED-Anzahl korrigiert: 720 → **600** (2× 300 LEDs in Serie)
- Preset 3 / 26 „Aus": `on: true, bri: 1` → **`on: false`** (echtes Abschalten)
- Button-Belegung vollständig überarbeitet (Short / Long / Double press je Button)
- README neu strukturiert mit Tabellen für Hardware, Buttons und Presets
- `docs/shopping.md` formatiert und um Zubehör-Empfehlungen ergänzt

### Button-Mapping v2

| Button | GPIO | Short | Long | Double |
|---|---|---|---|---|
| Rot   | 17 | Preset 20 (50 %) | Preset 22 (10 %) | Preset 21 (100 %) |
| Weiß  | 18 | Preset 23 (50 %) | Preset 25 (10 %) | Preset 24 (100 %) |
| Aus   | 19 | Preset 26 (Aus)  | Preset 28 (1/10) | Preset 27 (1/2)   |
| Funky | 21 | Preset 29 (Rainbow) | Preset 31 (Theater Chase) | Preset 30 (Larson) |

---

## [v1] — 2026-04-13

### Hinzugefügt
- Initiale WLED-Konfiguration für ESP32 + 2× WS2812B
- `config/wled_cfg.json` (720 LEDs, GPIO16, 4 Buttons)
- `config/wled_presets.json` (Presets 1–14, Buttons auf Presets 1–3 + Funky)
- `README.md`, `docs/shopping.md`
