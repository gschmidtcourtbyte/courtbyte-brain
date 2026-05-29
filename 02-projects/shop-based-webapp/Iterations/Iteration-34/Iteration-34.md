# Iteration 34 - Ladezeiten der OUT-Padel-Web-App reduzieren

**Status:** Done
**Repo:** /home/andersen/git/shop-based-webapp
**Datum:** 2026-05-12
**Rolle:** Acting as Product Owner. Planning Iteration 34 for shop-based-webapp.

## Ziel

Iteration 34 reduziert die realen Ladezeiten der oeffentlichen OUT-Padel-Web-App. Der Fokus liegt auf den gemessenen Bottlenecks: teure `/_next/image`-Erstgenerierung, sehr grosse Originalbilder, kurze Image-Cache-TTLs und vermeidbare clientseitige Arbeit auf oeffentlichen Seiten.

## Messbefund

- `https://padel-out.de/` liefert HTML schnell: ca. 0.16 bis 0.20 Sekunden TTFB.
- `https://backend-api-production-7ae9.up.railway.app/api/v1/camp-events` liegt bei ca. 0.14 Sekunden.
- `https://padel-out.de/api/v1/camp-events` liegt ueber den Web-Proxy bei ca. 0.31 Sekunden.
- Railway-HTTP-Logs der letzten 24h zeigen langsame Requests vor allem auf `/_next/image`, nicht auf Backend-API-Routen.
- Einzelne Originalbilder in `web/customer/public/images/camp` sind 6 bis 20 MB gross.

## Scope

**In:**
- Next-Image-Konfiguration fuer statische Camp-/Founder-Bilder verbessern.
- Grosse Bildquellen webtauglich neu speichern, ohne die verwendeten Dateipfade zu aendern.
- Oeffentliche API-/Camp-Event-Pfade cachefreundlicher machen und Timeouts absichern.
- Oeffentliche Startseite von sichtbarer Auth-Ladearbeit entlasten.
- Build, gezielte Tests und Performance-Smoke ausfuehren.

**Out:**
- Kein CDN-Provider-Wechsel.
- Keine neue Bildverwaltung oder CMS-Migration.
- Kein Redesign der Startseite.
- Keine Datenbankmigration.
- Keine Aenderung am Buchungs- oder Admin-Fachflow.

## Waves

| Wave | Skill | Focus | Status |
|---|---|---|---|
| Wave 1 | frontend-eng | Bildpipeline, Next-Image-Cache und grosse Assets optimieren | Done |
| Wave 2 | service-eng | Oeffentliche API-Pfade, Proxy-Timeouts und Auth-Ladearbeit entlasten | Done |
| Wave 3 | testing-eng | Build, Tests und Performance-Smoke pruefen | Done |
| Wave 4 | review-eng | Performance-, Scope- und Risiko-Review abschliessen | Done |

## Critical Path

```text
Wave 1 (Images)
    v
Wave 2 (Runtime/API)
    v
Wave 3 (Verification)
    v
Wave 4 (Review)
```

## Acceptance Criteria

- [x] Sehr grosse Camp-/Founder-Originalbilder sind deutlich kleiner, ohne sichtbare Pfad- oder UI-Aenderung.
- [x] Next erzeugt keine unnoetigen 3840px-Varianten fuer die aktuelle Bildnutzung.
- [x] `/_next/image` nutzt eine laengere Cache-TTL fuer statische Assets.
- [x] Oeffentliche Camp-Event-Antworten sind kurzzeitig cachebar.
- [x] Der Next-API-Proxy bricht langsame Backend-Calls kontrolliert ab.
- [x] Die oeffentliche Navigation zeigt keinen Auth-Ladeskeleton mehr.
- [x] Customer-Web Build ist gruen.
- [x] Relevante Frontend- und Backend-Tests sind gruen oder konkrete lokale Blocker sind dokumentiert.
- [x] Performance-Smoke zeigt schnelle HTML/API-Timings und keine offensichtliche Regression.
- [x] Scope-Audit bestaetigt: keine Planungsdateien im Code-Repo.

## Notes

Planung erstellt am 2026-05-12 auf Basis der Production-Messungen gegen `padel-out.de` und Railway-HTTP-Logs.

Abgeschlossen am 2026-05-12. Ergebnis: 16 statische Camp-/Founder-Bilder von 63.1 MB auf 4.5 MB reduziert, `minimumCacheTTL` fuer Next-Images auf 7 Tage erhoeht, Bildbreiten auf real genutzte Varianten begrenzt, oeffentliche Camp-Endpoints kurzzeitig cachebar gemacht, Next-Proxy und Browser-API-Client mit Timeouts abgesichert und Auth-Skeleton auf oeffentlichen Seiten entfernt. Verifikation: gezielte Go-/Vitest-Tests gruen, Customer-Build gruen, Production-Smoke ohne offensichtliche Verfuegbarkeitsregression.
