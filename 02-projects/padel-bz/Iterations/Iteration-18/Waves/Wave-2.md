# Wave 2: Rechtstexte und E-Mail-Design vorbereiten

**Status:** Done (Legacy-Migration)
**Skill:** frontend-eng, documentation-eng
**Quelle:** `plan/iterations/iteration-18.md`

## Legacy-Inhalt

## Wave 2 - Rechtstexte und E-Mail-Design vorbereiten
**Agenten**: Agent A, Agent B, Agent C
**Kann parallel ausgefuehrt werden**: ja, Legal-Seiten und E-Mail-Design beruehren getrennte Dateien
**Ziel**: Die sichtbaren Rechtstextseiten sind fachlich befuellt; die E-Mail-Templates existieren als bearbeitbare Brand-Artefakte.

### Task 18.3: Impressum-Template aus `plan/impressum.md` hinterlegen
**Priority**: P0
**Blocked by**: 18.1
**Agent**: Agent B (frontend-eng), Agent A (documentation-eng)

**Akzeptanzkriterien**:
- [ ] `web/templates/pages/impressum.html` enthaelt den Inhalt aus `plan/impressum.md`.
- [ ] Alte Platzhalter sind entfernt.
- [ ] Struktur ist semantisch: sinnvolle `h2`/`h3`, Abschnitte, Kontaktblock, Link fuer OS-Plattform.
- [ ] Sonderzeichen sind HTML-sicher abgebildet.
- [ ] Telefonnummer und E-Mail sind bei Bedarf als `tel:` und `mailto:` Links nutzbar.
- [ ] Route `/impressum` bleibt unveraendert und ohne Site-Passwort erreichbar.
- [ ] Layout nutzt bestehende `.legal-content`-Klasse oder minimal ergaenzte Legal-Styles.
- [ ] Mobile Darstellung bleibt gut lesbar, keine ueberbreiten Zeilen oder Tabellen.
- [ ] Inhaltliche Marker wie "Registernummer: in Eintragung - wird ergaenzt" bleiben sichtbar, bis der Kunde sie final aendert.

**Kontext**:
- `plan/impressum.md`
- `web/templates/pages/impressum.html`
- `internal/handler/public.go`
- `internal/handler/site_auth.go`

**Output**:
- Aktualisiertes `web/templates/pages/impressum.html`.

### Task 18.4: Datenschutz-Template aus `plan/datenschutz.md` hinterlegen
**Priority**: P0
**Blocked by**: 18.1
**Agent**: Agent B (frontend-eng), Agent A (documentation-eng)

**Akzeptanzkriterien**:
- [ ] `web/templates/pages/datenschutz.html` enthaelt den Inhalt aus `plan/datenschutz.md`.
- [ ] Alte generische Datenschutz-Platzhalter sind entfernt.
- [ ] Inhaltsverzeichnis oder Abschnittsstruktur ist nutzerfreundlich umgesetzt.
- [ ] Abschnitte 01 bis 13 sind vollstaendig sichtbar.
- [ ] Listen zu Rechtsgrundlagen, Rechten und Logfile-Daten sind semantisch als Listen umgesetzt.
- [ ] Externe Links wie Playtomic, Google Fonts, Aufsichtsbehoerde und ODR werden als Links mit `rel="noopener"` abgebildet, wenn sie auf neue Tabs zielen.
- [ ] Route `/datenschutz` bleibt unveraendert und ohne Site-Passwort erreichbar.
- [ ] Der Strato/Railway-Pruefpunkt wird nicht still korrigiert, sondern im Review dokumentiert.
- [ ] Mobile Darstellung bleibt gut lesbar.

**Kontext**:
- `plan/datenschutz.md`
- `web/templates/pages/datenschutz.html`
- `internal/handler/public.go`
- `internal/handler/site_auth.go`

**Output**:
- Aktualisiertes `web/templates/pages/datenschutz.html`.

### Task 18.5: E-Mail-Brand-Layout und Asset-Fallback vorbereiten
**Priority**: P0
**Blocked by**: 18.2
**Agent**: Agent C (frontend-eng)

**Akzeptanzkriterien**:
- [ ] Ein wiederverwendbares visuelles Muster fuer HTML-E-Mails ist festgelegt: Header, Content-Bereich, CTA/Info-Block, Footer.
- [ ] Farben orientieren sich am Website-Corporate-Design, bleiben aber auf hellem oder dunklem Mail-Hintergrund gut lesbar.
- [ ] Keine Website-CSS-Datei wird in E-Mails referenziert.
- [ ] Keine Google Fonts werden in E-Mails vorausgesetzt.
- [ ] Logo/Wordmark funktioniert auch, wenn Bilder blockiert sind.
- [ ] Falls ein PNG-Fallback benoetigt wird, wird ein statisches Asset unter `web/static/img/` vorbereitet und die SVG-Quelle dokumentiert.
- [ ] E-Mail-Breite ist auf ca. 600px begrenzt und mobil fluessig.
- [ ] Alle dynamischen Felder sind als Go-Template-Platzhalter geplant.

**Kontext**:
- `web/static/css/style.css`
- `web/static/img/logo_courts_transparent_mit_slogan.svg`
- `logo/logo_courts_transparent_mit slogan.svg`
- Bestehende Brand-Verwendung in `web/templates/partials/nav.html`

**Output**:
- Brand-Layout-Entscheidung und ggf. neues statisches Mail-Logo-Asset.

### Task 18.6: HTML- und Text-E-Mail-Templates erstellen
**Priority**: P0
**Blocked by**: 18.2, 18.5
**Agent**: Agent C (frontend-eng), Agent A (documentation-eng)

**Akzeptanzkriterien**:
- [ ] `contact_notification.html` enthaelt die Betreiber-Mail im Brand-Layout.
- [ ] `contact_notification.txt` enthaelt denselben Inhalt als Plain Text.
- [ ] `contact_confirmation.html` enthaelt die Bestaetigung an den Anfragenden im Brand-Layout.
- [ ] `contact_confirmation.txt` enthaelt denselben Inhalt als Plain Text.
- [ ] `hansefit_confirmation.html` enthaelt die Hansefit-Bestaetigung im Brand-Layout.
- [ ] `hansefit_confirmation.txt` bleibt als Plain-Text-Fallback erhalten und wird ggf. an neue Platzhalter angepasst.
- [ ] Inhalte sind bewusst kurz und spaeter leicht editierbar.
- [ ] Betreiber-Mail zeigt alle relevanten Formularfelder: Name, Kontakt-E-Mail, Playtomic-E-Mail, Hansefit-ID, Telefon, Hansefit-Registrierung, Nachricht, Eingangszeit, ID.
- [ ] Anfragenden-Bestaetigung enthaelt keine internen IDs und keine Admin-/Operator-Details.
- [ ] Hansefit-Bestaetigung enthaelt Playtomic-E-Mail und Hansefit-ID wie bisher.

**Kontext**:
- `web/templates/emails/`
- `internal/service/contact_service.go`
- `internal/model/contact.go`

**Output**:
- Neue und aktualisierte Template-Dateien unter `web/templates/emails/`.

**Wave-2-Exit-Gate**:
- [ ] Impressum und Datenschutz sind als Web-Templates umgesetzt.
- [ ] E-Mail-Templates existieren als HTML + Plain Text.
- [ ] Brand-/Logo-Strategie ist umgesetzt oder sauber dokumentiert.
- [ ] Noch keine Service-Anbindung ist erforderlich, aber Templates sind parsebar.

---
