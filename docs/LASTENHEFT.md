# Lastenheft: Urlaubs- und Abwesenheitsplanung (Arbeitstitel „Urlaubsplaner“)

| | |
|---|---|
| **Version** | 0.1 (Entwurf) |
| **Stand** | 16.11.2026 |
| **Status** | In Abstimmung mit dem Fachbereich, noch nicht mit IT, Datenschutz und Betriebsrat abgestimmt |
| **Geltungsbereich** | Abteilungen 21 und 27 (Pilot), erweiterbar |

---

## 1. Ausgangslage

Abwesenheiten für das kommende Jahr werden heute in einer Excel-Datei geplant. Sie wird jedes Jahr Ende Oktober je Abteilung neu erstellt: Datumsreihe, Berliner Feiertage, betriebliche Sondertage (24.12., 31.12.), Schulferien und Infozeilen (z. B. SAP-Upgrade) werden von Hand gepflegt. Die Kolleg*innen tragen ihre Planung per Kürzel ein (P = Urlaub, G = Gleittag, S = Schulung, EZ = Elternzeit, R = Reha, F = Ferien, X = SAP). Ein Übersichtsblatt führt je Person Rest aus dem Vorjahr, Anspruch, Planung je Monat und Übertrag.

Schwachstellen der heutigen Lösung:

| Nr. | Schwachstelle | Auswirkung |
|---|---|---|
| S1 | Jährliche manuelle Vorbereitung (Datumsreihe, Feiertage, Formatierungen) | Aufwand, Fehlerquelle |
| S2 | Urlaubstage werden per `ZÄHLENWENN` gezählt, auch an Wochenenden und Feiertagen | Falsche Restwerte möglich |
| S3 | „R = Reha“ ist für alle sichtbar | Gesundheitsdatum (Art. 9 DSGVO) offen für alle Dateinutzer*innen |
| S4 | Urlaubskonten aller Personen sind für alle sichtbar | Nicht erforderliche Offenlegung, ggf. Rückschlüsse (Zusatzurlaub, Teilzeit) |
| S5 | Gleichzeitiges Bearbeiten, Versionsstände, versehentliches Überschreiben fremder Zeilen | Datenqualität |
| S6 | Kein Hinweis auf Engpässe (zu viele gleichzeitig abwesend, Sperrzeiten) | Konflikte werden spät erkannt |

## 2. Ziele und Abgrenzung

### 2.1 Ziele

- **Z1:** Kolleg*innen planen ihre Abwesenheiten für das kommende Jahr **unverbindlich** und frühzeitig, bevor sie den offiziellen Antrag im ESS stellen.
- **Z2:** Jede*r sieht die Planung des eigenen Teams und erkennt Überschneidungen früh.
- **Z3:** Kein jährlicher Vorbereitungsaufwand mehr. Feiertage werden berechnet, neue Jahre sind automatisch verfügbar.
- **Z4:** Jede*r führt ein eigenes Urlaubskonto (Anspruch, Rest, geplant, Übertrag) als persönliche Planungshilfe.
- **Z5:** Geplante Abwesenheiten lassen sich auf Wunsch in den eigenen Outlook-Kalender übernehmen.

### 2.2 Nicht-Ziele (bewusst ausgeschlossen)

- **N1:** Kein Antrags- und Genehmigungsverfahren. Maßgeblich bleibt das ESS. Das Tool hat **keine Schnittstelle zum ESS**.
- **N2:** Kein verbindliches Urlaubskonto. Die Werte sind eigene Angaben und können vom ESS abweichen.
- **N3:** Keine Erfassung von Krankheitszeiten oder Abwesenheitsgründen. Kein Freitext zu Gesundheit.
- **N4:** Keine Auswertungen über Einzelpersonen (z. B. „wer plant spät“, „wer nimmt wann Urlaub“) und keine Statistiken über das Planungsverhalten.
- **N5:** Halbe Tage sind in Version 1 nicht enthalten (siehe Abschnitt 14).

## 3. Begriffe

