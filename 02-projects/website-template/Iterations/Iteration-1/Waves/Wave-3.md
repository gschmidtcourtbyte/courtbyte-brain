# Wave 3 — Home (Hero, Sections, Gallery+Lightbox)

**Iteration:** [[Iteration-1]]
**Skill:** frontend-eng
**Status:** Todo

## Tasks

- [ ] `frontend/components/Blobs.tsx` + `ScrollHint.tsx` + `HeroText.tsx`
- [ ] `frontend/components/heroes/HeroSplit.tsx`, `HeroFullBg.tsx`, `HeroFloatingCard.tsx`
- [ ] `frontend/components/Hero.tsx` — Variant-Router
- [ ] `frontend/components/FeaturesStrip.tsx` (3 Karten)
- [ ] `frontend/components/StatsBar.tsx` — animated Counter (IntersectionObserver Trigger)
- [ ] `frontend/components/LeistungenPreview.tsx` (4 Karten)
- [ ] `frontend/components/Testimonials.tsx` — auto-rotating Karussell (5,5s Interval)
- [ ] `frontend/components/Process.tsx` — 4-Schritte-Timeline
- [ ] `frontend/components/Gallery.tsx` — Filter-Tabs + Grid + Lightbox + Keyboard-Nav
- [ ] `frontend/lib/data.ts` — `NAV_ITEMS`, `RESSOURCEN_TABS`, `GALLERY`, `LEISTUNGEN`, `STATS`, `TESTIMONIALS`, `PROCESS_STEPS`
- [ ] `frontend/app/page.tsx` — Komposition aller Home-Sektionen + Footer

## Done Criteria

- [ ] Home-Page rendert Hero + Features + Stats + Leistungen + Gallery + Testimonials + Process + Footer in dieser Reihenfolge
- [ ] Hero-Variante ist live über Tweaks umschaltbar
- [ ] Gallery-Filter + Lightbox (Keyboard ←/→/ESC) funktionieren

## Notes

- Counter-Animation startet erst beim Sichtbar-Werden, sonst „verbraucht" sie sich
  vor dem Scrollen (IntersectionObserver).
- Testimonial-Karussell pausiert auf Hover.
