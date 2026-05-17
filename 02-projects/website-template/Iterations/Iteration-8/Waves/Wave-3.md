# Wave 3 — Frontend Kontakt und Admin-Anfragen

**Status:** Done
**Skill:** frontend-eng

## Ziel

Kontaktformular und Admin-Anfragen-Seite produktionsnah auf echte Daten
umstellen.

## Tasks

- Kontaktformular Submit-Guard und Fehlertexte haerten.
- Admin-Anfragen-Seite von `INQUIRIES` auf Fetch aus `/admin/api/inquiries`
  umstellen.
- Loading, Empty und Error State bauen.
- Detailauswahl mit `read=true` patchen.
- Statusbuttons mit Backend-PATCH verbinden.
- Summary-/Badge-Refresh nach Read-/Status-Aenderung ausloesen.

## Acceptance Criteria

- [x] Keine statischen Mock-Anfragen in der Admin-Anfragen-Seite.
- [x] Neue Anfrage erscheint nach Staging-Submit im Admin.
- [x] Badge und Seitenzaehler zeigen echte ungelesene Anfragen.
