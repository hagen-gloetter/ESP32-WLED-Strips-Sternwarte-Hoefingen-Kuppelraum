# Einkaufsliste

## Controller

| Artikel | Link |
|---------|------|
| ESP32 NodeMCU Module WLAN WiFi Dev Kit C Development Board | [Amazon](https://amzn.to/4cpewvm) |

## LED-Strips

| Artikel | Menge | Link |
|---------|-------|------|
| WS2812B ECO 5M 300 LEDs DC5V — Einzeln adressierbar | 3× | [Amazon](https://amzn.to/3OgrdAQ) |

## Empfohlenes Zubehör

### Netzteil

Verbaut: Mean Well LRS-100-5 (5V / 18A / 90W).
WLED-Limiter: `maxpwr: 16000 mA`.

Für 3× WS2812B 300 LEDs (auf 721 LEDs gekuerzt) + ESP32:

| Artikel | Warum | Empfehlung |
|---------|-------|------------|
| **5V / 18A (90W) Schaltnetzteil** | ✅ Bereits verbaut, ausreichend Reserve mit WLED-Limit 16A | **Im Einsatz** |
| 5V / 15A (75W) Schaltnetzteil | Funktioniert, aber weniger Reserve | Optional |

Mögliche Produkte:
- **Meanwell LRS-100-5** (5V / 18A, 90W) — verbaut [Amazon](https://amzn.to/4tQMU9P)
- **Meanwell LRS-75-5** (5V / 15A, 75W) — Alternative [Amazon](https://amzn.to/3QDesAQ)

> ⚠️ Kein USB-Netzteil für die LED-Strips verwenden — zu wenig Strom.
> ⚠️ Kein Billig-Netzteil verwenden — kann durchbrennen - Brandgefahr.
> Den ESP32 separat über USB-C versorgen oder vom gleichen 5V-Rail (GND gemeinsam).

### Weitere Teile

| Artikel | Hinweis |
|---------|---------|
| 470 Ω Widerstand | Datenleitungsschutz (zwischen GPIO16 und Strip-DIN) |
| 1000 µF / 10V Elektrolytkondensator | Puffer am 5V-Rail der Strips |
| Lautsprecherkabel 2-adrig, 0,75 mm2 | Versorgungsleitung fuer die Einspeisepunkte |
| Verteilklemmen / WAGO | Saubere Aufteilung auf Einspeisung bei 0 m, 5 m, 10 m |
| Momentary Push-Button (4×) | Taster für Buttons 1–4 (active LOW, kein Entprellen nötig) |
| JST-SM 3-Pin Stecker | Lösbare Verbindung zwischen Strip und Platine (optional) |
