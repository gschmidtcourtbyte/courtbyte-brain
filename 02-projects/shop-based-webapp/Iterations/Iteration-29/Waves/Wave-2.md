# Wave 2 - Warum-wir Founder-Bildboxen angleichen

**Iteration:** [[Iteration-29]]
**Skill:** frontend-eng
**Status:** Done

## Ziel

Die Ariadna- und Sascha-Bilder auf `/warum-wir` werden konsistent zur finalen AboutUs-Umsetzung aus Wave 1 angeglichen. Beide Bilder sollen in diesem Bereich dieselbe sichtbare Breite und Hoehe haben.

## Agent

Frontend Engineer: Fokus auf Seitenlayout, konsistente Wiederverwendung der Founder-Bildlogik und responsive Stabilitaet auf `/warum-wir`.

## Tasks

- [x] `web/customer/src/app/warum-wir/page.tsx` analysieren: aktuelle Founder-Bildstruktur, Wrapper-Klassen, `Image`-Props und responsive Layoutregeln fuer Ariadna/Sascha erfassen.
- [x] Die in Wave 1 finalisierte Bildbox-Logik auf `/warum-wir` uebertragen.
- [x] Ariadna und Sascha in `warum-wir/page.tsx` auf gleiche Breite, gleiche Hoehe und dieselben responsive Constraints bringen.
- [x] Personen-spezifische Groessen-, Hoehen- oder Aspect-Unterschiede entfernen, sofern sie die sichtbare Gleichheit verhindern.
- [x] Sicherstellen, dass Inhalt, Reihenfolge, Section-Abstaende und CTAs unveraendert bleiben.
- [x] Responsive Verhalten fuer Desktop, Tablet und Mobile beruecksichtigen.
- [x] Keine Bildassets veraendern, ersetzen oder neu exportieren.

## Akzeptanzkriterien

- [x] Ariadnas und Saschas Bildboxen auf `/warum-wir` nutzen dieselbe gerenderte Breite innerhalb desselben Breakpoints.
- [x] Ariadnas und Saschas Bildboxen auf `/warum-wir` nutzen dieselbe gerenderte Hoehe innerhalb desselben Breakpoints.
- [x] Ariadnas Bild wirkt auf `/warum-wir` nicht mehr laenger als Saschas.
- [x] Die Umsetzung ist visuell und technisch konsistent zur finalen AboutUs-Bildbox aus Wave 1.
- [x] Mobile Stack-Layouts bleiben stabil und ohne Textueberlagerungen.
- [x] Es gibt keinen Diff an Bildassets oder nicht betroffenen Bereichen.

## Abhaengigkeiten

Wave 1 muss die Ziel-Umsetzung fuer die gemeinsame Founder-Bildbox definiert haben.

## Notes

Diese Wave soll verhindern, dass "Ueber uns" und "Warum wir" fuer dieselben Founder-Bilder unterschiedliche Regeln verwenden.

Umsetzung 2026-04-26:
- `warum-wir/page.tsx` nutzt dieselbe `FounderImageFrame`-Komponente wie `AboutUs.tsx`.
- Ariadna und Sascha verwenden auf `/warum-wir` denselben Frame mit fixer `aspectRatio: "2 / 3"`, `w-full`, `overflow-hidden` und `Image fill`.
- Inhalt, Reihenfolge, Section-Abstaende, CTAs und Bildassets blieben unveraendert.
