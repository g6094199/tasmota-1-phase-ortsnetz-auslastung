# tasmota-1-phase-ortsnetz-auslastung
Anbindung von einpahsigen Tasmota Geräten (zB. Schaltbare Steckdosen) an die Ortsnetz Auslastung API


# Tasmota → Ortsnetz-Auslastung

Diese Tasmota-Regel überträgt die Netzspannung einer Tasmota-Steckdose direkt
an die API von ortsnetz-auslastung.de. Die Übertragung erfolgt mit Tasmota-
Bordmitteln (`Rules` und `WebQuery`). Es werden weder Tasmota-Scripting noch
MQTT, Node-RED oder Home Assistant benötigt.

## Voraussetzungen

- Tasmota meldet einen Spannungswert unter `ENERGY.Voltage`.
- Das Tasmota-Binary enthält `Rules` und `WebQuery`.
- Für die HTTPS-Verbindung ist ein ESP32 erforderlich.
- Die Steckdose hat Internetzugang und eine gültige Uhrzeit via NTP.
- Breitengrad, Längengrad und die tatsächliche Phase der Steckdose sind bekannt.

Beispiel einer passenden Tasmota-Telemetrie:

```json
{
  "Time": "2026-10-21T17:20:36",
  "ENERGY": {
    "Voltage": 233,
    "Current": 0.031
  }
}
```

## Einrichtung

1. Öffne die Tasmota-Weboberfläche und gehe zu **Consoles**.
2. Prüfe mit `Rule1`, ob dieser Regelsatz bereits verwendet wird. Falls ja,
   verwende stattdessen `Rule2` oder `Rule3` und ändere die Befehle entsprechend.
3. Öffne `tasmota/rule-console.txt`, ersetze die Beispielkoordinaten und passe
   die Phasenzuordnung an.
4. Sende die Befehle aus der Datei einzeln über die Tasmota-Konsole.
5. Prüfe die Konsole nach dem nächsten Telemetrie-Intervall auf eine
   WebQuery-Antwort oder einen Fehler.

Die Regel sendet standardmäßig alle 300 Sekunden. Das Intervall lässt sich
mit `TelePeriod` ändern.

## Phasenzuordnung

Die mitgelieferte Regel ordnet den Steckdosenwert **L1** zu. Ändere die Felder
im JSON, wenn die Steckdose tatsächlich an L2 oder L3 angeschlossen ist.
Nicht vorhandene Phasen werden mit `-1` gemeldet.

Die Netzfrequenz wird nicht übertragen, da sie in der gezeigten
Tasmota-Messung nicht enthalten ist.

## Zeitstempel

Die Regel verwendet `%timestamp%Z`. `Timezone 0` stellt die Gerätezeit auf UTC;
eine funktionierende NTP-Zeitsynchronisierung ist erforderlich.

## Deaktivieren

Um die Regel zu deaktivieren, sende zum Beispiel:

```text
Rule1 0
```

Das deaktiviert die Regel, löscht sie aber nicht.