| Begriff | Bedeutung |
|---|---|
| Abwesenheit | Geplanter Zeitraum (von–bis, ganztägig) einer Person mit einer Abwesenheitsart |
| Abwesenheitsart | Z. B. Urlaub, Gleittag, Schulung, Elternzeit, Reha, mit Eigenschaften nach Abschnitt 6 |
| Arbeitstag | Montag bis Freitag, außer gesetzlichen Feiertagen in Berlin und betrieblichen Sondertagen |
| Betrieblicher Sondertag | Von der Gewobag festgelegter arbeitsfreier Tag (z. B. 24.12., 31.12.) |
| Sperrzeit | Zeitraum, in dem Abwesenheit ungünstig ist (z. B. SAP-Upgrade, Jahresabschluss). Erzeugt einen Hinweis, keine Sperre |
| Infozeitraum | Reine Information ohne Hinweis beim Eintragen (z. B. Schulferien Berlin oder Brandenburg) |
| Mindestbesetzung | Mindestanzahl anwesender Personen je Team und Arbeitstag |
| Status | Selbst gesetzter Bearbeitungsstand einer Abwesenheit: *Idee*, *im ESS beantragt*, *genehmigt* |

## 4. Rollen

| Rolle | Beschreibung |
|---|---|
| **Mitarbeitende** | Alle Personen eines Teams |
| **Führungskraft** | Leitung eines Teams oder einer Abteilung. Zuordnung über Entra-Gruppen |
| **Fachadministration** | Pflegt Stammdaten (Organisation, Abwesenheitsarten, Sondertage, Sperr- und Infozeiträume, Mindestbesetzung). Hat **keinen** Einblick in Urlaubskonten |

Die genaue Rechtezuordnung steht in [`BERECHTIGUNGEN.md`](../BERECHTIGUNGEN.md).

## 5. Funktionale Anforderungen

Priorität: **M** = Muss (Version 1), **S** = Soll (Version 1, verschiebbar), **K** = Kann (spätere Version).

### 5.1 Planung

| ID | Anforderung | Prio |
|---|---|---|
| F-01 | Eine Person legt eine Abwesenheit als Zeitraum an (Startdatum, Enddatum, Art). Ganztägig | M |
| F-02 | Eine Person ändert und löscht nur ihre eigenen Abwesenheiten | M |
| F-03 | Für jede Abwesenheit wird der Status *Idee*, *im ESS beantragt* oder *genehmigt* gesetzt. Standard: *Idee* | M |
| F-04 | Planung ist für das laufende und das folgende Kalenderjahr möglich. Ab wann das Folgejahr freigeschaltet ist, klärt O-09 | M |
| F-05 | Überschneidet sich eine neue Abwesenheit mit einer eigenen bestehenden, wird das verhindert und begründet | M |
| F-06 | Bei Anlage oder Änderung zeigt das Tool die Anzahl der betroffenen **Arbeitstage** | M |
| F-07 | Liste „Meine Planung“ mit Filter nach Status, insbesondere „noch nicht im ESS beantragt“ | M |

### 5.2 Kalender und Übersicht

| ID | Anforderung | Prio |
|---|---|---|
| F-10 | Teamübersicht als Kalender: Personen als Zeilen, Tage als Spalten. Wählbar ist Monat, Quartal oder Jahr, mit Blättern über den Jahreswechsel | M |
| F-11 | Wochenenden, gesetzliche Feiertage und betriebliche Sondertage werden gekennzeichnet und zeigen per Tooltip ihren Namen | M |
| F-12 | Abwesenheiten werden farbig mit Kürzel dargestellt. Für Arten mit eingeschränkter Sichtbarkeit (Abschnitt 6) sehen andere nur „abwesend“ | M |
| F-13 | Unter der Teamübersicht steht je Arbeitstag „anwesend x von y“, mit Hervorhebung bei Unterschreitung der Mindestbesetzung | S |
| F-14 | Sperr- und Infozeiträume erscheinen als eigene Zeilen über dem Kalender | S |
| F-15 | Der heutige Tag ist hervorgehoben. Die Ansicht springt beim Öffnen zum aktuellen Monat | M |
| F-16 | Die Darstellung ist farbunabhängig erkennbar (Kürzel im Feld, nicht nur Farbe) | M |
| F-17 | Auf dem Smartphone gibt es statt der breiten Matrix eine Listen- bzw. Wochenansicht | M |

### 5.3 Hinweise (keine Sperren)

