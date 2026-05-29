# Wave 1 - Camp-Booking-Schema fuer Add-ons und Zahlungsstatus

**Iteration:** [[Iteration-38]]
**Skill:** migration-eng
**Status:** Done

## Tasks

- [x] Bestehende Camp-Booking-Migrationen und Constraints pruefen, insbesondere `migrations/038_create_camp_bookings.up.sql`, `migrations/042_alter_camp_bookings_status_default_pending.up.sql`, `migrations/046_alter_camp_bookings_add_inquiry_metadata.up.sql` und `migrations/047_alter_camp_bookings_add_package_interest_slugs.up.sql`.
- [x] Neue Migration `migrations/049_alter_camp_bookings_add_addons_and_payment_status.up.sql` anlegen.
- [x] Down-Migration `migrations/049_alter_camp_bookings_add_addons_and_payment_status.down.sql` anlegen.
- [x] Add-on-Auswahl additiv an `camp_bookings` ergaenzen:
  - `airport_transfer_requested BOOLEAN NOT NULL DEFAULT false`
  - `full_board_requested BOOLEAN NOT NULL DEFAULT false`
  - `private_training_hours INTEGER NOT NULL DEFAULT 0`
- [x] Preis-Snapshot-Felder ergaenzen und aus bestehenden Daten backfillen:
  - `base_price_cents`
  - `airport_transfer_price_cents`
  - `full_board_price_cents`
  - `private_training_price_cents`
  - `total_price_cents`
  - `deposit_amount_cents`
  - `balance_amount_cents`
- [x] Check-Constraints fuer nicht-negative Stunden und nicht-negative Cent-Betraege hinzufuegen.
- [x] Status-Constraint so erweitern, dass `deposit_paid` und `balance_requested` erlaubt sind und bestehende Status erhalten bleiben.
- [x] Kommentare an neuen Spalten hinterlegen, damit Add-on-/Zahlungssemantik im Schema nachvollziehbar ist.
- [x] Locking Impact im Migration Header dokumentieren; additive Spalten mit Defaults und Constraint-Anpassung bewusst einschaetzen.

## Done Criteria

- [x] Up- und Down-Migration sind vollstaendig, reversibel und versioniert.
- [x] Bestehende Buchungen erhalten sinnvolle Defaults: keine Add-ons, `total_price_cents = price_cents`, `deposit_amount_cents = ceil(price_cents * 20 / 100)`, `balance_amount_cents = total - deposit`.
- [x] Neue Statuswerte koennen gespeichert werden, ohne bestehende Status zu brechen.
- [x] Keine direkte manuelle DB-Aenderung ausserhalb der Migration ist vorgesehen.

## Notes

Diese Wave legt die gemeinsame Grundlage fuer Backend, API und Frontend. Wenn PostgreSQL integer arithmetic genutzt wird, die 20-Prozent-Anzahlung bewusst auf volle Cent aufrunden, damit die Summe nicht unterfordert wird.

Abgeschlossen am 2026-05-18. Angelegt wurden `migrations/049_alter_camp_bookings_add_addons_and_payment_status.up.sql` und `.down.sql`. Die Up-Migration fuegt optionale Add-ons, Preis-Snapshot-Felder, nicht-negative Check-Constraints, Komponenten-/Zahlungsbetrags-Checks und die erweiterten Statuswerte `deposit_paid` sowie `balance_requested` hinzu. Bestehende Buchungen werden mit `total_price_cents = price_cents`, `deposit_amount_cents = (price_cents + 4) / 5` und `balance_amount_cents = price_cents - deposit` gebackfillt. Die Down-Migration mappt neue Zwischenstatus vor Rueckbau auf `confirmed` und stellt den bisherigen Status-Constraint wieder her. Kein Remote-DB-Zugriff und keine manuelle Schema-Aenderung.
