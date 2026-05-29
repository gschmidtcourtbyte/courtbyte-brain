# Iteration 31 - Admin-Zugang und Buchungs-FAQ

**Status:** Done
**Repo:** /home/andersen/git/shop-based-webapp
**Datum:** 2026-05-04
**Rolle:** Acting as Product Owner. Planning Iteration 31 for shop-based-webapp.

## Ziel

Iteration 31 stellt einen produktionsfaehigen Admin-Zugang fuer OUT Padel bereit und ergaenzt die FAQ um die konkrete Antwort auf "Wie buche ich bei euch?". Der Admin-Zugang muss im bestehenden Auth-/User-Modell sauber als aktiver, verifizierter Admin-User angelegt werden. Das initiale Passwort wird nicht in Git, Migrationen, Vault-Dateien, Logs oder Tests festgehalten.

## Kundenfeedback

1. Im Backend wird ein Admin User fuer `info@padel-out.de` benoetigt.
2. Fuer den Admin User soll ein sicheres initiales Passwort mit 12 Zeichen, einem Sonderzeichen und einer Zahl verwendet werden.
3. Im FAQ soll der Punkt "Wie buche ich bei euch?" ergaenzt werden.
4. Die FAQ-Antwort soll Anfrageformular, WhatsApp, E-Mail, persoenliche Rueckmeldung, Buchungsbestaetigung inklusive Sicherungsschein, 20 Prozent Anzahlung und Telefonnummer enthalten.

## Scope

**In:**
- Bestehenden Auth-/User-Mechanismus fuer Admin-User nutzen: `users.role = 'admin'`, `status = 'active'`, `email_verified_at` gesetzt.
- Admin-User `info@padel-out.de` idempotent anlegen oder aktualisieren.
- Passwort ausschliesslich ueber einen sicheren Runtime-Kanal uebergeben, z. B. Environment Variable oder einmaliges lokales Seed-/Admin-Command.
- Passwort mit bestehendem bcrypt-Hasher hashen.
- Kein Klartextpasswort und kein aus dem Klartext abgeleiteter statischer Hash in Repo- oder Vault-Dateien.
- FAQ-Content in der bestehenden Customer-Web-Quelle `web/customer/src/lib/faq-content.ts` ergaenzen.
- Relevante Backend- und Frontend-Tests/Checks ausfuehren.

**Out:**
- Kein neues Admin-Rollenmodell.
- Keine Aenderung der Login-Session- oder CSRF-Mechanik.
- Keine manuelle DB-Schema-Aenderung ausserhalb versionierter Migrationen, falls eine Migration doch notwendig wird.
- Keine Speicherung des generierten Passworts in Planung, Code, Tests, Logs oder Deployment-Konfiguration.
- Kein Umbau der FAQ-Seite ausser dem neuen Eintrag.
- Keine neuen Buchungs- oder Payment-Funktionen.

## Produktentscheidungen

1. **Bestehendes Rollenmodell verwenden.**
   Das Projekt hat bereits `users.role` mit den Werten `user` und `admin`. Der neue Zugang wird darueber abgebildet.
2. **Aktiver, verifizierter Admin.**
   Der Account muss direkt loginfaehig sein: `status = 'active'` und `email_verified_at` gesetzt.
3. **Passwort nie versionieren.**
   Das initiale Passwort wird nur ausserhalb der Planungs- und Code-Artefakte kommuniziert und bei der Umsetzung als Secret/Runtime-Eingabe verarbeitet.
4. **Idempotente Anlage.**
   Mehrfaches Ausfuehren darf keinen zweiten Nutzer erzeugen. Wenn `info@padel-out.de` existiert, soll Rolle/Status/Passwort kontrolliert aktualisiert werden.
5. **FAQ bleibt Content-Aenderung.**
   Der FAQ-Eintrag nutzt die vorhandene Content-Struktur und erzeugt keine neue CMS-/API-Abhaengigkeit.

## Bestandsaufnahme

| Bereich | Datei | Relevanter Kontext |
|---|---|---|
| User Schema | `migrations/001_create_users.up.sql` | `users` enthaelt `email`, `password_hash`, `email_verified_at`, `status` und `role`; Rolle ist auf `user`/`admin` beschraenkt. |
| Auth Hasher | `cmd/server/runtime_adapters.go` | Bestehender Runtime-Adapter nutzt bcrypt fuer Passwort-Hashes. |
| Auth Service | `internal/auth/app/service.go` | Login verlangt verifizierte E-Mail und aktiven Status. Signup erstellt normale User, nicht Admins. |
| User Repository | `internal/auth/adapters/repository/postgres/user_repository.go` | Repository kann User erstellen, lesen, Passwort aktualisieren und Status setzen. Eine Admin-Provisioning-Route/CLI existiert noch nicht sichtbar. |
| Admin Guard | `pkg/middleware/require_admin_test.go`, `cmd/server/main.go` | Admin-Endpunkte verlassen sich auf authentifizierte Principals mit Rolle `admin`. |
| FAQ Content | `web/customer/src/lib/faq-content.ts` | FAQ-Eintraege sind statischer Customer-Web-Content in `FAQ_ENTRIES`. |

## Waves

