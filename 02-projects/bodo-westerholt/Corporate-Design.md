# Bodo Westerholt - Corporate Design

**Stand:** 2026-05-29
**Wirkt auf:** `frontend/lib/theme.ts`, `frontend/lib/site-config.ts`,
`frontend/app/globals.css`, `frontend/components/`
**Verbunden mit:** [[Projektplan]], [[Iteration-1]]

## Brand-Anker

- **Firmenname (lang):** Bodo Westerholt Unternehmensgruppe
- **Firmenname (kurz):** Bodo Westerholt
- **Branche:** Tiefbau (Erdbau, Kanalbau, Strassenbau, Rueckbau)
- **Logo:** Repo-Pfad geplant unter `frontend/public/logo-westerholt.jpg`
  (Original liegt in `Bilder/Logo Westerholt-Unternehmensgruppe_schwarz.jpg`)
  - Rote, kursive Handschrift "Bodo Westerholt"
  - Serifenlose Subzeile "UNTERNEHMENSGRUPPE"
  - Logo traegt den Brand-Akzent komplett allein - der Rest der Seite darf
    bewusst nuechtern-industriell danebenstehen statt mit ihm zu konkurrieren

## Aesthetik-Richtung: Industrial-Editorial Tiefbau

Tiefbau heisst Asphalt, Baustelle, Vermessung, DIN-Tafeln. Die Seite soll
danach aussehen - nicht nach Consulting-Stockphoto. Konsequente Form-Sprache:

- Scharfe Kanten statt Rundungen (max. 4-8 px Border-Radius)
- Dicke schwarze/dunkle Trennlinien als Bauelement
- Grosse, plakative Headlines wie gestanzte Metallschilder
- Technische Mono-Akzente fuer Projektnummern, Daten, Masse
- Keine Emoji-Icons - SVG-Strich-Icons (Bagger, Rohr, Pylon, Helm)
- Keine bunten Blob-Hintergruende - flach, gridbasiert, atmosphaerisch
- Default-Theme: **Dark (Asphalt-Grund)**; Light bleibt funktional verfuegbar

## Farbpalette

### Anker

| Rolle | Hex | Einsatz |
|---|---|---|
| Primaer (Schwarz/Asphalt) | `#1C1C1A` | Anker, Headlines, Footer, Text-auf-Hell |
| Sekundaer (Signal-Rot) | `#D81F26` | CTAs, Akzente, Marker, Links-Hover |

### Neutrale (Beton-/Kies-Skala)

| Token | Hex | Einsatz |
|---|---|---|
| Tiefschwarz | `#0B0B0A` | Dark-BG, Footer, hoechste Dichte |
| Anthrazit | `#2A2A28` | Dark-Surface |
| Grau-dunkel | `#4A4A47` | Sekundaer-Text auf Dunkel, Borders Dark |
| Grau-mittel | `#8C8C88` | Muted, Trennlinien |
| Grau-hell | `#C7C5C0` | Borders Light, Divider |
| Beton-hell | `#E8E6E1` | Light-Surface-Alt |
| Papier | `#F5F3EE` | Light-BG (warmes Off-White, kein reines Weiss) |

### Rot-Skala (Signal-System)

| Token | Hex | Einsatz |
|---|---|---|
| Rot-tief | `#A8181E` | Hover, aktiver Zustand |
| Rot-signal | `#D81F26` | = Sekundaer (Primaer-CTA) |
| Rot-hell | `#F26269` | Hinweis-Badges, Marker |

### Semantik (sparsam)

| Token | Hex | Einsatz |
|---|---|---|
| Baustellen-Gelb | `#F2C200` | Warnstreifen, Statusbalken |
| Signal-Gruen | `#2E7D3A` | Erfolg/Status |

## Typografie

| Rolle | Schrift | Quelle | Einsatz |
|---|---|---|---|
| Display | **Big Shoulders Display** (700/900) | Google Fonts | Hero-Headlines, Section-Titel, Stat-Zahlen |
| Body | **DM Sans** (400/500/700) | Google Fonts | Fliesstext, Subheadlines, UI |
| Mono | **JetBrains Mono** (500/700) | Google Fonts | Projekt-Codes, Daten, Masse, technische Labels |

