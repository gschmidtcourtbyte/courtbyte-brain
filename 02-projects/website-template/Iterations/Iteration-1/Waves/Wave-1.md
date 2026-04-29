# Wave 1 — Bootstrap Monorepo

**Iteration:** [[Iteration-1]]
**Skill:** infrastructure-eng
**Status:** In Progress

## Tasks

- [ ] Root: `.gitignore`, `README.md`, `.vault`, `CLAUDE.md`, `docker-compose.yml`
- [ ] `frontend/` mit Next.js 14 (App Router, TypeScript, ESLint, no Tailwind, no src/) bootstrappen — `package.json`, `tsconfig.json`, `next.config.mjs`, `app/layout.tsx`, `app/page.tsx`
- [ ] `frontend/app/layout.tsx` mit DM-Sans-Font, globalen CSS-Resets, `<TweaksProvider>`-Slot
- [ ] `backend/` mit Go-Modul (`go mod init github.com/andersen-k8s/website-template/backend`), chi-Router, `cmd/api/main.go`, `/healthz` Endpoint
- [ ] `backend/Makefile` mit `dev` / `build` / `test` / `lint` Targets
- [ ] Beide Apps müssen lokal starten (`npm run dev`, `go run ./cmd/api`)

## Done Criteria

- [ ] `npm install && npm run dev` im `frontend/` läuft fehlerfrei
- [ ] `go run ./cmd/api` im `backend/` läuft, `/healthz` antwortet `{"status":"ok"}`
- [ ] CLAUDE.md verweist korrekt auf den Vault und die Iterations-Struktur

## Notes

- Wir verzichten bewusst auf `create-next-app` und schreiben die minimal nötigen
  Files selbst, um keinen unnötigen Boilerplate (Tailwind-Setup, src/, etc.) mitzunehmen.
- Backend bleibt in Wave 1 minimal — die richtigen Module (DB, Auth, Mail) kommen in Iteration 3+.
