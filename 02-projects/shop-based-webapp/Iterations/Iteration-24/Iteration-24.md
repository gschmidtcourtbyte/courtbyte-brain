# Iteration 24 - Kundenfeedback: Warum-wir, Pakete, Experience-Ausblendung und Gallery

## Ziel

Das aktuelle Kundenfeedback wird in kleine, einzeln ausführbare Frontend-Waves zerlegt. Der Fokus liegt auf redaktioneller Klarheit, visueller Gleichgewichtung von Ariadna und Sascha, einer kompakteren Paketdarstellung, temporärem Ausblenden der Experience Section und einer stärker verkaufsorientierten Gallery-Reihenfolge.

---

## Produktentscheidungen / Scope-Leitplanken

1. **Ariadna und Sascha werden gleichberechtigt vorgestellt.**
   Beide Profile stehen an allen sichtbaren Vorstellungsstellen nebeneinander, nutzen die neuen Portraits und wirken visuell gleichwertig. Keine Person darf layoutseitig als nachgeordnet erscheinen.
2. **Die neuen Portraitdateien sind die kuratorische Quelle.**
   Neue Rohbilder liegen unter `Bilder/Sascha.png` und `Bilder/Ariadna.jpeg`. Für die Runtime-Ausspielung werden sie als optimierte Customer-Web-Assets eingebunden.
3. **"Sicherheit & Vertrauen" wird kompakter.**
   Die unteren Detailkacheln "Unfallversicherung" und "Betriebshaftpflicht" werden entfernt. Ihr Content wandert in die oberen Sicherheitskacheln und wird dort über "Mehr erfahren" zugänglich.
4. **Das Bild unter "Sicherheit & Vertrauen" wird entfernt.**
   Die Versicherungssektion soll ohne den unteren Bild-/Detailbereich auskommen.
5. **Obsolete Paket-Highlightkacheln werden entfernt.**
   Die Kacheln wie "15 Stunden echtes Training! 7 Stunden lockere Einheit" entfallen auf der Startseite.
6. **"Was immer drin ist" wird in vier Kategorien gegliedert.**
   Kategorien: `Hotel`, `Training`, `Sicherheit`, `Erlebnis`. Jede Kategorie bekommt eine passende Überschrift und farbliche Differenzierung im bestehenden OUT-Padel-Look.
7. **Paketkarten werden kaufnäher strukturiert.**
   Der Preis steht direkt über dem CTA-Button. Die Labeltexte "Im Paket enthalten" entfallen. Die Preisbox mit "Preis" und Betrag wird zentriert.
8. **Paket-Reihenfolge ist verbindlich.**
   Reihenfolge: `Single`, `Doppel`, `Begleitperson`, `VIP`.
9. **Experience wird temporär ausgeblendet.**
   Die Experience Section wird aus der Startseite und aus Navigation/Footer entfernt, bleibt aber so dokumentiert/isolierbar, dass sie später wieder aktiviert werden kann.
10. **Impressionen folgen direkt unter den Paketen.**
    Nach dem Ausblenden der Experience Section steht der Gallery-/Impressionenbereich unmittelbar nach den Paketkarten.
11. **Gallery-Reihenfolge wird hotel-first.**
    Erst Hotelbilder: Einzelzimmer, Doppelzimmer, Gym, Pool. Danach Padel-Courts. Danach der restliche kuratierte Bestand.

---

## Priorisierung

- **P0**
  - Neue Portraits technisch einbinden und Ariadna/Sascha gleichberechtigt nebeneinander darstellen.
  - Obsolete Paket-Highlightkacheln entfernen.
  - Experience Section temporär ausblenden und aus Navigation/Footer entfernen.
  - Impressionen direkt unter den Paketen anzeigen.

- **P1**
  - "Sicherheit & Vertrauen" mit "Mehr erfahren" je relevanter Kachel umbauen.
  - "Was immer drin ist" in vier farblich unterscheidbare Kategorien gliedern.
  - Paketkarten: Preis über Button, Preisbox zentriert, Label "Im Paket enthalten" entfernen.
  - Gallery-Reihenfolge anpassen und Tests aktualisieren.

- **P2**
  - Content-Dokumentation aktualisieren, falls die Content-Single-Source betroffen ist.
  - Visuellen Smoke-Test für Desktop und Mobile dokumentieren.

