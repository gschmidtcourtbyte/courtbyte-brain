# Vault Map

```
Courtbyte/                          ← Vault Root (Obsidian öffnet hier)
│
├── CLAUDE.md                       ← LLM-Schema: alle Regeln, Workflows, Conventions
│
│
├── 02-projects/                    ← Aktive Coding-Projekte
│   └── {ProjectName}/
│       ├── Projektplan.md          ← Scope, Ziele, Tech-Stack
│       ├── Rules.md                ← Coding-Regeln & Architektur-Konventionen
│       ├── {context}.md            ← Weitere Kontext-Dateien (API-Contracts etc.)
│       └── Iterations/
│           └── Iteration-N/
│               ├── Iteration-N.md  ← Ziel, Scope, Wave-Liste, Acceptance Criteria
│               └── Waves/
│                   └── Wave-X.md   ← Tasks, Dateiziele, Done-Criteria
│
│
├── meta/                           ← System-Notizen
│   ├── vault-map.md                ← Diese Datei
│   ├── workflow.md                 ← Workflow-Patterns
│   └── ai-rules.md                 ← Regelreferenz
│
└── templates/                      ← Vorlagen
    ├── Iteration.md
    └── Wave.md
```

---

## Wichtige Konventionen

- **Obsidian-Notizen** ohne `.md` im Namen benennen → Obsidian speichert als `Name.md`
- **Planungsdateien** gehören immer in die Vault, nie ins Git-Repo
- **Git-Repos** haben ihr eigenes minimales `CLAUDE.md` mit Vault-Pfad-Zeiger
- **raw/** ist immutable — LLM liest, schreibt nie
- **wiki/** gehört dem LLM — User liest, LLM schreibt
