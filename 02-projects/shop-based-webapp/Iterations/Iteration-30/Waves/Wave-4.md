# Wave 4 - Frontend fuer Anfrageoption, Kapazitaet und Homepage-Reihenfolge

**Iteration:** [[Iteration-30]]
**Skill:** frontend-eng
**Status:** Done

## Ziel

Die Customer-Web-App macht die neuen fachlichen Optionen nutzbar: Admins koennen Camp-Kapazitaeten pflegen, Kunden sehen eine rote, optisch getrennte Platzanzeige im Campkalender, das Anfrageformular bietet "Gruppenanfrage / Individualanfrage" nur dort an, und "Ueber uns" steht auf der Startseite direkt vor dem Campkalender.

## Agent

Frontend Engineer: Fokus auf bestehende Next.js-/React-Patterns, TypeScript-Contracts, responsive UI, ruhige visuelle Hierarchie und Erhalt des OUT Padel Designs.

## Tasks

- [x] `web/customer/src/types/api.ts` erweitern: Camp Event Request-/Response-Typen enthalten optionale Kapazitaetsfelder.
- [x] `web/customer/src/app/admin/camp-events/page.tsx` erweitern: freie Plaetze und Gesamtplaetze als optionale numerische Inputs pflegen.
- [x] Admin-Formularwerte fuer Kapazitaet korrekt initialisieren, speichern, leeren und nach Refresh erneut anzeigen.
- [x] Admin-UI-validierung ergaenzen: keine negativen Werte, freie Plaetze nicht groesser als Gesamtplaetze, klare Fehlermeldung.
- [x] `web/customer/src/components/NextCampBanner.tsx` erweitern: bei gueltiger Kapazitaet roten, separaten Hinweis wie `10 von 12 Plaetzen frei` anzeigen.
- [x] Kapazitaet in Event-Liste und/oder Detailkarte so platzieren, dass sie sichtbar ist, aber Datum, Ort, Zielgruppe und CTA nicht ueberlagert.
- [x] Anfrageformular in `web/customer/src/app/checkout/page.tsx` erweitern: neue Option "Gruppenanfrage / Individualanfrage" in der Paketauswahl anzeigen.
- [x] Sicherstellen, dass diese Anfrageoption nur im Anfrageformular auftaucht und nicht in `Pricing`, Camp-Empfehlungen oder Paketkarten.
- [x] Checkout-Helfer und Paketlabel-Logik so erweitern, dass Auswahl, Mehrfachauswahl und Erfolgsansicht die neue Option lesbar darstellen.
- [x] Startseitenreihenfolge in `web/customer/src/app/page.tsx` anpassen: `AboutUs` direkt nach `Hero`, vor `NextCampBanner`.
- [x] Responsive Smoke fuer Homepage, Campkalender, Admin-Campeditor und Anfrageformular beruecksichtigen.

## Akzeptanzkriterien

- [x] Admins koennen freie und gesamte Plaetze fuer einen Camp-Termin eintragen.
- [x] Admins koennen vorhandene Kapazitaetswerte wieder leeren.
- [x] Admin-Frontend verhindert offensichtlich ungueltige Kapazitaetswerte vor dem Speichern oder zeigt eine klare API-Fehlermeldung.
- [x] Der Campkalender zeigt bei gepflegter Kapazitaet einen roten, optisch getrennten Hinweis im Format `X von Y Plaetzen frei`.
- [x] Ohne gepflegte vollstaendige Kapazitaet wird kein leerer oder falscher Hinweis angezeigt.
- [x] "Gruppenanfrage / Individualanfrage" ist im Anfrageformular auswaehlbar.
- [x] Die neue Anfrageoption wird beim Absenden im Payload beruecksichtigt und in der Erfolgsansicht lesbar angezeigt.
- [x] Die neue Anfrageoption erscheint nicht in Pricing, Homepage-Paketkarten oder Camp-Empfehlungsboxen.
- [x] `AboutUs` steht auf der Startseite unmittelbar nach `Hero` und vor `NextCampBanner`.
- [x] Auf Mobile und Desktop entstehen keine Textueberlagerungen, Layout-Spruenge oder abgeschnittenen CTAs.

## Abhaengigkeiten

Wave 3 muss die API-Contracts fuer Kapazitaet und Anfrageoption bereitstellen. Die reine Homepage-Reihenfolge kann technisch frueher angepasst werden, soll aber gemeinsam mit den Customer-Web-Aenderungen integriert werden.

## Notes

Die neue Anfrageoption soll als kompakte Auswahlzeile im bestehenden Formular wirken. Keine neue Paketkarte, kein langer Beschreibungstext und keine Preisbox dafuer anlegen.

Umsetzung 2026-04-27:
- `web/customer/src/types/api.ts` um `capacity_available` und `capacity_total` fuer Camp Events und Admin Requests erweitert.
- Separate Anfrageoption `group-individual` in `INQUIRY_ONLY_PACKAGE_OPTIONS` und `CHECKOUT_PACKAGE_OPTIONS` angelegt; `CAMP_PACKAGES` bleibt unveraendert mit vier regulaeren Paketen.
- Checkout nutzt `CHECKOUT_PACKAGE_OPTIONS`, zeigt "Gruppenanfrage / Individualanfrage" nur im Anfrageformular und mappt Erfolgsansicht ueber `getCheckoutPackageName`.
- Admin-Campeditor hat neue optionale Felder fuer freie und gesamte Plaetze, inklusive UI-Validierung fuer ganze Zahlen ab 0 und `frei <= gesamt`.
- Admin-Create/Update sendet Kapazitaetswerte als Zahl oder `null`; leere Felder setzen die Anzeige zurueck.
- `getCampCapacityLabel` zentralisiert die Anzeige und blendet unvollstaendige/ungueltige Werte aus.
- `NextCampBanner` zeigt gueltige Kapazitaet als roten separaten Hinweis in Eventliste und Detailkarte.
- Homepage-Reihenfolge angepasst: `AboutUs` direkt nach `Hero`, vor `NextCampBanner`.
- Verifikation: `npm test -- --configLoader runner src/lib/camp-packages.test.ts src/lib/camp-events.test.ts src/lib/camp-booking-form.test.ts` mit 3 Testdateien / 55 Tests gruen.
- Visueller Browser-Smoke folgt in Wave 5 bzw. wird dort als lokaler Tooling-Blocker dokumentiert, falls nicht moeglich.