---

## Bestandsaufnahme

| Bereich | Datei | Befund |
|--------|-------|--------|
| Warum-wir Portraits und Versicherungen | `web/customer/src/app/warum-wir/page.tsx` | Ariadna/Sascha stehen aktuell in einem Textblock untereinander; Ariadna nutzt Platzhalter, Sascha nutzt `/images/Sascha.jpg`; Versicherungsdetails stehen unten in separaten Kacheln neben einem Bild. |
| Homepage Über-uns | `web/customer/src/components/AboutUs.tsx` | Sascha und Ariadna stehen in einer großen Card untereinander mit Divider; Ariadna nutzt Platzhalter, Sascha nutzt `/images/Sascha.jpg`. |
| Homepage Reihenfolge | `web/customer/src/app/page.tsx` | Experience Section steht zwischen `Pricing` und `GallerySlider`. |
| Navigation | `web/customer/src/components/Navigation.tsx` | `Experience` verweist auf `/#experience`. |
| Footer | `web/customer/src/components/Footer.tsx` | `Experience` steht in der Spalte `Erlebnis`. |
| Pricing UI | `web/customer/src/components/Pricing.tsx` | Highlightkacheln werden aus `PRICING_EDITORIAL_CONTENT.highlights` gerendert; "Was immer drin ist" ist aktuell eine ungegliederte Liste; Preis steht oben rechts in der Karte; "Im Paket enthalten" wird als Label angezeigt. |
| Pricing Content | `web/customer/src/lib/camp-packages.ts` | Enthält obsolete `highlights`, ungegliederte `includedItems` und die vier Pakete in der korrekten Reihenfolge. |
| Gallery | `web/customer/src/lib/gallery.ts` | Aktuelle Reihenfolge startet mit Courts/Venue; Hotelbilder sind verteilt. |
| Gallery Tests | `web/customer/src/lib/gallery.test.ts` | Erwartet exakte Slide-Reihenfolge und muss bei Gallery-Umsortierung angepasst werden. |
| Paket Tests | `web/customer/src/lib/camp-packages.test.ts` | Prüft Slugs und Pricing-Logik; kann um Reihenfolge-/Kategorietests ergänzt werden. |
| Neue Rohbilder | `Bilder/Ariadna.jpeg`, `Bilder/Sascha.png` | Noch nicht als `web/customer/public/images/...` Runtime-Assets eingebunden. |

---

## Wave 1 - Assets und gleichberechtigte Gründer-Vorstellung

**Ziel:** Ariadna und Sascha werden auf der "Warum wir?" Seite und in der Homepage-Über-uns-Sektion mit neuen Bildern gleichwertig nebeneinander vorgestellt.

| Workstream | Task | Primäres Skillset | Mitwirkend |
|-----------|------|-------------------|------------|
| Asset Pipeline | `Bilder/Ariadna.jpeg` und `Bilder/Sascha.png` als webfähige Assets unter `web/customer/public/images/...` bereitstellen | `frontend-eng` | `review-eng` |
| Warum-wir Layout | Ariadna- und Sascha-Profile als gleich große Cards/Spalten nebeneinander darstellen, responsiv gestapelt auf Mobile | `frontend-eng` | `review-eng` |
| Homepage Über-uns Layout | `AboutUs.tsx` auf gleichberechtigte Zweierdarstellung umbauen oder bewusst mit der Warum-wir-Darstellung harmonisieren | `frontend-eng` | `review-eng` |
| Content Preservation | Bereits gemachte Content-Anpassungen erhalten und nur Layout/Bildreferenzen gezielt ändern | `frontend-eng` | `product-own` |

**Abhängigkeiten:** keine  
**Parallelisierbar:** Asset-Kopie/Optimierung und Layout-Entwurf sind parallel vorbereitbar, finale Integration sequenziell.  
**Exit Gate:** Beide Profile nutzen neue Bilder, haben gleiche visuelle Hierarchie und bleiben auf Mobile sauber lesbar.

### Akzeptanzkriterien

