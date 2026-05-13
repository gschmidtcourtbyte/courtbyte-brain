# Wave 1 - Bildpipeline und grosse Assets optimieren

**Iteration:** [[Iteration-34]]
**Skill:** frontend-eng
**Status:** Done

## Tasks

- [x] Next-Image-Konfiguration mit laengerer Cache-TTL und begrenzten Bildbreiten setzen - `web/customer/next.config.ts`
- [x] Grosse Camp-Bilder webtauglich neu speichern, ohne Dateipfade zu aendern - `web/customer/public/images/camp/*`
- [x] Founder-Bilder webtauglich neu speichern, ohne Dateipfade zu aendern - `web/customer/public/images/founders/*`
- [x] Bildgroessen vor/nach der Optimierung dokumentieren.

## Done Criteria

- [x] Kein verwendeter Bildpfad bricht.
- [x] Grosse Originalbilder sind deutlich kleiner.
- [x] Next-Build akzeptiert die neue Image-Konfiguration.

## Notes

Initialer Messbefund: `sunrise-club-overview.jpg` ca. 20 MB, `court-net-detail.jpg` ca. 10 MB, mehrere weitere Camp-Bilder 6 bis 7 MB. Cold `/_next/image`-Generierung fuer grosse Varianten dauerte 7 bis 9 Sekunden.

Umgesetzt am 2026-05-12: `images.deviceSizes` auf max. 1920 begrenzt und `minimumCacheTTL` auf 604800 gesetzt. 16 optimierte Camp-/Founder-Bilder schrumpften zusammen von 63.1 MB auf 4.5 MB; groesste Einzelreduktionen: `sunrise-club-overview.jpg` 20.4 MB -> 466 KB, `court-net-detail.jpg` 10.4 MB -> 272 KB, `sascha.png` 2.0 MB -> 541 KB.
