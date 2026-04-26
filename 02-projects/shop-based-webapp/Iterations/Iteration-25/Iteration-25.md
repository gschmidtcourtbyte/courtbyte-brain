# Iteration 25 - Insolvenzversicherung Copy und unverzoomte Bilddarstellung

**Status:** Done
**Repo:** /home/andersen/git/shop-based-webapp
**Datum:** 2026-04-26
**Rolle:** Acting as Product Owner. Planning Iteration 25 for shop-based-webapp.

## Ziel

Iteration 25 setzt zwei konkrete Kundenfeedbackpunkte um: Der Insolvenzversicherungstext wird auf der Seite "Warum wir?" bei der Insolvenzversicherung platziert, und hochgeladene Bilder werden ohne Zoom, Crop oder manuelle Ausschnittverschiebung dargestellt. Die Umsetzung soll den bestehenden OUT-Padel-Look erhalten, aber die kuratorische Bildquelle respektieren: Was hochgeladen wurde, soll vollständig sichtbar bleiben.

## Kundenfeedback

1. Unter "Warum wir?" bei der Insolvenzversicherung diesen Text einfuegen:

   > Dein Geld ist bei uns sicher — zu 100%. Als lizenzierter Reiseveranstalter sind wir gesetzlich verpflichtet, deine Zahlungen durch eine offizielle Insolvenzversicherung (§651r BGB) abzusichern. Das bedeutet für dich: Egal was passiert — Anzahlung und Restzahlung sind vollständig geschützt. Mit deiner Buchungsbestätigung erhältst du deinen persönlichen Reisesicherungsschein als Nachweis.

2. Bilder wurden teilweise "reingezoomt" und nicht so angewendet wie hochgeladen. Bilder sollen so eingebunden werden, wie sie hochgeladen wurden: keine Zoom-Aenderung, keine Crop-Aenderung, keine manuelle Ausschnittverschiebung.

## Scope

**In:**
- `web/customer/src/app/warum-wir/page.tsx`: Insolvenzversicherungskarte um den neuen Text ergaenzen.
- `web/customer/src/app/warum-wir/page.tsx`: Bilder ohne Crop/Zoom darstellen, insbesondere Founder-Bilder und das Netzwerkbild.
- `web/customer/src/components/AboutUs.tsx`: Founder-Bilder ohne Crop/Zoom darstellen.
- `web/customer/src/components/GallerySlider.tsx`: Gallery-Bilder ohne Crop/Zoom darstellen.
- Relevante Tests/Build/visueller Smoke fuer Customer Web.

**Out:**
- Neue Bildauswahl, Retusche oder Asset-Ersetzung.
- Bildbearbeitung, Zuschnitt oder generierte Ersatzbilder.
- Aenderungen an Booking-, Pricing- oder Backend-Logik.
- Vollstaendige Migration aller historischen Planungsdateien.

## Produktentscheidungen

1. **Der Insolvenzversicherungstext wird exakt uebernommen.**
   Keine inhaltliche Kuerzung und keine juristische Umformulierung ohne neue Freigabe.
2. **Placement folgt dem bestehenden Sicherheitskarten-Pattern.**
   Die Insolvenzversicherung erhaelt denselben "Mehr erfahren"-Detailzugang wie Unfallversicherung und Betriebshaftpflicht, sofern dadurch die Karte nicht dauerhaft ueberladen wird.
3. **Originalbild vor Card-Crop.**
   Falls ein Bild nicht ins bisherige feste Seitenverhaeltnis passt, gewinnt die vollstaendige Bildsichtbarkeit. Letterboxing/ruhige Hintergrundflaechen sind akzeptabel; abgeschnittene Personen, Courts oder Hotelbereiche nicht.
4. **Keine manuelle Ausschnittsteuerung.**
   Klassen wie `object-cover`, `object-[...]`, Hover-/Animation-Scale oder aehnliche Crop-/Zoom-Mechaniken werden fuer diese Upload-Bilder entfernt oder ersetzt.
5. **Layout bleibt stabil.**
   Die Umstellung auf unverzoomte Darstellung darf keine Layout-Spruenge, Textueberlagerungen oder unruhige Bildcontainer verursachen.

## Bestandsaufnahme

| Bereich | Datei | Befund |
|---|---|---|
| Insolvenzversicherung | `web/customer/src/app/warum-wir/page.tsx` | Card `insolvency` hat Titel, Provider und Kurztext, aber aktuell kein `detail`; dadurch kein "Mehr erfahren". |
| Founder-Bilder Warum-wir | `web/customer/src/app/warum-wir/page.tsx` | Ariadna/Sascha nutzen `fill` mit `object-cover` und manueller `object-position`. |
| Founder-Bilder Homepage | `web/customer/src/components/AboutUs.tsx` | Ariadna/Sascha nutzen ebenfalls `fill`, `object-cover` und manuelle `object-position`. |
| Netzwerkbild Warum-wir | `web/customer/src/app/warum-wir/page.tsx` | `/images/camp/Prio3.jpg` nutzt `object-cover` in festem Container. |
| Homepage Gallery | `web/customer/src/components/GallerySlider.tsx` | Slides nutzen `object-cover` in responsiven Festformaten, wodurch Hoch-/Querformate beschnitten werden. |
| Gallery Tests | `web/customer/src/lib/gallery.test.ts` | Sichert Reihenfolge/Metadaten, nicht die Render-Darstellung. |

