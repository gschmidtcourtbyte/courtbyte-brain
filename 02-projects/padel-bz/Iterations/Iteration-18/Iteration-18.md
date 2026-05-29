# Iteration 18: Rechtstexte final hinterlegen und E-Mail-Templates ins Corporate Design bringen

## Ziel

Iteration 18 ersetzt die bisherigen Platzhalterseiten fuer Impressum und Datenschutz durch die gelieferten Inhalte aus `plan/impressum.md` und `plan/datenschutz.md`. Zusaetzlich werden die bestehenden E-Mail-Inhalte aus reinen Plain-Text-Strings in gepflegte Templates ueberfuehrt und visuell an das Corporate Design des courts padelclub angelehnt.

Die E-Mail-Inhalte bleiben bewusst leicht editierbar, weil der finale Text spaeter noch angepasst wird. Der Schwerpunkt dieser Iteration liegt deshalb auf sauberer Template-Struktur, Brand-Rahmen, Wiederverwendbarkeit, Tests und sicherem Versandverhalten.

## Ausgangslage

- `web/templates/pages/impressum.html` enthaelt noch Platzhalter wie `[Name des Betreibers]`, `[Telefonnummer]`, `[HRB-Nummer]`.
- `web/templates/pages/datenschutz.html` enthaelt eine kurze generische Datenschutzerklaerung mit Platzhalterdaten.
- Neue Quellartefakte liegen unter:
  - `plan/impressum.md`
  - `plan/datenschutz.md`
- Diese beiden Plan-Dateien sind aktuell Content-Quelle fuer die Rechtstexte. Sie duerfen nicht geloescht werden, solange der Kunde sie als Referenz nutzen will.
- `plan/datenschutz.md` nennt aktuell `Hosting (Strato)`, waehrend die Website technisch ueber Railway betrieben wird. Das ist ein fachlich/rechtlicher Pruefpunkt, aber nicht automatisch durch Entwickler umzuschreiben.
- Die korrekte Kontaktadresse ist `hello@courts-padelclub.de`. Aeltere oder falsch genannte Varianten wie `hallo@courts-padel.de`, `hallo@courts-padelclub.de` und historische `info@courts-bz.de` muessen in der Iteration bereinigt werden.
- `web/templates/emails/hansefit_confirmation.txt` existiert bereits als Plain-Text-Go-Template.
- Kontakt-Betreiber-Mail und Kontakt-Bestaetigungsmail werden im Service erzeugt; Inhalte sind derzeit teilweise direkt in Go-Strings abgebildet.
- Der SMTP-Versand erzeugt aktuell Plain-Text-Mails mit `Content-Type: text/plain; charset=UTF-8`.
- Corporate Design Tokens liegen in `web/static/css/style.css`:
  - `--bg: #475243`
  - `--bg-deep: #3a4437`
  - `--fg: #FAF2E8`
  - `--accent: #D8633D`
  - Fonts: Space Grotesk / Fraunces / JetBrains Mono
- Das Logo mit Slogan ist als SVG vorhanden:
  - `logo/logo_courts_transparent_mit slogan.svg`
  - Web-Asset: `web/static/img/logo_courts_transparent_mit_slogan.svg`

## Scope

- Inhalte aus `plan/impressum.md` in `web/templates/pages/impressum.html` ueberfuehren.
- Inhalte aus `plan/datenschutz.md` in `web/templates/pages/datenschutz.html` ueberfuehren.
- Rechtstextseiten semantisch, lesbar und im bestehenden Website-Layout darstellen.
- Bestehende Routen `/impressum` und `/datenschutz` unveraendert erhalten.
- Rechtliche Seiten bleiben weiterhin ohne Site-Passwort erreichbar.
- E-Mail-Templates in `web/templates/emails/` strukturiert erweitern:
  - Betreiber-Benachrichtigung bei neuer Kontaktanfrage
  - Bestaetigung an den Anfragenden
  - Hansefit-Bestaetigung
- HTML-E-Mail-Templates mit Corporate-Design-Rahmen erstellen.
- Plain-Text-Fallbacks beibehalten oder ergaenzen.
- Versand auf `multipart/alternative` erweitern, damit HTML und Plain Text gemeinsam versendet werden.
- Logo-/Brand-Strategie fuer E-Mails umsetzen: Logo oder robuste Fallback-Wordmark, mit klarer Client-Kompatibilitaet.
- Kanonische Kontaktadresse repo-weit auf `hello@courts-padelclub.de` umstellen.
- Tests fuer Rechtstext-Rendering, Template-Rendering und E-Mail-Empfaengerverhalten ergaenzen.
- Operations-/Content-Doku aktualisieren.

