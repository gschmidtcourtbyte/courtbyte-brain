# Iteration 32 - Rechtstexte fuer OUT Padel aktualisieren

**Status:** Done
**Repo:** /home/andersen/git/shop-based-webapp
**Datum:** 2026-05-05
**Rolle:** Acting as Product Owner. Planning Iteration 32 for shop-based-webapp.

## Ziel

Iteration 32 aktualisiert die oeffentlich sichtbaren Rechtstexte der OUT Padel Customer-Web-App. Die Datenschutzerklaerung und die Allgemeinen Geschaeftsbedingungen werden durch die vom Kunden gelieferten Fassungen ersetzt. Der fachliche Fokus liegt auf korrekter Uebernahme, bestehender Seitenstruktur, sauberer Darstellung und technischer Build-Faehigkeit.

## Kundenfeedback

1. Die Datenschutzerklaerung soll durch die neue Fassung `Padel-Out Datenschutzerklaerung gemaess Art. 13 und 14 DSGVO | Stand: April 2026` ersetzt werden.
2. Die AGB sollen durch die erneuerte Kundenfassung `Padel-Out Allgemeine Geschaeftsbedingungen` ersetzt werden.
3. Die erneuerte AGB-Fassung ist massgeblich und ersetzt die zuvor gelieferte AGB-Version.
4. Die Rechtstexte sollen als Website-Content sichtbar sein, ohne neue Backend-, Datenbank- oder Admin-Funktionen.

## Scope

**In:**
- Bestehende Legal-/Content-Struktur in `web/customer` fuer Datenschutz und AGB identifizieren.
- Datenschutzerklaerung mit allen 10 Abschnitten aus der gelieferten Kundenfassung aktualisieren.
- AGB mit allen Paragrafen `§ 1` bis `§ 12` aus der erneuerten Kundenfassung aktualisieren.
- Bestehende Routen, Navigation/Footer-Links und Rendering-Konventionen erhalten.
- Textformatierung fuer Lesbarkeit auf Desktop und Mobile sicherstellen.
- Relevante Customer-Web Tests, Build und Lint ausfuehren.
- Content-Abgleich gegen die gelieferten Quelltexte durchfuehren.

**Out:**
- Keine juristische Beratung oder eigenstaendige rechtliche Umformulierung.
- Keine Backend-, Datenbank-, API- oder Admin-Aenderungen.
- Kein neues CMS-Modell.
- Kein neuer Consent-, Cookie- oder Buchungsflow.
- Keine Planung im Code-Repo.

## Produktentscheidungen

1. **Kundentext ist Source of Truth.**
   Die Umsetzung uebernimmt die gelieferte Kundenfassung inhaltlich. Offensichtliche technische Formatierung ist erlaubt; juristische oder fachliche Aenderungen sind nicht Teil der Umsetzung.
2. **AGB-Fassung vom 2026-05-05 ist massgeblich.**
   Die vom Kunden nachgereichte AGB-Version ersetzt die vorherige Version. Insbesondere ist `info@padel-out.de` die Ruecktritts-E-Mail in § 6.1 und der Flughafentransfer ist unter nicht enthaltenen Leistungen optional buchbar.
3. **Bestehende Legal-Seiten verwenden.**
   Rechtstexte werden in die vorhandene Customer-Web-Struktur eingebettet. Es wird kein neues Content-System eingefuehrt.
4. **Technischer Review statt Rechtspruefung.**
   Review prueft Vollstaendigkeit, Rendering, Links, Build und Scope. Eine rechtliche Bewertung bleibt ausserhalb dieser Iteration.

## Bestandsaufnahme

| Bereich | Datei | Erwarteter Kontext |
|---|---|---|
| Customer-Web Legal Content | `web/customer/src/**` | Bestehende Quelle fuer Datenschutz/AGB suchen und aktualisieren. |
| Legal Pages | `web/customer/src/app/**` | Bestehende Routen fuer Datenschutz und AGB muessen weiter erreichbar bleiben. |
| Navigation/Footer | `web/customer/src/components/**` | Bestehende Links auf Rechtstexte duerfen nicht brechen. |
| Tests/Build | `web/customer` | Vitest, Build und Lint als technische Qualitaetsgates. |

## Waves

| Wave | Skill | Agent | Focus | Status |
|---|---|---|---|---|
| Wave 1 | frontend-eng | Frontend Engineer | Rechtstexte in bestehender Customer-Web-Struktur aktualisieren | Done |
| Wave 2 | testing-eng | QA Engineer | Build, Lint, Tests und Legal-Link-Smoke pruefen | Done |
| Wave 3 | review-eng | Review Engineer | Content-Abgleich, Formatierung und Scope-Audit | Done |

## Critical Path

```text
Wave 1 (Legal Content aktualisieren)
    v
Wave 2 (Technische Verifikation)
    v
Wave 3 (Content- und Scope-Review)
```

