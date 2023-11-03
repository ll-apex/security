# Einführung

In diesem Abschnitt des Workshops arbeiten Sie am Upgrade von Oracle-Datenbanken mit dem Utility AutoUpgrade. Das Utility AutoUpgrade identifiziert Probleme vor Upgrades, führt Aktionen vor und nach dem Upgrade aus, stellt Upgrades bereit, führt Aktionen nach dem Upgrade aus und startet das upgegradete Oracle ohne menschliches Eingreifen.

Das Utility AutoUpgrade wurde entwickelt, um den Upgradeprozess zu automatisieren, sowohl vor dem Start von Upgrades, während Upgrade-Deployments als auch bei Post-Upgrade-Prüfungen und Konfigurationsmigration. Sie verwenden AutoUpgrade, nachdem Sie Binärdateien für das neue Oracle Database-Release heruntergeladen und neue Oracle Homes für das Release eingerichtet haben. Wenn Sie AutoUpgrade verwenden, können Sie mehrere Oracle Database-Deployments gleichzeitig mit einer einzelnen Konfigurationsdatei upgraden, die für jedes Datenbank-Deployment nach Bedarf angepasst wird. AutoUpgrade kann auch zum Upgrade von Nicht-CDB-Datenbanken auf PDB-Datenbanken verwendet werden.

Geschätzte Workshop-Zeit: 60 Minuten

### Labors

*   AutoUpgrade
*   AutoUpgrade: Nicht-CDB zu CDB

### Voraussetzungen

*   Ein Oracle Cloud-Account - Auf der Landingpage LiveLabs dieses Workshops können Sie sehen, welche Umgebungen unterstützt werden
*   Praktische Erfahrung mit vi

_Hinweis: Wenn Sie über ein **kostenloses Testkonto** verfügen, wird Ihr Account nach Ablauf der kostenlosen Testversion in einen Account vom Typ "**Immer kostenlos**" konvertiert. Sie können keine Free Tier-Workshops durchführen, es sei denn, die Umgebung vom Typ "Immer kostenlos" ist verfügbar. **[Klicken Sie hier, um die häufig gestellte Fragen zu Free Tier aufzurufen.](https://www.oracle.com/cloud/free/faq.html)**_

Sie können jetzt [mit der nächsten Übung fortfahren](#next).

## Oracle Database 21c

Die 21c-Generation der konvergierten Datenbank von Oracle bietet Kunden erstklassige Unterstützung für alle Datentypen (z. B. relationale, JSON, XML, räumliche, grafische, OLAP usw.) und branchenführende Performance, Skalierbarkeit, Verfügbarkeit und Sicherheit für alle ihre betrieblichen, analytischen und anderen gemischten Workloads.

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