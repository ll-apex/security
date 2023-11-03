# Einführung

In diesem Abschnitt des Workshops werden Verbesserungen in Oracle Database 21c zur Verbesserung der Sicherheit von Passwörtern vorgestellt.

Geschätzte Zeit: 60 Minuten

## Administration in Oracle Database 21c

Ab diesem Release wird der Parameter zum Aktivieren oder Deaktivieren der Groß-/Kleinschreibung bei Kennwortdateien entfernt. Bei allen Passwörtern in neuen Passwortdateien muss die Groß-/Kleinschreibung beachtet werden.

Bei Passwortdateien, bei denen die Groß-/Kleinschreibung beachtet wird, erhalten Sie mehr Sicherheit als bei älteren Passwortdateien, bei denen die Groß-/Kleinschreibung nicht beachtet wird. Oracle empfiehlt die Verwendung von Kennwortdateien, bei denen die Groß-/Kleinschreibung beachtet wird. Upgegradete Kennwortdateien aus früheren Oracle Database-Releases können jedoch ihre ursprüngliche Groß-/Kleinschreibung nicht beachten. Bei Passwortdateien muss zwischen Groß- und Kleinschreibung unterschieden werden, indem Sie Passwortdateien von einem Format in ein anderes migrieren.

### Labors

*   Groß- und Kleinschreibung bei aktualisierten Kennwortdateien
*   Durchsetzung der Kennwortlänge

### Voraussetzungen

*   Ein Oracle Cloud-Account - Auf der Landingpage dieses Workshops können Sie sehen, welche Umgebungen unterstützt werden
*   Praktische Erfahrung mit vi

## Oracle Database 21c

Die 21c-Generation der konvergierten Datenbank von Oracle bietet Kunden erstklassigen Support für alle Datentypen (z. B. relationale, JSON, XML, räumliche, grafische, OLAP usw.) und branchenführende Performance, Skalierbarkeit, Verfügbarkeit und Sicherheit für alle ihre betrieblichen, analytischen und anderen gemischten Workloads.

![Oracle DB 21c - Vorteile](images/21c-support.png "Oracle DB 21c - Vorteile") Die wichtigsten Aktualisierungen in Datenbank 21c sind:

*   JSON-Binärdatentyp
*   Blockchain-Tabellen
*   Automatisches maschinelles Lernen mit Python
*   Verbesserungen für Sharding, In-Memory-Datenbank und Diagrammanalysen.

Mit 21c können Kunden

*   Reduzierte IT-Kosten und -Komplexität
*   Innovation freisetzen
*   Entwickeln Sie leistungsstarke, datengesteuerte Anwendungen

## Weitere Informationen

*   [Oracle Database-Blog](http://blogs.oracle.com/database)
*   [Oracle Database 21c - Einführung](https://blogs.oracle.com/database/introducing-oracle-database-21c)

## Danksagungen

*   **Autor** - Donna Keesling, Datenbank-UA-Team
*   **Mitwirkende** - Kay Malcolm, David Start, Kamryn Vinson, Anoosha Pilli
*   **Zuletzt aktualisiert am/um** - Kay Malcolm, November 2020