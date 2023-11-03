# Einführung

In den Übungen im Workshop **SecureOracle 8.0** werden einige der Funktionen erläutert, um mit der Verwendung von **Oracle IAM Suite 12c R2 PS4 (12.2.1.4.0)** zu beginnen. Sie üben Mitarbeiterlebenszyklusmanagement, Identitätszertifizierungen, RESTful-APIs für Identity Governance, OAuth-Client- und Tokenverwaltung.

Die Oracle IAM Suite bietet eine einheitliche, integrierte Sicherheitsplattform zur Verwaltung des Benutzerlebenszyklus und bietet sicheren Zugriff über Unternehmensressourcen hinweg, sowohl innerhalb als auch außerhalb der Firewall und in die Cloud.

_Geschätzte Workshopzeit_: 4 Stunden

### Ziele

Am Ende dieses Workshops haben Sie ein gutes Verständnis von:

*   SecureOracle Demoumgebung
*   Verwaltung des Mitarbeiterlebenszyklus
*   Identitätszertifizierungen
*   RESTful APIs für Identity Governance, OAuth Client- und Tokenmanagement

### Voraussetzungen

Für die Teilnahme an diesem Workshop ist Folgendes erforderlich:

*   Free Tier-, Cloud-Account vom Typ "Immer kostenlos", "Kostenpflichtig" oder LiveLabs von Oracle

## Über SecureOracle 8.0?

SecureOracle 8.0 ist eine Demoumgebung für die Oracle IAM Suite 12c R2 PS4 (12.2.1.4.0), die folgende Oracle-Komponenten enthält:

*   Identitäts-Governance:
    *   OIG 12c, SOA Suite 12c, BI Publisher 12c, OUD 12c, DB 19c und 12c Connectors
*   Zugriffsverwaltung:
    *   OAM 12c, OHS/WebGate 12c, OUD 12c und DB 19c
