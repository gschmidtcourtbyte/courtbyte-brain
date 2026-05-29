# Wave 4 — Frontend UX

**Status:** Done
**Skill:** frontend-eng

## Ziel

Bewerbungsformular und Admin-Menue so anpassen, dass Nutzer klares Feedback und
echte Daten sehen.

## Ergebnis

- Formular schliesst nach erfolgreichem Versand.
- Persistente, sichtbare Bestaetigung "Bewerbung eingegangen".
- Drag-and-Drop-Upload mit Dateiliste, Groessenanzeige und Entfernen einzelner
  Dateien.
- Admin-Sidebar ruft `/admin/api/summary` ab.
- Statische Mock-Badges fuer Anfragen/Karriere entfernt.

## Akzeptanzkriterien

- [x] Bewerber kann nicht versehentlich mehrfach senden, waehrend Upload laeuft.
- [x] Erfolgsmeldung bleibt sichtbar und ist nicht nur kurz eingeblendet.
- [x] Karriere-Badge entspricht echten neuen Bewerbungen.
