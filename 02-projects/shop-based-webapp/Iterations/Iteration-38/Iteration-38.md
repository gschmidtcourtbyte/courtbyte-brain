# Iteration 38 - Camp-Add-ons und Zahlungsstatus

**Status:** In Progress
**Repo:** /home/andersen/git/shop-based-webapp
**Datum:** 2026-05-18
**Rolle:** Acting as Product Owner. Planning Iteration 38 for shop-based-webapp.

## Ziel

Iteration 38 erweitert die Camp-Anfrage um optionale Zusatzleistungen und macht daraus einen zweistufigen Anfragefluss wie bei einer Flugbuchung: zuerst Camp/Paket und Kontaktdaten, danach optionale Extras. Im Admin-Campportal wird der Zahlungsprozess feiner steuerbar: Anfrage bearbeitet, Anzahlung erfolgt, Restzahlungsaufforderung und Restzahlung erfolgt. Die E-Mails fuer Anzahlung und Restzahlung werden aus dem jeweiligen Statuswechsel generiert und verwenden die berechnete Campsumme.

## Scope

**In:**
- Checkout Step 2 fuer optionale Zusatzleistungen:
  - Flughafentransfer als Checkbox.
  - Vollpension als Checkbox.
  - Einzeltrainerstunden als optionale positive Stundenanzahl.
- Backend-Persistenz fuer Add-on-Auswahl und Preis-Snapshots an `camp_bookings`.
- Preisberechnung vorbereiten:
  - Basispreis bleibt aus Paket/Camp-Event.
  - Add-on-Preise sind initial `0`, koennen spaeter zentral ergaenzt werden und werden auf die Campsumme addiert.
  - Snapshot-Felder sichern Gesamtpreis, Anzahlung und Restzahlung pro Anfrage.
- Anzahlungsmail bei Statuswechsel auf `confirmed` / Anzeige `Anfrage bearbeitet`:
  - Betrag ist 20 Prozent der Campsumme.
  - Mail enthaelt Gesamtpreis, Anzahlungsbetrag, Restbetrag, Bankdaten und Referenz.
- Restzahlungsaufforderung im Admin-Campportal:
  - Neuer Status `balance_requested` / Anzeige `Restzahlungsaufforderung`.
  - Statuswechsel triggert Restzahlungsmail mit Restbetrag.
- Admin-Campportal:
  - Statusauswahl statt nur zwei feste Buttons.
  - Anzeigenamen: `Anfrage bearbeitet`, `Anzahlung erfolgt`, `Restzahlungsaufforderung`, `Restzahlung erfolgt`.
  - Add-on-Auswahl und Zahlungsbetraege in der Buchungsliste sichtbar.
- API- und OpenAPI-Anpassungen fuer neue Request-/Response-Felder und Admin-Statuswechsel.
- Tests fuer Migration, Domain-Statusfluss, Preisberechnung, Mail-Templates, Handler und Frontend-Formularlogik.
- Optionaler Railway-Migrations-/Deployment-Check nach erfolgreicher Implementierung, falls eine Zielumgebung explizit freigegeben ist.

**Out:**
- Konkrete Add-on-Preise fachlich festlegen. Die technische Struktur wird vorbereitet; Preise bleiben initial `0`.
- Payment-Provider-Integration oder automatische Zahlungsreconciliation.
- Direkte manuelle Schema-Aenderungen in Railway/PostgreSQL ohne versionierte SQL-Migration.
- Re-Design der kompletten Checkout- oder Admin-Oberflaeche ausserhalb der benoetigten Status-/Add-on-Flows.
- Aenderungen an Camp-Event-Content, Paketnamen oder bestehenden Paketpreisen ausser wenn fuer Preisberechnung noetig.

## Statusmodell

| Status | Admin-Label | Bedeutung | Mail-Trigger |
|---|---|---|---|
| `pending` | Ausstehend | Anfrage ist eingegangen und noch nicht bearbeitet. | Eingangsmail wie bisher |
| `confirmed` | Anfrage bearbeitet | Anfrage wurde bestaetigt; Anzahlung wird angefordert. | Anzahlungsmail 20 Prozent |
| `deposit_paid` | Anzahlung erfolgt | Admin hat Zahlungseingang der Anzahlung markiert. | optional keine Gast-Mail, falls fachlich nicht anders entschieden |
| `balance_requested` | Restzahlungsaufforderung | Admin fordert den offenen Restbetrag an. | Restzahlungsmail |
| `paid` | Restzahlung erfolgt | Buchung ist voll bezahlt. | Zahlungsbestaetigung wie bisher, fachlich als Restzahlung erfolgt |
| `cancelled` | Storniert | Anfrage/Buchung wurde storniert. | keine neue Mail in dieser Iteration |

