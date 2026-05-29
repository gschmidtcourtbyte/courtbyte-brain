# Iteration 7 — Karriereportal UX, Uploads, Mails und Admin-Badge

**Status:** Implemented
**Repo:** /home/andersen/git/website-template
**Datum:** 2026-05-14
**Bearbeitung:** Codex MCP Server

## Ziel

Das Karriereportal soll fuer Bewerber und Admins eindeutig rueckmelden, dass
eine Bewerbung eingegangen ist. Gleichzeitig wird der Upload-Flow produktionsnah:
mehrere Dateien, Dateiliste im Formular, Drag and Drop, 10 MB Gesamtlimit und
einzelne Admin-Downloads. Neue Bewerbungen sollen echte Admin-Badges treiben
und E-Mail-Benachrichtigungen ausloesen.

## Scope

**In:**

- Bewerbungsformular schliesst nach erfolgreichem Versand.
- Dauerhafte, sichtbare UI-Bestaetigung "Bewerbung eingegangen".
- Mehrdatei-Upload mit Dateiliste, Entfernen einzelner Dateien und Drag and Drop.
- Serverseitiges Gesamtlimit 10 MB und bis zu 10 Dateien.
- Bewerberbestaetigung per SMTP mit Beschreibung des weiteren Prozesses.
- Interne Benachrichtigung an konfigurierbare `APPLICATION_NOTIFICATION_EMAIL`.
- Admin-Summary-API fuer echte Badge-Zahlen.
- Karriereportal-Badge im Admin-Menue aus echten neuen Bewerbungen statt Mock.
- Uploads im Admin weiterhin einzeln herunterladbar.
- Doku/Runbook-Variablen fuer neue Mail-Konfiguration.

**Out:**

- HTML-Mail-Templates.
- Malware-Scanning-Pipeline.
- Mail-Retry/Outbox mit Zustellstatus.
- Vollstaendige Admin-Dashboard-Statistik abseits der Sidebar-Badges.

## Entscheidungen

- Mails sind nicht blockierend: Bewerbung und Statuswechsel bleiben gespeichert,
  auch wenn SMTP fehlt oder temporaer ausfaellt.
- SMTP-Konfiguration bleibt zentral. Der bestehende SMTP-Mailer sendet nun
  Ablehnung, Eingangsbestätigung und interne Benachrichtigung.
- Admin-Badge zaehlt `career_applications.status = 'new'`; Kontakt-Badge zaehlt
  ungelesene Anfragen.
- Upload-Grenze ist 10 MB insgesamt. Zusaetzlich gibt es ein Missbrauchslimit
  von 10 Dateien pro Bewerbung.

## Waves

| Wave | Skill | Focus | Status |
|---|---|---|---|
| Wave 1 | product-own | Scope, Akzeptanzkriterien, Critical Path | Done |
| Wave 2 | service-eng | Backend-Mailer, Summary, Upload-Limits | Done |
| Wave 3 | handler-eng | Admin/Public API-Routen und Tests | Done |
| Wave 4 | frontend-eng | Bewerbungs-UX, Drag and Drop, Admin-Badge | Done |
| Wave 5 | testing-eng/documentation-eng | Tests, Build, README/Runbook | Done |

## Acceptance Criteria

- [x] Bewerber sieht nach erfolgreichem Absenden eine deutliche Bestaetigung.
- [x] Formular wird nach erfolgreichem Absenden geschlossen.
- [x] Mehrere Dateien koennen per Auswahl und Drag and Drop hinzugefuegt werden.
- [x] Ausgewaehlte Dateien werden im Formular einzeln angezeigt und sind entfernbar.
- [x] Backend und Frontend erzwingen 10 MB Gesamtlimit.
- [x] Admin kann Uploads je Datei herunterladen.
- [x] Admin kann Bewerbungsstatus weiterhin aendern.
- [x] Karriereportal-Badge basiert auf echten neuen Bewerbungen.
- [x] Neue Bewerbung sendet Bewerberbestaetigung, sofern SMTP konfiguriert ist.
- [x] Neue Bewerbung sendet interne Benachrichtigung an konfigurierbare Adresse.
- [x] Relevante Tests und Build laufen gruen.

## Verification

- `go test ./...` im Backend: gruen.
- `npm run lint` im Frontend: gruen.
- `npm run typecheck` im Frontend: gruen.
- `npm run build` im Frontend: gruen.
- `npm test` im Frontend: nicht vorhanden (`Missing script: "test"`).

## Risiken / Follow-up

- Ohne SMTP-Credentials werden Mails bewusst nicht gesendet, aber Bewerbungen
  bleiben erfolgreich. Monitoring/Outbox kann spaeter ergaenzt werden.
- Die interne Benachrichtigung braucht `APPLICATION_NOTIFICATION_EMAIL`; ohne
  diese Variable wird nur die Bewerberbestaetigung aktiviert.
- Malware-Scanning ist weiterhin ein spaeteres Security-Thema.
