# Wave 4 - Verifikation & Politur

**Iteration:** [[Iteration-2]]
**Skill:** testing-eng (+ frontend-design fuer Politur)
**Status:** Geplant
**Referenz:** [[CD-Manual]]
**Voraussetzung:** [[Wave-3]] abgeschlossen

## Zweck

Sicherstellen, dass das umgesetzte System dem Manual entspricht - technisch
(Build/Lint) und visuell (Manual-Abgleich).

## Tasks

- [ ] `npm run build` + `npm run lint` (+ typecheck) gruen.
- [ ] App starten (`npm run dev`), Key-Pages screenshoten und gegen das Manual
      pruefen: Home, Ueber-uns, Karriere, Kontakt.
- [ ] **60/30/10-Flaechenaudit:** Rot bleibt Signal (~10 %), nicht Teppich.
- [ ] **Typo-Check:** Archivo ueberall, IBM Plex Mono nur in technischen
      Akzenten; kein Rest von Big Shoulders / DM Sans / JetBrains Mono.
- [ ] **A11y/Kontrast:** Rot-auf-Anthrazit, Wordmark-Varianten, Fokus-States;
      Warnschutz-Ausnahme dokumentieren.
- [ ] **Responsive:** `lib/responsive.ts`-Breakpoints; Wordmark-Blocksatz +
      Mindestbreite 120 px auf Mobile.

## Done Criteria

- [ ] Build/Lint/Typecheck sauber
- [ ] Screenshots der 4 Key-Pages im Brain/PR abgelegt, Abweichungen vom Manual
      notiert oder behoben
- [ ] [[Iteration-2]] Acceptance Criteria alle abgehakt
- [ ] [[Corporate-Design]] Typografie-/Akzent-Abschnitt auf das Manual
      nachgezogen oder als ueberholt markiert (Brain-Konsistenz)

## Notes

- Visuelle Abnahme ist der eigentliche Gate - Build-Gruen allein reicht nicht.
- Gefundene Polituren klein halten; groessere Befunde als Tasks fuer eine
  Folge-Wave/Iteration notieren statt hier aufzublaehen.
