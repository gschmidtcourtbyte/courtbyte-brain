# Wave 6 - Gallery-Reihenfolge hotel-first

**Iteration:** [[Iteration-24]]
**Skill:** frontend-eng
**Status:** Done

## Source

Migrated from `/home/andersen/git/shop-based-webapp/docs/plans/Iteration-24.md`.

## Inhalt

**Ziel:** Die Impressionen starten mit den aus Kundensicht wichtigsten Hotel-/Ausstattungsbildern und wechseln danach zu Courts und restlicher Atmosphäre.

| Workstream | Task | Primäres Skillset | Mitwirkend |
|-----------|------|-------------------|------------|
| Gallery Ordering | `HOMEPAGE_SLIDES` neu sortieren: Einzelzimmer, Doppelzimmer, Gym, Pool, danach Padel-Courts, danach Rest | `frontend-eng` | `product-own` |
| Copy Check | Titel, Alt-Texte und Captions auf neue Reihenfolge prüfen | `frontend-eng` | `review-eng` |
| Gallery Tests | Exakte erwartete Reihenfolge in `gallery.test.ts` aktualisieren | `testing-eng` | `frontend-eng` |

**Abhängigkeiten:** Keine harte Abhängigkeit zu Pricing, aber sinnvoll nach Wave 5, weil Gallery dann direkter sichtbar ist.  
**Parallelisierbar:** Sortierung und Testupdate sind gekoppelt und sollten zusammen umgesetzt werden.  
**Exit Gate:** Gallery-Test spiegelt die neue hotel-first Reihenfolge.

### Akzeptanzkriterien

- [x] Die ersten vier Gallery-Slides sind Einzelzimmer, Doppelzimmer, Gym und Pool.
- [x] Danach folgen Padel-Court-Motive, bevor sonstige Stimmungs-/Destination-Motive kommen.
- [x] Alle Slides bleiben eindeutig, mit Alt-Text, Titel und Caption.
- [x] `gallery.test.ts` ist auf die neue Reihenfolge aktualisiert.

### Status

**Abgeschlossen.** `HOMEPAGE_SLIDES` startet jetzt mit Einzelzimmer, Doppelzimmer, Gym und Pool. Danach folgen Court-Motive und anschließend die restlichen Team-, Morgenlicht-, Night- und Destination-Motive. `gallery.test.ts` spiegelt die neue Reihenfolge.

---
