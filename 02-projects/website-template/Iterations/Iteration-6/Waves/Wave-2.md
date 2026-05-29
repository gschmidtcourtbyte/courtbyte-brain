# Wave 2 — Railway-Env-Variablen für Backend + Frontend

**Skill:** `/infrastructure-eng`
**Status:** Planned
**Blocked by:** Wave 1 (für S3-Credentials)

## Ziel

Alle Env-Variablen so im Railway-Projekt `Website-template` setzen, dass
Backend und Frontend gegen die produktive URL korrekt arbeiten — insbesondere
ohne `forbidden_origin` und ohne `service_unavailable`.

## Vorbedingungen

- Railway-Projekt `Website-template` existiert mit zwei Services: `backend`
  und `frontend` (gemäß Iteration 2 Railway-Setup).
- Railway-Plugins für Postgres und Redis sind im Projekt aktiv.
- R2-Credentials aus Wave 1 liegen bereit.
- Admin-Initialdaten (E-Mail + starkes Passwort, ≥ 12 Zeichen) sind
  festgelegt.

## Tasks

### Backend-Service

Variablen setzen (Railway-Dashboard oder
`railway variables set KEY=VALUE --service backend`):

| Variable | Wert | Quelle |
|---|---|---|
| `PORT` | `8080` | fix |
| `DATABASE_URL` | `${{Postgres.DATABASE_URL}}` | Railway-Reference |
| `REDIS_URL` | `${{Redis.REDIS_URL}}` | Railway-Reference |
| `FRONTEND_ORIGIN` | `https://<frontend>.up.railway.app` (kommasepariert weitere, z. B. Custom-Domain) | Railway-Frontend-Domain |
| `S3_ENDPOINT` | `https://<accountid>.r2.cloudflarestorage.com` | Wave 1 |
| `S3_REGION` | `auto` | R2 |
| `S3_BUCKET` | `website-template-attachments-prod` | Wave 1 |
| `S3_ACCESS_KEY_ID` | aus Wave 1 | Wave 1 |
| `S3_SECRET_ACCESS_KEY` | aus Wave 1 | Wave 1 |
| `S3_USE_PATH_STYLE` | `true` | R2 erfordert das |
| `SMTP_HOST` | aus Wave 3 | Wave 3 |
| `SMTP_PORT` | aus Wave 3 | Wave 3 |
| `SMTP_USERNAME` | aus Wave 3 | Wave 3 |
| `SMTP_PASSWORD` | aus Wave 3 | Wave 3 |
| `MAIL_FROM_ADDRESS` | z. B. `karriere@template.example` | Wave 3 |
| `MAIL_FROM_NAME` | `Mittelstand Website` | fix |
| `APPLICATION_RETENTION_CLEANUP_ENABLED` | `true` | fix |
| `APPLICATION_RETENTION_CLEANUP_INTERVAL` | `24h` | fix |
| `APPLICATION_RETENTION_CLEANUP_LIMIT` | `100` | fix |
| `ADMIN_BOOTSTRAP_ENABLED` | `true` (initial), nach erstem Login `false` | manuelles Toggle |
| `ADMIN_EMAIL` | gewählte Admin-Mail | fix |
| `ADMIN_PASSWORD` | starkes Passwort, ≥ 12 Zeichen | fix, Secret |
| `ADMIN_SESSION_TTL` | `12h` | fix |

### Frontend-Service

| Variable | Wert |
|---|---|
| `BACKEND_API_URL` | interne Service-zu-Service-URL, z. B. `http://${{Backend.RAILWAY_PRIVATE_DOMAIN}}:8080` |
| `NEXT_PUBLIC_API_URL` | öffentliche Backend-URL — derzeit nicht zwingend nötig, da Frontend serverseitig proxied; setzen mit Default `https://<backend>.up.railway.app` für Konsistenz |
| `NEXT_PUBLIC_ENABLE_TWEAKER` | `false` für Production, `true` für Staging |
| `NEXT_PUBLIC_HERO_VARIANT` | `split` |
| `NEXT_PUBLIC_THEME_MODE` | `dark` |
| `NEXT_PUBLIC_PRIMARY_COLOR` | `#2B7A9F` |
| `NEXT_PUBLIC_ACCENT_COLOR` | `#E8834A` |
| `NEXT_PUBLIC_COMPANY_NAME` | `Mittelstand & Friends` |
| `NEXT_PUBLIC_LOGO_INITIALS` | `MF` |

### Nach Setzen aller Variablen

1. Backend-Service redeployen (Railway-Dashboard „Deploy" oder
   `railway up --service backend`).
2. Healthcheck `/healthz` muss `200 OK` liefern.
3. Smoke gegen `/api/jobs`: `curl https://<backend>/api/jobs` → `200` mit
   leerem `data: []` Array, falls noch keine Stellen.
4. Frontend-Service redeployen.
5. Frontend `/karriere` öffnen → keine Konsolen-Fehler, leere Stellen-Liste
   sichtbar.

## Akzeptanzkriterien

- [ ] Alle oben gelisteten Variablen sind im jeweiligen Service gesetzt.
- [ ] Backend-Logs zeigen `admin auth initialized` beim Start (DB + Redis
      Verbindung steht).
- [ ] Backend-Logs zeigen **nicht** `s3 attachment storage disabled` — S3 ist
      angebunden.
- [ ] Backend-Logs zeigen **nicht** `rejection mailer disabled` — SMTP steht
      (nach Wave 3).
- [ ] Frontend `/karriere` lädt ohne Fehler.
- [ ] Admin-Login `/admin/login` funktioniert.

## Risiken / Stolpersteine

- Railway-Service-Referenzen schreiben sich `${{ServiceName.VAR}}` exakt mit
  dem Service-Namen aus dem Dashboard.
- `RAILWAY_PRIVATE_DOMAIN` ist nur intern erreichbar, keine TLS. Für die
  interne Service-zu-Service-Kommunikation reicht `http://`, nicht `https://`.
- Wenn `ADMIN_BOOTSTRAP_ENABLED=true` und beim Redeploy weiterhin
  `ADMIN_PASSWORD` gesetzt ist, wird das Passwort bei jedem Restart
  zurückgesetzt. Nach erstem Login auf `false` zurückdrehen.

## Notizen für Codex

- Falls Railway-CLI verfügbar: `railway link` → `railway service backend`
  → `railway variables set KEY=VALUE` (eine Zeile pro Variable).
- Alternative: über `railway variables set` ein Skript ausführen, das alle
  Variablen aus einer Quelle einliest.
- Secrets niemals committen, niemals loggen, niemals in MEMORY.md ablegen.
