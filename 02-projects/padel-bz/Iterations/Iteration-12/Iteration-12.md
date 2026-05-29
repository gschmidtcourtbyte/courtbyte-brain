# Iteration 12: Bauphasen-Auswahl und Hansefit-Admin

## Ziel
Die Startseite soll den Baufortschritt in klickbare Bauphasen unterteilen. Beim ersten Aufruf steht der aktuelle Baufortschritt wie bisher im Vordergrund, inklusive kleiner Status-Animation. Weitere Bauphasen werden auf derselben Seite geladen und zeigen erst nach Klick ihr hinterlegtes Video bzw. den gestreiften Platzhalter.

Zusaetzlich bekommt die Hansefit-Registrierung ein kleines Admin-Backend. Hansefit-Anfragen sollen weiterhin per E-Mail eingehen, aber zusaetzlich in einer Admin-Uebersicht verwaltet werden. Admins koennen eine Anfrage von `unbearbeitet` rot auf `bearbeitet` gruen setzen; beim Bearbeiten wird eine Hansefit-Bestaetigung an den Anfragenden gesendet.

## Ausgangslage
- Baufortschritt ist aktuell statisch in `web/templates/pages/home.html`.
- Aktuelles Bauvideo ist `/static/videos/construction.mp4`.
- Gestreifter Platzhalter existiert bereits als CSS-Klasse `.ph` in `web/static/css/style.css`.
- Kontaktformular speichert bereits in `contact_submissions` und sendet Betreiber-E-Mail.
- `contact_submissions` hat noch keine `playtomic_email`, keinen Bearbeitungsstatus und keine Bestaetigungs-Metadaten.
- `web/templates/admin/` existiert, aber ein Admin-Backend ist noch nicht implementiert.
- Hansefit-Logo liegt unter `logo/hansefit.png` und muss fuer Web-Auslieferung nach `web/static/img/hansefit.png` uebernommen werden.

## Scope
- Klickbare Bauphasen auf der Startseite, ohne CMS und ohne Upload.
- Aktuelle Bauphase bleibt initial aktiv und visuell wie bisher hervorgehoben.
- Naechste Bauphase `Hallenbau` wird angelegt und nutzt vorerst den gestreiften Platzhalter statt eines echten Videos.
- Jede Bauphase hat maximal ein Video oder genau einen Platzhalter.
- Kontaktformular wird um `Playtomic-E-Mail-Adresse` und Checkbox `Hansefitregistrierung` erweitert.
- Nicht angehakte Hansefit-Checkbox sendet trotzdem normale Anfrage/E-Mail, erscheint aber nicht in der Hansefit-Admin-Uebersicht.
- Admin-Backend fuer Hansefit-Anfragen mit Login, Liste, Detail/Status-Aktion und Bestaetigungs-E-Mail.
- Test-Admin `PadelBZ` mit Passwort `PadelBZ`.
- Hansefit-Logo wird sichtbar in die Website integriert.

## Nicht im Scope
- Baufortschritt-CMS, Video-Upload oder DB-Modell fuer Bauphasen.
- Mehrere Videos pro Bauphase.
- Vollstaendige Admin-Benutzerverwaltung.
- Rollen/Rechte jenseits eines Admin-Logins.
- Rich-Text oder Datei-Upload im Hansefit-Admin.
- Externe Queue fuer E-Mail-Versand.

