# Wave 1 - Rechtstexte aktualisieren

**Iteration:** [[Iteration-32]]
**Skill:** frontend-eng
**Status:** Done

## Ziel

Die Customer-Web-App zeigt die neue Datenschutzerklaerung und die erneuerten Allgemeinen Geschaeftsbedingungen von Padel-Out in der bestehenden Legal-Seiten- und Content-Struktur.

## Agent

Frontend Engineer: Fokus auf bestehende Next.js-/React-Patterns, zentrale Content-Quellen, semantische Textstruktur, responsive Darstellung und Erhalt des vorhandenen OUT Padel Designs.

## Tasks

- [x] Bestehende Legal-Struktur fuer Datenschutz und AGB in `web/customer` finden.
- [x] Bestehende Routen und Links fuer Datenschutz/AGB identifizieren.
- [x] Datenschutzerklaerung durch die gelieferte Kundenfassung ersetzen.
- [x] AGB durch die erneuerte Kundenfassung ersetzen.
- [x] Listen, Abschnitte und Paragrafen semantisch lesbar in die vorhandene Komponentenkonvention uebertragen.
- [x] Keine juristischen oder fachlichen Umformulierungen vornehmen.
- [x] Keine neuen Backend-, DB-, API-, CMS- oder Admin-Funktionen anlegen.
- [x] Sicherstellen, dass Umlaute, `§`, Euro-Angaben, Prozentwerte und Links korrekt gerendert werden.
- [x] Falls vorhandene Content-Tests existieren, diese zielgerichtet erweitern.

## Akzeptanzkriterien

- [x] Datenschutzerklaerung enthaelt die Ueberschrift `Padel-Out Datenschutzerklaerung`.
- [x] Datenschutzerklaerung enthaelt `Gemaess Art. 13 und 14 DSGVO | Stand: April 2026`.
- [x] Datenschutzerklaerung enthaelt die Abschnitte 1 bis 10 aus der Kundenvorlage.
- [x] AGB enthalten die Ueberschrift `Padel-Out Allgemeine Geschaeftsbedingungen`.
- [x] AGB enthalten die Paragrafen `§ 1 Geltungsbereich` bis `§ 12 Schlussbestimmungen`.
- [x] AGB verwenden die erneuerte Kundenfassung mit `Padel-Out` als Schreibweise.
- [x] AGB § 6.1 nennt `info@padel-out.de`.
- [x] AGB § 4.3 nennt den Flughafentransfer als optional buchbare, nicht enthaltene Leistung.
- [x] Bestehende Legal-Routen bleiben erhalten.
- [x] Kein Code ausserhalb des Customer-Web-Legal-/Content-Kontexts wird geaendert.

## Abhaengigkeiten

Keine. Diese Wave ist der Einstiegspunkt der Iteration.

## Quelltext AGB

