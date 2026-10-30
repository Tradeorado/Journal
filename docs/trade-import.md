# Schnittstelle: Trades-Import (`journal_YYYY-MM-DD.json`)

Beschreibt die Datei, die ein externes Werkzeug (z. B. ein lokaler Broker-Bericht) am Tagesende erzeugt und die das Journal **zusätzlich** zum bestehenden Voll-Import einlesen soll. Alle Werte in den Beispielen sind erfunden.

## Ziel

Zweiter Button neben dem Voll-Import: **„Trades hinzufügen"**. Er fügt nur Trades hinzu. Rituale, Ein-/Auszahlungen und Einstellungen bleiben unberührt. Der bestehende Import (ersetzt den gesamten Zustand) bleibt unverändert.

## Dateiformat

```json
{
  "format": "tradeorado-journal-trades",
  "version": 1,
  "created_at": "2099-01-15T18:30:00.000Z",
  "trades": [
    {
      "id": "sc-DE0000000001-20990115T080501Z",
      "date": "2099-01-15",
      "symbol": "DAX",
      "amount": -123.45,
      "sl_gesetzt": true,
      "sl_stand": false,
      "notiz": "Automatisch erzeugt: 2 Käufe, 1 Verkauf",
      "created_at": "2099-01-15T18:30:00.000Z"
    }
  ]
}
```

### Optionales Zusatzfeld `tages_pl` (Dateiebene)

Der Export kann zur Kontrolle `"tages_pl": {"overview": 100.0, "summe_trades": 99.5, "differenz": 0.5}` mitliefern (Tages-P/L des Brokers gegen die Summe der Trades, erfundene Werte). Der Import darf es in der Vorschau anzeigen, muss es aber nicht auswerten und speichert es nicht.

### Felder je Trade

Die Felder entsprechen dem Trade-Objekt im Journal (`state.trades[]`).

| Feld | Typ | Bedeutung |
|---|---|---|
| `id` | String, nicht leer | Eindeutig und **stabil**: derselbe Trade bekommt bei jedem Export dieselbe `id`. Kein UUID-Zwang. Grundlage für die Dublettenerkennung. |
| `date` | String `YYYY-MM-DD` | Tag, an dem die Position vollständig geschlossen wurde (Ortszeit Europe/Berlin). |
| `symbol` | `"DAX"` oder `"NAS100"` | Wie im Journal. Unbekannte Werte: Trade überspringen und in der Vorschau melden. |
| `amount` | Zahl (EUR) | Gewinn (+) oder Verlust (−) des gesamten Trades: Summe aller Verkäufe minus Summe aller Käufe dieser Position. |
| `sl_gesetzt` | `true` / `false` / `null` | Stop Loss vor der Order gesetzt. `null` = unbekannt (Journal zeigt „–"). |
| `sl_stand` | `true` / `false` | Stop Loss nicht verschoben. Fehlt das Feld oder ist `sl_gesetzt` nicht `true`: als `false` behandeln. |
| `notiz` | String | Freitext, darf leer sein. |
| `created_at` | ISO-8601-String | Zeitpunkt der Erzeugung. |

Ein Trade ist eine Position: vom ersten Kauf bis zum vollständigen Verkauf, inklusive Nachkäufen und Teilverkäufen.

## Verhalten des Imports

1. Datei wählen, JSON parsen. Prüfen: `format === "tradeorado-journal-trades"`, `version === 1`, `trades` ist ein Array. Sonst verständliche Fehlermeldung, nichts ändern.
2. Jeden Trade prüfen (`id`, `date`, `symbol`, `amount` gültig). Ungültige Trades überspringen und in der Vorschau zählen.
3. Dubletten: Trade gilt als vorhanden, wenn `id` schon in `state.trades` steht. Vorhandene Trades werden **nicht** überschrieben.
4. Vorschau vor dem Speichern: „N neue Trades, M schon vorhanden, K ungültig" mit Liste der neuen Trades (Datum, Symbol, Betrag).
5. Nach Bestätigung nur die neuen Trades an `state.trades` anhängen und speichern. Abbrechen ändert nichts.
6. Unbekannte Zusatzfelder ignorieren (Vorwärtskompatibilität).

## Abnahmekriterien

- Zweimal dieselbe Datei importieren: beim zweiten Mal „0 neue Trades".
- Datei mit ungültigem `format` oder ohne `trades`: Fehlermeldung, Zustand unverändert.
- Rituale, Transfers und Einstellungen sind nach dem Import byte-gleich.
- Trade mit `sl_gesetzt: null` wird als „–" angezeigt.
