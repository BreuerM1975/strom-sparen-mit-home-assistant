# Vorlagen einbinden

Diese Anleitung gehört zum Buch „Strom sparen mit Home Assistant". Sie erklärt, wo jede
Datei hingehört und worauf du achten musst. Vorausgesetzt wird, dass Home Assistant bei
dir läuft und dein Messgerät bereits eingebunden ist.

## Übersicht

| Datei | Wohin | Was du anpassen musst |
|---|---|---|
| `dashboard/energie-live-vorlage.yaml` | YAML einer einzelnen manuellen Karte | zwei Beispiel-Entitäten, beide in Watt |
| `dashboard/energie-dashboard-vorlage.yaml` | YAML einer einzelnen manuellen Karte | Energiequellen vorher einrichten |
| `automationen/nachtlast-hinweis-vorlage.yaml` | YAML-Editor einer neuen einzelnen Automation | Sensor in Watt, Schwelle, Uhrzeit |
| `automationen/nachtabschaltung-package.yaml` | gesondertes Konfigurationspaket | externe Sensor- und Switch-Entitäten, Schwelle, Nachtfenster |

Keine dieser Dateien ersetzt eine bestehende Konfiguration. Jede ist ein Baustein, der zu
dem passen muss, was du schon hast.

## Mit nur einem Messgerät anfangen

Du brauchst keine Messung der ganzen Wohnung. Kapitel 5 des Buches zeigt, wie du
Entitäts-ID, Leistung in Watt und Energiezähler in Kilowattstunden auseinanderhältst. Ein
fortlaufender kWh-Stand ist dabei kein Tagesverbrauch, sondern ein Zählerstand.

Für `energie-live-vorlage.yaml` ohne Gesamtmessung entfernst du in einer Arbeitskopie
beide Verweise auf `sensor.demo_wohnung_leistung`: den Eintrag unter `entities` und den im
`history-graph`. Die beiden Verweise auf `sensor.demo_buero_leistung` ersetzt du durch
deinen einzelnen Leistungssensor in Watt.

`energie-dashboard-vorlage.yaml` kommt später, wenn du die Energie-Konfiguration von Home
Assistant eingerichtet hast.

## Die Hinweis-Automation einbinden

Das ist der einfache Fall. Gehe zu *Einstellungen → Automationen & Szenen → Neue
Automation*, dann im Drei-Punkte-Menü oben rechts auf *In YAML bearbeiten* und ersetze den
Inhalt durch die Datei.

Passe drei Dinge an: den Sensor, die Schwelle von 5 W und die Uhrzeit 23:15. Beides sind
Beispiele aus dem Buch und keine allgemeingültigen Grenzen.

Die Regel prüft einmal am Abend und erzeugt bei passendem Messwert eine Meldung innerhalb
von Home Assistant. Sie sendet nichts nach außen und schaltet nichts. War Home Assistant
zur Prüfzeit aus, wird die Prüfung nicht nachgeholt.

## Das Abschaltpaket einbinden

**Beginne an einem Testaufbau, nicht an einem Gerät, das dir wichtig ist.**

Prüfe zuerst, ob deine `configuration.yaml` schon einen `homeassistant:`- oder
`packages:`-Abschnitt hat. Lege keinen zweiten Hauptschlüssel an. Dieses Beispiel bindet
die Datei gezielt ein:

```yaml
homeassistant:
  packages:
    energieratgeber_nacht: !include nachtabschaltung-package.yaml
```

Die Paketdatei liegt dann direkt neben `configuration.yaml`. Wer schon
`packages: !include_dir_named packages` verwendet, legt sie in diesem vorhandenen Ordner
ab und ergänzt keinen zweiten `packages`-Schlüssel.

Vor dem Neustart die Konfigurationsprüfung von Home Assistant benutzen. Danach
kontrollieren: Der Freigabeschalter startet aus, der Prüfsensor ist ohne Freigabe aus, und
die neue Sensor-ID entspricht der Vorlage. **Bei ID-Konflikten vergibt Home Assistant eine
andere ID.** Dann müssen alle Verweise angepasst werden. Aktiviere nie zwei Exemplare
derselben Vorlage.

