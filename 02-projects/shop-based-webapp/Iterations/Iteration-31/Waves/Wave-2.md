# Wave 2 - FAQ "Wie buche ich bei euch?" ergaenzen

**Iteration:** [[Iteration-31]]
**Skill:** frontend-eng
**Status:** Done

## Ziel

Die Customer-Web-App beantwortet in der bestehenden FAQ-Struktur die Frage "Wie buche ich bei euch?" mit dem abgestimmten Buchungsablauf.

## Agent

Frontend Engineer: Fokus auf bestehende Next.js-/React-Patterns, zentrale Content-Quelle, responsive Darstellung und Erhalt des OUT Padel Designs.

## Tasks

- [x] Bestehende FAQ-Struktur in `web/customer/src/lib/faq-content.ts`, `web/customer/src/components/FAQ.tsx` und `web/customer/src/app/faq/page.tsx` pruefen.
- [x] Neuen FAQ-Eintrag mit Frage `Wie buche ich bei euch?` in `FAQ_ENTRIES` ergaenzen.
- [x] Antworttext in der bestehenden Content-Konvention formulieren und die Pflichtinhalte erhalten.
- [x] Sicherstellen, dass der neue Eintrag auf der FAQ-Seite und in allen FAQ-Komponenten gerendert wird.
- [x] Keine neue FAQ-Komponente, kein neues CMS-Modell und keinen neuen API-Endpoint anlegen.
- [x] Falls vorhandene Tests fuer FAQ-Content existieren, diese erweitern; sonst Build/Lint als Mindestpruefung nutzen.

## Zieltext

Frage:

```text
Wie buche ich bei euch?
```

Antwort:

```text
Ganz einfach - schreib uns via "Anfrage senden", WhatsApp oder per E-Mail. Wir antworten persoenlich, klaeren alle Fragen und schicken dir eine Buchungsbestaetigung inkl. des Sicherungsscheins der Insolvenz-Versicherung zu. Mit der Anzahlung in Hoehe von 20 % ist dein Platz verbindlich reserviert. Alle weiteren Details folgen per Mail von uns. Sollte etwas offen sein, ruf einfach an: 0155-67149871
```

## Akzeptanzkriterien

- [x] Die FAQ enthaelt die Frage `Wie buche ich bei euch?`.
- [x] Die Antwort nennt `Anfrage senden`, WhatsApp und E-Mail als Kontaktwege.
- [x] Die Antwort nennt persoenliche Rueckmeldung und Klaerung offener Fragen.
- [x] Die Antwort nennt Buchungsbestaetigung inklusive Sicherungsschein der Insolvenz-Versicherung.
- [x] Die Antwort nennt die Anzahlung in Hoehe von 20 Prozent als verbindliche Reservierung.
- [x] Die Antwort nennt weitere Details per Mail und die Telefonnummer `0155-67149871`.
- [x] Die FAQ-Seite rendert ohne Layout- oder TypeScript-Regression.

## Abhaengigkeiten

Keine harte technische Abhaengigkeit zu Wave 1. Sequenziell geplant, weil der Gesamtumfang klein ist.

## Notes

Bei der Umsetzung duerfen Umlaute und typografische Zeichen gemaess bestehender Customer-Web-Content-Konvention verwendet werden. Die fachlichen Pflichtinhalte aus dem Zieltext muessen erhalten bleiben.

Umsetzung 2026-05-04:
- `web/customer/src/lib/faq-content.ts` um den Eintrag `Wie buche ich bei euch?` ergaenzt.
- Antworttext enthaelt Anfrage senden, WhatsApp, E-Mail, persoenliche Klaerung, Buchungsbestaetigung inkl. Sicherungsschein der Insolvenz-Versicherung, 20 % Anzahlung, weitere Details per Mail und `0155-67149871`.
- Keine neue FAQ-Komponente, kein CMS-Modell und kein API-Endpoint angelegt; `FAQ.tsx` rendert weiter zentral aus `FAQ_ENTRIES`.
- Neuer Test `web/customer/src/lib/faq-content.test.ts` prueft die Pflichtinhalte.
- Verifikation gruen: `npm test -- --configLoader runner src/lib/faq-content.test.ts`.
