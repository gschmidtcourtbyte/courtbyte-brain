# Wave 1 - Design-Tokens & Foundation

**Iteration:** [[Iteration-2]]
**Skill:** frontend-design
**Status:** In Umsetzung (2026-05-30)
**Referenz:** [[CD-Manual]]

## Zweck

Reines Token-/Config-Layer: Schrift, Farbe und Form-Tokens auf das Manual
ziehen. **Keine** Komponenten-Logik anfassen - das ist [[Wave-2]] / [[Wave-3]].

## Tasks

- [ ] `frontend/app/layout.tsx`: `Archivo` (`weight: 400/500/600/700/900`,
      `style: ['normal','italic']`) und `IBM_Plex_Mono` (400/500/600) via
      `next/font/google`. `DM_Sans`, `Big_Shoulders_Display`, `JetBrains_Mono`
      entfernen. CSS-Variablen `--font-body` + `--font-display` -> Archivo,
      `--font-mono` -> IBM Plex Mono.
- [ ] `frontend/app/globals.css`: `--font-body`/`--font-display`/`--font-mono`
      auf neue Familien. Neue Tokens: `--skew: -12deg`, Signal-Stripe-Gradient
      (Rot/Anthrazit, 45 Grad Repeating-Linear-Gradient) und Diag-Clip
      `clip-path` als wiederverwendbare Custom-Properties. `.display`/`.mono`
      Utility beibehalten.
- [ ] `frontend/lib/theme.ts`: Light-Neutralskala auf exakte Manual-Werte -
      `bg #F4F1EA` (Papierweiss), `surfaceAlt #DEDAD1` (Beton Hell),
      `border #DEDAD1`/`#9C988D`, `textMuted #6E6A60` (Beton Dunkel).
      `DEFAULT_ACCENT` (`#F2C200`) **entfernen**; `accent` = `#1C1C1A`
      (Anthrazit) in beiden Modes. `buildTheme`-Signatur ggf. anpassen, falls
      `accent` nicht mehr als Parameter durchgereicht wird.
- [ ] `frontend/lib/tweaks/*`: Gelb-Akzent-Regler aus dem Tweaks-Panel-State
      entfernen (kein toter Regler). Falls `accent` dort konfigurierbar war,
      auf Anthrazit defaulten.
- [ ] Repo nach `#F2C200` / `Baustellen-Gelb` durchsuchen und verbliebene
      Akzent-Referenzen bereinigen (Warnschutz-Gelb nur belassen, wenn
      explizit funktional/Sicherheit).

## Done Criteria

- [ ] `npm run build` sauber
- [ ] `npm run lint` sauber (TS strict)
- [ ] Kein `Big_Shoulders_Display` / `DM_Sans` / `JetBrains_Mono`-Import mehr
- [ ] Kein `#F2C200`-Akzent mehr in `theme.ts` / Tweaks
- [ ] Light-BG ist `#F4F1EA`, Neutralskala = Beton-Werte aus [[CD-Manual]]

## Notes

- Diese Wave aendert nur Tokens + Config. Komponenten ziehen die neuen Tokens
  automatisch, sehen aber optisch noch "alt" aus (kein Wordmark, keine
  Schraege) - das loesen [[Wave-2]] + [[Wave-3]].
- Wenn beim Anpassen auffaellt, dass eine Komponente Schrift/Farbe hardcoded
  statt aus Token/Theme liest: als Befund fuer [[Wave-3]] notieren, nicht hier
  patchen.
- Archivo Black italic (900) ist die Basis fuer die Wortmarke in [[Wave-2]] -
  sicherstellen, dass Weight 900 + italic wirklich geladen werden.
