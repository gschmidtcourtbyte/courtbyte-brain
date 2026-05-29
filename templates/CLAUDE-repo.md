# {Projektname} — {Kurzbeschreibung}

## Vault

Lies `.vault` in diesem Repo-Root um den Vault-Pfad zu ermitteln.
Planungsdateien liegen in: `{vault}/02-projects/{Projektname}/`

### Coding Session — Wave ausführen

1. `.vault` lesen → `{vault}` Pfad ermitteln
2. `{vault}/02-projects/{Projektname}/Rules.md` lesen
3. `{vault}/02-projects/{Projektname}/Projektplan.md` lesen
4. `{vault}/02-projects/{Projektname}/Iterations/Iteration-N/Iteration-N.md` lesen
5. `{vault}/02-projects/{Projektname}/Iterations/Iteration-N/Waves/Wave-X.md` lesen
6. Skill aus der Wave-Datei aufrufen (Claude: `/skill-name` / Codex: Rolle benennen)
7. Tasks ausführen — nur Code-Repo-Dateien anfassen
8. Nach Abschluss: Wave-X.md in der Vault abhaken

### Planning Session — Iteration planen

1. `.vault` lesen → `{vault}` Pfad ermitteln
2. `{vault}/02-projects/{Projektname}/Projektplan.md` lesen
3. Letzte abgeschlossene Iteration lesen
4. Claude: `/product-own` aufrufen / Codex: *"Acting as Product Owner"*
5. Plan mit User besprechen — erst dann Dateien schreiben
6. Iteration-N.md und Wave-X.md in der Vault erstellen (nicht im Repo!)
7. Vault committen und pushen nach Freigabe durch User

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
