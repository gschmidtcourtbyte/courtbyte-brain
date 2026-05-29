# Iteration 1 - Frontend-Kundentouch (Tiefbau Branding)

**Status:** Code-Tasks Done — Visuelle Abnahme offen
**Repo:** /home/andersen/git/bodo-westerholt
**Datum:** 2026-05-29 (Start) · 2026-05-30 (Wave 2+3 abgeschlossen)

## Ziel

Das Frontend traegt komplett die Bodo-Westerholt-Handschrift: Theme-Tokens,
Logo, Display-Schrift und Form-Sprache sind auf die Corporate-Design-Vorgabe
([[Corporate-Design]]) angepasst. Komponenten und Inhalte sind so weit
ueberarbeitet, dass die Seite nicht mehr wie ein generisches Consulting-
Template wirkt, sondern wie eine Tiefbau-Praesenz.

## Scope

**In:**
- Theme-Tokens auf neue Schwarz/Rot-Palette
- Logo als Asset einbinden, Site-Config mit echten Stammdaten
- Display-Schrift Big Shoulders Display, Mono-Akzent JetBrains Mono
- Form-Sprache global anpassen (Radien runter, harte Linien)
- Visueller Refresh aller wichtigen Komponenten (Hero, Navbar, Footer,
  StatsBar, Features, Process, Gallery, PageHeader)
- Emoji-Icons durch SVG-Strich-Icons ersetzen
- `data.ts` von Mittelstand-Consulting auf Tiefbau-Sprache umstellen
- Bild-Platzhalter kuratiert, offene Asset-Luecken im Brain dokumentiert

**Out:**
- Finale Kundenbilder (warten auf Lieferung)
- Final ausformulierte Legal-/Impressum-Texte
- Backend-/Deployment-Anpassungen (in Iteration 2)
- CMS-Anbindung (in Iteration 3)

## Waves

| Wave | Skill | Focus | Status |
|---|---|---|---|
| [[Wave-1]] | frontend-eng | Theme-Foundation, Logo, Fonts, Form-Sprache | Done |
| [[Wave-2]] | frontend-eng | Komponenten-Refresh, Icons, Layout | Done |
| [[Wave-3]] | frontend-eng | Tiefbau-Content in `data.ts`, Asset-Lueckenliste | Done |

## Acceptance Criteria

- [ ] `theme.ts` enthaelt die Corporate-Palette aus [[Corporate-Design]]
- [ ] `site-config.ts` traegt Firmenname "Bodo Westerholt Unternehmensgruppe",
      Initialen "BW", Branchen-Tagline (Tiefbau)
- [ ] Logo unter `frontend/public/` eingebunden und im Header/Footer sichtbar
- [ ] Big Shoulders Display + DM Sans + JetBrains Mono geladen und in
      globalen Tokens referenziert
- [ ] Keine Emoji-Icons mehr in oeffentlichen Komponenten
- [ ] Hero zeigt Tiefbau-Tagline statt "Mittelstandspartner"-Copy
- [ ] `data.ts` enthaelt Tiefbau-Leistungen, -Stats, -Prozessschritte
- [ ] Mobile und Desktop ohne offensichtliche Layout-Probleme
- [ ] Offene Asset-Luecken in einer Brain-Notiz festgehalten

## Notes

- Verschoben aus Originalplan: die urspruengliche Wave 2
  (Backend/Deployment-Basischeck) wandert nach Iteration 2 - sie ist nicht
  von Kundenmaterial blockiert, konkurriert aber hier um Aufmerksamkeit.
- Default-Theme ist Dark (Asphalt). Light-Mode bleibt funktional verfuegbar.
- Wenn waehrend der Komponenten-Wave Bedarf entsteht, eine generische
  Konfigurations-Luecke zu schliessen (z. B. Logo-Variante, Display-Font als
  Token), gehoert das nach `theme.ts` / `site-config.ts` - nicht in die
  Komponente.
