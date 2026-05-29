# Wave 2 - Responsive Smoke, Build/Lint und UI-Regression pruefen

**Iteration:** [[Iteration-33]]
**Skill:** testing-eng
**Status:** Done

## Tasks
- [x] Gezielte Package-Content-Regression pruefen - `web/customer/src/lib/camp-packages.test.ts`
- [x] Vollstaendige Vitest-Suite fuer Customer-Web ausfuehren - `web/customer`
- [x] Lint ausfuehren - `web/customer`
- [x] Production Build ausfuehren - `web/customer`
- [x] Public-Route-Smoke fuer `/`, `/login`, `/signup` ausfuehren - `web/customer`

## Done Criteria
- [x] Tests bestehen.
- [x] Lint ist sauber.
- [x] Build ist sauber.
- [x] Gerenderte Homepage enthaelt Footer-`Login`, Saison-Hinweis und Zahlungs-Hinweis.
- [x] Gerenderte Homepage enthaelt keine Navbar-`Sign in`-/`Sign up`-Texte.

## Notes
- `npm test -- --configLoader runner src/lib/camp-packages.test.ts` bestanden: 8 Tests.
- `npm test -- --configLoader runner` bestanden: 12 Dateien, 92 Tests.
- `npm run lint` bestanden; nur Next.js-Deprecation-Hinweis fuer `next lint`.
- `npm run build` bestanden.
- Dev-Smoke auf `http://127.0.0.1:3002`: `/`, `/login`, `/signup` jeweils HTTP 200.
- Plain `npm test -- src/lib/camp-packages.test.ts` war lokal durch `node_modules/.vite-temp` Ownership blockiert; `--configLoader runner` vermeidet den nicht beschreibbaren Cache-Pfad.
