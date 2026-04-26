# Iteration 28 - Boxbreite Bilddarstellung ohne Zoom

**Status:** Done
**Repo:** /home/andersen/git/shop-based-webapp
**Datum:** 2026-04-26
**Rolle:** Acting as Product Owner. Planning Iteration 28 for shop-based-webapp.

## Ziel

Iteration 28 korrigiert die Bilddarstellung aus dem Feedback zu Iteration 25/27: Gallery-Bilder sollen wieder die gesamte Boxbreite nutzen, aber nicht hineingezoomt oder beschnitten werden. Die Ariadna- und Sascha-Bilder in "Ueber uns" sowie auf der Seite "Warum wir?" sollen ebenfalls ohne Zoom dargestellt werden und sich an die Breite ihrer Box anpassen.

Die Iteration bleibt bewusst klein und frontend-nah. Es werden keine Bildassets geaendert und kein neues visuelles Konzept eingefuehrt.

## Kundenfeedback

1. Die Bilder der Gallery sollen wieder ueber die gesamte Box gehen.
2. Die Gallery-Bilder duerfen dabei nicht hineingezoomt werden.
3. Die Bilder bei "Ueber uns" von Ariadna und Sascha sollen keinen Zoom verwenden.
4. Die Ariadna- und Sascha-Bilder sollen auf die Breite der Box angepasst werden.
5. Gleiches gilt fuer die Ariadna- und Sascha-Bilder bei "Warum wir?".
6. Hierfuer sollen einzelne Waves geplant werden.

## Scope

**In:**
- `web/customer/src/components/GallerySlider.tsx`: Gallery-Bilder boxbreit darstellen, ohne Crop/Zoom.
- `web/customer/src/components/AboutUs.tsx`: Ariadna/Sascha auf Boxbreite skalieren, ohne Crop/Zoom.
- `web/customer/src/app/warum-wir/page.tsx`: Ariadna/Sascha analog zu "Ueber uns" darstellen.
- Statischer Audit der betroffenen Bildklassen.
- Customer-Web Tests und Build.
- Visueller Smoke fuer Homepage-Gallery, Homepage-"Ueber uns" und `/warum-wir`, sofern lokal moeglich.

**Out:**
- Neue Bildassets, Retusche, Re-Export oder Ersetzung vorhandener Bilder.
- Backend-, API-, Datenbank- oder Booking-Aenderungen.
- Vollstaendiges Redesign der Gallery oder der Founder-Sektionen.
- Neue CMS-/Admin-Funktionalitaet fuer Bildzuschnitt.
- Aenderungen an nicht betroffenen Bildflaechen.

## Produktentscheidungen

1. **Boxbreite gewinnt, aber ohne Crop.**
   Die Bilder sollen die verfuegbare Breite ihres Bildbereichs nutzen. Gleichzeitig duerfen Personen oder Motive nicht durch `object-cover`, manuelle `object-position` oder Scale-Effekte abgeschnitten werden.
2. **Keine Asset-Aenderungen.**
   Die Korrektur erfolgt ausschliesslich ueber Layout- und Rendering-Klassen. Bilddateien bleiben unveraendert.
3. **Gemeinsames Verhalten fuer Ariadna/Sascha.**
   `AboutUs.tsx` und `warum-wir/page.tsx` sollen fuer die Founder-Bilder konsistent wirken: unverzoomt, boxbreit, responsiv stabil.
4. **Gallery bleibt ein Slider.**
   Navigation, Captions und Dot-Indikatoren bleiben erhalten. Diese Iteration aendert nur die Bilddarstellung im vorhandenen Frame.
5. **Responsive Stabilitaet ist Teil des Outcomes.**
   Falls Bildformate freie Flaechen erzeugen, sind ruhige Containerflaechen akzeptabel. Nicht akzeptabel sind Layout-Spruenge, Textueberlagerungen oder sichtbarer Crop.

## Bestandsaufnahme

| Bereich | Datei | Relevanter Kontext |
|---|---|---|
| Iteration 25 | `Iterations/Iteration-25/Iteration-25.md` | Fuehrte unverzoomte Bilddarstellung fuer Gallery, AboutUs und Warum-wir ein. |
| Iteration 27 | `Iterations/Iteration-27/Iteration-27.md` | Korrigierte erneut "Ueber uns" und Gallery auf unverzoomt/eckig; neues Feedback praezisiert nun Boxbreite statt reines Contain. |
| Gallery | `web/customer/src/components/GallerySlider.tsx` | Muss boxbreit wirken, ohne `object-cover` oder Scale-Zoom zu verwenden. |
| Ueber uns | `web/customer/src/components/AboutUs.tsx` | Ariadna/Sascha sollen unverzoomt und an Boxbreite angepasst dargestellt werden. |
| Warum wir | `web/customer/src/app/warum-wir/page.tsx` | Ariadna/Sascha sollen konsistent zu AboutUs behandelt werden. |

## Waves

