# Wave 1 - Admin-Portal Login Redirect

**Iteration:** [[Iteration-27]]
**Skill:** handler-eng
**Status:** Done

## Ziel

Nach erfolgreichem Login im Admin-Portal-Kontext landet der Nutzer direkt auf `/admin`, sofern kein expliziter sicherer Redirect angegeben wurde. Die bestehende Redirect-Haertung bleibt erhalten.

## Agent

Handler/API Edge Engineer: Fokus auf Auth-Flow, Middleware-Kontext und sichere Redirect-Entscheidung am Web-Rand.

## Tasks

- [x] `web/customer/src/app/login/page.tsx`: Default-Redirect fuer Admin-Portal-Kontext auf `/admin` erweitern.
- [x] `web/customer/src/app/login/page.tsx`: bestehende Sicherheitsregeln fuer `redirect` beibehalten (`/`-Prefix, kein `//`).
- [x] `web/customer/src/middleware.ts`: pruefen, ob der vorhandene `adminportal:` Host-Kontext fuer `/login` nutzbar gemacht werden muss, ohne Staging-Gate-Logik zu veraendern.
- [x] Bei Bedarf kleinen Helper einfuehren, damit Redirect-Logik testbar bleibt.
- [x] Keine Aenderungen an Backend-Auth-Endpunkten vornehmen.

## Akzeptanzkriterien

- [x] Login ohne `redirect` fuehrt im Admin-Portal-Kontext nach `/admin`.
- [x] Login ohne Admin-Portal-Kontext fuehrt weiterhin auf den normalen sicheren Default.
- [x] Login mit `redirect=/admin/content` fuehrt weiterhin zu `/admin/content`.
- [x] Login mit unsicherem Redirect wie `https://example.com` oder `//example.com` wird nicht akzeptiert.
- [x] Staging-Gate-Weiterleitungen bleiben unveraendert.

## Abhaengigkeiten

Keine. Diese Wave kann parallel zu Wave 2 und Wave 3 vorbereitet werden.

## Notes

Bestandsbefund: `middleware.ts` leitet `adminportal:*` Root bereits nach `/admin`, aber `login/page.tsx` nutzt ohne Query-Redirect aktuell `/dashboard` als Fallback.

Umsetzung 2026-04-26:
- `web/customer/src/lib/post-login-redirect.ts` eingefuehrt und in Login + Middleware wiederverwendet.
- Admin-Portal-Fallback ist `/admin`; normaler Fallback bleibt `/dashboard`.
- Unsichere Redirects mit externem oder protocol-relative Ziel fallen auf den jeweiligen sicheren Default zurueck.
- Staging-Gate-Reihenfolge blieb unveraendert; nur die Admin-Host-Erkennung wurde extrahiert.
- Verifiziert mit `npm test -- --configLoader runner src/lib/post-login-redirect.test.ts src/lib/stagingGate.test.ts` und `npm run build`.