| ID | Anforderung | Prio |
|---|---|---|
| F-20 | Fällt eine geplante Abwesenheit ganz oder teilweise in eine Sperrzeit des eigenen Teams oder der Abteilung, erscheint vor dem Speichern ein Hinweis mit Name und Zeitraum. Speichern bleibt möglich | S |
| F-21 | Würde durch die Abwesenheit an einem Arbeitstag die Mindestbesetzung des Teams unterschritten, erscheint ein Hinweis mit den betroffenen Tagen. Speichern bleibt möglich. Der Hinweis nennt **keine Namen** anderer Personen | S |
| F-22 | In die Berechnung von F-13 und F-21 fließen alle Abwesenheiten unabhängig vom Status ein | S |

### 5.4 Urlaubskonto

| ID | Anforderung | Prio |
|---|---|---|
| F-30 | Je Person und Jahr gibt es ein Konto mit **Anspruch**, **Rest aus Vorjahr**, **geplant** (berechnet) und **verfügbar/Übertrag** (berechnet). Berechnung siehe Abschnitt 7 | M |
| F-31 | Anspruch und Rest pflegt die Person selbst und kann sie jederzeit korrigieren. Das Datum der letzten Änderung wird angezeigt | M |
| F-32 | Beim ersten Aufruf eines neuen Jahres schlägt das Tool vor: Anspruch = Vorjahreswert, Rest = Übertrag des Vorjahres. Die Person bestätigt oder korrigiert | M |
| F-33 | Ist „verfügbar“ negativ, wird das deutlich angezeigt. Speichern bleibt möglich | M |
| F-34 | Das Konto zeigt zusätzlich, wie viele geplante Tage davon *im ESS beantragt* bzw. *genehmigt* sind | S |
| F-35 | Am Konto steht dauerhaft der Hinweis: „Eigene Angaben zur Planung. Maßgeblich sind die Werte im ESS.“ | M |
| F-36 | Die Führungskraft sieht die Konten ihrer Teammitglieder lesend (Anspruch, Rest, geplant, verfügbar). Bearbeiten kann sie sie nicht | M |

### 5.5 Outlook

| ID | Anforderung | Prio |
|---|---|---|
| F-40 | **Version 1:** An jeder eigenen Abwesenheit gibt es „In Outlook übernehmen“. Das lädt eine Kalenderdatei (.ics, RFC 5545) herunter, die Outlook öffnet | M |
| F-41 | Der Kalendereintrag ist ganztägig. Standard für „Anzeigen als“: *Mit Vorbehalt*. Die Person kann vor dem Herunterladen *Frei* oder *Abwesend* wählen | M |
| F-42 | Bei Arten mit eingeschränkter Sichtbarkeit lautet der Betreff nur „Abwesend“, und der Termin ist als *privat* markiert | M |
| F-43 | Ein Hinweis erklärt, dass spätere Änderungen in Outlook selbst nachgezogen werden müssen | M |
| F-44 | **Version 2:** Direkter Eintrag über Microsoft Graph in den eigenen Kalender, nur auf ausdrücklichen Klick der Person. Ändert oder löscht sie die Abwesenheit im Tool, wird angeboten, den Termin mitzuändern bzw. mitzulöschen | K |
| F-45 | Version 2 nutzt ausschließlich delegierte Rechte (nur der eigene Kalender der angemeldeten Person). Voraussetzung ist die Freigabe der IT | K |

### 5.6 Fachadministration

| ID | Anforderung | Prio |
|---|---|---|
| F-50 | Organisation pflegen: Bereich, Abteilung, Team (Kürzel, Name, übergeordnete Einheit, Mindestbesetzung) | M |
| F-51 | Personen einem Team zuordnen. Grundlage siehe O-03 | M |
| F-52 | Abwesenheitsarten pflegen (Abschnitt 6). Arten mit vorhandenen Einträgen können nur deaktiviert, nicht gelöscht werden | M |
| F-53 | Betriebliche Sondertage pflegen (Datum, Bezeichnung), auch für künftige Jahre | M |
| F-54 | Sperr- und Infozeiträume pflegen (Bezeichnung, von–bis, Typ, Geltungsbereich: Team, Abteilung oder alle) | S |
| F-55 | Schulferien Berlin und Brandenburg als Infozeiträume per CSV-Import einspielen. Kein Live-Abruf externer Dienste | S |
| F-56 | Gesetzliche Feiertage Berlin werden automatisch berechnet und sind nicht pflegbar. Ein gesetzlicher Einmal-Feiertag (wie der 8. Mai 2025) kann als Sondertag ergänzt werden | M |

