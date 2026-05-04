# Wave 5: Prelaunch Deployment und Betriebscheck

**Status:** Planned
**Skill:** infrastructure-eng
**Rolle:** infrastructure-eng
**Kann parallel ausgefuehrt werden:** nein
**Blocked by:** Wave 4

## Ziel

Der Prelaunch-Zustand ist deploybar und fuer den Betrieb klar dokumentiert. Der relevante Schalter ist `MAINTENANCE_MODE=true`; erlaubte oeffentliche Pfade und Preview-Verhalten sind nachvollziehbar.

## Tasks

### Task 22.13: Deployment-Konfiguration fuer Prelaunch pruefen
**Priority:** P1
**Blocked by:** Wave 4
**Agent:** Agent E (infrastructure-eng)

**Akzeptanzkriterien:**
- [ ] `.env.example` dokumentiert `MAINTENANCE_MODE`.
- [ ] Erwarteter Produktionswert fuer Prelaunch ist `MAINTENANCE_MODE=true`.
- [ ] Keine Secrets werden in Git geschrieben.
- [ ] Bestehende Railway-Deployment-Konventionen bleiben unveraendert.

**Kontext:**
- `internal/config/config.go`
- `.env.example`
- Railway Env Variables werden extern gesetzt.

**Output:**
- Aktualisierte Beispiel-/Betriebsdoku falls noetig.

### Task 22.14: Prelaunch Smoke-Check definieren
**Priority:** P1
**Blocked by:** 22.13
**Agent:** Agent E (infrastructure-eng)

**Akzeptanzkriterien:**
- [ ] Smoke-Check umfasst `/`, `/kontakt`, `/impressum`, `/datenschutz`, `/agb`.
- [ ] Smoke-Check umfasst mindestens einen gesperrten Pfad, z. B. `/baufortschritt` oder direkten Home-Content.
- [ ] Erwartete Statuscodes/Verhalten sind dokumentiert.
- [ ] Admin-Erreichbarkeit wird getrennt geprueft.

**Kontext:**
- Deployment auf Railway.
- Prelaunch soll oeffentlich nutzbar sein, aber Content noch nicht freigeben.

**Output:**
- Kurze Betriebsnotiz im passenden Repo-Dokument oder Wave-Abschlussnotiz.

## Wave-5-Exit-Gate

- [ ] Prelaunch-Konfiguration ist dokumentiert.
- [ ] Smoke-Check ist definiert.
- [ ] Keine Deployment-Secrets wurden veraendert oder committed.

## Notes

- Diese Wave deployt nicht automatisch. Deployment erfolgt nur, wenn der ausfuehrende Agent bzw. User das ausdruecklich freigibt.

## Abschluss

- [ ] Wave abgeschlossen
