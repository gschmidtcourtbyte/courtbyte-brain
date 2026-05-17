# Wave 5 — Runbook für Kunden-Clones

**Skill:** `/infrastructure-eng`
**Status:** Planned
**Blocked by:** Waves 1–4

## Ziel

Die in den Waves 1–4 gemachten Erfahrungen in ein reproduzierbares Runbook
gießen, damit Kunden-Clones in wenigen Minuten produktionsreif sind und
diese Iteration nicht in Personen-Wissen stecken bleibt.

## Tasks

1. **`docs/railway-setup.md` (neu) im Repo anlegen** mit folgenden
   Abschnitten:
   - Vorbedingungen (Railway-Account, GitHub-Repo, Cloudflare-Account,
     SMTP-Provider).
   - Schritt 1: Railway-Projekt anlegen, 2 Services aus dem Repo
     (`/backend` + `/frontend`), Postgres- und Redis-Plugins hinzufügen.
   - Schritt 2: R2-Bucket + API-Token anlegen (Tabelle der Werte).
   - Schritt 3: SMTP-Provider entscheiden + Credentials erzeugen.
   - Schritt 4: Env-Variablen setzen (Tabellen für Backend + Frontend
     analog zu Wave 2).
   - Schritt 5: Erstes Deploy, Admin-Bootstrap zurückdrehen.
   - Schritt 6: Staging-Smoke-Sequenz (verlinkt zu Wave 4).
   - Schritt 7: Custom-Domain hinzufügen — wie `FRONTEND_ORIGIN`
     erweitern.

2. **`README.md` ergänzen:**
   - Im Abschnitt „Railway Deployment" auf `docs/railway-setup.md`
     verlinken.
   - Hinweis: „S3 ist kein Railway-Plugin — externer Provider nötig
     (R2 default)."

3. **`backend/.env.example` polieren:**
   - Kommentare über jeder Variable, was sie tut und welcher Default
     sinnvoll ist.
   - `FRONTEND_ORIGIN` Beispiel mit Multi-Origin.

4. **Optional: `scripts/railway-setup.sh`** — Shell-Skript, das alle
   Variablen via `railway variables set` einträgt. Geheimnisse werden aus
   `.env.railway.local` (gitignored) gelesen. Idempotent.

## Akzeptanzkriterien

- [ ] `docs/railway-setup.md` existiert und ist von einer dritten Person
      ohne Rückfrage durcharbeitbar.
- [ ] `README.md` verlinkt das Runbook.
- [ ] `.env.example` hat Kommentare zu allen relevanten Variablen.
- [ ] Optionales Skript läuft idempotent (zweiter Aufruf ohne Fehler).

## Out of Scope

- Vollautomatisches Provisioning (Terraform für Railway gibt es nicht
  stabil; Cloudflare-Terraform-Provider ginge, aber nicht in dieser
  Iteration).
- Mehrsprachiges Runbook.
- Backup-Strategie für DB und Bucket.

## Notizen für Codex

- Das Runbook ist Code-Repo-Inhalt (`/home/andersen/git/website-template`),
  nicht Vault-Inhalt — analog zu README.
- Beim Anlegen darauf achten, keine Secrets in Beispielen zu schreiben.
- Beispiel-Werte für URLs sollen klar als Platzhalter erkennbar sein
  (`<frontend>`, `<accountid>` etc.).
