# Setup-Anleitung — Vault & Workflow

---

## Pfade verstehen (wichtig zuerst lesen)

Die Vault liegt im WSL-Dateisystem. Dieselbe Vault ist über zwei verschiedene
Pfade erreichbar — je nachdem von wo aus du arbeitest:

| Von wo | Pfad |
|---|---|
| WSL-Terminal / Claude Code (WSL) | `/home/andersen/git/courtbyte-brain` |
| Windows Explorer / Obsidian (Windows) | `\\wsl.localhost\vibecoding\home\andersen\git\courtbyte-brain` |

**Für `.vault`-Dateien in Code-Repos gilt:**
- Claude Code läuft in WSL → WSL-Pfad: `/home/andersen/git/courtbyte-brain`
- Codex läuft unter Windows → Windows-Pfad: `\\wsl.localhost\vibecoding\home\andersen\git\courtbyte-brain`
- Im Zweifel: WSL-Pfad — Claude Code ist der primäre LLM

**Das Teammitglied** klont die Vault irgendwo auf seiner Maschine und trägt
seinen lokalen Pfad in `.vault` ein. Die Repo-Dateien bleiben für alle gleich.

---

## Zwei-Repo-Modell

```
GitHub
├── team/courtbyte-brain   ← Vault (Planungsdateien, Wiki)
└── team/foxletter          ← Code-Repo

Deine Maschine (WSL)
├── /home/andersen/git/courtbyte-brain/   ← Vault-Klon
└── /home/andersen/git/foxletter/          ← Code-Klon
    └── .vault  →  /home/andersen/git/courtbyte-brain

Teammate's Maschine (Beispiel Mac)
├── /Users/max/vault/        ← Vault-Klon (anderer Pfad, egal)
└── /Users/max/foxletter/    ← Code-Klon
    └── .vault  →  /Users/max/vault
```

`.vault` ist immer gitignored — jeder trägt seinen eigenen Pfad ein.
`CLAUDE.md` und `AGENTS.md` im Code-Repo enthalten keinen Pfad, sie sagen
nur "lies `.vault` zuerst". Deshalb funktionieren sie für alle.

---

## Teil 1 — Vault auf GitHub pushen (einmalig, nur du)

Die Vault ist bereits ein Git-Repo (`.git` vorhanden). Nur noch pushen:

```bash
cd /home/andersen/git/courtbyte-brain

# GitHub: neues Repo anlegen ("courtbyte-brain"), dann:
git remote add origin https://github.com/DEIN-USER/courtbyte-brain.git
git add .
git commit -m "init: vault setup"
git branch -M main
git push -u origin main

# Teammitglied einladen:
# GitHub → Settings → Collaborators → Add people
```

---

## Teil 2 — Code-Repo vorbereiten (einmalig pro Projekt, nur du)

### 2a. `.vault` anlegen — WSL-Pfad

```bash
cd /home/andersen/git/PROJEKTNAME
echo "/home/andersen/git/courtbyte-brain" > .vault
```

### 2b. `.vault` in `.gitignore` eintragen

```bash
echo ".vault" >> .gitignore
```

### 2c. `CLAUDE.md` anlegen

```markdown
# PROJEKTNAME — Technical Rules (Claude Code)

## Vault
Lies `.vault` in diesem Repo-Root um den Vault-Pfad zu ermitteln.
Planungsdateien: {vault}/02-projects/PROJEKTNAME/

## Session Start
1. `.vault` lesen → {vault} Pfad ermitteln
2. {vault}/02-projects/PROJEKTNAME/Rules.md lesen
3. {vault}/02-projects/PROJEKTNAME/Projektplan.md lesen
4. {vault}/02-projects/PROJEKTNAME/Iterations/Iteration-N/Iteration-N.md lesen
5. {vault}/02-projects/PROJEKTNAME/Iterations/Iteration-N/Waves/Wave-X.md lesen
6. Skill aus der Wave-Datei aufrufen (/skill-name)

## Architecture
(Framework, Sprache, Constraints)

## Code Rules
(Naming, Linting, Tests)
```

### 2d. `AGENTS.md` anlegen

```markdown
# PROJEKTNAME — Technical Rules (Codex)

## Vault
Lies `.vault` in diesem Repo-Root um den Vault-Pfad zu ermitteln.
Planungsdateien: {vault}/02-projects/PROJEKTNAME/

## Session Start
1. `.vault` lesen → {vault} Pfad ermitteln
2. {vault}/02-projects/PROJEKTNAME/Rules.md lesen
3. {vault}/02-projects/PROJEKTNAME/Projektplan.md lesen
4. {vault}/02-projects/PROJEKTNAME/Iterations/Iteration-N/Iteration-N.md lesen
5. {vault}/02-projects/PROJEKTNAME/Iterations/Iteration-N/Waves/Wave-X.md lesen
6. Rolle aus dem Skill-Feld der Wave übernehmen

## Architecture
(Framework, Sprache, Constraints)

## Code Rules
(Naming, Linting, Tests)
```

### 2e. Committen (`.vault` bleibt lokal)

```bash
git add CLAUDE.md AGENTS.md .gitignore
git commit -m "chore: add LLM workflow config"
git push
```

---

## Teil 3 — Teammitglied einrichten (einmalig, auf seiner Maschine)

```bash
# 1. Vault klonen
git clone https://github.com/DEIN-USER/courtbyte-brain.git ~/vault

# 2. Code-Repo klonen
git clone https://github.com/DEIN-USER/foxletter.git ~/foxletter

# 3. Eigene .vault anlegen (SEINEN lokalen Pfad eintragen)
cd ~/foxletter
echo "/home/max/vault" > .vault     # Linux/Mac
# Windows: echo "C:\Users\max\vault" > .vault
```

`.vault` ist bereits in `.gitignore` — wird nie committet.

---

## Teil 4 — Laufender Workflow

### Vor einer neuen Iteration: Stand holen

```bash
cd /home/andersen/git/courtbyte-brain && git pull
cd /home/andersen/git/PROJEKTNAME && git pull
```

### Nach abgeschlossener Iteration: pushen

```bash
# Vault pushen
cd /home/andersen/git/courtbyte-brain
git add 02-projects/PROJEKTNAME/Iterations/Iteration-N/
git commit -m "feat: Iteration N abgeschlossen"
git push

# Code pushen
cd /home/andersen/git/PROJEKTNAME
git push
```

---

## Checkliste

### Vault-Repo — einmal für alle, bereits erledigt
- [x] `CLAUDE.md` — Vault-Schema für Claude Code
- [x] `AGENTS.md` — Vault-Schema für Codex
- [x] `.gitignore` — enthält `.vault` und Obsidian-Cache
- [x] `meta/onboarding.md`, `meta/workflow.md`, `meta/vault-map.md`
- [x] `templates/Iteration.md`, `templates/Wave.md`
- [x] `02-projects/` mit Projektordnern

### Code-Repo — committed, für alle gleich
- [ ] `CLAUDE.md` — Vault-Pointer + Projektregeln (kein Pfad hardcodiert)
- [ ] `AGENTS.md` — Vault-Pointer + Projektregeln (kein Pfad hardcodiert)
- [ ] `.gitignore` — enthält `.vault`

### Code-Repo — lokal, nie committed
- [ ] `.vault` — dein persönlicher lokaler Vault-Pfad
