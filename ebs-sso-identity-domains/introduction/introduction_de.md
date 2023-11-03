# Einführung

## Über diesen Workshop

OCI IAM Identity Domains ist eine umfassende Identity-as-a-Service-(IDaaS-)Lösung, mit der eine Vielzahl von IAM-Anwendungsfällen und -Szenarien angegangen werden kann. Mit OCI IAM kann der Zugriff für Benutzer über zahlreiche Cloud- und On-Premise-Anwendungen hinweg verwaltet werden, was eine sichere Authentifizierung, eine einfache Verwaltung von Berechtigungen und ein nahtloses SSO für Endbenutzer ermöglicht. In diesem Workshop stellen wir die EBS-Anwendung, den EBS Asserter-Server und die OCI-IAM-Identitätsdomains auf OCI mit Terraform bereit und konfigurieren dann das bereitgestellte Setup mit Terraform, um das EBS Asserter-Feature von Identitätsdomains zu nutzen, um SSO für die EBS-Anwendung zu erreichen.

Die folgende Grafik zeigt die allgemeine Architektur des E-Business Suite Asserter-Setups mit OCI IAM.

![ebs-oci-iam-identity-domains](./images/ebs-oci-iam-identity-domains.png "Bild 1")

Das folgende Diagramm zeigt den Anmeldeablauf bei Verwendung von E-Business Suite Asserter zur Integration von Oracle E-Business Suite mit OCI IAM.

![Workflow](./images/workflow.png)

*   Der Benutzer fordert Zugriff auf eine geschützte Oracle E-Business Suite-Ressource an.
*   Oracle E-Business Suite leitet den Benutzerbrowser zur E-Business Suite Asserter um.
*   E-Business Suite Asserter generiert mit einem OCI-IAM-SDK die Autorisierungs-URL und leitet den Browser dann zu OCI IAM um.
*   OCI IAM zeigt dem Benutzer seine Anmeldeseite an.
*   Der Benutzer sendet Zugangsdaten an OCI IAM.
*   OCI IAM gibt einen Autorisierungscode aus und leitet den Browser des Benutzers zu E-Business Suite Asserter um.
*   E-Business Suite Asserter kommuniziert mithilfe eines OCI-IAM-SDK mit OCI IAM, um den Autorisierungscode gegen ein Zugriffstoken auszutauschen.
*   OCI IAM gibt ein Zugriffstoken und ein ID-Token an E-Business Suite Asserter aus.
*   E-Business Suite Asserter erstellt ein Oracle E-Business Suite-Cookie und leitet den Browser des Benutzers zu Oracle E-Business Suite um.
*   Oracle E-Business Suite zeigt die vom Benutzer angeforderte geschützte Ressource an.

In dieser Übung werden Sie durch die Schritte zu den ersten Schritten mit **OCI IAM-Identitätsdomains** mit einem beliebten Anwendungsfall geführt: **EBS-SSO mit EBS Asserter erreichen**. In diesem Workshop werden die Schritte zum Deployment der **EBS**\-Anwendung auf OCI mit _Terraform_ über **Resource Manager** ausgeführt. Derselbe Stack stellt auch einen **EBS Asserter Server** und eine **OCI IAM-Identitätsdomain** bereit. Nach dem Deployment werden auch einige erforderliche Konfigurationsänderungen im _EBS Asserter-Server_ und in der _Identitätsdomain_ mit Terraform vorgenommen. Wir führen einige _manuelle Aufgaben_ aus, um die Konfiguration in der EBS-Anwendung sowie in Identitätsdomains abzuschließen, bevor wir zur Validierung des gesamten Ablaufs übergehen. Nach der Validierung durchlaufen wir die Bereinigungsaktivitäten/-schritte mit dem _Resource Manager_.

_Geschätzte Zeit:_ 1 Stunden

### Ziele

In diesem Workshop erfahren Sie, wie Sie:

*   Stellen Sie einen Terraform-Stack bereit, um über Resource Manager eine EBS-Anwendung, eine EBS Asserter-Instanz und eine OCI-IAM-Identitätsdomain auf OCI zu erstellen.
*   Vertrauliche Anwendung in OCI-IAM-Identitätsdomain erstellen
*   Terraform-Stack zur Konfiguration der bereitgestellten EBS Asserter-Instanz und OCI-IAM-Identitätsdomain bereitstellen
*   Siteprofile in EBS-Anwendung manuell aktualisieren, um SSO zu aktivieren
*   Setup validieren und SSO-Ablauf testen
*   Bereitgestellte Ressourcen löschen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Ein Pay Go-Mandant (nicht kostenlos), bei dem Sie administrativen Zugriff haben

## Weitere Informationen

*   [OCI-IAM-IDs](https://docs.oracle.com/en-us/iaas/Content/Identity/home.htm)
*   [E-Business Suite Asserter](https://docs.oracle.com/en/solutions/sso-oci-iam-ebs-asserter/index.html#GUID-9E74257D-396D-49E5-A2FD-6E59ACC48946)

## Danksagungen

*   **Autor** - Gautam Mishra, Aqib Bhat, Samratha S P
*   **Mitwirkender** - Chetan Soni, Sagar Takkar
*   **Unterstützt von** - Deepak Rao Narasimha Gajendragad
*   **Lead By** - Deepthi Shetty
*   **Zuletzt aktualisiert am/um** - Gautam Mishra Mai 2023