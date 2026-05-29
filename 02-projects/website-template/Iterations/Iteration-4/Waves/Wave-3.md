# Wave 3 — Services, Validierung, Rate Limits, Browser-Schutz

**Skill:** `/service-eng` + `/security-eng`
**Status:** Planned
**Blocked by:** Wave 2

## Ziel

Business-Logik für öffentliche Submissions bauen: Eingaben validieren,
normalisieren, persistieren und Missbrauch über Rate Limits + Origin-Prüfung
reduzieren.

## Tasks

### 1. Domain-Modelle

Neue Pakete nach bestehendem Backend-Stil:

- `backend/internal/submission/model.go`
- `backend/internal/submission/repository.go`
- `backend/internal/submission/service.go`

Modelle:

- `ContactInquiry`
- `CareerApplication`
- `ContactStatus`
- `ApplicationStatus`

### 2. Repository

- `InsertContactInquiry(ctx, input)`
- `InsertCareerApplication(ctx, input)`
- `ListContactInquiries(ctx, filters)`
- `ListCareerApplications(ctx, filters)`
- `UpdateContactInquiryState(ctx, id, status, read)`
- `UpdateCareerApplicationState(ctx, id, status, read)`

### 3. Service-Validierung

- Trim + Normalize Email lower-case.
- Max-Längen:
  - Name/Felder: konservativ 160 Zeichen
  - Subject/Position: 200 Zeichen
  - Message: 5000 Zeichen
- Email-Format pragmatisch prüfen.
- Pflichtfelder: Contact `name`, `email`, `subject`, `message`;
  Application `firstName`, `lastName`, `email`, `position`.
- IP/User-Agent nur speichern, nicht loggen.

### 4. Rate Limiting

- Redis-backed fixed window oder sliding window.
- Keys nach Route + IP:
  - Contact: z. B. 5 Requests / 10 Minuten
  - Applications: z. B. 3 Requests / 30 Minuten
- Bei Überschreitung: `429` mit JSON `{ "error": "rate_limited" }`.
- Wenn Redis nicht verfügbar ist: klarer `503`, kein ungeschützter Betrieb.

### 5. Browser-Origin-Schutz

- `FRONTEND_ORIGIN` aus Config nutzen.
- Für state-changing Browser-POSTs `Origin`/`Referer` gegen erlaubte Origin
  prüfen.
- Railway-Staging-Domain dokumentieren.

## Akzeptanzkriterien

- [ ] Service lehnt invalide Payloads deterministisch ab.
- [ ] Service speichert valide Contact/Application-Datensätze.
- [ ] Rate Limit ist route-spezifisch und IP-basiert.
- [ ] Überschreitung liefert `429`.
- [ ] Origin-Verstoß liefert `403`.
- [ ] Keine personenbezogenen Payloads in Logs.
- [ ] Unit-Tests für Validierung und Rate-Limit-Entscheidungen existieren.

## Out of Scope

- Captcha.
- Mail-Versand.
- Komplexe Anti-Spam-Heuristiken.
