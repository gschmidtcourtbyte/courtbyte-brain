# Wave 4: Service-Integration fuer E-Mail-Templates

**Status:** Done (Legacy-Migration)
**Skill:** service-eng, security-eng
**Quelle:** `plan/iterations/iteration-18.md`

## Legacy-Inhalt

## Wave 4 - Service-Integration fuer E-Mail-Templates
**Agenten**: Agent D, Agent E, Agent S
**Kann parallel ausgefuehrt werden**: teilweise; Tests koennen parallel vorbereitet werden, Service-Integration bleibt sequenziell
**Ziel**: Der bestehende E-Mail-Versand nutzt die neuen Templates robust und ohne Regressionen.

### Task 18.9: E-Mail-Renderer fuer HTML + Plain Text implementieren
**Priority**: P0
**Blocked by**: 18.2, 18.6, 18.8
**Agent**: Agent D (service-eng)

**Akzeptanzkriterien**:
- [ ] Es gibt eine zentrale Render-Funktion fuer E-Mail-Templates.
- [ ] HTML- und Text-Templates werden aus `web/templates/emails/` geladen.
- [ ] Rendering-Fehler enthalten Template-Namen im Fehlerkontext.
- [ ] Der MIME-Body wird als `multipart/alternative` erzeugt.
- [ ] Plain-Text-Part kommt vor HTML-Part.
- [ ] Boundary wird sicher erzeugt und kollidiert nicht mit Content.
- [ ] Header-Sanitizing fuer `From`, `To`, `Subject` bleibt erhalten.
- [ ] `Content-Type` enthaelt `charset=UTF-8`.
- [ ] Tests koennen den SMTP-Sendepfad weiterhin mocken.

**Kontext**:
- `internal/service/contact_service.go`
- `internal/service/contact_service_test.go`
- `web/templates/emails/`

**Output**:
- Zentraler E-Mail-Renderer und MIME-Builder im Service-Layer oder in einer kleinen service-internen Hilfsdatei.

### Task 18.10: Kontakt- und Hansefit-Mails auf Templates umstellen
**Priority**: P0
**Blocked by**: 18.9
**Agent**: Agent D (service-eng)

**Akzeptanzkriterien**:
- [ ] Betreiber-Benachrichtigung nutzt `contact_notification.html` + `.txt`.
- [ ] Anfragenden-Bestaetigung nutzt `contact_confirmation.html` + `.txt`.
- [ ] Hansefit-Bestaetigung nutzt `hansefit_confirmation.html` + `.txt`.
- [ ] Keine laengeren Mail-Bodies bleiben als Go-String-Bloecke im Service.
- [ ] Empfaengerlogik bleibt unveraendert:
  - Betreiber-Mail an `SMTP_TO`, Fallback `SMTP_FROM`.
  - Anfragenden-Bestaetigung an `submission.Email`.
  - Hansefit-Bestaetigung an Playtomic-E-Mail, Fallback Kontakt-E-Mail.
- [ ] Betreffzeilen bleiben im Code oder zentral konfigurierbar, aber sanitisiert.
- [ ] `AppURL` wird fuer absolute Logo-/Asset-URLs genutzt.
- [ ] Wenn `AppURL` lokal ist, darf E-Mail-Rendering nicht fehlschlagen; LogoURL kann dann leer oder lokal sein.
- [ ] Fehler beim asynchronen Kontakt-Mailversand blockieren das Formular weiterhin nicht.
- [ ] Fehler beim Hansefit-Bestaetigungsversand verhindern weiterhin das Markieren als bearbeitet, wie bisher.

**Kontext**:
- `internal/service/contact_service.go`
- `internal/model/contact.go`
- `internal/config/config.go`

**Output**:
- Template-basierter Versand fuer alle Kontakt-/Hansefit-Mails.

### Task 18.11: Security- und Datenschutz-Haertung im Mailpfad
**Priority**: P1
**Blocked by**: 18.9, 18.10
**Agent**: Agent S (security-eng), Agent D (service-eng)

**Akzeptanzkriterien**:
- [ ] Header-Injection-Test bleibt vorhanden oder wird erweitert.
- [ ] HTML-Templates escapen dynamische Werte ueber Go `html/template`.
- [ ] Plain-Text-Templates nutzen `text/template`.
- [ ] Keine untrusted Eingabe wird als `template.HTML` markiert.
- [ ] Betreiber-Mail darf Formular-Nachricht enthalten, aber HTML-escaped.
- [ ] Anfragenden-Mail enthaelt keine interne Submission-ID und keine IP-Adresse.
- [ ] Keine Tracking-Pixel oder externen Drittanbieter-Assets.
- [ ] Logo-Asset kommt ausschliesslich von eigener Domain/AppURL.

**Kontext**:
- `internal/service/contact_service.go`
- `web/templates/emails/*.html`
- `web/templates/emails/*.txt`

**Output**:
- Abgesicherter Mail-Rendering-Pfad.

**Wave-4-Exit-Gate**:
- [ ] Alle Mailtypen rendern HTML und Plain Text.
- [ ] SMTP-Ausgabe ist multipart/alternative.
- [ ] Empfaengerlogik ist unveraendert und getestet.
- [ ] Security-relevante Template-/Header-Risiken sind adressiert.

---
