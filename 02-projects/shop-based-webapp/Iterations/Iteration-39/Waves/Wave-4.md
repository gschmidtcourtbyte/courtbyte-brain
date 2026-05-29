# Wave 4 - Railway Staging Rollout

**Iteration:** [[Iteration-39]]
**Skill:** infrastructure-eng
**Status:** Done

## Tasks

- [x] Railway-Ziel auf OUT Padel `staging` verifizieren, bevor Remote-Aktionen laufen.
- [x] Backend und Web-Customer nur nach `staging` deployen, soweit der Iterationsstand dies benoetigt.
- [x] API-/Web-Healthchecks und Camp-/FAQ-Smokes gegen Staging ausfuehren.
- [x] Production bleibt ohne explizite Freigabe unangetastet.
- [x] Rollback-Pfad fuer Code-Deploy und Preis-Snapshot-Auswirkungen dokumentieren.

## Done Criteria

- [x] Staging meldet gesunde Deployments.
- [x] Smokes zeigen FAQ, Instagram-Link und Add-on-Pricing gegen Staging.
- [x] Kein Production-Deploy wurde ausgefuehrt.

## Notes

User-Freigabe am 2026-05-21: fuer diese Iteration vorerst nur Railway Staging verwenden.

Stand 2026-05-21:
- Railway-Browserless-Login wurde erneuert und das Zielprojekt `outpadel` mit Environment `staging` vor Deploy verifiziert.
- Staging-Backend-Deployment `f0d4ddc6-0c4e-40bc-891a-963e64f4db2f` steht auf `SUCCESS`.
- Staging-Web-Deployment `0b11dcf7-c890-4ac3-9b8f-0a9e476cc5f4` steht auf `SUCCESS`.
- Backend-Smoke: `GET /ready` liefert `{"status":"ok"}`; Deploy-Logs zeigen die vorhandene `schema_migrations`-Tabelle ohne Error-Hinweis.
- Web-Smoke: `/healthz` liefert `200`, FAQ-Staging-HTML rendert alle Antworten offen und referenziert das Instagram-Profil `padel.out`.
- Checkout-Smoke: der ausgelieferte Staging-Checkout-Chunk enthaelt `Flughafentransfer`, `Vollpension`, `Einzeltrainerstunden`, Cent-Werte `5000`/`40000`/`3000` und die `Zusatzkosten`-Zusammenfassung.
- Alle Deploy-Aktionen wurden mit explizitem Environment `staging` und Service-Ziel ausgefuehrt. Production blieb unangetastet.
- Rollback fuer den spaeteren Staging-Code-Deploy: vorheriges gesundes Backend- und Web-Deployment in Railway erneut aktivieren/deployen. Neue Buchungen behalten ihre gespeicherten Preis-Snapshots; ein Rollback der Preis-Konstanten wirkt nur auf nachfolgende Buchungen.
- Ad-hoc-Web-Rollout am 2026-05-21: Deployment `89660ea1-2f4b-4680-a14f-d6897bd47f4f` nur fuer `web-customer-frontend` nach `staging`; `/healthz` blieb gruen und der ausgelieferte Homepage-Chunk enthaelt `Weitere Auswahl`, aber nicht mehr `Initial priorisiert`.