- [x] Ariadna nutzt ein echtes Bild statt Initial-Platzhalter.
- [x] Sascha nutzt das neue Bild aus `Bilder/Sascha.png` statt des alten `/images/Sascha.jpg`, sofern das neue Bild qualitativ geeignet ist.
- [x] Auf Desktop stehen Ariadna und Sascha in `warum-wir/page.tsx` und `AboutUs.tsx` nebeneinander mit gleichen Card-Dimensionen, Bildgrößen und typografischer Gewichtung.
- [x] Auf Mobile stapeln die Profile kontrolliert ohne abgeschnittene Bilder oder Textüberlagerung.
- [x] Bereits vorhandene Content-Änderungen in `warum-wir/page.tsx` und `AboutUs.tsx` werden nicht versehentlich zurückgesetzt.

### Status

**Abgeschlossen.** Die neuen Portraits wurden nach `web/customer/public/images/founders/` übernommen. `warum-wir/page.tsx` und `AboutUs.tsx` nutzen jetzt gleichwertige Profilkarten mit identischem Bildformat, gleicher typografischer Gewichtung und responsiver Stapelung.

---

## Wave 2 - "Sicherheit & Vertrauen" kompakt mit "Mehr erfahren"

**Ziel:** Die Versicherungssektion erklärt Unfallversicherung und Betriebshaftpflicht direkt in den oberen Kacheln über eine hochwertige "Mehr erfahren"-Interaktion; der untere Bild-/Detailbereich entfällt.

| Workstream | Task | Primäres Skillset | Mitwirkend |
|-----------|------|-------------------|------------|
| UX Pattern | Darstellung für "Mehr erfahren" entwickeln: z. B. elegante Inline-Expansion, Drawer-ähnlicher Detailbereich oder Card-Aufklappung innerhalb des bestehenden Designs | `frontend-eng` | `review-eng` |
| Content Migration | Detailtexte aus den unteren Kacheln in die passenden oberen Kacheln übertragen | `frontend-eng` | `product-own` |
| Section Cleanup | Unteres Bild und die separaten Detailkacheln entfernen; Layout neu ausbalancieren | `frontend-eng` | `review-eng` |
| Accessibility | Buttons/Controls semantisch korrekt mit `aria-expanded` oder äquivalentem Zustand auszeichnen | `frontend-eng` | `testing-eng` |

**Abhängigkeiten:** Wave 1 kann unabhängig laufen; Umsetzung kann in derselben Datei kollidieren, daher final sequenziell integrieren.  
**Parallelisierbar:** UX-Pattern und Content-Mapping können parallel vorbereitet werden.  
**Exit Gate:** Die drei Sicherheitskacheln bleiben sichtbar; Unfallversicherung und Betriebshaftpflicht haben jeweils einen "Mehr erfahren"-Zugang mit Detailcontent; das untere Bild ist entfernt.

### Akzeptanzkriterien

- [x] In den Kacheln "Unfallversicherung" und "Betriebshaftpflicht" gibt es einen sichtbar benannten Button/Link "Mehr erfahren".
- [x] Der Detailcontent stammt fachlich aus den bisherigen unteren Kacheln.
- [x] Die Insolvenzversicherungs-Kachel bleibt konsistent dargestellt; falls kein zusätzlicher Detailtext existiert, bleibt sie ohne künstliche Füllkopie oder erhält nur eine kurze passende Erklärung.
- [x] Das Bild `/images/camp/Prio1.jpeg` wird in dieser Sektion nicht mehr als unteres Detailbild gerendert.
- [x] Die Interaktion funktioniert per Maus, Tastatur und Screenreader-Zustand.

### Status

**Abgeschlossen.** Die oberen Sicherheitskacheln enthalten jetzt die Detailtexte aus den früheren unteren Kacheln über semantische "Mehr erfahren"-Buttons mit `aria-expanded` und `aria-controls`. Der untere Bild-/Detailblock wurde entfernt.

---

## Wave 3 - Homepage Pricing: Highlights raus, "Was immer drin ist" kategorisieren

**Ziel:** Die Paketsektion wird schlanker: obsolete Highlightkacheln verschwinden, und die Inklusivleistungen werden in vier klar unterscheidbare Kategorien sortiert.

