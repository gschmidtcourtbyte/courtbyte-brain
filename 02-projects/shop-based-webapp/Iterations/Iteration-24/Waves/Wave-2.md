# Wave 2 - "Sicherheit & Vertrauen" kompakt mit "Mehr erfahren"

**Iteration:** [[Iteration-24]]
**Skill:** frontend-eng
**Status:** Done

## Source

Migrated from `/home/andersen/git/shop-based-webapp/docs/plans/Iteration-24.md`.

## Inhalt

**Ziel:** Die Versicherungssektion erklärt Unfallversicherung und Betriebshaftpflicht direkt in den oberen Kacheln über eine hochwertige "Mehr erfahren"-Interaktion; der untere Bild-/Detailbereich entfällt.

| Workstream | Task | Primäres Skillset | Mitwirkend |
|-----------|------|-------------------|------------|
| UX Pattern | Darstellung für "Mehr erfahren" entwickeln: z. B. elegante Inline-Expansion, Drawer-ähnlicher Detailbereich oder Card-Aufklappung innerhalb des bestehenden Designs | `frontend-eng` | `review-eng` |
| Content Migration | Detailtexte aus den unteren Kacheln in die passenden oberen Kacheln übertragen | `frontend-eng` | `product-own` |
| Section Cleanup | Unteres Bild und die separaten Detailkacheln entfernen; Layout neu ausbalancieren | `frontend-eng` | `review-eng` |
| Accessibility | Buttons/Controls semantisch korrekt mit `aria-expanded` oder äquivalentem Zustand auszeichnen | `frontend-eng` | `testing-eng` |

**Abhängigkeiten:** Wave 1 kann unabhängig laufen; Umsetzung kann in derselben Datei kollidieren, daher final sequenziell integrieren.  
**Parallelisierbar:** UX-Pattern und Content-Mapping können parallel vorbereitet werden.  
**Exit Gate:** Die drei Sicherheitskacheln bleiben sichtbar; Unfallversicherung und Betriebshaftpflicht haben jeweils einen "Mehr erfahren"-Zugang mit Detailcontent; das untere Bild ist entfernt.

### Akzeptanzkriterien

- [x] In den Kacheln "Unfallversicherung" und "Betriebshaftpflicht" gibt es einen sichtbar benannten Button/Link "Mehr erfahren".
- [x] Der Detailcontent stammt fachlich aus den bisherigen unteren Kacheln.
- [x] Die Insolvenzversicherungs-Kachel bleibt konsistent dargestellt; falls kein zusätzlicher Detailtext existiert, bleibt sie ohne künstliche Füllkopie oder erhält nur eine kurze passende Erklärung.
- [x] Das Bild `/images/camp/Prio1.jpeg` wird in dieser Sektion nicht mehr als unteres Detailbild gerendert.
- [x] Die Interaktion funktioniert per Maus, Tastatur und Screenreader-Zustand.

### Status

**Abgeschlossen.** Die oberen Sicherheitskacheln enthalten jetzt die Detailtexte aus den früheren unteren Kacheln über semantische "Mehr erfahren"-Buttons mit `aria-expanded` und `aria-controls`. Der untere Bild-/Detailblock wurde entfernt.

---
