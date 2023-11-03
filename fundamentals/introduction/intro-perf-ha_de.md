# Einführung

In diesem Abschnitt des Workshops werden Verbesserungen in Oracle Database 21c zur Verbesserung der Performance und High Availability vorgestellt. Die Verbesserungen umfassen:

*   Ein Point-in-Time Recovery (Flashback), um eine Datenbank ab einem bestimmten Zeitpunkt wiederherzustellen
*   Automatische Zonenzuordnungen, um das Pruning von Blöcken und Partitionen basierend auf den Prädikaten in den Abfragen ohne Benutzereingriff zu ermöglichen
*   SecureFile-LOBs verkleinern, Speicherplatz freigeben und Performance verbessern
*   Automatisches In-Memory zum automatischen und dynamischen Erstellen von In-Memory-Objekten
*   In Memory Hybrid Scans wählt automatisch die optimale Methode zum Scannen von Zeilen, die sowohl im Arbeitsspeicher als auch nicht im Arbeitsspeicher enthaltene Spaltendaten enthalten. Dies kann die Leistung um Größenordnungen verbessern
*   Anzahl der Anweisungen reduzieren, die zum Synchronisieren mehrerer Anwendungen in Anwendungs-PDBs erforderlich sind
*   Blockierende Session mit dem neuen Initialisierungsparameter MAX\_IDLE\_BLOCKER\_TIME beenden

Geschätzte Workshop-Zeit: 60 Minuten

### Labors

*   PDB-Point-in-Time Recovery
*   Automatische Zonenzuordnungen
*   SecureFile-LOBs
*   Automatischer In-Memory
*   In-Memory Hybrid Scans
*   Apps in App-PDBs synchronisieren
*   MAX\_IDLE\_BLOCKER\_TIME-Parameter

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