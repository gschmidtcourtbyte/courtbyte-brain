# Wave 5 - Checkout UI: Paketauswahl im Anfrageformular

**Iteration:** [[Iteration-26]]
**Skill:** frontend-eng
**Status:** Todo

## Ziel

Nutzer koennen direkt im "Anfrage senden" Formular Paketinteressen aendern. Unter "Ausgewaehltes Paket" steht eine kompakte Zusammenfassung statt der bisherigen langen Paketbeschreibung.

## Tasks

- [ ] `web/customer/src/app/checkout/page.tsx`: Paket-State von fixem `selectedPackage` auf editierbare Paketinteressen erweitern.
- [ ] Initialauswahl aus URL-Paket oder Camp-Empfehlung setzen.
- [ ] Im Formular ein kompaktes Auswahlpattern einbauen: Checkbox-Liste oder Multi-Select/Dropdown mit den vier Paketnamen und Kurzpreis.
- [ ] Mindestens ein Paket erzwingen; Submit-Button oder Fehlermeldung entsprechend behandeln.
- [ ] Linke Karte "Ausgewaehltes Paket" in kompakte Zusammenfassung der ausgewaehlten Pakete umbauen.
- [ ] Lange `description`-Abschnitte und `included`-Featurelisten aus der Checkout-Zusammenfassung entfernen.
- [ ] `web/customer/src/lib/camp-booking-form.ts`: Payload-Builder um Paketinteressen erweitern.
- [ ] Erfolgsmeldung nach Submit auf mehrere Paketinteressen textlich korrekt machen.

## Akzeptanzkriterien

- [ ] Das Formular startet mit einem vorausgewaehlten Paket.
- [ ] Nutzer koennen im Formular ein anderes Paket auswaehlen oder weitere Pakete anhaengen.
- [ ] Mindestens ein Paket ist fuer Submit erforderlich.
- [ ] Die Zusammenfassung zeigt nur Paketname, Preislabel und kurzen Teaser/Status pro Auswahl.
- [ ] Es wird keine zweite vollstaendige Paketliste mit allen Featuredetails angezeigt.
- [ ] Payload sendet Primaerpaket und Paketinteressen korrekt.
- [ ] Mobile Layout bleibt scanbar und ohne ueberlange Cards.

## Abhaengigkeiten

Waves 2 bis 4 fuer echten Persistenz-/API-Contract. Wenn fachlich doch Single-Select reicht, kann diese Wave ohne Backend-Aenderung vereinfacht werden.

## Notes

Bevorzugtes UI: kompakte Checkbox-Gruppe oder Disclosure/Dropdown mit Checkboxen. Kein grosses Card-Raster.
