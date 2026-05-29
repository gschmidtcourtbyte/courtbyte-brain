# Wave 1 - Design-Tokens & Foundation

**Iteration:** [[Iteration-2]]
**Skill:** frontend-design
**Status:** Done (2026-05-30)
**Referenz:** [[CD-Manual]]

## Zweck

Reines Token-/Config-Layer: Schrift, Farbe und Form-Tokens auf das Manual
ziehen. **Keine** Komponenten-Logik anfassen - das ist [[Wave-2]] / [[Wave-3]].

## Tasks

- [x] `frontend/app/layout.tsx`: `Archivo` (`weight: 400/500/600/700/900`,
      `style: ['normal','italic']`) und `IBM_Plex_Mono` (400/500/600) via
      `next/font/google`. `DM_Sans`, `Big_Shoulders_Display`, `JetBrains_Mono`
      entfernen. CSS-Variablen `--font-body` + `--font-display` -> Archivo,
      `--font-mono` -> IBM Plex Mono.
- [x] `frontend/app/globals.css`: `--font-body`/`--font-display`/`--font-mono`
      auf neue Familien. Neue Tokens: `--skew: -12deg`, Signal-Stripe-Gradient
      (Rot/Anthrazit, 45 Grad Repeating-Linear-Gradient) und Diag-Clip
      `clip-path` als wiederverwendbare Custom-Properties. `.display`/`.mono`
      Utility beibehalten.
- [x] `frontend/lib/theme.ts`: Light-Neutralskala auf exakte Manual-Werte -
      `bg #F4F1EA` (Papierweiss), `surfaceAlt #DEDAD1` (Beton Hell),
      `border #DEDAD1`/`#9C988D`, `textMuted #6E6A60` (Beton Dunkel).
      `DEFAULT_ACCENT` (`#F2C200`) **entfernen**; `accent` = `#1C1C1A`
      (Anthrazit) in beiden Modes. `buildTheme`-Signatur ggf. anpassen, falls
      `accent` nicht mehr als Parameter durchgereicht wird.
- [x] `frontend/lib/tweaks/*`: Gelb-Akzent-Regler aus dem Tweaks-Panel-State
      entfernen (kein toter Regler). Falls `accent` dort konfigurierbar war,
      auf Anthrazit defaulten.
- [x] Repo nach `#F2C200` / `Baustellen-Gelb` durchsuchen und verbliebene
      Akzent-Referenzen bereinigen (Warnschutz-Gelb nur belassen, wenn
      explizit funktional/Sicherheit).

## Done Criteria

- [x] `npm run build` sauber (Compiled successfully, 27 Routes)
- [x] `npm run lint` sauber (TS strict, keine Warnings)
- [x] Kein `Big_Shoulders_Display` / `DM_Sans` / `JetBrains_Mono`-Import mehr
- [x] Kein `#F2C200`-Akzent mehr in `theme.ts` / Tweaks (nur ADMIN_THEME.warning
      `#F59E0B` bleibt — funktionaler Admin-Status, off-limits)
- [x] Light-BG ist `#F4F1EA`, Neutralskala = Beton-Werte aus [[CD-Manual]]

## Ergebnis (2026-05-30)

Geaendert: `app/layout.tsx`, `app/globals.css`, `lib/theme.ts`,
`lib/template-config.ts`, `lib/tweaks/TweaksProvider.tsx`,
`lib/tweaks/TweaksPanel.tsx`. `buildTheme`-Signatur auf `(mode, primary)`
reduziert; `accent` ist jetzt fester `BRAND_ACCENT = '#1C1C1A'` in beiden Modes.
Befunde fuer [[Wave-3]] unten dokumentiert.

## Notes

- Diese Wave aendert nur Tokens + Config. Komponenten ziehen die neuen Tokens
  automatisch, sehen aber optisch noch "alt" aus (kein Wordmark, keine
  Schraege) - das loesen [[Wave-2]] + [[Wave-3]].
- Wenn beim Anpassen auffaellt, dass eine Komponente Schrift/Farbe hardcoded
  statt aus Token/Theme liest: als Befund fuer [[Wave-3]] notieren, nicht hier
  patchen.
- Archivo Black italic (900) ist die Basis fuer die Wortmarke in [[Wave-2]] -
  sicherstellen, dass Weight 900 + italic wirklich geladen werden.
