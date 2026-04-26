# Wave 3: Formular-UX und Hansefit-Gating

**Status:** Done (Legacy-Migration)
**Skill:** product-own
**Quelle:** `plan/iterations/iteration-14.md`

## Legacy-Inhalt

## Wave 3 - Formular-UX und Hansefit-Gating
**Agenten**: Agent D, Agent C, Agent E
**Kann einzeln ausgefuehrt werden**: ja, nach Wave 2
**Ziel**: Nutzer koennen das Formular nur mit den jeweils relevanten Pflichtfeldern absenden; Hansefit-Felder erscheinen erst nach Checkbox-Aktivierung.

### Task: 14.7 Kontaktformular-Markup umbauen
**Priority**: P0
**Blocked by**: 14.4, 14.5
**Agent**: Agent D

**Akzeptanzkriterien**:
- [ ] Formular enthaelt Nachname, Vorname und E-Mail als immer sichtbare Pflichtfelder.
- [ ] Freitext/Nachricht ist immer sichtbar und optional.
- [ ] Datenschutzerklaerung und AGB sind als verpflichtende Checkboxen sichtbar.
- [ ] Hansefit-Registrierung ist eine Checkbox.
- [ ] Hansefit-ID und Playtomic-E-Mail sind initial nicht sichtbar.
- [ ] Hansefit-ID und Playtomic-E-Mail erscheinen erst nach aktivierter Hansefit-Checkbox.
- [ ] Bei Server-Validierungsfehlern bleiben Hansefit-Felder sichtbar, wenn Hansefit aktiv war.
- [ ] Labels und Hilfetexte sind eindeutig, kurz und nicht widerspruechlich.

**Kontext**:
- `web/templates/pages/contact.html`
- `web/static/css/style.css`

**Output**:
- Ueberarbeitetes Kontaktformular-Template.
- Aktualisierte Formularstyles.

### Task: 14.8 Client-seitige Blockade des Absenden-Buttons
**Priority**: P0
**Blocked by**: 14.7
**Agent**: Agent D

**Akzeptanzkriterien**:
- [ ] `novalidate` wird entfernt oder durch eine eigene, vollstaendige Validierungslogik ersetzt.
- [ ] Native `required`-Attribute blockieren Absenden fuer immer verpflichtende Felder.
- [ ] Hansefit-Felder erhalten `required` nur bei aktivierter Hansefit-Checkbox.
- [ ] Hansefit-Felder sind bei deaktivierter Checkbox `hidden` und `disabled`.
- [ ] Beim Deaktivieren der Hansefit-Checkbox werden Hansefit-Feldwerte geloescht oder serverseitig ignoriert.
- [ ] Submit-Button bleibt bedienbar, aber Browser/JS verhindern ungueltige Submission.
- [ ] Ohne JavaScript bleibt die serverseitige Validierung korrekt; progressive Enhancement bricht den Flow nicht.

**Kontext**:
- `web/static/js/main.js`
- `web/templates/pages/contact.html`

**Output**:
- Aktualisierte Hansefit-JS-Logik.
- Manuell pruefbare Client-Validierung.

### Task: 14.9 Accessibility und mobile Formular-QA
**Priority**: P1
**Blocked by**: 14.7, 14.8
**Agent**: Agent D + Agent E

**Akzeptanzkriterien**:
- [ ] Checkboxen sind per Tastatur erreichbar und klar fokussierbar.
- [ ] `aria-controls` und `aria-expanded` spiegeln den Hansefit-Zustand korrekt.
- [ ] Versteckte Hansefit-Felder werden nicht per Tab-Fokus erreicht.
- [ ] Mobile Layout hat keine Ueberlappungen oder zu kleine Touch-Ziele.
- [ ] Fehlermeldungen bleiben fuer Screenreader nachvollziehbar.

**Kontext**:
- `web/templates/pages/contact.html`
- `web/static/css/style.css`
- `web/static/js/main.js`

**Output**:
- Accessibility-Checkliste in QA-Notizen.

**Wave-Exit-Gate**:
- [ ] Hansefit-Felder erscheinen nur nach aktiver Checkbox.
- [ ] Client verhindert ungueltiges Absenden fuer alle Pflichtfelder.
- [ ] Server bleibt Quelle der Wahrheit fuer Validierung.

---
