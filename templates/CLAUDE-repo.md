# {Projektname} — {Kurzbeschreibung}

## Vault

Lies `.vault` in diesem Repo-Root um den Vault-Pfad zu ermitteln.
Planungsdateien liegen in: `{vault}/02-projects/{Projektname}/`

### Session Start — immer in dieser Reihenfolge lesen

1. `.vault` lesen → `{vault}` Pfad ermitteln
2. `{vault}/02-projects/{Projektname}/Rules.md`
3. `{vault}/02-projects/{Projektname}/Projektplan.md`
4. `{vault}/02-projects/{Projektname}/Iterations/Iteration-N/Iteration-N.md`
5. `{vault}/02-projects/{Projektname}/Iterations/Iteration-N/Waves/Wave-X.md`
6. Skill aus der Wave-Datei aufrufen

---

## Projekt

{Kurze Beschreibung was die App macht. 2–3 Sätze. Zielgruppe, Hauptfeature, Status.}

## Tech Stack

- **Backend**: {Sprache, Framework, Router}
- **Frontend**: {Rendering-Ansatz, CSS-Framework, JS}
- **Datenbank**: {DB + Version, Cache}
- **Deployment**: {Plattform, Container}

## Projekt-Struktur

```
{verzeichnis}/     — {Zweck}
{verzeichnis}/     — {Zweck}
```

## Kommandos

```bash
{make dev / npm run dev / ...}   # Lokale Entwicklung starten
{make build / npm run build}     # Build
{make test / npm test}           # Tests
{make migrate-up / ...}          # Migrationen anwenden
{make lint / npm run lint}       # Linter
```

## Konventionen

- {Wichtigste Architektur-Entscheidung}
- {Naming-Konvention}
- {Pattern das konsequent eingehalten wird}
- {Was explizit NICHT verwendet wird}

## Design / UI

- {Theme, Stil, Orientierung}
- {Responsive-Strategie}
- {Design-System oder Referenz-Seite}
