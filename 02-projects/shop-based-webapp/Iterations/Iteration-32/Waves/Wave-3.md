# Wave 3 - Content-Abgleich und Scope-Review

**Iteration:** [[Iteration-32]]
**Skill:** review-eng
**Status:** Done

## Ziel

Die Iteration wird mit einem expliziten Review abgeschlossen: Die Website-Texte stimmen mit den Kundenvorlagen ueberein, die Darstellung ist plausibel und der technische Scope bleibt auf Customer-Web-Legal-Content begrenzt.

## Agent

Review Engineer: Fokus auf risikoorientierten Content-Abgleich, Vollstaendigkeit, unerwuenschte Nebeneffekte, Test-Gaps und Merge-Readiness.

## Tasks

- [x] Diff pruefen und bestaetigen, dass nur erwartete Customer-Web-Legal-/Content-Dateien geaendert wurden.
- [x] Datenschutzerklaerung Abschnitt fuer Abschnitt gegen die Kundenvorlage abgleichen.
- [x] AGB Paragraf fuer Paragraf gegen die erneuerte Kundenvorlage abgleichen.
- [x] Pruefen, dass keine vorherige AGB-Version oder alte Kontaktdaten weiter sichtbar sind.
- [x] Pruefen, dass keine juristische Eigenkorrektur den Kundeninhalt veraendert hat.
- [x] Darstellung auf Lesbarkeit und semantische Struktur pruefen: Ueberschriften, Listen, Abschnitte, Links.
- [x] Testergebnisse aus Wave 2 bewerten und offene Test-Gaps dokumentieren.
- [x] Scope-Audit ausfuehren: keine Planungsdateien im Code-Repo, keine Backend-/DB-/API-Aenderungen.
- [x] Abschlussnotes in Iteration- und Wave-Dateien ergaenzen und erledigte Tasks abhaken.

## Akzeptanzkriterien

- [x] Datenschutz enthaelt alle 10 Abschnitte der Kundenvorlage.
- [x] AGB enthalten alle 12 Paragrafen der erneuerten Kundenvorlage.
- [x] `info@padel-out.de` ist die sichtbare Kontaktadresse in Datenschutz und AGB.
- [x] Es gibt keinen sichtbaren Ruecktrittshinweis auf `hellopadelout@gmail.com`.
- [x] AGB § 4.2 enthaelt keinen Standard-Flughafentransfer.
- [x] AGB § 4.3 enthaelt den optional buchbaren Transfer Flughafen `el Prat` in Barcelona - Hotel und zurueck.
- [x] Review nennt keine offenen P0/P1-Blocker.
- [x] Bekannte Rest-Risiken oder Test-Gaps sind konkret dokumentiert.
- [x] Scope-Audit bestaetigt: Planung liegt nur in der Vault.

## Abhaengigkeiten

Wave 1 und Wave 2 muessen abgeschlossen sein.

## Notes

Der Review ist kein Legal Review. Er prueft technische und redaktionelle Uebernahmequalitaet gegen die vom Kunden gelieferte Source of Truth.

Umsetzung 2026-05-05:
- Diff-/Scope-Review abgeschlossen: geaendert wurden `web/customer/src/app/agb/page.tsx`, `web/customer/src/app/datenschutz/page.tsx`, neu hinzugekommen sind `web/customer/src/lib/legal-content.ts` und `web/customer/src/lib/legal-content.test.ts`.
- Datenschutz gegen Kundenvorlage abgeglichen: 10 Abschnitte, Stand April 2026, Kontakt, Real Decreto 933/2021, Versicherer, BfDI und EU/EWR-Formulierung enthalten.
- AGB gegen erneuerte Kundenvorlage abgeglichen: 12 Paragrafen, Schreibweise `Padel-Out`, `info@padel-out.de`, kein Standard-Flughafentransfer in § 4.2, optionaler Flughafentransfer in § 4.3.
- Suche in Legal-Dateien ohne Treffer fuer alte AGB-Details: `hellopadelout@gmail.com`, `padel.out`, `Transfer vom und zum Flughafen Barcelona`, `15 Stunden intensives Padel-Training`, `Reiseruecktrittsversicherung`, `Reisemanngel`, `Umstaende`.
- Review-Finding: keine offenen P0/P1-Blocker. Rest-Risiko: keine juristische Pruefung; Umsetzung ist technischer und redaktioneller Abgleich gegen Kundentext.
- `git diff --check` ohne Whitespace-Fehler.
- Scope-Audit: keine Iteration-32-Planungsdateien im Code-Repo gefunden; keine Backend-/DB-/API-Aenderungen.
