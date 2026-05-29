# Wave 2 - Admin-Buchungsliste vollstaendig anzeigen

**Iteration:** [[Iteration-27]]
**Skill:** frontend-eng
**Status:** Done

## Ziel

Die neuesten Buchungseingaenge im Admin Dashboard sollen auf Desktop die volle verfuegbare Breite nutzen, damit die Tabelle vollstaendiger lesbar ist. "Traffic nach Seiten" wird darunter als eigener Abschnitt angezeigt.

## Agent

Frontend Engineer: Fokus auf Admin-Dashboard-Layout, responsive Tabellen-Ergonomie und bestehende OUT-Padel UI-Konventionen.

## Tasks

- [x] `web/customer/src/app/admin/page.tsx`: zweispaltiges Layout fuer Buchungsliste und Traffic aufloesen.
- [x] `web/customer/src/app/admin/page.tsx`: Buchungsliste als full-width Abschnitt unter den KPI-Karten platzieren.
- [x] `web/customer/src/app/admin/page.tsx`: "Traffic nach Seiten" unterhalb der Buchungsliste platzieren.
- [x] `web/customer/src/app/admin/page.tsx`: horizontalen Tabellen-Overflow fuer kleinere Viewports erhalten.
- [x] `web/customer/src/app/admin/page.tsx`: bestehende Suche, Statusfilter und Aktionen unveraendert funktionsfaehig halten.

## Akzeptanzkriterien

- [x] "Neueste Buchungseingaenge" nutzt auf Desktop die volle Dashboard-Breite.
- [x] Die Spalten Zeit, Name, E-Mail, Camp, Paket, Preis, Status und Aktionen bleiben sichtbar bzw. horizontal scrollbar auf kleinen Viewports.
- [x] "Traffic nach Seiten" steht unterhalb der Buchungsliste und wird nicht entfernt.
- [x] Suche und Statusfilter funktionieren unveraendert.
- [x] Buchungsaktionen "Bestaetigen" und "Zahlung bestaetigen" bleiben unveraendert erreichbar.

## Abhaengigkeiten

Keine. Diese Wave kann parallel zu Wave 1 und Wave 3 vorbereitet werden.

## Notes

Bestandsbefund: `admin/page.tsx` verwendet aktuell ein `xl:grid-cols-[minmax(0,1.2fr)_minmax(0,0.8fr)]`, das die Buchungstabelle zugunsten der Traffic-Karte einengt.

Umsetzung 2026-04-26:
- Zweispalten-Grid durch vertikales `space-y-6` Layout ersetzt.
- Buchungsliste steht als full-width Abschnitt direkt unter den KPI-Karten.
- Tabelle behaelt `overflow-x-auto` und nutzt `min-w-[1180px]`, damit alle Kernspalten auf kleinen Viewports horizontal scrollbar bleiben.
- "Traffic nach Seiten" steht darunter und nutzt die Breite mit responsivem 1/2/3-Spalten-Grid.
- Suche, Statusfilter und Buchungsaktionen wurden nicht funktional veraendert.
- Verifiziert mit `npm run build`.
