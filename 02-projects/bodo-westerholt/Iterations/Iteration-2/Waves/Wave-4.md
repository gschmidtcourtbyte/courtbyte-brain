# Wave 4 - Verifikation & Politur

**Iteration:** [[Iteration-2]]
**Skill:** testing-eng (+ frontend-design fuer Politur)
**Status:** Done (2026-05-30) — **abnahmefaehig**
**Referenz:** [[CD-Manual]]
**Voraussetzung:** [[Wave-3]] abgeschlossen

## Ergebnis (2026-05-30)

Gesamturteil **abnahmefaehig**, keine Blocker (2 Agenten: `wave4-fix` Code,
`wave4-verify` Abnahme).

- **#18 StatFigure-Kontrast:** `tone`-Prop ('muted'|'inherit'); StatsBar nutzt
  gedimmten Papierweiss (`rgba(244,241,234,0.72)`). Label **3.16 -> 8.35:1 (AAA)**.
- **#19 Build/Audit:** `lint`/`build`/`tsc` gruen, 27 Routen. 60/30/10 ✅ (Rot
  nur Signal). A11y: alle CD-Kombis AA bzw. Large-Text-AA (Rot/Anthrazit nur
  fuer >=30px Display). Typo ✅ (nur Archivo + IBM Plex Mono). Responsive ✅
  (Wordmark minWidth 120 + Blocksatz @390px).
- **#20 Visuell:** 8 Screenshots (Desktop 1366 + Mobile 390), alle 4 Key-Pages
  Manual-konform. **Karriere = Anthrazit, kein Gelb** (Fix bestaetigt). HTTP 200
  auf allen 6 Marketing-Routen.

### Offene Mini-Hinweise (nicht-blockierend, Backlog)

1. **StatsBar zeigt "0" im fullPage-Screenshot** — Count-up ist scroll-getriggert
   (IntersectionObserver), feuert im synthetischen Shot nicht. Kein Defekt.
2. **Karriere-Button "Initiativbewerbung" `borderRadius:8`** statt DS-`4px`
   (`--radius-*`). Pre-existing Mikro-Inkonsistenz, optionaler Folge-Fix.
3. **`companySubline`** in site-config weiterhin offen (Wordmark leitet ab).

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

## Konkrete Befunde aus [[Wave-3]] (hier pruefen/fixen)

- **StatFigure-Label-Kontrast:** Auf der fixen Anthrazit-Flaeche der StatsBar
  rendert `StatFigure` sein Label via `MonoLabel tone="muted"` = `#6E6A60` auf
  `#1C1C1A` (~3:1, grenzwertig fuer kleines Uppercase). Sauberer Fix: optionales
  `tone="inherit"`-Prop an `StatFigure` (dann hellerer Label-Ton auf dunklem
  Grund). Im A11y-Schritt entscheiden + umsetzen.
- **Karriere-Seite visuell pruefen:** zieht jetzt `t.accent = #1C1C1A` (frueher
  Gelb-Gradient) — Anthrazit-Wirkung gegenchecken.
- **`companySubline`-Feld** (optionaler site-config-Cleanup, [[Wave-2]]/[[Wave-3]])
  — entweder hier nachziehen oder bewusst offen lassen.

## Notes

- Visuelle Abnahme ist der eigentliche Gate - Build-Gruen allein reicht nicht.
- Gefundene Polituren klein halten; groessere Befunde als Tasks fuer eine
  Folge-Wave/Iteration notieren statt hier aufzublaehen.
