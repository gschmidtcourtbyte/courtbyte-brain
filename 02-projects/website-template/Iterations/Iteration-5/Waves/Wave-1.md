# Wave 1 — Schema für Karriereportal, Jobs und Retention

**Skill:** `/migration-eng` + `/database-eng`
**Status:** Done (2026-05-14) — Schema implementiert, Migration-Embed-Tests grün; runtime DB-Smoke gegen Railway bleibt im operativen Betrieb (Wave 6).
**Blocked by:** Iteration 4 fachlich abgeschlossen

## Ziel

Das Datenmodell so erweitern, dass Uploads, Ablehnungs-Retention und echte
Stellenverwaltung stabil implementiert werden können.

## Tasks

1. Migration für `career_jobs`.
2. `career_applications` um Rejection-/Retention-Felder erweitern:
   - `rejected_at`
   - `delete_after`
   - `deletion_marked_at`
   - `rejection_email_sent_at`
3. Sinnvolle Constraints:
   - `delete_after >= rejected_at`
   - Retention-Felder nur bei `status = 'rejected'`
4. Indexe:
   - `career_jobs (is_active, created_at desc)`
   - `career_applications (delete_after)` für Cleanup-Job
5. Migration-Tests und lokale DB-Smokes ergänzen.

## Akzeptanzkriterien

- [x] Schema unterstützt alle Folge-Waves ohne weitere Grundsatzänderung.
- [x] Rejection-/Retention-Konsistenz ist per Constraint abgesichert.
- [x] Cleanup-Job kann fällige Bewerbungen effizient selektieren.
- [x] `go test ./...` deckt Migration-Embed sauber ab.

## Umsetzung 2026-05-13

- `backend/internal/migrations/sql/003_career_portal_retention.up.sql`
  ergänzt:
  - `career_jobs`
  - `rejected_at`
  - `delete_after`
  - `deletion_marked_at`
  - `rejection_email_sent_at`
  - Retention-Constraints und Delete-After-Index
- `backend/internal/migrations/migrations_test.go` erweitert.

## Verifikation 2026-05-13

- `go test ./...` — clean
- Laufzeit-Smoke gegen Railway-/lokale PostgreSQL-DB bleibt für Wave 6 bzw.
  die nächste DB-nahe Implementierungswelle offen.
