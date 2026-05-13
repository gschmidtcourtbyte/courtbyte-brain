# Iteration 33 - Navbar bereinigen und Paketdarstellung verfeinern

**Status:** Done
**Repo:** shop-based-webapp
**Datum:** 2026-05-11

## Ziel
Iteration 33 verbessert die oeffentliche OUT-Padel-Startseite mit drei gezielten Frontend-Anpassungen: Login/Signup verschwinden aus der Navbar, die Paketsektion bekommt einen dezenten Zahlungs-Hinweis, und Single-/Doppel-Paketkarten werden visuell besser ausgerichtet.

## Scope
**In:** Customer-Web-Navigation, Paketkarten-Layout, Paket-Hinweistexte und passende Frontend-Verifikation.

**Out:** Backend-, DB-, Auth-Flow-, Admin- oder Routen-Entfernungsaenderungen.

## Waves
| Wave | Skill | Focus | Status |
|---|---|---|---|
| Wave 1 | frontend-eng | Navbar-Auth-Links entfernen und Paketsektion anpassen | Done |
| Wave 2 | testing-eng | Responsive Smoke, Build/Lint und gezielte UI-Regression pruefen | Done |
| Wave 3 | review-eng | Scope-, Content- und UX-Review gegen Akzeptanzkriterien | Done |

## Acceptance Criteria
- [x] Desktop-Navbar zeigt ausgeloggt keine `Sign in`-/`Sign up`-Buttons.
- [x] Mobile-Menue zeigt ausgeloggt keine `Sign in`-/`Sign up`-Eintraege.
- [x] Authenticated-Zustand behaelt Dashboard, Profile und Logout.
- [x] `/login` und `/signup` bleiben direkt erreichbar.
- [x] Footer-Link `Login` bleibt sichtbar.
- [x] Zahlungs-Hinweis steht unter den Paketen und bleibt kleiner/ruhiger als Paketkarten und CTA.
- [x] Saisonpreis-Hinweis bleibt erhalten.
- [x] Single- und Doppel-Paket-Featurekaesten beginnen auf Desktop auf gleicher Hoehe.
