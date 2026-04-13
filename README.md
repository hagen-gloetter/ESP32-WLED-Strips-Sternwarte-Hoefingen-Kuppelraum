# ESP32-WLED-Strips-Sternwarte-Hoefingen-Kuppelraum

WLED-basierte LED-Steuerung für den Kuppelraum der Sternwarte Höfingen:
ESP32 + 2× WS2812B LED-Strips (5V, adressierbar) + drei Hardware-Buttons für **Rot**, **Weiß** und **Aus**.

## Hardware
- Controller: ESP32
- LED-Strips: 2× WS2812B ECO, 5m, 300 LEDs, 5V
- Taster:
  - Button1 = Rot
  - Button2 = Weiß
  - Button3 = Aus
  - Button4 = Funky Mode

## WLED-Konfiguration (Kurzüberblick)
- LED-Data-Pin: GPIO16
- Buttons:
  - GPIO17 → Rot
  - GPIO18 → Weiß
  - GPIO19 → Aus
- Backups:
  - `wled_cfg.json` (Gerätekonfiguration)
  - `wled_presets.json` (Presets/Buttonszenen)

## Presets / Szenen
- Rot: Preset „Red“
- Weiß: Preset „White“
- Aus: Preset „Aus“ (alle LEDs aus)

## Backup / Restore
1. WLED auf den ESP32 flashen (Release-Bin oder Web-Installer).
2. In der WLED-Weboberfläche die Konfiguration/Presets importieren:
   - `wled_cfg.json`
   - `wled_presets.json`
3. Neustarten.

## Repo-Struktur (Empfehlung)
- `/config/` → `wled_cfg.json`, `wled_presets.json`
- `/docs/` → Verdrahtung, Hinweise, Bilder/Skizzen (optional)

## Lizenz
Für dieses Repository (Konfiguration, Doku, eigene Scripts): **MIT License**.  
Hinweis: **WLED selbst** ist ein separates Projekt mit eigener Lizenz (EUPL‑1.2). Wenn du WLED-Quellcode oder eine Fork-Firmware hier eincheckst, gelten dafür die WLED-Lizenzbedingungen.
