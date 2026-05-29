# Iteration 30 - Anfrageoptionen, Camp-Kapazitaet und Homepage-Reihenfolge

**Status:** Done
**Repo:** /home/andersen/git/shop-based-webapp
**Datum:** 2026-04-27
**Rolle:** Acting as Product Owner. Planning Iteration 30 for shop-based-webapp.

## Ziel

Iteration 30 erweitert die OUT Padel Customer Journey an drei Stellen: Im Anfrageformular soll es zusaetzlich zu den bestehenden Paketen die Option "Gruppenanfrage / Individualanfrage" geben, ohne dass dieses Paket auf der Website als regulaeres Paket definiert oder beworben wird. Camp-Termine sollen eine manuell pflegbare, optisch abgesetzte Kapazitaetsanzeige erhalten, z. B. "10 von 12 Plaetzen frei". Ausserdem soll auf der Startseite "Ueber uns" direkt als erster Inhaltsbereich nach dem Hero erscheinen und damit vor dem Campkalender stehen.

Die Iteration ist bewusst cross-funktional geplant, weil die Kapazitaetsanzeige Datenbank, Backend, API, Admin-Editor und Customer-Web betrifft. Die Gruppen-/Individualanfrage bleibt dagegen Anfrageformular-spezifisch und darf nicht in Pricing, Paketkarten oder Camp-Empfehlungen auftauchen.

## Kundenfeedback

1. Im Anfrageformular soll es bei den Paketen auch die Moeglichkeit "Gruppenanfrage / Individualanfrage" geben.
2. Dieses Paket soll nur im Anfrageformular auftauchen.
3. Dieses Paket muss nicht naeher definiert werden.
4. Bei den Camps soll eine optische Limitierung moeglich sein, z. B. "10 von 12 Plaetzen frei".
5. Die Limitierung soll sich optisch klar abtrennen, z. B. in Rot.
6. Die Limitierung soll im Campeditor im Backend einstellbar sein.
7. Auf der Startseite soll "Ueber uns" jetzt direkt als erstes dargestellt werden und somit ueber dem Campkalender stehen.
8. Die Iteration soll in Waves mit passenden Skillsets geplant werden.

## Scope

**In:**
- Neue optionale Camp-Kapazitaetsfelder fuer freie und gesamte Plaetze per SQL-Migration.
- Backend-Domain, Repository, Service und HTTP-DTOs fuer diese Kapazitaetsfelder.
- Admin-Campeditor unter `web/customer/src/app/admin/camp-events/page.tsx`: Kapazitaet pflegen, zuruecksetzen und validieren.
- Public Campkalender unter `web/customer/src/components/NextCampBanner.tsx`: rote, optisch abgesetzte Kapazitaetsanzeige anzeigen, wenn gepflegt.
- Anfrageformular unter `web/customer/src/app/checkout/page.tsx`: "Gruppenanfrage / Individualanfrage" als zusaetzliche auswaehlbare Option nur dort anzeigen.
- Booking-Service/API so erweitern, dass diese Anfrageoption gespeichert und in Admin-/Bestaetigungsansichten sinnvoll lesbar bleibt.
- Startseitenreihenfolge in `web/customer/src/app/page.tsx`: `AboutUs` direkt nach Hero, vor dem Campkalender.
- Backend- und Frontend-Tests fuer geaenderte Validierungen, API-Contracts und UI-Helfer.
- Build-/Test-Verifikation und visueller Smoke fuer Homepage, Campeditor und Anfrageformular, sofern lokal moeglich.

**Out:**
- Keine neue Pricing-Karte, Detailseite oder Marketingdefinition fuer "Gruppenanfrage / Individualanfrage".
- Keine automatische Platzzaehlung durch eingehende Anfragen.
- Keine Sold-out-Logik, Warteliste, Buchungssperre oder Payment-Aenderung.
- Keine Aenderung bestehender Paketpreise oder Camp-Empfehlungslogik ausser notwendiger Validierung fuer Anfrageoptionen.
- Kein Umbau des Admin-Dashboards ausser Anzeige der bereits gespeicherten Anfrageoption, falls vorhandene Booking-Views sonst unverstaendlich waeren.
- Keine Content- oder Bildasset-Aenderungen an "Ueber uns".

## Produktentscheidungen

1. **Anfrageoption statt neues Paket.**
   "Gruppenanfrage / Individualanfrage" ist nur eine Anfrageoption im Formular. Sie bekommt keine regulaere Paketkarte, keine Preisdefinition und keine redaktionellen Details in `PRICING_EDITORIAL_CONTENT`.
2. **Backend muss die Anfrageoption akzeptieren.**
   Da das Anfrageformular gegen `package_slug` und `package_interest_slugs` validiert, muss die Option backendseitig als erlaubter Anfrage-Slug persistierbar sein. Sie soll aber nicht in kundenseitigen Paketlisten erscheinen.
3. **Kapazitaet ist manuell und optisch.**
   Die Felder bilden eine redaktionelle Anzeige ab: freie Plaetze und Gesamtplaetze. Sie reduzieren sich nicht automatisch durch Buchungen und blockieren keine Anfrage.