## Nicht im Scope

- Juristische Pruefung oder inhaltliche Korrektur der Rechtstexte.
- Finale Textredaktion der E-Mail-Inhalte.
- Domain-/DNS-Setup, Cloudflare-Setup oder Railway-Deployment.
- SMTP-Provider-Konfiguration in Railway.
- SPF/DKIM/DMARC-Einrichtung oder Deliverability-Optimierung ausserhalb der App.
- Vollstaendiger Newsletter-/Marketing-Mail-Baukasten.
- Tracking-Pixel, externe Analytics oder dynamische E-Mail-Kampagnen.
- PDF-Generierung oder Download-Versionen der Rechtstexte.

## Produktentscheidungen

- **Rechtstexte werden als geliefertes Material behandelt**: Entwickler ueberfuehren die Inhalte strukturiert in Templates, korrigieren aber keine Rechtsaussagen ohne fachliche Freigabe.
- **Abweichungen werden markiert, nicht still korrigiert**: `Strato` vs. `Railway` bleibt ein fachlicher Pruefpunkt; die Kontaktadresse ist dagegen entschieden und wird auf `hello@courts-padelclub.de` vereinheitlicht.
- **Runtime-Quelle bleibt das Web-Template**: `web/templates/pages/impressum.html` und `web/templates/pages/datenschutz.html` sind die ausgelieferten Seiten. Die Dateien unter `plan/` bleiben Content-Referenz.
- **E-Mails werden multipart**: HTML fuer bessere Darstellung, Plain Text fuer robuste Zustellung und Clients ohne HTML-Rendering.
- **Keine externen Webfonts in E-Mails**: E-Mail-Clients unterstuetzen Fonts unzuverlaessig. Templates nutzen systemnahe Fallbacks und visuelle Brand-Signale ueber Farben, Spacing und Logo/Wordmark.
- **Logo in E-Mail defensiv verwenden**: SVG-Unterstuetzung in Mailclients ist uneinheitlich. Wenn das SVG nicht verlaesslich angezeigt wird, wird ein PNG/Web-Fallback aus dem vorhandenen Logo-Asset vorbereitet und dokumentiert.
- **Keine CSS-Abhaengigkeit auf Website-Styles**: E-Mail-Templates verwenden inline- oder template-lokale Styles, keine Abhaengigkeit auf `web/static/css/style.css`.
- **Inhalte bleiben editierbar**: Alle Mailtexte liegen in Template-Dateien, nicht in Go-String-Bloecken.

## Critical Path

1. Rechtstext- und E-Mail-Template-Vertrag festlegen.
2. Rechtstextseiten aus `plan/impressum.md` und `plan/datenschutz.md` in Web-Templates ueberfuehren.
3. Kanonische Kontaktadresse `hello@courts-padelclub.de` repo-weit umstellen.
4. E-Mail-Designsystem und Asset-Strategie umsetzen.
5. E-Mail-Rendering im Service auf HTML + Plain Text erweitern.
6. Kontakt- und Hansefit-Mails an die neuen Templates anbinden.
7. Tests fuer Rendering, Empfaenger, Header-Sicherheit und Seitenzugriff ergaenzen.
8. Manuelle QA fuer Desktop/Mobile und Mail-Vorschau.
9. Doku und finaler Review.

## Agenten-Zuordnung

| Agent | Skillset | Verantwortung |
|-------|----------|----------------|
| PO Lead | product-own | Scope, Priorisierung, Wave-Freigabe, Akzeptanzkriterien, Blockertracking |
| Agent A | documentation-eng | Rechtstext-Quellen mappen, Content-Doku, Review-Hinweise fuer fachliche Pruefpunkte |
| Agent B | frontend-eng | Impressum-/Datenschutz-Templates, Legal-Layout, responsive Lesbarkeit |
| Agent C | frontend-eng | HTML-E-Mail-Design, Brand-Rahmen, Logo-/Fallback-Asset-Strategie |
| Agent D | service-eng | E-Mail-Rendering, multipart/alternative, Template-Anbindung im Service |
| Agent E | testing-eng | Unit-/Render-/Regressionstests, manuelle QA-Checkliste |
| Agent F | documentation-eng/service-eng | Repo-weite Kontaktadress-Kanonisierung und Doku-Abgleich |
| Agent S | security-eng | Datenschutz-/Header-/E-Mail-Sicherheitsreview, externe Asset-Risiken |
| Agent R | review-eng | Finaler risikoorientierter Review und Merge-Readiness |

