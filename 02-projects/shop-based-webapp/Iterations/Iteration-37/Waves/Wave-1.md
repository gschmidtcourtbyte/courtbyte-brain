# Wave 1 - Mobile Navbar und sichtbare Content-Korrekturen umsetzen

**Iteration:** [[Iteration-37]]
**Skill:** frontend-eng
**Status:** Done

## Tasks

- [x] `web/customer/src/components/Navigation.tsx`: Social Icons auf Mobile direkt in der Navbar sichtbar machen, neben dem Burger-Button beziehungsweise im rechten Header-Bereich. Bestehende `socialLinks`, `externalLinkProps` und `socialIconMap` weiterverwenden.
- [x] `web/customer/src/components/Navigation.tsx`: Mobile Burger-Menue so anpassen, dass Navigationslinks und Auth-Links erhalten bleiben, die Social Icons aber nicht mehr als separate Social-Sektion im Menue verschwinden.
- [x] `web/customer/src/components/Navigation.tsx`: Mobile Layout auf kleinen Breiten stabil halten, ohne Logo, Social Icons und Burger-Button zu ueberlappen.
- [x] `web/customer/src/app/warum-wir/page.tsx`: Hero-Zeile `Kein Touristenprogramm.` durch `Kein Standard.` ersetzen.
- [x] `web/customer/src/app/warum-wir/page.tsx`: Sascha-Text in der Ariadna-&-Sascha-Sektion an den Sascha-Text aus `web/customer/src/components/AboutUs.tsx` angleichen. Falls sinnvoll, gemeinsamen Content lokal extrahieren, aber keine groessere Content-Architektur einfuehren.
- [x] `web/customer/src/app/impressum/page.tsx`: Kontakt-Telefonnummer auf `+49 (0)155-67149871` setzen.
- [x] `web/customer/src/lib/camp-packages.ts`: Inclusion `Mittagessen täglich an ausgewählten Restaurants (exklusive Getränke)` aus der Kategorie `Hotel` entfernen und in Kategorie `Erlebnis` einfuegen.
- [x] Relevante bestehende Tests aktualisieren, insbesondere `web/customer/src/lib/camp-packages.test.ts` fuer die neue Kategorie-Zuordnung.

## Done Criteria

- [x] Mobile Navbar zeigt Social Links ohne geoeffnetes Burger-Menue.
- [x] Burger-Menue bleibt auf Mobile bedienbar und schliesst weiterhin bei Link-Auswahl.
- [x] Desktop-Navigation verhaelt sich unveraendert.
- [x] Warum-wir-Headline und Sascha-Text entsprechen den fachlichen Vorgaben.
- [x] Impressum zeigt die neue Telefonnummer.
- [x] Paket-Inclusion fuer Mittagessen liegt fachlich unter `Erlebnis`, nicht mehr unter `Hotel`.
- [x] TypeScript bleibt strikt; kein neues `any`.
- [x] Keine unnoetigen Refactors ausserhalb der genannten Dateien.

## Notes

Die Navbar-Aenderung ist die einzige Layout-Arbeit dieser Wave. Prioritaet ist ein robuster mobiler Header mit klaren Tap-Zielen. Die restlichen Aufgaben sind Content- und Datenzuordnungskorrekturen mit geringem technischem Risiko.

Umgesetzt am 2026-05-18. `Navigation.tsx` nutzt eine gemeinsame `SocialIconLinks`-Ausgabe: Desktop bleibt in der bestehenden Desktop-Gruppe, Mobile zeigt zwei 40px-Tap-Ziele direkt neben dem Burger-Button. Die Social-Sektion im mobilen Menue wurde entfernt. Saschas Bio liegt jetzt in `web/customer/src/lib/founder-content.ts` und wird von `AboutUs.tsx` sowie `/warum-wir` gemeinsam genutzt. `Kein Touristenprogramm.` wurde durch `Kein Standard.` ersetzt. Das Impressum zeigt die neue Telefonnummer. Die Mittagessen-Inclusion wurde von `Hotel` nach `Erlebnis` verschoben und der Paket-Test entsprechend erweitert.