## Produktentscheidungen
- Bauphasen werden fuer Iteration 12 als serverseitig gerenderte Daten bzw. Template-Daten gepflegt. Das ist absichtlich kleiner als ein CMS.
- Die Bauphasen-Auswahl funktioniert ohne Seitenwechsel. Alle Bauphasen sind auf der Seite vorhanden; JavaScript schaltet die aktive Phase um.
- Ohne JavaScript bleibt der aktuelle Baufortschritt sichtbar.
- `Rohbau` bleibt die aktuelle Phase und wird beim Seitenaufruf aktiv angezeigt.
- `Hallenbau` wird als zweite Phase angezeigt und zeigt nach Klick einen gestreiften Platzhalter.
- Die Hansefit-Admin-Uebersicht zeigt ausschliesslich Eintraege mit `hansefit_registration = true`.
- Die normale Kontaktanfrage bleibt fuer alle Formularsendungen erhalten.
- Die Bestaetigungs-E-Mail wird beim Wechsel von `unbearbeitet` auf `bearbeitet` versendet. Bei bereits bearbeiteten Anfragen darf die E-Mail nicht unbeabsichtigt erneut gesendet werden.
- Empfaenger der Hansefit-Bestaetigung ist die Playtomic-E-Mail, wenn angegeben; sonst die normale Kontakt-E-Mail.
- `PadelBZ`/`PadelBZ` ist nur ein Testzugang und muss vor Produktivbetrieb ersetzt werden.

## Critical Path
1. Datenmodell fuer Hansefit-Anfragen erweitern.
2. Kontaktformular und Contact-Service anpassen.
3. Admin-Auth-MVP und Test-Admin bereitstellen.
4. Hansefit-Admin-Liste und Statuswechsel inklusive Bestaetigungs-E-Mail bauen.
5. Bauphasen-UI und Hansefit-Logo integrieren.
6. End-to-End pruefen: Formular, Admin-Liste, Statuswechsel, E-Mail, Bauphasen-Klick.

## Agenten-Zuordnung

| Agent | Skillset | Verantwortung |
|-------|----------|----------------|
| PO Lead | product-own | Scope, Priorisierung, Wave-Freigabe, Abnahmekriterien |
| Agent A | migration-eng + database-eng | Migrationen fuer Contact-Erweiterung und Admin-User |
| Agent B | security-eng + handler-eng | Admin-Auth, Session-Schutz, CSRF, Status-Handler |
| Agent C | service-eng | Contact-/Hansefit-Service, Statuslogik, Bestaetigungs-E-Mail |
| Agent D | frontend-eng | Bauphasen-UI, Kontaktformular, Hansefit-Logo, Admin-Templates |
| Agent E | testing-eng | Unit-/Handler-/Regressionstests und manuelle QA-Matrix |
| Agent R | review-eng | Finaler Risiko- und Merge-Readiness-Review |

---

## Wave 1 - Datenbasis und Kontaktvertrag
**Agenten**: Agent A, Agent C
**Kann einzeln ausgefuehrt werden**: ja
**Ziel**: Datenbank, Modell und Service-Vertrag koennen neue Hansefit-Felder speichern und filtern.

### Task: 12.1 Contact-Migration erweitern
**Priority**: P0
**Blocked by**: none
**Agent**: Agent A

**Akzeptanzkriterien**:
- [x] Neue Migration `002_hansefit_admin.up.sql` erweitert `contact_submissions`.
- [x] Felder vorhanden: `playtomic_email`, `hansefit_status`, `hansefit_processed_at`, `hansefit_confirmation_sent_at`.
- [x] `hansefit_status` erlaubt mindestens `unprocessed` und `processed`.
- [x] Bestehende Datensaetze bleiben gueltig.
- [x] Index fuer Admin-Liste vorhanden: Hansefit-Anfragen nach Status/Erstellzeit.
- [x] Down-Migration entfernt nur die neuen Felder/Indizes.

**Kontext**:
- Bestehende Tabelle: `migrations/001_contact_submissions.up.sql`.
- Admin-Liste soll nur `hansefit_registration = true` anzeigen.

**Output**:
- `migrations/002_hansefit_admin.up.sql`
- `migrations/002_hansefit_admin.down.sql`

### Task: 12.2 Contact-Model und Repository anpassen
**Priority**: P0
**Blocked by**: 12.1
**Agent**: Agent C

**Akzeptanzkriterien**:
- [x] `ContactSubmission` enthaelt `PlaytomicEmail`, `HansefitStatus`, `HansefitProcessedAt`, `HansefitConfirmationSentAt`.
- [x] `ContactSubmissionRequest` enthaelt `PlaytomicEmail`.
- [x] Create/Scan-Queries beruecksichtigen neue Felder.
- [x] Repository bietet Admin-Query `ListHansefitRegistrations`.
- [x] Repository bietet Status-Update mit Schutz gegen doppelte Bestaetigung.
- [x] Nicht-Hansefit-Anfragen bleiben speicherbar und werden nicht in der Hansefit-Liste ausgegeben.

