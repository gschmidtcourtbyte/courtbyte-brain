# Wave 2: Homepage-Struktur: Team ausblenden, Sponsoren vorbereiten

**Status:** Done (Legacy-Migration)
**Skill:** frontend-eng
**Quelle:** `plan/iterations/iteration-16.md`

## Legacy-Inhalt

## Wave 2 - Homepage-Struktur: Team ausblenden, Sponsoren vorbereiten
**Agenten**: Agent E, Agent F
**Kann einzeln ausgefuehrt werden**: ja, parallel zu Wave 1 nach kurzer Abstimmung
**Ziel**: Die oeffentliche Homepage zeigt vorerst keine Team-Inhalte und bekommt die Grundlage fuer die Sponsoren-Section.

### Task 16.5: Team-Section sichtbar ausblenden
**Priority**: P1
**Blocked by**: none
**Agent**: Agent E (frontend-eng)

**Akzeptanzkriterien**:
- [ ] `#team` ist auf der Homepage nicht mehr sichtbar.
- [ ] Desktop-Navigation enthaelt keinen sichtbaren Link zu `/#team`.
- [ ] Mobile-Navigation enthaelt keinen sichtbaren Link zu `/#team`.
- [ ] Footer enthaelt keinen sichtbaren Link zu `/#team`.
- [ ] Es bleiben keine sichtbaren Dead-Anchor-Links auf `/#team`.
- [ ] Kontakt-Section wird nach der Ausblendung konsistent nummeriert.
- [ ] CSS fuer spaetere Team-Reaktivierung darf bestehen bleiben, wird aber nicht als sichtbarer Inhalt genutzt.

**Kontext**:
- `web/templates/pages/home.html`
- `web/templates/partials/nav.html`
- `web/templates/partials/footer.html`
- `web/static/css/style.css`
- `docs/Content.md`

**Output**:
- Homepage ohne sichtbare Team-Section.
- Navigation und Footer ohne Team-Link.

### Task 16.6: Sponsoren-Assets vorbereiten
**Priority**: P1
**Blocked by**: none
**Agent**: Agent E (frontend-eng)

**Akzeptanzkriterien**:
- [ ] AXA-Logo wird aus `logo/axa-versicherung-haake-haake-ohg_full_1584974628.jpg` in einen webfaehigen Pfad unter `web/static/img/sponsors/` uebernommen.
- [ ] Browserpfad fuer das Logo ist eindeutig dokumentiert, z.B. `/static/img/sponsors/axa-versicherung-haake-haake-ohg.jpg`.
- [ ] `logo/axa-versicherung-haake-haake-ohg_full_1584974628.jpg:Zone.Identifier` wird nicht verwendet und nicht ausgeliefert.
- [ ] Platzhalter fuer weitere Sponsoren sind ohne externe Abhaengigkeiten verfuegbar.
- [ ] Asset-Namen sind stabil und sprechend.
- [ ] Bildgroesse/Dateigroesse wird geprueft; bei Bedarf wird eine optimierte Web-Kopie erstellt.

**Kontext**:
- `logo/axa-versicherung-haake-haake-ohg_full_1584974628.jpg`
- `web/static/img/`
- `web/templates/pages/home.html`

**Output**:
- Webfaehiges Sponsorenlogo.
- Struktur fuer weitere Sponsor-Assets.

### Task 16.7: Sponsoren-Section als letzte Section designen
**Priority**: P1
**Blocked by**: 16.6
**Agent**: Agent E (frontend-eng)

**Akzeptanzkriterien**:
- [ ] Neue Section mit `id="sponsoren"` steht als letzte Content-Section vor dem Footer.
- [ ] Section-Titel lautet "Sponsoren".
- [ ] Sponsorendarstellung ist logo-only: keine Beschreibungen und keine CTA-Texte pro Sponsor.
- [ ] AXA-Logo wird als reales Sponsorlogo angezeigt.
- [ ] Weitere Slots sind als klare Logo-Platzhalter dargestellt.
- [ ] Startlayout deckt 4-6 Sponsoren sauber ab.
- [ ] Layout bleibt stabil bei 1, 4, 6 und 8 Sponsor-Slots.
- [ ] Logos werden nicht verzerrt; `object-fit: contain` oder gleichwertige Loesung wird genutzt.
- [ ] Bilder sind lazy geladen, ausser eine spaetere Performance-Pruefung zeigt einen Grund dagegen.
- [ ] Mobile Ansicht hat keine ueberlappenden Logos oder Texte.
- [ ] Desktop Ansicht wirkt hochwertig und passt zur bestehenden dunklen visuellen Sprache.
- [ ] Keine externen Bildquellen oder CDN-Abhaengigkeiten.

**Kontext**:
- `web/templates/pages/home.html`
- `web/static/css/style.css`
- `web/static/img/sponsors/`

**Output**:
- Neue Sponsoren-Section.
- CSS fuer responsive Logo-Grid/Logo-Tiles.

**Wave-Exit-Gate**:
- [ ] Team-Inhalte und Team-Links sind nicht sichtbar.
- [ ] Sponsoren-Section ist letzte sichtbare Section vor dem Footer.
- [ ] AXA-Logo wird ausgeliefert.
- [ ] Platzhalter decken die erwartete Sponsorenzahl ab.

---
