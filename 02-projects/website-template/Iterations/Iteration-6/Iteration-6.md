# Iteration 6 — Railway-Setup + S3-Anbindung produktiv schalten

**Status:** Planned
**Repo:** /home/andersen/git/website-template
**Datum:** 2026-05-14
**Bearbeitung:** Codex MCP Server

## Ziel

Die in Iteration 5 fertiggestellte Karriere-Backend-Funktionalität betriebsfertig
auf Railway bringen. Konkret:

1. Backend- und Frontend-Service in Railway mit korrekten Env-Variablen
   konfigurieren, sodass `forbidden_origin` und `service_unavailable` Fehler
   verschwinden.
2. S3-kompatibles Object Storage (vorzugsweise Cloudflare R2) anbinden, damit
   Bewerbungsanhänge produktiv hochgeladen, abgerufen und nach Retention
   gelöscht werden.
3. SMTP-Provider konfigurieren, damit der Rejection-Mail-Workflow tatsächlich
   Mails verschickt.
4. Staging-Smokes aus Wave 6 der Iteration 5 endlich gegen die laufende
   Railway-Umgebung ausführen und dokumentieren.

## Kontext

- Iteration 5 hat Code für Upload, Download, Retention, Rejection-Mail und
  Job-CRUD vollständig implementiert. Lokale Tests sind grün. Was offen blieb,
  ist die operative Inbetriebnahme.
- Iteration 5 Bugfix `forbidden_origin`: zeigt klar, dass die `FRONTEND_ORIGIN`
  Konfiguration in Railway aktuell nicht zur Browser-URL passt.
- `FRONTEND_ORIGIN` akzeptiert seit 2026-05-14 eine kommaseparierte Liste —
  Multi-Origin (z. B. Custom-Domain + `.up.railway.app`-Domain) ist möglich.
- Railway bietet **kein** natives S3-Storage-Plugin. Es braucht einen externen
  S3-kompatiblen Anbieter. Empfehlung: Cloudflare R2 (S3-API, keine Egress-
  Kosten, einfache Token-Verwaltung).
- SMTP-Provider liegt nicht im Master-Repo, sondern wird pro Kunden-Clone
  konfiguriert. Für dieses Master-Template wählen wir einen Test-Provider
  (siehe Wave 3).

## Scope

**In:**

- Railway-Env-Konfiguration für Backend- und Frontend-Service.
- Anlegen + Anbinden eines S3-kompatiblen Buckets (Cloudflare R2 default).
- SMTP-Konfiguration mit Test-Provider (z. B. Mailtrap Sandbox oder Postmark
  Test-Server).
- Staging-Smokes gemäß Iteration-5 Wave-6 Playbook ausführen und Ergebnisse
  in dieser Iteration dokumentieren.
- README/Runbook-Update mit „so deployst du das Template" inklusive R2-
  und SMTP-Schritte.

**Out:**

- Custom-Domain-Setup (DNS), CDN-Konfiguration, TLS-Custom-Zertifikate.
- Migration bestehender Bewerbungsdaten zwischen Buckets.
- Vollautomatisches Backup der Datenbank.
- Erweiterte Mail-Templates oder mehrsprachige Mails.
- Echte Malware-Scanning-Pipeline auf hochgeladenen Dateien.
- Galerie-/Seiten-Upload (Iteration 7+).

## Entscheidungen

- **S3-Provider: Cloudflare R2** als Default. Begründung: kostenlose Egress,
  S3-API-kompatibel, gutes Web-Dashboard, Token-basierte Auth. Falls der
  Kunde lieber AWS/Backblaze nutzen will, sind die Env-Variablen identisch
  bis auf `S3_ENDPOINT`, `S3_REGION`, `S3_USE_PATH_STYLE`.
- **Bucket-Naming:** `website-template-attachments-<env>` (z. B.
  `website-template-attachments-prod`). Pro Kunden-Clone wird das via
  Suffix differenziert.
- **Path-Style:** Bei R2 `S3_USE_PATH_STYLE=true`, bei AWS `false`.
- **SMTP für Master-Template:** Mailtrap Sandbox als Standard für die
  Demoumgebung — sichert keine echten Mails versendet werden. Kunden-Clones
  bekommen den echten Provider.
- **Bootstrap-Admin:** beim ersten Deploy `ADMIN_BOOTSTRAP_ENABLED=true` +
  `ADMIN_EMAIL` + `ADMIN_PASSWORD` setzen, nach erstem erfolgreichen Login
  auf `false` zurückdrehen.
- **Origin-Allowlist:** Sowohl Railway-Default-Domain als auch (optional)
  Custom-Domain in `FRONTEND_ORIGIN` aufnehmen. Komma-separiert.

## Datenmodell-/Codeänderungen

Keine. Iteration 6 ist reine Konfiguration + Operations. Das Schema und der
Code aus Iteration 5 bleiben unverändert.

