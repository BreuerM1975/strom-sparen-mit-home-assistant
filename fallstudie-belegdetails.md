# Belege zur Wohnungsfallstudie

Dieses Dokument gehört zu Kapitel 8 des Buches. Dort steht die Geschichte, hier stehen die
Datenwege, die Messgrenzen und die Stellen, an denen die Daten nicht das hergeben, was man
gern hätte.

Du brauchst es nicht, um mit dem Buch zu arbeiten. Es ist für Leser gedacht, die
nachvollziehen wollen, wie die Zahlen in Kapitel 8 zustande gekommen sind, und für alle,
die ähnliche Auswertungen an eigenen Daten machen.

**Keine der folgenden Zahlen ist ein isolierter Nachweis einer Ersparnis durch
Automationen.** Wo das im Buch anders klingen könnte, gilt dieser Satz.

## 1 Datenwege und Messgrenzen

Am Schreibtisch messen ein Leistungssensor in Watt und ein Energiezähler in
Kilowattstunden. Beide gehören zu einem Shelly PM Mini Gen3. Geschaltet wird die
Versorgung dagegen von einer separaten Funksteckdose, nicht vom Shelly.

| Aufgabe | Gerät |
|---|---|
| Leistung und Energie erfassen | Shelly PM Mini Gen3 |
| Versorgung nach Zeitplan schalten | separate Funksteckdose |

Das klingt nach einer Nebensächlichkeit und ist der Kern der Messgrenze. Der Shelly misst
an der Steckdose, in der die Funksteckdose steckt. Die Funksteckdose liegt damit **vor**
der geschalteten Gerätegruppe und **innerhalb** des gemessenen Bereichs. Ihr eigener
Verbrauch wird deshalb nicht zusätzlich zum Gruppenwert addiert.

Dass beide Geräte zum Schreibtisch gehören, macht aus ihnen keinen einzigen Messstecker.
Wer zwei Geräte an einem Platz sieht und sie gedanklich zu einem zusammenzieht, rechnet
später falsch.

Die nächtliche Meldung von 0 W ist **kein Beleg für exakt null Verbrauch**. Sie kann auch
bedeuten, dass der verbleibende Verbrauch unter der Auflösung des Messgeräts liegt. Wie
gut dieser Shelly sehr kleine Lasten erfasst, ist eine eigene Messfrage, die hier nicht
beantwortet wird. Ein geschätzter Ersatzwert wird an dieser Stelle ausdrücklich nicht
eingesetzt.

Im Schaltverlauf gab es an einem untersuchten Tag zwei kurze Phasen mit nicht verfügbarem
Zustand. Das sind keine gespeicherten Ausschaltzustände und kein Beweis für eine
tatsächliche Stromunterbrechung. **Die Meldung eines Geräts und seine physische Wirkung
sind zwei getrennte Fragen.**

## 2 Kurzfristige Zustandshistorie

Am 17.10.2026 wurden vorhandene Daten ausgelesen. Es wurde nichts geschaltet und keine
Regel verändert.

Die Anfrage nach sieben Tagen kurzfristiger Zustandshistorie lieferte erst ab dem 12.10.
Daten. Daraus wird deshalb keine vollständige Messwoche gemacht. Über ältere Zeiträume
sagt das nichts aus; die Langzeitstatistik in Abschnitt 3 reicht deutlich weiter zurück.

Der Energiezähler stieg vom 12.10. gegen 23:00 Uhr bis zum 16.10. gegen 23:00 Uhr um rund
2,73 kWh. Die Grenzwerte stammen aus tatsächlich gespeicherten Ablesungen, jeweils weniger
als fünf Sekunden vom genannten Zeitpunkt entfernt.

| Abschnitt, jeweils etwa 23:00 bis 23:00 Uhr | Zählerzuwachs |
|---|---:|
| 12. bis 13. Oktober 2026 | 0,668 kWh |
| 13. bis 14. Oktober 2026 | 0,624 kWh |
| 14. bis 15. Oktober 2026 | 0,847 kWh |
| 15. bis 16. Oktober 2026 | 0,590 kWh |