---

## Wave 1 - Foundation: Content- und E-Mail-Vertrag
**Agenten**: PO Lead, Agent A, Agent C, Agent D, Agent S
**Kann parallel ausgefuehrt werden**: ja, Analyse-Tasks sind unabhaengig
**Ziel**: Bevor Templates gebaut werden, sind Quellen, Zielstruktur, offene fachliche Pruefpunkte und technische E-Mail-Konventionen eindeutig.

### Task 18.1: Rechtstext-Quellen mappen
**Priority**: P0
**Blocked by**: none
**Agent**: Agent A (documentation-eng)

**Akzeptanzkriterien**:
- [ ] `plan/impressum.md` ist als Quelle fuer `/impressum` dokumentiert.
- [ ] `plan/datenschutz.md` ist als Quelle fuer `/datenschutz` dokumentiert.
- [ ] Alle Hauptabschnitte der Quelldateien sind den Ziel-HTML-Strukturen zugeordnet.
- [ ] Alte Platzhalter in `web/templates/pages/impressum.html` und `web/templates/pages/datenschutz.html` sind als zu ersetzender Content identifiziert.
- [ ] Fachliche Pruefpunkte sind explizit notiert: `Hosting (Strato)` vs. Railway, Registerdaten "in Eintragung".
- [ ] Es ist entschieden: Inhalte werden ohne juristische Umformulierung uebernommen, nur HTML-semantisch strukturiert und escaped.

**Kontext**:
- `plan/impressum.md`
- `plan/datenschutz.md`
- `web/templates/pages/impressum.html`
- `web/templates/pages/datenschutz.html`

**Output**:
- Mapping-Notiz im PR/Umsetzungsbericht oder als kurzer Abschnitt in `docs/Content.md`.
- Liste offener fachlicher Pruefpunkte fuer den Kunden.

### Task 18.2: E-Mail-Template-Vertrag definieren
**Priority**: P0
**Blocked by**: none
**Agent**: Agent C (frontend-eng), Agent D (service-eng), Agent S (security-eng)

**Akzeptanzkriterien**:
- [ ] Zieltemplates sind festgelegt:
  - `web/templates/emails/contact_notification.html`
  - `web/templates/emails/contact_notification.txt`
  - `web/templates/emails/contact_confirmation.html`
  - `web/templates/emails/contact_confirmation.txt`
  - `web/templates/emails/hansefit_confirmation.html`
  - `web/templates/emails/hansefit_confirmation.txt`
- [ ] Gemeinsame Template-Daten sind definiert: Empfaenger, Betreff, LogoURL/BrandText, AppURL, Name, Kontaktfelder, Hansefit-Felder.
- [ ] Service-Kontrakt fuer multipart/alternative ist festgelegt: Plain Text zuerst, HTML danach.
- [ ] E-Mail-Header bleiben sanitisiert; dynamische Betreffzeilen duerfen keine Header-Injection ermoeglichen.
- [ ] Bei `SMTP_HOST == ""` bleibt Versand weiterhin ein No-op.
- [ ] Logo-Strategie ist entschieden:
  - bevorzugt robustes PNG/Web-Asset fuer Mailclients, abgeleitet aus dem vorhandenen SVG; oder
  - SVG nur mit klar dokumentiertem Fallback auf Text-Wordmark.
- [ ] HTML-E-Mails nutzen inline-kompatibles CSS, Tabellen/Container mit max. Breite, keine externen Fonts, kein JavaScript.
- [ ] Plain-Text-Fallbacks bleiben inhaltlich vollstaendig und unabhaengig vom HTML.

**Kontext**:
- `internal/service/contact_service.go`
- `web/templates/emails/hansefit_confirmation.txt`
- `web/static/img/logo_courts_transparent_mit_slogan.svg`
- `logo/logo_courts_transparent_mit slogan.svg`
- `internal/config/config.go` fuer `AppURL`, `SMTP_FROM`, `SMTP_TO`

