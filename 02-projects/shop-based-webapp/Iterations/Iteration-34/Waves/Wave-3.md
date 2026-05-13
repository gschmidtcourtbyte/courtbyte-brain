# Wave 3 - Build, Tests und Performance-Smoke pruefen

**Iteration:** [[Iteration-34]]
**Skill:** testing-eng
**Status:** Done

## Tasks

- [x] Backend-Handler-Test fuer Camps ausfuehren - `internal/camps/adapters/http`
- [x] Gezielte Frontend-API-Tests ausfuehren - `web/customer/src/lib/api.test.ts`
- [x] Customer-Web Production Build ausfuehren - `web/customer`
- [x] Bildgroessen und Build-Routen nach der Optimierung pruefen.
- [x] Production-HTTP-Smoke gegen `padel-out.de` und relevante API-Routen dokumentieren.

## Done Criteria

- [x] Tests und Build sind gruen oder lokale Blocker sind konkret dokumentiert.
- [x] Performance-Smoke zeigt keine offensichtliche Regression.
- [x] `git diff --check` ist sauber.

## Notes

Lokaler Vitest-Default kann wegen `node_modules/.vite-temp` Ownership blockieren; `--configLoader runner` ist als bekannter Workaround dokumentiert.

Verifikation am 2026-05-12: `go test ./internal/camps/adapters/http` gruen; `npm test -- src/lib/api.test.ts 'src/app/api/[...path]/route.test.ts' --configLoader runner` gruen; `npm run build` gruen. Build-Routen blieben stabil (`/` 17.9 kB, `/warum-wir` 5.84 kB, `/api/[...path]` 132 B). Production-Smoke: `https://padel-out.de/` 200 mit ca. 0.274 s TTFB, `https://padel-out.de/api/v1/camp-events` 200 mit ca. 0.441 s TTFB, `https://backend-api-production-7ae9.up.railway.app/api/v1/camp-events` 200 mit ca. 0.293 s TTFB.