Zwei kurze Ereignisse „nicht verfügbar" am 15.10. bleiben Teil des Befunds. Zwischen den
vorhandenen Zählerständen gab es keinen Rücksprung. Das beweist nicht unabhängig, dass
während jeder Unterbrechung vollständig gezählt wurde. Fehlende Zustände werden weder auf
null gesetzt noch automatisch als Energieverlust behandelt.

Der Tagesverlauf des 16.10. ist in Kapitel 8 abgebildet. Drei Übergänge passen dort sauber
zusammen: Der gespeicherte Schaltzustand wechselt um 03:58:00 Uhr auf an, der
Leistungssensor meldet ab 03:58:01 Uhr positive Werte. Um 23:00:00 Uhr wechselt der
Schaltzustand auf aus, um 23:00:02 Uhr folgt die Nullmeldung.

Diese Werte zeigen den Verbrauch **unter der bereits aktiven Regel**. Sie zeigen keine
eingesparte Energie gegenüber einem anderen Betrieb. Eine Jahreshochrechnung aus vier
Abschnitten wäre zu weitgehend.

## 3 Langzeitstatistik und die Grenzen des Vergleichs

Ein lokales Backup bis August 2026 enthält Statistiken ab Ende 2024. Für 19 Monate wurden
die Grenzen der kumulierten Energiestatistik geprüft; bei 17 Monaten waren beide
benötigten Grenzwerte vorhanden.

Für Januar und Februar 2026 fehlte eine gemeinsame exakte Monatsgrenze. Diese Werte
bleiben offen. **Ein ungefähr passender Stand wird nicht ohne Kennzeichnung zu einem
exakten Monatswert.**

| Monat | 2025 | 2026 | Unterschied |
|---|---:|---:|---:|
| März | 18,84 kWh | 23,35 kWh | rund 23,9 % mehr |
| April | 20,10 kWh | 19,81 kWh | rund 1,4 % weniger |
| Mai | 24,16 kWh | 20,47 kWh | rund 15,3 % weniger |
| Juni | 23,37 kWh | 19,18 kWh | rund 17,9 % weniger |
| Juli | 23,32 kWh | 16,91 kWh | rund 27,5 % weniger |

Die Tabelle verwendet vollständige Kalendermonate. Ältere Berichte mit einem verschobenen
30-Tage-Fenster oder einem hochgerechneten Teilmonat beschreiben andere Zeiträume und sind
nicht vergleichbar.

Die Prozentwerte sind aus den ungerundeten Zuwächsen gerechnet. Sie beschreiben die
Entwicklung dieses einen Messpunkts. **Gerätebestand und Nutzungsdauer sind nicht für alle
Monate ausreichend dokumentiert**, deshalb wird der Unterschied nicht pauschal einer
bestimmten Regel zugeschrieben. Der höhere Märzwert zeigt außerdem, dass die Entwicklung
nicht jeden Monat in dieselbe Richtung ging.

Eine ältere Auswertung nennt den 29.04.2026 als Datum einer Sparautomation. Eine Prüfung
der Nachtstunden vor und nach diesem Datum zeigt jedoch schon davor fast durchgehend
keinen Zuwachs. **Das Datum allein ist deshalb kein Beleg dafür, dass dort eine hohe
Nachtlast verschwunden wäre.** Welche Änderung damals genau vorgenommen wurde, müsste
zusätzlich zugeordnet werden.

Die drei Phasenreihen der Gesamtmessung ergeben für das Kalenderjahr 2025 rund 2.112 kWh.
Das ist ein Statistikwert mit den beschriebenen Unsicherheiten und gehört zu einem anderen
Zeitraum als die 2.154 kWh der Abrechnung von November 2024 bis Oktober 2025. Beide Werte
werden nicht addiert und nicht als dieselbe Messung behandelt.

Monatsdifferenzen wurden aus der kumulierten Statistiksumme an exakten lokalen
Monatsgrenzen gebildet. Bei Stundenstatistiken entspricht der verwendete Eintrag dem
Stundenintervall vor der Grenze. Fehlende Grenzen werden nicht still interpoliert;
fehlende Stundenzeilen und Zeitumstellungen bleiben dokumentiert.

## 4 Zwei Messausfälle, die völlig unterschiedlich wirken

Die Daten liefern einen lehrreichen Fall für den Umgang mit Lücken. Im September 2025
standen die Phasensummen an zwei Tagen still, am 9. September folgte ein auffällig großer
Zuwachs. Dokumentiert ist ein Ausfall von Home Assistant, **während der Shelly
weiterzählte**.

