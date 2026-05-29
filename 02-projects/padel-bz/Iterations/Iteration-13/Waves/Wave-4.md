# Wave 4: E-Mail-Templates und Content-Dokumentation

**Status:** Done (Legacy-Migration)
**Skill:** product-own
**Quelle:** `plan/iterations/iteration-13.md`

## Legacy-Inhalt

## Wave 4 - E-Mail-Templates und Content-Dokumentation
**Agenten**: Agent C, Agent Doc
**Kann einzeln ausgefuehrt werden**: ja, nach Wave 1 fuer Hansefit-ID im Template-Kontext
**Ziel**: E-Mail-Texte fuer Registrierende sind als Template-Dateien pflegbar und in `docs/Content.md` auffindbar.

### Task: 13.8 Hansefit-Bestaetigungs-Mail als Template auslagern
**Priority**: P1
**Blocked by**: 13.2
**Agent**: Agent C

**Akzeptanzkriterien**:
- [ ] Hansefit-Bestaetigungs-Mail wird aus einer Template-Datei gerendert.
- [ ] Template liegt unter `web/templates/emails/`.
- [ ] Template kann Vorname, Nachname, Playtomic-E-Mail und Hansefit-ID nutzen.
- [ ] Bestehende Versandlogik und Empfaengerlogik bleiben erhalten.
- [ ] Bei Template-Parse- oder Renderfehler wird die Anfrage nicht faelschlich als bearbeitet markiert.
- [ ] Lokaler Betrieb ohne SMTP bleibt wie in Iteration 12 dokumentiert.

**Kontext**:
- `internal/service/contact_service.go`
- `docs/Operations.md`
- Neuer Zielpfad: `web/templates/emails/hansefit_confirmation.txt`

**Output**:
- Neues E-Mail-Template
- Template-Rendering im Contact-Service
- Regressionstest fuer Template-Daten, soweit ohne SMTP sinnvoll

### Task: 13.9 Content.md um E-Mail-Templates erweitern
**Priority**: P1
**Blocked by**: 13.8
**Agent**: Agent Doc

**Akzeptanzkriterien**:
- [ ] `docs/Content.md` enthaelt einen Abschnitt `E-Mail-Templates`.
- [ ] Hansefit-Bestaetigungs-Mail ist mit Dateipfad verlinkt.
- [ ] Bearbeitbare Textstellen im Template sind in der Tabelle auffindbar.
- [ ] Hinweise zu Variablen wie Vorname, Nachname, Playtomic-E-Mail und Hansefit-ID sind dokumentiert.
- [ ] Falls Betreiber-Benachrichtigung ebenfalls ausgelagert wird, ist sie ebenfalls verlinkt.

**Kontext**:
- `docs/Content.md`
- `web/templates/emails/hansefit_confirmation.txt`

**Output**:
- Aktualisierte Content-Dokumentation

**Wave-Exit-Gate**:
- [ ] Bestaetigungs-Mail ist template-basiert.
- [ ] Content-Verantwortliche finden die Mailtexte in `docs/Content.md`.

---
