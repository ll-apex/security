# Einführung

## Über diesen Workshop

OCI IAM Identity Domains ist eine umfassende Identity-as-a-Service-(IDaaS-)Lösung, mit der eine Vielzahl von IAM-Anwendungsfällen und -Szenarien angegangen werden kann. Mit OCI IAM kann der Zugriff für Benutzer über zahlreiche Cloud- und On-Premise-Anwendungen hinweg verwaltet werden, was eine sichere Authentifizierung, eine einfache Verwaltung von Berechtigungen und ein nahtloses SSO für Endbenutzer ermöglicht. In diesem Workshop zeigen wir, wie Sie mit dem integrierbaren OCI IAM Linux-Authentifizierungsmodul (PAM) Ihre Linux-Umgebung mit OCI IAM-Identitätsdomains integrieren können, um die Endbenutzerauthentifizierung mit der Erst- und Zweitfaktorauthentifizierung durchzuführen.

Die folgende Grafik zeigt die allgemeine Architektur des Linux PAM-Setups mit OCI IAM.

![Architektur](./images/architecture-diagram.png "Architektur")

In dieser Übung werden die Schritte für die ersten Schritte mit **OCI IAM-Identitätsdomains** mit dem Anwendungsfall **Authentifizierung bei Linux-Umgebung mit dem Linux Pluggable Authentication Module-(PAM-)Modul von OCI IAM** beschrieben. In diesem Workshop werden die Schritte zum Deployment eines **Linux-Servers** auf OCI mit _Terraform_ über **Resource Manager** ausgeführt. Derselbe Stack stellt auch eine **OCI-IAM-Identitätsdomain** bereit. Nach dem Deployment nehmen wir auch einige erforderliche Konfigurationsänderungen im _Linux-Server_ und in der _Identitätsdomain_ mit Terraform vor. Wir führen einige _manuelle Aufgaben_ aus, um die Konfigurationen in Identitätsdomains abzuschließen, bevor wir zur Validierung des gesamten Ablaufs übergehen. Nach der Validierung durchlaufen wir die Bereinigungsaktivitäten/-schritte mit dem _Resource Manager_.

_Geschätzte Workshopzeit:_ 1 Stunden

### Ziele

In diesem Workshop erfahren Sie, wie Sie:

*   Stellen Sie einen Terraform-Stack bereit, um über Resource Manager einen Linux-Server und eine OCI-IAM-Identitätsdomain auf OCI zu erstellen.
*   Erstellen Sie eine vertrauliche Anwendung in der OCI-IAM-Identitätsdomain.
*   Stellen Sie einen Terraform-Stack bereit, um die Linux-Instanz und die OCI-IAM-Identitätsdomain zu konfigurieren.
*   Testen Sie den Authentifizierungsablauf _mit MFA_.
*   Bereinigen Sie die bereitgestellten Ressourcen.

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Ein Pay Go-Mandant (nicht kostenlos), bei dem Sie administrativen Zugriff haben

## Weitere Informationen

*   [OCI-IAM-IDs](https://docs.oracle.com/en-us/iaas/Content/Identity/home.htm)
*   [OCI IAM Linux PAM-Modul](https://docs.oracle.com/en/cloud/paas/identity-cloud/uaids/manage-linux-authentication-using-linux-pam-module.html#GUID-8FE587F4-D44C-47C1-BBE2-3D32886D0553)

## Danksagungen

*   **Autor** - Gautam Mishra, Aqib Bhat
*   **Mitwirkender** - Deepthi Shetty
*   **Zuletzt aktualisiert am/um** - Gautam Mishra Juli 2023