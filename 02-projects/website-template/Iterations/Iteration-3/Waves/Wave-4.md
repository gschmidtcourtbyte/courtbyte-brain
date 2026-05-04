# Wave 4 — Karriere, Seiten, Galerie, Einstellungen

**Skill:** `/frontend-eng`
**Status:** Done (2026-04-29)
**Blocked by:** Wave 2 (Atoms + Mock; Done); Wave 3 nicht nötig (parallel zu Wave 3 möglich)

## Outcome

- `/admin/karriere` angelegt mit Bewerbungs-/Stellenanzeigen-Tabs,
  Bewerbungs-Master-Detail, Status-Toggles und lokalem Job-Aktiv-State.
- `/admin/seiten` angelegt mit 5-spaltiger Tabelle, StatusBadges und deutscher
  Zahlenformatierung.
- `/admin/galerie` angelegt mit visual-only Upload-Zone und 9er Grid aus
  `GALLERY_ITEMS`/`picsum.photos`-Seeds.
- `/admin/einstellungen` angelegt mit Website- und Social-Media-Cards aus
  `SETTINGS_DATA`.
- Verification: `npm run lint`, `npm run typecheck`, `npm run build` clean.

## Ziel

Die verbleibenden vier Sections bauen — alle mit Mock-Daten und lokalem State.

## Tasks

### 1. `app/admin/(authed)/karriere/page.tsx` — Karriereportal

Matching `KarriereView` ab Zeile 433 in `Backend CMS.html`:

- **Header:** H1 "Karriereportal" + Untertitel.
- **Tab-Bar:** "Bewerbungen" (mit Count-Pill für `Neu`) + "Stellenanzeigen".
  Tab-Border-Bottom matching Design.

**Tab "Bewerbungen":**
- Master-Detail wie in Anfragen, aber mit `APPLICATIONS`:
  - List: Avatar (Akzent-Farbe), Name, Position, Datum, StatusBadge.
  - Detail: Name + Position-Header, 2-Spalten-Info-Grid (Email, Telefon, Position, Datum),
    Mock-Document-Box (PDF-Icon + Download-Button), Motivationsschreiben-Box,
    Status-Toggle-Pills (Neu / Im Gespräch / Angenommen / Abgelehnt).
- Status mutiert lokalen State.

**Tab "Stellenanzeigen":**
- Top-Right: "+ Neue Stelle anlegen"-Button (No-Op).
- Liste von Job-Cards mit: Title, Tags (Dept / Type / Loc), Bewerbungs-Count,
  "Bearbeiten" + Active-Toggle-Buttons.
- Active-Toggle ändert lokalen State.

### 2. `app/admin/(authed)/seiten/page.tsx` — Seiten

Matching `SeitenView` ab Zeile 617:

- **Header:** H1 "Seiten & Inhalte" + Untertitel + "+ Neue Seite"-Button (No-Op).
- **Tabelle** in einer AdminCard ohne Padding:
  - Header-Row: 5 Spalten (`2fr 1.5fr 1fr 1fr 1fr`):
    Seite / URL / Status / Aufrufe / Geändert (uppercase, dim).
  - Data-Rows aus `PAGES_DATA`: Name, monospace-URL, StatusBadge,
    deutsche Zahlenformatierung (`toLocaleString('de')`), Datum.
  - `table-row:hover`-Effekt.

### 3. `app/admin/(authed)/galerie/page.tsx` — Galerie

Matching `GalerieView` ab Zeile 654:

- **Header:** H1 "Galerie" + Untertitel + "+ Medien hochladen"-Button (No-Op).
- **Upload-Zone:** Dashed-Border-Box mit "📤"-Icon, "Dateien hier ablegen",
  "oder auswählen" (visual; kein echter Upload).
- **3-Spalten-Grid** mit `GALLERY_ITEMS` (`picsum.photos`-Seeds):
  - Card mit `aspect-ratio: 3/2` Image + Footer mit Name + "JPG · 240 KB".
  - `card-hover`-Effekt.

### 4. `app/admin/(authed)/einstellungen/page.tsx` — Einstellungen

Matching `EinstellungenView` ab Zeile 699:

- **Header:** H1 "Einstellungen".
- **Form** mit `maxWidth: 720`:
  - Card "Website" mit Feldern: Firmenname, Domain, Email, Telefon.
  - Card "Social Media" mit Feldern: WhatsApp-Nummer, Instagram-Handle.
  - Inputs sind `defaultValue`-prefilled aus `SETTINGS_DATA` (lokaler State
    via uncontrolled inputs reicht für die Optik-Iteration).
- "Änderungen speichern"-Button am Ende (No-Op).

## Akzeptanzkriterien

- [x] Karriere-Tabs funktionieren (Tab-Wechsel resetet `selected`).
- [x] Bewerbungs-Detail rendert alle Mock-Felder + Document-Box.
- [x] Job-Cards toggeln Active-State lokal.
- [x] Seiten-Tabelle rendert alle 5 Zeilen mit korrekten Spalten +
      deutscher Zahlenformatierung.
- [x] Galerie-Grid rendert 9 Bilder via `picsum.photos`-Seeds.
- [x] Galerie-Upload-Zone ist dashed + visual-only.
- [x] Einstellungen-Form rendert beide Cards mit allen Feldern.
- [x] `npm run build` + `npm run lint` clean.

## Out of Scope (in Wave 4)

- Echter Datei-Upload
- Echte Settings-Persistenz
- Echte Stellen-/Bewerbungs-CRUD

## Kontext

- **Design-Referenz:** `Backend CMS.html` Zeilen 433–614 (KarriereView),
  617–651 (SeitenView), 654–696 (GalerieView), 699–734 (EinstellungenView)
- **Atoms:** AdminCard, StatusBadge aus Wave 2
- **Mock-Daten:** `lib/admin-mock.ts` aus Wave 2