*   Tools und Ressourcen für die Entwicklung:
    *   [JDeveloper 12c mit SOA-Erweiterungen](http://www.oracle.com/technetwork/middleware/soasuite/downloads/index.html)
    *   [SQL Developer 19.2.1](https://www.oracle.com/database/technologies/appdev/sql-developer.html)
    *   [Apache Studio 2](https://directory.apache.org/studio/)
    *   [Oracle APEX 19,2](https://apex.oracle.com/en/)
    *   Beispiele für meine HR- und meine IGA-Anwendungen und Demo-Szenarios

**Hinweis:** OIM und OIG sind austauschbare Begriffe und beziehen sich auf dasselbe Produkt Oracle Identity Manager oder Oracle Identity Governance.

SecureOracle kann mit Oracle Identity Cloud Service (IDCS) verwendet werden, um eine hybride Identity Management-Umgebung zu präsentieren. Bei der Integration mit IDCS bietet die OIG-Komponente Provisioning- und Governance-Services, während IDCS Zugriffsmanagementservices für Cloud- und On-Premise-Anwendungen bereitstellt.

Die Oracle IAM Suite 12c R2 PS4 kann mit der Oracle IAM-Standardinstallationstopologie bereitgestellt werden, die flexibel ist und als Ausgangspunkt in Produktionsumgebungen verwendet werden kann. In _Abbildung 1_ wird eine WebLogic Server-Standarddomain dargestellt, die einen Administrationsserver und mindestens ein Cluster enthält, das mindestens einen Managed Server enthält.

![](./images/idm12cps4-standard-topology2.png " ")

_Abbildung 1_. Standardtopologie für Oracle Identity and Access Management

In _Abbildung 2_ werden die Domains dargestellt, aus denen die SecureOracle-Umgebung besteht. Die OIG- und OAM-Komponenten können einzeln oder alle zusammen gestartet werden. Bei weiterer Konfiguration können diese gemäß der offiziellen Dokumentation [Integration von OIG und OAM](https://docs.oracle.com/en/middleware/idm/suite/12.2.1.4/integrate.html) online integriert werden.

![](./images/img-sodomains.png " ")

_Abbildung 2_. SecureOracle - Demonstrationsplattform für IAM Suite 12c R2 PS4

Standardmäßig werden alle Nicht-SSL-Ports zur Ausführung der verschiedenen Übungen verwendet. Sie können jedoch SSL-Ports nach Bedarf aktivieren, um bestimmte Demoanforderungen zu erfüllen. Einzelheiten zum Aktivieren von SSL finden Sie in der offiziellen Produktdokumentation.

### Laborübersicht

*   **Labor: Erste Schritte mit SecureOracle 8.0** - In dieser Übung lernen wir, was in der Demonstrationsplattform SecureOracle 8.0 neu ist, und lernen, wie Sie die verschiedenen Komponenten starten, die Entwicklungstools ausführen und auf die verschiedenen Administrationskonsolen und Demoanwendungen zugreifen.
    
*   **Übung: Mitarbeiterlebenszyklusmanagement** - In dieser Übung werden mehrere Anwendungsfälle im Zusammenhang mit Mitarbeiteronboarding, Benutzerlebenszyklus, Versetzungen, Managergenehmigungen und Mitarbeiterkündigungen mit "Meine HR-Anwendung" als maßgebliche Quelle in Oracle Identity Manager ausgeführt.
    
*   **Labor: Identitätszertifizierung** - Bei der Identitätszertifizierung werden Benutzerberechtigungen und Zugriffsberechtigungen in einem Unternehmen geprüft, um sicherzustellen, dass Benutzer keine Berechtigungen erworben haben, für die sie nicht autorisiert sind. Außerdem muss jede Zugriffsberechtigung genehmigt (zertifiziert) oder abgelehnt (entzogen) werden. In dieser Übung prüfen wir die Benutzerzertifizierung mit benutzerdefinierten Prüfern.
    
*   **Übung: RESTful OIM- und OAM-APIs** - In dieser Übung werden verschiedene Anwendungsfälle im Zusammenhang mit dem Aufrufen von REST-APIs für OIM-Selfservice, Benutzerprofilaktualisierungen, Anforderungen, Genehmigungen und OAM OAuth Service geprüft.
    

## Was ist neu in Version 8.0

Version 8.0 von SecureOracle umfasst die folgenden Features:

*   Neuinstallation von Oracle IAM Suite 12c R2 PS4 (12.2.1.4.0)
    
*   Oracle Access Management umfasst die folgenden Services:
    
    *   Adaptiver Authentifizierungsservice
    *   Service OAuth und OpenIDConnect
    *   Föderation
    *   Access Portal Service
*   Standardmäßig ist die Kommunikation zwischen OAM und WebGate OAP über REST
    
*   [HTTPie](https://httpie.org/) Open Source-Befehlszeilen-HTTP-Client zur Interaktion mit RESTful-APIs, Webservices und HTTP-Servern.
    
*   Oracle Identity Governance umfasst die folgenden 12c-Connectors:
    
    *   Box, Concur, DB-Anwendungstabelle, Oracle/MySQL DB-Benutzerverwaltung, Flat File, Google Apps, GoToMeeting, IDCS, Microsoft AD-Benutzerverwaltung, Office365, EBS-Mitarbeiterabstimmung, EBS-Benutzerverwaltung, OID/ODSEE/OUD/LDAP, Salesforce, SAP-Erfolgsfaktor, SAP-Benutzerverwaltung, SAP-Benutzerverwaltungs-Engine, ServiceNow, Unix, WebEx
*   [Cockpit](https://cockpit-project.org/) ist ein Open-Source-Tool zur Verwaltung von Servern über eine benutzerfreundliche Webkonsole. Cockpit ist mit SecureOracle vorinstalliert und ermöglicht das Starten aller Komponenten, ohne einen SSH-Client verwenden zu müssen.
    
    ![](./images/img-cockpit.png " ")
    
    Abbildung 1. Cockpit-Webkonsole
    
*   Oracle Unified Directory umfasst die folgenden Services:
    
    *   REST-APIs für den Zugriff auf Identitätsinformationen und die Verwaltung von Verzeichnisdaten
*   Vorkonfigurierte Connectors
    
    *   12c IDCS-Connector für die Integration mit IDCS-Provisioning und -Governance
    *   12c ODSEE/OUD/LDAP-Connector für die Integration mit OUD
    *   12c DBAT-Connector für die Integration mit My HR Application
*   Aktualisierte SecureOracle-Dokumentation, einschließlich:
    
    *   Handbuch für erste Schritte
    *   SecureOracle in OCI bereitstellen
    *   SecureOracle wird in VMware-VirtualBox bereitgestellt
    *   Verwaltung des Mitarbeiterlebenszyklus
    *   RESTful OIM- und OAM-APIs
    *   OAM Hybrid-Szenario
    *   Identitätszertifizierungen
    *   Anleitung für Android Emulator NoxPlayer
*   Open-Source-E-Mail-Server und -Client zur Unterstützung von Demo-Anwendungsfällen
    
    *   Webmail-Client: [Roundcube 1.4.1](https://roundcube.net/ "Roundcube") mit Unterstützung für HTML-Inhalte
    *   E-Mail-Server: [Hedwig Mail Server](http://hwmail.sourceforge.net/ "Hedwig") mit einfacher Webkonsole zur Verwaltung von E-Mail-Konten
*   SecureOracle-Images sind für das Deployment verfügbar auf:
    
    *   OCI-Computing
    *   VirtualBox oder VMware
*   Aktualisierte Demoassets in APEX 19.2
    
    *   Meine HR-Anwendung - vollständig in APEX entwickelt und in der Oracle Database ausgeführt - demonstriert das Management des HR-Mitarbeiterlebenszyklus und die Integration mit OIG. Vollständige HTML5-Schnittstelle zur Unterstützung moderner Browser.
    *   Meine IGA-Anwendung - ebenfalls in APEX entwickelt, zielt darauf ab, Governance-Funktionen mit OIG-REST-APIs und Zertifizierungen wie benutzerdefinierten Prüfern zu präsentieren. Vollständige HTML5-Schnittstelle zur Unterstützung moderner Browser.
    
    ![](./images/img-myhr-app-menu.png " ")
    
    Abbildung 2. Meine HR-Anwendung
    
    ![](./images/img-iga-app-dash.png " ")
    
    Abbildung 3. Meine IGA-Anwendung
    
*   Unterstützung für hybride Anwendungsfälle bei der Integration mit IDCS, einschließlich:
    
    *   OIG 12c-Connector für Provisioning mit IDCS
    *   OAM Webgate im Cloud-Modus für Integration mit IDCS konfiguriert

## Weitere Informationen

Über diese Links erhalten Sie weitere Informationen zu Oracle Identity and Access Management:

*   [Oracle Identity Management-Website](https://docs.oracle.com/en/middleware/idm/suite/12.2.1.4/index.html)
*   [Oracle Identity Governance-Dokumentation](https://docs.oracle.com/en/middleware/idm/identity-governance/12.2.1.4/index.html)
*   [Oracle Access Management-Dokumentation](https://docs.oracle.com/en/middleware/idm/access-manager/12.2.1.4/books.html)

## Danksagungen

*   **Autor** - Ricardo Gutierrez, Solution Engineering - Sicherheit und Management
*   **Mitwirkende** - Meghana Banka, Rene Fontcha
*   **Zuletzt aktualisiert am/um** - Rene Fontcha, LiveLabs Platform Lead, NA Technology, November 2020