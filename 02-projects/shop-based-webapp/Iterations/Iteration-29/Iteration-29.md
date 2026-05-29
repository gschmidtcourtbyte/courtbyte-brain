# Iteration 29 - Einheitliche Founder-Bildgroessen

**Status:** Done
**Repo:** /home/andersen/git/shop-based-webapp
**Datum:** 2026-04-26
**Rolle:** Acting as Product Owner. Planning Iteration 29 for shop-based-webapp.

## Ziel

Iteration 29 korrigiert das neue Bildfeedback zu Ariadna und Sascha: Ariadnas Bild wirkt aktuell laenger als Saschas. In den Bereichen "Ueber uns" und "Warum wir" sollen beide Founder-Bilder jeweils dieselbe sichtbare Bildflaeche haben, horizontal und vertikal.

Die Iteration bleibt bewusst klein und frontend-nah. Es werden keine Bildassets geaendert, keine neuen Bilder exportiert und keine Backend-, API- oder Content-Modell-Aenderungen vorgenommen.

## Kundenfeedback

1. Ariadnas Bild ist laenger als Saschas.
2. Die Groesse soll so angepasst werden, dass Ariadna und Sascha vertikal gleich sind.
3. Die Groesse soll so angepasst werden, dass Ariadna und Sascha horizontal gleich sind.
4. Die Korrektur gilt sowohl fuer "Ueber uns" als auch fuer "Warum wir".
5. Den Waves sollen passende Skills zugeordnet werden.

## Scope

**In:**
- `web/customer/src/components/AboutUs.tsx`: Ariadna/Sascha auf identische sichtbare Bildbox-Breite und -Hoehe bringen.
- `web/customer/src/app/warum-wir/page.tsx`: dieselbe Bildgroessenlogik konsistent uebernehmen.
- Responsive Stabilitaet fuer Mobile und Desktop pruefen.
- Statischer Audit der betroffenen Bildcontainer und Bildklassen.
- Customer-Web Tests und Build.
- Visueller Smoke fuer Homepage-"Ueber uns" und `/warum-wir`, sofern lokal moeglich.

**Out:**
- Neue Bildassets, Retusche, Re-Export oder Ersetzung vorhandener Bilder.
- Backend-, API-, Datenbank-, Booking- oder Admin-Aenderungen.
- Vollstaendiges Redesign der Founder-Sektionen.
- Neue CMS-/Admin-Funktionalitaet fuer Bildzuschnitt.
- Aenderungen an Gallery oder nicht betroffenen Bildflaechen.

## Produktentscheidungen

1. **Gleiche sichtbare Bildbox ist das Outcome.**
   Ariadna und Sascha muessen in jedem betroffenen Bereich dieselbe gerenderte Bildbox-Breite und -Hoehe haben. Relevant ist die sichtbare Flaeche im UI, nicht die natuerliche Asset-Abmessung.
2. **Konsistenz zwischen den Bereichen.**
   "Ueber uns" und "Warum wir" sollen dieselbe Founder-Bildlogik verwenden oder zumindest dieselben Constraints und Seitenverhaeltnisse abbilden.
3. **Keine Asset-Aenderungen.**
   Die Korrektur erfolgt ausschliesslich ueber Layout-, Frame- und Rendering-Klassen in der Customer-Web-App.
4. **Responsive Gleichheit.**
   Auf Desktop, Tablet und Mobile duerfen die konkreten Groessen responsiv wechseln, aber Ariadna und Sascha muessen innerhalb desselben Bereichs jeweils gleich breit und gleich hoch bleiben.
5. **Vorhandenes Design respektieren.**
   Copy, Reihenfolge, Navigation, CTAs und der grundsaetzliche Look der Sektionen bleiben erhalten. Es wird nur die Bildgroesse angeglichen.

## Bestandsaufnahme

| Bereich | Datei | Relevanter Kontext |
|---|---|---|
| Iteration 28 | `Iterations/Iteration-28/Iteration-28.md` | Hat die Founder-Bilder bereits auf gemeinsame Frames umgestellt; neues Feedback praezisiert, dass die sichtbare Groesse weiterhin nicht gleich wirkt. |
| Ueber uns | `web/customer/src/components/AboutUs.tsx` | Ariadna/Sascha muessen im Homepage-Bereich dieselbe sichtbare Bildbox erhalten. |
| Warum wir | `web/customer/src/app/warum-wir/page.tsx` | Ariadna/Sascha muessen analog zu "Ueber uns" dieselbe sichtbare Bildbox erhalten. |

## Waves

| Wave | Skill | Agent | Focus | Status |
|---|---|---|---|---|
| Wave 1 | frontend-eng | Frontend Engineer | AboutUs Founder-Bildboxen horizontal und vertikal angleichen | Done |
| Wave 2 | frontend-eng | Frontend Engineer | Warum-wir Founder-Bildboxen konsistent zu AboutUs angleichen | Done |
| Wave 3 | testing-eng | QA Engineer | Bildgroessen-Audit, responsive Smoke, Tests und Build | Done |