| Wave | Skill | Agent | Focus | Status |
|---|---|---|---|---|
| Wave 1 | service-eng | Backend Service Engineer | Sichere, idempotente Admin-User-Provisionierung umsetzen | Done |
| Wave 2 | frontend-eng | Frontend Engineer | FAQ-Eintrag "Wie buche ich bei euch?" ergaenzen | Done |
| Wave 3 | testing-eng | QA Engineer | Auth-/FAQ-Regression, Build/Lint und Secret-Audit | Done |

## Critical Path

```text
Wave 1 (Admin-User-Provisionierung)
    v
Wave 2 (FAQ-Content)
    v
Wave 3 (Regression und Secret-Audit)
```

Wave 1 ist sicherheitsrelevant und muss zuerst klaeren, wie der User ohne versioniertes Passwort angelegt wird. Wave 2 ist funktional unabhaengig, bleibt aber sequenziell, weil der Scope klein ist. Wave 3 prueft beide Aenderungen gemeinsam und fuehrt den finalen Secret-Audit aus.

## Risiken und Mitigationen

| Risiko | Impact | Mitigation |
|---|---|---|
| Klartextpasswort oder Hash wird versehentlich versioniert | Hoch | Passwort nur via Runtime Secret; finaler `rg`-/Diff-Audit auf Passwortfragmente, Hashes und E-Mail-Kontext. |
| Account ist angelegt, aber nicht loginfaehig | Hoch | Akzeptanzkriterien verlangen `status = active`, gesetztes `email_verified_at` und Rolle `admin`; Login-Pfad gezielt pruefen. |
| Mehrfaches Seed/Provisioning erzeugt Duplikate | Mittel | Idempotente Operation per E-Mail/Unique Constraint und Update-on-existing. |
| Admin-Provisioning wird als dauerhafte unsichere Hintertuer implementiert | Hoch | Nur explizites Operational Command oder kontrollierter Seed mit Secret-Eingabe; keine oeffentliche HTTP-Route. |
| FAQ-Text bricht Layout oder liest sich inkonsistent | Mittel | Bestehende FAQ-Komponente und Content-Struktur verwenden; Text responsiv/buildseitig pruefen. |
| Telefonnummer oder Versicherungsformulierung wird falsch uebernommen | Mittel | FAQ-Akzeptanzkriterien enthalten Pflichtinhalte und Zieltext. |

## Gesamt-Akzeptanzkriterien

- [x] `info@padel-out.de` kann als Admin im bestehenden Auth-System angelegt werden.
- [x] Der User hat Rolle `admin`, Status `active` und eine gesetzte `email_verified_at`.
- [x] Das initiale Passwort wird mit dem bestehenden bcrypt-Hasher verarbeitet.
- [x] Kein Klartextpasswort und kein statischer Passwort-Hash werden in Repo, Vault, Tests, Logs oder Deployment-Dateien gespeichert.
- [x] Provisioning ist idempotent: erneutes Ausfuehren erzeugt keinen zweiten User.
- [x] Bestehende Login- und Admin-Middleware-Verhalten bleiben stabil.
- [x] FAQ enthaelt die Frage `Wie buche ich bei euch?`.
- [x] FAQ-Antwort nennt "Anfrage senden", WhatsApp, E-Mail, persoenliche Rueckmeldung, Buchungsbestaetigung inklusive Sicherungsschein der Insolvenz-Versicherung, 20 Prozent Anzahlung und `0155-67149871`.
- [x] FAQ-Seite baut ohne TypeScript-/Lint-Fehler.
- [x] Relevante Backend-Tests sind gruen oder konkrete Blocker sind dokumentiert.
- [x] Relevante Customer-Web Tests, Build und Lint sind gruen oder konkrete Blocker sind dokumentiert.
- [x] Finaler Scope-Audit bestaetigt: keine Planungsdateien im Code-Repo, keine Secrets, keine ungewollten Auth-/FAQ-Nebeneffekte.

## Notes

Planung erstellt am 2026-05-04.

Das initiale Passwort wurde ausserhalb dieser Datei an den Product Owner kommuniziert. Es darf bei der Umsetzung nicht in Dateien, Commits oder Logs erscheinen.

Abschluss 2026-05-04:
- Wave 1 bis Wave 3 abgeschlossen.
- Admin-Provisioning erfolgt ueber internes Command `cmd/provision-admin`, nicht ueber eine oeffentliche HTTP-Route.
- Das Command liest `ADMIN_PASSWORD` zur Laufzeit, nutzt optional `ADMIN_EMAIL` und schreibt `role = 'admin'`, `status = 'active'`, `email_verified_at` und bcrypt-Hash idempotent per `ON CONFLICT (email) DO UPDATE`.
- Echte lokale DB-Provisionierung wurde nicht ausgefuehrt, weil in der aktuellen Shell keine `DATABASE_URL` gesetzt ist. Die Umsetzung ist dafuer vorbereitet und die fehlende Runtime-DB ist der dokumentierte Operational-Blocker.
- FAQ-Eintrag `Wie buche ich bei euch?` in `web/customer/src/lib/faq-content.ts` ergaenzt und per `faq-content.test.ts` abgesichert.
- Verifikation gruen: `go test ./...`, `npm test -- --configLoader runner`, `npm run build`, `npm run lint`, `git diff --check`.
- Secret-Audit: kein Treffer fuer das generierte Passwort, kein statischer bcrypt-Hash in den geaenderten Artefakten.
- Scope-Audit: keine Iteration-/Wave-Planungsdateien im Code-Repo.
