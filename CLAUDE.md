# Second Brain — Vault Schema

You are an LLM agent operating inside a personal knowledge vault and coding workspace.
Two distinct roles: **Wiki Agent** (knowledge base) and **Coding Agent** (project planning + execution).

> MCP tools not yet configured. Use filesystem tools (Read, Write, Edit, Grep, Glob, Bash) directly.

---

## 1. Directory Structure

```
raw/              → source documents. READ-ONLY. Never modify.
wiki/             → LLM-maintained wiki. You own this layer entirely.
wiki/index.md     → catalog of all wiki pages — update on every ingest
wiki/log.md       → append-only event log
00-inbox/         → unprocessed captures
01-ideas/         → raw ideas
02-projects/      → coding project planning (vault-side only)
03-research/      → domain research
04-knowledge/     → refined long-term notes
meta/             → system notes (vault-map, workflow)
templates/        → note templates
```

---

## 2. Agents & Skills

Skills are invoked with `/skill-name` at the start of a coding session. Each wave specifies which skill to use. Always invoke the skill **before** writing any code.

### Skill → Task-Type Mapping

| Skill | Invoke with | Use for |
|---|---|---|
| `product-own` | `/product-own` | Iteration planning, scope decisions, wave breakdown |
| `database-eng` | `/database-eng` | DB schema, migrations, RLS policies, query optimization |
| `migration-eng` | `/migration-eng` | DB migrations specifically (schema changes, data transforms) |
| `service-eng` | `/service-eng` | Business logic, API integrations, background jobs, data pipelines |
| `handler-eng` | `/handler-eng` | API route handlers, Server Actions, middleware, auth flows |
| `frontend-eng` | `/frontend-eng` | UI components, pages, client-side state, styling |
| `testing-eng` | `/testing-eng` | Unit tests, integration tests, test strategy |
| `infrastructure-eng` | `/infrastructure-eng` | Deployment, CI/CD, environment config, hosting |

### Rules for Skill Usage

- **One skill per wave** — if a wave spans multiple domains, split it into two waves.
- **Invoke skill first** — read vault context, then invoke skill, then write code.
- **Skill defines the lens** — a `database-eng` session thinks in schemas and migrations; a `frontend-eng` session thinks in components and state. Trust the skill's defaults.
- **Planning waves use `product-own`** — never plan inside a coding skill session.

### Wave → Skill: Common Patterns

```
Schema / DB setup wave      → database-eng
Migration wave              → migration-eng
API integration wave        → service-eng
Route / Action wave         → handler-eng
UI / Dashboard wave         → frontend-eng
Test coverage wave          → testing-eng
Deploy / Infra wave         → infrastructure-eng
Iteration planning          → product-own
```

---

## 3. Two-Repo Setup (Team)

The vault is itself a Git repository. Code lives in a separate Git repository.
Team members clone both repos independently and work sequentially (not simultaneously).

```
vault-repo  (z.B. github.com/team/vault)   ← Planungsdateien, Wiki
code-repo   (z.B. github.com/team/project) ← nur Quellcode
```

**Path resolution — `.vault` file in every code repo:**
- Each team member creates a local `.vault` file (gitignored) in the code repo
- Content: one line, absolute path to the local vault-repo clone
- Both LLMs read `.vault` first to resolve `{vault}` before any vault file access
- If `.vault` is missing: ask the user for the path before proceeding

`.vault` format:
```
/absoluter/pfad/zum/vault-klon
```

**Push-Rhythmus — nach jeder abgeschlossenen Iteration:**
```bash
# 1. Vault pushen (Planungsstand)
cd {vault}
git add 02-projects/{Project}/Iterations/Iteration-N/
git commit -m "Iteration N abgeschlossen"
git push

# 2. Code-Repo pushen
cd {code-repo}
git push
```

Teammate pullt danach beide Repos → vollständiger, konsistenter Stand.
Kein gleichzeitiges Arbeiten → keine Merge-Konflikte.

---

## 4. Project–Repo Split

**Core rule: planning files live in the Vault. Code lives in the Git repo. Never mix them.**

### Vault structure (`02-projects/{ProjectName}/`)

```
02-projects/{ProjectName}/
├── Projektplan.md              → scope, goals, tech stack
├── Rules.md                    → coding rules and architecture conventions
├── {context}.md                → additional context (API contracts, ADRs, etc.)
└── Iterations/
    └── Iteration-N/
        ├── Iteration-N.md      → goal, scope, wave list, acceptance criteria
        └── Waves/
            └── Wave-X.md       → skill, tasks, file targets, done criteria
```

### Git repo structure

- Source code only. No planning markdown.
- Three setup files in every repo (all team members do this once per machine):

```
.vault          ← gitignored — local absolute path to vault root
CLAUDE.md       ← Claude Code instructions + vault pointer
AGENTS.md       ← Codex instructions + vault pointer
.gitignore      ← must include .vault
```

### `.vault` (gitignored, per-machine)

One line, absolute path to the vault root on this machine:
```
/mnt/c/Users/svenj/Desktop/Courtbyte/Courtbyte
```

### `.gitignore` entry

```
.vault
```

### Repo `CLAUDE.md` template

```markdown
# {ProjectName} — Technical Rules (Claude Code)

## Vault
Read `.vault` in this repo root to get the vault base path.
Project planning lives at: {vault}/02-projects/{ProjectName}/

## Session Start
1. Read `.vault` → get {vault} path
2. Read {vault}/02-projects/{ProjectName}/Rules.md
3. Read {vault}/02-projects/{ProjectName}/Projektplan.md
4. Read {vault}/02-projects/{ProjectName}/Iterations/Iteration-N/Iteration-N.md
5. Read {vault}/02-projects/{ProjectName}/Iterations/Iteration-N/Waves/Wave-X.md
6. Invoke the skill from the Wave's Skill: field

## Architecture
{framework, language, key constraints}

## Code Rules
{naming, linting, test requirements}
```