Nur `sensor.demo_buero_leistung` und `switch.demo_buero_zubehoer` zeigen auf externe
Geräte. Die Helfer- und Sensor-IDs, die mit `energieratgeber_` beginnen, bleiben wie sie
sind. Ersetze sie nicht aus Bequemlichkeit durch IDs, die du schon produktiv benutzt.

[Offizielle Dokumentation zu Paketen](https://www.home-assistant.io/docs/configuration/packages/)

## Die einmalige Freigabe bedienen

Nach dem Einbinden existiert der Helfer `input_boolean.energieratgeber_nacht_freigabe`. Er
ist zunächst aus. Kapitel 6.6 des Buches enthält eine Entitätenkarte, mit der du ihn
zusammen mit dem Prüfsensor und der Teststeckdose bedienst. Diese Karte ist nicht Teil der
Paketdatei; sie kommt in den manuellen Karteneditor.

**Der Schalter ist kein Abschaltknopf.** Einschalten erlaubt nur den nächsten Ruhetest und
die anschließende einmalige Abschaltung im Nachtfenster. Am Morgen schaltet nichts
automatisch wieder ein; den Stecker bedienst du selbst. Eine neue Freigabe ist jedes Mal
eine neue Entscheidung.

[Home Assistant: Schalter-Helfer](https://www.home-assistant.io/integrations/input_boolean/)

## Test und Rückweg

Nimm eine kleine Testlampe. **Nie die Versorgung von Rechner, NAS, Router, Kühlgerät oder
der Home-Assistant-Zentrale.** Der Leistungssensor muss genau die Last messen, die
geschaltet wird. Stelle Testfenster und Schwelle auf deinen Aufbau ein.

Die Fallliste in Kapitel 6.7 des Buches geht alle Zustände durch: Neustart, nicht
verfügbare Quelle, höhere Leistung, verbrauchte Freigabe, manuelles Wiedereinschalten. Der
Knopf zum manuellen Ausführen von Aktionen beweist dabei nichts über die Auslöser; sieh
dir die tatsächlichen Automationsläufe und den echten Gerätezustand an.

**Rückweg:** Erst die Freigabe aus, dann den Stecker über Gerätetaste oder Home Assistant
wieder ein. Nach einem Abschaltversuch ist die Freigabe auch dann verbraucht, wenn der
Befehl fehlgeschlagen ist. Prüfe in dem Fall den Fehler und den wirklichen Gerätezustand,
statt einen Erfolg anzunehmen. Ein manueller Zugang zum Gerät muss immer funktionieren.

## Was der Ruhetest nicht kann

Der Prüfsensor ist an die Freigabe gebunden. Ohne Freigabe und bei fehlenden, ungültigen
oder negativen Werten wird er zurückgesetzt. Er ist **keine allgemeine Zustandsanzeige**
deines Geräts, sondern nur die Freigabeprüfung dieses einen Rezepts.

Er erkennt nicht:

- einen Messwert, der plausibel aussieht, aber vom Gerät nicht mehr aktualisiert wird,
- eine laufende Aufgabe im Gerät, etwa einen Druckauftrag oder ein Update,
- ob ein Rechner wirklich heruntergefahren ist.

Eine niedrige Wattzahl ist keine Schaltfreigabe. Ob ein Gerät vom Netz getrennt werden
darf, steht in seiner Anleitung, nicht in einem Messwert.

## Die Arbeitsblätter

Die vier CSV-Dateien sind UTF-8 mit BOM und Semikolon als Trennzeichen. Keine Formeln,
keine Makros, keine vorausgefüllten Daten. Beim Import prüfst du Trennzeichen und
Datentypen; Zählerwerte führst du in Kilowattstunden.

Das Tagesprotokoll hat 14 leere Zeilen für zwei Wochen. Jede Zeile ist ein Intervall mit
Anfangs- und Endablesung. Trage Phase, Messpunkt und tatsächliche Nutzung ein und
unterscheide vorhandene Schaltzeiten von einer späteren Änderung.

Druckfassungen aller Blätter stehen im Anhang des Buches. Nicht benötigte Spalten darfst
du in deiner Arbeitskopie ausblenden; die leeren Vorlagen bleiben als Muster erhalten.
