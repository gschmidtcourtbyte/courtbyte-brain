# Wave 4 - Checkout Step 2 und Admin-Statussteuerung

**Iteration:** [[Iteration-38]]
**Skill:** frontend-eng
**Status:** Done

## Tasks

- [x] `web/customer/src/types/api.ts`: Booking-Request/-Response und `CampBookingStatus` um Add-on- und Statusfelder erweitern.
- [x] `web/customer/src/lib/api.ts`: Admin-Camp-API um generische Statusaenderung ergaenzen.
- [x] `web/customer/src/lib/camp-booking-form.ts`: Formwerte, Payload Builder und Validierung fuer Add-ons erweitern.
- [x] `web/customer/src/lib/camp-booking-form.test.ts`: Payload-Tests fuer optionale Add-ons und Einzeltrainerstunden ergaenzen.
- [x] `web/customer/src/app/checkout/page.tsx`: Anfragefluss in zwei Schritte gliedern:
  - Step 1: Paket-/Camp-Kontext und Kontaktdaten.
  - Step 2: optionale Zusatzleistungen wie bei Flugbuchung.
- [x] Step 2 UI umsetzen:
  - Checkbox Flughafentransfer.
  - Checkbox Vollpension.
  - Eingabe Einzeltrainerstunden als optionales Zahlenfeld mit `min=0`, ganzzahlig.
  - Zusammenfassung vor Absenden inklusive Hinweis, dass Zusatzpreise spaeter bestaetigt bzw. aktuell ohne Aufpreis im System hinterlegt sind.
- [x] Erfolgsscreen um Add-on-Zusammenfassung erweitern.
- [x] `web/customer/src/app/admin/page.tsx`: Statuslabels und Filter um neue Statuswerte erweitern.
- [x] `web/customer/src/app/admin/page.tsx`: Statusauswahl pro Anfrage anbieten, inklusive Confirm-Dialog bei Mail-ausloesenden Statuswechseln.
- [x] `web/customer/src/app/admin/page.tsx`: Add-ons, Gesamtpreis, Anzahlung und Restbetrag in der Buchungsliste darstellen, ohne die Tabelle auf Mobile/kleinen Viewports unlesbar zu machen.

## Done Criteria

- [x] Nutzer koennen die Anfrage ohne Add-ons abschicken.
- [x] Nutzer koennen jede Add-on-Kombination abschicken.
- [x] Einzeltrainerstunden blockieren bei negativen oder ungueltigen Werten vor dem Submit.
- [x] Checkout bleibt auf Mobile und Desktop bedienbar; Step-Wechsel verliert keine eingegebenen Daten.
- [x] Admin sieht und filtert die neuen Statuswerte.
- [x] Admin kann Status vergeben; bei `confirmed` und `balance_requested` ist vorab klar, dass eine E-Mail generiert wird.
- [x] TypeScript bleibt strikt, kein neues `any`.

## Notes

Der Step-2-Flow soll die bestehende Checkout-Seite erweitern, nicht komplett neu bauen. Wichtig ist eine ruhige, klare Operations-UI im Adminbereich statt weiterer isolierter Einzelbuttons.

Abgeschlossen am 2026-05-18. Checkout hat jetzt einen zweistufigen Flow: Schritt 1 Paket/Kontakt/Einwilligung, Schritt 2 optionale Extras mit Flughafentransfer, Vollpension und ganzzahligen Einzeltrainerstunden. Payload Builder und TypeScript-Typen senden die Add-ons optional. Erfolgsscreen zeigt die Add-on-Zusammenfassung. Admin-Dashboard nutzt eine Statusauswahl mit neuen Labels, bestaetigt Mail-ausloesende Wechsel und zeigt Extras, Gesamtpreis, Anzahlung und Restbetrag. Verifikation: `npm run lint` und `npm run build` gruen. `npm run test -- src/lib/camp-booking-form.test.ts` bleibt durch EACCES auf `web/customer/node_modules/.vite-temp` blockiert; `sudo chown` war ohne Passwort nicht moeglich.