### Repo `AGENTS.md` template

```markdown
# {ProjectName} — Technical Rules (Codex)

## Vault
Read `.vault` in this repo root to get the vault base path.
Project planning lives at: {vault}/02-projects/{ProjectName}/

## Session Start
1. Read `.vault` → get {vault} path
2. Read {vault}/02-projects/{ProjectName}/Rules.md
3. Read {vault}/02-projects/{ProjectName}/Projektplan.md
4. Read {vault}/02-projects/{ProjectName}/Iterations/Iteration-N/Iteration-N.md
5. Read {vault}/02-projects/{ProjectName}/Iterations/Iteration-N/Waves/Wave-X.md
6. Adopt the role matching the Wave's Skill: field (see AGENTS.md in vault root)

## Architecture
{framework, language, key constraints}

## Code Rules
{naming, linting, test requirements}
```

---

## 4. Coding Session Workflow

**Trigger:** User says "Work on Wave X, Iteration N, Project Y"  
**Setup:** Claude Code runs in the Git repo directory.

1. **Read `.vault`** in repo root → get `{vault}` path
2. **Read vault context** (in order):
   - `{vault}/02-projects/{Project}/Rules.md`
   - `{vault}/02-projects/{Project}/Projektplan.md`
   - `{vault}/02-projects/{Project}/Iterations/Iteration-N/Iteration-N.md`
   - `{vault}/02-projects/{Project}/Iterations/Iteration-N/Waves/Wave-X.md`
3. **Invoke skill** listed in the Wave's `Skill:` field (e.g. `/frontend-eng`)
4. **Execute tasks** from the Wave file — check off mentally as you go
5. **After completion:** update `Wave-X.md` in vault — mark tasks done, write Notes

---

## 5. Iteration Planning Workflow

**Trigger:** User says "Plan Iteration N for Project Y"

1. Read `.vault` if running from repo, or use vault directly if running from vault
2. Read `{vault}/02-projects/{Project}/Projektplan.md` + last `Iteration-(N-1).md`
3. Invoke `/product-own` (Claude Code) or adopt Product Owner role (Codex)
4. Create `Iterations/Iteration-N/Iteration-N.md` using the iteration template
5. Create one `Wave-X.md` per wave, assign a Skill to each
6. Discuss with user before touching any code

---

## 6. Templates

### Iteration (`Iteration-N.md`)

```markdown
# Iteration N — {Goal Title}

**Status:** Planning | In Progress | Done
**Repo:** {absolute path to Git repo}
**Date:** {YYYY-MM-DD}

## Goal
{What this iteration delivers. 2–3 sentences.}

## Scope
**In:** {feature list}
**Out:** {explicit exclusions}

## Waves
| Wave | Skill | Focus | Status |
|---|---|---|---|
| Wave 1 | {skill} | {focus} | Todo |
| Wave 2 | {skill} | {focus} | Todo |

## Acceptance Criteria
- [ ] {criterion}
```

### Wave (`Wave-X.md`)

```markdown
# Wave X — {Focus Title}

**Iteration:** [[Iteration-N]]
**Skill:** {skill-name}
**Status:** Todo | In Progress | Done

## Tasks
- [ ] {task} — `{repo/path/file.ext}`

## Done Criteria
- [ ] {what done looks like}

## Notes
{Decisions, blockers, deviations.}
```

---

## 7. Wiki Workflows

### Ingest

When user drops a source in `raw/` or pastes content:

1. Read source fully
2. Discuss 2–4 key takeaways
3. Save to `raw/{slug}.md` if pasted
4. Write `wiki/source-{slug}.md`
5. Create/update `concept-`, `tool-`, `person-` pages
6. Add `[[wikilinks]]` between all touched pages
7. Update `wiki/index.md`
8. Append to `wiki/log.md`: `## [YYYY-MM-DD] ingest | {Title}`

### Query

1. Read `wiki/index.md`
2. Read relevant pages
3. Synthesize answer with `[[wikilink]]` citations
4. Offer to file as `wiki/synthesis-{topic}.md` if valuable

### Lint

- Flag contradictions, orphan pages, missing concept pages
- Suggest next sources or questions
- Append to `wiki/log.md`: `## [YYYY-MM-DD] lint | {summary}`

---

## 8. General Rules

- **Never modify `raw/`.**
- **Never create planning files in the Git repo.**
- **Never modify `raw/`.**
- **Never create planning files in the Git repo.**
- **Always read `.vault` first** when running from a repo — it resolves the vault path.
- **Always read vault before touching code** — Rules.md, Projektplan, Iteration, Wave.
- **Always invoke the skill** (Claude: `/skill-name` / Codex: adopt role) before coding.
- **Mark tasks done in Wave-X.md** after each session.
- **Push vault + code repo together** when an iteration is fully done.
- **Pull vault before starting a new iteration** — get teammate's latest planning state.
- **Search before creating** — check `wiki/index.md` before writing a new wiki page.
- **Never delete** — move to `00-inbox/` if unsure.
- **Note names without `.md`** — Obsidian appends `.md` automatically.
- **`.vault` file is never committed** — each team member manages it locally.

---

## 9. System Files

- `meta/vault-map.md` — full directory reference
- `meta/workflow.md` — workflow quick-reference