| Datum | Zuwachs der drei Phasen zusammen |
|---|---:|
| 7. September 2025 | 0,00 kWh |
| 8. September 2025 | 0,00 kWh |
| 9. September 2025 | 20,45 kWh |
| 10. September 2025 | 5,40 kWh |

Der große Zuwachs am 9. September ist nicht der Verbrauch dieses einen Tages, sondern
enthält das Nachgeholte. Wer zusätzlich für die beiden Nulltage eine normale Tagesmenge
hochrechnet und auf die Monatssumme schlägt, **zählt denselben Strom zweimal**.

Im Mai 2026 war es umgekehrt. Vom 15. bis 19. Mai steht ebenfalls kein Zuwachs, dokumentiert
ist hier ein stromloser Messzähler mit anschließendem Wiederanlauf und einer
Klemmenreparatur. Die in dieser Zeit nicht erfasste Menge lässt sich **nicht** durch einen
späteren Zählerstand desselben ausgefallenen Geräts nachholen.

Für Mai ergibt die rohe Statistik rund 142,3 kWh. Eine spätere Auswertung ergänzt eine
Schätzung und kommt auf rund 169,7 kWh. Der höhere Wert ist keine nachträglich gefundene
Messung, sondern ein ergänzter Wert mit Annahme, und wird auch so behandelt.

**Die Unterscheidung entsteht aus Verlauf und dokumentiertem Ereignis zusammen.** Eine
Nullzeile allein sagt nicht, ob ein Gerät nichts verbrauchte, eine Verbindung fehlte oder
das Messgerät selbst ausgefallen war. Genau deshalb bleiben Rohdaten, Ergänzung und
Begründung getrennt nachvollziehbar.

## 5 Was mit diesen Daten nicht geht

Für das Lieferjahr November 2025 bis Oktober 2026 existiert eine Prognose von rund
2.090 kWh. Sie beruht teilweise auf einer rückwärts gerechneten Anfangsangabe und einem
vorausgerechneten Ende und wird deshalb nicht als abgerechnetes zweites Jahr verwendet.
Die Nachkommastelle einer Hochrechnung macht ihre Annahmen nicht genauer.

Für den Arbeitsplatz beginnt die Untersuchung mit der vorhandenen Regel und den bereits
gespeicherten Daten. **Die Nachtabschaltung wird nicht aufgehoben, um einen höheren
Ausgangsverbrauch zu erzeugen.** Das wäre die einzige Methode, eine saubere Vorher-Messung
zu bekommen, und sie wäre den Mehrverbrauch nicht wert.

Für eine Aussage zur eingesparten Energie bräuchte es einen geeigneten Vergleichszustand:
derselbe Messpunkt, nachvollziehbare Zeitgrenzen, vergleichbare Nutzung. Fehlt eine
brauchbare Grundlage, bleibt die Wirkung unbeziffert. Eine Rechnung mit angenommenem
Bereitschaftsverbrauch wäre dann ein Rechenbeispiel und kein Ergebnis.

## 6 Was offen bleibt

- Für den Rechner ist der genaue Ablauf des Herunterfahrens und sein zeitlicher Abstand
  zur Stromtrennung ungeprüft. **Die Nachtregel enthält keine Bedingung, die ein
  abgeschlossenes Herunterfahren nachweist.**
- Die Ursache sehr kleiner oder als null gemeldeter Nachtwerte ist nicht eindeutig
  geklärt. Fehlende Werte werden nicht durch erfundene Ersatzwerte ergänzt.
- Der Test der Abschaltvorlage an einer echten Last steht aus. Geprüft wurde mit
  simuliertem Sensor und simulierter Steckdose.

Der Jahresverbrauch von 2.154 kWh ist seit dem 17.10.2026 an der Originalabrechnung des
Versorgers geprüft. Dabei kam heraus, dass der Endzählerstand errechnet und nicht abgelesen
war: 98 von 2.154 kWh, also 4,5 Prozent. Eine Gegenprobe über den vollständig abgelesenen
Teil von 348 Tagen weicht um 0,1 Prozent ab. Der Fall steht als Beispiel in Kapitel 1.1.
