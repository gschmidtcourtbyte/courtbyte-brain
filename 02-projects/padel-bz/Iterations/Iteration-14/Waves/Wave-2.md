# Wave 2: Backend-Validierung und HTTP-Rand

**Status:** Done (Legacy-Migration)
**Skill:** product-own
**Quelle:** `plan/iterations/iteration-14.md`

## Legacy-Inhalt

## Wave 2 - Backend-Validierung und HTTP-Rand
**Agenten**: Agent B, Agent C, Agent E
**Kann einzeln ausgefuehrt werden**: ja, nach Wave 1
**Ziel**: Manipulierte oder unvollstaendige Requests werden serverseitig korrekt abgewiesen.

### Task: 14.4 Server-seitige Pflichtfeldmatrix absichern
**Priority**: P0
**Blocked by**: 14.2, 14.3
**Agent**: Agent B

**Akzeptanzkriterien**:
- [ ] Fehlender Nachname erzeugt Validierungsfehler.
- [ ] Fehlender Vorname erzeugt Validierungsfehler.
- [ ] Fehlende oder ungueltige E-Mail-Adresse erzeugt Validierungsfehler.
- [ ] Fehlende Datenschutzerklaerung erzeugt Validierungsfehler.
- [ ] Fehlende AGB-Zustimmung erzeugt Validierungsfehler.
- [ ] Ohne Hansefit-Checkbox sind Hansefit-ID und Playtomic-E-Mail nicht erforderlich.
- [ ] Mit Hansefit-Checkbox sind Hansefit-ID und Playtomic-E-Mail erforderlich.
- [ ] Mit Hansefit-Checkbox wird eine ungueltige Playtomic-E-Mail abgewiesen.

**Kontext**:
- `internal/model/contact.go`
- `internal/model/contact_test.go`
- `internal/service/contact_service.go`

**Output**:
- Erweiterte Domain-Tests fuer die komplette Pflichtfeldmatrix.

### Task: 14.5 Handler-Parsing und Consent-Mapping aktualisieren
**Priority**: P0
**Blocked by**: 14.3
**Agent**: Agent C

**Akzeptanzkriterien**:
- [ ] Handler liest `terms`/`terms_accepted` aus dem Formular.
- [ ] Handler erhaelt Formularwerte nach Validierungsfehlern.
- [ ] Handler setzt Hansefit-Felder nur, wenn Hansefit aktiv ist, oder Service verwirft sie deterministisch.
- [ ] CSRF-Token bleibt unveraendert erforderlich.
- [ ] Rate-Limit-Verhalten bleibt unveraendert.
- [ ] Fehler werden weiterhin im vorhandenen Fehlerbereich ausgegeben.

**Kontext**:
- `internal/handler/contact.go`
- `web/templates/pages/contact.html`

**Output**:
- Aktualisierter Contact-Handler.
- Handler-Test oder manuelle Testmatrix, falls keine Handler-Teststruktur existiert.

### Task: 14.6 AGB-Seite oder AGB-Link klaeren
**Priority**: P1
**Blocked by**: 14.1
**Agent**: Agent C + Agent Doc

**Akzeptanzkriterien**:
- [ ] Kontaktformular verlinkt auf eine erreichbare AGB-URL.
- [ ] Footer-AGB-Link zeigt auf dieselbe URL.
- [ ] Wenn finaler AGB-Text vorhanden ist, existieren Template und Route.
- [ ] Wenn finaler AGB-Text fehlt, ist der Release-Blocker dokumentiert.

**Kontext**:
- `cmd/server/main.go`
- `internal/handler/public.go`
- `web/templates/partials/footer.html`
- `docs/Content.md`

**Output**:
- Erreichbare AGB-Route oder klar dokumentierter Content-Blocker.

**Wave-Exit-Gate**:
- [ ] Server lehnt alle unvollstaendigen Pflichtfeld-Kombinationen ab.
- [ ] Consent-Felder werden korrekt gemappt.
- [ ] AGB-Link ist technisch geloest oder als Release-Blocker sichtbar.

---
