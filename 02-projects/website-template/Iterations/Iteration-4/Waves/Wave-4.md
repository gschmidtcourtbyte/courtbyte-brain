# Wave 4 — Public POST-Handler + Frontend-Formulare

**Skill:** `/handler-eng` + `/frontend-eng`
**Status:** Planned
**Blocked by:** Wave 3

## Ziel

Die öffentlichen Formulare werden funktional: Kontakt und Karriere posten
same-origin an Next.js API-Proxies, die serverseitig an das Go-Backend
weiterleiten.

## Tasks

### 1. Backend Handler

In `backend/internal/handler/`:

- `POST /api/contact`
- `POST /api/applications`

Anforderungen:

- JSON-only.
- `http.MaxBytesReader` für Body-Limit.
- Klare Error-Codes:
  - `201` created bei Erfolg
  - `400` validation
  - `403` origin
  - `429` rate limit
  - `503` service unavailable
- Response enthält neue ID und Status, aber keine unnötigen PII-Felder.

### 2. Main Router verdrahten

- `handler.NotImplemented` für `/api/contact` und `/api/applications`
  ersetzen.
- Shared Dependencies aus `main.go` sauber injizieren.
- Keine direkten SQL-Zugriffe im Handler.

### 3. Next.js API-Proxies

Neue Routes:

- `frontend/app/api/contact/route.ts`
- `frontend/app/api/applications/route.ts`

Anforderungen:

- Same-Origin Request vom Formular.
- Serverseitig `BACKEND_API_URL` nutzen.
- Status und JSON vom Backend sauber durchreichen.
- Keine Tokens/Secrets im Client.

### 4. Kontaktformular

`frontend/app/(marketing)/kontakt/page.tsx`:

- Form-State aus DOM oder React-State serialisieren.
- Submit an `/api/contact`.
- Loading-State, Success-State, Error-State.
- 429-spezifische Message: später erneut versuchen.

### 5. Karriereformular

`frontend/app/(marketing)/karriere/page.tsx`:

- Position aus ausgewähltem Job mitsenden.
- First/last/email/phone/message persistieren.
- Upload-Zone bleibt visual-only; kein File-POST.
- Loading-, Success-, Error-State.

## Akzeptanzkriterien

- [ ] Kontaktformular erzeugt Persistenzdatensatz über `/api/contact`.
- [ ] Karriereformular erzeugt Persistenzdatensatz über `/api/applications`.
- [ ] Frontend zeigt nutzbare Loading-, Success- und Error-Zustände.
- [ ] Backend validiert und persistiert ohne Handler-SQL.
- [ ] Next API-Proxies geben Backend-Status korrekt weiter.
- [ ] `npm run lint`, `npm run typecheck`, `npm run build`, `go test ./...`
      clean.

## Out of Scope

- Datei-Upload.
- Admin-UI-Bindung an echte Listen.
- Mail-Benachrichtigungen.