4. **Nur vollstaendige Kapazitaet wird angezeigt.**
   Kundenseitig wird die Kapazitaetsanzeige nur gerendert, wenn freie und gesamte Plaetze beide gepflegt und fachlich gueltig sind.
5. **Rote Absetzung fuer Knappheit.**
   Die Anzeige soll als klar separater roter Hinweis erscheinen, ohne den restlichen Campkalender einfarbig oder alarmistisch zu ueberfrachten.
6. **Startseitenreihenfolge bleibt ruhig.**
   Hero bleibt der erste Screen. Danach kommt `AboutUs`, anschliessend Campkalender, Pricing, Gallery, FinalCTA und BookingGuide in einer sinnvollen Reihenfolge.
7. **Bestehende Package-UI bleibt stabil.**
   Die vier regulaeren Pakete bleiben unveraendert in Pricing, Empfehlungen und bestehenden Camp-Preisfeldern.

## Bestandsaufnahme

| Bereich | Datei | Relevanter Kontext |
|---|---|---|
| Camp Events Schema | `migrations/039_create_camp_events.up.sql`, `migrations/040_alter_camp_events_add_detail_fields.up.sql` | `camp_events` existiert mit Detailfeldern, Sichtbarkeit, Slug und Paketempfehlung. Neue Kapazitaetsfelder muessen per neuer Migration additiv ergaenzt werden. |
| Camp Event Domain | `internal/camps/domain/entities.go` | `CampEvent` traegt Public/Admin Felder und zentrale `Validate()`-Regeln. |
| Camp Event Service | `internal/camps/app/service.go` | `CreateCampEventInput`, Create/Update-Use-Cases und Package-Validierung leben hier. |
| Camp Event Repository | `internal/camps/adapters/repository/postgres/camp_event_repository.go` | SELECT/INSERT/UPDATE muessen neue Spalten lesen und schreiben. |
| Camp Event HTTP | `internal/camps/adapters/http/handler.go` | Admin/Public DTOs und Mapping muessen neue Felder abbilden. |
| Admin Campeditor | `web/customer/src/app/admin/camp-events/page.tsx` | Bestehendes Formular pflegt Slug, Texte, Sichtbarkeit und saisonale Preise. Neue Kapazitaetsfelder passen in diese Editoroberflaeche. |
| Public Campkalender | `web/customer/src/components/NextCampBanner.tsx` | Listet sichtbare Camp Events und zeigt Detailkarte fuer ausgewaehltes Camp. Kapazitaet soll dort optisch separat erscheinen. |
| Anfrageformular | `web/customer/src/app/checkout/page.tsx` | Paketinteressen werden aktuell aus `CAMP_PACKAGES` gerendert und als `package_slug`/`package_interest_slugs` gesendet. |
| Paketdefinitionen | `web/customer/src/lib/camp-packages.ts` | Enthaelt die vier regulaeren sichtbaren Pakete. Die neue Anfrageoption soll nicht in dieser Pricing-Liste erscheinen. |
| Startseite | `web/customer/src/app/page.tsx` | Aktuelle Reihenfolge: Hero, Campkalender, Pricing, Gallery, AboutUs, FinalCTA, BookingGuide. |

## Waves

| Wave | Skill | Agent | Focus | Status |
|---|---|---|---|---|
| Wave 1 | migration-eng | Migration Engineer | Camp-Kapazitaetsfelder additiv migrieren | Done |
| Wave 2 | service-eng | Backend Service Engineer | Domain, Repository, Service und Booking-Anfrageoption erweitern | Done |
| Wave 3 | handler-eng | Handler/API Edge Engineer | HTTP Contracts fuer Camp Events und Booking-Anfrageoption mappen | Done |
| Wave 4 | frontend-eng | Frontend Engineer | Admin-Campeditor, Campkalender, Anfrageformular und Homepage-Reihenfolge umsetzen | Done |
| Wave 5 | testing-eng | QA Engineer | Backend/Web Regression, Build, Smoke und Scope-Audit | Done |

## Critical Path

```text
Wave 1 (DB-Kapazitaetsfelder)
    v
Wave 2 (Domain, Service, Repository, Booking-Option)
    v
Wave 3 (HTTP DTOs und API-Contracts)
    v
Wave 4 (Admin- und Customer-Web UI)
    v
Wave 5 (Regression, Build, Smoke, Scope-Audit)
```

Wave 1 blockiert die persistente Kapazitaetsarbeit. Wave 2 und Wave 3 muessen sequenziell bleiben, damit Frontend und Tests gegen einen stabilen API-Contract arbeiten. Wave 4 kann die reine Homepage-Reihenfolge frueh anfassen, soll aber finale Kapazitaets- und Anfrageformulararbeit erst nach Wave 3 integrieren. Wave 5 startet nach allen funktionalen Aenderungen.

## Risiken und Mitigationen

