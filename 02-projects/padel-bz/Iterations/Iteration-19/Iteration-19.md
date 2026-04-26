# Iteration 19: Anfahrtskarte animieren und Footer bereinigen

## Ziel

Die stilisierte SVG-Anfahrtskarte auf der Startseite erhaelt eine hochwertigere, sequentielle Draw-In-Animation: Hauptzufahrtsstrassen werden einzeln nacheinander bis zum Zielort gezeichnet — ausgeloest per IntersectionObserver beim Scroll. Die bisherige pauschale CSS-Glow-Animation wird durch eine gestaffelte Stroke-Dashoffset-Animation ersetzt, die den Routenverlauf erlebbar macht. Ausserdem wird der eingezeichnete Kreisel korrekt als Autobahnabfahrt gekennzeichnet. Im Footer werden nicht mehr benoetigte Links entfernt.

## Ausgangslage

### Anfahrtskarte (`web/templates/pages/home.html`, Zeilen 261-312)
- SVG-Karte mit `viewBox="0 0 600 400"`, Grid-Pattern-Hintergrund.
- Strassen als SVG `<path>` Elemente mit Klassen: `road-a28`, `road-oldenburger`, `road-local`, `road-kranstrasse`.
- Zwei `<circle>` Elemente als "Kreisverkehr" kommentiert — das obere (cx=390, cy=88, r=14) ist die **Autobahnabfahrt A28**, nicht ein Kreisverkehr.
- Location Pin (3 circles) an cx=115, cy=365 fuer Kranstrasse 1a.
- Labels: A28, Oldenburger Str., Kranstr., Haarenstroth, Westerholtsfelder Weg.

### Aktuelle Animation (`web/static/css/style.css`, Zeilen 822-829)
- `@keyframes road-glow` — alle Strassen pulsieren gleichzeitig in der Accent-Farbe, nur zeitversetzt (0s, 2s, 4s, 6s).
- Kein Draw-In-Effekt, kein sequentielles Aufbauen.
- Pin pulsiert permanent (`svg-pulse`, `svg-ring-pulse`).

### JavaScript (`web/static/js/main.js`)
- Kein map-spezifischer Code vorhanden.
- IntersectionObserver fuer `.reveal-on-scroll` ist implementiert (threshold 0.1).

### Footer (`web/templates/partials/footer.html`)
- **Spielen**: Court buchen, Mitgliedschaft, Hansefit, GPS Tour.
- **Standort**: Bad Zwischenahn, Niedersachsen, Anfahrt.

## Scope

### Anfahrtskarte
- Kreisverkehr-Kommentare im SVG auf "Autobahnabfahrt" korrigieren.
- Bestehende `road-glow` CSS-Animation entfernen.
- Neue sequentielle Draw-In-Animation implementieren:
  1. **A28 Autobahn** zeichnet sich zuerst von oben nach unten.
  2. **Autobahnabfahrt** (Kreis an A28) wird sichtbar.
  3. **Oldenburger Str.** zeichnet sich vom Kreisverkehr nach links (oder vom Kreisel Richtung Haarenstroth).
  4. **Nebenstrassen** zeichnen sich nacheinander ein.
  5. **Kranstrasse** zeichnet sich als letzte Hauptroute zum Zielort.
  6. **Location Pin** erscheint mit Pulse-Animation am Ende.
- Technik: `stroke-dasharray` + `stroke-dashoffset` Transition, getriggert per JS IntersectionObserver.
- Labels faden nach ihren zugehoerigen Strassen ein.
- Animation startet erst beim Scrollen in den Viewport (kein Autostart).
- Gesamtdauer ca. 4-6 Sekunden, professionelle Easing-Kurven.

### Footer
- "Court buchen" und "Mitgliedschaft" aus "Spielen" entfernen.
- "Niedersachsen" aus "Standort" entfernen.
- Verbleibende Links: Spielen (Hansefit, GPS Tour), Standort (Bad Zwischenahn, Anfahrt).

## Nicht im Scope

- Inhaltliche Aenderungen an der Karte (neue Strassen, anderer Kartenausschnitt).
- Interaktive Karte oder Google Maps Einbindung.
- Aenderungen am Kontakt-Info-Block neben der Karte.
- Neue Footer-Eintraege.

## Produktentscheidungen

