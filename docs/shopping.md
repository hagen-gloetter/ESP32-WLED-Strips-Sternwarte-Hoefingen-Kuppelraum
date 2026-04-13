# Einkaufsliste

## Controller

| Artikel | Link |
|---------|------|
| ESP32 NodeMCU Module WLAN WiFi Dev Kit C Development Board | [Amazon](https://amzn.to/4cpewvm) |

## LED-Strips

| Artikel | Menge | Link |
|---------|-------|------|
| WS2812B ECO 5M 300 LEDs DC5V — Einzeln adressierbar | 2× | [Amazon](https://amzn.to/3OgrdAQ) |

## Empfohlenes Zubehör

### Netzteil (kaufen!)

Für 2× WS2812B 300 LEDs (600 gesamt) + ESP32 bei WLED `maxpwr: 9000 mA`:

| Artikel | Warum | Empfehlung |
|---------|-------|------------|
| **5V / 10A (50W) Schaltnetzteil** | ✅ Ausreichend für alle Rot-Presets + Weiß mit WLED-Limit | **Kaufen** |
| 5V / 15A (75W) Schaltnetzteil | Weiß 50 % ohne Limiter-Eingriff | Optional |

Empfohlene Produkte:
- **Meanwell LRS-50-5** (5V / 10A, 50W) — Industriequalität, langlebig
- **Meanwell LRS-75-5** (5V / 15A, 75W) — falls mehr Weißlicht gewünscht

> ⚠️ Kein USB-Netzteil für die LED-Strips verwenden — zu wenig Strom.
> Den ESP32 separat über USB-C versorgen oder vom gleichen 5V-Rail (GND gemeinsam).

### Weitere Teile

| Artikel | Hinweis |
|---------|---------|
| 470 Ω Widerstand | Datenleitungsschutz (zwischen GPIO16 und Strip-DIN) |
| 1000 µF / 10V Elektrolytkondensator | Puffer am 5V-Rail der Strips |
| Momentary Push-Button (4×) | Taster für Buttons 1–4 (active LOW, kein Entprellen nötig) |
| JST-SM 3-Pin Stecker | Lösbare Verbindung zwischen Strip und Platine (optional) |
