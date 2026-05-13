# Wave 1 - Navbar-Auth-Links entfernen und Paketsektion anpassen

**Iteration:** [[Iteration-33]]
**Skill:** frontend-eng
**Status:** Done

## Tasks
- [x] Desktop-Navbar fuer ausgeloggte Nutzer ohne `Sign in`-/`Sign up`-Buttons rendern - `web/customer/src/components/Navigation.tsx`
- [x] Mobile-Menue fuer ausgeloggte Nutzer ohne `Sign in`-/`Sign up`-Eintraege rendern - `web/customer/src/components/Navigation.tsx`
- [x] Authenticated-Zustand unveraendert lassen - `web/customer/src/components/Navigation.tsx`
- [x] Zahlungs-Hinweis unter den Paketkarten ergaenzen - `web/customer/src/components/Pricing.tsx`, `web/customer/src/lib/camp-packages.ts`
- [x] Single-/Doppel-Paketkarten so ausrichten, dass Featurekaesten auf Desktop gleich starten - `web/customer/src/components/Pricing.tsx`

## Done Criteria
- [x] `/login` und `/signup` Routen bleiben bestehen.
- [x] Bestehender Saisonpreis-Hinweis bleibt erhalten.
- [x] Keine fachlichen Inhalte der anderen Pakete wurden veraendert.

## Notes
- `renderAuthButtons` und `renderMobileAuth` geben fuer ausgeloggte Nutzer `null` zurueck; Loading-Skeleton und eingeloggte Controls bleiben erhalten.
- Der Zahlungs-Hinweis liegt zentral in `PRICING_EDITORIAL_CONTENT.paymentNotice`.
- Paket-Intro-Copy nutzt `xl:min-h-[13rem]`, damit Single und Doppel im Desktop-Grid vor den Featurekaesten gleich hoch laufen.
