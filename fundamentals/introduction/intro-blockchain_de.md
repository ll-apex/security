# Einführung

In diesem Workshop werden Verbesserungen an der Oracle Database-Sicherheit vorgestellt, einschließlich Blockchain-Tabellen.

Geschätzte Workshop-Zeit: 60 Minuten

## Blockchain-Tabellen in Oracle Database 21c

Blockchain-Tabellen sind reine Anhängen-Tabellen, in denen nur Einfügevorgänge zulässig sind. Das Löschen von Zeilen ist basierend auf der Zeit entweder unzulässig oder eingeschränkt. Zeilen in einer Blockchain-Tabelle werden durch spezielle Sequenzierungs- und Verkettungsalgorithmen manipulationssicher gemacht. Benutzer können prüfen, ob Zeilen nicht manipuliert wurden. Ein Hashwert, der Teil der Zeilenmetadaten ist, wird zum Verketten und Validieren von Zeilen verwendet.

Mit Blockchain-Tabellen können Sie ein zentralisiertes Ledger-Modell implementieren, bei dem alle Teilnehmer im Blockchainnetzwerk Zugriff auf dasselbe manipulationssichere Ledger haben.

Ein zentralisiertes Ledger-Modell reduziert den Verwaltungsaufwand für die Einrichtung eines dezentralen Ledger-Netzwerks, führt zu einer relativ geringeren Latenz im Vergleich zu dezentralen Ledgern, steigert die Entwicklerproduktivität, verkürzt die Time-to-Market und führt zu erheblichen Einsparungen für das Unternehmen. Datenbankbenutzer können weiterhin dieselben Tools und Übungen verwenden, die sie für die Entwicklung anderer Datenbankanwendungen verwenden würden.

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