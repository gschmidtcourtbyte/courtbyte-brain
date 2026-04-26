# Wave 3: Kanonische Kontaktadresse repo-weit umstellen

**Status:** Done (Legacy-Migration)
**Skill:** service-eng, frontend-eng, testing-eng, documentation-eng
**Quelle:** `plan/iterations/iteration-18.md`

## Legacy-Inhalt

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
