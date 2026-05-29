# Wave 1 - Theme-Foundation, Logo, Fonts, Form-Sprache

**Iteration:** [[Iteration-1]]
**Skill:** frontend-eng
**Status:** Done (2026-05-29)
**Referenz:** [[Corporate-Design]]

## Tasks

- [x] Logo aus `Bilder/Logo Westerholt-Unternehmensgruppe_schwarz.jpg` nach
      `frontend/public/logo-westerholt.jpg` kopiert. Dark-Theme-Loesung:
      Logo wird in einem weissen "Schild"-Container (Logo-Card) gezeigt -
      passt zur Industrial-Editorial-Richtung, keine invertierte Variante
      noetig
- [x] `frontend/lib/theme.ts`: Palette auf Corporate-Design umgestellt
      (Schwarz/Rot/Beton-Skala), `DEFAULT_PRIMARY = '#D81F26'`,
      `DEFAULT_ACCENT = '#F2C200'`, `DEFAULT_THEME_MODE = 'dark'`,
      `BASE_THEMES.dark/.light` neu, Blob-Tokens auf SurfaceAlt entwertet,
      `shadow`/`hoverShadow` auf harten Offset-Schatten umgestellt
- [x] `frontend/lib/site-config.ts`: `companyName`, neues Feld
      `companyNameLong`, `logoInitials: 'BW'`, neues Feld `logoSrc`,
      Tiefbau-Tagline, Kontaktwege mit TODO-Markierung als Platzhalter
- [x] `frontend/app/layout.tsx`: DM Sans, Big Shoulders Display (700/900)
      und JetBrains Mono (500/700) via `next/font/google` geladen,
      CSS-Variablen `--font-body`, `--font-display`, `--font-mono`. Alter
      `<link>` zu Google Fonts entfernt
- [x] `frontend/app/globals.css`: Form-Sprache-Tokens
      (`--radius-card: 4px`, `--shadow-hard`, `--shadow-hard-sm`,
      `--border-width`), Utility-Klassen `.display` und `.mono`,
      Hover-Schatten der btn-primary auf hartem Offset
- [x] `frontend/components/Logo.tsx`: next/image-Variante in weissem
      Card-Container (h=36 px), Initialen-Fallback bleibt erhalten und
      nutzt jetzt Display-Font

## Done Criteria

- [x] `npm run build` sauber (27 Routen statisch generiert)
- [x] `npm run lint` sauber (keine Warnings)
- [x] `npm run typecheck` sauber
- [x] Keine generischen Template-Farben (Petrol `#2B7A9F` / Orange `#E8834A`)
      mehr in `theme.ts`
- [ ] **Offen: visuelle Sichtkontrolle** — `npm run dev` und Hero/Header/
      Footer am Browser pruefen. Wave 1 hat nur Tokens und Logo angefasst;
      Hero/Navbar/Footer ziehen die neuen Tokens automatisch, aber die
      Komponenten haben weiterhin Template-Layout (perspective tilt, Blobs,
      Emoji-Badges) - das wird in [[Wave-2]] aufgeloest

## Notes

Diese Wave veraendert nur Tokens, Konfiguration und das `Logo`-Bauteil.
Komponenten-Logik wird hier nicht angefasst - dafuer ist [[Wave-2]] da. Wenn
beim Anpassen auffaellt, dass eine Komponente Werte hardcoded hat (statt
sie aus dem Theme zu lesen), als Befund notieren und in Wave 2 sammeln,
nicht hier patchen.
