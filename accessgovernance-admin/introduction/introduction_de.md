# Einführung

## Über diesen Workshop

Oracle Access Governance löst die wachsenden Herausforderungen, denen Sicherheitsverantwortliche bei der Bewältigung der zunehmenden Sicherheitsbedrohungen und -vorschriften gegenüberstehen. Mit dieser Cloud-nativen Lösung können Sie Governance- und Complianceanforderungen für viele Anwendungen, Workloads, Infrastrukturen und Identitätsplattformen erfüllen. Sie bietet unternehmensweite Transparenz und Funktionen, um Anomalien zu erkennen und Sicherheitsrisiken in Cloud- und On-Premises-Umgebungen zu minimieren. Mit erweiterten Analysen bietet Oracle Access Governance eine intuitive Benutzererfahrung, die Empfehlungen und Einblicke in Zugriffsberechtigungen, Verhaltensweisen und Risiken bietet.

Die Grafik unten zeigt die allgemeine Architektur von Oracle Access Governance.

![Kampagnenliste anzeigen](images/oracle-access-governance-overview.png)

In dieser Übung werden Sie durch die Schritte für die ersten Schritte mit **Oracle Access Governance** mit verschiedenen Anwendungsfällen geführt: **Zugriffskontrollen, Zugriffsprüfungen und Policy-Prüfungen**. In diesem Workshop verwendet ein fiktives Unternehmen Oracle Access Governance, um den Anwendungszugriff seiner Mitarbeiter und Auftragnehmer zu verwalten und zu steuern. In dieser Übung wird gezeigt, wie die Datenbank mit AG als Zielsystem verbunden ist und die Zugriffskontrolle implementiert wird, indem Identitäts-Collections, Genehmigungsworkflows, Rollen, Zugriffs-Bundles und zentralisierte Policys erstellt werden. Außerdem wird gezeigt, wie Sie Zugriffsprüfungs- und Policy-Prüfungskampagnen sowie zugehörige Prüfungsaufgaben ausführen.

**Oracle Access Governance** ermöglicht:

*   **Administrator** zur Ausführung verschiedener administrativer System-, Service- und Zugriffskontrollaufgaben
*   **Kampagnenadministrator** zum Ausführen intelligenter Zugriffsprüfungskampagnen für Zugriffs-Governance und Compliance
*   **Prüfer für Cloud-Zugriff**, um Einblicke in den Policy-Zugriff zu prüfen und fundierte Entscheidungen basierend auf **vorschriftsmäßigen Analysen zu treffen**
*   **Benutzer** und **Benutzermanager** validieren den Zugriff, der dem Benutzer und seinen direkt unterstellten Mitarbeitern zugewiesen ist.
*   **Access Control-Administrator** zum Verwalten von Rollen, Identitätserfassungen, Policys und Genehmigungsworkflows.

_Geschätzte Workshopzeit:_ 3 Stunden

### Ziele

In diesem Workshop erfahren Sie, wie Sie:

*   Oracle Access Governance-Serviceinstanz einrichten und konfigurieren
*   Oracle Access Governance-Agents für OIG und Database installieren und konfigurieren
*   AG-Dataload ausführen und IAM-Benutzer erstellen
*   Integration mit OCI als Cloud-Identitätsprovider
*   Identity Collections verwalten, auf Bundles, Policys und Genehmigungsworkflows zugreifen
*   Zugriffsprüfungskampagne erstellen und Zugriffsprüfungsaufgaben für das Zieldatenbanksystem ausführen
*   Policy-Prüfungskampagne erstellen und Policy-Prüfungsaufgaben für die OCI-IAM-Policys ausführen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Ein Mandant mit administrativem Zugriff

## Weitere Informationen

*   [Oracle Access Governance - Produktseite](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Dokumentation für Oracle Access Governance](https://docs.oracle.com/en/cloud/paas/access-governance/index.html)
*   [Oracle Access Governance - Produkttour](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access Governance - Häufig gestellte Fragen](https://www.oracle.com/security/cloud-security/access-governance/faq/)
*   [Oracle Access Governance - Ankündigungsblog](https://blogs.oracle.com/cloudsecurity/post/intelligent-cloud-delivered-access-governance-with-prescriptive-analytics)

## Danksagungen

*   **Autoren** - Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu