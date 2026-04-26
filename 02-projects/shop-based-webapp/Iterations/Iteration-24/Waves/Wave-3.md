# Wave 3 - Homepage Pricing: Highlights raus, "Was immer drin ist" kategorisieren

**Iteration:** [[Iteration-24]]
**Skill:** frontend-eng
**Status:** Done

## Source

Migrated from `/home/andersen/git/shop-based-webapp/docs/plans/Iteration-24.md`.

## Inhalt

**Ziel:** Die Paketsektion wird schlanker: obsolete Highlightkacheln verschwinden, und die Inklusivleistungen werden in vier klar unterscheidbare Kategorien sortiert.

| Workstream | Task | Primäres Skillset | Mitwirkend |
|-----------|------|-------------------|------------|
| Content Model | `PRICING_EDITORIAL_CONTENT` um kategorisierte Included-Daten ergänzen oder bestehende Liste lokal sauber gruppieren | `frontend-eng` | `product-own` |
| Pricing UI | Highlightkachel-Rendering aus `Pricing.tsx` entfernen | `frontend-eng` | `review-eng` |
| Included Categories | Kategorien `Hotel`, `Training`, `Sicherheit`, `Erlebnis` mit eigener Überschrift, Icons/Farbakzent und ruhiger visueller Hierarchie gestalten | `frontend-eng` | `review-eng` |
| Regression Tests | Paket-/Content-Tests für Reihenfolge und Kategorie-Vollständigkeit ergänzen | `testing-eng` | `frontend-eng` |

**Abhängigkeiten:** keine harte Abhängigkeit zu Wave 1/2.  
**Parallelisierbar:** Content-Gruppierung und UI-Entwurf sind parallel möglich; Tests nach finalem Datenmodell.  
**Exit Gate:** Keine obsoleten Highlightkacheln mehr; "Was immer drin ist" zeigt genau vier Kategorien im passenden visuellen Stil.

### Akzeptanzkriterien

- [x] Texte wie "15 Stunden echtes Training! 7 Stunden lockere Einheit" werden nicht mehr als eigene Kacheln gerendert.
- [x] Der Abschnitt "Was immer drin ist" zeigt die vier Kategorien `Hotel`, `Training`, `Sicherheit`, `Erlebnis`.
- [x] Jede Kategorie besitzt eine visuelle Unterscheidung, die zur bestehenden OUT-Padel-Atmosphäre passt und nicht wie ein Fremdtheme wirkt.
- [x] Alle relevanten bisherigen Inklusivleistungen bleiben fachlich erhalten, nur sortiert und gestrafft.
- [x] Tests decken mindestens Paket-Slug-Reihenfolge und die Existenz der vier Kategorien ab.

### Status

**Abgeschlossen.** Die obsoleten Highlightkacheln wurden aus `Pricing.tsx` und dem Pricing-Content-Modell entfernt. `PRICING_EDITORIAL_CONTENT` führt die Inklusivleistungen jetzt in den vier Kategorien `Hotel`, `Training`, `Sicherheit` und `Erlebnis`; die Pricing UI rendert diese Kategorien mit eigenen Icons und Farbakzenten.

---