```text
Padel-Out
Allgemeine Geschäftsbedingungen

§ 1 Geltungsbereich
Diese Allgemeinen Geschäftsbedingungen gelten für alle Buchungen von Padel Training Camps, die über Padel-Out angeboten werden.
§ 2 Veranstalter / Vertragspartner
Veranstalter und dein direkter Vertragspartner ist:

Padel-Out
c/o Glaswerk Oldenburg
Ariadna Ortega Orriols
Emsstraße 18
26135 Oldenburg
info@padel-out.de
Steuer-Nummer: 64/132/09880
Telefon: +49 155 567149871

Padel-Out ist Reiseveranstalter im Sinne des § 651a BGB und damit für alle vertraglich vereinbarten Pauschalreiseleistungen verantwortlich.
§ 3 Vertragsschluss
3.1 Buchungsanfrage
Du kannst eine unverbindliche Anfrage per WhatsApp, E-Mail oder über unsere Website stellen. Auf Basis deiner Anfrage erstellen wir dir ein individuelles, befristetes Angebot.
3.2 Vertragsschluss
Der Pauschalreisevertrag kommt mit Übersendung der schriftlichen Buchungsbestätigung durch Padel-Out zustande. Die Buchungsbestätigung wird per E-Mail zugesandt und enthält alle wesentlichen Vertragsinhalte.
3.3 Vorvertragliche Informationen
Vor Vertragsschluss stellen wir dir gemäß Art. 250 §§ 1 bis 3 EGBGB alle wesentlichen Reiseinformationen zur Verfügung, insbesondere:
Wesentliche Eigenschaften der Reiseleistungen
Den Gesamtreisepreis inklusive aller Steuern und Gebühren
Zahlungsmodalitäten und Fälligkeitstermine
Mindestteilnehmerzahl und Rücktrittsfristen
Informationen zu Reisepass- und Visumpflicht
Informationen zum Insolvenzschutz

§ 4 Leistungsbeschreibung
4.1 Vertragsgegenstand
Der Umfang der vertraglich vereinbarten Reiseleistungen ergibt sich aus der Leistungsbeschreibung auf unserer Website, den Angaben in der Buchungsbestätigung sowie den ergänzenden Informationen, die dir vor Vertragsschluss mitgeteilt werden.
4.2 Standardleistungen
Unsere Padel-Camp-Pakete am Standort Sant Esteve Sesrovires bei Barcelona (Spanien) umfassen regelmäßig:
5 Übernachtungen im 4-Sterne-Superior-Hotel mit Halbpension (Frühstück + Mittagessen)
15 Stunden Padel-Training mit professionellen Trainern
7 Stunden lockere Trainingseinheiten am Nachmittag (Americanos, Matches gegen lokale Spieler, freies Spiel)
Getränke und Snacks am Court während des Trainings
Zugang zu Pool, Spa und Fitnessstudio der Hotelanlage
Willkommens- und Abschiedspaket
Unfallversicherung und Haftpflichtversicherung für den gebuchten Zeitraum
Insolvenzabsicherung gemäß § 651r BGB
4.3 Nicht enthaltene Leistungen
An- und Abreise zum Zielort
Transfer Flughafen „el Prat“ in Barcelona – Hotel und zurück (Optional buchbar)
Abendessen (Empfehlungen werden ausgesprochen)
Persönliche Ausgaben und Trinkgelder
Reiserücktrittsversicherung (wird ausdrücklich empfohlen)
4.4 Leistungsänderungen
Änderungen einzelner Reiseleistungen sind zulässig, wenn sie nach Vertragsschluss notwendig werden, nicht von Padel-Out treuwidrig herbeigeführt wurden und den Gesamtzuschnitt der Reise nicht erheblich beeinträchtigen. Du wirst darüber unverzüglich informiert.

§ 5 Reisepreise und Zahlung
5.1 Gesamtpreis
Der in der Buchungsbestätigung genannte Reisepreis ist ein Gesamtpreis und umfasst alle in § 4.2 aufgeführten Leistungen. Er versteht sich pro Person.
5.2 Aktuelle Pakete
Single-Paket (€ 1.799,-)
Doppel-Paket (€ 1.499,- p.P.)
Begleitperson-Paket (€ 799,-)
VIP-Paket (Preis auf Anfrage – da individual vereinbart. Ab ca. € 2.299,-)
Maßgeblich ist stets der im individuellen Angebot und in der Buchungsbestätigung genannte Preis.
5.3 Anzahlung
Bei Vertragsschluss wird eine Anzahlung in Höhe von 20 Prozent des Reisepreises fällig. Die Anzahlung ist nach Erhalt der Buchungsbestätigung und des Sicherungsscheins zahlbar.
5.4 Restzahlung
Die Restzahlung (80 Prozent des Reisepreises) ist spätestens 50 Tage vor dem vereinbarten Anreisedatum fällig.
5.5 Kurzfristige Buchung
Erfolgt die Buchung weniger als 50 Tage vor Anreise, ist der gesamte Reisepreis sofort bei Vertragsschluss fällig.
5.6 Zahlungsverzug
Kommt der Reisende mit der Zahlung in Verzug, ist Padel-Out nach Mahnung und angemessener Fristsetzung berechtigt, vom Vertrag zurückzutreten und Stornierungskosten gemäß § 6 zu verlangen.
5.7 Preisanpassungen
Padel-Out behält sich Preisanpassungen gemäß § 651f BGB vor, sofern sich nach Vertragsschluss die Beförderungskosten, Abgaben oder Wechselkurse ändern. Preisanpassungen sind nur bis 20 Tage vor Reisebeginn zulässig und werden unverzüglich mitgeteilt. Bei einer Preiserhöhung von mehr als 8 Prozent bist du berechtigt, kostenfrei vom Vertrag zurückzutreten.
§ 6 Rücktritt durch den Reisenden
6.1 Rücktrittsrecht
Du kannst jederzeit vor Reisebeginn vom Vertrag zurücktreten (§ 651h BGB). Maßgeblich ist der Zugang der Rücktrittserklärung bei Padel-Out. Wir empfehlen, den Rücktritt schriftlich per E-Mail an info@padel-out.de zu erklären.
6.2 Stornierungspauschalen
Trittst du zurück, erhebt Padel-Out folgende Stornierungspauschalen. Diese sind so bemessen, dass sie die tatsächlich anfallenden Kosten durch Hotelstornierung, Trainerausfall und Verwaltungsaufwand abdecken, ohne den Reisenden unangemessen zu belasten:

Bis 50 Tage vor Anreise: 20 Prozent des Reisepreises (entspricht der Anzahlung)
49 bis 45 Tage vor Anreise: 30 Prozent des Reisepreises
44 bis 30 Tage vor Anreise: 50 Prozent des Reisepreises
29 bis 15 Tage vor Anreise: 70 Prozent des Reisepreises
Ab 14 Tage vor Anreise oder bei Nichtantritt (No-Show): 100 Prozent des Reisepreises

Die Stornierungspauschalen ergeben sich aus folgenden tatsächlichen Kostenstrukturen:
Hotel berechnet ab 44 Tagen: 20 Prozent des Zimmerpreises
Hotel berechnet ab 30 Tagen: 10 Prozent des Zimmerpreises (kumulativ)
Hotel berechnet ab 14 Tagen: 100 Prozent des Zimmerpreises
Zusätzlich anfallende Kosten für Trainer,Courts-Miete, Verpflegung und Organisation
6.3 Ersatzperson
Du kannst bis zum Reisebeginn verlangen, dass eine Ersatzperson in deine Rechte und Pflichten aus dem Reisevertrag eintritt (§ 651e BGB). Etwaige Mehrkosten durch den Eintritt der Ersatzperson gehen zu deinen Lasten. Padel-Out kann dem Eintritt widersprechen, wenn die Ersatzperson die Reiseerfordernisse nicht erfüllt.
6.4 Empfehlung Reiserücktrittsversicherung
Wir empfehlen ausdrücklich den Abschluss einer Reiserücktrittskostenversicherung sowie einer Reiseabbruchversicherung bei einem Versicherer deiner Wahl, um dich gegen unvorhergesehene Ereignisse abzusichern.

§ 7 Rücktritt und Änderungen durch Padel-Out
7.1 Mindestteilnehmerzahl
Padel-Out kann vom Vertrag zurücktreten, wenn die Mindestteilnehmerzahl von 8 Personen nicht erreicht wird. In diesem Fall wirst du unverzüglich, spätestens 20 Tage vor Reisebeginn, über die Absage informiert. Alle bereits geleisteten Zahlungen werden vollständig und unverzüglich zurückerstattet.
7.2 Höhere Gewalt
Padel-Out kann vor Reisebeginn vom Vertrag zurücktreten, wenn die Durchführung der Reise durch unvermeidbare, außergewöhnliche Umstände erheblich beeinträchtigt wird (§ 651h Abs. 4 BGB), etwa durch Naturkatastrophen, Pandemien oder politische Unruhen. Du erhältst in diesem Fall den vollständigen Reisepreis zurück.
7.3 Wesentliche Leistungsänderungen
Wird Padel-Out gezwungen, nach Vertragsschluss wesentliche Eigenschaften der Reiseleistung zu ändern, wirst du unverzüglich informiert. Du kannst dann die Änderung annehmen, vom Vertrag kostenlos zurücktreten oder eine Ersatzreise in Anspruch nehmen, sofern eine angeboten wird.

§ 8 Reisemangel und Gewährleistung
8.1 Mangelanzeige
Stellst du während der Reise einen Mangel fest, bist du verpflichtet, diesen unverzüglich gegenüber Padel-Out oder dem Leistungsträger vor Ort anzuzeigen und Abhilfe zu verlangen. Unterlassung der Mangelanzeige kann zu Minderung oder Ausschluss von Ansprüchen führen.
8.2 Abhilfe
Ist die Reise mangelhaft, kannst du Abhilfe verlangen (§ 651k BGB). Padel-Out ist berechtigt, die Abhilfe auf andere Weise zu leisten, wenn die verlangte Abhilfe unverhältnismäßige Kosten verursachen würde. Wird Abhilfe nicht innerhalb angemessener Frist geleistet, kannst du selbst Abhilfe schaffen und Ersatz der erforderlichen Aufwendungen verlangen.
8.3 Preisminderung und Schadensersatz
Für die Dauer eines Reisemangels kannst du eine Minderung des Reisepreises verlangen (§ 651m BGB). Schadensersatzansprüche gemäß § 651n BGB bleiben hiervon unberührt.
8.4 Verjährung
Ansprüche wegen Reisemängeln verjähren in zwei Jahren. Die Verjährungsfrist beginnt mit dem Tag, an dem die Reise vertragsmäßig enden sollte.

§ 9 Haftung
Wir haften nicht für Schäden, die durch leichte Fahrlässigkeit entstehen, es sei denn, es handelt sich um die Verletzung wesentlicher Vertragspflichten. Die Haftung ist auf vorhersehbare Schäden begrenzt.
Die Teilnahme am Training erfolgt auf eigene Gefahr. Wir haften nicht für Verletzungen oder Unfälle, die während des Trainings entstehen, sofern uns kein vorsätzliches oder grob fahrlässiges Verhalten zur Last fällt.

§ 10 Versicherungsschutz
10.1 Im Reisepreis enthaltene Versicherungen
Folgende Versicherungen sind im Reisepreis aller Pakete (außer Begleitperson: nur Unfall- und Haftpflicht) bereits enthalten:

Unfallversicherung (Versicherer: Occident): Deckt Unfälle ab, die während der Padel-Camp-Aktivitäten auftreten. Leistungen: Todesfall 10.000 Euro, Invalidität bis 10.000 Euro, Krankenhauskosten bis 1.800 Euro, erste Prothese bis 900 Euro, Krankentransport bis 750 Euro.
Betriebshaftpflichtversicherung (Versicherer: Allianz): Deckungssumme bis 1.200.000 Euro pro Schadenfall, bis 3.600.000 Euro pro Jahr. Deckt Personen-, Sach- und Vermögensschäden im Zusammenhang mit unserer Veranstaltertätigkeit.
Insolvenzabsicherung gemäß § 651r BGB (Versicherer: R+V Allgemeine Versicherung AG): Sichert deine geleisteten Zahlungen im Falle einer Insolvenz des Veranstalters ab. Den Sicherungsschein erhältst du zusammen mit der Buchungsbestätigung.


10.2 Nicht enthaltene Versicherungen
Eine Reiserücktrittsversicherung und eine Reiseabbruchversicherung sind nicht im Reisepreis enthalten.

§ 11 Datenschutz
Die Verarbeitung deiner personenbezogenen Daten erfolgt gemäß unserer Datenschutzerklärung und den Vorgaben der Datenschutz-Grundverordnung (DSGVO). Dort erfährst du, welche Daten wir erheben, zu welchem Zweck wir sie verarbeiten, an welche Dritte wir sie weitergeben (insbesondere ans Hotel in Spanien zur Erfüllung gesetzlicher Meldepflichten) und welche Rechte dir zustehen.
§ 12 Schlussbestimmungen
12.1 Anwendbares Recht
Es gilt das Recht der Bundesrepublik Deutschland unter Ausschluss des UN-Kaufrechts (CISG).
12.2 Gerichtsstand
Gerichtsstand für Klagen des Reisenden gegen Padel-Out ist der Sitz des Veranstalters Gerichtsstand für Klagen von Padel-Out gegen den Reisenden ist der Sitz des Veranstalters.
12.3 Salvatorische Klausel
Sollten einzelne Bestimmungen dieser AGB unwirksam sein oder werden, berührt dies die Wirksamkeit der übrigen Bestimmungen nicht. Anstelle der unwirksamen Bestimmung tritt die gesetzliche Regelung.
12.4 Online-Streitbeilegung
Die Europäische Kommission stellt eine Plattform zur Online-Streitbeilegung (OS) bereit: https://ec.europa.eu/consumers/odr. Padel-Out ist weder verpflichtet noch bereit, an einem Streitbeilegungsverfahren vor einer Verbraucherschlichtungsstelle teilzunehmen.
```