## 6. Abwesenheitsarten (Startkonfiguration)

| Kürzel | Bezeichnung | Zählt als Urlaubstag | Sichtbar für Teammitglieder | Sichtbar für Führungskraft |
|---|---|---|---|---|
| U | Urlaub | ja | mit Art | mit Art |
| G | Gleittag | nein | mit Art | mit Art |
| S | Schulung/Fortbildung | nein | mit Art | mit Art |
| D | Dienstreise | nein | mit Art | mit Art |
| SU | Sonderurlaub | nein | mit Art | mit Art |
| EZ | Elternzeit | nein | nur „abwesend“ | mit Art |
| R | Reha | nein | nur „abwesend“ | nur „abwesend“ |
| A | Sonstige Abwesenheit | nein | nur „abwesend“ | nur „abwesend“ |

Hinweise:
- Kürzel und Farben sind konfigurierbar. Das heutige „P = Plan Urlaub“ wird zu „U“, weil jeder Eintrag im Tool ohnehin Planung ist. Falls das Kürzel „P“ bleiben soll, ist das reine Konfiguration.
- „F = Ferien“ und „X = SAP“ aus der Excel sind keine Abwesenheiten. Sie werden zu Infozeiträumen (F-55) bzw. Sperrzeiten (F-54).
- Die Sichtbarkeit für Elternzeit und Reha bei Führungskräften ist ein Vorschlag (O-04).

## 7. Berechnungsregeln

1. **Arbeitstage einer Abwesenheit** = alle Tage im Zeitraum von Montag bis Freitag, abzüglich gesetzlicher Feiertage in Berlin und betrieblicher Sondertage.
2. **Gesetzliche Feiertage Berlin:** Neujahr, Internationaler Frauentag (8.3.), Karfreitag, Ostermontag, Tag der Arbeit, Christi Himmelfahrt, Pfingstmontag, Tag der Deutschen Einheit, 1. und 2. Weihnachtstag. Die Berechnung nutzt eine etablierte Bibliothek, keine selbst gebaute Osterformel. Gesetzliche Grundlage ist das Berliner Feiertagsgesetz. Bei Änderungen wird die Bibliothek aktualisiert oder ein Sondertag ergänzt.
3. **Geplant (Konto)** = Summe der Arbeitstage aller Abwesenheiten mit „zählt als Urlaubstag = ja“ im Kalenderjahr, unabhängig vom Status. Eine Abwesenheit über den Jahreswechsel wird tageweise dem jeweiligen Jahr zugeordnet.
4. **Verfügbar/Übertrag** = Anspruch + Rest aus Vorjahr − geplant.
5. **Anwesend an Tag t (Team)** = Anzahl Teammitglieder − Anzahl Teammitglieder mit einer Abwesenheit an t (jede Art, jeder Status). Wird nur für Arbeitstage berechnet.
6. Verfallsregeln für Resturlaub (z. B. aus Tarifvertrag oder Betriebsvereinbarung) bildet Version 1 **nicht** ab. Ein konfigurierbarer Infotext ist möglich (O-08).

## 8. Sichtbarkeit (Übersicht)

| Was | Person selbst | Teammitglieder | Führungskraft des Teams | Fachadministration |
|---|---|---|---|---|
| Eigene Abwesenheiten (Art, Zeitraum) | ja | ja, eingeschränkt nach Abschnitt 6 | ja, eingeschränkt nach Abschnitt 6 | nein |
| Status (Idee/ESS/genehmigt) | ja | nein | ja | nein |
| Urlaubskonto | ja (bearbeiten) | nein | ja (lesen) | nein |
| Abwesenheiten anderer Teams | – | nein | nur eigene Teams (Abteilungsleitung: alle Teams der Abteilung) | nein |
| Anwesenheitszahl je Tag | ja | ja | ja | nein |

Teamübergreifende Sicht (z. B. für Vertretungen zwischen 21.10 und 21.20) ist in Version 1 nicht vorgesehen (O-05).

## 9. Datenmodell (fachlich)

