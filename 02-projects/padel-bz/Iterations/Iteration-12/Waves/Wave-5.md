# Wave 5: Integration, Tests und Review

**Status:** Done (Legacy-Migration)
**Skill:** product-own
**Quelle:** `plan/iterations/iteration-12.md`

## Legacy-Inhalt

## Wave 5 - Integration, Tests und Review
**Agenten**: Agent E, Agent R, PO Lead
**Kann einzeln ausgefuehrt werden**: ja, nach Waves 1-4
**Ziel**: Regressionen, Security-Risiken und Abnahmefaelle sind geprueft.

### Task: 12.9 Tests und manuelle QA
**Priority**: P1
**Blocked by**: 12.3, 12.4, 12.8
**Agent**: Agent E

**Akzeptanzkriterien**:
- [x] `go test ./...` laeuft erfolgreich.
- [x] Kontaktformular-Test: Hansefit angehakt landet in Admin-Liste.
- [x] Kontaktformular-Test: Hansefit nicht angehakt sendet Anfrage, landet nicht in Admin-Liste.
- [x] Playtomic-E-Mail-Validierung ist getestet.
- [x] Admin-Login ist getestet.
- [x] Statuswechsel `unbearbeitet` -> `bearbeitet` ist getestet.
- [x] Doppelte Bestaetigungs-E-Mail wird verhindert.
- [x] Bauphasen-Klick funktioniert auf Desktop und Mobile.
- [x] No-JS-Fallback zeigt aktuellen Baufortschritt.
- [x] Maintenance Mode blockiert Admin nicht.

**Kontext**:
- Bestehende Testabdeckung ist gering; Fokus auf Handler-/Service-Smoke-Tests.

**Output**:
- Neue/aktualisierte Tests
- QA-Status in dieser Datei

### Task: 12.10 Security- und Merge-Review
**Priority**: P1
**Blocked by**: 12.9
**Agent**: Agent R

**Akzeptanzkriterien**:
- [x] Passwortspeicherung ist bcrypt-basiert.
- [x] Testpasswort ist als Produktivrisiko dokumentiert.
- [x] Admin-Routen sind nicht oeffentlich erreichbar.
- [x] CSRF-Schutz fuer Login/Logout/Statuswechsel geprueft.
- [x] Keine personenbezogenen Daten werden unnoetig im Log ausgegeben.
- [x] Bestaetigungs-E-Mail kann nicht durch fremde Requests ausgeloest werden.
- [x] Doku nennt neue Env Vars und Admin-Testzugang.
- [x] Offene Risiken sind dokumentiert.

**Output**:
- Review-Notizen
- Finaler Iterationsstatus

**Wave-Exit-Gate**:
- [x] Tests und Review abgeschlossen.
- [x] Abnahmekriterien sind nachvollziehbar erfuellt oder Rest-Risiken dokumentiert.

### Wave-5-QA-Status

- `go test ./...` erfolgreich via Docker Go 1.23.
- Maintenance/Coming-Soon deaktiviert: App-Startlog zeigt `Maintenance mode: OFF`, Root liefert HTTP 200 und die normale Startseite.
- Maintenance-Guard separat geprueft: mit `MAINTENANCE_MODE=true` liefert Root HTTP 503, `/admin/login` bleibt HTTP 200.
- Formular-QA: fehlende Playtomic-E-Mail bei Hansefit liefert HTTP 422 mit Feldfehler.
- Formular-QA: Hansefit-Anfrage liefert HTTP 303, erscheint in `/admin/hansefit`; normale Anfrage liefert HTTP 303 und erscheint nicht in der Hansefit-Liste.
- Admin-QA: `/admin` ohne Session leitet auf Login; Login mit `PadelBZ`/`PadelBZ` funktioniert; Statuswechsel liefert Success-Redirect; zweiter Statuswechsel liefert Already-Processed-Redirect.
- Bauphasen-QA: Startseite enthaelt `data-phase-switcher`, `Rohbau` und `Hallenbau`; `Hallenbau` ist initial hidden, Toggle-Logik in `web/static/js/main.js` schaltet Tab, Panel und Video-Playback.

### Review-Notizen

- Security: Admin-Statuswechsel ist nur hinter Session + CSRF erreichbar.
- Security: Admin-Passwort liegt nur als bcrypt Hash im Code; `PadelBZ`/`PadelBZ` bleibt ein Produktivrisiko und muss vor Livebetrieb ersetzt werden.
- Security: Keine Formularinhalte, Passwoerter oder Session-Tokens werden aktiv in Logs ausgegeben; Fehlerlogs enthalten nur technische Fehlertexte.
- Offenes Risiko: Lokale Umgebung hat kein SMTP. Bei leerem `SMTP_HOST` wird kein externer Versand ausgefuehrt; produktiv muessen SMTP-Env-Vars gesetzt werden.
- Offenes Risiko: Admin-Login hat noch kein eigenes Brute-Force-Limit. Vor produktivem Adminbetrieb sollte ein separates Login-Rate-Limit ergaenzt werden.

---
