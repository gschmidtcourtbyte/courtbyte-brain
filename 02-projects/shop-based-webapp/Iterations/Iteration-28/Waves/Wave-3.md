# Wave 3 - Warum-wir Founder-Bilder boxbreit ohne Zoom

**Iteration:** [[Iteration-28]]
**Skill:** frontend-eng
**Status:** Done

## Ziel

Die Ariadna- und Sascha-Bilder auf der Seite "Warum wir?" werden konsistent zu `AboutUs.tsx` auf die Breite ihrer Box angepasst, ohne Crop, Zoom oder manuelle Ausschnittverschiebung.

## Agent

Frontend Engineer: Fokus auf Konsistenz zwischen Homepage-Founder-Darstellung und der Seite `/warum-wir`.

## Tasks

- [x] `web/customer/src/app/warum-wir/page.tsx` analysieren: aktuelle Founder-Bildcontainer, `Image`-Props und Klassen erfassen.
- [x] Ariadna-Bild analog zur AboutUs-Darstellung boxbreit und unverzoomt umsetzen.
- [x] Sascha-Bild analog zur AboutUs-Darstellung boxbreit und unverzoomt umsetzen.
- [x] Sicherstellen, dass fuer Ariadna/Sascha kein `object-cover`, keine manuelle `object-position` und kein Scale-/Hover-Zoom verwendet wird.
- [x] Layout der Founder-Sektion auf Desktop und Mobile pruefen, insbesondere Textnaehe, Card-Hoehen und Bildcontainer.
- [x] Keine Bildassets veraendern, ersetzen oder neu exportieren.

## Akzeptanzkriterien

- [x] Ariadna in `warum-wir/page.tsx` ist auf die Boxbreite angepasst und nicht hineingezoomt.
- [x] Sascha in `warum-wir/page.tsx` ist auf die Boxbreite angepasst und nicht hineingezoomt.
- [x] Die Darstellung ist konsistent zu `AboutUs.tsx`.
- [x] Ariadna/Sascha werden nicht per `object-cover` beschnitten.
- [x] Ariadna/Sascha verwenden keine manuelle `object-[...]` Positionierung.
- [x] Es gibt keinen neuen Scale-/Hover-Zoom fuer diese Bilder.
- [x] Die `/warum-wir`-Seite bleibt auf Mobile und Desktop stabil, ohne ueberlappende Texte oder abgeschnittene Inhalte.

## Abhaengigkeiten

Wave 2 sollte als visuelle Referenz genutzt werden. Falls Wave 3 zuerst umgesetzt wird, muss die finale Abstimmung nach Wave 2 erfolgen.

## Notes

Diese Wave verhindert, dass Homepage und `/warum-wir` bei denselben Founder-Bildern auseinanderlaufen.

Umsetzung 2026-04-26:
- Ariadna und Sascha in `warum-wir/page.tsx` analog zu `AboutUs.tsx` auf normale `Image`-Tags mit echten Bilddimensionen und `block h-auto w-full` umgestellt.
- Keine `object-cover`, manuelle `object-[...]` Positionierung oder Scale-/Hover-Zoom-Klassen fuer diese Bilder.
- Keine Bildassets geaendert.

Hotfix 2026-04-26:
- Beide Founder-Bildbereiche in `warum-wir/page.tsx` nutzen nun denselben `aspect-[2/3]`-Frame.
- Bilder bleiben `object-contain`, damit die gleich grossen Boxen ohne Crop/Zoom dargestellt werden.

Hotfix 2 2026-04-26:
- `warum-wir/page.tsx`: beide Founder-Bilder fuellen den gemeinsamen `aspect-[2/3]`-Frame mit `object-cover`.
- Ariadna nutzt `object-bottom`, damit der notwendige Zuschnitt primaer oben statt unten erfolgt.
