# Workflow Patterns

---

## Übersicht — Zwei-Repo-Modell

```
vault-repo   → Planungsdateien (Iterationen, Waves, Wiki). Eigenes Git-Repo.
code-repo    → Quellcode. Eigenes Git-Repo.
.vault       → Gitignorierte Datei im code-repo. Enthält lokalen Pfad zum vault-repo-Klon.
```

Push-Rhythmus: **nach jeder abgeschlossenen Iteration** werden beide Repos gepusht.
Pull-Rhythmus: **vor Beginn einer neuen Iteration** werden beide Repos gepullt.

---

## Setup — Neues Repo in den Workflow integrieren (einmalig pro Teammitglied)

```bash
# 1. Im Repo-Verzeichnis:
echo "/absoluter/pfad/zur/vault" > .vault

# 2. .vault gitignoren
echo ".vault" >> .gitignore

# 3. CLAUDE.md und AGENTS.md anlegen (Templates aus vault CLAUDE.md Abschnitt 4)
```

Jedes Teammitglied führt diese Schritte lokal aus. `.vault` wird nie committed.

---

## Iteration-Zyklus (Gesamtablauf)

```
git pull (vault)          ← Stand vom Kollegen holen
git pull (code-repo)      ← Code-Stand holen
  ↓
Iteration planen          ← Vault: Iteration-N.md + Wave-X.md anlegen
  ↓
Wave 1 ausführen          ← Code-Repo: LLM liest Vault, schreibt Code
Wave 1 abhaken            ← Vault: Wave-1.md Tasks als done markieren
  ↓
Wave 2 ausführen
Wave 2 abhaken
  ↓
Iteration abgeschlossen
  ↓
git push (vault)          ← Planungsstand teilen
git push (code-repo)      ← Code teilen
```

---

## A — Wave ausführen (Coding Session)

**Claude Code:** `claude` im Repo-Verzeichnis starten
**Codex:** `codex` im Repo-Verzeichnis starten

Beide LLMs laden automatisch ihre Konfigurationsdatei (`CLAUDE.md` / `AGENTS.md`).

1. LLM liest `.vault` → ermittelt Vault-Pfad
2. LLM liest Vault-Kontext (in Reihenfolge):
   - `{vault}/02-projects/{Project}/Rules.md`
   - `{vault}/02-projects/{Project}/Projektplan.md`
   - `{vault}/02-projects/{Project}/Iterations/Iteration-N/Iteration-N.md`
   - `{vault}/02-projects/{Project}/Iterations/Iteration-N/Waves/Wave-X.md`
3. **Claude Code:** Skill aus Wave-Datei aufrufen (`/frontend-eng` etc.)
   **Codex:** Rolle aus Wave-Datei übernehmen ("Acting as Frontend Engineer")
4. Tasks aus Wave-Datei abarbeiten
5. Wave-Datei in Vault abhaken + Notes schreiben

---

## B — Neue Iteration planen

**Aus der Vault heraus** (Claude oder Codex):

1. Lese `02-projects/{Project}/Projektplan.md`
2. Lese letzte abgeschlossene `Iteration-(N-1).md`
3. **Claude Code:** `/product-own` aufrufen
   **Codex:** Product Owner Rolle übernehmen
4. Erstelle `Iterations/Iteration-N/Iteration-N.md`
5. Erstelle `Iterations/Iteration-N/Waves/Wave-X.md` pro Wave (mit Skill-Zuweisung)
6. Plan mit User besprechen — kein Code wird angefasst

---

## C — Iteration abschließen & pushen

Wenn alle Waves einer Iteration done sind:

```bash
# Vault pushen
cd $(cat .vault)
git add 02-projects/{Project}/Iterations/Iteration-N/
git commit -m "feat: Iteration N — {Titel} abgeschlossen"
git push

# Code-Repo pushen
cd {code-repo}
git push
```

Teammate führt danach aus:
```bash
cd vault && git pull
cd code-repo && git pull
```

---

## D — Neues Projekt anlegen

1. Erstelle `02-projects/{ProjectName}/`
2. Erstelle `Projektplan.md` und `Rules.md`
3. Erstelle `Iterations/` (leer)
4. Im Git-Repo: `.vault`, `CLAUDE.md`, `AGENTS.md`, `.gitignore` anlegen
5. Vault-Repo-Änderung committen und pushen

---

## Skill → Codex-Rollen-Mapping

| Skill | Claude Code | Codex |
|---|---|---|
| `product-own` | `/product-own` | "Acting as Product Owner" |
| `database-eng` | `/database-eng` | "Acting as Database Engineer" |
| `migration-eng` | `/migration-eng` | "Acting as Migration Engineer" |
| `service-eng` | `/service-eng` | "Acting as Backend/Service Engineer" |
| `handler-eng` | `/handler-eng` | "Acting as API/Handler Engineer" |
| `frontend-eng` | `/frontend-eng` | "Acting as Frontend Engineer" |
| `testing-eng` | `/testing-eng` | "Acting as QA/Testing Engineer" |
| `infrastructure-eng` | `/infrastructure-eng` | "Acting as DevOps/Infrastructure Engineer" |

---

## D — Wiki-Ingest

1. Quelle lesen
2. 2–4 Key Takeaways besprechen
3. In `raw/{slug}.md` speichern (falls eingefügt)
4. `wiki/source-{slug}.md` erstellen
5. Relevante `concept-`, `tool-`, `person-`-Seiten erstellen/updaten
6. Wikilinks setzen
7. `wiki/index.md` updaten
8. `wiki/log.md` appenden: `## [YYYY-MM-DD] ingest | {Titel}`

---

## E — Wiki-Query

1. `wiki/index.md` lesen
2. Relevante Seiten lesen
3. Antwort mit `[[wikilinks]]` synthetisieren
4. Als `wiki/synthesis-{topic}.md` ablegen anbieten

---

## F — Wiki-Lint

- Widersprüche, Orphan-Seiten, fehlende Konzeptseiten markieren
- `wiki/log.md` appenden: `## [YYYY-MM-DD] lint | {Summary}`