```
Organisationseinheit (id, kürzel, name, typ [Bereich|Abteilung|Team], übergeordnet_id, mindestbesetzung)
Person               (id, entra_object_id, anzeigename, team_id, aktiv, angelegt_am)
Abwesenheitsart      (id, kürzel, bezeichnung, farbe, zählt_als_urlaub, sichtbarkeit_team, sichtbarkeit_fk, aktiv)
Abwesenheit          (id, person_id, art_id, von, bis, status, outlook_event_id?, angelegt_am, geändert_am)
Urlaubskonto         (person_id, jahr, anspruch, rest_vorjahr, geändert_am)
Sondertag            (id, datum, bezeichnung)
Zeitraum             (id, bezeichnung, typ [Sperrzeit|Info], von, bis, geltung_org_id?)
```

- Abgeleitete Werte (geplant, verfügbar, anwesend) werden **berechnet**, nicht gespeichert.
- Es gibt keine Freitextfelder an Abwesenheiten (N3).
- `outlook_event_id` wird erst in Version 2 benötigt.

## 10. Nicht-funktionale Anforderungen

| ID | Anforderung |
|---|---|
| NF-01 | **Anmeldung** ausschließlich über Microsoft Entra ID (Single Sign-on, OpenID Connect, Code Flow mit PKCE). Keine eigene Benutzerverwaltung |
| NF-02 | **Rollen** über Entra-App-Rollen bzw. Entra-Gruppen. Rechte werden serverseitig bei jedem Aufruf geprüft. Datenfilter (eigenes Team) sitzen in der Datenbankabfrage, nicht in der Oberfläche |
| NF-03 | **Einbindung in Microsoft Teams** als Tab im bestehenden Team, zusätzlich direkt im Browser nutzbar |
| NF-04 | **Responsiv** (Smartphone, Tablet, Desktop), installierbar als **Progressive Web App**. Der Service Worker speichert nur die App-Hülle, keine Planungsdaten |
| NF-05 | **Barrierefreiheit** nach WCAG 2.1 AA: Tastaturbedienung der Kalendermatrix, sichtbarer Fokus, Kontraste, Kürzel zusätzlich zu Farben, Hell- und Dunkelmodus |
| NF-06 | **Sicherheit** nach OWASP ASVS Level 2 (Anwendung mit Personendaten). Sicherheits-Header (CSP, HSTS u. a.), CSRF-Schutz, nur HTTPS |
| NF-07 | **Betriebslogs** ohne Klardaten zu Personen (nur technische IDs), begrenzt und rotiert |
| NF-08 | **Keine externen Dienste** zur Laufzeit (keine CDNs, keine externen Schriften, kein Tracking). Ausnahme in Version 2: Microsoft Graph |
| NF-09 | **Performance:** Teamübersicht für ein Jahr (bis 40 Personen) lädt in unter 2 Sekunden |
| NF-10 | **Verfügbarkeit:** Bürozeiten, kein 24/7-Betrieb nötig. Ein Ausfall von bis zu einem Arbeitstag ist vertretbar, weil die Planung unverbindlich ist |
| NF-11 | **Backup** täglich, Wiederherstellung getestet und dokumentiert |
| NF-12 | **Quellcode** in Azure DevOps, Betrieb als Container (`Dockerfile`, `compose.yaml`) |
| NF-13 | **Sprache** der Oberfläche: Deutsch, geschlechtergerecht |

## 11. Datenschutz und Mitbestimmung

> Unverbindliche Einschätzung des Fachbereichs. Abstimmung mit Datenschutz, Betriebsrat und bei Bedarf der Rechtsabteilung erforderlich.