| Workstream | Task | Primäres Skillset | Mitwirkend |
|-----------|------|-------------------|------------|
| Content Model | `PRICING_EDITORIAL_CONTENT` um kategorisierte Included-Daten ergänzen oder bestehende Liste lokal sauber gruppieren | `frontend-eng` | `product-own` |
| Pricing UI | Highlightkachel-Rendering aus `Pricing.tsx` entfernen | `frontend-eng` | `review-eng` |
| Included Categories | Kategorien `Hotel`, `Training`, `Sicherheit`, `Erlebnis` mit eigener Überschrift, Icons/Farbakzent und ruhiger visueller Hierarchie gestalten | `frontend-eng` | `review-eng` |
| Regression Tests | Paket-/Content-Tests für Reihenfolge und Kategorie-Vollständigkeit ergänzen | `testing-eng` | `frontend-eng` |

**Abhängigkeiten:** keine harte Abhängigkeit zu Wave 1/2.  
**Parallelisierbar:** Content-Gruppierung und UI-Entwurf sind parallel möglich; Tests nach finalem Datenmodell.  
**Exit Gate:** Keine obsoleten Highlightkacheln mehr; "Was immer drin ist" zeigt genau vier Kategorien im passenden visuellen Stil.

### Akzeptanzkriterien

- [x] Texte wie "15 Stunden echtes Training! 7 Stunden lockere Einheit" werden nicht mehr als eigene Kacheln gerendert.
- [x] Der Abschnitt "Was immer drin ist" zeigt die vier Kategorien `Hotel`, `Training`, `Sicherheit`, `Erlebnis`.
- [x] Jede Kategorie besitzt eine visuelle Unterscheidung, die zur bestehenden OUT-Padel-Atmosphäre passt und nicht wie ein Fremdtheme wirkt.
- [x] Alle relevanten bisherigen Inklusivleistungen bleiben fachlich erhalten, nur sortiert und gestrafft.
- [x] Tests decken mindestens Paket-Slug-Reihenfolge und die Existenz der vier Kategorien ab.

### Status

**Abgeschlossen.** Die obsoleten Highlightkacheln wurden aus `Pricing.tsx` und dem Pricing-Content-Modell entfernt. `PRICING_EDITORIAL_CONTENT` führt die Inklusivleistungen jetzt in den vier Kategorien `Hotel`, `Training`, `Sicherheit` und `Erlebnis`; die Pricing UI rendert diese Kategorien mit eigenen Icons und Farbakzenten.

---

## Wave 4 - Paketkarten: Preisposition, Label Cleanup und Reihenfolge

**Ziel:** Die Paketkarten führen Nutzer klarer zum CTA: Preis direkt oberhalb des Buttons, keine unnötigen Label, zentrierte Preisdarstellung und verbindliche Reihenfolge.

| Workstream | Task | Primäres Skillset | Mitwirkend |
|-----------|------|-------------------|------------|
| Pricing Card Layout | Preisbereich vom oberen Card-Kopf in den unteren CTA-Bereich verschieben | `frontend-eng` | `review-eng` |
| Price Tile Alignment | "Preis" und Betrag innerhalb der Preiskachel zentrieren | `frontend-eng` | `review-eng` |
| Copy Cleanup | Label "Im Paket enthalten" entfernen, Featureliste ohne leeren Kontextbruch darstellen | `frontend-eng` | `product-own` |
| Package Ordering | Reihenfolge `Single`, `Doppel`, `Begleitperson`, `VIP` explizit absichern | `frontend-eng` | `testing-eng` |

**Abhängigkeiten:** Wave 3 berührt denselben Bereich in `Pricing.tsx`; Wave 4 sollte nach Wave 3 integriert werden.  
**Parallelisierbar:** Layout-Review und Testentwurf parallel; Code-Änderung sequenziell zu Wave 3.  
**Exit Gate:** Paketkarten sind scanbarer, der Preis steht unmittelbar vor der Entscheidung und die Reihenfolge ist stabil.

### Akzeptanzkriterien

- [x] In jeder Paketkarte steht der Preis oberhalb des CTA-Buttons.
- [x] "Preis" und Wert/Summe sind zentriert ausgerichtet.
- [x] Der Text "Im Paket enthalten" wird nicht mehr gerendert.
- [x] Reihenfolge der Paketkarten: Single, Doppel, Begleitperson, VIP.
- [x] Saisonpreis-Hinweis und "Preis auf Anfrage" bleiben fachlich korrekt.

### Status

**Abgeschlossen.** Der Preisbereich wurde in den unteren Entscheidungsbereich jeder Paketkarte verschoben und steht jetzt direkt über dem CTA. Die Preiskachel ist zentriert, das Label "Im Paket enthalten" wurde entfernt und die sichtbare Paket-Reihenfolge ist per Test abgesichert.

