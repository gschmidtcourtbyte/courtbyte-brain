# Wave 2: Kontaktformular, Hansefit-Logo und Bauphasen-UI

**Status:** Done (Legacy-Migration)
**Skill:** product-own
**Quelle:** `plan/iterations/iteration-12.md`

## Legacy-Inhalt

## Wave 2 - Kontaktformular, Hansefit-Logo und Bauphasen-UI
**Agenten**: Agent D, Agent C
**Kann einzeln ausgefuehrt werden**: ja, nach Wave 1 fuer Formularspeicherung
**Ziel**: Besucher koennen korrekte Hansefit-Daten absenden; Bauphasen sind klickbar.

### Task: 12.3 Kontaktformular Hansefit anpassen
**Priority**: P0
**Blocked by**: 12.2
**Agent**: Agent D

**Akzeptanzkriterien**:
- [x] Formular enthaelt Feld `Playtomic-E-Mail-Adresse`.
- [x] Formular enthaelt Checkbox mit Label `Hansefitregistrierung`.
- [x] Nicht angehakte Checkbox blockiert den Submit nicht.
- [x] Bei nicht angehakter Checkbox wird die Anfrage gespeichert und Betreiber-E-Mail versendet, erscheint aber nicht in der Hansefit-Admin-Liste.
- [x] Playtomic-E-Mail wird validiert, wenn Hansefitregistrierung angehakt ist.
- [x] Validierungsfehler erscheinen im bestehenden Fehlerbereich.
- [x] Hansefit-Logo ist sichtbar eingebunden, ohne Layout-Bruch auf Mobile.

**Kontext**:
- Formular: `web/templates/pages/contact.html`
- Handler: `internal/handler/contact.go`
- Logo-Quelle: `logo/hansefit.png`
- Ziel fuer Web-Asset: `web/static/img/hansefit.png`

**Output**:
- Aktualisiertes Kontaktformular
- Kopiertes Hansefit-Logo in Web-Assets
- Angepasster Contact-Handler

### Task: 12.4 Bauphasen-Auswahl auf Startseite
**Priority**: P0
**Blocked by**: none
**Agent**: Agent D

**Akzeptanzkriterien**:
- [x] Startseite zeigt Bauphasen-Auswahl im Baufortschritt-Bereich.
- [x] `Rohbau` ist initial aktiv und steht im Vordergrund.
- [x] Der aktuelle Status behalt die kleine pulsierende Animation.
- [x] Klick auf `Hallenbau` zeigt den Hallenbau-Inhalt.
- [x] Hallenbau verwendet den vorhandenen gestreiften Platzhalter (`.ph`) statt eines Videos.
- [x] Pro Bauphase wird maximal ein Video bzw. ein Platzhalter angezeigt.
- [x] Alle Bauphasen werden mit der Seite geladen; Umschalten erfolgt clientseitig.
- [x] Ohne JavaScript bleibt der aktuelle Baufortschritt sichtbar.
- [x] Desktop und Mobile haben keine Text-/Video-Ueberlappungen.

**Kontext**:
- Startseite: `web/templates/pages/home.html`
- CSS: `web/static/css/style.css`
- JS: `web/static/js/main.js`
- Aktuelles Video: `/static/videos/construction.mp4`

**Output**:
- Aktualisierte Baufortschritt-Section
- CSS fuer aktive/inaktive Bauphase
- Minimaler JS-Toggle fuer Phase-Auswahl

**Wave-Exit-Gate**:
- [x] Kontaktformular kann neue Daten absenden.
- [x] Hansefit-Logo ist eingebunden.
- [x] Bauphasen sind im Browser bedienbar.

---
