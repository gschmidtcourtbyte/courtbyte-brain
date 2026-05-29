# Wave 5 - Experience temporär ausblenden, Navigation bereinigen

**Iteration:** [[Iteration-24]]
**Skill:** frontend-eng
**Status:** Done

## Source

Migrated from `/home/andersen/git/shop-based-webapp/docs/plans/Iteration-24.md`.

## Inhalt

**Ziel:** Die Experience Section wird vorerst aus der Customer Journey entfernt, ohne die spätere Reaktivierung unnötig zu erschweren. Die Impressionen rücken direkt unter die Pakete.

| Workstream | Task | Primäres Skillset | Mitwirkend |
|-----------|------|-------------------|------------|
| Homepage Flow | Experience Section aus `web/customer/src/app/page.tsx` entfernen oder feature-flag-ähnlich ausblenden | `frontend-eng` | `product-own` |
| Navigation | `Experience` aus `Navigation.tsx` entfernen | `frontend-eng` | `review-eng` |
| Footer | `Experience` aus `Footer.tsx` entfernen; Footer-Spalte sauber halten | `frontend-eng` | `review-eng` |
| Reaktivierungsnotiz | In Plan oder Content-Doku festhalten, dass Experience später wiederkommen soll | `documentation-eng` | `product-own` |

**Abhängigkeiten:** Sollte nach Wave 3/4 erfolgen, weil dadurch die finale Reihenfolge `Pricing -> Gallery` geprüft wird.  
**Parallelisierbar:** Navigation/Footer und Homepage-Flow sind kleine getrennte Edits, können zusammen reviewed werden.  
**Exit Gate:** Keine sichtbaren Experience-Links; Gallery/Impressionen folgen direkt auf die Pakete.

### Akzeptanzkriterien

- [x] `/#experience` wird in Navigation und Footer nicht mehr verlinkt.
- [x] Die Startseite rendert nach `Pricing` direkt den `GallerySlider`.
- [x] Der Experience-Content ist nicht sichtbar, aber fachlich als später wieder zu implementieren dokumentiert.
- [x] Keine leeren Anker, ungenutzten Imports oder sichtbaren Layout-Lücken bleiben zurück.

### Status

**Abgeschlossen.** Die Experience Section wurde aus `page.tsx` entfernt, wodurch die Impressionen direkt auf die Paketsektion folgen. Navigation und Footer verlinken weder `/#experience` noch den dadurch ebenfalls entfernten Standort-Anker `/#location`; `docs/Content.md` hält die spätere Reaktivierung als gekoppelte Content-/Navigationsarbeit fest.

---
