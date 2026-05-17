# Wave 2 — Backend Service

**Status:** Done
**Skill:** service-eng

## Ziel

Backend so erweitern, dass Bewerbungen Prozessmails ausloesen, echte
Admin-Zaehlwerte liefern und Upload-Limits konsistent validieren.

## Ergebnis

- `ApplicationConfirmationMailer` und `ApplicationNotificationMailer`
  ergaenzt.
- SMTP-Mailer sendet Bewerberbestaetigung und interne Benachrichtigung.
- `AdminSummary` zaehlt ungelesene Anfragen und neue Bewerbungen.
- Upload-Batch validiert maximal 10 Dateien und 10 MB Gesamtgroesse.
- Mailfehler blockieren keine gespeicherte Bewerbung.

## Akzeptanzkriterien

- [x] Neue Bewerbung wird gespeichert, auch wenn SMTP fehlt.
- [x] Mailer erhaelt korrekten Attachment-Count.
- [x] Summary liefert reale Zahlen aus PostgreSQL.
