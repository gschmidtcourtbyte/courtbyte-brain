# Wave 7 - Qualität, Review und Abschluss

**Iteration:** [[Iteration-24]]
**Skill:** testing-eng
**Status:** Done

## Source

Migrated from `/home/andersen/git/shop-based-webapp/docs/plans/Iteration-24.md`.

## Inhalt

**Ziel:** Die Änderungen sind visuell geprüft, testseitig abgesichert und ohne unbeabsichtigte Rücksetzungen reviewbar.

| Workstream | Task | Primäres Skillset | Mitwirkend |
|-----------|------|-------------------|------------|
| Automated Tests | Relevante Customer-Web Tests ausführen | `testing-eng` | `frontend-eng` |
| Build Check | Customer-Web Build ausführen | `testing-eng` | `infrastructure-eng` |
| Visual Smoke | Desktop und Mobile für Startseite und Warum-wir prüfen | `frontend-eng` | `review-eng` |
| Final Review | Diff auf Scope-Treue, Content-Erhalt, ungenutzte Imports und responsive Brüche prüfen | `review-eng` | `product-own` |
| Documentation | `docs/Content.md` aktualisieren, falls neue Content-Struktur oder Asset-Pfade dokumentationsrelevant sind | `documentation-eng` | `product-own` |

**Abhängigkeiten:** Waves 1 bis 6.  
**Parallelisierbar:** Tests, Build und Review nach Implementierung parallel.  
**Exit Gate:** Tests/Build grün oder Blocker dokumentiert; keine offenen P0/P1 Review-Findings.

### Akzeptanzkriterien

- [x] `cd web/customer && npm test -- --configLoader runner` ist grün oder ein konkreter Blocker ist dokumentiert.
- [x] `cd web/customer && npm run build` ist grün oder ein konkreter lokaler Blocker ist dokumentiert.
- [x] Startseite zeigt die Reihenfolge `Hero -> Camps -> Pakete -> Impressionen -> Über uns/CTA/BookingGuide`.
- [x] Warum-wir und Homepage-Über-uns zeigen gleichberechtigte Profile; Warum-wir zeigt zusätzlich kompakte Versicherungsdetails ohne unteres Bild.
- [x] Keine Content-Anpassungen des Users wurden unbeabsichtigt überschrieben.

### Status

**Abgeschlossen.** Customer-Web Tests und Build sind grün. Der finale Review bestätigt die Startseitenreihenfolge, die gleichberechtigten Gründerprofile, die kompakte Versicherungssektion, die entfernten Experience-/Location-Links, die hotel-first Gallery und die aktualisierte Content-Dokumentation. Der lokale Smoke-Check lief über den Next-Dev-Server für `/` und `/warum-wir`; ein Screenshot-Smoke ist lokal nicht eingerichtet, da kein Headless-Browser-Tool im Customer-Paket oder Systempfad verfügbar ist.

---