## Waves

| Wave | Skill | Focus | Status |
|---|---|---|---|
| Wave 1 | frontend-eng | Insolvenzversicherungstext in "Warum wir?" integrieren | Done |
| Wave 2 | frontend-eng | Bilddarstellung ohne Zoom/Crop fuer Founder, Warum-wir Bild und Gallery | Done |
| Wave 3 | testing-eng | Regression, Build, statischer Crop-Audit und visueller Smoke | Done |

## Critical Path

```text
Wave 1 (Copy in Insolvenzversicherung)
    v
Wave 2 (Unverzoomte Bilddarstellung)
    v
Wave 3 (Tests, Build, Visual Review)
```

Wave 1 und Wave 2 koennen vorbereitet werden, sollten aber wegen gemeinsamer Datei `warum-wir/page.tsx` sequenziell integriert werden. Wave 3 startet erst nach beiden Frontend-Waves.

## Risiken und Mitigationen

| Risiko | Impact | Mitigation |
|---|---|---|
| Langer Versicherungstext macht die Card unruhig | Mittel | Bestehendes "Mehr erfahren"-Pattern auf Insolvenzversicherung ausweiten; Detailtext nicht in dauerhaft sichtbare Kurzzeile pressen. |
| `object-contain` erzeugt freie Flaechen | Mittel | Container mit ruhiger Hintergrundflaeche und stabiler Hoehe gestalten; Bildvollstaendigkeit hat Prioritaet. |
| Gallery wird durch unterschiedliche Bildformate unruhiger | Mittel | Einheitlichen Rahmen behalten, aber Bilder voll sichtbar anzeigen; Captions/Overlays duerfen das Motiv nicht verdecken. |
| Entfernen von `object-cover` betrifft nicht alle relevanten Bilder | Hoch | Statischer Audit per `rg "object-cover|object-\\["` fuer betroffene Dateien als Done-Kriterium. |
| Tests decken visuelle Crop-Frage nicht automatisch ab | Mittel | Kombination aus statischem Audit, Build und manuellem Desktop/Mobile-Smoke dokumentieren. |

## Gesamt-Akzeptanzkriterien

- [x] Der exakte Kundenfeedbacktext steht bei der Insolvenzversicherung auf der Seite "Warum wir?".
- [x] Die Insolvenzversicherungskarte bleibt konsistent mit Unfallversicherung und Betriebshaftpflicht.
- [x] Ariadna- und Sascha-Bilder werden in `warum-wir/page.tsx` und `AboutUs.tsx` vollstaendig sichtbar und ohne manuelle Ausschnittverschiebung dargestellt.
- [x] Gallery-Bilder werden ohne Zoom/Crop dargestellt; Hoch- und Querformate bleiben vollstaendig sichtbar.
- [x] Keine `object-cover`, `object-[...]` oder Scale-Effekte bleiben auf den betroffenen Kundenbildern aktiv.
- [x] Customer-Web Tests und Build sind gruen oder Blocker sind konkret dokumentiert.
- [ ] Desktop- und Mobile-Smoke fuer `/warum-wir` und die Homepage-Gallery bestaetigt: lokal blockiert, weil kein Playwright/Chromium/Screenshot-Tool verfuegbar ist; statischer Audit, Tests und Build sind gruen.

## Umsetzung

- Product Owner / Integrator: Scope-Steuerung, Sequencing und Vault-Status.
- Frontend Engineer: Wave 1 und Wave 2 umgesetzt in `warum-wir/page.tsx`, `AboutUs.tsx`, `GallerySlider.tsx`.
- Frontend Review Agent: Read-only Audit der Crop-/Zoom-Oberflaechen; Restfund `fadeIn`-Scale wurde entfernt.
- Testing Engineer: Statischer Crop-Audit, Vitest-Suite und Next Build ausgefuehrt.

## Verification

- `rg "object-cover|object-\[|scale-\[|group-hover:scale|hover:scale|animate-\[fadeIn" web/customer/src/app/warum-wir/page.tsx web/customer/src/components/AboutUs.tsx web/customer/src/components/GallerySlider.tsx` -> keine Treffer.
- `cd web/customer && npm test -- --configLoader runner` -> 9 Testdateien, 70 Tests gruen.
- `cd web/customer && npm run build` -> erfolgreich.
- Visual Smoke per Browser/Screenshot lokal nicht ausgefuehrt: kein Playwright, Chromium oder Google Chrome im Projekt/Systempfad verfuegbar.

## Notes

Diese Iteration ist bewusst klein und feedbackgetrieben. Es wurden keine Bildassets veraendert; nur die Art der Darstellung wurde korrigiert.
