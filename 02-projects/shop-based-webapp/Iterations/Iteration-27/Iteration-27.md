# Iteration 27 - Admin-Portal Redirect, Admin-Buchungsliste und Bild-Revert

**Status:** Done
**Repo:** /home/andersen/git/shop-based-webapp
**Datum:** 2026-04-26
**Rolle:** Acting as Product Owner. Planning Iteration 27 for shop-based-webapp.

## Ziel

Iteration 27 setzt drei konkrete UI-/Flow-Korrekturen um: Admins sollen nach dem Login im Admin Portal direkt auf `/admin` landen, die neuesten Buchungseingaenge sollen im Admin Dashboard vollstaendig lesbar sein, und die in Iteration 25/26 beruehrten Bilddarstellungen fuer "Ueber uns" und Gallery werden optisch korrigiert. Die Umsetzung ist bewusst frontend-nah und regressionsorientiert.

## Kundenfeedback

1. Nach Login beim Admin Portal direkt auf `/admin` weiterleiten.
2. "Neuste Buchungseintraege" soll vollstaendig auf der Seite angezeigt werden koennen.
3. Falls noetig soll "Traffic nach Seiten" dafuer nach unten verschoben werden.
4. Beim "Ueber uns" werden Bilder wieder gezoomt dargestellt; diese Aenderung wieder rueckgaengig machen.
5. Auch fuer die Gallery-Bilder wieder den alten Stand herstellen.
6. Keine runden Ecken, weder bei "Ueber uns" noch in der Gallery.
7. Die relevante Bildaenderung wurde in Iteration 25 vorgenommen.

## Scope

**In:**
- `web/customer/src/app/login/page.tsx`: Post-Login Default fuer Admin-Portal-Kontext auf `/admin` fuehren.
- `web/customer/src/middleware.ts`: vorhandene Admin-Portal-Erkennung beruecksichtigen oder fuer Login-Kontext nutzbar machen, ohne Staging-Gate zu brechen.
- `web/customer/src/app/admin/page.tsx`: Layout der neuesten Buchungseingaenge verbreitern; "Traffic nach Seiten" darunter platzieren.
- `web/customer/src/components/AboutUs.tsx`: Ueber-uns Bilder wieder unverzoomt und ohne runde Bildcontainer darstellen.
- `web/customer/src/components/GallerySlider.tsx`: Gallery-Bilder im alten unverzoomten Stand darstellen und runde Ecken am betroffenen Gallery-Frame entfernen.
- Relevante Frontend-Tests, statischer Bild-Audit und Customer-Web Build.

**Out:**
- Backend-Schema- oder API-Aenderungen fuer Buchungen.
- Neue Admin-Detailansicht fuer einzelne Buchungen.
- Neue Bildassets, Retusche oder Asset-Ersetzung.
- Vollstaendiges Redesign des Admin Dashboards.
- Aenderungen an Checkout-, Pricing- oder Payment-Flows.

## Produktentscheidungen

1. **Admin-Portal Login gewinnt gegen generischen Dashboard-Default.**
   Ohne expliziten `redirect` soll der Login im Admin-Portal-Kontext auf `/admin` fuehren. Explizite sichere Redirects wie `/admin/content` bleiben priorisiert.
2. **Redirect-Sicherheit bleibt unveraendert wichtig.**
   Externe URLs, protocol-relative URLs und unsichere Redirect-Ziele duerfen nicht akzeptiert werden.
3. **Buchungsliste bekommt Prioritaet vor Traffic.**
   Die Tabelle "Neueste Buchungseingaenge" soll auf Desktop nicht durch die Traffic-Spalte eingeengt werden. "Traffic nach Seiten" wird unterhalb der Buchungsliste als eigener Abschnitt angezeigt.
4. **Bild-Revert meint unverzoomt plus eckig.**
   Fuer `AboutUs.tsx` und `GallerySlider.tsx` sollen `object-contain` bzw. voll sichtbare Bilder genutzt werden. Runde Ecken an den betroffenen Bildflaechen werden entfernt.
5. **Keine Asset-Aenderungen.**
   Es wird nur die Darstellung korrigiert; Bilddateien selbst bleiben unveraendert.

## Bestandsaufnahme

| Bereich | Datei | Befund |
|---|---|---|
| Login Redirect | `web/customer/src/app/login/page.tsx` | `resolvePostLoginRedirect` faellt ohne Redirect auf `/dashboard` zurueck. |
| Admin Portal Host | `web/customer/src/middleware.ts` | Root `/` auf `adminportal:*` wird bereits nach `/admin` umgeleitet; `/login` nach erfolgreichem Login faellt clientseitig aber noch auf `/dashboard`. |
| Admin Buchungsliste | `web/customer/src/app/admin/page.tsx` | Buchungsliste und "Traffic nach Seiten" teilen sich auf XL eine zweispaltige Grid-Zeile, wodurch die Tabelle eingeengt wird. |
| Admin Dashboard API | `internal/camps/adapters/http/handler.go`, `internal/camps/app/service.go` | Dashboard akzeptiert bereits `limit`; kein Backend-Contract ist fuer das Layoutproblem erforderlich. |
| Ueber-uns Bilder | `web/customer/src/components/AboutUs.tsx` | Aktuell `object-cover` mit manueller `object-position` und runden Bildcontainern (`rounded-2xl`). |
| Gallery | `web/customer/src/components/GallerySlider.tsx` | Bild nutzt `object-contain`, aber der Slider-Frame ist rund (`rounded-[--radius-xl]`). |
| Warum-wir Bilder | `web/customer/src/app/warum-wir/page.tsx` | Weiterhin Iteration-25-nahe `object-contain` Darstellung; bleibt ausser Scope, solange nicht explizit anders beauftragt. |

