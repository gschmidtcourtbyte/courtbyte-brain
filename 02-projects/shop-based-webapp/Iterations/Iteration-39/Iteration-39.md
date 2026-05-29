# Iteration 39 - FAQ-Layout, Social-Link und Add-on-Preise

**Status:** Done
**Repo:** /home/andersen/git/shop-based-webapp
**Datum:** 2026-05-21
**Rolle:** Acting as Product Owner. Planning Iteration 39 for shop-based-webapp.

## Ziel

Iteration 39 macht die neue Camp-Add-on-Struktur aus Iteration 38 fachlich sichtbar: Transfer, Vollpension und Einzeltrainerstunden erhalten vorlaeufige Content-Preise und werden in Campsumme, Anzahlung und Restzahlung eingerechnet. Gleichzeitig wird die OUT-Padel-FAQ ohne Accordion neu gestaltet und der Instagram-Social-Link auf die Basis-Seite `padel.out` korrigiert.

## Scope

**In:**
- FAQ-UI im Customer-Web ueberarbeiten:
  - FAQ-Inhalte bleiben dauerhaft sichtbar.
  - Das bisherige Ausklapp-/Accordion-Verhalten wird entfernt.
  - Layout bleibt auf Mobile und Desktop hochwertig, scanbar und konsistent zur OUT-Padel-Seite.
- Instagram-Social-Link auf `https://www.instagram.com/padel.out/` setzen.
- Vorlaeufige Add-on-Preise aktivieren:
  - Flughafentransfer: `50 EUR`.
  - Einzeltrainerstunde: `30 EUR` je Stunde.
  - Vollpension: `400 EUR`.
- Preise an die vorhandene Iteration-38-Preislogik anbinden:
  - Add-on-Summe fliesst in die Campsumme ein.
  - 20-Prozent-Anzahlung und Restzahlung werden aus der aktualisierten Summe berechnet.
  - Preis-Snapshots bleiben pro Camp-Anfrage nachvollziehbar.
- Checkout-/Admin-Anzeige dort aktualisieren, wo Nutzende oder Admins die Add-on-Preise und resultierenden Summen sehen muessen.
- Relevante Backend-, Frontend- und Pricing-Regression pruefen.
- Railway-Rollout nur nach `staging`; kein Production-Deploy.

**Out:**
- Finales fachliches Pricing-Modell oder Preisverwaltung im Adminportal. Die Preise sind vorlaeufige Content-Preise.
- Payment-Provider-Integration oder automatische Zahlungsreconciliation.
- Neues FAQ-CMS oder kompletter FAQ-Content-Rewrite.
- Weitere Social-Link- oder Footer-Redesigns ausserhalb des Instagram-Ziels.
- Production-Rollout.

## Produktentscheidungen

1. **FAQ dauerhaft sichtbar.**
   FAQ-Fragen und Antworten werden nicht mehr hinter Interaktionszustand versteckt. Designqualitaet und Scanbarkeit ersetzen das Accordion.
2. **Preise als naechster Schritt der bestehenden Add-on-Struktur.**
   Iteration 38 hat Add-on-Auswahl und Snapshot-Felder vorbereitet. Iteration 39 setzt die ersten fachlichen Werte in Cent an und laesst das Snapshot-Prinzip bestehen.
3. **Preise sind vorlaeufig.**
   Die Werte stammen vorerst aus Content-/Produktannahmen. Alte Buchungen duerfen durch spaetere Preiswechsel nicht rueckwirkend veraendert werden.
4. **Staging zuerst.**
   Railway-Aktionen fuer diese Iteration sind auf OUT Padel `staging` begrenzt, bis Production explizit freigegeben wird.

## Waves

| Wave | Skill | Focus | Status |
|---|---|---|---|
| Wave 1 | service-eng | Add-on-Preise in Service-Preislogik, Summen und Mail-Betraege aktivieren | Done |
| Wave 2 | frontend-eng | FAQ redesignen, Instagram-Link korrigieren und Add-on-Preise im UI sichtbar machen | Done |
| Wave 3 | testing-eng | Pricing-, FAQ-, Social- und Checkout-Regression verifizieren | Done |
| Wave 4 | infrastructure-eng | Staging-only Railway-Rollout und Smoke-Checks | Done |

## Critical Path

```text
Wave 1 (Pricing-Logik)
    v
Wave 2 (FAQ, Social und Pricing UI)
    v
Wave 3 (Verification)
    v
Wave 4 (Staging rollout)
```

## Acceptance Criteria

- [x] FAQ zeigt alle Fragen und Antworten ohne Ausklappinteraktion dauerhaft.
- [x] FAQ bleibt auf Mobile und Desktop gut lesbar und visuell sauber in der bestehenden OUT-Padel-Sprache.
- [x] Instagram-Social-Link fuehrt auf `https://www.instagram.com/padel.out/`.
- [x] Flughafentransfer addiert `50 EUR` zur Camp-Anfrage.
- [x] Vollpension addiert `400 EUR` zur Camp-Anfrage.
- [x] Einzeltrainerstunden addieren `30 EUR` je angefragter Stunde.
- [x] Add-on-Preiswerte werden in den Booking-Snapshots gespeichert und in Booking-Responses nachvollziehbar zurueckgegeben.
- [x] Gesamtpreis, 20-Prozent-Anzahlung und Restbetrag verwenden die Add-on-Summe.
- [x] Zahlungs-E-Mails fuer Anzahlung und Restzahlung erhalten die Summen aus dem Snapshot, nicht aus spaeteren Preisupdates.
- [x] Checkout bleibt auch ohne Add-ons absendbar.
- [x] Admin-Darstellung bleibt fuer Add-on-Auswahl und Preise scanbar.
- [x] Relevante Backend-Tests, Frontend-Checks und Pricing-Smokes sind gruen oder konkrete Blocker dokumentiert.
- [x] Railway-Deploy erfolgt nur nach `staging`; Production bleibt unangetastet.

## Risiken und Entscheidungen

- Die Preise sind fachlich vorlaeufig. Zentral gesetzte Cent-Werte muessen spaeter bewusst geaendert werden koennen, ohne alte Snapshots umzuschreiben.
- Iteration 38 ist im Vault noch `In Progress`; Iteration 39 haengt fachlich an deren Add-on- und Payment-Snapshot-Grundlage.
- FAQ-Umbau ist ein UI-Risiko auf schmalen Viewports, weil alle Antworten dauerhaft Platz einnehmen.
- UI-Preise und Backend-Preise muessen dieselben Werte kommunizieren; Tests sollen Preisdrift sichtbar machen.

## Notes

Planung am 2026-05-21 nach User-Freigabe geschrieben. Gegenueber der Scope-Diskussion wurde eine eigene Infrastruktur-Wave aufgenommen, weil der User den Rollout explizit auf Railway Staging begrenzt hat.
