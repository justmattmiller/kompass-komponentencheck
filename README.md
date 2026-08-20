# Kompass-Komponentencheck

Ein einzelnes, offline lauffähiges HTML-Werkzeug, das eine CSV-Datei mit
Design-System-Audit-Daten einliest und sichtbar macht, wo die Umsetzung vom
Spec abweicht.

## Szenario

Fiktives Beispielunternehmen: **Terra Marken GmbH**, drei Marken — *Terra
Frisch*, *Terra Wohnen*, *Terra Kids* — die sich ein gemeinsames
Design-System namens **„Kompass"** teilen. Jede Zeile in der CSV ist eine
konkrete Komponenten-Instanz (z. B. der Primär-Button im Bestell-Team von
Terra Frisch) mit dem laut Spec **erwarteten** Wert und dem **tatsächlich**
umgesetzten Wert.

## Was das Werkzeug macht

1. CSV per Drag & Drop oder Dateiauswahl laden
2. Zeilen mit `Erwartet ≠ Tatsächlich` werden als **Abweichung** markiert
3. Abweichungen werden eingeordnet:
   - **Kandidat für neue Variante** — `Begründung` ist ausgefüllt
   - **Prüfung erforderlich** — `Begründung` ist leer
4. Sortierung: Abweichungen zuerst, absteigend nach `Nutzungshäufigkeit`;
   danach Übereinstimmungen in ursprünglicher Reihenfolge
5. Alterung: Differenz von mehr als 180 Tagen zum aktuellsten Datum in
   `Zuletzt_geändert` gilt als **auffällig alt**

Der erwartete Wert (`Erwartet`) wird je Zeile ausgewertet, also pro
Marke und Komponenten-Instanz — nicht als ein einziger globaler Wert pro
`Komponente`. Dieselbe Komponente darf bei unterschiedlichen Marken
unterschiedliche, jeweils gültige Vorgaben haben (z. B. Primär-Button bei
Terra Frisch in Kompass-Blau, bei Terra Kids in Kompass-Gelb) — das ist
kein Abweichungsfall, solange `Tatsächlich` der markeneigenen Vorgabe
entspricht.

Alle angewendeten Regeln (Sortierung, Alterungsschwelle) werden direkt auf
der Seite offengelegt — nicht versteckt in einer Konfigurationsdatei.

## Datenschutz

**Die CSV-Datei verlässt den Browser nie.** Es findet keinerlei
Netzwerkkommunikation statt — kein Upload, kein externer Aufruf, kein
Tracking. Das Werkzeug ist eine einzelne HTML-Datei ohne Build-Schritt, ohne
Server und ohne externe Bibliotheken; sie läuft komplett offline per
Doppelklick.

## Benutzung

1. [`index.html`](index.html) im Browser öffnen (kein Server nötig)
2. CSV-Datei aus [`sample-data/`](sample-data) ablegen oder eigene Datei
   verwenden

### CSV-Format

Semikolon-getrennt, UTF-8, mit folgenden Spalten:

| Spalte | Bedeutung |
|---|---|
| `Instanz_ID` | eindeutige Kennung der Komponenten-Instanz |
| `Marke` | Terra Frisch / Terra Wohnen / Terra Kids |
| `Komponente` | Name der Komponente (z. B. Primär-Button) |
| `Erwartet` | laut Kompass-Spec erwarteter Wert |
| `Tatsächlich` | tatsächlich umgesetzter Wert |
| `Team` | verantwortliches Team |
| `Zuletzt_geändert` | Datum der letzten Änderung (TT.MM.JJJJ oder JJJJ-MM-TT) |
| `Nutzungshäufigkeit` | Nutzungshäufigkeit, als Zahl (Komma als Dezimaltrennzeichen) |
| `Begründung` | Begründung für eine Abweichung, falls vorhanden |

## Bekannte Grenzen

- **Muster ist nicht Bedeutung:** Zeile `INST-010` besteht jede Prüfung
  (Erwartet = Tatsächlich), wird aber seit Januar 2023 nicht mehr geändert
  und kaum genutzt. Das Werkzeug kann nicht unterscheiden, ob das eine
  stabile, bewährte Komponente ist oder eine tote, die niemand mehr
  braucht — beides sieht in den Daten gleich aus. Das ist bewusst so
  belassen: die Alterungs-Kennzeichnung soll einen Menschen zum genaueren
  Hinsehen bringen, nicht automatisch entscheiden.
- **`Begründung` wird nur auf Leere geprüft, nicht auf Qualität.** Ein
  leeres Feld heißt nicht zwingend, dass keine gültige Begründung
  existiert — nur, dass niemand sie eingetragen hat. Eine lokale, regelbasierte
  Qualitätsheuristik ist als nächster Schritt geplant.

## Status

Ursprünglich als Übung für einen Kurs zu KI-Anwendungsfällen entstanden,
wird dieses Repository jetzt als Portfolio-Stück weitergeführt — inklusive
echtem Branch/PR-Workflow für neue Verbesserungen.

## Lizenz

[MIT](LICENSE)
