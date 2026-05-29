# Wave 1 - Ueber-uns Bilder mit Crop/Zoom und softer Roundness

**Iteration:** [[Iteration-26]]
**Skill:** frontend-eng
**Status:** Todo

## Ziel

Die Homepage-"Ueber uns" Bilder bekommen wieder die kuratierte Crop-/Zoom-Darstellung, wirken aber nicht eckig. Die Ecken sollen leicht abgerundet sein, ohne runde Portrait-Circles zu werden.

## Tasks

- [ ] `web/customer/src/components/AboutUs.tsx`: Ariadna-Bild von `object-contain` zurueck auf Crop-Darstellung stellen.
- [ ] `web/customer/src/components/AboutUs.tsx`: Sascha-Bild von `object-contain` zurueck auf Crop-Darstellung stellen.
- [ ] Vorherige sinnvolle `object-position` Werte wiederherstellen oder neu feinjustieren, ohne Gesichter unguenstig anzuschneiden.
- [ ] Bildcontainer mit softer Roundness versehen, z. B. `rounded-[--radius-lg]`; nicht `rounded-full`.
- [ ] Sicherstellen, dass die Card selbst weiterhin zum bestehenden OUT-Padel-Look passt.

## Akzeptanzkriterien

- [ ] Ariadna und Sascha werden in `AboutUs.tsx` wieder cropped/gezoomt angezeigt.
- [ ] Die Bildflaechen haben sichtbar leicht abgerundete Ecken.
- [ ] Die Bilder sind nicht komplett rund.
- [ ] `warum-wir/page.tsx` und `GallerySlider.tsx` werden nicht versehentlich reverted.
- [ ] Mobile und Desktop behalten stabile Card-Hoehen ohne Textueberlagerung.

## Abhaengigkeiten

Keine. Diese Wave kann unabhaengig von der Paketauswahl umgesetzt werden.

## Notes

Das Kundenfeedback bezieht sich auf "Ueber uns"; die Iteration-25-Darstellung auf "Warum wir?" bleibt bewusst ausser Scope.
