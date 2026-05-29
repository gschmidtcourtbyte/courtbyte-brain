# Wave 2: Formular, Hansefit-Branding und Bautagebuch

**Status:** Done (Legacy-Migration)
**Skill:** product-own
**Quelle:** `plan/iterations/iteration-13.md`

## Legacy-Inhalt

## Wave 2 - Formular, Hansefit-Branding und Bautagebuch
**Agenten**: Agent D, Agent B, Agent C
**Kann einzeln ausgefuehrt werden**: ja, nach Wave 1 fuer Hansefit-ID-Speicherung
**Ziel**: Oeffentliche Website wirkt ruhiger, das Formular fragt die Hansefit-ID korrekt ab und Bautagebuch-Eintraege sind einheitlich auswaehlbar.

### Task: 13.3 Hansefit-Registrierungsformular erweitern
**Priority**: P0
**Blocked by**: 13.2
**Agent**: Agent D + Agent B

**Akzeptanzkriterien**:
- [ ] Formular enthaelt Feld `Hansefit-ID`.
- [ ] Hansefit-ID ist optisch und inhaltlich der Hansefit-Checkbox zugeordnet.
- [ ] Bei aktivierter `Hansefitregistrierung` ist Hansefit-ID Pflicht.
- [ ] Bei nicht aktivierter Checkbox darf das Formular ohne Hansefit-ID abgesendet werden.
- [ ] Validierungsfehler erscheinen im bestehenden Fehlerbereich.
- [ ] Werte bleiben nach Validierungsfehlern im Formular erhalten.
- [ ] Handler liest `hansefit_id` sicher und explizit aus.

**Kontext**:
- `web/templates/pages/contact.html`
- `internal/handler/contact.go`
- `internal/model/contact.go`

**Output**:
- Aktualisiertes Kontaktformular
- Aktualisierter Contact-Handler
- Validierungs-Regressionstest

### Task: 13.4 Hansefit-Logo dezenter integrieren
**Priority**: P1
**Blocked by**: none
**Agent**: Agent D

**Akzeptanzkriterien**:
- [ ] Hansefit-Logo wirkt kleiner und unauffaelliger als in Iteration 12.
- [ ] Logo bleibt sichtbar, aber dominiert weder Kontaktformular noch Startseite.
- [ ] Auf Mobile entsteht kein Layout-Bruch.
- [ ] Logo wird nicht als grosser eigener Content-Block verwendet, sondern als ruhiger Partnerhinweis.
- [ ] Kontrast und Lesbarkeit bleiben ausreichend.

**Kontext**:
- `web/templates/pages/contact.html`
- `web/templates/pages/home.html`
- `web/static/css/style.css`
- Asset: `web/static/img/hansefit.png`

**Output**:
- Schlankere Hansefit-Darstellung
- CSS-Anpassungen fuer dezentes Branding

### Task: 13.5 Bautagebuch-Eintraege ueberarbeiten
**Priority**: P0
**Blocked by**: none
**Agent**: Agent D

**Akzeptanzkriterien**:
- [ ] Zusaetzliches Bild im Baufortschritt wird entfernt.
- [ ] `Aktueller Fortschritt` ist als auswaehlbarer Bautagebuch-Eintrag sichtbar.
- [ ] `Hallenbau` ist als weiterer Bautagebuch-Eintrag im gleichen UI-Muster sichtbar.
- [ ] Aktueller Fortschritt ist beim Seitenaufruf aktiv.
- [ ] Status-Animation bleibt nur beim aktuellen Fortschritt erhalten.
- [ ] Pro Eintrag wird weiterhin nur ein Video oder ein Platzhalter angezeigt.
- [ ] Hallenbau nutzt weiterhin den gestreiften Platzhalter.
- [ ] Ohne JavaScript bleibt der aktuelle Fortschritt sichtbar.
- [ ] Desktop und Mobile haben keine Ueberlappungen.

**Kontext**:
- `web/templates/pages/home.html`
- `web/static/css/style.css`
- `web/static/js/main.js`
- Aktuelles Video: `/static/videos/construction.mp4`

**Output**:
- Ueberarbeiteter Bautagebuch-Bereich
- Entferntes Zusatzbild
- Konsistente Auswahl-Komponente fuer Eintraege

**Wave-Exit-Gate**:
- [ ] Kontaktformular zeigt und validiert Hansefit-ID korrekt.
- [ ] Hansefit ist visuell zurueckgenommen.
- [ ] Bautagebuch ist einheitlich bedienbar.

---
