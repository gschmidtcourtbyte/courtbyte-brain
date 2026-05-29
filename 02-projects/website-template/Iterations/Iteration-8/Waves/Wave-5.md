# Wave 5 — Railway-Staging-Smoke

**Status:** Done
**Skill:** infrastructure-eng, testing-eng

## Ziel

Die echte Kontaktstrecke auf Railway Staging pruefen.

## Smoke-Sequenz

1. Backend- und Frontend-Healthcheck pruefen.
2. Kontaktformular ueber Staging-Frontend absenden.
3. Admin einloggen.
4. `/admin/anfragen` oeffnen und neue Anfrage sehen.
5. Anfrage auswaehlen; Read-State wird geschrieben.
6. Sidebar-Badge fuer Anfragen sinkt.
7. Status auf `In Bearbeitung` setzen, Reload, Status bleibt erhalten.

## Acceptance Criteria

- [x] Kein `service_unavailable` oder `forbidden_origin`.
- [x] Kontaktanfrage landet in PostgreSQL.
- [x] Admin-UI zeigt Backenddaten.