| Thema | Vorgehen |
|---|---|
| Personenbezogene Daten | Name, Teamzuordnung, Abwesenheitszeiträume und -arten, Urlaubskonto (eigene Angaben) |
| Besondere Kategorien (Art. 9 DSGVO) | „Reha“ lässt auf Gesundheit schließen, ein erhöhter Urlaubsanspruch ggf. auf Schwerbehinderung (Zusatzurlaub, § 208 SGB IX). Gegenmaßnahmen: eingeschränkte Sichtbarkeit (Abschnitt 6), Konto nur für Person und Führungskraft, keine Freitexte |
| Datensparsamkeit | Kein Grund, keine Notiz, nur Art und Zeitraum. Die Fachadministration sieht keine Planungsdaten |
| Löschkonzept (Vorschlag) | Abwesenheiten und Konten eines Jahres werden am **31.12. des Folgejahres** endgültig gelöscht (nötig für den Übertrag). Personen, die aus der Entra-Gruppe entfernt wurden, werden nach **30 Tagen** samt ihrer Daten gelöscht. Kein reines „Soft Delete“ |
| Verzeichnis von Verarbeitungstätigkeiten | Eintrag durch den Fachbereich mit dem Datenschutz |
| Mitbestimmung | Der Urlaubsplan berührt § 87 Abs. 1 Nr. 5 BetrVG. Als technische Einrichtung, die sich zur Verhaltens- und Leistungskontrolle eignen kann, auch § 87 Abs. 1 Nr. 6 BetrVG. Den Betriebsrat vor dem Pilot einbinden und eine bestehende Betriebsvereinbarung zum Urlaub prüfen (O-01) |
| Keine Leistungs- und Verhaltenskontrolle | Keine Zugriffsprotokolle mit Personenbezug, keine Auswertungen über Einzelne (N4). Gespeichert werden nur „angelegt am“ und „geändert am“ am Datensatz |

## 12. Architektur (Vorschlag, Abstimmung mit der IT)

```
Teams-Tab / Browser
        │  HTTPS, Sitzungs-Cookie
        ▼
Web-App (Frontend: TypeScript, React, Vite)
        │  REST/JSON (OpenAPI)
        ▼
Backend (Python, FastAPI)  ── Anmeldung als Backend-for-Frontend (Entra ID, MSAL)
        │                   ── V2: Microsoft Graph (delegiert, nur eigener Kalender)
        ▼
Datenbank (PostgreSQL; alternativ SQLite auf Docker-Volume)
```

Begründung:
- Ein **eigenes Backend** ist nötig wegen Datenbank, serverseitiger Rechteprüfung, Löschfristen (zeitgesteuert) und Microsoft Graph in Version 2.
- **Backend-for-Frontend:** Tokens bleiben auf dem Server, der Browser hat nur ein Sitzungs-Cookie.
- **Datenbank:** Bei etwa 70 Nutzenden und wenigen gleichzeitigen Schreibzugriffen reicht technisch SQLite. PostgreSQL ist vorzuziehen, wenn die IT eine verwaltete Instanz anbietet. Der Zugriff läuft über ein ORM mit Migrationen, die Datenbank bleibt austauschbar.
- **Feiertagsberechnung** über eine etablierte Bibliothek (z. B. das Python-Paket `holidays`, Region Berlin). Version und Sicherheitsstatus werden beim Projektstart geprüft.
- **Optional, spätere Version:** Ein lesender MCP-Zugang (z. B. „Wer aus meinem Team ist nächste Woche abwesend?“) mit denselben Rechten wie die Oberfläche. Nur nach Freigabe durch die IT.

Technisches Risiko: Anmeldung im Teams-Tab. Teams lädt Tabs eingebettet, und Sitzungs-Cookies in eingebetteten Seiten sind je nach Teams-Client eingeschränkt. Lösungsweg (Teams-SSO bzw. Anmeldung im Pop-up) wird im Prototyp früh geklärt (R-02).

## 13. Abnahmekriterien (Auszug)

| Nr. | Gegeben | Wenn | Dann |
|---|---|---|---|
| A-01 | Anspruch 30, Rest 0 | Person plant Urlaub vom Mo 03.05.2027 bis Fr 07.05.2027 (Himmelfahrt am 06.05.) | Geplant = 4, verfügbar = 26 |
| A-02 | Sondertage 24.12. und 31.12.2027 sind gepflegt | Person plant Urlaub vom 20.12. bis 31.12.2027 | Gezählt werden 8 Arbeitstage (Mo–Do 20.–23.12. und 27.–30.12.; 25./26.12. Wochenende, Fr 24.12. und Fr 31.12. Sondertag) |
| A-03 | Eine Teamkollegin hat Reha eingetragen | Ein anderes Teammitglied öffnet die Teamübersicht | Es erscheint nur „abwesend“, weder Art noch Status |
| A-04 | Team mit 5 Personen, Mindestbesetzung 3, zwei Personen am 12.07. abwesend | Eine dritte Person plant den 12.07. | Hinweis „Mindestbesetzung am 12.07. unterschritten (2 von 5 anwesend)“, ohne Namen. Speichern möglich |
| A-05 | Person A ist in Team 21.10 | A ruft die Daten von Team 27.20 über die API ab | Antwort 403, keine Daten |
| A-06 | Abwesenheit der Art Elternzeit | Person lädt die Kalenderdatei herunter | Betreff „Abwesend“, privat, ganztägig, „Mit Vorbehalt“ |
| A-07 | Führungskraft von 21.10 | Sie öffnet ein Konto eines Teammitglieds | Werte sichtbar, Eingabefelder gesperrt; Änderung per API ergibt 403 |

