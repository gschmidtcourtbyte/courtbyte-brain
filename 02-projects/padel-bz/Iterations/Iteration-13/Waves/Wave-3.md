# Wave 3: Admin-UI Feinschliff

**Status:** Done (Legacy-Migration)
**Skill:** product-own
**Quelle:** `plan/iterations/iteration-13.md`

## Legacy-Inhalt

## Wave 3 - Admin-UI Feinschliff
**Agenten**: Agent D, Agent B
**Kann einzeln ausgefuehrt werden**: ja, nach Wave 1 fuer Hansefit-ID-Anzeige
**Ziel**: Adminbereich wirkt hochwertiger, zentraler und besser strukturiert.

### Task: 13.6 Login-Maske zentral und hochwertiger gestalten
**Priority**: P1
**Blocked by**: none
**Agent**: Agent D

**Akzeptanzkriterien**:
- [ ] Login-Maske ist horizontal und vertikal sauber zentriert.
- [ ] Login nutzt eine reduzierte, professionelle Admin-Optik passend zur Website.
- [ ] Fehlermeldung ist sichtbar, aber nicht ueberbetont.
- [ ] Form-Felder, Button und Fokus-Stati sind auf Desktop und Mobile sauber.
- [ ] Keine sichtbaren technischen Erklaertexte oder unnoetigen Hinweise.
- [ ] CSRF und bestehende Login-Logik bleiben unveraendert funktionsfaehig.

**Kontext**:
- `web/templates/admin/login.html`
- `web/static/css/style.css`
- `internal/handler/admin_auth.go`

**Output**:
- Ueberarbeitetes Admin-Login
- Responsive Admin-Login-Styles

### Task: 13.7 Admin-Dashboard und Hansefit-Tabelle verbessern
**Priority**: P0
**Blocked by**: 13.2
**Agent**: Agent D + Agent B

**Akzeptanzkriterien**:
- [ ] Dashboard zeigt klarere Uebersicht statt nur einfachen Link.
- [ ] Dashboard enthaelt mindestens Kennzahlen fuer Hansefit-Anfragen: Gesamt, unbearbeitet, bearbeitet.
- [ ] Dashboard bietet direkten Einstieg in die Hansefit-Uebersicht.
- [ ] Hansefit-Uebersicht zeigt Inhalte in einer hochwertigen Tabelle.
- [ ] Tabelle enthaelt Vorname, Nachname, Playtomic-E-Mail, Hansefit-ID, Freitext, Status, Aktion.
- [ ] Tabelle bleibt auf Mobile nutzbar, z.B. durch horizontales Scrollen oder responsive Zeilen.
- [ ] Unbearbeitet/bearbeitet bleiben rot/gruen unterscheidbar, aber visuell ruhiger als harte Warnfarben.
- [ ] Leerer Zustand ist gepflegt und nicht technisch.

**Kontext**:
- `web/templates/admin/dashboard.html`
- `web/templates/admin/hansefit.html`
- `web/static/css/style.css`
- `internal/handler/admin_auth.go`
- `internal/handler/admin_hansefit.go`
- `internal/repository/contact_repo.go`

**Output**:
- Ueberarbeitetes Admin-Dashboard
- Ueberarbeitete Hansefit-Tabelle
- Ggf. Repository-/Service-Methode fuer Dashboard-Kennzahlen

**Wave-Exit-Gate**:
- [ ] Login und Dashboard wirken konsistent.
- [ ] Hansefit-ID ist im Admin sichtbar.
- [ ] Admin-Flows bleiben auth- und CSRF-geschuetzt.

---
