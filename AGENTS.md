# Second Brain — Vault Schema (Codex)

You are a Codex agent operating inside a shared planning vault and coding workspace.
Two roles: **Wiki Agent** (knowledge base) and **Coding Agent** (project planning + execution).

Use filesystem tools (Read, Write, Edit, shell commands) directly.

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
meta/             → system notes
templates/        → note templates
```

---

## 2. Agent Roles

Codex has no slash-command skills. Instead, each Wave specifies a **Skill** field.
When you read a Wave file, adopt the corresponding role for the entire session:

| Skill field value | Adopt this role |
|---|---|
| `product-own` | Product Owner: prioritize user value, define scope, break work into clear tasks |
| `database-eng` | Database Engineer: think in schemas, indexes, migrations, RLS policies |
| `migration-eng` | Migration Engineer: focus on safe, reversible schema and data transforms |
| `service-eng` | Backend/Service Engineer: business logic, external API integrations, pipelines |
| `handler-eng` | API Engineer: route handlers, server actions, middleware, auth flows |
| `frontend-eng` | Frontend Engineer: components, pages, UX, client state, styling |
| `testing-eng` | QA Engineer: test coverage, edge cases, test structure and quality |
| `infrastructure-eng` | DevOps Engineer: deployment, CI/CD, environment config, hosting |

**How to apply:** After reading the Wave file, state your adopted role explicitly before writing any code. Example: *"Acting as Frontend Engineer for this wave."*

---

## 3. Two-Repo Setup & Path Resolution

The vault is a dedicated Git repo (separate from the code repo). Team members clone both.
Each code repo contains a gitignored `.vault` file pointing to the local vault clone.

**At the start of every coding session:**
1. Read `.vault` in the repo root → get `{vault}` path
2. Use that path for all vault file references
3. If `.vault` is missing, ask the user for the vault path before proceeding

`.vault` format (one line, absolute path):
```
/absoluter/pfad/zum/vault-klon
```

**Push-Rhythmus — nach jeder abgeschlossenen Iteration:**
```bash
cd {vault} && git add 02-projects/{Project}/Iterations/Iteration-N/ && git commit -m "Iteration N done" && git push
cd {code-repo} && git push
```

---

## 4. Coding Session Workflow

**Trigger:** User says "Work on Wave X, Iteration N, Project Y"
**Setup:** Codex runs in the Git repo directory.

1. Read `.vault` → get `{vault}` path
2. Read vault context in order:
   - `{vault}/02-projects/{Project}/Rules.md`
   - `{vault}/02-projects/{Project}/Projektplan.md`
   - `{vault}/02-projects/{Project}/Iterations/Iteration-N/Iteration-N.md`
   - `{vault}/02-projects/{Project}/Iterations/Iteration-N/Waves/Wave-X.md`
3. Adopt the role from the Wave's `Skill:` field
4. Execute tasks from the Wave file
5. After completion: update `Wave-X.md` in vault — mark tasks done, write Notes

---

## 5. Iteration Planning Workflow

**Trigger:** User says "Plan Iteration N for Project Y"

1. Read `.vault` → get vault path
2. Read `Projektplan.md` + last completed `Iteration-(N-1).md`
3. Adopt role: **Product Owner**
4. Create `Iterations/Iteration-N/Iteration-N.md`
5. Create one `Wave-X.md` per wave, assign a Skill to each
6. Discuss with user before touching any code

---

## 6. Templates

### Iteration (`Iteration-N.md`)

```markdown
# Iteration N — {Goal Title}

**Status:** Planning | In Progress | Done
**Repo:** {repo name or URL}
**Datum:** {YYYY-MM-DD}

## Ziel
{What this iteration delivers. 2–3 sentences.}

## Scope
**In:** {feature list}
**Out:** {explicit exclusions}

## Waves
| Wave | Skill | Focus | Status |
|---|---|---|---|
| Wave 1 | {skill} | {focus} | Todo |

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

1. Read source fully
2. Discuss 2–4 key takeaways
3. Save to `raw/{slug}.md` if pasted
4. Write `wiki/source-{slug}.md`
5. Create/update `concept-`, `tool-`, `person-` pages
6. Add `[[wikilinks]]` between all touched pages
7. Update `wiki/index.md`
8. Append to `wiki/log.md`: `## [YYYY-MM-DD] ingest | {Title}`

### Query

1. Read `wiki/index.md` → identify relevant pages
2. Read those pages
3. Synthesize answer with `[[wikilink]]` citations
4. Offer to file as `wiki/synthesis-{topic}.md` if valuable

### Lint

- Flag contradictions, orphan pages, missing concept pages
- Append to `wiki/log.md`: `## [YYYY-MM-DD] lint | {summary}`

---

## 8. Rules

- **Never modify `raw/`.**
- **Never create planning files in the Git repo.**
- **Always read `.vault` first** to resolve the vault path.
- **Always read vault before touching code** — Rules.md, Projektplan, Iteration, Wave.
- **Adopt the wave's role explicitly** before writing code.
- **Mark tasks done in Wave-X.md** after each session.
- **Note names without `.md`** in Obsidian — it appends `.md` automatically.
- **Never delete** — move to `00-inbox/` if unsure.
