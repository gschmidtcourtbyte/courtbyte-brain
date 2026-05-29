# Wave 2 - Marken-Primitives (generische Komponenten)

**Iteration:** [[Iteration-2]]
**Skill:** frontend-design
**Status:** Geplant
**Referenz:** [[CD-Manual]]
**Voraussetzung:** [[Wave-1]] (Tokens) abgeschlossen

## Zweck

Der kreative Kern: die wiederverwendbaren Marken-Bausteine aus dem Manual
bauen. Alle aus `site-config` / `theme.ts` gespeist - **nichts
Kundenspezifisches hartcodiert**. Eine Komponente pro Datei, PascalCase.

## Tasks

- [ ] `frontend/components/Wordmark.tsx` - typografische Wortmarke:
      "Bodo Westerholt" in Archivo Black italic, **12 Grad** Neigung; Sublinie
      "Unternehmensgruppe" in Archivo SemiBold, **gesperrt im Blocksatz auf
      Namenbreite** (Buchstaben edge-to-edge - vgl. Manual-Script). Props:
      `variant: 'default' | 'negativ' | 'onRed' | 'mono'`, `size`. Namen aus
      `siteConfig.companyName` / `companyNameLong`. Schutzraum (X = B-Hoehe)
      als Padding-Logik.
- [ ] `frontend/components/SignalStripe.tsx` - Rot/Anthrazit-Warnband
      (Repeating-Gradient aus [[Wave-1]]-Token). Props: `orientation`,
      `thickness`. Als Kanten-/Trennelement.
- [ ] `frontend/components/DiagonalClip.tsx` - 12-Grad-Bild-/Section-Schnitt-
      Wrapper (`clip-path`/skew). Props: `children`, `side`.
- [ ] `frontend/components/MonoLabel.tsx` - Eyebrow/Cap-Primitive in IBM Plex
      Mono, uppercase, letter-spaced (ersetzt heutige Ad-hoc-Eyebrows).
- [ ] `frontend/components/StatFigure.tsx` - grosse italic-Black-Rot-Zahl
      (Manual "75") + Mono-Einheit/Label. Props: `value`, `unit`, `label`.

## Done Criteria

- [ ] `npm run build` + `npm run lint` sauber (TS strict)
- [ ] Jede Komponente isoliert renderbar, keine Hardcodes (Namen/Farben aus
      config/theme)
- [ ] Wordmark deckt alle 4 Farbvarianten des Manuals ab
- [ ] Blocksatz-Sublinie sitzt korrekt auf Namenbreite (responsive)

## Notes

- Die 4 Primitives sind unabhaengig -> parallelisierbar auf mehrere Agenten.
- Wordmark ist der heikelste Teil (Blocksatz-Logik). Manual-Inline-Script
  (Buchstaben einzeln in Spans, `justify-content: space-between`) als Vorlage.
- Noch **nicht** in Bestandsseiten einbauen - das ist [[Wave-3]]. Hier nur die
  Bausteine + ggf. eine interne Vorschau/Story.