**Kontext**:
- `internal/model/contact.go`
- `internal/repository/contact_repo.go`

**Output**:
- Aktualisiertes Contact-Model
- Aktualisiertes Contact-Repository

**Wave-Exit-Gate**:
- [x] Migrationen sind reviewbar.
- [x] Kontaktmodell kann neue Daten speichern.
- [x] Admin-Filter ist technisch vorbereitet.

---

## Wave 2 - Kontaktformular, Hansefit-Logo und Bauphasen-UI
**Agenten**: Agent D, Agent C
**Kann einzeln ausgefuehrt werden**: ja, nach Wave 1 fuer Formularspeicherung
**Ziel**: Besucher koennen korrekte Hansefit-Daten absenden; Bauphasen sind klickbar.

### Task: 12.3 Kontaktformular Hansefit anpassen
**Priority**: P0
**Blocked by**: 12.2
**Agent**: Agent D

**Akzeptanzkriterien**:
- [x] Formular enthaelt Feld `Playtomic-E-Mail-Adresse`.
- [x] Formular enthaelt Checkbox mit Label `Hansefitregistrierung`.
- [x] Nicht angehakte Checkbox blockiert den Submit nicht.
- [x] Bei nicht angehakter Checkbox wird die Anfrage gespeichert und Betreiber-E-Mail versendet, erscheint aber nicht in der Hansefit-Admin-Liste.
- [x] Playtomic-E-Mail wird validiert, wenn Hansefitregistrierung angehakt ist.
- [x] Validierungsfehler erscheinen im bestehenden Fehlerbereich.
- [x] Hansefit-Logo ist sichtbar eingebunden, ohne Layout-Bruch auf Mobile.

**Kontext**:
- Formular: `web/templates/pages/contact.html`
- Handler: `internal/handler/contact.go`
- Logo-Quelle: `logo/hansefit.png`
- Ziel fuer Web-Asset: `web/static/img/hansefit.png`

**Output**:
- Aktualisiertes Kontaktformular
- Kopiertes Hansefit-Logo in Web-Assets
- Angepasster Contact-Handler

### Task: 12.4 Bauphasen-Auswahl auf Startseite
**Priority**: P0
**Blocked by**: none
**Agent**: Agent D

**Akzeptanzkriterien**:
- [x] Startseite zeigt Bauphasen-Auswahl im Baufortschritt-Bereich.
- [x] `Rohbau` ist initial aktiv und steht im Vordergrund.
- [x] Der aktuelle Status behalt die kleine pulsierende Animation.
- [x] Klick auf `Hallenbau` zeigt den Hallenbau-Inhalt.
- [x] Hallenbau verwendet den vorhandenen gestreiften Platzhalter (`.ph`) statt eines Videos.
- [x] Pro Bauphase wird maximal ein Video bzw. ein Platzhalter angezeigt.
- [x] Alle Bauphasen werden mit der Seite geladen; Umschalten erfolgt clientseitig.
- [x] Ohne JavaScript bleibt der aktuelle Baufortschritt sichtbar.
- [x] Desktop und Mobile haben keine Text-/Video-Ueberlappungen.

**Kontext**:
- Startseite: `web/templates/pages/home.html`
- CSS: `web/static/css/style.css`
- JS: `web/static/js/main.js`
- Aktuelles Video: `/static/videos/construction.mp4`

**Output**:
- Aktualisierte Baufortschritt-Section
- CSS fuer aktive/inaktive Bauphase
- Minimaler JS-Toggle fuer Phase-Auswahl

**Wave-Exit-Gate**:
- [x] Kontaktformular kann neue Daten absenden.
- [x] Hansefit-Logo ist eingebunden.
- [x] Bauphasen sind im Browser bedienbar.

---

