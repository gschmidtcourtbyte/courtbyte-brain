# Wave 2 - Bilder ohne Zoom, Crop oder manuelle Ausschnittverschiebung

**Iteration:** [[Iteration-25]]
**Skill:** frontend-eng
**Status:** Done

## Ziel

Alle in Iteration 24 betroffenen hochgeladenen Bilder werden so angezeigt, wie sie hochgeladen wurden: vollstaendig sichtbar, ohne `object-cover`, ohne manuelle `object-position`, ohne Hover-/Animations-Zoom.

## Tasks

- [x] `web/customer/src/app/warum-wir/page.tsx`: Ariadna- und Sascha-Bilder von Crop-Darstellung auf voll sichtbare Darstellung umstellen.
- [x] `web/customer/src/components/AboutUs.tsx`: Ariadna- und Sascha-Bilder analog umstellen, damit Homepage und "Warum wir?" konsistent bleiben.
- [x] `web/customer/src/app/warum-wir/page.tsx`: Netzwerkbild `/images/camp/Prio3.jpg` pruefen und ebenfalls ohne Crop/Zoom darstellen, sofern es sichtbar beschnitten wird.
- [x] `web/customer/src/components/GallerySlider.tsx`: Gallery-Slides von `object-cover` auf voll sichtbare Darstellung umstellen.
- [x] Betroffene Container so gestalten, dass unterschiedliche Seitenverhaeltnisse nicht zu Layout-Spruengen fuehren.
- [x] Keine Bilddateien selbst veraendern, zuschneiden, neu exportieren oder ersetzen.

## Akzeptanzkriterien

- [x] In den betroffenen Founder-Komponenten gibt es kein `object-cover` und keine `object-[...]` Positionsklasse mehr.
- [x] In `GallerySlider.tsx` wird das sichtbare Slide-Bild nicht per `object-cover` beschnitten.
- [x] Keine neue Scale-/Zoom-Klasse wird fuer diese Bilder eingefuehrt.
- [x] Hochformatige Bilder wie `Ariadna.jpeg`, `Sascha.png` und `Prio1.JPG` bleiben vollstaendig sichtbar.
- [x] Querformatige Bilder bleiben vollstaendig sichtbar und werden nicht in ein unpassendes Festformat hineingezoomt.
- [x] Bildcontainer bleiben auf Desktop und Mobile professionell: keine Textueberlagerung, keine Layout-Spruenge, kein abgeschnittener CTA.

## Abhaengigkeiten

Wave 1 sollte vorher integriert sein, weil `warum-wir/page.tsx` gemeinsam betroffen ist.

## Notes

Umgesetzt in `warum-wir/page.tsx`, `AboutUs.tsx` und `GallerySlider.tsx`. `object-cover`, manuelle `object-[...]` Positionen, der Netzwerkbild-Overlay und die Gallery-Caption-Overlay wurden entfernt. Der Frontend-Review-Agent fand zusaetzlich die `fadeIn`-Scale-Animation; diese wurde aus `GallerySlider.tsx` entfernt. Bildassets wurden nicht veraendert.
