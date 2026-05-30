# Iteration 2 - CD-Manual-Alignment (Brand-System scharfstellen)

**Status:** Abgeschlossen (2026-05-30) — abnahmefaehig
**Repo:** bodo-westerholt (Kunden-Clone)
**Datum:** 2026-05-30 (Start)
**Referenz:** [[CD-Manual]] (verbindlich) - praezisiert [[Corporate-Design]]

## Ziel

Das gelieferte Corporate Design Manual (Ausgabe 01 / 2026) wird zum echten
Design-System der Live-Website. Iteration 1 hatte die Marke auf Basis einer
Interim-Festlegung gesetzt; das Manual korrigiert Schrift, Akzentfarbe,
Neutralskala und fuehrt Wortmarke + "Die Schraege" als Signatur ein. Das
Manual selbst wird **nicht** als Seite veroeffentlicht - nur seine Vorgaben
werden umgesetzt.

## Scope

**In:**
- Schrift-Tausch: Archivo (statt Big Shoulders + DM Sans) + IBM Plex Mono
  (statt JetBrains Mono)
- Farbkorrektur: exakte Beton-Neutralskala, **Gelb-Akzent entfernen**
  (strikt Rot + Anthrazit)
- Neue generische Marken-Primitives: `Wordmark`, `SignalStripe`,
  `DiagonalClip`, `MonoLabel`, `StatFigure`
- Anwendung auf Bestandskomponenten (Navbar, Footer, Hero, StatsBar,
  Sections, Kontakt) + Blobs entfernen
- 60/30/10-Flaechenruhe, Mono fuer Kontakt/Zahlen/Labels
- Build/Lint/Visual-Verifikation gegen das Manual

**Out:**
- Manual als eigene Route/Seite (ausdruecklich nicht gewuenscht)
- Admin-/CMS-Bereich (`/admin`) - Dark-only, kein CD-Manual-Scope
- Backend/Deployment (Iteration spaeter)
- Finale Kundenbilder (warten auf Lieferung)

## Waves

| Wave | Skill | Focus | Status |
|---|---|---|---|
| [[Wave-1]] | frontend-design | Tokens: Fonts, exakte Farben, Gelb raus, Schraegen-Tokens | Done |
| [[Wave-2]] | frontend-design | Marken-Primitives (Wordmark, SignalStripe, DiagonalClip, MonoLabel, StatFigure) | Done |
| [[Wave-3]] | frontend-design | Anwendung auf Seiten/Komponenten, Blobs raus, Mono-Daten | Done |
| [[Wave-4]] | testing-eng | Build/Lint/Visual-Verifikation, 60/30/10- & A11y-Audit | Done |

> Hinweis: Auf Kundenwunsch wird in allen Build-Waves der Skill
> **frontend-design** statt frontend-eng verwendet.

## Abhaengigkeiten

Wave 1 -> 2 -> 3 sequenziell (Tokens vor Primitives vor Anwendung).
Wave 4 nach Wave 3. Innerhalb Wave 2 koennen die 5 Primitives parallel
gebaut werden.

## Acceptance Criteria

- [x] `layout.tsx` laedt Archivo (400-900 + italic) + IBM Plex Mono; Big
      Shoulders / DM Sans / JetBrains Mono entfernt
- [x] `theme.ts` traegt exakte Beton-Neutralskala aus [[CD-Manual]];
      `DEFAULT_ACCENT` (Gelb) entfernt, Akzent = Anthrazit
- [x] Kein `#F2C200`-Akzent mehr in Theme/Tweaks (Warnschutz-Gelb nur falls
      explizit funktional)
- [x] `Wordmark`-Komponente: Archivo Black italic, 12 Grad, Blocksatz-Sublinie,
      4 Farbvarianten, aus `site-config` gespeist
- [x] `SignalStripe` + `DiagonalClip` als wiederverwendbare Primitives
- [x] Eyebrows/Labels in IBM Plex Mono, Kontaktdaten in Mono
- [x] `Blobs` aus dem Marketing-Layout entfernt
- [x] `npm run build` + `npm run lint` sauber (TS strict)
- [x] Visuelle Abnahme: Home/Ueber-uns/Karriere/Kontakt entsprechen dem Manual

## Notes

- Goldene Regel gilt: alle Primitives generisch, gespeist aus `site-config` /
  `theme.ts` - nichts Kundenspezifisches in Komponenten.
- Nach Abschluss: [[Corporate-Design]] Typografie- + Akzent-Abschnitt auf das
  Manual nachziehen (oder als ueberholt markieren), damit das Brain konsistent
  bleibt.