**Output**:
- Technischer Mini-Kontrakt fuer die folgenden Implementierungs-Tasks.

**Wave-1-Exit-Gate**:
- [ ] Rechtstext-Quellen und Zielseiten sind eindeutig.
- [ ] E-Mail-Template-Dateien, Datenmodell und MIME-Strategie sind eindeutig.
- [ ] Offene fachliche Pruefpunkte sind dokumentiert und blockieren die technische Umsetzung nicht.

---

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

## Wave 3 - Kanonische Kontaktadresse repo-weit umstellen
**Agenten**: Agent F, Agent A, Agent D, Agent E
**Kann parallel ausgefuehrt werden**: teilweise; Suche/Inventar kann parallel laufen, Schreibzugriffe sollten sequenziell integriert werden
**Ziel**: Alle ausgelieferten Kontaktadressen und SMTP-Defaults verwenden `hello@courts-padelclub.de`.

### Task 18.7: Falsche und historische Kontaktadressen inventarisieren
**Priority**: P0
**Blocked by**: none
**Agent**: Agent F (documentation-eng/service-eng), Agent E (testing-eng)

**Akzeptanzkriterien**:
- [ ] Repo-Suche erfasst mindestens diese Varianten:
  - `hallo@courts-padel.de`
  - `hallo@courts-padelclub.de`
  - `info@courts-bz.de`
  - `noreply@courts-bz.de`
- [ ] Treffer werden in Kategorien getrennt: Runtime-Code, Web-Templates, E-Mail-Templates, Doku, historische Iterationsplaene.
- [ ] Historische Iterationsplaene duerfen als Historie unveraendert bleiben, muessen aber nicht als Runtime-Quelle gelten.
- [ ] Runtime- und sichtbare Content-Treffer sind als zu aendern markiert.
- [ ] Zieladresse ist eindeutig: `hello@courts-padelclub.de`.

**Kontext**:
- `internal/config/config.go`
- `web/templates/pages/`
- `web/templates/partials/`
- `web/templates/emails/`
- `docs/Content.md`
- `docs/Operations.md`
- `plan/impressum.md`
- `plan/datenschutz.md`

**Output**:
- Kurze Trefferliste oder PR-Notiz mit geaenderten Pfaden.

### Task 18.8: Runtime- und Content-Adressen auf `hello@courts-padelclub.de` umstellen
**Priority**: P0
**Blocked by**: 18.7
**Agent**: Agent F (documentation-eng/service-eng), Agent D (service-eng), Agent B (frontend-eng)

**Akzeptanzkriterien**:
- [ ] `SMTP_FROM` Default ist `hello@courts-padelclub.de`.
- [ ] `SMTP_TO` Default ist `hello@courts-padelclub.de`.
- [ ] Betreiber-Mail-Tests erwarten `hello@courts-padelclub.de`.
- [ ] Sichtbare Website-Kontaktadressen und `mailto:` Links nutzen `hello@courts-padelclub.de`.
- [ ] Impressum und Datenschutz nutzen `hello@courts-padelclub.de`.
- [ ] Neue E-Mail-Templates nutzen `hello@courts-padelclub.de` als sichtbare Kontaktadresse, sofern eine Kontaktadresse enthalten ist.
- [ ] Doku fuer SMTP- und Contentpflege nennt `hello@courts-padelclub.de`.
- [ ] Keine Runtime-/Template-Treffer fuer `hallo@courts-padel.de`, `hallo@courts-padelclub.de`, `info@courts-bz.de` oder `noreply@courts-bz.de` verbleiben.
- [ ] Historische Iterationsplaene duerfen alte Adressen enthalten, wenn sie klar historische Referenz bleiben.

**Kontext**:
- `internal/config/config.go`
- `internal/config/config_test.go`
- `internal/service/contact_service_test.go`
- `web/templates/pages/home.html`
- `web/templates/pages/contact.html`
- `web/templates/pages/impressum.html`
- `web/templates/pages/datenschutz.html`
- `web/templates/pages/coming-soon.html`
- `web/templates/emails/*.html`
- `web/templates/emails/*.txt`
- `docs/Content.md`
- `docs/Operations.md`

**Output**:
- Kanonische Kontaktadresse in Runtime, Templates und Doku.