- **Draw-In statt Glow**: Die neue Animation soll den Anfahrtsweg visuell nachvollziehbar machen — eine Strasse nach der anderen baut sich auf, als wuerde man die Route nachzeichnen.
- **Autobahnabfahrt statt Kreisverkehr**: Der obere Kreisel auf der Karte stellt die Abfahrt von der A28 dar und wird entsprechend im Code kommentiert und ggf. visuell angepasst (z.B. Abfahrtsymbol statt einfacher Kreis).
- **Sequentielle Reihenfolge**: A28 → Autobahnabfahrt → Hauptzufahrtsstrassen einzeln → Kranstrasse → Pin. Jede Strasse zeichnet sich einzeln fertig, bevor die naechste startet.
- **Footer-Bereinigung**: Entfernte Links sind aktuell ohnehin nur Platzhalter die auf `/kontakt` verweisen.

## Critical Path

1. SVG-Kommentare und -Struktur anpassen (Autobahnabfahrt).
2. CSS: `road-glow` Animation entfernen, neue Draw-In-Klassen vorbereiten.
3. JS: IntersectionObserver-basierte Draw-In-Animation implementieren.
4. CSS: Timing, Easing und finale visuelle Politur.
5. Footer-Links bereinigen.
6. Visueller Test auf Desktop und Mobile.

## Agenten-Zuordnung

| Agent | Skillset | Verantwortung |
|-------|----------|----------------|
| PO Lead | product-own | Scope, Wave-Freigabe, Akzeptanzabnahme |
| Agent A | frontend-eng | SVG-Anpassungen, CSS-Animation, JS Draw-In-Logik, Footer-Bereinigung |
| Agent B | testing-eng | Render-Tests, Regressionspruefung |

---

## Wave 1 — Footer bereinigen und SVG-Kommentare korrigieren
**Agenten**: Agent A (frontend-eng)
**Kann parallel ausgefuehrt werden**: ja (Footer und SVG-Kommentare sind unabhaengig)
**Ziel**: Schnelle Cleanup-Tasks die keine Abhaengigkeiten haben.

### Task 19.1: Footer-Links bereinigen
**Priority**: P0
**Blocked by**: none
**Agent**: Agent A (frontend-eng)

**Akzeptanzkriterien**:
- [ ] "Court buchen" (`<li><a href="/kontakt">Court buchen</a></li>`) ist aus dem Footer entfernt.
- [ ] "Mitgliedschaft" (`<li><a href="/kontakt">Mitgliedschaft</a></li>`) ist aus dem Footer entfernt.
- [ ] "Niedersachsen" (`<li><a href="/#kontakt">Niedersachsen</a></li>`) ist aus dem Footer entfernt.
- [ ] Verbleibende "Spielen" Links: Hansefit, GPS Tour.
- [ ] Verbleibende "Standort" Links: Bad Zwischenahn, Anfahrt.
- [ ] Footer rendert korrekt ohne Layout-Bruch.

**Kontext**:
- `web/templates/partials/footer.html`

**Output**:
- Aktualisiertes `web/templates/partials/footer.html`.

### Task 19.2: SVG-Kommentare auf "Autobahnabfahrt" korrigieren
**Priority**: P1
**Blocked by**: none
**Agent**: Agent A (frontend-eng)

**Akzeptanzkriterien**:
- [ ] HTML-Kommentar fuer den oberen Kreisel (cx=390, cy=88) lautet "Autobahnabfahrt A28" statt "Kreisverkehr oben an A28".
- [ ] Der untere Kreisel (cx=410, cy=120) bleibt als "Kreisverkehr" oder wird als "Abzweigung" umbenannt — je nach Kontext.
- [ ] Keine visuellen Aenderungen in diesem Task.

**Kontext**:
- `web/templates/pages/home.html`, Zeilen 270-277

**Output**:
- Aktualisierte Kommentare in `web/templates/pages/home.html`.

**Wave-1-Exit-Gate**:
- [ ] Footer zeigt nur noch Hansefit + GPS Tour unter "Spielen" und Bad Zwischenahn + Anfahrt unter "Standort".
- [ ] SVG-Kommentare sind inhaltlich korrekt.

---

## Wave 2 — Sequentielle Draw-In-Animation implementieren
**Agenten**: Agent A (frontend-eng)
**Kann parallel ausgefuehrt werden**: nein (CSS und JS muessen zusammen entwickelt werden)
**Ziel**: Die Hauptarbeit — die bisherige Glow-Animation wird durch eine professionelle, sequentielle Draw-In-Animation ersetzt.

### Task 19.3: CSS road-glow entfernen und Draw-In-Grundstruktur vorbereiten
**Priority**: P0
**Blocked by**: 19.2
**Agent**: Agent A (frontend-eng)

