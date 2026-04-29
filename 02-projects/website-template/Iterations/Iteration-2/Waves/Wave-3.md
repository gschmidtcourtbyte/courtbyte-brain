# Iteration 2 — Wave 3: Auth Service

**Skill:** service-eng, security-eng
**Status:** Done

## Tasks

- [x] Passwort-Hashing und Verification
- [x] Redis-basierte opaque Sessions mit TTL
- [x] Login-/Logout-Audit
- [x] Service-Methoden für Login, CurrentAdmin und Logout

## Acceptance

- Keine Tokens im LocalStorage.
- Redis-Session kann serverseitig widerrufen werden.
- Fehlgeschlagene Logins leaken nicht, ob ein Admin existiert.