## Wave 3 - Admin-Auth und Test-Admin
**Agenten**: Agent B, Agent A
**Kann einzeln ausgefuehrt werden**: ja
**Ziel**: Ein geschuetzter Adminbereich ist erreichbar und der Testnutzer existiert.

### Task: 12.5 Admin-User und Auth-MVP
**Priority**: P0
**Blocked by**: none
**Agent**: Agent B

**Akzeptanzkriterien**:
- [x] Admin-Login unter `GET /admin/login`.
- [x] Login-Submit unter `POST /admin/login`.
- [x] Logout unter `POST /admin/logout`.
- [x] Geschuetzte Admin-Routen leiten ohne Session auf Login um.
- [x] CSRF bleibt fuer Admin-Formulare aktiv.
- [x] Session-Cookie ist `HttpOnly`, `SameSite=Lax`, in Production `Secure`.
- [x] Passwortpruefung nutzt bcrypt.
- [x] Fehlertext beim Login verrat nicht, ob User oder Passwort falsch war.
- [x] Maintenance Mode laesst `/admin/*` durch.

**Kontext**:
- `github.com/gorilla/sessions` ist bereits im `go.mod`.
- `golang.org/x/crypto/bcrypt` muss ggf. ergaenzt werden.
- Maintenance-Middleware: `internal/handler/maintenance.go`.

**Output**:
- `internal/handler/admin_auth.go`
- Admin-Login-Template unter `web/templates/admin/`
- Config-/Router-Erweiterung

### Task: 12.6 Test-Admin `PadelBZ` anlegen
**Priority**: P0
**Blocked by**: 12.5
**Agent**: Agent A

**Akzeptanzkriterien**:
- [x] Test-Admin Username: `PadelBZ`.
- [x] Test-Passwort: `PadelBZ`.
- [x] Passwort wird nicht im Klartext gespeichert.
- [x] Seed/Migration oder Config-Mechanismus ist dokumentiert.
- [x] Plan/Doku markiert den Nutzer als Testzugang, der vor Produktion ersetzt werden muss.

**Kontext**:
- Wenn DB-User genutzt werden: separate Tabelle `admin_users`.
- Wenn config-basierter Admin genutzt wird: Env Vars mit bcrypt Hash und lokalem Default nur fuer Development.

**Output**:
- Admin-User-Setup
- Dokumentierter Testzugang

**Wave-Exit-Gate**:
- [x] `/admin/login` ist erreichbar.
- [x] Login mit `PadelBZ`/`PadelBZ` funktioniert lokal.
- [x] Admin-Routen sind ohne Login geschuetzt.

---

## Wave 4 - Hansefit-Admin und Bestaetigungs-E-Mail
**Agenten**: Agent B, Agent C, Agent D
**Kann einzeln ausgefuehrt werden**: ja, nach Wave 1 und Wave 3
**Ziel**: Admins koennen Hansefit-Anfragen sehen und bearbeiten.

### Task: 12.7 Hansefit-Admin-Uebersicht
**Priority**: P0
**Blocked by**: 12.2, 12.5
**Agent**: Agent D + Agent B

**Akzeptanzkriterien**:
- [x] `GET /admin` zeigt Dashboard mit Link zu Hansefit-Anfragen.
- [x] `GET /admin/hansefit` zeigt nur Anfragen mit `hansefit_registration = true`.
- [x] Tabelle ist unterteilt in: Vorname, Nachname, Playtomic-E-Mail, Inhalt vom Freitextfeld.
- [x] Status wird visuell angezeigt: `unbearbeitet` rot, `bearbeitet` gruen.
- [x] Sortierung: unbearbeitet zuerst, dann neueste Anfragen.
- [x] Leerer Zustand ist klar sichtbar.
- [x] Nicht-Hansefit-Kontaktanfragen erscheinen nicht in dieser Uebersicht.

**Kontext**:
- Templates unter `web/templates/admin/`
- Admin-Styles koennen in `web/static/css/style.css` oder separater Admin-CSS liegen.

**Output**:
- `internal/handler/admin_hansefit.go`
- Admin-Dashboard- und Hansefit-Templates

