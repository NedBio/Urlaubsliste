# Berechtigungen: Urlaubsplaner

Stand: Entwurf 0.1 (16.11.2026). Grundlage: [docs/LASTENHEFT.md](docs/LASTENHEFT.md), Abschnitte 4, 6 und 8.

Rechte werden serverseitig bei jedem Aufruf geprüft. Datenfilter (eigenes Team, eigene Daten) sitzen in der Datenbankabfrage.

## Rollen

| Rolle | Zweck | Entra-Zuordnung (offen, O-03) | Datenumfang |
|---|---|---|---|
| Mitarbeitende | Eigene Planung, Teamübersicht | App-Rolle `Planer.Nutzer` + Gruppe je Team | Eigene Daten, eigenes Team (eingeschränkt) |
| Führungskraft | Überblick über das eigene Team bzw. die eigene Abteilung | App-Rolle `Planer.Fuehrungskraft` + Gruppe je geführter Einheit | Teams der geführten Einheit |
| Fachadministration | Stammdaten pflegen | App-Rolle `Planer.Admin` | Nur Stammdaten, keine Planungsdaten und keine Konten |

Eine Person kann mehrere Rollen haben (z. B. Führungskraft und Mitarbeitende).

## Aktionen

| Aktion | Mitarbeitende | Führungskraft | Fachadministration |
|---|---|---|---|
| Eigene Abwesenheit anlegen, ändern, löschen | ✔ | ✔ | – |
| Abwesenheit anderer anlegen, ändern, löschen | – | – | – |
| Teamübersicht sehen (eigenes Team) | ✔ | ✔ (alle geführten Teams) | – |
| Art bei „nur abwesend“-Arten sehen (z. B. Reha) | nur eigene | nur eigene (Elternzeit: ja, Vorschlag O-04) | – |
| Status fremder Abwesenheiten sehen | – | ✔ (geführte Teams) | – |
| Eigenes Urlaubskonto sehen und bearbeiten | ✔ | ✔ | – |
| Urlaubskonto anderer sehen | – | ✔ lesend (geführte Teams) | – |
| Urlaubskonto anderer bearbeiten | – | – | – |
| Kalenderdatei (.ics) eigener Abwesenheiten | ✔ | ✔ | – |
| Export Teamübersicht (CSV/Excel), sofern umgesetzt | – | ✔ (eingeschränkt nach Abschnitt 6) | – |
| Organisation, Arten, Sondertage, Sperr-/Infozeiträume, Mindestbesetzung pflegen | – | – (Mindestbesetzung: offen, O-07) | ✔ |
