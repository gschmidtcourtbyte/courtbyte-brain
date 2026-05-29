# Iteration 26 - Ueber-uns Bild-Crop und flexible Paketauswahl im Anfrageformular

**Status:** Planning
**Repo:** /home/andersen/git/shop-based-webapp
**Datum:** 2026-04-26
**Rolle:** Acting as Product Owner. Planning Iteration 26 for shop-based-webapp.

## Ziel

Iteration 26 setzt zwei Kundenfeedbackpunkte um: Die Bilder im Homepage-Abschnitt "Ueber uns" bekommen wieder eine kuratierte Crop-/Zoom-Darstellung, aber mit leicht abgerundeten Ecken statt eckiger Bildflaechen. Ausserdem wird die Paketauswahl im Anfrageformular selbst aenderbar: Nutzer koennen im Formular weitere Pakete auswaehlen, und unter "Ausgewaehltes Paket" erscheint nur noch eine kompakte Zusammenfassung der gewaehlten Paketinteressen.

## Kundenfeedback

1. Bei den "Ueber uns" Bildern die Iteration-25-Aenderung rueckgaengig machen: Crop/Zoom soll wieder aktiv sein.
2. Die Bilder sollen nicht eckig wirken, aber auch nicht rund sein. Gewuenscht ist eine softe Roundness mit leicht abgerundeten Ecken.
3. Pakete sollen auch im "Anfrage senden" Formular noch auswaehlbar sein.
4. Aktuell ist im Formular entweder das empfohlene Paket oder das vorher ausgewaehlte Paket fix hinterlegt. Eine Anpassung soll auch dort moeglich sein.
5. Es soll nicht nochmal das komplette Paketangebot aller vier Pakete als grosse Cards gelistet werden.
6. Im Formular sollen Pakete zusaetzlich via kompakter Auswahl, z. B. Dropdown/Checkboxen, angehakt werden koennen.
7. Unter "Ausgewaehltes Paket" soll eine kleinere Zusammenfassung der ausgewaehlten Pakete erscheinen, nicht nochmal der komplette Paketinhalt.

## Scope

**In:**
- `web/customer/src/components/AboutUs.tsx`: Homepage-"Ueber uns" Bilder wieder mit Crop/Zoom und softer Ecken-Roundness darstellen.
- Checkout-/Anfrageformular unter `web/customer/src/app/checkout/page.tsx`: Paketinteressen im Formular aenderbar machen.
- Frontend-Form-Helper und Types: Mehrfachauswahl der Paketinteressen sauber modellieren.
- Backend/API: mehrere Paketinteressen persistieren und in Response/Admin/Mail sichtbar halten.
- Migration fuer additive Persistenz der Paketinteressen.
- Tests fuer Form-Payload, Backend-Validation, Handler-Contract und relevante Frontend-Datenlogik.

**Out:**
- Revert der Iteration-25-Bilddarstellung auf der Seite `warum-wir/page.tsx`.
- Revert der Gallery-`object-contain`-Darstellung.
- Neues Paket-Design oder neue Paketpreise.
- Payment-/Stripe-Flow.
- Vollstaendiger Redesign des Checkoutformulars.

## Produktentscheidungen

1. **"Ueber uns" meint den Homepage-Abschnitt.**
   `AboutUs.tsx` wird angepasst. Die Seite `warum-wir/page.tsx` bleibt zunaechst bei der Iteration-25-Originalbild-Darstellung, ausser die Umsetzung zeigt einen klaren visuellen Widerspruch, der separat abgestimmt werden muss.
2. **Crop/Zoom ja, harte Ecken nein.**
   Die Bilder duerfen wieder `object-cover` und die vorherigen `object-position` Werte nutzen. Der Bildrahmen bekommt eine dezente Rundung, z. B. `rounded-[--radius-lg]` oder eine aehnliche bestehende Token-Rundung, nicht `rounded-full`.
3. **Paketinteresse ist Mehrfachauswahl.**
   Die Anfrage soll mehrere Paketinteressen speichern koennen. Das bestehende `package_slug` bleibt als Primaer-/Defaultpaket fuer Rueckwaertskompatibilitaet bestehen; zusaetzlich wird eine Liste ausgewaehlter Paket-Slugs gespeichert.
4. **Formularauswahl statt Paket-Cards.**
   Im Formular reichen kompakte Checkboxen oder ein Multi-Select-Pattern mit den vier Paketnamen und Kurzpreis. Keine vollstaendigen Paketbeschreibungen, keine Featurelisten.
5. **Kompakte Zusammenfassung.**
   Unter "Ausgewaehltes Paket" werden nur Paketname, Preislabel, ggf. "Preis auf Anfrage" und ein sehr kurzer Teaser/Status angezeigt. Die bisherigen langen Beschreibungstexte und Included-Listen entfallen dort.

## Bestandsaufnahme

