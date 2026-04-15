# Pinout & Verdrahtung — ESP32 Sternwarte Höfingen

Referenz-Diagramm: [esp32-devkitC-v4-pinout.png](esp32-devkitC-v4-pinout.png)

---

## GPIO-Belegung

| GPIO | Funktion          | Richtung | Beschreibung                          |
|------|-------------------|----------|---------------------------------------|
| 16   | LED Data          | OUT      | WS2812B Strip — Datenleitung          |
| 17   | Button 1 — Rot    | IN       | Pull-Up intern, active LOW            |
| 18   | Button 2 — Weiß   | IN       | Pull-Up intern, active LOW            |
| 19   | Button 3 — Aus    | IN       | Pull-Up intern, active LOW            |
| 21   | Button 4 — Funky  | IN       | Pull-Up intern, active LOW            |

---

## Vollständiges Pinout — ESP32 DevKit C V4

Orientierung: USB-Anschluss **unten**. Linke Spalte = J2, rechte Spalte = J3.

```
[!] GPIO6     ────────○  1  Flash CLK    20 ○────────  VIN (+5V)
[!] GPIO7     ────────○  2  Flash SD0    21 ○────────  GPIO11 [!] Flash CMD
[!] GPIO8     ────────○  3  Flash SD1    22 ○────────  GPIO10 [!] Flash SD3
[!] GPIO15    ────────○  4  Strapping    23 ○────────  GPIO9  [!] Flash SD2
[!] GPIO2     ────────○  5  onb. LED     24 ○────────  GPIO13
[!] GPIO0     ────────○  6  Boot         25 ○────────  GND
    GPIO4     ────────○  7               26 ○────────  GPIO12 [!] Strapping
[*] GPIO16    ────────○  8  ► LED Data   27 ○────────  GPIO14
[*] GPIO17    ────────○  9  ◄ BTN1       28 ○────────  GPIO27
    GPIO5     ────────○ 10               29 ○────────  GPIO26     (DAC2)
[*] GPIO18    ────────○ 11  ◄ BTN2       30 ○────────  GPIO25     (DAC1)
[*] GPIO19    ────────○ 12  ◄ BTN3       31 ○────────  GPIO33
    GND       ────────○ 13               32 ○────────  GPIO32
[*] GPIO21    ────────○ 14  ◄ BTN4       33 ○────────  GPIO35     (Input only)
[!] GPIO3 RX  ────────○ 15               34 ○────────  GPIO34     (Input only)
[!] GPIO1 TX  ────────○ 16               35 ○────────  GPIO39     (Input only)
    GPIO22    ────────○ 17               36 ○────────  GPIO36     (Input only)
    GPIO23    ────────○ 18               37 ○────────  EN  (Reset)
    GND       ────────○ 19               38 ○────────  3V3
                      │                        │
                      │  ┌─────────────────┐  │
                      │  │   ESP32-WROOM   │  │
                      │  └─────────────────┘  │
                      │    ┌───────────────┐   │
                      │    │  USB  (unten) │   │
                      └────┴───────────────┴───┘

  [*] = in Verwendung    [!] = nicht verwenden / reserviert
```

> Die fünf verwendeten Pins (GPIO16–19, 21) liegen als kompakter Block
> auf einer Seite — ideal für das Löten auf Lochrasterplatine.

---

## Verdrahtung LED-Strip

```
ESP32 GPIO16 ──[ 10Ω ]──► DIN  (Strip 1, LED 0)
                             ├── DOUT (Strip 1, LED 299)
                             └──► DIN  (Strip 2, LED 300–599)

ESP32 GND    ──────────────── GND (beide Strips)
5V Netzteil  ──────────────── 5V  (beide Strips)
             ──[1000µF]──── GND  (Pufferkondensator am 5V-Rail)
```

> **Wichtig:** Die 5V-Versorgung der Strips **nicht** über den ESP32 führen —
> immer direkt vom Netzteil anschließen. GND gemeinsam verbinden.

> **Widerstandswert:** Der ESP32 liefert auf GPIO-Ausgängen nur **3,3V**. Der Datenpegel
> liegt damit knapp unterhalb des WS2812B-Schwellwerts von 0,7 × 5V = 3,5V.
> Ein 470Ω-Widerstand bildet zusammen mit der Leitungskapazität einen RC-Tiefpass,
> der die Flanken so weit rundet, dass die erste LED das Signal nicht mehr sicher
> dekodiert → Strip bleibt dunkel. Akzeptabler Bereich: **0Ω–100Ω** (z. B. 10Ω, 33Ω, 47Ω).
> Bei Problemen alternativ Level-Shifter (74AHCT125).

> **Troubleshooting:** Strip bleibt dunkel trotz korrektem Widerstandswert?
> → **Kalte Lötstelle prüfen.** Das Multimeter zeigt den richtigen Wert (Gleichstrom),
> der Kontakt versagt aber bei 800-kHz-Impulsen des WS2812B-Protokolls.
> Beide Beinchen des Widerstands nachlöten — Lötstelle muss glänzend/konisch sein,
> nicht matt/kugelförmig.

---

## Verdrahtung Buttons

```
GPIO17 ──┤ Button 1 ├── GND
GPIO18 ──┤ Button 2 ├── GND
GPIO19 ──┤ Button 3 ├── GND
GPIO21 ──┤ Button 4 ├── GND
```

Interne Pull-Ups aktiv (`pull: true` in `wled_cfg_v2.json`).
Kein externer Widerstand notwendig.

---

## Verdrahtungsplan

Physikalischer Anschlussplan (alle Komponenten):

