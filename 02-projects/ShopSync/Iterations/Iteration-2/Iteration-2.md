# Iteration 2 — Core: Import-Pipeline & Mapping

**Status:** In Progress
**Repo:** /path/to/shopsync
**Datum:** 2026-04-15

## Ziel

Die eigentliche Kernfunktion: Produkte aus Shopify importieren, einem Mapping unterziehen und im Dashboard anzeigen. Am Ende kann ein User eine Shopify-Connection anlegen, Produkte importieren und die gemappten Daten im Dashboard sehen.

## Scope

**In:**
- Connection-Setup UI (Shopify API-Key eingeben, speichern)
- Shopify Admin API: Produkte paginiert abrufen
- Produkt-Mapping (Shopify-Felder → ShopSync-Schema)
- Import-Job: Produkte in DB schreiben
- Products-Tabelle im Dashboard mit Status-Anzeige
- Fehlerbehandlung & Logging in `sync_log`

**Out:**
- WooCommerce-Export (Wave kommt in Iteration 3)
- Hintergrund-Jobs / Cron (manueller Import reicht für MVP)
- Bild-Sync

## Waves

| Wave | Skill | Focus | Status |
|---|---|---|---|
| Wave 1 | handler-eng | Connection-Setup & Shopify-Client | In Progress |
| Wave 2 | service-eng | Import-Pipeline & DB-Write | Todo |
| Wave 3 | frontend-eng | Products-Dashboard & Sync-Log UI | Todo |

## Acceptance Criteria

- [ ] User kann Shopify-Connection anlegen und testen
- [ ] "Import starten" Button triggert Import aller Produkte
- [ ] Importierte Produkte erscheinen in der Tabelle
- [ ] Fehlerhafte Produkte sind in sync_log sichtbar
- [ ] Alle API-Keys werden verschlüsselt gespeichert
