# Open Assets — Bodo Westerholt

**Stand:** 2026-05-30
**Eltern:** [[Projektplan]], [[Iteration-1]], [[Wave-3]]
**Zweck:** Zentrale Liste, was vom Kunden noch fehlt — Gespraechs- und
Statusleitfaden.

## Status

Alle Listen in `frontend/lib/data.ts` sind auf Tiefbau-Sprache umgestellt,
aber inhaltlich **noch Platzhalter mit "N. N."** oder "TODO: vom Kunden"
markiert. Bilder zeigen Picsum-Stubs (Seeds beginnen mit `wh-`) — auf der
Startseite trägt das Hero-Foto zusätzlich ein "IMG-STUB · ersetzen" Label.

## Was vom Kunden noch fehlt

### Bilder

- [ ] **Hero-Bild** — eine eigene Baustelle, ideal Erdbau oder Kanalbau,
      Querformat ca. 900×700 px, gut belichtet
- [ ] **Galerie / Projekte** — pro Projekt: 1-2 Bilder, plus echter
      Projektname, Kategorie, Kurzbeschreibung mit Kennzahl
      (m³, m, DN, Bauzeit). Quellen für aktuelle GALLERY-Stubs
      (Erschliessung, Kanalsanierung, Strassenbau, Rueckbau,
      Regenrueckhaltebecken, Hausanschluss-Sammelmassnahme).
- [ ] **Team-Portraits** — Geschaeftsfuehrung, Bauleitung, Disposition,
      Personal. Format quadratisch, schlichter Hintergrund oder eigene
      Werkstatt/Baustelle.
- [ ] **Maschinenpark** — Bagger, Walzen, Lkw — für eine kuenftige
      "Geraete"-Sektion (Iter. 2).
- [ ] **Logo als SVG** — falls verfügbar. Aktuell liegt nur das JPG
      (`/frontend/public/logo-westerholt.jpg`) — fuer das weisse Schild-Card
      ausreichend, fuer Print/SEO ideal wäre SVG.

### Texte

- [ ] **Echte Stellenanzeigen** — die 4 JOBS sind Platzhalter mit
      typischen Berufsbildern. Welche Stellen sind aktuell wirklich offen?
- [ ] **Echte Referenzen** — Auftraggeber-Namen mit Freigabe zur
      Nennung. Welche 3-6 Projekte sind die "Visitenkarte"?
- [ ] **Testimonials** — Zitate von Auftraggebern, namentlich mit Freigabe.
- [ ] **Ueber-uns** — Gruendungsjahr, Standort(e), Eigentuemerstruktur,
      Werte. Aktuell nur Branche und Region in Mono-Akzenten verbaut.
- [ ] **Tagline-Pruefung** — aktuelle Tagline:
      *"Erdbau. Kanalbau. Strassenbau. Seit Jahrzehnten im Boden zuhause."*
      → Soll *"seit XX Jahren"* mit konkretem Jahr werden.
- [ ] **Westfalen-Bezug** — Footer und PageHeader nennen
      "Tiefbau · Westfalen". Stimmt das? Andere Region? Mehrere Standorte?

### Stammdaten

- [ ] **Adresse** (`siteConfig.address`) — aktuell `Musterstrasse 1,
      00000 Musterstadt`
- [ ] **Telefon** (`siteConfig.contactPhone`) — aktuell `+49 0000 000000`
- [ ] **E-Mail** (`siteConfig.contactEmail`) — aktuell `info@westerholt.de`
      (Annahme — bitte verifizieren)
- [ ] **Oeffnungszeiten / Bürozeiten** (`siteConfig.hours`) — aktuell
      `Mo–Fr: 7:00–17:00 Uhr` als Annahme
- [ ] **WhatsApp / Instagram URLs** — aktuell leere Stubs. Existieren
      offizielle Kanaele?

### Rechtliches

- [ ] **Impressum** — Firmierung (Bodo Westerholt Unternehmensgruppe
      GmbH? GmbH & Co. KG? Einzelunternehmen?), Geschaeftsfuehrung,
      Sitz, Registergericht, HRB, USt-IdNr.
- [ ] **Datenschutzerklaerung** — aktuelle, anwaltlich gepruefte Fassung
- [ ] **AGB** — falls erforderlich/gewuenscht
- [ ] **Zertifikate / Praequalifikationen** — z. B. Praequalifizierung
      Bau, GU-Zertifikate, ISO. Ggf. eigene Sektion sinnvoll.

### Funktionale Lücken

- [ ] **Kontaktformular-Empfaenger** — Backend-Routing fuer Anfragen
      (siehe Iteration 2, Backend-Basischeck)
- [ ] **Karriere-Upload** — Wohin sollen Bewerbungs-Anhänge gehen?
      Mailbox / S3 / lokal?
- [ ] **Analytics / Cookie-Banner** — gewuenscht oder nicht?

## Wie damit umgehen

Diese Liste ist die Tagesordnung fuer das naechste Kunden-Gespraech. In
Iteration 2 werden Backend-Konfiguration und erste Unterseiten-Inhalte
gemacht — dafuer brauchen wir vom Kunden mindestens Adresse, Kontaktdaten,
Impressums-Basisdaten und 2-3 echte Referenzen mit Bild.

Bilder duerfen auch nach und nach kommen — die Picsum-Stubs sind klar als
solche markiert und stoeren nicht das Look-&-Feel.
