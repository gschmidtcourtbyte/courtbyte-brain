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

## Befunde aus [[Wave-2]] (hier zu beruecksichtigen)

- **Wordmark-Sublinie:** `siteConfig` hat kein eigenes Sublinie-Feld; `Wordmark`
  leitet "Unternehmensgruppe" per `slice` aus `companyNameLong` ab (robust, mit
  Fallback). Sauberer waere ein dediziertes Feld in `site-config.ts`
  (z. B. `companySubline`) - optionaler Cleanup in dieser Wave.
- **12-Grad kommt aus dem italic-Schnitt**, NICHT aus `skewX` (Manual fuehrt
  skewX als Fehlanwendung). Bei eigenen Schraegen-Anwendungen beachten:
  Wortmarke nie zusaetzlich skewen.
- Primitives sind gebaut, aber noch **nirgends importiert** (tree-shaken). Diese
  Wave webt sie ein: `Wordmark`, `SignalStripe`, `DiagonalClip`, `MonoLabel`,
  `StatFigure`.

## Befunde aus [[Wave-1]] (hier aufzuloesen)

1. **Veralteter Papier-Hex `#F5F3EE` hartcodiert** statt `t.bg`/`t.text` in:
   `components/Hero.tsx:104`, `components/PageHeader.tsx:18`,
   `components/StatsBar.tsx:51`, `app/(marketing)/ueber-uns/page.tsx:125`.
   -> auf Token bzw. `#F4F1EA` ziehen.
2. **`components/Testimonials.tsx:80`** — `fontFamily:'Georgia, serif'`
   hartcodiert, passt nicht zur CD (Archivo). Auf `--font-display`/`--font-body`
   ziehen oder Designentscheid einholen.
3. **`app/(marketing)/karriere/page.tsx`** (Z.133 Gradient, Z.160 background)
   liest jetzt `t.accent = #1C1C1A` (frueher Gelb) — kein Code-Fix noetig, aber
   **visuell pruefen** (Anthrazit statt Gelb-Gradient).
4. Schrift-Referenzen laufen sonst durchweg ueber `var(--font-*)` — Swap
   propagiert automatisch (Ausnahme: Punkt 2).
5. Viele `color:'#FFFFFF'/'#fff'` auf farbigen Flaechen — meist legitim (Text
   auf Rot/Anthrazit). Optional in Wave 3 gegen ein `t.onPrimary`-Token
   vereinheitlichen, falls gewuenscht.

## Notes

- 60/30/10 ist Leitplanke: Rot bleibt 10-%-Signal, nicht Flaeche.
- `/admin` bleibt unangetastet (Dark-only CMS, kein CD-Manual-Scope).