**Akzeptanzkriterien**:
- [ ] `@keyframes road-glow` ist entfernt.
- [ ] Die vier `.road-*` Animations-Zuweisungen (Zeilen 826-829) sind entfernt.
- [ ] Strassen haben initial `opacity: 0` oder `stroke-dashoffset` auf volle Laenge (unsichtbar).
- [ ] Pin und Pin-Ringe haben initial `opacity: 0`.
- [ ] Labels haben initial `opacity: 0`.
- [ ] Eine CSS-Klasse `.map-animate` (oder vergleichbar) aktiviert die Draw-In-Transition per Klassen-Toggle.
- [ ] Transition-Dauer und Easing sind definiert: `stroke-dashoffset` mit `cubic-bezier` oder `ease-in-out`.
- [ ] Keine visuellen Regressionen auf anderen Seitenteilen.

**Kontext**:
- `web/static/css/style.css`, Zeilen 813-850
- SVG-Pfade in `web/templates/pages/home.html`

**Output**:
- Aktualisiertes `web/static/css/style.css`.

### Task 19.4: JavaScript Draw-In-Animation mit IntersectionObserver
**Priority**: P0
**Blocked by**: 19.3
**Agent**: Agent A (frontend-eng)

**Akzeptanzkriterien**:
- [ ] Neuer Abschnitt in `web/static/js/main.js` fuer die Map-Animation.
- [ ] IntersectionObserver auf `.map` Container, threshold ca. 0.3.
- [ ] Bei Intersection: Klasse `.map-animate` wird auf das SVG oder den Container gesetzt.
- [ ] Strassen werden sequentiell per `stroke-dashoffset` Animation eingezeichnet:
  1. **A28** (road-a28, Hauptstrang) — zeichnet sich von oben nach unten (~1s).
  2. **Autobahnabfahrt-Kreis** — wird sichtbar (fade-in, ~0.3s).
  3. **Oldenburger Str.** (road-oldenburger, vom Kreisverkehr nach links) — zeichnet sich ein (~0.8s).
  4. **Strasse Richtung Kranstrasse** (road-oldenburger, nach Sueden) — zeichnet sich ein (~0.6s).
  5. **Nebenstrassen** (road-local) — faden nacheinander ein oder zeichnen sich leicht ein (~0.4s je).
  6. **Kranstrasse** (road-kranstrasse) — zeichnet sich zum Pin (~0.5s).
  7. **Location Pin** — erscheint mit Pulse-Start (~0.3s).
- [ ] `stroke-dasharray` und `stroke-dashoffset` werden per JS berechnet (via `getTotalLength()`).
- [ ] Labels faden zeitversetzt nach ihren Strassen ein.
- [ ] Animation spielt nur einmal (Observer disconnected nach Trigger).
- [ ] Gesamtdauer ca. 4-6 Sekunden.
- [ ] Kein jQuery, kein externes Framework — Vanilla JS.
- [ ] `prefers-reduced-motion: reduce` wird respektiert (sofort sichtbar, keine Animation).

**Kontext**:
- `web/static/js/main.js` — bestehender IntersectionObserver als Vorlage.
- `web/templates/pages/home.html` — SVG-Pfad-Klassen und IDs.
- `web/static/css/style.css` — Draw-In CSS aus Task 19.3.

**Output**:
- Erweitertes `web/static/js/main.js` mit Map-Draw-In-Logik.

### Task 19.5: SVG-Pfade fuer Draw-In optimieren
**Priority**: P1
**Blocked by**: 19.2
**Agent**: Agent A (frontend-eng)

**Akzeptanzkriterien**:
- [ ] Jeder Hauptstrassenpfad hat eine eindeutige Klasse oder ID fuer gezielte JS-Selektion.
- [ ] Falls Pfade fuer die Animation in der richtigen Zeichenrichtung sein muessen (Start → Ziel), werden sie ggf. umgekehrt.
- [ ] Oldenburger Str. zeichnet sich vom A28-Kreisverkehr (rechts) nach links — Pfad-Richtung pruefen.
- [ ] Die Strasse von Haarenstroth nach Sueden (Richtung Kranstrasse) zeichnet sich von oben nach unten.
- [ ] Kranstrasse zeichnet sich vom Abzweig nach unten zum Pin.
- [ ] Autobahnabfahrt-Kreis bekommt eine eigene Klasse (z.B. `road-exit`) wenn noetig.
- [ ] Neben-/Lokalstrassen koennen gruppiert bleiben.