| Risiko | Impact | Mitigation |
|---|---|---|
| "Gruppenanfrage / Individualanfrage" rutscht versehentlich in Pricing oder Camp-Empfehlungen | Hoch | Separaten Anfrageoptions-Datensatz nur fuer Checkout/Formular verwenden; finaler Scope-Audit mit `rg`. |
| Backend lehnt neue Anfrageoption ab, weil sie nicht in `campPackages` enthalten ist | Hoch | Service-Wave erweitert die erlaubten Anfrage-Slugs bewusst, ohne die sichtbaren Paketdefinitionen zu vermischen. |
| Kapazitaetswerte werden inkonsistent gespeichert, z. B. 14 frei von 12 | Hoch | DB-Check-Constraint plus Domain-/Handler-Validierung. |
| Null-/Leerwerte werden kundenseitig als fehlerhafte Anzeige gerendert | Mittel | Public UI zeigt Kapazitaet nur bei vollstaendig gueltigen Werten. |
| Rote Limitierungsanzeige wirkt wie Fehlermeldung oder dominiert den Campkalender | Mittel | Frontend-Wave setzt einen separaten, kleinen roten Hinweis mit ruhiger Typografie. |
| Startseiten-Umsortierung verschiebt Anchors oder erwartete Navigation | Mittel | IDs und bestehende Links erhalten; nur Komponentenreihenfolge anpassen. |
| Migration verursacht vermeidbare Locks | Niedrig | Additive nullable Integer-Spalten ohne Default; kurze Constraints bewusst definieren. |
| Tests werden grossflaechig statt zielgerichtet | Niedrig | Wave 5 definiert gezielte Go- und Web-Suites plus Build/Lint als Quality Gate. |

## Gesamt-Akzeptanzkriterien

- [x] `camp_events` hat optionale Felder fuer freie und gesamte Plaetze mit Up-/Down-Migration.
- [x] Kapazitaetswerte erlauben `NULL`/leer, aber keine negativen Werte und keine freien Plaetze groesser als Gesamtplaetze.
- [x] Admins koennen im Campeditor freie und gesamte Plaetze erstellen, bearbeiten und wieder leeren.
- [x] Public Camp Events liefern Kapazitaetsfelder an das Customer-Web aus.
- [x] Der Campkalender zeigt bei gepflegter Kapazitaet einen klar abgesetzten roten Hinweis wie `10 von 12 Plaetzen frei`.
- [x] Ohne gepflegte oder unvollstaendige Kapazitaet erscheint keine defekte Platzanzeige.
- [x] Das Anfrageformular zeigt zusaetzlich "Gruppenanfrage / Individualanfrage" in der Paketauswahl.
- [x] Diese Anfrageoption erscheint nicht in Pricing, Paketkarten, Camp-Empfehlungen oder regulaeren Paketdefinitionen.
- [x] Eine Anfrage mit dieser Option wird backendseitig akzeptiert und lesbar persistiert.
- [x] Mehrfachauswahl bleibt fuer bestehende Pakete und die neue Anfrageoption stabil.
- [x] Auf der Startseite steht `AboutUs` direkt nach dem Hero und vor dem Campkalender.
- [x] Bestehende Anchors und CTAs bleiben funktional.
- [x] Relevante Backend-Tests sind gruen oder Blocker sind konkret dokumentiert.
- [x] Relevante Customer-Web Tests, Build und Lint sind gruen oder Blocker sind konkret dokumentiert.
- [x] Finaler Scope-Audit bestaetigt: keine Planungsdateien im Code-Repo, keine Secrets, keine ungewollten Paket-/Pricing-Aenderungen.

## Notes

Planung freigegeben am 2026-04-27. Die Iteration wird in fuenf Waves umgesetzt: zuerst persistente Kapazitaetsgrundlage, dann Backend-Service und API-Contracts, danach Admin-/Customer-Frontend und abschliessend gezielte Regression.

Abschluss 2026-04-27:
- Wave 1 bis Wave 5 abgeschlossen.
- Migration `048_alter_camp_events_add_capacity_fields` fuegt `capacity_available` und `capacity_total` zu `camp_events` hinzu, inklusive Non-negative- und `available <= total`-Constraints sowie Down-Rollback.
- Backend-Domain, Service, Repository und HTTP-DTOs tragen Kapazitaetswerte durch Public/Admin Camp Event Flows.
- Booking-Service akzeptiert `group-individual` als anfrage-only Option mit Snapshot-Name `Gruppenanfrage / Individualanfrage` und `0 EUR`; Camp-Empfehlungen bleiben auf regulaere Pakete beschraenkt.
- Customer-Web API-Typen, Admin-Campeditor, Campkalender, Anfrageformular und Homepage-Reihenfolge aktualisiert.
- `CAMP_PACKAGES` bleibt bei vier regulaeren Paketen; `CHECKOUT_PACKAGE_OPTIONS` ergaenzt die Anfrageoption nur fuer Checkout.
- Automatisierte Verifikation gruen: `go test ./internal/camps/...`, `go test ./...`, `npm test -- --configLoader runner`, `npm run build`, `npm run lint`.
- Browser-Smoke lokal nicht moeglich: kein Chromium/Chrome und keine Playwright-Specs vorhanden. Statischer UI-/Scope-Audit plus Build/Lint/Test-Gates wurden dokumentiert.
