# Wave 1 — Admin-Theme-Tokens, Auth-Gate Layout, Login-Redesign

**Skill:** `/frontend-eng`
**Status:** Done (2026-04-29)
**Blocked by:** —

## Outcome

- `frontend/lib/theme.ts`: `AdminTheme`-Interface + Top-Level-Constant `ADMIN_THEME` (Dark-only) ins `Theme`-Objekt verdrahtet
- `frontend/app/admin/login/page.tsx` neu — Blob-Bg, 420 px Card, Logo-Initialen aus TweaksProvider, Spinner-Submit, Error-Banner
- `frontend/app/admin/(authed)/layout.tsx` neu — Auth-Gate, Loading-Card, Redirect bei 401
- `frontend/app/admin/(authed)/page.tsx` neu — Stub-Dashboard mit Logout
- `frontend/app/admin/layout.tsx` neu — lädt `admin.css` für beide Branches
- `frontend/app/admin/admin.css` neu — `@keyframes spin`
- `frontend/app/admin/page.tsx` gelöscht
- Build + Lint clean

## Notes für Folge-Waves

- Marketing-Navbar/Footer rendert weiterhin um `/admin`-Routen — Wave 2 muss
  das per Route-Group `(marketing)` für die öffentliche Site oder per
  Conditional-Rendering im Root-Layout adressieren.
- Doppelter `/admin/api/me`-Fetch (Layout + Stub-Page) — Wave 2 löst das
  mit einem `SessionProvider`-Context.
- `tweaks.logoInitials` aus TweaksProvider funktioniert über das Root-Layout.

## Ziel

Foundation für die Admin-Optik schaffen: Dark-Theme-Tokens in `lib/theme.ts`,
Route-Group mit Auth-Gate, Login-Page-Redesign matching `Backend CMS.html`.

## Tasks

### 1. Admin-Theme-Tokens in `frontend/lib/theme.ts`

Neuer `admin`-Namespace mit Dark-only-Tokens. Werte 1:1 aus `Backend CMS.html`
(Konstante `C` ab Zeile 42):

```ts
admin: {
  bg:       '#0F1419',
  sidebar:  '#0A0F14',
  surface:  '#1A2332',
  surfaceB: '#1E2D40',
  border:   '#263548',
  success:  '#22C55E',
  warning:  '#F59E0B',
  danger:   '#EF4444',
  text:     '#E8EDF2',
  muted:    '#94A3B8',
  dim:      '#64748B',
}
```

`primary` und `accent` werden aus dem bestehenden Theme-Object wiederverwendet —
NICHT erneut hardcoden. Hook `useTheme()` muss `theme.admin` exposed machen.

### 2. Route-Group `app/admin/(authed)/`

- `app/admin/(authed)/layout.tsx` — Client-Component mit Auth-Gate:
  - Ruft `/admin/api/me` beim Mount.
  - Während Loading: zeigt eine schlanke Loading-Card (kein Sidebar-Flash).
  - Bei `401`: `router.replace('/admin/login')`.
  - Bei `200`: rendert `<AdminShell>` (Wave 2 liefert die Komponente; Wave 1
    legt nur einen Stub mit `children` und einem TODO-Kommentar an).
- Existing `app/admin/page.tsx` (aktuelle Login+Stub-Seite) wird **bewegt** zu
  `app/admin/(authed)/page.tsx` und auf einen Dashboard-Stub reduziert
  (Wave 3 baut den echten Dashboard).

### 3. Login-Page `app/admin/login/page.tsx`

- Neuer Route. Übernimmt die Login-Form-Logik aus aktuellem
  `app/admin/page.tsx` (POST `/admin/api/login`, success → `router.push('/admin')`).
- Visual 1:1 aus `LoginScreen` in `Backend CMS.html` (ab Zeile 797):
  - Dark-Background mit zwei blurred Color-Blobs (top-left + bottom-right).
  - Zentrierte Card (`maxWidth: 420`, `borderRadius: 20`, `boxShadow`).
  - Logo-Square 52×52 mit Initialen aus `site-config`.
  - Headline "CMS Backend" + Untertitel.
  - Email + Password Inputs mit Focus-Border-Color-Switch auf `primary`.
  - "Passwort vergessen?" Link (visual nur, kein Handler).
  - Submit-Button mit Loading-Spinner.
  - Demo-Hint unten ("Demo: …") **weglassen** — wir haben echtes Backend.
- Error-State: roter Banner über dem Submit (matching Design).

### 4. Cleanup

- Aktuelle `app/admin/page.tsx` wird in `app/admin/(authed)/page.tsx` umgezogen
  (Stub-Dashboard).
- Auth-Logik (`/admin/api/me`-Check + Redirect) wandert komplett ins
  Layout — die Section-Pages selbst kennen kein Auth mehr.

## Akzeptanzkriterien

- [ ] `theme.admin` ist typisiert und in `useTheme()` verfügbar.
- [ ] Aufruf `/admin` ohne Cookie → Redirect zu `/admin/login`.
- [ ] Aufruf `/admin/login` rendert das neue Design (Blobs, Card, Inputs).
- [ ] Login mit gültigen Credentials → Redirect zu `/admin` (Stub-Dashboard
      matched dem authed-Layout).
- [ ] Logout-Funktionalität bleibt aus Iteration 2 erhalten (auch wenn
      Stub-Dashboard noch keinen Button rendert — Sidebar kommt in Wave 2).
- [ ] `npm run build` und `npm run lint` clean.

## Out of Scope (in Wave 1)

- Sidebar, HeaderBar, geteilte Atoms (Wave 2)
- Section-Pages (Wave 3 + 4)
- Mock-Daten-Layer (Wave 2)

## Kontext

- **Design-Referenz:** `/tmp/design-bundle/website-template/project/Backend CMS.html`
  (Das Bundle ist in Iteration-3-Planning entpackt; Coding-Agent muss es
  gegebenenfalls erneut über die User-URL fetchen + ungzippen.)
- **Theme-File:** `frontend/lib/theme.ts`
- **Site-Config:** `frontend/lib/site-config.ts` (Initialen, Firmenname)
- **Bestehende Auth-API-Routes:** `app/admin/api/{login,logout,me}/route.ts`
  bleiben unverändert.
