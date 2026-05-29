# Wave 3 — API Handler

**Status:** Done
**Skill:** handler-eng

## Ziel

Admin- und Public-API fuer die neuen Backend-Faehigkeiten anbinden.

## Ergebnis

- `GET /api/admin/summary` ergaenzt.
- Public Multipart-Handler akzeptiert bis zu 10 Anhaenge mit 10 MB Gesamtlimit.
- Bestehende Admin-Download-Routen bleiben je Attachment nutzbar.
- Handler-Tests fuer Summary und Statuswechsel laufen.

## Akzeptanzkriterien

- [x] Summary ist admin-geschuetzt.
- [x] Attachment-Downloads bleiben auth-geschuetzt.
- [x] Ungueltige Uploads werden weiterhin sauber gemappt.
