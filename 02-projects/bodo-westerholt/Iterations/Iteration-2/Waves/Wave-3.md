# Wave 3 - Anwendung auf Seiten & Bestandskomponenten

**Iteration:** [[Iteration-2]]
**Skill:** frontend-design
**Status:** Geplant
**Referenz:** [[CD-Manual]]
**Voraussetzung:** [[Wave-2]] (Primitives) abgeschlossen

## Zweck

Primitives + neues Typo-System ausrollen, sodass die Live-Seite dem Manual
entspricht (60/30/10, Schraege, Mono-Daten).

## Tasks

- [ ] `frontend/components/Navbar.tsx` + `Footer.tsx`: Bild-Logo durch
      `Wordmark` ersetzen (oder als bewusster Fallback behalten - Entscheid bei
      Logo-Qualitaet im Header dokumentieren).
- [ ] `frontend/components/Hero.tsx` + `HeroText.tsx`: Headline Archivo Black,
      `DiagonalClip` als Bildschnitt, `SignalStripe` als Akzentkante.
- [ ] `frontend/components/StatsBar.tsx`: auf `StatFigure` + Mono-Einheiten
      (kg / m3 / t) umstellen.
- [ ] Section-Eyebrows ueberall -> `MonoLabel` (FeaturesStrip, Process,
      Testimonials, Gallery, LeistungenPreview, PageHeader).
- [ ] Kontakt/Ansprechpartner-Seiten: Telefon/E-Mail/Adresse/Oeffnungszeiten
      in IBM Plex Mono (technische Schrift wie im Manual S. 09/17).
- [ ] `frontend/components/Blobs.tsx` entfernen + Verwendung aus
      `app/(marketing)/layout.tsx` herausloesen (Flaechenruhe statt Blobs).
- [ ] Schraegen-Divider zwischen Sektionen (`DiagonalClip`/`SignalStripe`)
      sparsam einsetzen - 10-%-Regel fuer Rot beachten.

## Done Criteria

- [ ] `npm run build` + `npm run lint` sauber (TS strict)
- [ ] Keine `Blobs`-Referenz mehr im Marketing-Layout
- [ ] Wordmark im Header/Footer sichtbar, Mindestbreite >= 120 px gewahrt
- [ ] Kontaktdaten + Stat-Zahlen in Mono
- [ ] Mobile + Desktop ohne offensichtliche Layout-Probleme

## Notes

- 60/30/10 ist Leitplanke: Rot bleibt 10-%-Signal, nicht Flaeche.
- `/admin` bleibt unangetastet (Dark-only CMS, kein CD-Manual-Scope).
- Befunde aus [[Wave-1]] (hardcodierte Schrift/Farbe in Komponenten) hier
  aufloesen.