## Quelltext Datenschutz

```text
Padel-Out
Datenschutzerklärung
Gemäß Art. 13 und 14 DSGVO | Stand: April 2026


1. Verantwortlicher
Padel-Out
c/o Glaswerk Oldenburg
Ariadna Ortega Orriols
Emsstraße 18
info@padel-out.de
Tel.: +49 155 567149871

2. Welche Daten wir erheben
Wir erheben ausschließlich die Daten, die für die Durchführung deiner Reise notwendig sind:

Vorname und Nachname
Geburtsdatum
Nationalität
Ausweis- oder Reisepassnummer sowie Ausstellungsdatum
E-Mail-Adresse und Telefonnummer
Anreise- und Abreisedatum
Gewähltes Paket und Zahlungsinformationen

3. Zweck und Rechtsgrundlage der Verarbeitung
3.1 Vertragsdurchführung
Wir verarbeiten deine Daten zur Durchführung des Reisevertrags (Art. 6 Abs. 1 lit. b DSGVO). Ohne diese Daten können wir die Reise nicht organisieren.
3.2 Gesetzliche Meldepflicht in Spanien
Das spanische Recht (Real Decreto 933/2021) verpflichtet Hotels, die Daten aller Gasteinzelheiten an die spanischen Behörden zu übermitteln. Wir sind daher verpflichtet, folgende Daten vor Anreise ans Hotel weiterzugeben:

Vollständiger Name
Geburtsdatum
Nationalität
Ausweis- oder Reisepassnummer und Ausstellungsdatum
Anreisedatum

Rechtsgrundlage: Art. 6 Abs. 1 lit. c DSGVO (gesetzliche Verpflichtung).
3.3 Berechtigte Interessen
Für die interne Kommunikation, das Buchungsmanagement und die Betreuung vor und nach der Reise verarbeiten wir deine Kontaktdaten auf Basis von Art. 6 Abs. 1 lit. f DSGVO (berechtigtes Interesse).

4. Weitergabe an Dritte
Deine Daten werden nur in folgenden Fällen an Dritte weitergegeben:

Hotel 4-Sterne-Superior, Sant Esteve Sesrovires, Barcelona (Spanien): Übermittlung der Pflichtdaten gemäß spanischem Gästemeldungsrecht (Real Decreto 933/2021).
Versicherungen: Occident (Unfallversicherung) und Allianz (Haftpflichtversicherung) erhalten Daten, die zur Policenverwaltung und im Schadensfall notwendig sind.
R+V Allgemeine Versicherung AG: Insolvenzabsicherung gemäß § 651r BGB.
Steuerberater und Buchhalter: soweit gesetzlich erforderlich und unter Verschwiegenheitspflicht.

Eine Weitergabe an sonstige Dritte oder zu Werbezwecken erfolgt nicht.

5. Datenübertragung in Drittländer
Die Weitergabe ans Hotel in Spanien stellt eine Datenübertragung innerhalb der EU dar und ist durch die DSGVO abgedeckt. Es erfolgt keine Datenübertragung in Länder außerhalb der EU oder des EWR.

6. Speicherdauer
Wir speichern deine Daten nur so lange, wie es für die oben genannten Zwecke notwendig ist oder gesetzliche Aufbewahrungspflichten bestehen:

Buchungs- und Vertragsdaten: 10 Jahre (gemäß handels- und steuerrechtlichen Aufbewahrungspflichten)
Kommunikationsdaten (WhatsApp, E-Mail): 3 Jahre nach Abschluss der Geschäftsbeziehung
Daten, die nur zur Vertragsdurchführung nötig waren: Löschung 6 Monate nach Reiseende

7. Deine Rechte
Du hast gegenüber uns jederzeit folgende Rechte:

Auskunft: Du kannst erfahren, welche Daten wir über dich gespeichert haben (Art. 15 DSGVO).
Berichtigung: Du kannst die Korrektur unrichtiger Daten verlangen (Art. 16 DSGVO).
Löschung: Du kannst die Löschung deiner Daten verlangen, soweit keine gesetzlichen Aufbewahrungspflichten entgegenstehen (Art. 17 DSGVO).
Einschränkung: Du kannst die Einschränkung der Verarbeitung verlangen (Art. 18 DSGVO).
Widerspruch: Du kannst der Verarbeitung auf Basis berechtigter Interessen widersprechen (Art. 21 DSGVO).
Datenübertragbarkeit: Du kannst deine Daten in einem gängigen Format erhalten (Art. 20 DSGVO).

Zur Ausübung dieser Rechte wende dich per E-Mail an: info@padel-out.de

8. Beschwerderecht
Du hast das Recht, dich bei einer Datenschutzbehörde zu beschweren. Die zuständige Aufsichtsbehörde ist:

Der Bundesbeauftragte für den Datenschutz und die Informationsfreiheit (BfDI)
Graurheindorfer Straße 153, 53117 Bonn
www.bfdi.bund.de

9. Sicherheit deiner Daten
Wir treffen angemessene technische und organisatorische Maßnahmen zum Schutz deiner Daten gegen unbefugten Zugriff, Verlust oder Missbrauch. Die Kommunikation per WhatsApp und E-Mail erfolgt verschlüsselt.

10. Änderungen dieser Datenschutzerklärung
Wir behalten uns vor, diese Datenschutzerklärung bei Bedarf anzupassen. Die jeweils aktuelle Fassung ist auf unserer Website veröffentlicht. Maßgeblich ist die zum Zeitpunkt deiner Buchung gültige Fassung.
```

## Notes

Die Kundenvorlage enthaelt bewusst die konkrete Schreibweise `Padel-Out`. Inhaltliche Glattung oder juristische Korrektur ist nicht Teil dieser Wave.

Umsetzung 2026-05-05:
- Bestehende Legal Pages gefunden: `web/customer/src/app/datenschutz/page.tsx` und `web/customer/src/app/agb/page.tsx`.
- Bestehende Links bestaetigt: Footer, Checkout und Cookie Consent verlinken weiter auf `/datenschutz` und `/agb`.
- Rechtstexte in zentrale, testbare Content-Quelle `web/customer/src/lib/legal-content.ts` verschoben.
- Datenschutz und AGB aus der Kundenvorlage uebernommen und mit semantischen Abschnitten, Listen, internen und externen Links gerendert.
- Seiten rendern weiter in der bestehenden Legal-Seitenoptik mit Zurueck-Link, Brand-Zeile, Ueberschrift, Abschnitten und Listen.
- Keine Backend-, DB-, API-, CMS- oder Admin-Funktionen angelegt.
