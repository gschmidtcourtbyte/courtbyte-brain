# Iteration 1: Vorzeigbare Landingpage (MVP)

## Ziel
Eine visuell beeindruckende, responsive Landingpage für "Courts Bad Zwischenahn" die der Betreiber sofort potenziellen Partnern und Interessenten zeigen kann. Inklusive funktionierendem Kontaktformular für Hansefit-Registrierungen.

## Design-Richtung
Orientierung an [The Cube Padel](https://thecubepadel.de/):
- **Farbschema**: Dunkles Theme (Schwarz/Anthrazit) mit hellen Akzenten
- **Stil**: Minimalistisch, sportlich, premium
- **Layout**: Full-width Sektionen, großzügige Abstände
- **Typografie**: Moderne Sans-Serif (Inter oder ähnlich)
- **Bildsprache**: Großflächige Baustellenfotos, Drohnenaufnahmen

## Scope

### Epic 1.1: Projekt-Scaffolding & Infrastruktur
**Agent**: infrastructure-eng
**Priority**: P0 (Blocker)
**Blocked by**: none

**Stories**:
1. **Go-Projekt initialisieren**
   - go.mod mit Modul `github.com/courts-bz/padel-bz`
   - Ordnerstruktur: cmd/, internal/, web/, migrations/
   - .gitignore (Go, IDE, .env, uploads)

2. **Docker Compose Setup**
   - docker-compose.yml mit Services: app, postgres, redis
   - PostgreSQL 16 mit Health Check
   - Redis 7 mit Health Check
   - Volume Mounts für Persistenz
   - .env.example mit allen nötigen Variablen

3. **Dockerfile**
   - Multi-stage Build (Builder + Runtime)
   - Minimales Runtime-Image (alpine/distroless)
   - Non-root User

4. **Makefile**
   - `make dev` — Docker Compose up + Hot Reload
   - `make build` — Go Build
   - `make migrate-up` / `make migrate-down`
   - `make test`

5. **Basis-HTTP-Server**
   - cmd/server/main.go mit chi Router
   - Config aus Environment Variables
   - Static File Serving (/static/)
   - Template Engine Setup
   - Graceful Shutdown

**Akzeptanzkriterien**:
- [ ] `docker compose up` startet App, PostgreSQL, Redis
- [ ] App ist unter localhost:8080 erreichbar
- [ ] Static Files werden korrekt ausgeliefert
- [ ] Templates werden gerendert

---

### Epic 1.2: Landingpage Frontend
**Agent**: frontend-eng
**Priority**: P0 (Blocker)
**Blocked by**: Epic 1.1 (Template Engine muss stehen)

**Design-Briefing**:
Erstelle eine moderne, dunkle Landingpage inspiriert von thecubepadel.de. Der Betreiber will etwas Beeindruckendes zeigen. Padel Tennis Anlage in Bad Zwischenahn, aktuell im Bau, Eröffnung Herbst 2026.

**Stories**:
1. **Base Layout & Navigation**
   - HTML Base Template mit Tailwind CSS (CDN für MVP)
   - Sticky Navigation: Logo, Sektions-Links, CTA-Button
   - Mobile Hamburger Menü
   - Smooth Scroll zu Sektionen

2. **Hero Section**
   - Vollbild-Hero mit Drohnenfoto als Hintergrund
   - Overlay mit Logo/Titel "Courts Bad Zwischenahn"
   - Untertitel: "Padel Tennis — Coming Herbst 2026"
   - CTA: "Jetzt für Hansefit registrieren"
   - Optional: Video-Background (vorhandenes MP4)

3. **"Über Padel" Sektion**
   - Was ist Padel Tennis? (kurze Erklärung)
   - Warum Padel? (Vorteile, Spaß, für alle Altersgruppen)
   - Icons oder Illustrationen

4. **"Die Anlage" Sektion**
   - Geplante Ausstattung (Courts, Anzahl, Indoor/Outdoor)
   - Standort Bad Zwischenahn
   - Karte oder Standort-Hinweis
   - Baufortschritt-Teaser (Drohnenfoto)

5. **"Coming Soon" / Countdown Sektion**
   - Eröffnung Herbst 2026
   - Visueller Countdown oder "Coming Soon" Badge
   - Registrierungs-CTA

6. **FAQ Sektion**
   - Akkordeon mit häufigen Fragen
   - Was ist Padel? Wo seid ihr? Was kostet es? Hansefit?

7. **Footer**
   - Kontaktdaten (E-Mail, Telefon)
   - Social Media Links (Platzhalter)
   - Adresse Bad Zwischenahn
   - Links: Impressum, Datenschutz
   - Copyright

**Akzeptanzkriterien**:
- [ ] Seite lädt unter localhost:8080
- [ ] Responsive auf Mobile (375px), Tablet (768px), Desktop (1440px)
- [ ] Dunkles, modernes Design
- [ ] Smooth Scroll Navigation funktioniert
- [ ] Drohnenfoto/Video ist eingebunden
- [ ] Alle Sektionen sind vorhanden

---

### Epic 1.3: Kontaktformular (Hansefit)
**Agent**: handler-eng + service-eng
**Priority**: P1 (Critical)
**Blocked by**: Epic 1.1

**Stories**:
1. **Kontaktformular-Seite**
   - Route: GET /kontakt
   - Felder: Vorname, Nachname, E-Mail, Telefon (optional), Nachricht (optional)
   - Checkbox: "Ich möchte mich für Hansefit registrieren"
   - Checkbox: Datenschutz-Einwilligung (Pflicht)
   - Submit Button

2. **Formular-Handler**
   - Route: POST /kontakt
   - Server-seitige Validierung
   - CSRF Token
   - Erfolgs-Seite / Flash Message
   - Fehler-Darstellung inline

3. **Kontakt-Service**
   - Kontaktanfrage in PostgreSQL speichern
   - E-Mail-Benachrichtigung an Betreiber (SMTP, optional für MVP)

4. **Rate Limiting**
   - Redis-basiertes Rate Limiting
   - Max 5 Anfragen pro IP pro Stunde

**Akzeptanzkriterien**:
- [ ] Formular ist ausfüllbar und abschickbar
- [ ] Validierung funktioniert (Pflichtfelder, E-Mail-Format)
- [ ] CSRF-Schutz aktiv
- [ ] Anfrage wird in DB gespeichert
- [ ] Erfolgs-Feedback nach Absenden
- [ ] Rate Limiting verhindert Spam

---

### Epic 1.4: Datenbank-Migrations
**Agent**: migration-eng
**Priority**: P0 (Blocker)
**Blocked by**: none (kann parallel zu Epic 1.1)

**Stories**:
1. **Migration 001: contact_submissions**
   ```sql
   CREATE TABLE contact_submissions (
       id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
       first_name VARCHAR(100) NOT NULL,
       last_name VARCHAR(100) NOT NULL,
       email VARCHAR(255) NOT NULL,
       phone VARCHAR(50),
       message TEXT,
       hansefit_registration BOOLEAN DEFAULT FALSE,
       privacy_accepted BOOLEAN NOT NULL DEFAULT TRUE,
       ip_address INET,
       created_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
   );
   ```

**Akzeptanzkriterien**:
- [ ] Migration läuft ohne Fehler
- [ ] Rollback funktioniert
- [ ] Tabelle hat korrekte Spalten und Typen

---

### Epic 1.5: Statische Seiten
**Agent**: frontend-eng (oder handler-eng)
**Priority**: P2 (Normal)
**Blocked by**: Epic 1.1

**Stories**:
1. **Impressum-Seite** — Route: /impressum, Platzhalter-Text
2. **Datenschutz-Seite** — Route: /datenschutz, Platzhalter-Text

**Akzeptanzkriterien**:
- [ ] Beide Seiten erreichbar und gerendert
- [ ] Konsistentes Design mit Hauptseite

---

## Abhängigkeits-Graph

```
Epic 1.1 (Infra) ──────┬──→ Epic 1.2 (Frontend)
                        ├──→ Epic 1.3 (Kontaktformular)
                        └──→ Epic 1.5 (Statische Seiten)

Epic 1.4 (Migrations) ─────→ Epic 1.3 (Kontaktformular)
```

## Execution Waves

```
Wave 1 (Parallel, keine Abhängigkeiten):
  ├── infrastructure-eng  → Epic 1.1: Projekt-Scaffolding
  └── migration-eng       → Epic 1.4: DB Migrations

Wave 2 (nach Wave 1):
  ├── frontend-eng        → Epic 1.2: Landingpage + Epic 1.5: Statische Seiten
  ├── database-eng        → Repository für contact_submissions
  └── service-eng         → Contact Service

Wave 3 (nach Wave 2):
  ├── handler-eng         → Epic 1.3: Kontaktformular Handler
  └── frontend-eng        → Kontaktformular UI (Teil von Epic 1.3)

Wave 4 (nach Wave 3):
  └── testing-eng         → Tests für alle Komponenten
```

## Definition of Done — Iteration 1
- [ ] `docker compose up` startet die komplette Anwendung
- [ ] Landingpage ist visuell beeindruckend und responsive
- [ ] Kontaktformular funktioniert end-to-end
- [ ] Hansefit-Registrierung wird gespeichert
- [ ] Impressum und Datenschutz sind erreichbar
- [ ] Keine offensichtlichen Sicherheitslücken (CSRF, SQL Injection)
- [ ] Code ist sauber strukturiert nach Go-Konventionen
