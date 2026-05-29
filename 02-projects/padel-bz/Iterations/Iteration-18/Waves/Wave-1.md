# Wave 1: Foundation: Content- und E-Mail-Vertrag

**Status:** Done (Legacy-Migration)
**Skill:** service-eng, frontend-eng, security-eng, documentation-eng
**Quelle:** `plan/iterations/iteration-18.md`

## Legacy-Inhalt

## Wave 1 - Foundation: Content- und E-Mail-Vertrag
**Agenten**: PO Lead, Agent A, Agent C, Agent D, Agent S
**Kann parallel ausgefuehrt werden**: ja, Analyse-Tasks sind unabhaengig
**Ziel**: Bevor Templates gebaut werden, sind Quellen, Zielstruktur, offene fachliche Pruefpunkte und technische E-Mail-Konventionen eindeutig.

### Task 18.1: Rechtstext-Quellen mappen
**Priority**: P0
**Blocked by**: none
**Agent**: Agent A (documentation-eng)

**Akzeptanzkriterien**:
- [ ] `plan/impressum.md` ist als Quelle fuer `/impressum` dokumentiert.
- [ ] `plan/datenschutz.md` ist als Quelle fuer `/datenschutz` dokumentiert.
- [ ] Alle Hauptabschnitte der Quelldateien sind den Ziel-HTML-Strukturen zugeordnet.
- [ ] Alte Platzhalter in `web/templates/pages/impressum.html` und `web/templates/pages/datenschutz.html` sind als zu ersetzender Content identifiziert.
- [ ] Fachliche Pruefpunkte sind explizit notiert: `Hosting (Strato)` vs. Railway, Registerdaten "in Eintragung".
- [ ] Es ist entschieden: Inhalte werden ohne juristische Umformulierung uebernommen, nur HTML-semantisch strukturiert und escaped.

**Kontext**:
- `plan/impressum.md`
- `plan/datenschutz.md`
- `web/templates/pages/impressum.html`
- `web/templates/pages/datenschutz.html`

**Output**:
- Mapping-Notiz im PR/Umsetzungsbericht oder als kurzer Abschnitt in `docs/Content.md`.
- Liste offener fachlicher Pruefpunkte fuer den Kunden.

### Task 18.2: E-Mail-Template-Vertrag definieren
**Priority**: P0
**Blocked by**: none
**Agent**: Agent C (frontend-eng), Agent D (service-eng), Agent S (security-eng)

**Akzeptanzkriterien**:
- [ ] Zieltemplates sind festgelegt:
  - `web/templates/emails/contact_notification.html`
  - `web/templates/emails/contact_notification.txt`
  - `web/templates/emails/contact_confirmation.html`
  - `web/templates/emails/contact_confirmation.txt`
  - `web/templates/emails/hansefit_confirmation.html`
  - `web/templates/emails/hansefit_confirmation.txt`
- [ ] Gemeinsame Template-Daten sind definiert: Empfaenger, Betreff, LogoURL/BrandText, AppURL, Name, Kontaktfelder, Hansefit-Felder.
- [ ] Service-Kontrakt fuer multipart/alternative ist festgelegt: Plain Text zuerst, HTML danach.
- [ ] E-Mail-Header bleiben sanitisiert; dynamische Betreffzeilen duerfen keine Header-Injection ermoeglichen.
- [ ] Bei `SMTP_HOST == ""` bleibt Versand weiterhin ein No-op.
- [ ] Logo-Strategie ist entschieden:
  - bevorzugt robustes PNG/Web-Asset fuer Mailclients, abgeleitet aus dem vorhandenen SVG; oder
  - SVG nur mit klar dokumentiertem Fallback auf Text-Wordmark.
- [ ] HTML-E-Mails nutzen inline-kompatibles CSS, Tabellen/Container mit max. Breite, keine externen Fonts, kein JavaScript.
- [ ] Plain-Text-Fallbacks bleiben inhaltlich vollstaendig und unabhaengig vom HTML.

**Kontext**:
- `internal/service/contact_service.go`
- `web/templates/emails/hansefit_confirmation.txt`
- `web/static/img/logo_courts_transparent_mit_slogan.svg`
- `logo/logo_courts_transparent_mit slogan.svg`
- `internal/config/config.go` fuer `AppURL`, `SMTP_FROM`, `SMTP_TO`

**Output**:
- Technischer Mini-Kontrakt fuer die folgenden Implementierungs-Tasks.

**Wave-1-Exit-Gate**:
- [ ] Rechtstext-Quellen und Zielseiten sind eindeutig.
- [ ] E-Mail-Template-Dateien, Datenmodell und MIME-Strategie sind eindeutig.
- [ ] Offene fachliche Pruefpunkte sind dokumentiert und blockieren die technische Umsetzung nicht.

---
