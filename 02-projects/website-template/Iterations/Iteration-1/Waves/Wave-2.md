# Wave 2 — Theme System + Layout (Navbar/Footer/Logo)

**Iteration:** [[Iteration-1]]
**Skill:** frontend-eng
**Status:** Todo

## Tasks

- [ ] `frontend/lib/theme.ts` — `BASE_THEMES` (light, dark) + `buildTheme(base, primary, accent)` 1:1 aus Designvorlage
- [ ] `frontend/lib/site-config.ts` — `companyName`, `logoInitials`, `whatsappUrl`, `instagramUrl`
- [ ] `frontend/lib/tweaks/TweaksProvider.tsx` — React-Context für Theme + heroVariant + companyName + logoInitials
- [ ] `frontend/lib/tweaks/TweaksPanel.tsx` — Dev-only floating Panel mit Theme-Toggle, Color-Picker, Hero-Variant-Radio, Text-Inputs
- [ ] `frontend/app/layout.tsx` — globale CSS-Resets, Animationen (`@keyframes blobFloat1/2/3`, `fadeUp`, `pulseRing`, …), `<TweaksProvider>` Wrapper
- [ ] `frontend/components/Logo.tsx` — Initialen-Square + Firmenname
- [ ] `frontend/components/Navbar.tsx` — sticky-on-scroll, Desktop-Links inkl. Ressourcen-Dropdown, WhatsApp + Instagram Icons, Kontakt-CTA, einfacher Mobile-Drawer
- [ ] `frontend/components/Footer.tsx` — Logo, Spalten-Navigation, Social-Links
- [ ] `frontend/components/icons/*.tsx` — `IcWhatsApp`, `IcInstagram`, `IcChevron` als eigene Files

## Done Criteria

- [ ] Theme-Switch Hell/Dunkel funktioniert live, alle Komponenten respektieren `t`
- [ ] Tweaks-Panel ist nur in Dev sichtbar (NODE_ENV check), in Prod-Build nicht enthalten
- [ ] Navbar & Footer sehen identisch zur Designvorlage aus

## Notes

- TweaksProvider hält den `tweaks` State im Memory — kein localStorage, da nur Dev-Tool.
- Per-Kunde sollen statt Tweaks-Panel die Werte direkt in `lib/site-config.ts` und `lib/theme.ts` gepinnt werden (Tweaks-Panel ist nur Iterations-Tool für interne Vorschau).
