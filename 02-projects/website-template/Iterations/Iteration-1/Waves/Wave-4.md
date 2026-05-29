# Wave 4 — Content Pages

**Iteration:** [[Iteration-1]]
**Skill:** frontend-eng
**Status:** Todo

## Tasks

- [ ] `frontend/components/PageHeader.tsx` — Hero-Light-Header für Subseiten
- [ ] `frontend/app/ueber-uns/page.tsx` — Story + Werte-Grid
- [ ] `frontend/app/ansprechpartner/page.tsx` — Team-Karten-Grid
- [ ] `frontend/app/karriere/page.tsx` — Job-Accordion + Bewerbungs-Modal mit Upload-UI + Initiativbewerbungs-Banner
- [ ] `frontend/app/kontakt/page.tsx` — Form (links) + Kontaktdaten + Kartenplatzhalter (rechts)
- [ ] `frontend/app/ressourcen/page.tsx` — Tab-Layout mit `?tab=` Query-Param + Sub-Komponenten:
  - [ ] `LeistungenTab` — 4 Karten, Detailtexte
  - [ ] `BlogTab` — 3 Cards (Titel, Datum, Cat, Bild)
  - [ ] `FaqTab` — Accordion
  - [ ] `ReferenzenTab` — 3 Karten mit Logo-Platzhalter, Industry, Result
- [ ] `frontend/lib/data.ts` ergänzen: `TEAM`, `JOBS`, `BLOG_POSTS`, `FAQS`, `REFERENZEN`

## Done Criteria

- [ ] Alle 5 Subseiten erreichbar via Navbar
- [ ] Karriere-Modal öffnet beim Klick auf "Jetzt bewerben", zeigt Upload-Drop-Zone, Submit löst Erfolgs-State aus
- [ ] Kontakt-Form-Submit zeigt Erfolgs-State

## Notes

- Forms posten in dieser Iteration noch nicht zur API — `e.preventDefault()` + lokaler `sent`-State (wie in der Designvorlage).
- Datei-Upload ist visuell nur Drop-Zone; reales Multipart-Upload kommt mit dem Backend in Iteration 3.
