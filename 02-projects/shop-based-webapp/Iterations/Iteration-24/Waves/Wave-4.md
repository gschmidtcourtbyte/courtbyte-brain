# Wave 4 - Paketkarten: Preisposition, Label Cleanup und Reihenfolge

**Iteration:** [[Iteration-24]]
**Skill:** frontend-eng
**Status:** Done

## Source

Migrated from `/home/andersen/git/shop-based-webapp/docs/plans/Iteration-24.md`.

## Inhalt

**Ziel:** Die Paketkarten führen Nutzer klarer zum CTA: Preis direkt oberhalb des Buttons, keine unnötigen Label, zentrierte Preisdarstellung und verbindliche Reihenfolge.

| Workstream | Task | Primäres Skillset | Mitwirkend |
|-----------|------|-------------------|------------|
| Pricing Card Layout | Preisbereich vom oberen Card-Kopf in den unteren CTA-Bereich verschieben | `frontend-eng` | `review-eng` |
| Price Tile Alignment | "Preis" und Betrag innerhalb der Preiskachel zentrieren | `frontend-eng` | `review-eng` |
| Copy Cleanup | Label "Im Paket enthalten" entfernen, Featureliste ohne leeren Kontextbruch darstellen | `frontend-eng` | `product-own` |
| Package Ordering | Reihenfolge `Single`, `Doppel`, `Begleitperson`, `VIP` explizit absichern | `frontend-eng` | `testing-eng` |

**Abhängigkeiten:** Wave 3 berührt denselben Bereich in `Pricing.tsx`; Wave 4 sollte nach Wave 3 integriert werden.  
**Parallelisierbar:** Layout-Review und Testentwurf parallel; Code-Änderung sequenziell zu Wave 3.  
**Exit Gate:** Paketkarten sind scanbarer, der Preis steht unmittelbar vor der Entscheidung und die Reihenfolge ist stabil.

### Akzeptanzkriterien

- [x] In jeder Paketkarte steht der Preis oberhalb des CTA-Buttons.
- [x] "Preis" und Wert/Summe sind zentriert ausgerichtet.
- [x] Der Text "Im Paket enthalten" wird nicht mehr gerendert.
- [x] Reihenfolge der Paketkarten: Single, Doppel, Begleitperson, VIP.
- [x] Saisonpreis-Hinweis und "Preis auf Anfrage" bleiben fachlich korrekt.

### Status

**Abgeschlossen.** Der Preisbereich wurde in den unteren Entscheidungsbereich jeder Paketkarte verschoben und steht jetzt direkt über dem CTA. Die Preiskachel ist zentriert, das Label "Im Paket enthalten" wurde entfernt und die sichtbare Paket-Reihenfolge ist per Test abgesichert.

---
