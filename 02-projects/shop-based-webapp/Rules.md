# shop-based-webapp - Coding Rules

## Workflow

- Vor jeder Wave `.vault` im Code-Repo lesen und den Vault-Pfad aufloesen.
- Kontext in dieser Reihenfolge lesen:
  1. `Rules.md`
  2. `Projektplan.md`
  3. `Iterations/Iteration-N/Iteration-N.md`
  4. `Iterations/Iteration-N/Waves/Wave-X.md`
- Skill aus der Wave-Datei uebernehmen und die Rolle explizit benennen.
- Neue Iterationen und Waves ausschliesslich in der Vault anlegen.
- Code-Aenderungen ausschliesslich im Code-Repo ausfuehren.

## Architektur

- Backend bleibt ein modularer Monolith.
- Fachlogik liegt in `internal/*/app`.
- Ports liegen in `internal/*/ports`.
- Adapter liegen in `internal/*/adapters`.
- Shared technische Bausteine liegen in `pkg/*`.
- `pkg/*` darf nicht aus `internal/*` importieren.
- Direkte DB-Zugriffe gehoeren in Repository-/Adapter-Code, nicht in Handler.

## HTTP und Handler

- HTTP-Handler validieren Requests und mappen Responses.
- Business-Regeln gehoeren in Application Services.
- Fehler werden explizit behandelt und zentral auf HTTP-Problem-Responses gemappt.
- Mutierende browsernahe Routen muessen CSRF-Schutz beachten.

## Datenbank

- Schema-Aenderungen nur ueber versionierte SQL-Migrationen in `migrations/`.
- Up- und Down-Migrationen zusammen pflegen.
- Lock-Impact bei grossen Tabellen beachten.
- Keine direkten manuellen Schema-Aenderungen in Staging oder Produktion.

## Frontend

- Customer Web lebt unter `web/customer`.
- Komponenten und Content-Modelle moeglichst nahe an bestehenden Patterns erweitern.
- TypeScript strikt halten; kein neues `any` ohne zwingenden Grund.
- UI-Aenderungen responsiv pruefen.
- Bestehende OUT Padel Design- und Content-Entscheidungen nicht versehentlich zuruecksetzen.

## Tests und Verifikation

- Backend: zielgerichtete `go test` Pakete fuer geaenderte Module, bei groesserem Scope `go test ./...`.
- Frontend: relevante Vitest-Suites, TypeScript-/Build-Check bei UI- oder Datenmodell-Aenderungen.
- Migrationen: Up/Down-Paar und betroffene Queries pruefen.

## Dokumentation

- Technische Docs, ADRs, Runbooks und API-Doku bleiben im Code-Repo unter `docs/`.
- Produktplanung, Iterationen und Waves gehoeren in die Vault.
- Bei abgeschlossenen Waves die Wave-Datei in der Vault abhaken und Notes ergaenzen.

## Was nicht ins Repo gehoert

- `.vault`
- `.env` Dateien und Secrets
- Neue Iterations-/Wave-Planungsdateien
- Generierte Build-Artefakte