**Wave-3-Exit-Gate**:
- [ ] Zieladresse `hello@courts-padelclub.de` ist in Runtime-Code, sichtbaren Templates und aktueller Doku konsistent.
- [ ] Alte Adressvarianten existieren nur noch in historischen Planartefakten oder gar nicht.
- [ ] Tests fuer Config-/SMTP-Defaults sind vorbereitet oder aktualisiert.

---

## Wave 4 - Service-Integration fuer E-Mail-Templates
**Agenten**: Agent D, Agent E, Agent S
**Kann parallel ausgefuehrt werden**: teilweise; Tests koennen parallel vorbereitet werden, Service-Integration bleibt sequenziell
**Ziel**: Der bestehende E-Mail-Versand nutzt die neuen Templates robust und ohne Regressionen.

### Task 18.9: E-Mail-Renderer fuer HTML + Plain Text implementieren
**Priority**: P0
**Blocked by**: 18.2, 18.6, 18.8
**Agent**: Agent D (service-eng)

**Akzeptanzkriterien**:
- [ ] Es gibt eine zentrale Render-Funktion fuer E-Mail-Templates.
- [ ] HTML- und Text-Templates werden aus `web/templates/emails/` geladen.
- [ ] Rendering-Fehler enthalten Template-Namen im Fehlerkontext.
- [ ] Der MIME-Body wird als `multipart/alternative` erzeugt.
- [ ] Plain-Text-Part kommt vor HTML-Part.
- [ ] Boundary wird sicher erzeugt und kollidiert nicht mit Content.
- [ ] Header-Sanitizing fuer `From`, `To`, `Subject` bleibt erhalten.
- [ ] `Content-Type` enthaelt `charset=UTF-8`.
- [ ] Tests koennen den SMTP-Sendepfad weiterhin mocken.

**Kontext**:
- `internal/service/contact_service.go`
- `internal/service/contact_service_test.go`
- `web/templates/emails/`

**Output**:
- Zentraler E-Mail-Renderer und MIME-Builder im Service-Layer oder in einer kleinen service-internen Hilfsdatei.

### Task 18.10: Kontakt- und Hansefit-Mails auf Templates umstellen
**Priority**: P0
**Blocked by**: 18.9
**Agent**: Agent D (service-eng)

**Akzeptanzkriterien**:
- [ ] Betreiber-Benachrichtigung nutzt `contact_notification.html` + `.txt`.
- [ ] Anfragenden-Bestaetigung nutzt `contact_confirmation.html` + `.txt`.
- [ ] Hansefit-Bestaetigung nutzt `hansefit_confirmation.html` + `.txt`.
- [ ] Keine laengeren Mail-Bodies bleiben als Go-String-Bloecke im Service.
- [ ] Empfaengerlogik bleibt unveraendert:
  - Betreiber-Mail an `SMTP_TO`, Fallback `SMTP_FROM`.
  - Anfragenden-Bestaetigung an `submission.Email`.
  - Hansefit-Bestaetigung an Playtomic-E-Mail, Fallback Kontakt-E-Mail.
- [ ] Betreffzeilen bleiben im Code oder zentral konfigurierbar, aber sanitisiert.
- [ ] `AppURL` wird fuer absolute Logo-/Asset-URLs genutzt.
- [ ] Wenn `AppURL` lokal ist, darf E-Mail-Rendering nicht fehlschlagen; LogoURL kann dann leer oder lokal sein.
- [ ] Fehler beim asynchronen Kontakt-Mailversand blockieren das Formular weiterhin nicht.
- [ ] Fehler beim Hansefit-Bestaetigungsversand verhindern weiterhin das Markieren als bearbeitet, wie bisher.

**Kontext**:
- `internal/service/contact_service.go`
- `internal/model/contact.go`
- `internal/config/config.go`

**Output**:
- Template-basierter Versand fuer alle Kontakt-/Hansefit-Mails.

### Task 18.11: Security- und Datenschutz-Haertung im Mailpfad
**Priority**: P1
**Blocked by**: 18.9, 18.10
**Agent**: Agent S (security-eng), Agent D (service-eng)

