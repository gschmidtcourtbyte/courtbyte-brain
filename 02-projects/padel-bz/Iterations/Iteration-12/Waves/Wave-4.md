# Wave 4: Hansefit-Admin und Bestaetigungs-E-Mail

**Status:** Done (Legacy-Migration)
**Skill:** product-own
**Quelle:** `plan/iterations/iteration-12.md`

## Legacy-Inhalt

## Wave 4 - Hansefit-Admin und Bestaetigungs-E-Mail
**Agenten**: Agent B, Agent C, Agent D
**Kann einzeln ausgefuehrt werden**: ja, nach Wave 1 und Wave 3
**Ziel**: Admins koennen Hansefit-Anfragen sehen und bearbeiten.

### Task: 12.7 Hansefit-Admin-Uebersicht
**Priority**: P0
**Blocked by**: 12.2, 12.5
**Agent**: Agent D + Agent B

**Akzeptanzkriterien**:
- [x] `GET /admin` zeigt Dashboard mit Link zu Hansefit-Anfragen.
- [x] `GET /admin/hansefit` zeigt nur Anfragen mit `hansefit_registration = true`.
- [x] Tabelle ist unterteilt in: Vorname, Nachname, Playtomic-E-Mail, Inhalt vom Freitextfeld.
- [x] Status wird visuell angezeigt: `unbearbeitet` rot, `bearbeitet` gruen.
- [x] Sortierung: unbearbeitet zuerst, dann neueste Anfragen.
- [x] Leerer Zustand ist klar sichtbar.
- [x] Nicht-Hansefit-Kontaktanfragen erscheinen nicht in dieser Uebersicht.

**Kontext**:
- Templates unter `web/templates/admin/`
- Admin-Styles koennen in `web/static/css/style.css` oder separater Admin-CSS liegen.

**Output**:
- `internal/handler/admin_hansefit.go`
- Admin-Dashboard- und Hansefit-Templates

### Task: 12.8 Statuswechsel und Bestaetigungs-E-Mail
**Priority**: P0
**Blocked by**: 12.7
**Agent**: Agent C + Agent B

**Akzeptanzkriterien**:
- [x] Admin kann Status von `unbearbeitet` auf `bearbeitet` setzen.
- [x] Beim erfolgreichen Statuswechsel wird eine Hansefit-Bestaetigung an den Anfragenden gesendet.
- [x] Empfaenger ist Playtomic-E-Mail, falls vorhanden; sonst Kontakt-E-Mail.
- [x] Bestaetigungs-E-Mail enthaelt Vorname, Nachname und Hinweis auf Hansefit-Bestaetigung bei courts padelclub.
- [x] Bei E-Mail-Fehler bleibt die Anfrage sichtbar bearbeitbar oder zeigt klaren Fehler.
- [x] Bereits bearbeitete Anfrage sendet nicht versehentlich erneut.
- [x] Statuswechsel ist CSRF-geschuetzt.
- [x] Aktion ist per POST umgesetzt, nicht per GET.

**Kontext**:
- Bestehender SMTP-Versand in `internal/service/contact_service.go`.
- Bestehende Betreiber-Benachrichtigung darf nicht entfernt werden.

**Output**:
- Hansefit-Service-Methoden
- Status-Handler
- Bestaetigungs-E-Mail-Template oder plain-text Builder

**Wave-Exit-Gate**:
- [x] Admin sieht Hansefit-Anfragen.
- [x] Status rot/gruen funktioniert.
- [x] Bestaetigungs-E-Mail wird genau beim Bearbeiten versendet.

---