### Task: 12.8 Statuswechsel und Bestaetigungs-E-Mail
**Priority**: P0
**Blocked by**: 12.7
**Agent**: Agent C + Agent B

**Akzeptanzkriterien**:
- [x] Admin kann Status von `unbearbeitet` auf `bearbeitet` setzen.
- [x] Beim erfolgreichen Statuswechsel wird eine Hansefit-Bestaetigung an den Anfragenden gesendet.
- [x] Empfaenger ist Playtomic-E-Mail, falls vorhanden; sonst Kontakt-E-Mail.
- [x] Bestaetigungs-E-Mail enthaelt Vorname, Nachname und Hinweis auf Hansefit-Bestaetigung bei courts padelclub.
- [x] Bei E-Mail-Fehler bleibt die Anfrage sichtbar bearbeitbar oder zeigt klaren Fehler.
- [x] Bereits bearbeitete Anfrage sendet nicht versehentlich erneut.
- [x] Statuswechsel ist CSRF-geschuetzt.
- [x] Aktion ist per POST umgesetzt, nicht per GET.

**Kontext**:
- Bestehender SMTP-Versand in `internal/service/contact_service.go`.
- Bestehende Betreiber-Benachrichtigung darf nicht entfernt werden.

**Output**:
- Hansefit-Service-Methoden
- Status-Handler
- Bestaetigungs-E-Mail-Template oder plain-text Builder

**Wave-Exit-Gate**:
- [x] Admin sieht Hansefit-Anfragen.
- [x] Status rot/gruen funktioniert.
- [x] Bestaetigungs-E-Mail wird genau beim Bearbeiten versendet.

---

## Wave 5 - Integration, Tests und Review
**Agenten**: Agent E, Agent R, PO Lead
**Kann einzeln ausgefuehrt werden**: ja, nach Waves 1-4
**Ziel**: Regressionen, Security-Risiken und Abnahmefaelle sind geprueft.

### Task: 12.9 Tests und manuelle QA
**Priority**: P1
**Blocked by**: 12.3, 12.4, 12.8
**Agent**: Agent E

**Akzeptanzkriterien**:
- [x] `go test ./...` laeuft erfolgreich.
- [x] Kontaktformular-Test: Hansefit angehakt landet in Admin-Liste.
- [x] Kontaktformular-Test: Hansefit nicht angehakt sendet Anfrage, landet nicht in Admin-Liste.
- [x] Playtomic-E-Mail-Validierung ist getestet.
- [x] Admin-Login ist getestet.
- [x] Statuswechsel `unbearbeitet` -> `bearbeitet` ist getestet.
- [x] Doppelte Bestaetigungs-E-Mail wird verhindert.
- [x] Bauphasen-Klick funktioniert auf Desktop und Mobile.
- [x] No-JS-Fallback zeigt aktuellen Baufortschritt.
- [x] Maintenance Mode blockiert Admin nicht.

**Kontext**:
- Bestehende Testabdeckung ist gering; Fokus auf Handler-/Service-Smoke-Tests.

**Output**:
- Neue/aktualisierte Tests
- QA-Status in dieser Datei

### Task: 12.10 Security- und Merge-Review
**Priority**: P1
**Blocked by**: 12.9
**Agent**: Agent R

**Akzeptanzkriterien**:
- [x] Passwortspeicherung ist bcrypt-basiert.
- [x] Testpasswort ist als Produktivrisiko dokumentiert.
- [x] Admin-Routen sind nicht oeffentlich erreichbar.
- [x] CSRF-Schutz fuer Login/Logout/Statuswechsel geprueft.
- [x] Keine personenbezogenen Daten werden unnoetig im Log ausgegeben.
- [x] Bestaetigungs-E-Mail kann nicht durch fremde Requests ausgeloest werden.
- [x] Doku nennt neue Env Vars und Admin-Testzugang.
- [x] Offene Risiken sind dokumentiert.

**Output**:
- Review-Notizen
- Finaler Iterationsstatus

**Wave-Exit-Gate**:
- [x] Tests und Review abgeschlossen.
- [x] Abnahmekriterien sind nachvollziehbar erfuellt oder Rest-Risiken dokumentiert.

