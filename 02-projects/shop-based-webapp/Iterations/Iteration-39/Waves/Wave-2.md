# Wave 2 - FAQ, Instagram und Pricing-UI

**Iteration:** [[Iteration-39]]
**Skill:** frontend-eng
**Status:** Done

## Tasks

- [x] FAQ-Frontend so umbauen, dass alle Fragen und Antworten dauerhaft sichtbar bleiben.
- [x] FAQ-Sektion visuell neu ordnen, ohne Marketing-Landingpage- oder Accordion-Verhalten einzufuehren.
- [x] Instagram-Social-Link auf `https://www.instagram.com/padel.out/` korrigieren.
- [x] Add-on-Preise im Checkout passend zur Auswahl anzeigen: Transfer, Vollpension und Einzeltrainerstunden.
- [x] Admin-/Response-nahe UI auf Preiswerte und Summen pruefen, damit die neuen Backend-Snapshots scanbar bleiben.

## Done Criteria

- [x] FAQ ist auf Mobile und Desktop ohne Ausklappen lesbar.
- [x] Social-Instagram-Link zeigt auf `padel.out`.
- [x] Checkout kommuniziert die Add-on-Preise ohne die optionale Auswahl zu erzwingen.

## Notes

Die FAQ soll schick dauerhaft ausgeklappt wirken. Fragen und Antworten bleiben Content aus der bestehenden Quelle; diese Wave fokussiert Darstellung und Link-/Preis-Kommunikation.

Umgesetzt am 2026-05-21:
- FAQ von Accordion auf dauerhaft sichtbare nummerierte Frage-Antwort-Liste umgebaut.
- Instagram-Social-Link auf das Profil `padel.out` umgestellt.
- Checkout zeigt `50 EUR`, `400 EUR` und `30 EUR` je Einzeltrainerstunde sowie eine Extras-Summe.
- Admin-Buchungstabelle zeigt die Add-on-Preisaufschluesselung neben Gesamt-, Anzahlungs- und Restbetrag.
- Verifiziert mit `npm test -- --configLoader runner src/lib/social-links.test.ts src/lib/faq-content.test.ts`, `npm run lint` und `npm run build`.
- Ad-hoc am 2026-05-21: Schriftzug `Initial priorisiert` fuer den ersten Camp-Explorer-Termin entfernt; weitere Camp-Eintraege behalten `Weitere Auswahl`.