---

## Wave 5 - Experience temporär ausblenden, Navigation bereinigen

**Ziel:** Die Experience Section wird vorerst aus der Customer Journey entfernt, ohne die spätere Reaktivierung unnötig zu erschweren. Die Impressionen rücken direkt unter die Pakete.

| Workstream | Task | Primäres Skillset | Mitwirkend |
|-----------|------|-------------------|------------|
| Homepage Flow | Experience Section aus `web/customer/src/app/page.tsx` entfernen oder feature-flag-ähnlich ausblenden | `frontend-eng` | `product-own` |
| Navigation | `Experience` aus `Navigation.tsx` entfernen | `frontend-eng` | `review-eng` |
| Footer | `Experience` aus `Footer.tsx` entfernen; Footer-Spalte sauber halten | `frontend-eng` | `review-eng` |
| Reaktivierungsnotiz | In Plan oder Content-Doku festhalten, dass Experience später wiederkommen soll | `documentation-eng` | `product-own` |

**Abhängigkeiten:** Sollte nach Wave 3/4 erfolgen, weil dadurch die finale Reihenfolge `Pricing -> Gallery` geprüft wird.  
**Parallelisierbar:** Navigation/Footer und Homepage-Flow sind kleine getrennte Edits, können zusammen reviewed werden.  
**Exit Gate:** Keine sichtbaren Experience-Links; Gallery/Impressionen folgen direkt auf die Pakete.

### Akzeptanzkriterien

- [x] `/#experience` wird in Navigation und Footer nicht mehr verlinkt.
- [x] Die Startseite rendert nach `Pricing` direkt den `GallerySlider`.
- [x] Der Experience-Content ist nicht sichtbar, aber fachlich als später wieder zu implementieren dokumentiert.
- [x] Keine leeren Anker, ungenutzten Imports oder sichtbaren Layout-Lücken bleiben zurück.

### Status

**Abgeschlossen.** Die Experience Section wurde aus `page.tsx` entfernt, wodurch die Impressionen direkt auf die Paketsektion folgen. Navigation und Footer verlinken weder `/#experience` noch den dadurch ebenfalls entfernten Standort-Anker `/#location`; `docs/Content.md` hält die spätere Reaktivierung als gekoppelte Content-/Navigationsarbeit fest.

---

## Wave 6 - Gallery-Reihenfolge hotel-first

**Ziel:** Die Impressionen starten mit den aus Kundensicht wichtigsten Hotel-/Ausstattungsbildern und wechseln danach zu Courts und restlicher Atmosphäre.

| Workstream | Task | Primäres Skillset | Mitwirkend |
|-----------|------|-------------------|------------|
| Gallery Ordering | `HOMEPAGE_SLIDES` neu sortieren: Einzelzimmer, Doppelzimmer, Gym, Pool, danach Padel-Courts, danach Rest | `frontend-eng` | `product-own` |
| Copy Check | Titel, Alt-Texte und Captions auf neue Reihenfolge prüfen | `frontend-eng` | `review-eng` |
| Gallery Tests | Exakte erwartete Reihenfolge in `gallery.test.ts` aktualisieren | `testing-eng` | `frontend-eng` |

**Abhängigkeiten:** Keine harte Abhängigkeit zu Pricing, aber sinnvoll nach Wave 5, weil Gallery dann direkter sichtbar ist.  
**Parallelisierbar:** Sortierung und Testupdate sind gekoppelt und sollten zusammen umgesetzt werden.  
**Exit Gate:** Gallery-Test spiegelt die neue hotel-first Reihenfolge.

### Akzeptanzkriterien

- [x] Die ersten vier Gallery-Slides sind Einzelzimmer, Doppelzimmer, Gym und Pool.
- [x] Danach folgen Padel-Court-Motive, bevor sonstige Stimmungs-/Destination-Motive kommen.
- [x] Alle Slides bleiben eindeutig, mit Alt-Text, Titel und Caption.
- [x] `gallery.test.ts` ist auf die neue Reihenfolge aktualisiert.

### Status

**Abgeschlossen.** `HOMEPAGE_SLIDES` startet jetzt mit Einzelzimmer, Doppelzimmer, Gym und Pool. Danach folgen Court-Motive und anschließend die restlichen Team-, Morgenlicht-, Night- und Destination-Motive. `gallery.test.ts` spiegelt die neue Reihenfolge.

