# Journal

**Das Trading-Journal für Disziplin. Eine Datei, kein Server, deine Daten bleiben bei dir.**

Das Journal beantwortet jeden Tag drei Fragen: Habe ich mich vorbereitet? Habe ich meine Regeln gehalten? Wohin führt mich das auf Sicht von Monaten?

Ein Teil von [Tradeorado](https://tradeorado.de).

## Die drei Seiten

### Ritual
Startseite. Das tägliche Byron-Katie-Ritual vor dem Trading-Tag: Gedanke untersuchen, vier Fragen beantworten, Reframe-Satz mitnehmen. Gedanke und Reframe sind frei wählbar.

### Trades
Jeder Trade mit Datum, Symbol, Gewinn oder Verlust und zwei Checks: Stop Loss vor der Order gesetzt, Stop Loss nicht verschoben. Eine rote Zeile zeigt dir jeden Trade an einem Tag ohne Ritual.

### Prognose
Hochrechnung deines Kontos über eine frei wählbare Anzahl Monate, auf Basis deines echten Tagesdurchschnitts aus dem Journal. Linear, ohne Zinseszins, mit Monatstabelle. Darüber steht die Warnung, die du nicht vergessen sollst: Kein Handel mehr möglich, wenn Verluste dauerhaft ein Vielfaches der Gewinne übersteigen.

### Einstellungen (⚙)
Anzahl Monate, Handelstage pro Woche, Jahresgehalt, Startkapital, Ein- und Auszahlungen sowie Export und Import deiner Daten.

## Benutzen

1. `journal.html` herunterladen.
2. Im Browser öffnen. Fertig.

Es gibt keine Installation, keine Registrierung und keine Verbindung ins Netz.

## Deine Daten

Alles liegt im lokalen Speicher deines Browsers (`localStorage`). Das bedeutet:

- Die Daten bleiben nach dem Schließen erhalten, aber nur in diesem Browser auf diesem Rechner.
- Sichere regelmäßig über **⚙ > Als JSON-Datei exportieren**. Auf einem neuen Rechner oder in einem anderen Browser holst du die Daten mit **JSON-Datei importieren** zurück.
- Browserdaten löschen entfernt auch dein Journal. Ein Export davor rettet es.

Export-Dateien (`*.json`) gehören nicht ins Repository. Die `.gitignore` schließt sie aus.

## Technik

Eine einzelne HTML-Datei mit eingebettetem CSS und JavaScript, ohne Abhängigkeiten und ohne Build.

## Hinweis

Trading birgt Risiken bis zum Totalverlust. Das Journal ist ein Werkzeug zur Selbstkontrolle und keine Anlageberatung.
