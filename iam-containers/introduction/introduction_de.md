# Einführung

## Über diesen Workshop

Mit Oracle Identity Management können Unternehmen den End-to-End-Lebenszyklus von Benutzeridentitäten über alle Unternehmensressourcen hinweg effektiv verwalten, sowohl innerhalb als auch außerhalb der Firewall und in der Cloud. Die Oracle Identity Management-Plattform bietet skalierbare Lösungen für Identity Governance, Zugriffsverwaltung und Verzeichnisservices. Diese moderne Plattform hilft Unternehmen, die Sicherheit zu stärken, die Compliance zu vereinfachen und Geschäftsmöglichkeiten rund um den mobilen und sozialen Zugriff zu erfassen.

Oracle hat das Bereitstellungsmodell DevOps integriert, indem Container für Docker und Kubernetes genutzt werden, um das Lebenszyklusmanagement von Oracle Identity and Access Management-Produkten zu modernisieren. Dieser Ansatz vereinfacht die Bereitstellung und Wartung von Oracle Identity and Access Management-Produkten über verschiedene Deployments in physischen, privaten Clouds oder Public Clouds hinweg.

Kubernetes ist ein System zum Ausführen und Koordinieren von containerisierten Anwendungen über Cluster hinweg. Es verwaltet den Lebenszyklus containerisierter Anwendungen und Services und bietet so Vorhersagbarkeit, Skalierbarkeit und High Availability.

Oracle stellt einen Open-Source-WebLogic Server Kubernetes-Operator bereit, der mehrere wichtige Features zur Unterstützung der Bereitstellung und Verwaltung verschiedener Fusion Middleware-Produkte bietet, einschließlich Oracle Identity Governance (OIG) und Oracle Access Management (OAM). Das Oracle Unified Directory-Deployment auf Kubernetes nutzt Deployment-Skripte von Oracle zum Erstellen von Oracle Unified Directory-Containern anhand von bereitgestellten Beispielen oder Helm-Diagrammen.

Mit Hilfe des Kubernetes-Operators und Out-of-the-box-Deployment-Skripten können Sie:

1.  Erstellen Sie OIG/OAM/OUD-Instanzen in einem persistenten Kubernetes-Volume. Dieses persistente Volume kann sich in einem NFS-Dateisystem oder anderen Kubernetes-Volume-Typen befinden
2.  Server basierend auf deklarativen Startparametern und gewünschten Status starten
3.  OIG/OAM/OUD-Services für externen Zugriff bereitstellen
4.  Skalieren Sie OIG/OAM/OUD, indem Sie Server nach Bedarf starten und stoppen
5.  Operator und WebLogic Server-Logs bei Elasticsearch veröffentlichen und mit ihnen in Kibana interagieren
6.  OIG-Instanz mit Prometheus und Grafana überwachen

_Geschätzte Zeit:_ 90 Minuten

### Ziele

In diesem Workshop erfahren Sie, wie Sie:

*   Lokales Kubernetes-Cluster mit einem Knoten einrichten
*   OIG in der Kubernetes-Umgebung bereitstellen
*   OAM in der Kubernetes-Umgebung bereitstellen
*   OUD-Instanz in der Kubernetes-Umgebung erstellen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account

_Hinweis: Wenn Sie über ein **kostenloses Testkonto** verfügen, wird Ihr Account nach Ablauf der kostenlosen Testversion in einen Account vom Typ "**Immer kostenlos**" konvertiert. Sie können keine Free Tier-Workshops durchführen, es sei denn, die Umgebung vom Typ "Immer kostenlos" ist verfügbar. **[Klicken Sie hier, um die häufig gestellte Fragen zu Free Tier aufzurufen.](https://www.oracle.com/cloud/free/faq.html)**_

## Weitere Informationen

*   [OIG in einer Kubenetes-Umgebung](https://oracle.github.io/fmw-kubernetes/oig/)
*   [OAM in einer Kubenetes-Umgebung](https://oracle.github.io/fmw-kubernetes/oam/)
*   [OUD in einer Kubenetes-Umgebung](https://oracle.github.io/fmw-kubernetes/oud/)

## Danksagungen

*   **Autor** - Keerti R, Anuj Tripathi, NATD Solution Engineering
*   **Mitwirkende** - Keerti R, Anuj Tripathi
*   **Zuletzt aktualisiert am/um** - Keerti R, NATD Solution Engineering, Januar 2022