Big Shoulders Display ist kondensiert und engineered - fuehlt sich an wie
gestanzte Metallschilder auf der Baustelle. DM Sans bleibt fuer Lesbarkeit
im Body. JetBrains Mono erscheint nur in technischen Akzenten
(z. B. `PROJ-2024-017`, `12.400 m3`, `DN 800`).

## Form-Sprache (globale Tokens)

- **Border-Radius:** 0 px Default (Container), 4 px Eingabefelder, 8 px Karten max.
- **Schatten:** harte Offset-Schatten statt weicher Blurs
  (z. B. `4px 4px 0 rgba(0,0,0,0.8)`)
- **Linien:** 1-2 px solid in `Anthrazit`/`Grau-hell` als Bauelement
- **Spacing:** generoes, Sektion-Padding 96-120 px Desktop
- **Layout:** 12-Spalten-Grid, asymmetrisch besetzbar, oversized Typografie
  darf bewusst ueberlappen

## Ikonografie

- Strich-Icons, 1.5-2 px stroke, schwarz oder rot
- Quelle: eigene SVGs in `frontend/components/icons.tsx`
- Motiv-Sprache aus Tiefbau: Bagger, Rohr, Vermessungspegel, Pylon, Helm,
  Schaufel, Kanaldeckel, Strasse
- **Keine Emoji** im Code, kein Material-Icon-Set

## Fotografie

- Echte Baustellen, eigene Maschinen, Personen am Geraet
  (sobald Kundenmaterial geliefert)
- Bis dahin: kuratierte freie Baustellen-Placeholder, klar als Stub markiert
- Bildbehandlung: leicht entsaettigt, hoher Kontrast - keine glatten
  Beauty-Shots

## Theme-Mapping (Verbindung zu `theme.ts`)

```
DARK (Default)
  bg          #0B0B0A   Tiefschwarz
  surface     #1C1C1A   Primaer
  surfaceAlt  #2A2A28   Anthrazit
  border      #4A4A47   Grau-dunkel
  text        #F5F3EE   Papier
  textMuted   #8C8C88   Grau-mittel
  primary     #D81F26   Signal-Rot (Akzent-Rolle)
  accent      #F2C200   Baustellen-Gelb (sehr sparsam)
  footerBg    #0B0B0A
  footerText  #8C8C88

LIGHT
  bg          #F5F3EE   Papier
  surface     #FFFFFF   Reines Weiss
  surfaceAlt  #E8E6E1   Beton-hell
  border      #C7C5C0   Grau-hell
  text        #1C1C1A   Primaer
  textMuted   #4A4A47   Grau-dunkel
  primary     #D81F26   Signal-Rot
  accent      #1C1C1A   Schwarz als Sekundaer-Akzent
  footerBg    #1C1C1A
  footerText  #C7C5C0
```

Hinweis: Im Theme-Modell aus dem Template uebernimmt `primary` die Rolle der
Hauptakzentfarbe (CTA, Link, Hover). Da `#1C1C1A` als Anker eher die
"Textur" stellt und Rot die "Spannung" bringt, wird in beiden Modes **Rot
als `primary`** gemappt - der schwarze Anker wirkt ueber Text, Surfaces
und Footer.

## Verwendung in der Praxis

- **Wenn etwas signalisieren soll:** Rot (CTA, Hover, Marker)
- **Wenn etwas Gewicht braucht:** Schwarz/Anthrazit (Headlines, Footer)
- **Wenn etwas "Baustellen"-Vibe braucht:** Mono + Gelb sparsam
- **Wenn unsicher:** weniger Farbe, mehr Schwarz/Papier, klare Linien

## Was nicht passt

- Lila/violette Gradients, Pastellblau, helle Orange-Toene
- Runde Pillen-Buttons, weiche Drop-Shadows
- Emoji-Icons (kein 🏆, 🤝, ⚙️)
- Stock-Beauty-Shots, generische Consulting-Bilder
- Inter / Roboto / system-ui als Hauptschrift
