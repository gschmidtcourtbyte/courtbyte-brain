# Wave 1 - Assets und gleichberechtigte Gründer-Vorstellung

**Iteration:** [[Iteration-24]]
**Skill:** frontend-eng
**Status:** Done

## Source

Migrated from `/home/andersen/git/shop-based-webapp/docs/plans/Iteration-24.md`.

## Inhalt

**Ziel:** Ariadna und Sascha werden auf der "Warum wir?" Seite und in der Homepage-Über-uns-Sektion mit neuen Bildern gleichwertig nebeneinander vorgestellt.

| Workstream | Task | Primäres Skillset | Mitwirkend |
|-----------|------|-------------------|------------|
| Asset Pipeline | `Bilder/Ariadna.jpeg` und `Bilder/Sascha.png` als webfähige Assets unter `web/customer/public/images/...` bereitstellen | `frontend-eng` | `review-eng` |
| Warum-wir Layout | Ariadna- und Sascha-Profile als gleich große Cards/Spalten nebeneinander darstellen, responsiv gestapelt auf Mobile | `frontend-eng` | `review-eng` |
| Homepage Über-uns Layout | `AboutUs.tsx` auf gleichberechtigte Zweierdarstellung umbauen oder bewusst mit der Warum-wir-Darstellung harmonisieren | `frontend-eng` | `review-eng` |
| Content Preservation | Bereits gemachte Content-Anpassungen erhalten und nur Layout/Bildreferenzen gezielt ändern | `frontend-eng` | `product-own` |

**Abhängigkeiten:** keine  
**Parallelisierbar:** Asset-Kopie/Optimierung und Layout-Entwurf sind parallel vorbereitbar, finale Integration sequenziell.  
**Exit Gate:** Beide Profile nutzen neue Bilder, haben gleiche visuelle Hierarchie und bleiben auf Mobile sauber lesbar.

### Akzeptanzkriterien

- [x] Ariadna nutzt ein echtes Bild statt Initial-Platzhalter.
- [x] Sascha nutzt das neue Bild aus `Bilder/Sascha.png` statt des alten `/images/Sascha.jpg`, sofern das neue Bild qualitativ geeignet ist.
- [x] Auf Desktop stehen Ariadna und Sascha in `warum-wir/page.tsx` und `AboutUs.tsx` nebeneinander mit gleichen Card-Dimensionen, Bildgrößen und typografischer Gewichtung.
- [x] Auf Mobile stapeln die Profile kontrolliert ohne abgeschnittene Bilder oder Textüberlagerung.
- [x] Bereits vorhandene Content-Änderungen in `warum-wir/page.tsx` und `AboutUs.tsx` werden nicht versehentlich zurückgesetzt.

### Status

**Abgeschlossen.** Die neuen Portraits wurden nach `web/customer/public/images/founders/` übernommen. `warum-wir/page.tsx` und `AboutUs.tsx` nutzen jetzt gleichwertige Profilkarten mit identischem Bildformat, gleicher typografischer Gewichtung und responsiver Stapelung.

---