---

## Wave 7 - Qualität, Review und Abschluss

**Ziel:** Die Änderungen sind visuell geprüft, testseitig abgesichert und ohne unbeabsichtigte Rücksetzungen reviewbar.

| Workstream | Task | Primäres Skillset | Mitwirkend |
|-----------|------|-------------------|------------|
| Automated Tests | Relevante Customer-Web Tests ausführen | `testing-eng` | `frontend-eng` |
| Build Check | Customer-Web Build ausführen | `testing-eng` | `infrastructure-eng` |
| Visual Smoke | Desktop und Mobile für Startseite und Warum-wir prüfen | `frontend-eng` | `review-eng` |
| Final Review | Diff auf Scope-Treue, Content-Erhalt, ungenutzte Imports und responsive Brüche prüfen | `review-eng` | `product-own` |
| Documentation | `docs/Content.md` aktualisieren, falls neue Content-Struktur oder Asset-Pfade dokumentationsrelevant sind | `documentation-eng` | `product-own` |

**Abhängigkeiten:** Waves 1 bis 6.  
**Parallelisierbar:** Tests, Build und Review nach Implementierung parallel.  
**Exit Gate:** Tests/Build grün oder Blocker dokumentiert; keine offenen P0/P1 Review-Findings.

### Akzeptanzkriterien

- [x] `cd web/customer && npm test -- --configLoader runner` ist grün oder ein konkreter Blocker ist dokumentiert.
- [x] `cd web/customer && npm run build` ist grün oder ein konkreter lokaler Blocker ist dokumentiert.
- [x] Startseite zeigt die Reihenfolge `Hero -> Camps -> Pakete -> Impressionen -> Über uns/CTA/BookingGuide`.
- [x] Warum-wir und Homepage-Über-uns zeigen gleichberechtigte Profile; Warum-wir zeigt zusätzlich kompakte Versicherungsdetails ohne unteres Bild.
- [x] Keine Content-Anpassungen des Users wurden unbeabsichtigt überschrieben.

### Status

**Abgeschlossen.** Customer-Web Tests und Build sind grün. Der finale Review bestätigt die Startseitenreihenfolge, die gleichberechtigten Gründerprofile, die kompakte Versicherungssektion, die entfernten Experience-/Location-Links, die hotel-first Gallery und die aktualisierte Content-Dokumentation. Der lokale Smoke-Check lief über den Next-Dev-Server für `/` und `/warum-wir`; ein Screenshot-Smoke ist lokal nicht eingerichtet, da kein Headless-Browser-Tool im Customer-Paket oder Systempfad verfügbar ist.

---

## Critical Path

```text
Wave 1 (Portrait-Assets + Gründer-Layout)
    v
Wave 2 (Sicherheit & Vertrauen in derselben Seite)

Wave 3 (Pricing Included-Kategorien)
    v
Wave 4 (Paketkarten-Layout)
    v
Wave 5 (Experience ausblenden, Flow bereinigen)
    v
Wave 6 (Gallery-Reihenfolge)
    v
Wave 7 (Tests, Build, Visual Review)
```

Die beiden Hauptstränge `Warum-wir` und `Homepage/Pricing` können vorbereitet werden, sollten aber wegen gemeinsamer Dateien jeweils sequenziell integriert werden. Der kritischste Pfad liegt auf `Wave 3 -> Wave 4 -> Wave 5`, weil er die Startseitenstruktur und die Kaufentscheidung direkt betrifft.

---

## Agenten- und Skillset-Zuordnung

### Verwendete Skillsets

- `product-own` - Scope, Priorisierung, Akzeptanzkriterien, Reaktivierungsentscheidung für Experience
- `frontend-eng` - UI-Umsetzung, responsive Layouts, Assets, Interaktionsdesign
- `testing-eng` - Vitest-Updates, Build-/Regressionstests, Accessibility-Smoke
- `review-eng` - Diff-Review, visuelle Konsistenz, responsive Qualitätskontrolle
- `documentation-eng` - Content-Doku und Reaktivierungsnotiz für Experience
- `infrastructure-eng` - nur unterstützend, falls Build-/Asset-Pipeline lokal blockiert

### Mapping je Arbeitspaket