Falls während der Inbetriebnahme Bugs auffallen, werden sie als nachgezogene
Bugfix-Notizen in Wave 4 dokumentiert.

## Waves

| Wave | Skill | Focus | Status |
|---|---|---|---|
| Wave 1 | infrastructure-eng | Cloudflare R2 Bucket + Token anlegen, S3-Credentials erzeugen | Planned |
| Wave 2 | infrastructure-eng | Railway-Env-Variablen für Backend + Frontend setzen (inkl. R2, SMTP, FRONTEND_ORIGIN multi-origin) | Planned |
| Wave 3 | infrastructure-eng | SMTP-Test-Provider (Mailtrap Sandbox) anbinden | Planned |
| Wave 4 | testing-eng | Staging-Smokes aus Iter-5 Wave 6 Playbook gegen Railway ausführen und Ergebnisse dokumentieren | Planned |
| Wave 5 | infrastructure-eng | Runbook in `README.md` (oder `docs/railway-setup.md`) für Kunden-Clones aktualisieren | Planned |

## Critical Path

1. Wave 1 liefert R2-Bucket + Credentials. Ohne Bucket keine Anhänge.
2. Wave 2 setzt diese plus alle weiteren Variablen auf Railway. Erst danach
   funktioniert der Bewerbungs-Endpunkt überhaupt produktiv (Origin-Guard,
   DB, Redis, S3).
3. Wave 3 ermöglicht den Rejection-Mail-Test.
4. Wave 4 verifiziert die komplette Strecke gegen Staging.
5. Wave 5 macht das reproduzierbar für Kunden-Clones.

## Acceptance Criteria

- [ ] Bucket angelegt, Credentials gespeichert in Railway-Backend-Service.
- [ ] Backend in Railway läuft mit allen Env-Variablen (`FRONTEND_ORIGIN`,
      `DATABASE_URL`, `REDIS_URL`, `S3_*`, `SMTP_*`, `ADMIN_*`,
      `APPLICATION_RETENTION_*`).
- [ ] Frontend in Railway nutzt korrekten `BACKEND_API_URL`.
- [ ] Bewerbung mit PDF-Anhang über die produktive URL absendbar — Response
      201, Datei landet im R2-Bucket, Metadaten in DB.
- [ ] Admin-Login auf produktiver URL möglich, Bewerbungs-Detail zeigt
      Anhang, Download liefert die Datei zurück.
- [ ] Rejection-Workflow auf produktiver URL: Statuswechsel → Mail
      empfangen (Mailtrap), `delete_after = rejected_at + 90d`.
- [ ] Manueller Retention-Trigger
      `POST /api/admin/applications/retention/run` löscht eine fällige
      Bewerbung inkl. R2-Objekt.
- [ ] `forbidden_origin` und `service_unavailable` treten in den Smokes
      nicht mehr auf.
- [ ] Runbook in `README.md` (oder `docs/railway-setup.md`) beschreibt den
      kompletten Ablauf reproduzierbar — ein Kunden-Clone kann das ohne
      Rückfragen abarbeiten.

## Risiken

- **R2-Token-Scope falsch:** Wenn der R2-Token nur Read hat, schlägt Upload
  fehl mit Storage-Fehler. Mitigation: Token mit Read+Write+Delete für genau
  diesen Bucket beim Anlegen.
- **Path-Style-Mismatch:** AWS-S3 will Virtual-Hosted-Style, R2 will Path-
  Style. Falsche Einstellung erzeugt `SignatureDoesNotMatch`. Mitigation:
  `S3_USE_PATH_STYLE=true` für R2.
- **SMTP-Test-Provider Quota:** Mailtrap Sandbox hat Inbox-Limits.
  Mitigation: nur für Tests nutzen, pro Kunde echten Provider.
- **Origin-Mismatch wegen Custom-Domain:** Sobald Kunde eine eigene Domain
  vor Railway setzt, muss `FRONTEND_ORIGIN` erweitert werden. Mitigation:
  Multi-Origin (CSV) wird seit Iter-5-Bugfix unterstützt; Runbook nennt
  beide URL-Varianten.
- **Admin-Bootstrap nicht zurückgedreht:** Wenn `ADMIN_BOOTSTRAP_ENABLED`
  produktiv `true` bleibt, kann ein Restart das Passwort wieder auf den
  Initial-Wert setzen. Mitigation: nach erstem Login explizit zurückdrehen,
  im Runbook hervorgehoben.

## Offene Detailentscheidungen

- Welche Region für R2? (Default: `auto`.)
- Soll der Master-Template-Smoke mit echten Bewerbungsdaten oder Dummy-
  Daten laufen? (Empfehlung: Dummy mit klar erkennbarem Namen wie
  „Smoke Test", danach manuelles Cleanup.)
- Eigenes Mailtrap-Account oder existierendes Anthropic-Team-Account? (Wird
  am Wave-3-Start entschieden.)

## Verification

Wird ergänzt nach Abschluss der Waves analog zu Iteration 5.