**Akzeptanzkriterien**:
- [ ] Header-Injection-Test bleibt vorhanden oder wird erweitert.
- [ ] HTML-Templates escapen dynamische Werte ueber Go `html/template`.
- [ ] Plain-Text-Templates nutzen `text/template`.
- [ ] Keine untrusted Eingabe wird als `template.HTML` markiert.
- [ ] Betreiber-Mail darf Formular-Nachricht enthalten, aber HTML-escaped.
- [ ] Anfragenden-Mail enthaelt keine interne Submission-ID und keine IP-Adresse.
- [ ] Keine Tracking-Pixel oder externen Drittanbieter-Assets.
- [ ] Logo-Asset kommt ausschliesslich von eigener Domain/AppURL.

**Kontext**:
- `internal/service/contact_service.go`
- `web/templates/emails/*.html`
- `web/templates/emails/*.txt`

**Output**:
- Abgesicherter Mail-Rendering-Pfad.

**Wave-4-Exit-Gate**:
- [ ] Alle Mailtypen rendern HTML und Plain Text.
- [ ] SMTP-Ausgabe ist multipart/alternative.
- [ ] Empfaengerlogik ist unveraendert und getestet.
- [ ] Security-relevante Template-/Header-Risiken sind adressiert.

---

## Wave 5 - Tests und manuelle QA
**Agenten**: Agent E, Agent B, Agent C
**Kann parallel ausgefuehrt werden**: ja, automatisierte Tests und visuelle QA koennen parallel laufen
**Ziel**: Die Iteration ist regressionssicher und visuell pruefbar.

### Task 18.12: Tests fuer Rechtstextseiten
**Priority**: P0
**Blocked by**: 18.3, 18.4
**Agent**: Agent E (testing-eng)

**Akzeptanzkriterien**:
- [ ] Render-Test fuer `impressum.html` bleibt gruen.
- [ ] Render-Test fuer `datenschutz.html` bleibt gruen.
- [ ] Tests pruefen, dass zentrale neue Inhalte sichtbar sind:
  - `courts padelclub GmbH`
  - `Christian Haake`
  - `Henrik Haake`
  - `Landesbeauftragte fuer den Datenschutz Niedersachsen`
- [ ] Tests pruefen, dass alte Platzhalter wie `[Name des Betreibers]` nicht mehr gerendert werden.
- [ ] Route-Exemptions fuer `/impressum` und `/datenschutz` bleiben gruen.

**Kontext**:
- `internal/handler/render_test.go`
- `internal/handler/site_auth_test.go`
- `web/templates/pages/impressum.html`
- `web/templates/pages/datenschutz.html`

**Output**:
- Aktualisierte Render-/Auth-Tests.

### Task 18.13: Tests fuer E-Mail-Rendering und Versandvertrag
**Priority**: P0
**Blocked by**: 18.9, 18.10, 18.11
**Agent**: Agent E (testing-eng)

**Akzeptanzkriterien**:
- [ ] Alle HTML-E-Mail-Templates lassen sich parsen und rendern.
- [ ] Alle Text-E-Mail-Templates lassen sich parsen und rendern.
- [ ] Betreiber-Mail-Test prueft Empfaenger `SMTP_TO`.
- [ ] Config-Tests pruefen `SMTP_FROM` und `SMTP_TO` Default `hello@courts-padelclub.de`.
- [ ] Anfragenden-Bestaetigungs-Test prueft Empfaenger `submission.Email`.
- [ ] Hansefit-Test prueft Empfaenger Playtomic-E-Mail und Fallback Kontakt-E-Mail.
- [ ] MIME-Test prueft `multipart/alternative`.
- [ ] MIME-Test prueft, dass `text/plain` vor `text/html` kommt.
- [ ] Header-Sanitizing-Test deckt Betreff mit CR/LF ab.
- [ ] HTML-Escape-Test prueft, dass eine Formularnachricht mit `<script>` nicht als HTML ausgefuehrt wird.
- [ ] `go test ./...` ist gruen.

**Kontext**:
- `internal/service/contact_service_test.go`
- `web/templates/emails/`

**Output**:
- Erweiterte Service-Tests fuer E-Mail-Rendering.

### Task 18.14: Manuelle QA fuer Seiten und E-Mails
**Priority**: P1
**Blocked by**: 18.3, 18.4, 18.10
**Agent**: Agent B (frontend-eng), Agent C (frontend-eng), Agent E (testing-eng)