| Arbeitspaket | Primär | Mitwirkend |
|-------------|--------|------------|
| Wave 1: Portraits und Gründer-Layout | `frontend-eng` | `product-own`, `review-eng` |
| Wave 2: Sicherheit & Vertrauen | `frontend-eng` | `product-own`, `testing-eng`, `review-eng` |
| Wave 3: Included-Kategorien | `frontend-eng` | `product-own`, `testing-eng`, `review-eng` |
| Wave 4: Paketkarten-Layout | `frontend-eng` | `testing-eng`, `review-eng` |
| Wave 5: Experience ausblenden | `frontend-eng` | `documentation-eng`, `review-eng` |
| Wave 6: Gallery-Reihenfolge | `frontend-eng` | `testing-eng`, `review-eng` |
| Wave 7: Qualität und Abschluss | `testing-eng` | `frontend-eng`, `review-eng`, `documentation-eng`, `product-own` |

---

## Risiken und Mitigationen

| Risiko | Impact | Mitigation |
|:---|:---|:---|
| Neue Portraits sind sehr unterschiedlich zugeschnitten | Mittel | Einheitliche Aspect-Ratio-Container und `object-position` je Bild prüfen; bei Bedarf optimierte Web-Kopien erzeugen. |
| "Mehr erfahren" macht die Sicherheitskacheln unruhig | Mittel | Interaktion auf maximal einen geöffneten Detailbereich oder ruhige Inline-Expansion begrenzen; Mobile separat prüfen. |
| Entfernen der Experience Section erzeugt tote Links | Hoch | `Navigation.tsx`, `Footer.tsx` und interne Anker prüfen; `rg "#experience|/\\#experience"` als Review-Gate. |
| Pricing-Content wird doppelt gepflegt | Mittel | Kategorien im Content-Modell statt hart in JSX bevorzugen, falls es ohne Overengineering möglich ist. |
| Gallery-Test bricht wegen exakter Snapshot-Erwartung | Niedrig | `gallery.test.ts` bewusst mit neuer Reihenfolge aktualisieren. |
| User-Content-Anpassungen werden überschrieben | Hoch | Vor jedem Edit `git diff -- web/customer/src/app/warum-wir/page.tsx web/customer/src/components/AboutUs.tsx` prüfen und Änderungen gezielt erhalten. |

---

## Gesamt-Akzeptanzkriterien

- [x] Ariadna und Sascha werden auf "Warum wir?" und in der Homepage-Über-uns-Sektion gleichberechtigt nebeneinander vorgestellt.
- [x] Die neuen Bilder `Bilder/Ariadna.jpeg` und `Bilder/Sascha.png` sind als Customer-Web-Assets eingebunden.
- [x] "Sicherheit & Vertrauen" enthält "Mehr erfahren" in den relevanten Kacheln und übernimmt die bisherigen Detailtexte.
- [x] Das untere Bild und die unteren separaten Versicherungsdetailkacheln sind entfernt.
- [x] Obsolete Paket-Highlightkacheln sind von der Startseite entfernt.
- [x] "Was immer drin ist" ist in `Hotel`, `Training`, `Sicherheit` und `Erlebnis` gegliedert.
- [x] Paketkarten zeigen Preis/Summe zentriert und direkt oberhalb des Buttons; "Im Paket enthalten" ist entfernt.
- [x] Paket-Reihenfolge ist `Single`, `Doppel`, `Begleitperson`, `VIP`.
- [x] Experience ist vorerst ausgeblendet und aus Navigation/Footer entfernt.
- [x] Impressionen stehen direkt unter den Paketen.
- [x] Gallery startet mit Einzelzimmer, Doppelzimmer, Gym, Pool; danach Courts; danach Rest.
- [x] Relevante Frontend-Tests und Build sind grün oder nachvollziehbar blockiert.

---

## Definition of Done

Iteration 24 ist abgeschlossen, wenn die Kundenfeedback-Punkte auf Startseite und "Warum wir?" vollständig umgesetzt sind, die Experience Section ohne tote Navigation temporär ausgeblendet ist, die Gallery-Reihenfolge die neue Verkaufslogik abbildet, alle vorhandenen Content-Anpassungen erhalten bleiben und die Customer-Web-Qualitätsgates ohne offene P0/P1-Findings geschlossen wurden.
