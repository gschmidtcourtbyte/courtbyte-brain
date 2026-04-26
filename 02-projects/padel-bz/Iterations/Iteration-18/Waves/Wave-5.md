# Wave 5: Tests und manuelle QA

**Status:** Done (Legacy-Migration)
**Skill:** frontend-eng, testing-eng
**Quelle:** `plan/iterations/iteration-18.md`

## Legacy-Inhalt

## Wave 5 - Tests und manuelle QA
**Agenten**: Agent E, Agent B, Agent C
**Kann parallel ausgefuehrt werden**: ja, automatisierte Tests und visuelle QA koennen parallel laufen
**Ziel**: Die Iteration ist regressionssicher und visuell pruefbar.

### Task 18.12: Tests fuer Rechtstextseiten
**Priority**: P0
**Blocked by**: 18.3, 18.4
**Agent**: Agent E (testing-eng)

**Akzeptanzkriterien**:
- [ ] Render-Test fuer `impressum.html` bleibt gruen.
- [ ] Render-Test fuer `datenschutz.html` bleibt gruen.
- [ ] Tests pruefen, dass zentrale neue Inhalte sichtbar sind:
  - `courts padelclub GmbH`
  - `Christian Haake`
  - `Henrik Haake`
  - `Landesbeauftragte fuer den Datenschutz Niedersachsen`
- [ ] Tests pruefen, dass alte Platzhalter wie `[Name des Betreibers]` nicht mehr gerendert werden.
- [ ] Route-Exemptions fuer `/impressum` und `/datenschutz` bleiben gruen.

**Kontext**:
- `internal/handler/render_test.go`
- `internal/handler/site_auth_test.go`
- `web/templates/pages/impressum.html`
- `web/templates/pages/datenschutz.html`

**Output**:
- Aktualisierte Render-/Auth-Tests.

### Task 18.13: Tests fuer E-Mail-Rendering und Versandvertrag
**Priority**: P0
**Blocked by**: 18.9, 18.10, 18.11
**Agent**: Agent E (testing-eng)

**Akzeptanzkriterien**:
- [ ] Alle HTML-E-Mail-Templates lassen sich parsen und rendern.
- [ ] Alle Text-E-Mail-Templates lassen sich parsen und rendern.
- [ ] Betreiber-Mail-Test prueft Empfaenger `SMTP_TO`.
- [ ] Config-Tests pruefen `SMTP_FROM` und `SMTP_TO` Default `hello@courts-padelclub.de`.
- [ ] Anfragenden-Bestaetigungs-Test prueft Empfaenger `submission.Email`.
- [ ] Hansefit-Test prueft Empfaenger Playtomic-E-Mail und Fallback Kontakt-E-Mail.
- [ ] MIME-Test prueft `multipart/alternative`.
- [ ] MIME-Test prueft, dass `text/plain` vor `text/html` kommt.
- [ ] Header-Sanitizing-Test deckt Betreff mit CR/LF ab.
- [ ] HTML-Escape-Test prueft, dass eine Formularnachricht mit `<script>` nicht als HTML ausgefuehrt wird.
- [ ] `go test ./...` ist gruen.

**Kontext**:
- `internal/service/contact_service_test.go`
- `web/templates/emails/`

**Output**:
- Erweiterte Service-Tests fuer E-Mail-Rendering.

### Task 18.14: Manuelle QA fuer Seiten und E-Mails
**Priority**: P1
**Blocked by**: 18.3, 18.4, 18.10
**Agent**: Agent B (frontend-eng), Agent C (frontend-eng), Agent E (testing-eng)

**Akzeptanzkriterien**:
- [ ] `/impressum` ist auf Desktop und Mobile lesbar.
- [ ] `/datenschutz` ist auf Desktop und Mobile lesbar.
- [ ] Rechtstextseiten haben keine sichtbaren Template-Artefakte oder Platzhalter.
- [ ] Footer-Links fuehren weiterhin auf `/impressum` und `/datenschutz`.
- [ ] HTML-Mail-Vorschau zeigt Logo/Wordmark, Header, Content und Footer korrekt.
- [ ] Plain-Text-Mail ist vollstaendig lesbar.
- [ ] Bilder-blockiert-Szenario bleibt verstaendlich durch Alt-Text/Wordmark.
- [ ] Lange Nachrichten im Betreiber-Mailtemplate brechen Layout nicht.

**Kontext**:
- Lokaler Dev-Server oder Template-Render-Tests.
- SMTP-Capture/Mailhog optional, falls lokal vorhanden.

**Output**:
- Kurze QA-Notiz mit getesteten Viewports und Mailtypen.

**Wave-5-Exit-Gate**:
- [ ] Automatisierte Tests sind gruen.
- [ ] Desktop/Mobile Legal-QA ist bestanden.
- [ ] E-Mail-Preview/Rendering-QA ist bestanden.

---
