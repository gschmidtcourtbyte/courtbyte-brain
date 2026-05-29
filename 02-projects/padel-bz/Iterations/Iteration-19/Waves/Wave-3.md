# Wave 3: Visuelle Politur und Testing

**Status:** Done (Legacy-Migration)
**Skill:** frontend-eng, testing-eng
**Quelle:** `plan/iterations/iteration-19.md`

## Legacy-Inhalt

## Wave 3 — Visuelle Politur und Testing
**Agenten**: Agent A (frontend-eng), Agent B (testing-eng)
**Kann parallel ausgefuehrt werden**: ja
**Ziel**: Feinschliff der Animation, Regressionstests.

### Task 19.6: Animation Feinschliff und Mobile-Optimierung
**Priority**: P1
**Blocked by**: 19.4
**Agent**: Agent A (frontend-eng)

**Akzeptanzkriterien**:
- [ ] Timing und Easing wirken professionell und fluessig.
- [ ] Strassen leuchten beim Draw-In in Accent-Farbe und faden danach auf normale Strassenfarbe zurueck.
- [ ] Pin-Pulse-Animation startet erst nach dem Draw-In.
- [ ] Auf Mobile (< 768px) ist die Animation fluessig, kein Ruckeln.
- [ ] Kein Layout-Shift waehrend der Animation.
- [ ] Karte sieht vor und nach der Animation visuell stimmig aus.

**Kontext**:
- Gesamter Map-Code aus Wave 2.

**Output**:
- Feintuning in CSS und JS.

### Task 19.7: Regressionstests
**Priority**: P1
**Blocked by**: 19.1, 19.4
**Agent**: Agent B (testing-eng)

**Akzeptanzkriterien**:
- [ ] `go test ./...` ist gruen.
- [ ] Render-Tests fuer `home.html` bestehen.
- [ ] Render-Tests fuer `footer.html` bestehen (falls vorhanden).
- [ ] Footer zeigt korrekte Links (keine entfernten Eintraege).
- [ ] Kein JS-Syntaxfehler in `main.js`.
- [ ] `git diff --check` ist gruen.

**Kontext**:
- `internal/handler/render_test.go`
- `web/templates/partials/footer.html`
- `web/static/js/main.js`

**Output**:
- Gruene Tests, QA-Notiz.

**Wave-3-Exit-Gate**:
- [ ] Animation ist visuell poliert und fluesig auf Desktop und Mobile.
- [ ] Alle Tests gruen.
- [ ] Keine Regressionen.

---
