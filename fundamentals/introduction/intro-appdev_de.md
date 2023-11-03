# Einführung

In diesem Abschnitt des Workshops werden neue Operatoren, Parameter, Ausdrücke und SQL-Makros vorgestellt, die im neuesten Release von Oracle Database, 21c, verfügbar sind. Oracle 21c stellt drei neue Operatoren vor: EXCEPT, EXCEPT ALL und INTEREST ALL. Die SQL-Setoperatoren unterstützen jetzt alle Schlüsselwörter, die in ANSI SQL definiert sind. Der neue Operator EXCEPT \[ALL\] entspricht funktional MINUS \[ALL\]. Die Operatoren MINUS und INTERSECT unterstützen nun das Schlüsselwort ALL.

Sie können Ausdrücke in Initialisierungsparametern verwenden, die beim Hochfahren der Datenbank ausgewertet werden. Sie können jetzt einen Ausdruck angeben, der die aktuelle Systemkonfiguration und -umgebung berücksichtigt. Dies ist besonders in Oracle Autonomous Database-Umgebungen nützlich.

Sie können SQL-Makros (SQM) erstellen, um gängige SQL-Ausdrücke und -Anweisungen in wiederverwendbare, parametrisierte Konstrukte umzuwandeln, die in anderen SQL-Anweisungen verwendet werden können. SQL-Makros können skalare Ausdrücke sein, die normalerweise in SELECT-Listen, WHERE-, GROUP BY- und HAVING-Klauseln verwendet werden, um Berechnungen und Geschäftslogik zu kapseln, oder Tabellenausdrücke sein, die normalerweise in einer FROM-Klausel verwendet werden.

Geschätzte Workshop-Zeit: 60 Minuten

### Labors

*   Neue Set-Operatoren
*   Ausdrücke in Init-Parametern
*   SQM Skalare und Tabellenausdrücke
*   Text/JSON in In-Memory durchsuchen

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

Mit 21c in Autonomous Database können Kunden:

*   Reduzierte IT-Kosten und -Komplexität
*   Innovation freisetzen
*   Entwickeln Sie leistungsstarke, datengesteuerte Anwendungen

## Weitere Informationen

*   [Oracle Database-Blog](http://blogs.oracle.com/database)
*   [Oracle Database 21c - Einführung](https://blogs.oracle.com/database/introducing-oracle-database-21c)

## Danksagungen

*   **Autor** - Donna Keesling, Datenbank-UA-Team
*   **Mitwirkende** - Kay Malcolm, David Start, Kamryn Vinson, Anoosha Pilli
*   **Zuletzt aktualisiert am/um** - Kay Malcolm, März 2020