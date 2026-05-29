# Wave 1 — Cloudflare R2 Bucket + Credentials

**Skill:** `/infrastructure-eng`
**Status:** Planned
**Blocked by:** —

## Ziel

Einen S3-API-kompatiblen Bucket aufsetzen, der die Bewerbungsanhänge aufnimmt.
Default: Cloudflare R2. Begründung: keine Egress-Kosten, S3-API-kompatibel,
Token-basierte Auth.

## Tasks

1. **Cloudflare-Account vorbereiten.**
   - Falls noch nicht vorhanden: Cloudflare-Account anlegen.
   - R2 im Dashboard aktivieren (Free-Tier ausreichend für Master-Template).

2. **Bucket anlegen.**
   - Name: `website-template-attachments-prod`.
   - Region/Jurisdiction: `auto` (Default), außer es gibt EU-Datenresidenz-
     Pflicht — dann explizit `EU` wählen.
   - Public Access: **deaktiviert** (Downloads laufen ausschließlich über
     Backend-Auth).

3. **CORS-Regel setzen (defensiv, falls je Browser-Direkt-Upload kommt).**
   - Für die aktuelle Iteration genügt: keine Public-CORS-Konfiguration, weil
     der Upload serverseitig über das Backend läuft. CORS-Regeln explizit
     leer lassen.

4. **API-Token erzeugen.**
   - Im R2-Dashboard: „Manage R2 API Tokens" → „Create API token".
   - Permissions: **Object Read & Write** (Read, Write, Delete) **nur für den
     angelegten Bucket**.
   - TTL: kein Ablauf für Production-Token; für Staging-Token 90 Tage als
     Rotationssignal.
   - Token notieren:
     - `Access Key ID`
     - `Secret Access Key`
     - `S3 Endpoint` (Form: `https://<accountid>.r2.cloudflarestorage.com`)

5. **Smoke-Test mit `aws` CLI lokal (optional, schnelles Vertrauen).**
   ```bash
   aws s3 --endpoint-url https://<accountid>.r2.cloudflarestorage.com \
     --region auto \
     cp ./README.md s3://website-template-attachments-prod/smoke.txt
   aws s3 --endpoint-url https://<accountid>.r2.cloudflarestorage.com \
     --region auto \
     ls s3://website-template-attachments-prod/
   aws s3 --endpoint-url https://<accountid>.r2.cloudflarestorage.com \
     --region auto \
     rm s3://website-template-attachments-prod/smoke.txt
   ```
   Erwartung: Put, List und Delete laufen ohne Auth-Fehler.

## Akzeptanzkriterien

- [ ] Bucket `website-template-attachments-prod` existiert in Cloudflare R2.
- [ ] API-Token mit Read+Write+Delete-Scope auf genau diesen Bucket angelegt.
- [ ] Credentials sicher gespeichert (z. B. im Vault unter `00-secrets/`).
- [ ] Optionaler `aws s3`-Smoke erfolgreich.

## Out of Scope

- Lifecycle-Regeln (90-Tage-Auto-Löschung) — die Retention macht das Backend
  selbst. R2-Lifecycle bleibt deaktiviert, sonst löscht es uns Objekte unter
  den Füßen weg.
- Versioning. Default-Off ist ausreichend, weil Retention deterministisch im
  Backend läuft.
- Replikation in mehrere Regionen.

## Notizen für Codex

- R2-Konsole hat keine Terraform-Pflicht; manuell anlegen ist hier
  pragmatisch, weil pro Kunde sowieso 1× pro Clone passiert.
- Falls Cloudflare-Account nicht verfügbar ist, alternative Provider:
  Backblaze B2 (S3-API), AWS S3, MinIO. Der Codex MCP Server entscheidet
  basierend auf vorhandenen Credentials.
