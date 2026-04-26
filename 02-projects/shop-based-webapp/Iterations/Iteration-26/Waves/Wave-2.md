# Wave 2 - Persistenz fuer mehrere Paketinteressen

**Iteration:** [[Iteration-26]]
**Skill:** migration-eng
**Status:** Todo

## Ziel

Das Booking-Datenmodell kann mehrere Paketinteressen additiv speichern, ohne bestehende Buchungen oder das bestehende Primaerpaket zu brechen.

## Tasks

- [ ] Neue SQL-Migration unter `migrations/` erstellen, z. B. `047_alter_camp_bookings_add_package_interests`.
- [ ] Additive nullable Spalte fuer Paketinteressen einfuehren, bevorzugt `package_interest_slugs TEXT[] NULL` oder eine lokal konsistente Alternative.
- [ ] Down-Migration schreiben.
- [ ] Kommentar zur Spalte ergaenzen.
- [ ] Locking-Impact in der Migration dokumentieren.

## Akzeptanzkriterien

- [ ] Migration ist additiv und bricht bestehende `camp_bookings` nicht.
- [ ] Bestehendes `package_slug` bleibt erhalten.
- [ ] Altbuchungen ohne Paketinteressen koennen weiter gelesen werden.
- [ ] Up- und Down-Migration sind vorhanden.
- [ ] Keine Datenmigration ist fuer bestehende Datensaetze zwingend erforderlich.

## Abhaengigkeiten

Keine. Muss vor Service-/Repository-Erweiterung abgeschlossen sein.

## Notes

Rueckwaertskompatibilitaet ist wichtiger als ein perfektes neues Modell. `package_slug` bleibt Primaerpaket/Snapshot; die neue Liste beschreibt zusaetzliches Interesse.