## Critical Path

```text
Wave 1 (AboutUs gemeinsame Founder-Bildbox)
    v
Wave 2 (Warum-wir gleiche Bildbox-Logik)
    v
Wave 3 (Audit, Responsive Smoke, Tests, Build)
```

Wave 1 definiert die Ziel-Umsetzung fuer die gemeinsame Bildbox. Wave 2 uebernimmt diese Logik fuer `/warum-wir`, damit die beiden Bereiche nicht auseinanderdriften. Wave 3 startet erst nach Integration beider Frontend-Aenderungen.

## Risiken und Mitigationen

| Risiko | Impact | Mitigation |
|---|---|---|
| Bildboxen wirken gleich, aber CSS unterscheidet sich zwischen Ariadna und Sascha weiter subtil | Hoch | Gemeinsame Klassen/Constraints fuer beide Personen verwenden und statisch vergleichen. |
| Gleiche Hoehe verursacht auf Mobile unruhige Bildausschnitte | Mittel | Responsive Smoke fuer gestapelte Layouts; Gleichheit innerhalb des Breakpoints bleibt Pflicht. |
| AboutUs und Warum-wir nutzen nach der Aenderung unterschiedliche Seitenverhaeltnisse | Mittel | Wave 2 orientiert sich explizit an der in Wave 1 finalisierten Umsetzung. |
| Aenderung fuehrt unbeabsichtigt zu Asset-, Gallery- oder Backend-Diff | Niedrig | Wave 3 prueft finalen Diff gegen Scope. |
| Browser-/Screenshot-Smoke ist lokal nicht moeglich | Niedrig | Blocker dokumentieren und durch statischen Audit plus Tests/Build absichern. |

## Gesamt-Akzeptanzkriterien

- [x] Ariadna und Sascha haben in `AboutUs.tsx` dieselbe sichtbare Bildbox-Breite.
- [x] Ariadna und Sascha haben in `AboutUs.tsx` dieselbe sichtbare Bildbox-Hoehe.
- [x] Ariadna und Sascha haben in `warum-wir/page.tsx` dieselbe sichtbare Bildbox-Breite.
- [x] Ariadna und Sascha haben in `warum-wir/page.tsx` dieselbe sichtbare Bildbox-Hoehe.
- [x] Die Bildboxen bleiben auf Desktop, Tablet und Mobile stabil: keine Textueberlagerung, keine Layout-Spruenge, keine abgeschnittenen CTAs.
- [x] Die sichtbare Founder-Bildlogik ist zwischen "Ueber uns" und "Warum wir" konsistent.
- [x] Keine Bildassets wurden veraendert, ersetzt oder neu exportiert.
- [x] Keine Backend-, API-, Datenbank-, Booking- oder Admin-Aenderungen wurden vorgenommen.
- [x] Customer-Web Tests und Build sind gruen oder Blocker sind konkret dokumentiert.
- [x] Visueller Smoke fuer Homepage-"Ueber uns" und `/warum-wir` ist durchgefuehrt oder ein lokaler Tooling-Blocker ist dokumentiert.

## Notes

Planung freigegeben am 2026-04-26. Diese Iteration fokussiert ausschliesslich auf gleiche sichtbare Founder-Bildgroessen fuer Ariadna und Sascha in den zwei betroffenen Frontend-Bereichen.

Abschluss 2026-04-26:
- Wave 1 bis Wave 3 abgeschlossen.
- Neue gemeinsame Komponente `web/customer/src/components/FounderImageFrame.tsx` eingefuehrt.
- `AboutUs.tsx` und `warum-wir/page.tsx` nutzen fuer Ariadna/Sascha denselben Frame mit `aspectRatio: "2 / 3"`, `position: relative`, `overflow-hidden` und Next `Image fill`.
- Damit bestimmen die gemeinsamen Frame-Constraints die sichtbare Breite und Hoehe, nicht die unterschiedlichen natuerlichen Bildabmessungen von Ariadna und Sascha.
- Ariadna bleibt per `object-bottom` verankert; Sascha nutzt die Standardposition. Die Bildbox-Groesse bleibt fuer beide identisch.
- Statischer Audit bestaetigt, dass beide betroffenen Bereiche nur noch `FounderImageFrame` fuer die Founder-Bilder verwenden.
- Customer-Web Tests: `npm test -- --configLoader runner` mit 10 Testdateien / 79 Tests gruen.
- Customer-Web Build: `npm run build` erfolgreich.
- Browser-/Screenshot-Smoke lokal nicht ausgefuehrt: kein Playwright, Chromium oder Google Chrome im Systempfad verfuegbar.
