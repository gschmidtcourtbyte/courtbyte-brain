# Wave 6: Doku, Review und Release-Readiness

**Status:** Done (Legacy-Migration)
**Skill:** security-eng, documentation-eng, review-eng
**Quelle:** `plan/iterations/iteration-18.md`

## Legacy-Inhalt

## Wave 6 - Doku, Review und Release-Readiness
**Agenten**: Agent A, Agent S, Agent R, PO Lead
**Kann parallel ausgefuehrt werden**: Doku und Security-Review parallel, finaler Review danach
**Ziel**: Die Iteration ist nachvollziehbar dokumentiert und bereit fuer Merge/Release.

### Task 18.15: Content- und Operations-Doku aktualisieren
**Priority**: P1
**Blocked by**: 18.3, 18.4, 18.8, 18.10
**Agent**: Agent A (documentation-eng)

**Akzeptanzkriterien**:
- [ ] `docs/Content.md` beschreibt, dass Impressum und Datenschutz aus den gelieferten Rechtstexten hinterlegt wurden.
- [ ] `docs/Content.md` listet die neuen E-Mail-Templates und ihre Platzhalter.
- [ ] `docs/Operations.md` beschreibt relevante SMTP-/AppURL-Abhaengigkeiten fuer Logo-URLs in E-Mails.
- [ ] Offene fachliche Pruefpunkte sind klar dokumentiert:
  - Hosting-Aussage pruefen.
  - alte Kontaktadressvarianten sind bereinigt oder als historische Referenz markiert.
  - Registerdaten ergaenzen, sobald vorhanden.

**Kontext**:
- `docs/Content.md`
- `docs/Operations.md`
- `plan/impressum.md`
- `plan/datenschutz.md`

**Output**:
- Aktualisierte Projekt-Doku.

### Task 18.16: Finaler Security-/Privacy-Review
**Priority**: P1
**Blocked by**: 18.11, 18.13
**Agent**: Agent S (security-eng), Agent R (review-eng)

**Akzeptanzkriterien**:
- [ ] Rechtstextseiten enthalten keine versehentlichen Secrets oder internen Umgebungsdetails.
- [ ] E-Mail-Templates enthalten keine IP-Adresse in Anfragenden-Mails.
- [ ] HTML-E-Mail-Dynamik ist escaped.
- [ ] Keine externen Drittanbieter-Assets werden in E-Mails geladen.
- [ ] AppURL/LogoURL kann in Produktion auf `https://courts-padelclub.de` zeigen.
- [ ] Known legal-content risks sind als fachliche Punkte dokumentiert, nicht als technische Blocker verschleiert.

**Kontext**:
- `web/templates/pages/impressum.html`
- `web/templates/pages/datenschutz.html`
- `web/templates/emails/*.html`
- `internal/service/contact_service.go`

**Output**:
- Review-Kommentar oder Abschlussnotiz mit Findings.

### Task 18.17: Merge-Readiness und Release-Check
**Priority**: P0
**Blocked by**: 18.12, 18.13, 18.14, 18.15, 18.16
**Agent**: PO Lead, Agent R (review-eng)

**Akzeptanzkriterien**:
- [ ] Alle P0-Tasks sind erledigt.
- [ ] `go test ./...` ist gruen.
- [ ] `git diff --check` ist gruen.
- [ ] Keine alten Legal-Platzhalter bleiben in den ausgelieferten Templates.
- [ ] Keine laengeren E-Mail-Body-Texte bleiben als Go-String-Bloecke im Service.
- [ ] Rechtstext-Pruefpunkte sind fuer den Kunden sichtbar.
- [ ] Rest-Risiken sind dokumentiert.
- [ ] Deployment-Hinweise fuer Railway Env enthalten mindestens `APP_URL`, `SMTP_HOST`, `SMTP_PORT`, `SMTP_USER`, `SMTP_PASSWORD`, `SMTP_FROM`, `SMTP_TO`.

**Kontext**:
- Gesamter Iterationsdiff.

**Output**:
- Merge-/Release-Readiness-Status.

---