## 14. Releaseplanung (Vorschlag)

| Phase | Inhalt | Ziel |
|---|---|---|
| 0 – Klärung | Freigaben (IT-Betrieb, Entra-App-Registrierung, Datenschutz, Betriebsrat), Azure-DevOps-Repository | Grundlage für den Pilot |
| 1 – Prototyp | Klickbarer Prototyp mit fiktiven Daten, ohne echte Anmeldung. Zur Abstimmung mit Fachbereich, IT und Betriebsrat | Anforderungen bestätigt |
| 2 – Version 1 (MVP) | Alle M- und S-Anforderungen, Outlook per .ics | Pilot mit einem Team |
| 3 – Rollout | Abteilungen 21 und 27 | Einsatz zur Planungsrunde 2028 (Oktober 2027) |
| 4 – Version 2 | Outlook über Microsoft Graph, ggf. halbe Tage, teamübergreifende Sicht, MCP-Zugang | nach Bedarf |

Für 2027 läuft die Planung noch über die bestehende Excel. Ein Import der 2027-Planung in das Tool ist optional (K) und nur sinnvoll bei einem Start vor Mitte 2027.

## 15. Offene Punkte

| Nr. | Frage | Verantwortlich |
|---|---|---|
| O-01 | Gibt es eine Betriebsvereinbarung zu Urlaub bzw. Urlaubsplanung? Wann wird der Betriebsrat eingebunden? | Fachbereich |
| O-02 | Wer betreibt die Anwendung (Server, Updates, Backups)? Gibt es eine verwaltete PostgreSQL-Instanz? | IT |
| O-03 | Woher kommt die Teamzuordnung: eine Entra-Gruppe je Team, das Entra-Attribut „Abteilung“ oder Pflege im Tool? | IT/Fachbereich |
| O-04 | Sieht die Führungskraft bei Elternzeit die Art, bei Reha nur „abwesend“? (Vorschlag in Abschnitt 6) | Datenschutz/Betriebsrat |
| O-05 | Braucht es eine teamübergreifende Sicht für Vertretungen? | Fachbereich |
| O-06 | Wo sind Bereichsleitung und Assistenz („2 DuO“) organisatorisch zugeordnet, und was sehen sie? | Fachbereich |
| O-07 | Mindestbesetzung je Team: Werte und wer sie pflegt (Fachadministration oder Führungskraft) | Fachbereich |
| O-08 | Gibt es Verfallsregeln für Resturlaub, die als Hinweis erscheinen sollen? | Personal |
| O-09 | Ab wann wird das Folgejahr freigeschaltet (immer, oder ab einem Stichtag)? | Fachbereich |
| O-10 | Abwesenheitsarten final: Liste aus Abschnitt 6 bestätigen | Fachbereich |
| O-11 | Löschfristen aus Abschnitt 11 bestätigen | Datenschutz |

## 16. Risiken

| Nr. | Risiko | Gegenmaßnahme |
|---|---|---|
| R-01 | Kein Betreiber in der IT, das Tool wird zur Schatten-IT | Betrieb vor Entwicklungsbeginn klären (O-02), Ablage in Azure DevOps |
| R-02 | Anmeldung im Teams-Tab funktioniert nicht in allen Teams-Clients | Im Prototyp früh testen. Rückfalloption: Teams-Tab als Link auf die Browser-Version |
| R-03 | Konto-Werte im Tool und im ESS laufen auseinander, es entstehen Missverständnisse | Dauerhafter Hinweis (F-35), keine Bezeichnung als „Urlaubskonto“ in offiziellen Kontexten |
| R-04 | Der Betriebsrat lehnt ab oder verzögert | Früh einbinden, Datensparsamkeit und Nicht-Ziele (N1–N4) betonen |
| R-05 | Geringe Akzeptanz, Kolleg*innen planen weiter „nebenher“ | Pilot mit einem Team, Excel nach dem Rollout nicht weiter anbieten |
