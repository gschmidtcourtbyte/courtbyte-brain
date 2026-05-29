# Wave 3 - Tiefbau-Content & Asset-Lueckenliste

**Iteration:** [[Iteration-1]]
**Skill:** frontend-eng
**Status:** Done (2026-05-30)
**Referenz:** [[Corporate-Design]], [[Open-Assets]]

## Tasks

- [x] `frontend/lib/data.ts` komplett auf Tiefbau umgestellt:
  - `LEISTUNGEN`: Erdbau, Kanalbau, Strassenbau, Rueckbau — `icon`-Feld
    ist jetzt ein Key in ICON_MAP (`excavator`/`pipe`/`road`/`demolition`)
  - `HOME_STATS`: Abgeschlossene Baustellen (+), bewegte Erde p.a. (tsd. m³),
    Jahre im Tiefbau, Mitarbeitende
  - `HOME_PROCESS`: Erstgespraech → Vermessung & Planung →
    Ausfuehrung → Abnahme; `icon`-Feld optional (Process zeigt Nummer)
  - `JOBS`: Baugeraetefuehrer, Vorarbeiter Tiefbau, Polier, Azubi
    Tiefbaufacharbeiter — Standort "Westfalen", TODO-Kommentar
  - `TESTIMONIALS`/`REFERENZEN`: Kommunen, Stadtwerke, Generalunternehmer
    als anonymisierte Platzhalter ("N. N.")
  - `FAQS`: Tiefbau-Fragen (Angebot, Privatkunden, Genehmigungen,
    Witterung, Bauzeit, Begleitung)
  - `GALLERY`: 6 Tiefbau-Projekte mit Kategorien Erdbau/Kanalbau/
    Strassenbau/Rueckbau, deskriptive Picsum-Seeds (`wh-*`)
  - `BLOG_POSTS`: 3 Tiefbau-News-Stubs
  - `TEAM`: 4 Rollen (GF, Bauleitung, Disposition, Personal) als "N. N."
- [x] `frontend/app/(marketing)/ressourcen/page.tsx`: LeistungenTab nutzt
      jetzt `getIcon(l.icon)` statt Emoji-Text-Rendering; Karten scharfkantig
- [x] `frontend/app/(marketing)/ueber-uns/page.tsx`: Komplette Neufassung —
      neue Tiefbau-Werte (DIN-Treue, Sicherheit, Handfest) mit IcSurveyor/
      IcHelmet/IcShovel statt 🎯🤝🌱; neuer Display-Titel "Im Boden zuhause."
      und PageHeader-Breadcrumb; alle Konsulting-Floskeln entfernt
- [x] Bild-Placeholder: Picsum-Seeds gegen deskriptive Tiefbau-Slugs
      ausgetauscht (`wh-erdbau-01`, `wh-kanal-01`, `wh-strasse-01`, ...).
      Hero und Ueber-uns zeigen zusaetzlich Mono-Label "IMG-STUB · ersetzen"
- [x] `tagline` in `site-config.ts` finalisiert:
      "Erdbau. Kanalbau. Strassenbau. Seit Jahrzehnten im Boden zuhause."
- [x] [[Open-Assets]] im Brain angelegt und im [[Projektplan]] verlinkt —
      konkrete Liste an Kundenmaterial fuer kommende Iterationen

## Done Criteria

- [x] Keine generischen Consulting-Begriffe ("Mittelstandspartner",
      "98% Kundenzufriedenheit") mehr auf oeffentlichen Seiten
- [x] `data.ts`-Listen in Sprache und Inhalt Tiefbau-spezifisch
- [x] [[Open-Assets]] existiert im Brain und ist verlinkt
- [x] `npm run lint` sauber
- [x] `npm run typecheck` sauber
- [x] `npm run build` sauber
- [ ] **Offen: manuelle Sichtkontrolle** der Seiten Startseite, `/ueber-uns`,
      `/karriere`, `/ansprechpartner`, `/ressourcen` (alle Tabs), `/kontakt`

## Notes

Texte hier sind bewusst Platzhalter mit Tiefbau-Sprache - kein Marketing-
Hochglanz. Sobald der Kunde echte Texte und Bilder liefert, ersetzt eine
spaetere Iteration die Stubs. Das `Open-Assets.md` ist die zentrale
"warten auf Kunde"-Liste und sollte beim Status-Update mit dem Kunden
als Gespraechsleitfaden dienen.
