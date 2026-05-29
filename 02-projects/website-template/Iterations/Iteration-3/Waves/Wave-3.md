# Wave 3 — Dashboard-Page + Anfragen-Page

**Skill:** `/frontend-eng`
**Status:** Done (2026-04-29)
**Blocked by:** Wave 2 (Done)

## Outcome

- `/admin` rendert das Dashboard mit Greeting, KPI-Cards, BarChart,
  DonutChart und Recent-Listen für Anfragen/Bewerbungen.
- Greeting nutzt die Session-Email aus `useAdminSession()` und formatiert das
  aktuelle Datum clientseitig mit `Intl.DateTimeFormat('de-DE')`.
- `/admin/anfragen` angelegt: lokale Master-Detail-Inbox mit Read-State,
  Status-Toggles und Antwortformular-Stub.
- Dashboard-Links navigieren via Next-Link zu `/admin/anfragen` und
  `/admin/karriere`.
- Verification: `npm run lint`, `npm run typecheck`, `npm run build` clean.

## Ziel

Die zwei datenintensiven Sections bauen: Dashboard (Stats + Charts +
Recent-Listen) und Anfragen-Inbox (Master-Detail mit Status-Toggle und
Antwort-Form).

## Tasks

### 1. `app/admin/(authed)/page.tsx` — Dashboard

Komplettes Redesign matching `Dashboard` ab Zeile 208 in `Backend CMS.html`:

- **Greeting-Block** oben:
  - "Guten Tag, Admin 👋" (Email-Local-Part aus `/admin/api/me` — fallback "Admin")
  - Untertitel mit aktuellem deutschen Wochentag + Datum (`Intl.DateTimeFormat('de-DE')`)
- **4-Spalten-Stat-Grid** mit `STATS` aus `admin-mock.ts`:
  - Icon, Delta-Badge (grün/rot), Wert, Label
  - `card-hover`-Effekt (translateY + box-shadow)
- **Charts-Row** (2fr 1fr Grid):
  - Linke Card: BarChart "Besucherstatistik · Letzte 7 Tage" + "Woche"-Pill
  - Rechte Card: DonutChart "Anfragen-Status · Alle offenen Anfragen"
- **Recent-Listen-Row** (1fr 1fr Grid):
  - Linke Card: "Neue Anfragen" — top 4 aus `INQUIRIES` mit Avatar-Initial,
    Name + Read-Dot, Subject, Datum. "Alle ansehen →"-Link (Next-Link zu
    `/admin/anfragen`).
  - Rechte Card: "Neue Bewerbungen" — top 4 aus `APPLICATIONS` mit
    Akzent-Avatar, Name, Position, StatusBadge. Link zu `/admin/karriere`.

### 2. `app/admin/(authed)/anfragen/page.tsx` — Anfragen-Inbox

Matching `AnfragenView` ab Zeile 332 in `Backend CMS.html`:

- **Header:** H1 "Anfragen".
- **2-Spalten-Layout** wenn ein Inquiry selected, sonst 1 Spalte:
  - **Linke Card (List):**
    - Card-Header: "Alle Anfragen" + Badge mit Anzahl ungelesener.
    - Liste: Avatar mit Initial, Name + Read-Dot, Subject, Datum + StatusBadge.
    - Click auf Row → setSelected + markRead (lokaler State).
  - **Rechte Card (Detail):** wird nur gerendert wenn `selected`:
    - Header: Name, Email, Datum, Close-Button (×).
    - Subject + Body in `surfaceB`-Box.
    - Status-Toggle-Pills: Neu / In Bearbeitung / Erledigt — Click ändert
      lokalen State.
    - Antwort-Form: Textarea + Submit-Button (Submit ist No-Op,
      console.log nur als Stub-Marker).

State bleibt **lokal** in der Component (`useState<Inquiry[]>(INQUIRIES)`).
Keine API-Calls. `markRead` und `setStatus` mutieren nur lokalen State.

## Akzeptanzkriterien

- [x] Dashboard-Render matched Design pixel-nah (Stats-Cards, BarChart,
      DonutChart, Recent-Listen).
- [x] Stat-Cards haben `card-hover`-Animation.
- [x] "Alle ansehen →"-Links navigieren via Next-Link zu Anfragen / Karriere.
- [x] Greeting verwendet `me.email` und aktuelle deutsche Datum-Formatierung.
- [x] Anfragen-Liste: Click selektiert + markiert als gelesen (Read-Dot weg).
- [x] Detail-Panel öffnet/schließt korrekt; Status-Pills toggeln lokalen State.
- [x] `npm run build` + `npm run lint` clean.

## Out of Scope (in Wave 3)

- Karriere / Seiten / Galerie / Einstellungen (Wave 4)
- Echte API-Calls für Anfragen-CRUD (spätere Iteration)

## Kontext

- **Design-Referenz:** `Backend CMS.html` Zeilen 208–329 (Dashboard),
  331–430 (AnfragenView)
- **Mock-Daten:** `lib/admin-mock.ts` aus Wave 2
- **Atoms:** `AdminCard`, `BarChart`, `DonutChart`, `StatusBadge` aus Wave 2