## Waves

| Wave | Skill | Focus | Status |
|---|---|---|---|
| Wave 1 | migration-eng | Camp-Booking-Schema fuer Add-ons, Preis-Snapshots und Statuswerte erweitern | Done |
| Wave 2 | service-eng | Domain-Statusfluss, Preisberechnung und Zahlungs-Mail-Trigger implementieren | Done |
| Wave 3 | handler-eng | API DTOs, Admin-Status-Endpunkt und OpenAPI aktualisieren | Done |
| Wave 4 | frontend-eng | Checkout Step 2 und Admin-Statussteuerung umsetzen | Done |
| Wave 5 | testing-eng | Regressionstests, Build/Lint und Migration-Smokes ausfuehren | Done |
| Wave 6 | infrastructure-eng | Optional Railway-Migration/Deployment fuer freigegebene Zielumgebung pruefen | Done |

## Critical Path

```text
Wave 1 (Schema)
    v
Wave 2 (Domain, Preislogik, Mail)
    v
Wave 3 (API)
    v
Wave 4 (Frontend)
    v
Wave 5 (Verification)
    v
Wave 6 (optional Railway rollout)
```

## Acceptance Criteria

- [ ] Camp-Anfrage kann ohne Zusatzleistungen gesendet werden.
- [ ] Camp-Anfrage kann mit Flughafentransfer und/oder Vollpension gesendet werden.
- [ ] Einzeltrainerstunden sind optional; `0`/leer ist erlaubt, negative Werte und nicht-ganzzahlige Stunden werden abgewiesen.
- [ ] Backend speichert Add-on-Auswahl und gibt sie in Booking-Responses zurueck.
- [ ] Preisberechnung addiert technische Add-on-Preise auf den Camp-Basispreis; initial bleiben Add-on-Preise `0`.
- [ ] Jede Buchung hat nachvollziehbare Snapshot-Werte fuer Basispreis, Add-ons, Gesamtpreis, 20-Prozent-Anzahlung und Restbetrag.
- [ ] Statuswechsel `pending` -> `confirmed` sendet eine Anzahlungsmail mit 20-Prozent-Anzahlungsbetrag.
- [ ] Statuswechsel auf `balance_requested` sendet eine Restzahlungsmail mit offenem Restbetrag.
- [ ] Admin kann die Status `Anfrage bearbeitet`, `Anzahlung erfolgt`, `Restzahlungsaufforderung` und `Restzahlung erfolgt` vergeben.
- [ ] Ungueltige Statusuebergaenge werden serverseitig abgelehnt.
- [ ] Admin-Dashboard zeigt Add-ons, Gesamtpreis, Anzahlung und Restbetrag scanbar in der Buchungsliste.
- [ ] Bestehende Buchungen bleiben kompatibel: leere Add-ons, Gesamtpreis aus vorhandenem `price_cents`, Anzahlung/Restbetrag daraus berechnet.
- [ ] OpenAPI und TypeScript-Typen sind aktualisiert.
- [ ] Relevante Backend-Tests und Frontend-Checks sind gruen oder bekannte Infrastruktur-Blocker sind dokumentiert.

## Risiken und Entscheidungen

- Preis-Snapshot ist wichtig, damit spaetere Add-on-Preisupdates alte Buchungen nicht rueckwirkend veraendern.
- Die Statuswerte werden als technische Slugs geplant; die deutschen Labels bleiben Frontend-/Admin-Copy.
- `paid` wird fachlich als `Restzahlung erfolgt` weiterverwendet, damit bestehende Integrationen mit dem Abschlussstatus nicht unnoetig brechen.
- Railway darf fuer die eigentliche Datenbankanpassung nur ueber die versionierte Migration genutzt werden, nicht ueber direkte manuelle SQL-Aenderungen.

## Notes

Planung erstellt am 2026-05-18 nach User-Freigabe. Der Umfang betrifft Backend, Datenbank, API, Customer-Checkout, Admin-Campportal und E-Mail-Templates. Critical Path startet mit einer additiven Migration, weil Service-, Handler- und Frontend-Arbeit auf denselben Booking-Feldern aufbauen.