| Bereich | Datei | Befund |
|---|---|---|
| Homepage Ueber-uns Bilder | `web/customer/src/components/AboutUs.tsx` | Iteration 25 stellte Ariadna/Sascha auf `object-contain` mit `bg-brand-sand/10`; gewuenscht ist wieder Crop/Zoom mit soften Ecken. |
| Checkout Auswahlableitung | `web/customer/src/app/checkout/page.tsx` | `recommendedPackageSlug` wird aus URL-Paket oder Camp-Empfehlung abgeleitet; `selectedPackage` ist nicht frei editierbar. |
| Checkout UI | `web/customer/src/app/checkout/page.tsx` | Linke Karte sagt "Paket ist im Anfrageflow nicht frei editierbar"; rechte Formularkarte zeigt nur das fixe Paket. |
| Frontend Payload | `web/customer/src/lib/camp-booking-form.ts` | `buildCampBookingPayload` sendet nur `package_slug`. |
| Frontend Types | `web/customer/src/types/api.ts` | `CampBookingRequest` und `CampBooking` modellieren nur `package_slug`/`package_name`. |
| Backend Handler | `internal/camps/adapters/http/handler.go` | Request/Response enthalten ein einzelnes Paket. |
| Service/Domain/Persistenz | `internal/camps/app/service.go`, `internal/camps/domain/entities.go`, `booking_repository.go`, `camp_bookings` | Buchung speichert ein einzelnes Snapshot-Paket. |
| E-Mail/Admin | `internal/camps/adapters/confirmation/email_sender.go`, Admin-Bookings | Admin-Mails und Admin-Responses zeigen nur ein Paket. |

## Waves

| Wave | Skill | Focus | Status |
|---|---|---|---|
| Wave 1 | frontend-eng | Homepage-"Ueber uns" Bilder: Crop/Zoom zurueck, softe Ecken | Todo |
| Wave 2 | migration-eng | Paketinteressen im Booking-Datenmodell additiv persistieren | Todo |
| Wave 3 | service-eng | Backend-Service, Domain, Repository und Mail/Admin-Daten fuer mehrere Paketinteressen | Todo |
| Wave 4 | handler-eng | HTTP Contract und Frontend API Types fuer Paketinteressen erweitern | Todo |
| Wave 5 | frontend-eng | Checkout-UI: kompakte Paketauswahl und Zusammenfassung im Anfrageformular | Todo |
| Wave 6 | testing-eng | Regression, Contract-Tests, Build und Review | Todo |

## Critical Path

```text
Wave 1 (AboutUs UI quick fix)

Wave 2 (Migration)
    v
Wave 3 (Service/Repository/Mail/Admin)
    v
Wave 4 (HTTP/Frontend Contract)
    v
Wave 5 (Checkout UI)
    v
Wave 6 (Tests/Build/Review)
```

Wave 1 ist unabhaengig und kann vorgezogen werden. Waves 2 bis 5 sind fuer die Mehrfach-Paketauswahl fachlich sequenziell, damit UI und Backend denselben Contract nutzen.

## Risiken und Mitigationen

| Risiko | Impact | Mitigation |
|---|---|---|
| Mehrfachauswahl bricht bestehende Admin-/Mail-Logik | Hoch | `package_slug` als Primaerpaket behalten und neue Paketliste additiv einfuehren. |
| Schema-Aenderung ist zu gross fuer einen UI-Wunsch | Mittel | Additive Nullable-Spalte verwenden; keine Backfill-Pflicht fuer Altbuchungen. |
| Nutzer waehlen kein Paket aus | Mittel | Mindestens ein Paket muss ausgewaehlt bleiben; Default ist URL-Paket oder Camp-Empfehlung. |
| Preiszusammenfassung fuer mehrere Pakete wirkt wie Summe | Mittel | Kein Gesamtpreis summieren; pro Paket Preislabel zeigen und "Interesse an" klar benennen. |
| "Ueber uns" Bild-Crop wird zu stark | Mittel | Vorherige `object-position` Werte wiederherstellen, aber mit softer Roundness und visueller Smoke-Pruefung. |
| Backend speichert ungueltige Paket-Slugs | Hoch | Slugs in Service/Handler gegen bekannte Package-Slugs validieren, Duplikate normalisieren. |

## Gesamt-Akzeptanzkriterien

- [ ] `AboutUs.tsx` zeigt Ariadna/Sascha wieder mit aktivem Crop/Zoom statt `object-contain`.
- [ ] Die Ueber-uns-Bildflaechen haben leicht abgerundete Ecken, sind aber nicht komplett rund.
- [ ] Checkout startet weiterhin mit URL-Paket oder Camp-Empfehlung als vorausgewaehltem Paket.
- [ ] Nutzer koennen im Formular Paketinteressen aendern und mehrere der vier Pakete auswaehlen.
- [ ] Mindestens ein Paket bleibt fuer den Submit erforderlich.
- [ ] Unter "Ausgewaehltes Paket" erscheint eine kompakte Zusammenfassung der ausgewaehlten Paketinteressen ohne volle Paketbeschreibung/Featurelisten.
- [ ] Die Anfrage persistiert die ausgewaehlten Paketinteressen und macht sie in Response/Admin/Mail sichtbar.
- [ ] Bestehende Buchungen ohne neue Paketinteressen bleiben lesbar.
- [ ] Relevante Backend-, Handler-, Frontend-Helper-Tests sowie Customer-Web Build sind gruen oder Blocker sind dokumentiert.

## Notes

Die Iteration trennt bewusst den kleinen visuellen Ueber-uns-Fix von der groesseren Anfrage-Contract-Arbeit. Falls die Mehrfachauswahl fachlich doch nur als Einzelauswahl gemeint war, koennen Waves 2 bis 4 entfallen und Wave 5 auf ein simples Single-Select reduziert werden.
