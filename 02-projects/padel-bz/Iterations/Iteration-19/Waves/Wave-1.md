# Wave 1: Footer bereinigen und SVG-Kommentare korrigieren

**Status:** Done (Legacy-Migration)
**Skill:** frontend-eng
**Quelle:** `plan/iterations/iteration-19.md`

## Legacy-Inhalt

## Wave 1 — Footer bereinigen und SVG-Kommentare korrigieren
**Agenten**: Agent A (frontend-eng)
**Kann parallel ausgefuehrt werden**: ja (Footer und SVG-Kommentare sind unabhaengig)
**Ziel**: Schnelle Cleanup-Tasks die keine Abhaengigkeiten haben.

### Task 19.1: Footer-Links bereinigen
**Priority**: P0
**Blocked by**: none
**Agent**: Agent A (frontend-eng)

**Akzeptanzkriterien**:
- [ ] "Court buchen" (`<li><a href="/kontakt">Court buchen</a></li>`) ist aus dem Footer entfernt.
- [ ] "Mitgliedschaft" (`<li><a href="/kontakt">Mitgliedschaft</a></li>`) ist aus dem Footer entfernt.
- [ ] "Niedersachsen" (`<li><a href="/#kontakt">Niedersachsen</a></li>`) ist aus dem Footer entfernt.
- [ ] Verbleibende "Spielen" Links: Hansefit, GPS Tour.
- [ ] Verbleibende "Standort" Links: Bad Zwischenahn, Anfahrt.
- [ ] Footer rendert korrekt ohne Layout-Bruch.

**Kontext**:
- `web/templates/partials/footer.html`

**Output**:
- Aktualisiertes `web/templates/partials/footer.html`.

### Task 19.2: SVG-Kommentare auf "Autobahnabfahrt" korrigieren
**Priority**: P1
**Blocked by**: none
**Agent**: Agent A (frontend-eng)

**Akzeptanzkriterien**:
- [ ] HTML-Kommentar fuer den oberen Kreisel (cx=390, cy=88) lautet "Autobahnabfahrt A28" statt "Kreisverkehr oben an A28".
- [ ] Der untere Kreisel (cx=410, cy=120) bleibt als "Kreisverkehr" oder wird als "Abzweigung" umbenannt — je nach Kontext.
- [ ] Keine visuellen Aenderungen in diesem Task.

**Kontext**:
- `web/templates/pages/home.html`, Zeilen 270-277

**Output**:
- Aktualisierte Kommentare in `web/templates/pages/home.html`.

**Wave-1-Exit-Gate**:
- [ ] Footer zeigt nur noch Hansefit + GPS Tour unter "Spielen" und Bad Zwischenahn + Anfahrt unter "Standort".
- [ ] SVG-Kommentare sind inhaltlich korrekt.

---
