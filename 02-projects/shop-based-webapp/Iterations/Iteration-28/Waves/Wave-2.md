# Wave 2 - AboutUs Founder-Bilder boxbreit ohne Zoom

**Iteration:** [[Iteration-28]]
**Skill:** frontend-eng
**Status:** Done

## Ziel

Die Ariadna- und Sascha-Bilder im Homepage-Abschnitt "Ueber uns" werden auf die Breite ihrer Box angepasst, ohne Crop, Zoom oder manuelle Ausschnittverschiebung.

## Agent

Frontend Engineer: Fokus auf Founder-Bildcontainer, responsive Stabilitaet und Konsistenz mit dem bestehenden OUT-Padel-Look.

## Tasks

- [x] `web/customer/src/components/AboutUs.tsx` analysieren: aktuelle Bildcontainer, `Image`-Props und Klassen fuer Ariadna/Sascha erfassen.
- [x] Ariadna-Bild so darstellen, dass es die Boxbreite nutzt und unverzoomt sichtbar bleibt.
- [x] Sascha-Bild so darstellen, dass es die Boxbreite nutzt und unverzoomt sichtbar bleibt.
- [x] Sicherstellen, dass fuer Ariadna/Sascha kein `object-cover`, keine manuelle `object-position` und kein Scale-/Hover-Zoom verwendet wird.
- [x] Containerhoehen und responsive Constraints pruefen, damit keine Textueberlagerung oder Layout-Spruenge entstehen.
- [x] Keine Bildassets veraendern, ersetzen oder neu exportieren.

## Akzeptanzkriterien

- [x] Ariadna in `AboutUs.tsx` ist auf die Boxbreite angepasst und nicht hineingezoomt.
- [x] Sascha in `AboutUs.tsx` ist auf die Boxbreite angepasst und nicht hineingezoomt.
- [x] Ariadna/Sascha werden nicht per `object-cover` beschnitten.
- [x] Ariadna/Sascha verwenden keine manuelle `object-[...]` Positionierung.
- [x] Es gibt keinen neuen Scale-/Hover-Zoom fuer diese Bilder.
- [x] Der "Ueber uns"-Abschnitt bleibt auf Mobile und Desktop stabil, ohne ueberlappende Texte oder abgeschnittene Inhalte.

## Abhaengigkeiten

Keine. Die Umsetzung sollte aber als Referenz fuer Wave 3 dienen, damit "Warum wir?" konsistent wirkt.

## Notes

Diese Wave behandelt nur `AboutUs.tsx`. Die analoge Darstellung auf `/warum-wir` folgt separat in Wave 3.

Umsetzung 2026-04-26:
- Ariadna und Sascha in `AboutUs.tsx` von `fill`/Aspect-Frame auf normales `Image` mit echten Bilddimensionen und `block h-auto w-full` umgestellt.
- Keine `object-cover`, manuelle `object-[...]` Positionierung oder Scale-/Hover-Zoom-Klassen fuer diese Bilder.
- Keine Bildassets geaendert.
