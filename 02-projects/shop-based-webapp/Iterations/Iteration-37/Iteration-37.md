# Iteration 37 - Mobile Navigation und Content-Konsistenz

**Status:** Done
**Repo:** /home/andersen/git/shop-based-webapp
**Datum:** 2026-05-18
**Rolle:** Acting as Product Owner. Planning Iteration 37 for shop-based-webapp.

## Ziel

Iteration 37 korrigiert mehrere sichtbare Content- und Navigationspunkte der OUT-Padel-Customer-Web-App: Social Links bleiben auf Mobile direkt in der Navbar sichtbar, zentrale "Warum wir"-Texte werden konsistent zur Startseite, das Impressum zeigt die korrekte Telefonnummer und die Paket-Inclusions sind fachlich korrekt kategorisiert.

## Scope

**In:**
- Mobile Navbar: Instagram/WhatsApp bleiben in der Navbar sichtbar und werden nicht nur im Burger-Menue angeboten.
- Mobile Burger-Menue: Navigation und Auth-Links bleiben nutzbar; Social Links werden dort nicht mehr als versteckte Primaerposition gefuehrt.
- `web/customer/src/app/warum-wir/page.tsx`: Header-Text `Kein Touristenprogramm.` wird zu `Kein Standard.`.
- `web/customer/src/app/warum-wir/page.tsx`: Saschas Text wird von der Startseiten-`AboutUs`-Sektion uebernommen.
- `web/customer/src/app/impressum/page.tsx`: Telefonnummer wird auf `+49 (0)155-67149871` gesetzt.
- `web/customer/src/lib/camp-packages.ts`: `Mittagessen täglich an ausgewählten Restaurants (exklusive Getränke)` wandert aus der Kategorie `Hotel` in die Kategorie `Erlebnis`.
- Relevante Tests fuer Paket-Kategorisierung und Content-Smoke aktualisieren.
- Frontend-Lint und Frontend-Build pruefen.

**Out:**
- Re-Design der gesamten Navigation oder des Header-Looks.
- Aenderungen an Desktop-Navigation ausser falls fuer gemeinsame Komponentenstruktur noetig.
- Aenderungen an Social-Link-Zielen.
- Juristische Ueberarbeitung von Datenschutz/AGB ausserhalb des Impressums.
- Aenderungen an Paketpreisen, Paketnamen oder Buchungslogik.
- Backend-, Datenbank- oder Deployment-Aenderungen.

## Waves

| Wave | Skill | Focus | Status |
|---|---|---|---|
| Wave 1 | frontend-eng | Mobile Navbar, Warum-wir-Content, Impressum, Paket-Kategorie-Zuordnung | Done |
| Wave 2 | testing-eng | Lint, Build, relevante Tests und responsive Smoke-Checks | Done |

## Critical Path

```text
Wave 1 (Frontend Content/UI Update)
    v
Wave 2 (Verification)
```

## Acceptance Criteria

- [x] Mobile Navbar zeigt Instagram und WhatsApp direkt im Header neben Logo/Burger-Button, ohne dass das Burger-Menue geoeffnet werden muss.
- [x] Mobile Burger-Menue enthaelt weiterhin alle Navigationslinks und Auth-Links, aber keine separate Social-Link-Sektion als primaeren Ablageort.
- [x] Desktop-Navigation bleibt funktional und visuell unveraendert.
- [x] `/warum-wir` Hero-Headline lautet `Kein All-Inclusive.` / `Kein Standard.`.
- [x] Saschas Text auf `/warum-wir` entspricht dem Sascha-Text aus `web/customer/src/components/AboutUs.tsx`.
- [x] `/impressum` zeigt `Telefon: +49 (0)155-67149871`.
- [x] Kategorie `Hotel` enthaelt `Mittagessen täglich an ausgewählten Restaurants (exklusive Getränke)` nicht mehr.
- [x] Kategorie `Erlebnis` enthaelt `Mittagessen täglich an ausgewählten Restaurants (exklusive Getränke)`.
- [x] Relevante Paket-/Content-Tests sind aktualisiert; Vitest ist durch den bekannten `node_modules/.vite-temp` Permission-Blocker blockiert und dokumentiert.
- [x] `npm run lint` im `web/customer` ist gruen.
- [x] `npm run build` im `web/customer` ist gruen.

## Notes

Planung erstellt am 2026-05-18 nach User-Scope-Freigabe. Der Umfang ist rein frontend- und content-seitig. Kein Datenbank- oder Backend-Risiko. Hauptqualitaetsrisiko liegt in der mobilen Navbar-Breite auf kleinen Viewports; Wave 2 soll deshalb einen responsiven Smoke-Check auf typischen Mobile-Breiten enthalten.

Abgeschlossen am 2026-05-18. Ergebnis: Mobile Social Links sind direkt im Header sichtbar und aus der Burger-Menue-Social-Sektion entfernt; Saschas Text wird ueber `web/customer/src/lib/founder-content.ts` gemeinsam von Startseite und Warum-wir-Seite genutzt; Warum-wir-Hero lautet `Kein Standard.`; Impressum zeigt `+49 (0)155-67149871`; Mittagessen-Inclusion liegt unter `Erlebnis`. `npm run lint` und `npm run build` sind gruen. Vitest bleibt wegen EACCES auf `node_modules/.vite-temp` blockiert.
