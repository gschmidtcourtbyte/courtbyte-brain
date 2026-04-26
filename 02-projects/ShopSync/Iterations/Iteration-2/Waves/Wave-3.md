# Wave 3 — Products-Dashboard & Sync-Log UI

**Iteration:** [[Iteration-2]]
**Skill:** frontend-eng
**Status:** Todo

## Tasks

- [ ] Products-Tabelle: Spalten Name, Preis, Lagerstand, Status, Letzte Sync — `app/(dashboard)/products/page.tsx`
- [ ] Server Component: Produkte aus DB laden mit Supabase — `app/(dashboard)/products/page.tsx`
- [ ] Pagination der Tabelle (25 pro Seite) — `components/data-table.tsx`
- [ ] Sync-Log-Seite: Tabelle mit Timestamp, Produkt, Status, Fehlermeldung — `app/(dashboard)/logs/page.tsx`
- [ ] Status-Badge-Komponente (success / error / pending) — `components/status-badge.tsx`
- [ ] Ladestate während Import läuft (optimistic UI) — `app/(dashboard)/connections/[id]/page.tsx`

## Done Criteria

- [ ] Products-Tabelle zeigt alle importierten Produkte
- [ ] Sync-Log zeigt Fehler mit lesbarer Fehlermeldung
- [ ] Tabellen-Pagination funktioniert
- [ ] UI friert während Import nicht ein

## Notes

Optimistic UI: Import-Button auf "Importing..." setzen via `useTransition` + Server Action. Kein Polling nötig für MVP — User refresht manuell.