## Waves

| Wave | Skill | Agent | Focus | Status |
|---|---|---|---|---|
| Wave 1 | handler-eng | Handler/API Edge Engineer | Admin-Portal Login Redirect sicher korrigieren | Done |
| Wave 2 | frontend-eng | Frontend Engineer | Admin Dashboard Buchungsliste full-width, Traffic darunter | Done |
| Wave 3 | frontend-eng | Frontend Engineer | Ueber-uns und Gallery Bilddarstellung auf unverzoomt/eckig korrigieren | Done |
| Wave 4 | testing-eng | QA Engineer | Regression, statischer Audit, Tests und Build | Done |

## Critical Path

```text
Wave 1 (Admin-Portal Redirect)
Wave 2 (Admin Dashboard Layout)
Wave 3 (Bild-Revert)
    v
Wave 4 (Regression, Tests, Build)
```

Waves 1 bis 3 sind fachlich unabhaengig und koennen parallel vorbereitet werden. Wave 4 startet erst nach Integration aller UI-/Flow-Aenderungen.

## Risiken und Mitigationen

| Risiko | Impact | Mitigation |
|---|---|---|
| Login-Redirect oeffnet externe Redirect-Moeglichkeiten | Hoch | Bestehende `resolvePostLoginRedirect` Sicherheitsregeln beibehalten und testen. |
| Admin-Portal-Erkennung ist lokal/staging anders als erwartet | Mittel | Vorhandenes `adminportal:` Host-Pattern aus `middleware.ts` wiederverwenden und Fallback fuer expliziten `redirect` behalten. |
| Buchungstabelle wird auf Mobile unhandlich | Mittel | Horizontalen Scroll behalten, aber Desktop full-width machen. |
| "Traffic nach Seiten" verliert Sichtbarkeit | Niedrig | Als eigener Abschnitt direkt unter der Buchungsliste platzieren, nicht entfernen. |
| Bild-Revert kollidiert mit Iteration-26-Ziel "soft roundness" | Mittel | Neues Kundenfeedback priorisiert "keine runden Ecken"; Wave dokumentiert die bewusste Ruecknahme. |
| Statischer Audit ist zu breit und findet gewollte Rundungen anderer UI-Elemente | Niedrig | Audit auf `AboutUs.tsx` und `GallerySlider.tsx` begrenzen und Befunde kontextuell bewerten. |

## Gesamt-Akzeptanzkriterien

- [x] Login im Admin-Portal-Kontext fuehrt nach erfolgreichem Login ohne expliziten Redirect direkt zu `/admin`.
- [x] Explizite sichere Redirects, insbesondere `/admin/...`, funktionieren weiterhin.
- [x] Unsichere Redirect-Ziele wie externe URLs oder `//example.com` fallen weiterhin auf einen sicheren Default zurueck.
- [x] Die Admin-Buchungsliste nutzt auf Desktop die verfuegbare Seitenbreite.
- [x] "Traffic nach Seiten" steht unterhalb der neuesten Buchungseingaenge und bleibt sichtbar.
- [x] `AboutUs.tsx` zeigt Ariadna/Sascha unverzoomt, ohne `object-cover` und ohne manuelle `object-position`.
- [x] Betroffene Ueber-uns-Bildflaechen haben keine runden Ecken.
- [x] `GallerySlider.tsx` zeigt Gallery-Bilder unverzoomt/voll sichtbar.
- [x] Der betroffene Gallery-Bildframe hat keine runden Ecken.
- [x] Customer-Web Tests und Build sind gruen oder Blocker sind konkret dokumentiert.

## Notes

Diese Iteration korrigiert bewusst Feedback aus den letzten Bild-Iterationen. Die Umsetzung soll klein bleiben: kein neues visuelles Konzept, sondern gezielte Ruecknahme von Zoom/Crop und Roundness an den betroffenen Stellen.

Abschluss 2026-04-26:
- Wave 1 bis Wave 4 abgeschlossen.
- Customer-Web Tests: `npm test -- --configLoader runner` mit 10 Testdateien / 79 Tests gruen.
- Customer-Web Build: `npm run build` erfolgreich.
- Statischer Audit fuer Redirect, Admin-Layout, AboutUs und Gallery bestanden.
- Browser-/Screenshot-Smoke lokal nicht ausgefuehrt, da kein Playwright/Browser-Tool installiert ist; dies ist in Wave 4 dokumentiert.
