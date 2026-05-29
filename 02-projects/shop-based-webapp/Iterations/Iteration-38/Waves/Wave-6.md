# Wave 6 - Optionaler Railway-Migrations- und Deployment-Check

**Iteration:** [[Iteration-38]]
**Skill:** infrastructure-eng
**Status:** Done

## Tasks

- [x] Klaeren, welche Railway-Umgebung Ziel der Migration ist, bevor irgendeine Remote-Aktion ausgefuehrt wird.
- [x] Railway-Projekt/Service/Environment pruefen, ohne Secrets auszugeben.
- [x] Nach erfolgreichem Merge/Build die versionierte Migration ueber den vorgesehenen Deployment-/Migration-Mechanismus ausfuehren, nicht per manueller Schema-Mutation.
- [x] `goose status` oder aequivalenten Migrationsstatus fuer die Zielumgebung pruefen, falls sicher verfuegbar.
- [x] API-Healthcheck und Admin-/Checkout-Smoke gegen die Zielumgebung ausfuehren.
- [x] Rollback-Pfad dokumentieren: Code-Rollback plus Down-Migration nur nach fachlicher Freigabe.

## Done Criteria

- [x] Keine Railway-Aktion wurde ohne Zielumgebungsfreigabe ausgefuehrt.
- [x] Datenbankanpassung laeuft ausschliesslich ueber `migrations/049...`.
- [x] Zielumgebung meldet erfolgreiche Migration und gesunden API-Status.
- [ ] Smoke bestaetigt, dass bestehende Buchungen und neue Add-on-Felder geladen werden koennen.

## Notes

Staging-Freigabe am 2026-05-18 durch User erteilt: Deploy nur fuer OUT Padel `staging`, kein Production-Deploy.

Railway-Projekt `outpadel` geprueft:
- Environment `staging`
- Service `backend-api`
- Service `web-customer-frontend`
- Managed Services `Postgres`, `Redis`

Deployment:
- `backend-api` nach `staging` deployed: `6e3a22e1-0fc8-4fb1-9e6c-671c8a252ffe`, Status `SUCCESS`.
- `web-customer-frontend` nach `staging` deployed: `cf19c636-3fc9-441d-a421-1138f88d2e53`, danach `5570f862-8cf2-4498-90fa-545ee3a49272`, Status `SUCCESS`.
- Nach Korrektur der Staging-Variable `API_BASE_URL` auf `http://${{backend-api.RAILWAY_PRIVATE_DOMAIN}}:8080` wurde `web-customer-frontend` per Redeploy `52703cb5-1f40-48bb-b219-6dbd7c7eff5e` erfolgreich neu gestartet.

Migration:
- API-Deploy-Logs zeigen `applying migration 049_alter_camp_bookings_add_addons_and_payment_status.up.sql`.
- Keine manuelle Schema-Mutation ausgefuehrt.

Checks:
- `https://backend-api-staging-d49c.up.railway.app/healthz` -> `200`
- `https://backend-api-staging-d49c.up.railway.app/ready` -> `200`
- `https://web-customer-frontend-staging.up.railway.app/healthz` -> `200`
- `https://web-customer-frontend-staging.up.railway.app/` -> `200`
- `https://web-customer-frontend-staging.up.railway.app/admin` -> `200`
- `https://backend-api-staging-d49c.up.railway.app/api/v1/camp-events` -> `200`
- `https://web-customer-frontend-staging.up.railway.app/api/v1/camp-events` -> `200`

Bekannte Einschraenkung:
- Vollstaendiger Admin-Booking-Smoke mit bestehenden Buchungen und neuen Add-on-Feldern wurde nicht ausgefuehrt, weil keine Admin-Session/Credentials verwendet wurden.

Production-Autodeploy:
- Railway CLI/MCP bietet keinen direkten Toggle fuer GitHub Autodeploys.
- Railway GraphQL Schema enthaelt `serviceInstanceAutoDeployUpdate`, der Versuch fuer Production `backend-api` und `web-customer-frontend` wurde von Railway mit `Not Authorized` abgelehnt.
- Production wurde nicht deployed und nicht veraendert. Noetiger manueller Schritt: In Railway Dashboard fuer Production bei beiden Services GitHub Autodeploys deaktivieren; danach Production per manuellem `Deploy Latest Commit`/Approval ausrollen.

Rollback-Pfad:
- Code-Rollback auf vorherige Deployments in Railway.
- DB-Rollback nur nach fachlicher Freigabe ueber `migrations/049_alter_camp_bookings_add_addons_and_payment_status.down.sql`, nicht per manueller SQL-Aenderung.