**Akzeptanzkriterien**:
- [ ] `/impressum` ist auf Desktop und Mobile lesbar.
- [ ] `/datenschutz` ist auf Desktop und Mobile lesbar.
- [ ] Rechtstextseiten haben keine sichtbaren Template-Artefakte oder Platzhalter.
- [ ] Footer-Links fuehren weiterhin auf `/impressum` und `/datenschutz`.
- [ ] HTML-Mail-Vorschau zeigt Logo/Wordmark, Header, Content und Footer korrekt.
- [ ] Plain-Text-Mail ist vollstaendig lesbar.
- [ ] Bilder-blockiert-Szenario bleibt verstaendlich durch Alt-Text/Wordmark.
- [ ] Lange Nachrichten im Betreiber-Mailtemplate brechen Layout nicht.

**Kontext**:
- Lokaler Dev-Server oder Template-Render-Tests.
- SMTP-Capture/Mailhog optional, falls lokal vorhanden.

**Output**:
- Kurze QA-Notiz mit getesteten Viewports und Mailtypen.

**Wave-5-Exit-Gate**:
- [ ] Automatisierte Tests sind gruen.
- [ ] Desktop/Mobile Legal-QA ist bestanden.
- [ ] E-Mail-Preview/Rendering-QA ist bestanden.

---

## Wave 6 - Doku, Review und Release-Readiness
**Agenten**: Agent A, Agent S, Agent R, PO Lead
**Kann parallel ausgefuehrt werden**: Doku und Security-Review parallel, finaler Review danach
**Ziel**: Die Iteration ist nachvollziehbar dokumentiert und bereit fuer Merge/Release.

### Task 18.15: Content- und Operations-Doku aktualisieren
**Priority**: P1
**Blocked by**: 18.3, 18.4, 18.8, 18.10
**Agent**: Agent A (documentation-eng)

**Akzeptanzkriterien**:
- [ ] `docs/Content.md` beschreibt, dass Impressum und Datenschutz aus den gelieferten Rechtstexten hinterlegt wurden.
- [ ] `docs/Content.md` listet die neuen E-Mail-Templates und ihre Platzhalter.
- [ ] `docs/Operations.md` beschreibt relevante SMTP-/AppURL-Abhaengigkeiten fuer Logo-URLs in E-Mails.
- [ ] Offene fachliche Pruefpunkte sind klar dokumentiert:
  - Hosting-Aussage pruefen.
  - alte Kontaktadressvarianten sind bereinigt oder als historische Referenz markiert.
  - Registerdaten ergaenzen, sobald vorhanden.

**Kontext**:
- `docs/Content.md`
- `docs/Operations.md`
- `plan/impressum.md`
- `plan/datenschutz.md`

**Output**:
- Aktualisierte Projekt-Doku.

### Task 18.16: Finaler Security-/Privacy-Review
**Priority**: P1
**Blocked by**: 18.11, 18.13
**Agent**: Agent S (security-eng), Agent R (review-eng)

**Akzeptanzkriterien**:
- [ ] Rechtstextseiten enthalten keine versehentlichen Secrets oder internen Umgebungsdetails.
- [ ] E-Mail-Templates enthalten keine IP-Adresse in Anfragenden-Mails.
- [ ] HTML-E-Mail-Dynamik ist escaped.
- [ ] Keine externen Drittanbieter-Assets werden in E-Mails geladen.
- [ ] AppURL/LogoURL kann in Produktion auf `https://courts-padelclub.de` zeigen.
- [ ] Known legal-content risks sind als fachliche Punkte dokumentiert, nicht als technische Blocker verschleiert.

**Kontext**:
- `web/templates/pages/impressum.html`
- `web/templates/pages/datenschutz.html`
- `web/templates/emails/*.html`
- `internal/service/contact_service.go`

**Output**:
- Review-Kommentar oder Abschlussnotiz mit Findings.

### Task 18.17: Merge-Readiness und Release-Check
**Priority**: P0
**Blocked by**: 18.12, 18.13, 18.14, 18.15, 18.16
**Agent**: PO Lead, Agent R (review-eng)

