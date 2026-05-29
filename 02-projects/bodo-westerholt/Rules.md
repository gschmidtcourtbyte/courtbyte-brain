# bodo-westerholt - Coding Rules

## Architektur

- Monorepo mit `frontend/` (Next.js) und `backend/` (Go) als gleichwertige Top-Level-Ordner.
- Frontend: Next.js App Router, TypeScript, React 18; keine Pages-Router-Routen.
- Backend: Go Standard-Layout mit `cmd/` und `internal/`.
- Repository Pattern im Backend fuer alle DB-Zugriffe; kein direktes SQL in Handlern.
- Kein ORM; Datenbankzugriffe laufen ueber pgx und Raw SQL.

## Frontend-Konventionen

- TypeScript strict.
- Styling ueber lokale Theme-/Konfigurationsdateien, keine Tailwind-, shadcn-, MUI- oder Radix-Einfuehrung ohne explizite Entscheidung.
- Komponenten unter `frontend/components/`, eine Komponente pro Datei, PascalCase-Filename.
- Kundeninhalte und Branding bevorzugt ueber `frontend/lib/site-config.ts`, `frontend/lib/theme.ts`, `frontend/lib/data.ts` und `frontend/public/`.
- Deutsche Slugs und de-DE als Default-Sprache.

## Backend-Konventionen

- Structured Logging mit slog.
- Graceful Shutdown beibehalten.
- CSRF-Schutz auf state-changing Browser-Routen.
- Rate Limiting auf oeffentlichen Formular-Endpunkten.
- Admin-Auth ueber HttpOnly-Cookie/Sessions; keine Auth-Tokens im LocalStorage.

## Anpassbarkeit fuer diesen Kunden

Goldene Regel: Nichts Kunden-Spezifisches hart in Komponenten verdrahten. Texte,
Farben, Logos, Bilder und Listen laufen zuerst ueber Konfiguration, Content und
Assets. Wenn Kundenwuensche Komponenten-Aenderungen erfordern, erst pruefen, ob
eine generische Konfigurationsluecke geschlossen werden sollte.

## Was nicht ins Code-Repo gehoert

- Keine Planungs-Markdowns; Planung liegt in `{vault}/02-projects/bodo-westerholt/`.
- Keine `.env`-Dateien, nur `.env.example`.
- Keine produktiven Secrets.
- Keine finalen Kunden-Assets ohne Freigabe und klare Nutzungsrechte.
