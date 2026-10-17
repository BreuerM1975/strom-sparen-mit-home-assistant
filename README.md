# Strom sparen mit Home Assistant: Begleitdateien

Die Vorlagen zum Buch **„Strom sparen mit Home Assistant"** von Marco Breuer.
Arbeitsblätter, Dashboard-Karten und zwei Automationen, alle zum Abschreiben oder
Herunterladen. Das Buch erklärt, was damit zu tun ist; hier liegen nur die Dateien.

Du brauchst das Buch nicht, um die Vorlagen zu benutzen. Ohne das Buch fehlt dir
allerdings die Begründung, warum sie so aussehen, wie sie aussehen.

## Alles auf einmal herunterladen

Oben rechts auf **Code** und dann **Download ZIP**. Oder, wenn du Git benutzt:

```
git clone https://github.com/BreuerM1975/strom-sparen-mit-home-assistant.git
```

## Was hier liegt

### `arbeitsblaetter/` — vier leere Tabellen

Öffne sie mit LibreOffice Calc, Excel oder einer Tabellen-App. UTF-8 mit BOM,
Semikolon als Trennzeichen, keine Formeln und keine Makros. Leere Felder bleiben
leer; ein leeres Feld ist nicht dasselbe wie eine Null.

| Datei | Wofür | Kapitel |
|---|---|---|
| `massnahmenprotokoll-vorlage.csv` | **Das Hauptblatt.** Läuft von deiner Stromrechnung bis zur Entscheidung durch | 1 bis 4 |
| `tagesprotokoll-vorlage.csv` | sieben Tage vorher, sieben Tage nachher, mit Zählerständen | 4 |
| `geraeteinventar-vorlage.csv` | welches Gerät an welchem Messpunkt hängt und ob es aus darf | 2 und 3 |
| `monatsprotokoll-vorlage.csv` | die monatliche Kontrolle | 7 |

Wenn du nur eine Datei anfassen willst, nimm `massnahmenprotokoll-vorlage.csv`.
Die anderen drei sind Ergänzungen.

### `dashboard/` — zwei Karten für Lovelace

Beide sind **einzelne Karten**, keine vollständigen Dashboards. Du fügst sie über
*Karte hinzufügen → Manuell* ein und ersetzt den vorhandenen Inhalt.

- `energie-live-vorlage.yaml` zeigt die aktuelle Leistung in Watt und den Verlauf der
  letzten 24 Stunden. Funktioniert mit einem einzigen Messpunkt.
- `energie-dashboard-vorlage.yaml` setzt voraus, dass du die Energie-Konfiguration von
  Home Assistant bereits eingerichtet hast.

### `automationen/` — zwei YAML-Dateien, die sehr unterschiedlich sind

`nachtlast-hinweis-vorlage.yaml` ist harmlos. Sie prüft einmal am Abend einen
Leistungswert und schreibt eine Meldung. **Sie schaltet nichts.** Einfügen über
*Einstellungen → Automationen & Szenen → Neue Automation → Drei-Punkte-Menü →
In YAML bearbeiten*.

`nachtabschaltung-package.yaml` ist das Gegenteil: ein Konfigurationspaket mit Helfer,
Ruhetest und Abschaltlogik. Es schaltet ein Gerät aus, und zwar nur, wenn du es am Abend
ausdrücklich freigegeben hast. Diese Datei gehört **nicht** in den Editor einer einzelnen
Automation. Wie sie eingebunden wird, steht in `anleitung.md`.

> **Lies die Warnungen in `anleitung.md`, bevor du das Paket einsetzt.** Beginne mit einer
> Testlampe. Nicht mit Rechner, NAS, Router, Kühlgerät oder der Home-Assistant-Zentrale.

## Die Beispiel-Entitäten musst du ersetzen

In allen YAML-Dateien stehen Platzhalter, die es bei dir nicht gibt:

| Platzhalter | Ersetze ihn durch |
|---|---|
| `sensor.demo_buero_leistung` | deinen Leistungssensor, **in Watt** |
| `sensor.demo_wohnung_leistung` | deinen Gesamtleistungssensor, falls vorhanden |
| `switch.demo_buero_zubehoer` | die Steckdose, die geschaltet werden soll |

Die Entitäten, die mit `energieratgeber_` beginnen, erzeugt das Paket selbst. Die
änderst du nicht.

## Fehler gefunden?

Mach ein [Issue](https://github.com/BreuerM1975/strom-sparen-mit-home-assistant/issues)
auf. Das gilt auch für Fehler im Buch.

## Geprüft, und was nicht geprüft ist

Beide Automationen sind mit Home Assistant Core 2026.10.2 geprüft: Konfiguration,
Zeitauslöser, Wartezeit, ungültige Werte, erneutes manuelles Einschalten, Neustart.
**Sensor und Steckdose waren dabei simuliert, nicht real.**

Eine simulierte Steckdose antwortet immer, sofort und richtig. Ein echtes Funkgerät tut
das nicht: Es kann nicht erreichbar sein, verzögert antworten oder einen Befehl
bestätigen, ohne ihn auszuführen. Deshalb beginnst du mit einer Testlampe und prüfst nach
jedem Abschaltversuch den tatsächlichen Zustand deiner Last, nicht nur das Protokoll.

Die Dashboard-Karten sind auf ihre Struktur geprüft. Je nach Home-Assistant-Version kann
deine Oberfläche anders aussehen.

## Lizenz

[MIT](LICENSE). Du darfst die Dateien benutzen, ändern und weitergeben, auch
geschäftlich. Der Text des Buches ist davon nicht erfasst.

## Zum Buch

„Strom sparen mit Home Assistant: Verbrauch messen, Stromfresser finden und sinnvoll
automatisieren", Marco Breuer, Ausgabe 2026, Reihe smarthomeblox.

Unabhängiges Praxisbuch. Keine offizielle Veröffentlichung von Nabu Casa oder der Open
Home Foundation. „Home Assistant" und genannte Produktnamen sind Marken ihrer Inhaber.
