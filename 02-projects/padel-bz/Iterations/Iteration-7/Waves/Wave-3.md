# Wave 3: Scroll-/Responsive-Verhalten

**Status:** Done (Legacy-Migration)
**Skill:** frontend-eng, review-eng
**Quelle:** `plan/iterations/iteration-7.md`

## Legacy-Inhalt

### Wave 3 — Scroll-/Responsive-Verhalten
**Skillsets**: frontend-eng, review-eng
**Tasks**: 7.4
**Ziel**: Logo klebt beim Scrollen zentriert an der Navbar; Links und Mobile-Menue bleiben bedienbar.

## Legacy-Status

### Wave 3 — Scroll-/Responsive-Verhalten: done
- `.nav-scroll-logo` ist absolut zur Navbar-/Viewport-Mitte positioniert.
- Auf Hero-Seiten bleibt das kleine Logo mit der Navbar initial ausgeblendet und erscheint mit `body.nav-visible`.
- Auf Nicht-Hero-Seiten bleibt die Navbar direkt sichtbar; das transparente Logo dient dort als zentrierter Home-Link.
- Nav-Links und Mobile-Toggle liegen mit `z-index:102` ueber dem Logo, damit Bedienung bei engen Breiten nicht blockiert wird.
- Unter `1040px` werden die Textlinks ausgeblendet, damit das zentrierte Logo nicht mit der Linkgruppe kollidiert.
