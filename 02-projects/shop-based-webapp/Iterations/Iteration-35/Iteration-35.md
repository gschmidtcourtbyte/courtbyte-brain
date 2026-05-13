# Iteration 35 - Galerie-Verhalten und Wahrnehmung optimieren

**Status:** Done
**Repo:** /home/andersen/git/shop-based-webapp
**Datum:** 2026-05-12
**Rolle:** Acting as Product Owner. Planning Iteration 35 for shop-based-webapp.

## Ziel

Iteration 35 senkt Bandbreite und CPU-Last der Camp-Galerie auf der OUT-Padel-Startseite und verbessert das wahrgenommene Loading-Erlebnis. Iteration 34 hatte die Bildpipeline und Cache-TTLs adressiert; diese Iteration zielt auf das Galerie-Verhalten selbst.

## Messbefund

- Galerie liegt below the fold, ohne `priority` -> kein LCP-Faktor.
- Pro Slide werden 38-213 KB WebP transferiert (next/image transformiert die Sources korrekt).
- Autoplay alle 6 s ueber 14 Slides -> ca. 1,5 MB pro Session, auch bei verstecktem Tab oder Galerie ausserhalb des Viewports.
- 6 von 14 Slides in `lib/gallery.ts` haben falsche `width`/`height` gegenueber den realen Disk-Dimensionen (z.B. `court-net-detail.jpg` deklariert 5472x3648, real 1920x1280) -> suboptimale srcset-Auswahl.
- `<Image key={current}>` in `GallerySlider.tsx` erzwingt Re-Mount bei jedem Wechsel statt einfachen src-Wechsel.
- Kein Placeholder waehrend Bild-Load -> leerer Container sichtbar.

## Scope

**In:**
- Autoplay nur starten, wenn Galerie im Viewport ist und der Tab sichtbar ist.
- `width`/`height`-Werte in `web/customer/src/lib/gallery.ts` an die realen Disk-Dimensionen angleichen.
- `key={current}` auf `<Image>` in `GallerySlider.tsx` entfernen.
- `placeholder="blur"` mit statisch im Code vorgehaltenem `blurDataURL` fuer jeden Slide.
- Build, Lint und Smoke-Check der Galerie ausfuehren.

**Out:**
- Reduzierung der Slide-Anzahl (Content-Entscheidung).
- Vorprozessierte `<picture>`-Pipeline ohne next/image.
- Aenderung an Source-Bilddateien.
- Aenderungen an anderen Komponenten oder Routen.

## Waves

| Wave | Skill | Focus | Status |
|---|---|---|---|
| Wave 1 | frontend-eng | Vier Galerie-Optimierungen umsetzen | Done |
| Wave 2 | testing-eng | Build, Lint und Galerie-Smoke pruefen | Done |

## Critical Path

```text
Wave 1 (Galerie-Code)
    v
Wave 2 (Verification)
```

## Acceptance Criteria

- [x] Galerie startet Autoplay erst, wenn sichtbar; pausiert beim Verlassen des Viewports oder beim Tab-Wechsel.
- [x] Alle Slides in `gallery.ts` haben Disk-konforme `width`/`height` (per `sharp metadata` verifiziert).
- [x] `<Image>` ohne `key`-Prop, src-Wechsel funktioniert ohne Re-Mount.
- [x] Jeder Slide hat `placeholder="blur"` mit statischem `blurDataURL`.
- [x] `npm run build` und `npm run lint` gruen.
- [x] Smoke-Check: erste Galerie-Anzeige zeigt sofort einen Blur, kein leerer Container.

## Notes

Planung erstellt am 2026-05-12 nach dem Galerie-spezifischen Audit, das die in Iteration 34 nicht adressierten Verhaltensprobleme freigelegt hat.

Abgeschlossen am 2026-05-12. Ergebnis: Autoplay laeuft nur bei sichtbarer Galerie und sichtbarem Tab; sechs Slide-Dimensionen korrigiert; `<Image>` wechselt ohne Re-Mount; 14 inline Blur-Placeholder (1,5 KB gesamt) fuellen den Container vor dem Bild-Load. Homepage First Load JS: +2 kB. Bundle-Inspection bestaetigt korrekte Integration. Vorab-Snapshot fuer Projektplan: Letzte abgeschlossene Iteration auf 35 setzen.
