# Einführung

In diesem Abschnitt des Workshops werden die Verbesserungen in Oracle Database 21c für Oracle Audit-Policys vorgestellt. Ab diesem Release werden einheitliche Audit-Policys für den aktuellen Benutzer durchgesetzt, der die SQL-Anweisung ausführt.

In früheren Releases wurden einheitliche Audit-Policys für den Benutzer erzwungen, der Eigentümer der Benutzersession der obersten Ebene (d.h. der Anmeldebenutzersession) war, in der die SQL-Anweisung ausgeführt wird.

Zu den Szenarios, in denen sich der aktuelle Benutzer vom Anmeldebenutzer unterscheidet, gehören unter anderem:

*   Triggerausführung
*   Ausführung der Definer Rights-Prozedur
*   Funktionen und Prozeduren, die während der Auswertung von Views ausgeführt werden

Geschätzte Workshop-Zeit: 60 Minuten

### Labors

*   Einheitliche Audit-Policys
*   Einheitliche Audit-Policys für STIG
*   SYSLOG-Ziel für Audit-Policys

### Voraussetzungen

*   Ein Oracle Cloud-Account - Auf der Landingpage LiveLabs dieses Workshops können Sie sehen, welche Umgebungen unterstützt werden
*   Praktische Erfahrung mit vi

_Hinweis: Wenn Sie über ein **kostenloses Testkonto** verfügen, wird Ihr Account nach Ablauf der kostenlosen Testversion in einen Account vom Typ "**Immer kostenlos**" konvertiert. Sie können keine Free Tier-Workshops durchführen, es sei denn, die Umgebung vom Typ "Immer kostenlos" ist verfügbar. **[Klicken Sie hier, um die häufig gestellte Fragen zu Free Tier aufzurufen.](https://www.oracle.com/cloud/free/faq.html)**_

Sie können jetzt [mit der nächsten Übung fortfahren](#next).

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