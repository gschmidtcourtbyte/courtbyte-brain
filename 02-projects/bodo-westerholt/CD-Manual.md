# Bodo Westerholt - Corporate Design Manual (Ausgabe 01 / 2026)

**Stand:** 2026-05-30
**Quelle:** `cd-manual/Corporate Design Manual (1).html` (17-seitiges Markenhandbuch im Code-Repo)
**Status:** **Verbindlich** - loest die Interim-Richtung in [[Corporate-Design]]
(Typografie + Akzentfarbe) ab.
**Wirkt auf:** `frontend/lib/theme.ts`, `frontend/app/layout.tsx`,
`frontend/app/globals.css`, `frontend/components/`
**Umgesetzt durch:** [[Iteration-2]]

## Warum diese Notiz

Das gelieferte Markenhandbuch praezisiert die fruehere Interim-Festlegung aus
[[Corporate-Design]]. Wo sich beide widersprechen, **gilt das Manual**:

| Aspekt | Interim ([[Corporate-Design]]) | **Manual (verbindlich)** |
|---|---|---|
| Display/Body-Schrift | Big Shoulders Display + DM Sans | **Archivo** (eine Familie, 400-900 + italic) |
| Technische Schrift | JetBrains Mono | **IBM Plex Mono** |
| Akzentfarbe | + Baustellen-Gelb `#F2C200` | **Nur Rot + Anthrazit** ("Zwei Farben tragen die Marke") |
| Wortmarke | Bild-Logo allein | **Typografische Wortmarke** (Archivo Black italic, 12 Grad) |
| Signatur-Element | - | **"Die Schraege"** (12 Grad: Kante, Bildschnitt, Signalband) |

Gelb bleibt ausschliesslich **funktional** (Warnschutz EN ISO 20471) und ist
keine Markenfarbe.

## Markenwerte

bodenstaendig - verlaesslich - regional - praezise. "Ein Handschlag, der haelt."

## Farbpalette (exakt aus dem Manual)

### Primaer - "Zwei Farben tragen die Marke"

| Rolle | Hex | RAL | Pantone | Einsatz |
|---|---|---|---|---|
| Westerholt Rot | `#D81F26` | 3020 Verkehrsrot | 485 C | Akzent/Signal - nie Flaeche im Uebermass |
| Anthrazit | `#1C1C1A` | 9011 Graphitschwarz | Black 6 C | Schrift, Konstruktion, ruhige Flaechen |

### Neutrale (Beton-/Kies-Skala)

| Token | Hex | Einsatz |
|---|---|---|
| Papierweiss | `#F4F1EA` | Flaeche, Light-BG |
| Beton Hell | `#DEDAD1` | Surface-Alt, Linien |
| Beton | `#9C988D` | Muted, Trennlinien |
| Beton Dunkel | `#6E6A60` | Sekundaer-Text |

### Mischverhaeltnis

**60 / 30 / 10** - 60 % Neutral (Flaechenruhe), 30 % Anthrazit (Struktur),
10 % Rot (pointierter Akzent, "Signal, nicht Teppich").

## Typografie

| Rolle | Schrift | Schnitte | Einsatz |
|---|---|---|---|
| Display + Body | **Archivo** | 400 Regular -> 900 Black, + italic | Headlines, Subline, Fliesstext |
| Technisch | **IBM Plex Mono** | 400/500/600 | Masse, Datenpunkte, Kontakt, Auszeichnungen, Labels |

Archivo ist eine seriose Grotesk mit aufrechter Statik - ruhiger Gegenpol zur
dynamischen Wortmarke. Mono erscheint nur in technischen Akzenten
(`04402 8698-0`, `12.400 m3`, `KG / M3 / T`).

## Wortmarke

- Schriftschnitt: **Archivo Black, kursiv**
- Neigungswinkel: **konstant 12 Grad**
- Sublinie "Unternehmensgruppe": **Archivo SemiBold, gesperrt im Blocksatz auf
  Namenbreite** (Buchstaben edge-to-edge auf Breite von "Bodo Westerholt")
- Schutzraum: **X = Hoehe des Versal-"B"** ringsum frei
- Mindestgroesse: Print 32 mm / Digital **120 px** Breite

### Farbvarianten

1. **Vorzug:** Rot ("Bodo Westerholt") + Anthrazit (Sublinie) auf Weiss/Papier
2. **Negativ:** auf Anthrazit - Rot + Weiss
3. **Einfarbig Weiss:** auf Rot
4. **Einfarbig Schwarz:** Stempel/Fax

### Don'ts

Neigung aendern/aufrichten - Fremdfarben - verzerren/stauchen - auf unruhigem
Bild ohne Flaeche stellen.

## Gestaltungselement - "Die Schraege"

Die 12-Grad-Neigung der Wortmarke wird zum verbindenden Element:

- **Bildkante** in 12 Grad geschnitten - nie waagerecht beschnitten (diag-clip)
- **Signalband** Rot/Anthrazit (Warnstreifen-Muster) fuer Kanten und Absperrung
- **Akzentlinie** in 12 Grad unter Inhalten
- Duoton-Bilder moeglich

## Bildsprache

Echte Baustellen, Menschen, Material, Tageslicht. Hoher Kontrast, kein
Stock-Beauty. (Deckt sich mit [[Corporate-Design]].)

## Frontend-Mapping (Verbindung zu `theme.ts`)

```
LIGHT
  bg          #F4F1EA   Papierweiss
  surfaceAlt  #DEDAD1   Beton Hell
  border      #DEDAD1 / #9C988D
  textMuted   #6E6A60   Beton Dunkel
  text        #1C1C1A   Anthrazit
  primary     #D81F26   Westerholt Rot
  accent      #1C1C1A   Anthrazit (KEIN Gelb)

DARK
  bg          #0B0B0A   Tiefschwarz
  surface     #1C1C1A   Anthrazit
  text        #F4F1EA   Papierweiss
  primary     #D81F26   Westerholt Rot
  accent      #1C1C1A   Anthrazit
```

## Asset-Hinweis

Das Manual-HTML referenziert `assets/brand.css`, `assets/logo-original.jpg` -
diese liegen **nicht** im `cd-manual/`-Ordner (nur die HTML). Die Wortmarken-
und Schraegen-CSS muss aus den **Inline-Styles** der HTML rekonstruiert werden;
alle Werte (12 Grad, Archivo Black italic, Blocksatz-Sublinie, Stripe-Band)
stehen vollstaendig im Dokument.
