# Wave 3: Admin-Auth und Test-Admin

**Status:** Done (Legacy-Migration)
**Skill:** product-own
**Quelle:** `plan/iterations/iteration-12.md`

## Legacy-Inhalt

## Wave 3 - Admin-Auth und Test-Admin
**Agenten**: Agent B, Agent A
**Kann einzeln ausgefuehrt werden**: ja
**Ziel**: Ein geschuetzter Adminbereich ist erreichbar und der Testnutzer existiert.

### Task: 12.5 Admin-User und Auth-MVP
**Priority**: P0
**Blocked by**: none
**Agent**: Agent B

**Akzeptanzkriterien**:
- [x] Admin-Login unter `GET /admin/login`.
- [x] Login-Submit unter `POST /admin/login`.
- [x] Logout unter `POST /admin/logout`.
- [x] Geschuetzte Admin-Routen leiten ohne Session auf Login um.
- [x] CSRF bleibt fuer Admin-Formulare aktiv.
- [x] Session-Cookie ist `HttpOnly`, `SameSite=Lax`, in Production `Secure`.
- [x] Passwortpruefung nutzt bcrypt.
- [x] Fehlertext beim Login verrat nicht, ob User oder Passwort falsch war.
- [x] Maintenance Mode laesst `/admin/*` durch.

**Kontext**:
- `github.com/gorilla/sessions` ist bereits im `go.mod`.
- `golang.org/x/crypto/bcrypt` muss ggf. ergaenzt werden.
- Maintenance-Middleware: `internal/handler/maintenance.go`.

**Output**:
- `internal/handler/admin_auth.go`
- Admin-Login-Template unter `web/templates/admin/`
- Config-/Router-Erweiterung

### Task: 12.6 Test-Admin `PadelBZ` anlegen
**Priority**: P0
**Blocked by**: 12.5
**Agent**: Agent A

**Akzeptanzkriterien**:
- [x] Test-Admin Username: `PadelBZ`.
- [x] Test-Passwort: `PadelBZ`.
- [x] Passwort wird nicht im Klartext gespeichert.
- [x] Seed/Migration oder Config-Mechanismus ist dokumentiert.
- [x] Plan/Doku markiert den Nutzer als Testzugang, der vor Produktion ersetzt werden muss.

**Kontext**:
- Wenn DB-User genutzt werden: separate Tabelle `admin_users`.
- Wenn config-basierter Admin genutzt wird: Env Vars mit bcrypt Hash und lokalem Default nur fuer Development.

**Output**:
- Admin-User-Setup
- Dokumentierter Testzugang

**Wave-Exit-Gate**:
- [x] `/admin/login` ist erreichbar.
- [x] Login mit `PadelBZ`/`PadelBZ` funktioniert lokal.
- [x] Admin-Routen sind ohne Login geschuetzt.

---
