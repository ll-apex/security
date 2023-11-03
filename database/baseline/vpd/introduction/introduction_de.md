# Einführung

## Über diesen Workshop

In diesem Workshop werden die Funktionen von Oracle Virtual Private Database (VPD) vorgestellt. Sie bietet dem Benutzer die Möglichkeit, zu lernen, wie Sie dieses Feature konfigurieren, um die Sicherheit auf Zeilen- und Spaltenebene zu implementieren. Oracle VPD erstellt Sicherheits-Policys zur Kontrolle des Datenbankzugriffs auf Zeilen- und Spaltenebene.

_Geschätzte Workshopzeit_: 45 Minuten

### Über VPD

Oracle Virtual Private Database erzwingt die Sicherheit auf ein feines Maß an Granularität direkt in Datenbanktabellen, Views oder Synonymen. Da Sie Sicherheits-Policys direkt an diese Datenbankobjekte anhängen und die Policys automatisch angewendet werden, wenn ein Benutzer auf Daten zugreift, können Sie die Sicherheit nicht umgehen.

Wenn ein Benutzer direkt oder indirekt auf eine Tabelle, View oder ein Synonym zugreift, das mit einer Oracle Virtual Private Database-Policy geschützt ist, ändert Oracle Database die SQL-Anweisung des Benutzers dynamisch. Diese Änderung erstellt eine WHERE-Bedingung (als Prädikat bezeichnet), die von einer Funktion zurückgegeben wird, die die Sicherheits-Policy implementiert. Oracle Database ändert die Anweisung dynamisch und transparent für den Benutzer. Dabei wird jede Bedingung verwendet, die in einer Funktion ausgedrückt oder von einer Funktion zurückgegeben werden kann. Sie können Oracle Virtual Private Database-Policys auf SELECT-, INSERT-, UPDATE-, INDEX- und DELETE-Anweisungen anwenden.

### Ziele

Machen Sie sich mit den Grundlagen von Oracle Virtual Private Database (VPD) vertraut, einschließlich der Einschränkung von Zeilen und Spalten, die in einer Benutzerabfrage zurückgegeben werden. In dieser Übung wird gezeigt, wie Sie eine PL/SQL-Funktion erstellen und auf eine Tabelle anwenden, um Zeilen basierend auf Sessionbenutzer- und Client-ID einzuschränken, ohne dass sich dies auf die Glassfish-basierte HR-Anwendung auswirkt.

Das gesamte DB Security PMs Team wünscht Ihnen einen ausgezeichneten Workshop!

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten (nur _Free-Tier_ und _bezahlte Mandanten_)
    *   Übung: Umgebungssetup
    *   Übung: Umgebung initialisieren

## Danksagungen

*   **Autor** - Stephen Stuart & Noah Galloso, Solution Engineers, North America Specialist Hub
*   **Mitwirkende** - Richard C. Evans, Produktmanager für Datenbanksicherheit
*   **Zuletzt aktualisiert am/um** - Stephen Stuart & Noah Galloso, August 2023