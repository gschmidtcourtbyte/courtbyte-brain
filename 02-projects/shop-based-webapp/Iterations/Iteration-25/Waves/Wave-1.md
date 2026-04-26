# Wave 1 - Insolvenzversicherungstext in "Warum wir?"

**Iteration:** [[Iteration-25]]
**Skill:** frontend-eng
**Status:** Done

## Ziel

Die Insolvenzversicherung auf der Seite "Warum wir?" erklaert den rechtlichen Schutz und den Reisesicherungsschein mit dem vom Kunden gelieferten Text.

## Tasks

- [x] `web/customer/src/app/warum-wir/page.tsx`: Der Card `insolvency` ein `detail` mit exakt diesem Text geben:

  > Dein Geld ist bei uns sicher — zu 100%. Als lizenzierter Reiseveranstalter sind wir gesetzlich verpflichtet, deine Zahlungen durch eine offizielle Insolvenzversicherung (§651r BGB) abzusichern. Das bedeutet für dich: Egal was passiert — Anzahlung und Restzahlung sind vollständig geschützt. Mit deiner Buchungsbestätigung erhältst du deinen persönlichen Reisesicherungsschein als Nachweis.

- [x] `web/customer/src/app/warum-wir/page.tsx`: Sicherstellen, dass die Insolvenzversicherung wie Unfallversicherung und Betriebshaftpflicht einen "Mehr erfahren"-Zugang bekommt.
- [x] `web/customer/src/app/warum-wir/page.tsx`: Kurztext der Card nur dann anpassen, wenn er grammatikalisch/typografisch stoert; keine inhaltliche Kuerzung des gelieferten Detailtexts.

## Akzeptanzkriterien

- [x] Der gelieferte Text ist exakt und vollstaendig im Insolvenzversicherungskontext vorhanden.
- [x] Die Card zeigt "Mehr erfahren" fuer Insolvenzversicherung.
- [x] `aria-expanded` und `aria-controls` funktionieren weiterhin fuer alle Detailkarten.
- [x] Unfallversicherung und Betriebshaftpflicht verlieren keine bestehenden Detailtexte.

## Abhaengigkeiten

Keine. Diese Wave sollte vor Wave 2 integriert werden, weil beide `warum-wir/page.tsx` beruehren.

## Notes

Umgesetzt in `web/customer/src/app/warum-wir/page.tsx`. Die Insolvenzversicherung nutzt jetzt wie die anderen Detailkarten das bestehende "Mehr erfahren"-Pattern. Unfallversicherung und Betriebshaftpflicht blieben unveraendert.
