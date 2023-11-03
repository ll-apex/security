# Einführung

## Über diesen Workshop

Oracle Identity Governance (OIG) ist ein leistungsstarkes und flexibles Identity Management-System für Unternehmen, das die Zugriffsberechtigungen von Benutzern innerhalb von Unternehmens-IT-Ressourcen automatisch verwaltet. Oracle Identity Role Intelligence (OIRI) ist ein neuer Microservice, der als Teil der OIG-Suite verfügbar ist und eine intelligente, automatisierte und flexible Möglichkeit zur Optimierung der rollenbasierten Zugriffskontrolle (Role-Based Access Control, RBAC) darstellt. Dieser Workshop bietet praktische Erfahrungen beim Deployment von OIRI, beim Importieren von Daten aus OIG, beim Ausführen von Role Mining und beim Veröffentlichen von Rollen in OIG aus OIRI.

_Geschätzte Laborzeit_: 2 Stunden

### Über Produkt/Technologie

Oracle Identity Role Intelligence ist eine intelligente, automatisierte und flexible Möglichkeit zur Optimierung der rollenbasierten Zugriffskontrolle (Role-Based Access Control, RBAC). OIRI ist ein containerisierter Microservice und eine Erweiterung zu Oracle Identity Governance (OIG). Sie können den Microservice On Premise oder in der Cloud bereitstellen. Es kann auf Kubernetes-Containern für Ihre On-Premise-Landschaft bereitgestellt werden. Zu den wichtigsten Funktionen von OIRI gehören:

*   Erkennen von Berechtigungsmustern über Peer-Gruppen hinweg
*   Unterstützung für den Top-down-Ansatz für Role Mining basierend auf Benutzerattributen oder für den Bottom-up-Ansatz, der Daten basierend auf Anwendungen und Berechtigungen filtert, oder einen hybriden Ansatz
*   Kandidatenrollen mit vorhandenen Rollen vergleichen, um Rollenexplosion zu vermeiden
*   Optimieren der Kandidatenrollen basierend auf Benutzeraffinität und Rollenaffinität
*   Automatisches Veröffentlichen von Rollen in OIG, um Workflow zur Rollenakzeptanz auszulösen
*   Möglichkeit, Daten aus verschiedenen Quellen wie OIG-Datenbank und Flat Files zusammenzuführen und Was-wäre-wenn-Analysen bereitzustellen, bevor Kandidatenrollen in die Produktion verschoben werden

### Ziele

In diesem Workshop werden Sie:

*   OIRI auf dem lokalen Kubernetes-Knoten bereitstellen
*   Daten aus OIG in OIRI importieren
*   Rollen-Mining ausführen und Kandidatenrollen analysieren
*   Rolle in OIG veröffentlichen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account

_Hinweis: Wenn Sie über ein **kostenloses Testkonto** verfügen, wird Ihr Account nach Ablauf der kostenlosen Testversion in einen Account vom Typ "**Immer kostenlos**" konvertiert. Sie können keine Free Tier-Workshops durchführen, es sei denn, die Umgebung vom Typ "Immer kostenlos" ist verfügbar. **[Klicken Sie hier, um die häufig gestellte Fragen zu Free Tier aufzurufen.](https://www.oracle.com/cloud/free/faq.html)**_

## Weitere Informationen

*   [Oracle Identity Management 12.2.1.4.0](https://docs.oracle.com/en/middleware/idm/suite/12.2.1.4/index.html)
*   [Oracle Identity Role Intelligence](https://docs.oracle.com/en/middleware/idm/identity-role-intelligence/amiri/overview-oracle-identity-role-intelligence.html)

## Danksagungen

*   **Autor** - Keerti R, Brijith TG, Anuj Tripathi, NATD Solution Engineering
*   **Mitwirkende** - Keerti R, Brijith TG, Anuj Tripathi
*   **Zuletzt aktualisiert am/um** - Keerti R, NATD Solution Engineering, Juni 2021