| Wave | Skill | Agent | Focus | Status |
|---|---|---|---|---|
| Wave 1 | frontend-eng | Frontend Engineer | GallerySlider boxbreit und unverzoomt korrigieren | Done |
| Wave 2 | frontend-eng | Frontend Engineer | AboutUs Founder-Bilder boxbreit und unverzoomt korrigieren | Done |
| Wave 3 | frontend-eng | Frontend Engineer | Warum-wir Founder-Bilder analog korrigieren | Done |
| Wave 4 | testing-eng | QA Engineer | Bild-Audit, Tests, Build und visueller Smoke | Done |

## Critical Path

```text
Wave 1 (Gallery boxbreit ohne Zoom)
Wave 2 (AboutUs Ariadna/Sascha boxbreit ohne Zoom)
Wave 3 (Warum-wir Ariadna/Sascha boxbreit ohne Zoom)
    v
Wave 4 (Regression, Audit, Tests, Build)
```

Waves 1 bis 3 sind fachlich getrennt und koennen parallel vorbereitet werden. Wave 4 startet erst nach Integration aller Bilddarstellungs-Aenderungen.

## Risiken und Mitigationen

| Risiko | Impact | Mitigation |
|---|---|---|
| Boxbreite wird irrtuemlich mit `object-cover` geloest und fuehrt wieder zu Crop | Hoch | Akzeptanzkriterium: kein `object-cover`, keine manuelle `object-position`, kein Scale-Zoom in betroffenen Bildflaechen. |
| Hochformatige Founder-Bilder erzeugen freie Flaechen oder unruhige Hoehen | Mittel | Stabile Container mit responsiven Constraints; Boxbreite ohne Layout-Spruenge priorisieren. |
| Gallery-Bilder mit unterschiedlichen Seitenverhaeltnissen wirken inkonsistent | Mittel | Einheitlichen Slider-Frame behalten und Bilddarstellung ueber Breite/Contain/Max-Constraints abstimmen. |
| Captions oder Slider-Navigation ueberdecken Motive nach Layoutkorrektur | Mittel | Wave 1 prueft Captions, Pfeile und Dots explizit. |
| Warum-wir und AboutUs driften visuell auseinander | Mittel | Wave 3 orientiert sich an der in Wave 2 integrierten Founder-Darstellung. |
| Visueller Smoke ist lokal nicht moeglich | Niedrig | Blocker dokumentieren und durch statischen Audit plus Tests/Build absichern. |

## Gesamt-Akzeptanzkriterien

- [x] Gallery-Bilder nutzen die verfuegbare Boxbreite, ohne in Motive hineinzuzoomen.
- [x] `GallerySlider.tsx` nutzt fuer das sichtbare Slide-Bild kein `object-cover`, keine manuelle `object-[...]` Positionierung und keinen Scale-/Hover-Zoom.
- [x] Ariadna/Sascha in `AboutUs.tsx` sind auf die Boxbreite angepasst, unverzoomt und ohne Crop sichtbar.
- [x] Ariadna/Sascha in `warum-wir/page.tsx` sind konsistent zu `AboutUs.tsx` auf Boxbreite angepasst, unverzoomt und ohne Crop sichtbar.
- [x] Die betroffenen Bildcontainer bleiben auf Desktop und Mobile stabil: keine Textueberlagerung, keine Layout-Spruenge, keine abgeschnittenen CTAs.
- [x] Keine Bildassets wurden veraendert, ersetzt oder neu exportiert.
- [x] Customer-Web Tests und Build sind gruen oder Blocker sind konkret dokumentiert.
- [x] Visueller Smoke fuer Homepage-Gallery, Homepage-"Ueber uns" und `/warum-wir` ist durchgefuehrt oder ein lokaler Tooling-Blocker ist dokumentiert.

## Notes

Planung freigegeben am 2026-04-26. Diese Iteration fokussiert ausschliesslich auf die Praezisierung der Bilddarstellung: boxbreit, aber nicht hineingezoomt.

Abschluss 2026-04-26:
- Wave 1 bis Wave 4 abgeschlossen.
- GallerySlider rendert Slides mit echten Bilddimensionen und `block h-auto w-full`; `fill`/feste Aspect-Frames wurden fuer das sichtbare Slide-Bild entfernt.
- `AboutUs.tsx` und `warum-wir/page.tsx` rendern Ariadna/Sascha mit echten Bilddimensionen und `block h-auto w-full`.
- Gallery-Daten enthalten Breite/Hoehe je kuratiertem Slide; `gallery.test.ts` sichert diese Metadaten.
- Statischer Audit: `rg -n "object-cover|object-\[|scale-|hover:scale|group-hover:scale" web/customer/src/components/GallerySlider.tsx web/customer/src/components/AboutUs.tsx web/customer/src/app/warum-wir/page.tsx` ohne Treffer.
- Customer-Web Tests: `npm test -- --configLoader runner` mit 10 Testdateien / 79 Tests gruen.
- Customer-Web Build: `npm run build` erfolgreich.
- Browser-/Screenshot-Smoke lokal nicht ausgefuehrt: kein Playwright, Chromium oder Google Chrome im Projekt/Systempfad verfuegbar.
