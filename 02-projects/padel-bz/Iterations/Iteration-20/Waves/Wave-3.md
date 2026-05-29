# Wave 3: Testing und Verification

**Status:** Done (Legacy-Migration)
**Skill:** testing-eng
**Quelle:** `plan/iterations/iteration-20.md`

## Legacy-Inhalt

## Wave 3 — Testing und Verification

**Agenten**: Agent B (testing-eng)
**Kann parallel ausgefuehrt werden**: ja
**Blocked by**: Wave 2
**Ziel**: Regressionspruefung, Tests ausfuehren.

### Task 20.4: Regressionstests und Verifikation
**Priority**: P1
**Blocked by**: 20.3
**Agent**: Agent B (testing-eng)

**Akzeptanzkriterien**:
- [ ] `go test ./...` ist gruen.
- [ ] Render-Tests fuer `home.html` bestehen.
- [ ] SVG-Dateien sind valide.
- [ ] `git diff --check` ist gruen.
- [ ] Keine Whitespace-Fehler.

**Kontext**:
- `web/templates/pages/home.html`
- `web/static/css/style.css`
- `web/static/img/sponsors/`

**Output**:
- Gruene Tests, QA-Notiz.

**Wave-3-Exit-Gate**:
- [ ] Alle Tests gruen.
- [ ] Keine Regressionen.

---
