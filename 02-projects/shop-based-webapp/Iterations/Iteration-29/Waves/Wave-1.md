# Wave 1 - AboutUs Founder-Bildboxen angleichen

**Iteration:** [[Iteration-29]]
**Skill:** frontend-eng
**Status:** Done

## Ziel

Ariadna und Sascha erhalten im Homepage-Bereich "Ueber uns" dieselbe sichtbare Bildbox. Beide Bilder sollen horizontal und vertikal gleich gross wirken und dieselben responsiven Constraints verwenden.

## Agent

Frontend Engineer: Fokus auf Komponentenstruktur, responsive Bildcontainer, konsistente CSS-Klassen und Erhalt des bestehenden OUT Padel Designs.

## Tasks

- [x] `web/customer/src/components/AboutUs.tsx` analysieren: aktuelle Founder-Bildstruktur, Wrapper-Klassen, `Image`-Props und responsive Layoutregeln fuer Ariadna/Sascha erfassen.
- [x] Eine gemeinsame Bildbox-Logik fuer Ariadna und Sascha definieren: gleiche Breite, gleiche Hoehe, gleiches Seitenverhaeltnis und gleiche Overflow-/Object-Fit-Regeln.
- [x] Ariadna und Sascha in `AboutUs.tsx` auf diese gemeinsame Bildbox-Logik umstellen.
- [x] Personen-spezifische Groessen-, Hoehen- oder Aspect-Unterschiede entfernen, sofern sie die sichtbare Gleichheit verhindern.
- [x] Sicherstellen, dass Copy, Reihenfolge, Kartenlayout und CTAs unveraendert bleiben.
- [x] Responsive Verhalten fuer Desktop, Tablet und Mobile beruecksichtigen.
- [x] Keine Bildassets veraendern, ersetzen oder neu exportieren.

## Akzeptanzkriterien

- [x] Ariadnas und Saschas Bildboxen in "Ueber uns" nutzen dieselbe gerenderte Breite innerhalb desselben Breakpoints.
- [x] Ariadnas und Saschas Bildboxen in "Ueber uns" nutzen dieselbe gerenderte Hoehe innerhalb desselben Breakpoints.
- [x] Ariadnas Bild wirkt nicht mehr laenger als Saschas.
- [x] Die oberen und unteren Bildkanten wirken innerhalb der Karten konsistent ausgerichtet.
- [x] Mobile Stack-Layouts bleiben stabil und ohne Textueberlagerungen.
- [x] Es gibt keinen Diff an Bildassets oder nicht betroffenen Bereichen.

## Abhaengigkeiten

Keine. Diese Wave definiert die Ziel-Umsetzung fuer die Founder-Bildboxen.

## Notes

Iteration 28 hat bereits an den Founder-Bildern gearbeitet. Diese Wave priorisiert jetzt messbar gleiche sichtbare Bildflaechen gegenueber der reinen Frage, ob Bilder unverzoomt oder boxbreit sind.

Umsetzung 2026-04-26:
- `FounderImageFrame.tsx` als gemeinsame Bildbox fuer Founder-Bilder eingefuehrt.
- `AboutUs.tsx` nutzt fuer Ariadna und Sascha denselben Frame mit fixer `aspectRatio: "2 / 3"`, `w-full`, `overflow-hidden` und `Image fill`.
- Die vorherigen natuerlichen `width`/`height`-Props in `AboutUs.tsx` bestimmen die sichtbare Boxgroesse nicht mehr.
- Copy, Reihenfolge, Kartenlayout und Bildassets blieben unveraendert.
