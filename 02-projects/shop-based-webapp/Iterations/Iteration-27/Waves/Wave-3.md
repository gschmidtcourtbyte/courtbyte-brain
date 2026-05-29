# Wave 3 - Ueber-uns und Gallery Bilder unverzoomt und eckig

**Iteration:** [[Iteration-27]]
**Skill:** frontend-eng
**Status:** Done

## Ziel

Die Bilddarstellung aus dem Kundenfeedback wird korrigiert: "Ueber uns" und Gallery sollen wieder den unverzoomten Stand aus Iteration 25 herstellen, aber ohne runde Ecken an den betroffenen Bildflaechen.

## Agent

Frontend Engineer: Fokus auf Bilddarstellung, responsive Stabilitaet und visuelle Regression gegen Iteration 25/26.

## Tasks

- [x] `web/customer/src/components/AboutUs.tsx`: Ariadna-Bild von `object-cover` auf voll sichtbare Darstellung zurueckstellen.
- [x] `web/customer/src/components/AboutUs.tsx`: Sascha-Bild von `object-cover` auf voll sichtbare Darstellung zurueckstellen.
- [x] `web/customer/src/components/AboutUs.tsx`: manuelle `object-position` Klassen fuer diese Bilder entfernen.
- [x] `web/customer/src/components/AboutUs.tsx`: runde Ecken an den betroffenen Bildcontainern entfernen.
- [x] `web/customer/src/components/GallerySlider.tsx`: Gallery-Bilder weiter voll sichtbar/unverzoomt halten.
- [x] `web/customer/src/components/GallerySlider.tsx`: runde Ecken am betroffenen Gallery-Bildframe entfernen.
- [x] Keine Bildassets selbst veraendern, ersetzen oder neu exportieren.

## Akzeptanzkriterien

- [x] `AboutUs.tsx` enthaelt fuer Ariadna/Sascha kein `object-cover`.
- [x] `AboutUs.tsx` enthaelt fuer Ariadna/Sascha keine manuelle `object-[...]` Positionierung.
- [x] Ariadna/Sascha werden im "Ueber uns" Abschnitt voll sichtbar und nicht gecropped/gezoomt dargestellt.
- [x] Die betroffenen Ueber-uns-Bildflaechen haben keine runden Ecken.
- [x] `GallerySlider.tsx` zeigt Gallery-Bilder voll sichtbar und nicht per `object-cover` beschnitten.
- [x] Der betroffene Gallery-Bildframe hat keine runden Ecken.
- [x] Captions, Navigation und Dot-Indikatoren bleiben unveraendert nutzbar.

## Abhaengigkeiten

Keine. Diese Wave kann parallel zu Wave 1 und Wave 2 vorbereitet werden.

## Notes

Iteration 26 plante fuer `AboutUs.tsx` wieder Crop/Zoom mit softer Roundness. Das neue Feedback priorisiert die Ruecknahme: unverzoomt und keine Roundness an den betroffenen Bildern.

Umsetzung 2026-04-26:
- `AboutUs.tsx`: Ariadna und Sascha nutzen `object-contain` statt `object-cover`; manuelle `object-[...]` Positionierung entfernt.
- `AboutUs.tsx`: Rundung und `overflow-hidden` an den betroffenen Bild-/Card-Containern entfernt, damit die Bildflaechen eckig bleiben.
- `GallerySlider.tsx`: Gallery-Bild bleibt `object-contain`; Rundung am Slider-Frame entfernt.
- Navigation, Captions und Dot-Indikatoren wurden nicht funktional veraendert.
- Keine Bildassets geaendert.
- Verifiziert mit statischem Audit (`rg` auf `object-cover`, `object-[...]`, Frame-Roundness), `npm test -- --configLoader runner src/lib/gallery.test.ts` und `npm run build`.