Wave 1 ist der fachliche Kern. Wave 2 prueft, ob die Customer-Web-App mit den geaenderten Rechtstexten stabil baut. Wave 3 schliesst mit einem expliziten Abgleich gegen die Kundenvorlage und stellt sicher, dass keine nicht freigegebenen Aenderungen eingeflossen sind.

## Risiken und Mitigationen

| Risiko | Impact | Mitigation |
|---|---|---|
| Rechtstext wird unvollstaendig uebernommen | Hoch | Wave 3 mit explizitem Abschnitts-/Paragrafen-Abgleich gegen die Kundenvorlage. |
| Ungewollte juristische Umformulierung | Hoch | Kundentext als Source of Truth; nur technische Formatierung erlaubt. |
| Lange Listen/Paragrafen brechen mobile Darstellung | Mittel | Bestehende Content-Komponenten nutzen und Build/Smoke pruefen. |
| Bestehende Legal-Links brechen | Mittel | Link-/Route-Smoke in Wave 2. |
| Content wird an falscher Stelle dupliziert | Mittel | Zentrale bestehende Content-Quelle identifizieren und Scope-Audit durchfuehren. |
| Nicht-ASCII-Rechtszeichen werden fehlerhaft dargestellt | Mittel | Browser-/Build-Smoke mit Umlauten, `§`, Euro- und Prozentangaben. |

## Gesamt-Akzeptanzkriterien

- [x] Datenschutzerklaerung enthaelt `Padel-Out`, `Stand: April 2026` und die Abschnitte 1 bis 10 aus der gelieferten Kundenfassung.
- [x] Datenschutzerklaerung nennt `info@padel-out.de`, `+49 155 567149871`, `Real Decreto 933/2021`, Occident, Allianz, R+V Allgemeine Versicherung AG und BfDI.
- [x] AGB enthalten `Padel-Out Allgemeine Geschaeftsbedingungen` und die Paragrafen `§ 1` bis `§ 12`.
- [x] AGB § 6.1 nennt fuer Ruecktrittserklaerungen `info@padel-out.de`.
- [x] AGB § 4.2 und § 4.3 entsprechen der erneuerten Kundenfassung: Flughafentransfer ist nicht Standardleistung, sondern optional buchbar.
- [x] Bestehende Datenschutz- und AGB-Routen/Links funktionieren weiter.
- [x] Rechtstexte sind auf Desktop und Mobile lesbar, ohne abgeschnittenen Text oder offensichtliche Layout-Regression.
- [x] Customer-Web Tests sind gruen oder konkrete Blocker sind dokumentiert.
- [x] `npm run build` in `web/customer` ist gruen oder konkrete Blocker sind dokumentiert.
- [x] Lint ist gruen oder bekannte Ausnahmen sind konkret dokumentiert.
- [x] Finaler Content-Abgleich bestaetigt die vollstaendige Uebernahme der Kundentexte.
- [x] Finaler Scope-Audit bestaetigt: keine Planungsdateien im Code-Repo und keine Backend-/DB-/API-Aenderungen.

## Notes

Planung erstellt am 2026-05-05.

Die vom Kunden nachgereichte AGB-Version ist gegenueber der vorherigen AGB-Fassung massgeblich. Bei der Umsetzung duerfen typografische Anpassungen vorgenommen werden, wenn sie nur Darstellung/Lesbarkeit betreffen und den rechtlichen Inhalt nicht veraendern.

Abschluss 2026-05-05:
- Wave 1 bis Wave 3 abgeschlossen.
- Rechtstexte zentral in `web/customer/src/lib/legal-content.ts` hinterlegt.
- Datenschutz-Seite `web/customer/src/app/datenschutz/page.tsx` rendert `Padel-Out`, `Datenschutzerklaerung`, Stand April 2026 und alle 10 Abschnitte aus der Kundenvorlage.
- AGB-Seite `web/customer/src/app/agb/page.tsx` rendert `Padel-Out`, `Allgemeine Geschaeftsbedingungen` und alle Paragrafen `§ 1` bis `§ 12` aus der erneuerten Kundenvorlage.
- `web/customer/src/lib/legal-content.test.ts` ergaenzt: prueft Pflichtinhalte, Abschnitts-/Paragrafenstruktur und entfernt gebliebene alte AGB-Details.
- Verifikation gruen: `npm test -- --configLoader runner src/lib/legal-content.test.ts`, `npm test -- --configLoader runner`, `npm run build`, `npm run lint`, `git diff --check`.
- Build-Smoke bestaetigt statische Routen `/agb` und `/datenschutz`; Footer/Checkout/Cookie-Links zeigen weiter auf `/agb` und `/datenschutz`.
- Scope-Audit: Code-Aenderungen beschraenken sich auf Customer-Web Legal Pages und Legal Content/Test. Keine Backend-, DB-, API- oder Planungsdateien im Code-Repo.
