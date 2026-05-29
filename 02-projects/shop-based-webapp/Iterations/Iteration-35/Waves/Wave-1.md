# Wave 1 - Vier Galerie-Optimierungen umsetzen

**Iteration:** [[Iteration-35]]
**Skill:** frontend-eng
**Status:** Done

## Tasks

- [x] `width`/`height` in `web/customer/src/lib/gallery.ts` an reale Disk-Dimensionen angleichen.
- [x] `web/customer/src/components/GallerySlider.tsx`: Autoplay-Guard via `IntersectionObserver` + Page Visibility API.
- [x] `web/customer/src/components/GallerySlider.tsx`: `key={current}` auf `<Image>` entfernen.
- [x] `placeholder="blur"` mit statisch generierten `blurDataURL` (per `sharp` einmalig erzeugt) fuer alle Slides.

## Done Criteria

- [x] Autoplay startet/pausiert korrekt bei Viewport- und Tab-Sichtbarkeit.
- [x] Slide-Dimensionen entsprechen den realen Disk-Massen.
- [x] Slider wechselt Bilder ohne Re-Mount der `<Image>`-Komponente.
- [x] Slides zeigen vor dem Bild-Load einen Blur-Placeholder.
- [x] TypeScript und Next-Build kompilieren ohne neue Warnungen.

## Notes

Disk-Dimensionen aus dem Audit (sharp.metadata):
- `court-net-detail.jpg`: 1920x1280 (deklariert 5472x3648)
- `floodlit-match-courts.jpg`: 1920x1440 (deklariert 4032x3024)
- `Prio4.jpg`: 1920x1440 (deklariert 4032x3024)
- `sunrise-club-overview.jpg`: 1920x2997 (deklariert 4032x6294)
- `club-at-dusk.jpg`: 1920x1440 (deklariert 4032x3024)
- `Prio3.jpg`: 1920x1440 (deklariert 4032x3024)

Die uebrigen acht Slides haben bereits korrekte Dimensionen.

`blurDataURL` wird einmalig per `sharp(...).resize(16).webp({quality:30})` als base64 generiert und statisch im Datenmodell gehalten.

Umgesetzt am 2026-05-12: 14 Blur-Placeholder zusammen 1.584 Byte. Sechs Slide-Dimensionen korrigiert (court-net-detail, floodlit-match-courts, Prio4, sunrise-club-overview, club-at-dusk, Prio3). Autoplay haengt jetzt an drei Gates: Viewport-Sichtbarkeit (IntersectionObserver, threshold 0.25), Tab-Sichtbarkeit (Page Visibility API) und User-Interaktion (Hover/Focus pausieren via `userPausedRef`). `<Image>` wechselt jetzt per src-Aenderung statt Re-Mount. Homepage First Load JS: 170 -> 172 kB (+2 kB fuer Observer-Logik und 1.5 kB inline Blur-Daten).