```
  ┌─────────────┐
  │  Netzteil   │  +5V ──────┬─────────────────────────┬──── VIN (ESP32 Pin 20)
  │  5V / 10A   │            │                         │
  │  (PSU)      │           [C1]               ┌───────┴──────┐  ┌───────────────┐
  └──────┬──────┘        1000µF/10V            │  WS2812B     │  │  WS2812B      │
         │                   │                 │  Strip 1     │  │  Strip 2      │
         │  GND ─────────────┴─────────────────┤  300 LEDs    │  │  300 LEDs     │
         │                   │        +5V ─────┤  +5V         │  │  +5V          │
         └───────────────────┘        GND ─────┤  GND         │  │  GND          │
                                               │              │  │               │
  ESP32 GPIO16 ──[R1:  10Ω]────────────────────► DIN          │  │               │
                                               │       DOUT ──┼──► DIN           │
  ESP32 GPIO17 ──[SW1: Button Rot  ]── GND     └──────────────┘  └───────────────┘
  ESP32 GPIO18 ──[SW2: Button Weiß ]── GND
  ESP32 GPIO19 ──[SW3: Button Aus  ]── GND
  ESP32 GPIO21 ──[SW4: Button Funky]── GND
```

---

## Schaltplan

Elektrischer Schaltplan mit Bauteilwerten:

```
            +5V
             │
    ┌────────┴────────────────────────────────────────────────── +5V Rail
    │        │
   [C1]      │         ┌───────────────────────────────────────────────┐
  1000µF     │         │                    ESP32                      │
   /10V      │         │                                               │
    │         └─────────┤ VIN (Pin 20)                                  │
    │                  │                                               │
   GND ────────────────┤ GND (Pin 7)                                   │
    │                  │                              [R1: 470Ω]       │
    │                  │ GPIO16 (Pin 12) ──────────[10Ω]──────────────── DIN → Strip 1 → Strip 2
    │                  │                                               │
    │          ┌───────┤ GPIO17 (Pin 11)                               │
    │          │       │ GPIO18 (Pin  9)                               │
    │          │       │ GPIO19 (Pin  8)    Pull-Up intern (3,3V)      │
    │          │       │ GPIO21 (Pin  6)    aktiv LOW                  │
    │          │       └───────────────────────────────────────────────┘
    │        [SW1]  [SW2]  [SW3]  [SW4]
    │      Button1 Button2 Button3 Button4
    │          │
    └──────────┘ GND

  R1  =  10 Ω  (Schutzwiderstand Datenlinie, direkt am ESP32-Pin)
         ↳ Akzeptabler Bereich: 0Ω–100Ω (z. B. 10Ω, 33Ω, 47Ω)
         ↳ Nicht 470Ω verwenden: ESP32 (3,3V) + 470Ω → RC-Tiefpass dämpft Signal
           unter WS2812B-Schwellwert (0,7 × 5V = 3,5V) → Strip bleibt dunkel.
  C1  = 1000 µF / 10V  (Pufferkondensator, so nah wie möglich an Strip 1 +5V/GND)
  SW1–SW4 = Momentary Push-Button, NO (Normally Open), eine Seite an GPIO, andere an GND
```

---

## Stromverbrauch & Power-Budget

### WS2812B Referenzwerte

| Farbe       | Strom pro LED (max) |
|-------------|---------------------|
| Rot         | ~20 mA              |
| Grün / Blau | ~20 mA              |
| Weiß (R+G+B)| ~60 mA              |

WLED-Einstellung: `ledma: 55` (Worst-Case Weiß), `maxpwr: 9000 mA` (Software-Limit).

### Realer Stromverbrauch je Preset (600 LEDs)

| Preset / Modus            | Theoretisch | Tatsächlich (WLED-Limit 9 A) |
|---------------------------|-------------|-------------------------------|
| ESP32 idle                | —           | ~300 mA                       |
| Rot 10 % (Preset 22)      | ~1,2 A      | **1,2 A** (kein Eingriff)     |
| Rot 50 % (Preset 20)      | ~6,0 A      | **6,0 A** (kein Eingriff)     |
| Rot 100 % (Preset 21)     | ~12,0 A     | **→ gedimmt auf ~9 A**        |
| Weiß 10 % (Preset 25)     | ~3,6 A      | **3,6 A** (kein Eingriff)     |
| Weiß 50 % (Preset 23)     | ~18,0 A     | **→ gedimmt auf ~9 A**        |
| Weiß 100 % (Preset 24)    | ~36,0 A     | **→ gedimmt auf ~9 A**        |
| Jede zehnte (Preset 28)   | ~3,6 A      | **3,6 A** (kein Eingriff)     |
| Alle aus (Preset 26)       | 0 A         | **0 A**                       |

> **Hinweis:** Bei Weiß 50 % / 100 % greift der WLED-Stromlimiter ein
> und dimmt automatisch herunter. Die angezeigte Helligkeit ist geringer
> als eingestellt — das ist gewollt und schützt das Netzteil.

### Netzteil-Empfehlung

| Netzteil             | Für wen?                                              |
|----------------------|-------------------------------------------------------|
| **5V / 10A (50W)**   | ✅ Empfohlen — alle Rot-Presets voll, Weiß begrenzt   |
| 5V / 15A (75W)       | Weiß 50 % ohne Limit-Eingriff (≥ 9A für LEDs + ESP32)|
| 5V / 20A (100W)      | Weiß 100 % ohne Limit (nur bei sehr hellem Weißlicht) |

**Empfehlung für die Sternwarte: 5V / 10A (50W)** — für Astro-Beobachtungen
wird fast ausschließlich Rotlicht verwendet; der WLED-Limiter hält alles
im sicheren Bereich. Einkaufsliste: [shopping.md](shopping.md).