**Akzeptanzkriterien**:
- [ ] Alle P0-Tasks sind erledigt.
- [ ] `go test ./...` ist gruen.
- [ ] `git diff --check` ist gruen.
- [ ] Keine alten Legal-Platzhalter bleiben in den ausgelieferten Templates.
- [ ] Keine laengeren E-Mail-Body-Texte bleiben als Go-String-Bloecke im Service.
- [ ] Rechtstext-Pruefpunkte sind fuer den Kunden sichtbar.
- [ ] Rest-Risiken sind dokumentiert.
- [ ] Deployment-Hinweise fuer Railway Env enthalten mindestens `APP_URL`, `SMTP_HOST`, `SMTP_PORT`, `SMTP_USER`, `SMTP_PASSWORD`, `SMTP_FROM`, `SMTP_TO`.

**Kontext**:
- Gesamter Iterationsdiff.

**Output**:
- Merge-/Release-Readiness-Status.

---

## Abhaengigkeiten und Parallelisierung

| Wave | Parallel moeglich | Blockiert durch | Bemerkung |
|------|-------------------|-----------------|-----------|
| Wave 1 | ja | none | Analyse und technische Vertrage koennen parallel laufen. |
| Wave 2 | ja | Wave 1 | Legal-Seiten und E-Mail-Design beruehren getrennte Dateien. |
| Wave 3 | teilweise | Wave 1 | Inventar parallel, Integration sequenziell. |
| Wave 4 | teilweise | 18.2, 18.6, 18.8 | Service-Integration muss nach Template-Vertrag und Adresskanonisierung passieren. |
| Wave 5 | ja | Wave 2/3/4 | Tests und manuelle QA koennen parallel vorbereitet werden. |
| Wave 6 | teilweise | Wave 5 | Finaler Review erst nach Tests und QA. |

## Risiken und offene Entscheidungen

| Risiko | Impact | Umgang |
|--------|--------|--------|
| Datenschutztext nennt Strato, Hosting laeuft technisch ueber Railway | Hoch fuer Legal-Korrektheit | Nicht still korrigieren; als fachlichen Pruefpunkt dokumentieren und vor Release vom Kunden bestaetigen lassen. |
| Alte Kontaktadressvarianten bleiben in Runtime-Templates | Mittel | Wave 3 fuehrt ein explizites Inventar und eine Runtime-Suche auf `hallo@courts-padel.de`, `hallo@courts-padelclub.de`, `info@courts-bz.de` und `noreply@courts-bz.de` durch. |
| SVG-Logo wird in Mailclients nicht verlaesslich dargestellt | Mittel | PNG-Fallback oder Text-Wordmark nutzen; Bilder-blockiert-Szenario testen. |
| HTML-E-Mails koennen in Clients unterschiedlich aussehen | Mittel | Einfaches table-/containerbasiertes Layout, inline Styles, Plain-Text-Fallback. |
| Dynamische Formulartexte in HTML-Mail | Mittel | `html/template` nutzen, kein `template.HTML` fuer User-Input. |
| `APP_URL` falsch gesetzt | Niedrig bis Mittel | Mail-Logo darf nicht kritisch sein; Doku und Env-Check ergaenzen. |

## Definition of Done

- [ ] `web/templates/pages/impressum.html` enthaelt den gelieferten Impressum-Inhalt.
- [ ] `web/templates/pages/datenschutz.html` enthaelt den gelieferten Datenschutz-Inhalt.
- [ ] `/impressum` und `/datenschutz` rendern ohne Passwort und ohne Platzhalter.
- [ ] `hello@courts-padelclub.de` ist die kanonische Kontaktadresse in Runtime-Code, sichtbaren Templates, E-Mail-Templates und aktueller Doku.
- [ ] Falsche Kontaktadressvarianten wie `hallo@courts-padel.de` und `hallo@courts-padelclub.de` verbleiben nicht in Runtime-Quellen.
- [ ] Alle Kontakt-/Hansefit-Mails nutzen Templates unter `web/templates/emails/`.
- [ ] HTML- und Plain-Text-Mail werden als `multipart/alternative` versendet.
- [ ] Corporate-Design-Signale sind in E-Mails sichtbar: Logo/Wordmark, Farben, klare Typohierarchie.
- [ ] Plain-Text-Fallback ist vollstaendig.
- [ ] Header- und HTML-Injection-Risiken sind getestet.
- [ ] `go test ./...` ist gruen.
- [ ] `git diff --check` ist gruen.
- [ ] Content-/Operations-Doku ist aktualisiert.
- [ ] Fachliche Rechtstext-Pruefpunkte sind an den Kunden zur finalen Freigabe markiert.