### Wave-5-QA-Status

- `go test ./...` erfolgreich via Docker Go 1.23.
- Maintenance/Coming-Soon deaktiviert: App-Startlog zeigt `Maintenance mode: OFF`, Root liefert HTTP 200 und die normale Startseite.
- Maintenance-Guard separat geprueft: mit `MAINTENANCE_MODE=true` liefert Root HTTP 503, `/admin/login` bleibt HTTP 200.
- Formular-QA: fehlende Playtomic-E-Mail bei Hansefit liefert HTTP 422 mit Feldfehler.
- Formular-QA: Hansefit-Anfrage liefert HTTP 303, erscheint in `/admin/hansefit`; normale Anfrage liefert HTTP 303 und erscheint nicht in der Hansefit-Liste.
- Admin-QA: `/admin` ohne Session leitet auf Login; Login mit `PadelBZ`/`PadelBZ` funktioniert; Statuswechsel liefert Success-Redirect; zweiter Statuswechsel liefert Already-Processed-Redirect.
- Bauphasen-QA: Startseite enthaelt `data-phase-switcher`, `Rohbau` und `Hallenbau`; `Hallenbau` ist initial hidden, Toggle-Logik in `web/static/js/main.js` schaltet Tab, Panel und Video-Playback.

### Review-Notizen

- Security: Admin-Statuswechsel ist nur hinter Session + CSRF erreichbar.
- Security: Admin-Passwort liegt nur als bcrypt Hash im Code; `PadelBZ`/`PadelBZ` bleibt ein Produktivrisiko und muss vor Livebetrieb ersetzt werden.
- Security: Keine Formularinhalte, Passwoerter oder Session-Tokens werden aktiv in Logs ausgegeben; Fehlerlogs enthalten nur technische Fehlertexte.
- Offenes Risiko: Lokale Umgebung hat kein SMTP. Bei leerem `SMTP_HOST` wird kein externer Versand ausgefuehrt; produktiv muessen SMTP-Env-Vars gesetzt werden.
- Offenes Risiko: Admin-Login hat noch kein eigenes Brute-Force-Limit. Vor produktivem Adminbetrieb sollte ein separates Login-Rate-Limit ergaenzt werden.

---

## Abhaengigkeits-Graph

```text
Wave 1: DB + Contact Contract
   |--> Wave 2: Formular + Bauphasen UI
   `--> Wave 4: Hansefit Admin

Wave 3: Admin Auth + Test-Admin
   `--> Wave 4: Hansefit Admin

Wave 2 + Wave 4
   `--> Wave 5: Tests + Review
```

## Definition of Done
- [x] Baufortschritt ist in klickbare Bauphasen unterteilt.
- [x] Aktuelle Bauphase `Rohbau` ist beim Seitenaufruf aktiv und behaelt die Status-Animation.
- [x] `Hallenbau` ist als naechste Bauphase anklickbar und nutzt den gestreiften Platzhalter.
- [x] Pro Bauphase wird nur ein Video oder ein Platzhalter angezeigt.
- [x] Hansefit-Logo ist auf der Website eingebunden.
- [x] Kontaktformular enthaelt Playtomic-E-Mail und Checkbox `Hansefitregistrierung`.
- [x] Nicht-Hansefit-Anfragen werden weiter gesendet, aber nicht in der Hansefit-Admin-Uebersicht angezeigt.
- [x] Admin-Login mit `PadelBZ`/`PadelBZ` funktioniert lokal.
- [x] Hansefit-Admin zeigt Vorname, Nachname, Playtomic-E-Mail und Freitext.
- [x] Admin kann Status von rot/unbearbeitet auf gruen/bearbeitet setzen.
- [x] Beim Statuswechsel wird eine Hansefit-Bestaetigung an den Anfragenden versendet.
- [x] Tests und Security-Review sind abgeschlossen.

## Umsetzungsstatus
- Wave 1: done
- Wave 2: done
- Wave 3: done
- Wave 4: done
- Wave 5: done
