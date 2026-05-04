# Wave 1 - Admin-User sicher provisionieren

**Iteration:** [[Iteration-31]]
**Skill:** service-eng
**Status:** Done

## Ziel

Das Backend erhaelt einen sicheren, idempotenten Weg, den Admin-User `info@padel-out.de` als aktiven, verifizierten Admin anzulegen oder zu aktualisieren. Das initiale Passwort wird nur zur Laufzeit uebergeben und mit dem bestehenden bcrypt-Hasher verarbeitet.

## Agent

Backend Service Engineer: Fokus auf bestehende Auth-Domain, Repository-Vertraege, sichere Operational Flows, explizite Fehlerbehandlung und stabile Admin-Contracts.

## Tasks

- [x] Bestehenden Auth-/User-Code pruefen: `users`-Schema, UserRepository, AuthService, bcrypt-Hasher und Admin-Middleware.
- [x] Entscheiden, ob ein kleines Operational Command, Seed-Command oder bestehender Admin-Flow der passende Weg ist; keine oeffentliche HTTP-Route fuer Admin-Anlage bauen.
- [x] Provisioning fuer `info@padel-out.de` idempotent implementieren: Create wenn nicht vorhanden, sonst kontrolliertes Update.
- [x] User mit `role = 'admin'`, `status = 'active'` und gesetztem `email_verified_at` speichern.
- [x] Passwort ausschliesslich aus Runtime-Eingabe lesen, z. B. Environment Variable oder nicht geloggter CLI-Parameter.
- [x] Passwort mit dem bestehenden bcrypt-Hasher hashen.
- [x] Sicherstellen, dass weder Klartextpasswort noch statischer Passwort-Hash in Code, Migrationen, Tests, Logs oder Config-Dateien landen.
- [x] Fehlerfaelle sauber behandeln: fehlendes Passwort, ungueltige E-Mail, DB-Fehler, Duplicate/Existing User.
- [x] Zielgerichtete Backend-Tests fuer Idempotenz, Rolle/Status und Secret-Handling ergaenzen.

## Akzeptanzkriterien

- [x] `info@padel-out.de` kann ueber den vorgesehenen Operational Flow angelegt werden.
- [x] Der Account ist direkt loginfaehig: Rolle `admin`, Status `active`, E-Mail verifiziert.
- [x] Existiert der Account bereits, wird kein zweiter User erzeugt.
- [x] Passwort wird bcrypt-gehasht und nicht geloggt.
- [x] Es gibt keine versionierte Datei mit Klartextpasswort oder statischem Passwort-Hash.
- [x] Keine neue oeffentliche API erlaubt Admin-User-Anlage.
- [x] Relevante Go-Tests fuer den neuen Provisioning-Code sind gruen oder Blocker sind dokumentiert.

## Abhaengigkeiten

Keine. Diese Wave ist sicherheitsrelevant und bildet die Grundlage fuer den Admin-Zugang.

## Notes

Das initiale Passwort ist absichtlich nicht in dieser Wave dokumentiert. Der Agent muss es bei der Umsetzung ueber einen sicheren Runtime-Kanal vom Product Owner/Operator verwenden.

Umsetzung 2026-05-04:
- Wiederverwendbaren bcrypt-Adapter unter `internal/auth/adapters/password/bcrypt` angelegt und `cmd/server` darauf umgestellt.
- `AdminProvisioner` in `internal/auth/app` implementiert: E-Mail normalisieren/validieren, Passwortregeln pruefen, bcrypt-Hash erzeugen und aktiven/verifizierten Admin-Datensatz bauen.
- `UserRepository.UpsertAdmin` implementiert: `INSERT ... ON CONFLICT (email) DO UPDATE`, setzt `role = 'admin'`, `status = 'active'`, `email_verified_at` und aktualisiert den Passwort-Hash idempotent.
- Internes Command `cmd/provision-admin` angelegt. Es liest `ADMIN_PASSWORD` zur Laufzeit, nutzt optional `ADMIN_EMAIL` und verbindet per `DATABASE_URL` oder bestehender `DB_*`-Config.
- Keine oeffentliche HTTP-Route fuer Admin-Anlage hinzugefuegt.
- Verifikation gruen: `go test ./internal/auth/...`, `go test ./cmd/provision-admin`, `go test ./cmd/server`.
- Lokale echte DB-Provisionierung nicht ausgefuehrt: `DATABASE_URL` ist in der aktuellen Shell nicht gesetzt. Das Command kann mit `ADMIN_PASSWORD` und DB-Konfiguration ausgefuehrt werden.
