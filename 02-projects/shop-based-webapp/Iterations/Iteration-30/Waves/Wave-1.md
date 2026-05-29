# Wave 1 - Camp-Kapazitaetsfelder migrieren

**Iteration:** [[Iteration-30]]
**Skill:** migration-eng
**Status:** Done

## Ziel

Die Datenbank erhaelt additive, optionale Kapazitaetsfelder fuer Camp-Termine. Admins sollen spaeter freie und gesamte Plaetze pflegen koennen, ohne bestehende Camp-Daten, Public Queries oder Booking-Flows zu brechen.

## Agent

Migration Engineer: Fokus auf sichere Schema-Evolution, Up-/Down-Paar, Constraints, Lock-Impact und Rueckwaertskompatibilitaet.

## Tasks

- [x] Aktuellen Stand der Camp-Event-Migrationen in `migrations/039*` bis `migrations/047*` pruefen und naechste freie Migrationsnummer bestimmen.
- [x] Up-Migration fuer `camp_events` anlegen: optionale Integer-Spalten fuer freie Plaetze und Gesamtplaetze addieren.
- [x] DB-Constraints definieren: Werte duerfen `NULL` sein, gepflegte Werte duerfen nicht negativ sein, freie Plaetze duerfen Gesamtplaetze nicht ueberschreiten.
- [x] Down-Migration anlegen, die die neuen Spalten und Constraints sauber entfernt.
- [x] Kommentare an den neuen Spalten ergaenzen, damit klar ist, dass es manuelle redaktionelle Kapazitaetswerte sind.
- [x] Lock-Impact im Migrationsheader dokumentieren.
- [x] Pruefen, dass keine bestehende Migration veraendert wird.

## Akzeptanzkriterien

- [x] Es gibt ein konsistentes Up-/Down-Migrationspaar fuer die neuen `camp_events`-Kapazitaetsfelder.
- [x] Migration ist additiv und rueckwaertskompatibel: bestehende Camp Events bleiben gueltig.
- [x] `NULL` ist fuer beide Felder erlaubt.
- [x] Negative Kapazitaetswerte werden durch DB-Constraints verhindert.
- [x] Gepflegte freie Plaetze groesser als gepflegte Gesamtplaetze werden durch DB-Constraints verhindert.
- [x] Bestehende Tabellen, Indizes und Camp-Booking-Schema bleiben unangetastet.

## Abhaengigkeiten

Keine. Diese Wave bildet die Grundlage fuer Backend-, API- und Frontend-Arbeit.

## Notes

Empfohlene Feldnamen fuer die Umsetzung: `capacity_available` und `capacity_total`. Falls beim Implementieren ein bestehendes Naming-Pattern klar dagegen spricht, soll der Agent die lokale Konvention bevorzugen und die Abweichung in den Notes dokumentieren.

Umsetzung 2026-04-27:
- Migrationspaar `048_alter_camp_events_add_capacity_fields` angelegt.
- Neue nullable Integer-Spalten: `capacity_available` und `capacity_total`.
- Constraints ergaenzt: `camp_events_capacity_non_negative` und `camp_events_capacity_available_lte_total`.
- Constraints werden `NOT VALID` registriert und danach validiert; bestehende Null-Werte bleiben gueltig.
- Spaltenkommentare dokumentieren manuelle redaktionelle Kapazitaetswerte.
- Down-Migration entfernt Constraints und Spalten.
- Bestehende Migrationen 039 bis 047 wurden nicht veraendert.
