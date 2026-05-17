# Wave 4 — Staging-Smokes gegen Railway

**Skill:** `/testing-eng`
**Status:** Planned
**Blocked by:** Waves 1–3

## Ziel

Das Staging-Smoke-Playbook aus Iteration 5 / Wave 6 endlich gegen die
laufende Railway-Umgebung ausführen und die Ergebnisse hier
dokumentieren. Damit ist die offene Akzeptanz aus Iteration 5
nachgereicht.

## Vorbereitungen

- Wave 1 + 2 + 3 abgeschlossen, d. h. R2 angebunden, SMTP konfiguriert,
  Backend deploys clean.
- `BACKEND_URL` = `https://<backend>.up.railway.app`
- `FRONTEND_URL` = `https://<frontend>.up.railway.app`
- Ein gültiges Test-PDF (z. B. `smoke.pdf` mit echtem `%PDF-` Header).
- Admin-Login-Daten zur Hand.

## Smoke-Sequenz

### 1. Healthcheck

```bash
curl -s -o /dev/null -w "%{http_code}\n" $BACKEND_URL/healthz
```
Erwartung: `200`.

### 2. Public Jobs Endpoint

```bash
curl -s $BACKEND_URL/api/jobs | jq .
```
Erwartung: `{"data": []}` oder befüllte Liste.

### 3. Admin-Login

```bash
curl -s -X POST $BACKEND_URL/api/admin/login \
  -H 'Content-Type: application/json' \
  -d '{"email":"<admin>","password":"<pw>"}' | jq .
```
Erwartung: Session-Objekt mit Token.

Token in `ADMIN_TOKEN` speichern.

### 4. Stelle anlegen

```bash
curl -s -X POST $BACKEND_URL/api/admin/jobs \
  -H "Authorization: Bearer $ADMIN_TOKEN" \
  -H 'Content-Type: application/json' \
  -d '{"title":"Smoke Test","department":"QA","employmentType":"Vollzeit","location":"Remote","description":"Smoke-Test-Stelle","isActive":true}' | jq .
```
Erwartung: `201`, Job-Objekt mit `id`.

### 5. Job öffentlich sichtbar

```bash
curl -s $BACKEND_URL/api/jobs | jq .
```
Erwartung: Smoke-Test-Job in Liste.

Im Browser `$FRONTEND_URL/karriere` öffnen → Smoke-Test-Job sichtbar.

### 6. Bewerbung absenden (mit Anhang)

Browser: `$FRONTEND_URL/karriere` → „Smoke Test" aufklappen → „Jetzt
bewerben" → Felder + `smoke.pdf` anhängen → Absenden.

Erwartung: „Bewerbung eingegangen!" wird angezeigt. Keine Konsolen-Fehler.

### 7. Bewerbung ohne Anhang absenden

Wieder auf `/karriere`, andere Bewerbung absenden ohne Datei.

Erwartung: erfolgreich (Regressionstest für Iter-5-Bugfix).

### 8. Admin-UI: Bewerbung sehen + Anhang downloaden

`$FRONTEND_URL/admin/karriere` → Smoke-Test-Bewerbung anklicken → Anhang
download → PDF kommt korrekt zurück.

Erwartung: Datei öffnet sich als PDF.

### 9. Statuswechsel auf Abgelehnt → Mail

In Admin-UI Status auf „Abgelehnt" setzen.

Erwartung:
- Status persistiert (`rejected`).
- Mailtrap-Inbox enthält eine neue Rejection-Mail mit Bewerbername +
  Position.
- DB-Feld `delete_after` ist `rejected_at + 90 days`.

### 10. Auth-Guard für Attachment-Download

```bash
curl -s -o /dev/null -w "%{http_code}\n" \
  $BACKEND_URL/api/admin/applications/<id>/attachments/<aid>/download
```
Erwartung: `401`.

### 11. Manueller Retention-Trigger

In der DB für den Smoke-Test-Datensatz `delete_after` auf einen vergangenen
Zeitpunkt setzen (Railway Postgres-Query-Tab):

```sql
UPDATE career_applications SET delete_after = now() - interval '1 day'
WHERE first_name = 'SmokeTest';
```

Dann:

```bash
curl -s -X POST $BACKEND_URL/api/admin/applications/retention/run \
  -H "Authorization: Bearer $ADMIN_TOKEN" | jq .
```

Erwartung: `{"data":{"deleted": >= 1}}`. Datensatz und Anhang im R2-Bucket
sind weg. Re-Run liefert `{"deleted": 0}`.

### 12. Aufräumen

Smoke-Test-Stelle in der Admin-UI auf inaktiv setzen oder löschen.

## Akzeptanzkriterien

- [ ] Alle 11 Smoke-Schritte erfolgreich, Resultate in
      `Iteration-6.md` Verification-Block dokumentiert.
- [ ] Keine `forbidden_origin` oder `service_unavailable` Fehler in den
      Backend-Logs während der Smokes.
- [ ] R2-Bucket enthält nach erfolgreichem Upload genau die hochgeladenen
      Objekte, nach Retention-Trigger keine mehr.

## Risiken / Stolpersteine

- Rate-Limit: Default ist 3 Bewerbungen pro 30 Min pro IP. Wenn die Smokes
  mehrfach ausgeführt werden, kann das greifen → 429. Gegenmaßnahme: andere
  IP nutzen oder im Backend kurzfristig `applicationRateLimit` per Code
  hochsetzen (nicht persistent machen).
- Mailtrap-Inbox-Limit: bei vielen Re-Runs Inbox manuell leeren.

## Notizen für Codex

- Falls Smokes fehlschlagen, **nicht** automatisch im Code patchen.
  Stattdessen Befund in `Iteration-6.md` notieren und mit dem Menschen
  klären — Smokes sind hier Verifikations-, keine Implementierungs-Wave.
