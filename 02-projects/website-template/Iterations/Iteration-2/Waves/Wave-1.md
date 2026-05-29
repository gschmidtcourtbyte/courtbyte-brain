# Iteration 2 — Wave 1: Plan + Railway Foundations

**Skill:** product-own, infrastructure-eng
**Status:** Done

## Tasks

- [x] Redis-Service in Railway `website-template/staging` erstellen
- [x] Backend-Variablen setzen: `REDIS_URL`, `ADMIN_EMAIL`, `ADMIN_PASSWORD`, `FRONTEND_ORIGIN`
- [x] Frontend-Variablen prüfen: `NEXT_PUBLIC_API_URL`
- [x] Railway-only Verification als Iterationsregel festhalten

## Done

- Railway-Services und bestehende Domains sind bekannt:
  - Frontend: `https://frontend-staging-a1ac.up.railway.app`
  - Backend: `https://backend-staging-97f2.up.railway.app`
- Redis-Service ist in `staging` angelegt und per `REDIS_URL` mit dem Backend verbunden.
