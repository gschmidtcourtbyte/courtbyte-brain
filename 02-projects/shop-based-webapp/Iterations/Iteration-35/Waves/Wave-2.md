# Wave 2 - Build, Lint und Galerie-Smoke pruefen

**Iteration:** [[Iteration-35]]
**Skill:** testing-eng
**Status:** Done

## Tasks

- [x] `npm run lint` im `web/customer`.
- [x] `npm run build` im `web/customer`.
- [x] Quellgroesse der HOMEPAGE_SLIDES vor/nach pruefen (sollte unveraendert bleiben).
- [x] HTML-Snapshot `<Image>`-Render auf gueltige `width`/`height` und Vorhandensein von `data:image/webp;base64,` (`blurDataURL`) pruefen.

## Done Criteria

- [x] Lint gruen.
- [x] Build gruen, keine neuen Warnungen.
- [x] Galerie-Slides enthalten korrekte Dimensionen und `blurDataURL`.

## Notes

`vitest run` ist im aktuellen WSL-Setup wegen root-owned `node_modules/.vite-temp` blockiert (vorbestehender Infra-Bug aus Iteration 34). Ersatzweise tsc + Build als Verifikation.

Umgesetzt am 2026-05-12: `next lint` ohne Warnungen. `next build` gruen, 26 statische Seiten generiert. Bundle-Inspection der `app/page-*.js` zeigt 14 `data:image/webp;base64`-Eintraege und je 2 `IntersectionObserver` + `visibilitychange` Tokens. Alle 14 Slide-Dimensionen in `gallery.ts` entsprechen exakt den `sharp.metadata()`-Werten der Disk-Dateien.
