# Wave 3 — SMTP-Test-Provider (Mailtrap Sandbox)

**Skill:** `/infrastructure-eng`
**Status:** Planned
**Blocked by:** —

## Ziel

Für die Master-Template-Demoumgebung einen risikofreien SMTP-Provider
anbinden, der Mails NICHT an echte Empfänger ausliefert, aber den
Rejection-Workflow vollständig testbar macht. Empfehlung: **Mailtrap
Sandbox** (Free-Tier reicht).

Pro Kunden-Clone wird dieser Provider später durch einen echten ersetzt
(Postmark, Mailgun, Mailjet etc.) — das ist nicht Teil dieser Iteration.

## Tasks

1. **Mailtrap-Account vorbereiten.**
   - Bei [mailtrap.io](https://mailtrap.io) Account anlegen.
   - „Sandbox" auswählen → eine Inbox „website-template-staging" erstellen.

2. **SMTP-Credentials kopieren.**
   - Mailtrap → Inbox → „SMTP Settings".
   - Notieren:
     - `host` (i. d. R. `sandbox.smtp.mailtrap.io`)
     - `port` (`2525`, `465`, oder `587`)
     - `username`
     - `password`

3. **Variablen in Railway Backend setzen** (Wave 2 trägt sie ein,
   hier kommen die Werte rein):
   - `SMTP_HOST=sandbox.smtp.mailtrap.io`
   - `SMTP_PORT=587`
   - `SMTP_USERNAME=<aus Mailtrap>`
   - `SMTP_PASSWORD=<aus Mailtrap>`
   - `MAIL_FROM_ADDRESS=karriere@template.example`
   - `MAIL_FROM_NAME=Mittelstand Website`

4. **Smoke-Test direkt in der Anwendung.**
   - In Admin-UI eine Bewerbung anlegen (Wave 4 deckt das ab).
   - Status auf `Abgelehnt` setzen.
   - In Mailtrap-Inbox prüfen, dass die Rejection-Mail eintrifft.

## Akzeptanzkriterien

- [ ] Mailtrap-Sandbox-Inbox „website-template-staging" existiert.
- [ ] SMTP-Variablen in Railway Backend gesetzt.
- [ ] Beim Backend-Start fehlt das Log `rejection mailer disabled`.

## Out of Scope

- Mailtrap-Production-Plan (echtes Sending).
- Eigenes Mail-Template-Design (aktuell festes Template in
  `submission/mail_smtp.go`).
- DKIM/SPF/DMARC-Setup einer eigenen Sender-Domain — folgt im Kunden-Clone.

## Risiken / Stolpersteine

- Mailtrap-Sandbox blockiert Live-Sending — perfekt für Tests, ungeeignet
  für Live-Mails. Vor Go-Live pro Kunde umschalten.
- `MAIL_FROM_ADDRESS` muss eine gültig formatierte E-Mail sein, sonst
  schlägt der Versand fehl. Eine fiktive Domain wie
  `karriere@template.example` ist OK für Sandbox.

## Notizen für Codex

- Falls Mailtrap nicht passt, alternativer Test-Provider: Ethereal Email
  (`ethereal.email`) — generiert SMTP-Credentials einmalig pro Inbox.
- Falls direkt produktiv: Postmark Server-Token (kostenlos bis 100
  Mails/Monat, sehr einfaches Setup).