**Kontext**:
- `web/templates/pages/home.html`, SVG-Bereich

**Output**:
- Ggf. angepasste SVG-Pfade in `web/templates/pages/home.html`.

**Wave-2-Exit-Gate**:
- [ ] Die Karte baut sich beim Scrollen sequentiell auf — Strasse fuer Strasse.
- [ ] A28 zuerst, Autobahnabfahrt, dann Zufahrtsstrassen, dann Kranstrasse, dann Pin.
- [ ] Labels erscheinen zeitversetzt.
- [ ] Animation spielt nur einmal.
- [ ] Bei `prefers-reduced-motion` ist alles sofort sichtbar.

---

## Wave 3 — Visuelle Politur und Testing
**Agenten**: Agent A (frontend-eng), Agent B (testing-eng)
**Kann parallel ausgefuehrt werden**: ja
**Ziel**: Feinschliff der Animation, Regressionstests.

### Task 19.6: Animation Feinschliff und Mobile-Optimierung
**Priority**: P1
**Blocked by**: 19.4
**Agent**: Agent A (frontend-eng)

**Akzeptanzkriterien**:
- [ ] Timing und Easing wirken professionell und fluessig.
- [ ] Strassen leuchten beim Draw-In in Accent-Farbe und faden danach auf normale Strassenfarbe zurueck.
- [ ] Pin-Pulse-Animation startet erst nach dem Draw-In.
- [ ] Auf Mobile (< 768px) ist die Animation fluessig, kein Ruckeln.
- [ ] Kein Layout-Shift waehrend der Animation.
- [ ] Karte sieht vor und nach der Animation visuell stimmig aus.

**Kontext**:
- Gesamter Map-Code aus Wave 2.

**Output**:
- Feintuning in CSS und JS.

### Task 19.7: Regressionstests
**Priority**: P1
**Blocked by**: 19.1, 19.4
**Agent**: Agent B (testing-eng)

**Akzeptanzkriterien**:
- [ ] `go test ./...` ist gruen.
- [ ] Render-Tests fuer `home.html` bestehen.
- [ ] Render-Tests fuer `footer.html` bestehen (falls vorhanden).
- [ ] Footer zeigt korrekte Links (keine entfernten Eintraege).
- [ ] Kein JS-Syntaxfehler in `main.js`.
- [ ] `git diff --check` ist gruen.

**Kontext**:
- `internal/handler/render_test.go`
- `web/templates/partials/footer.html`
- `web/static/js/main.js`

**Output**:
- Gruene Tests, QA-Notiz.

**Wave-3-Exit-Gate**:
- [ ] Animation ist visuell poliert und fluesig auf Desktop und Mobile.
- [ ] Alle Tests gruen.
- [ ] Keine Regressionen.

---

## Abhaengigkeiten und Parallelisierung

| Wave | Parallel moeglich | Blockiert durch | Bemerkung |
|------|-------------------|-----------------|-----------|
| Wave 1 | ja | none | Footer und SVG-Kommentare sind unabhaengig |
| Wave 2 | teilweise | Wave 1 (19.2) | CSS muss vor JS stehen; SVG-Optimierung parallel zu CSS |
| Wave 3 | ja | Wave 2 | Politur und Tests koennen parallel laufen |

## Risiken

| Risiko | Impact | Umgang |
|--------|--------|--------|
| `getTotalLength()` ist bei Kreisen nicht definiert | Niedrig | Kreise per `opacity` statt `stroke-dashoffset` einblenden |
| Animation ruckelt auf schwachen Mobilgeraeten | Mittel | Nur `opacity` und `stroke-dashoffset` animieren (GPU-freundlich), `will-change` nutzen |
| Pfad-Richtung stimmt nicht mit gewuenschter Zeichenrichtung ueberein | Niedrig | Pfad-Koordinaten umkehren oder `stroke-dashoffset` von negativ nach 0 animieren |

## Definition of Done

- [ ] Footer "Spielen": Nur Hansefit und GPS Tour.
- [ ] Footer "Standort": Nur Bad Zwischenahn und Anfahrt.
- [ ] SVG-Kommentare korrekt (Autobahnabfahrt statt Kreisverkehr).
- [ ] Map-Animation: Sequentieller Draw-In beim Scrollen, A28 → Abfahrt → Zufahrtsstrassen → Kranstrasse → Pin.
- [ ] Animation spielt einmal, respektiert `prefers-reduced-motion`.
- [ ] Desktop und Mobile fluessig.
- [ ] `go test ./...` gruen.
- [ ] `git diff --check` gruen.
