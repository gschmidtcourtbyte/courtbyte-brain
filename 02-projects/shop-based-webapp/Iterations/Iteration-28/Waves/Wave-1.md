# Wave 1 - Gallery boxbreit ohne Zoom

**Iteration:** [[Iteration-28]]
**Skill:** frontend-eng
**Status:** Done

## Ziel

Die Gallery-Bilder gehen wieder ueber die gesamte verfuegbare Boxbreite, ohne dass Motive durch Crop, Zoom oder manuelle Ausschnittsteuerung abgeschnitten werden.

## Agent

Frontend Engineer: Fokus auf Slider-Bilddarstellung, responsive Stabilitaet und Erhalt der vorhandenen Gallery-Interaktionen.

## Tasks

- [x] `web/customer/src/components/GallerySlider.tsx` analysieren: aktuelles Zusammenspiel aus Frame, `Image`, Bildgroessen, Captions, Navigation und Dots erfassen.
- [x] Gallery-Bilder so anpassen, dass sie die verfuegbare Boxbreite nutzen.
- [x] Sicherstellen, dass das sichtbare Slide-Bild nicht per `object-cover`, manueller `object-position` oder Scale-/Hover-Zoom beschnitten wird.
- [x] Responsive Constraints fuer Desktop und Mobile pruefen, damit unterschiedliche Bildformate keine Layout-Spruenge erzeugen.
- [x] Captions, Pfeilnavigation und Dot-Indikatoren unveraendert nutzbar halten.
- [x] Keine Bildassets veraendern, ersetzen oder neu exportieren.

## Akzeptanzkriterien

- [x] Gallery-Bilder nutzen die Breite des vorhandenen Gallery-Frames.
- [x] Hoch- und Querformatbilder werden nicht hineingezoomt oder abgeschnitten.
- [x] `GallerySlider.tsx` enthaelt fuer das sichtbare Slide-Bild kein `object-cover`.
- [x] Es gibt fuer das sichtbare Slide-Bild keine manuelle `object-[...]` Positionierung.
- [x] Es gibt keinen neuen Scale-/Hover-Zoom fuer die betroffenen Gallery-Bilder.
- [x] Captions, Pfeile und Dots bleiben bedienbar und ueberdecken keine wichtigen Motive unzulaessig.

## Abhaengigkeiten

Keine. Diese Wave kann unabhaengig von Wave 2 und Wave 3 umgesetzt werden.

## Notes

Das Ziel ist keine Rueckkehr zu Crop, sondern eine Praezisierung von Iteration 25/27: Die Gallery soll breiter/boxfuellender wirken, aber die Bildinhalte muessen vollstaendig sichtbar bleiben.

Umsetzung 2026-04-26:
- `GallerySlider.tsx`: sichtbares Slide-Bild von `fill` in festem Aspect-Frame auf normales `Image` mit `width`/`height` und `block h-auto w-full` umgestellt.
- `GallerySlide` und `HOMEPAGE_SLIDES`: echte Bilddimensionen ergaenzt, damit die Slides boxbreit ohne Crop gerendert werden koennen.
- Fallback-Zustand behaelt stabile Mindesthoehen; Captions, Pfeile und Dots blieben funktional unveraendert.
- Keine Bildassets geaendert.
