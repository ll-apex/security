# Einführung

## Über diesen Workshop

OCI IAM Identity Domains ist eine umfassende Identity-as-a-Service-(IDaaS-)Lösung, mit der eine Vielzahl von IAM-Anwendungsfällen und -Szenarien angegangen werden kann. OCI IAM kann verwendet werden, um den Zugriff für Benutzer über zahlreiche Cloud- und On-Premise-Anwendungen hinweg zu verwalten. Dies ermöglicht eine sichere Authentifizierung, eine einfache Verwaltung von Berechtigungen und ein nahtloses SSO für Ende users.In in diesem Workshop. Wir stellen die OAS-Anwendung, App-Gatewayserver und OCI-IAM-Identitätsdomains auf OCI mit Terraform. Konfigurieren Sie dann das bereitgestellte Setup mit Terraform, um das App-Gatewayfeature von Identitätsdomains zu nutzen und SSO für die OAS-Anwendung zu erreichen.

Das folgende Diagramm zeigt die Architektur und den Anmeldeablauf bei Verwendung des App-Gateways zur Integration von Oracle Analytics Server mit OCI IAM.

![oas-oci-iam-identity-domains](./images/oas-oci-iam-identity-domains.png "Bild 1")

\*In einem Webbrowser fordert ein Benutzer Zugriff auf eine Anwendung über eine vom App-Gateway angegebene URL an. \*Das App-Gateway fängt die Anforderung ab, prüft, ob der Benutzer über keine Session mit IAM verfügt, und leitet den Browser des Benutzers dann zur Anmeldeseite um. Wenn der Benutzer in Schritt 2 eine Session mit IAM hat, bedeutet dies, dass sich der Benutzer bereits angemeldet hat. In diesem Fall wird ein Zugriffstoken an das App-Gateway gesendet, und die restlichen Schritte werden übersprungen. \*IAM zeigt die Anmeldeseite an oder welcher Anmeldemechanismus für die Domain konfiguriert wurde. \*Der Benutzer meldet sich bei IAM an. \*Nach erfolgreicher Authentifizierung erstellt IAM eine Session für den Benutzer und gibt ein Zugriffstoken für das App-Gateway aus. \* Das App-Gateway verwendet das Token zur Identifizierung des Benutzers. Anschließend werden der Anforderung Headervariablen hinzugefügt und die Anforderung an die Anwendung weitergeleitet. \*Die Anwendung erhält die Headerinformationen, validiert die Identität des Benutzers und startet die Benutzersession.

In dieser Übung werden Sie durch die Schritte zu den ersten Schritten mit **OCI IAM-Identitätsdomains** mit einem beliebten Anwendungsfall geführt: **SSO mit App-Gateway erreichen**. In diesem Workshop werden die Schritte zum Deployment der **OAS**\-Anwendung auf OCI mit _Terraform_ über **Resource Manager** ausgeführt. Derselbe Stack stellt auch einen **App-Gatewayserver** und eine **OCI-IAM-Identitätsdomain** bereit. Nach dem Deployment nehmen wir auch einige erforderliche Konfigurationsänderungen in _App Gateway Server_, _OAS Application Server_ und der _Identitätsdomain_ mit Terraform vor. Wir führen einige _manuelle Aufgaben_ aus, um die Konfiguration in Identitätsdomains abzuschließen, bevor wir zur Validierung des gesamten Ablaufs übergehen. Nach der Validierung werden die Bereinigungsaktivitäten/-schritte mit dem _Resource Manager_ durchlaufen.

_Geschätzte Zeit:_ 1 Stunden

### Ziele

In diesem Workshop erfahren Sie, wie Sie:

*   Stellen Sie einen Terraform-Stack bereit, um OAS-Anwendung, App-Gatewayinstanz und OCI-IAM-Identitätsdomain auf OCI über Resource Manager zu erstellen.
*   Vertrauliche Anwendung in OCI-IAM-Identitätsdomain erstellen
*   Terraform-Stack zur Konfiguration der bereitgestellten App-Gatewayinstanz, OAS-Anwendung und OCI-IAM-Identitätsdomain bereitstellen
*   Setup validieren und SSO-Ablauf testen
*   Bereitgestellte Ressourcen löschen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Ein Pay Go-Mandant (nicht kostenlos), bei dem Sie administrativen Zugriff haben

## Weitere Informationen

*   [OCI-IAM-IDs](https://docs.oracle.com/en-us/iaas/Content/Identity/home.htm)
*   [Informationen zu App-Gateway](https://docs.oracle.com/en-us/iaas/Content/Identity/appgateways/understand-app-gateway.htm)

## Danksagungen

*   **Autor** - Chetan Soni, Sagar Takkar
*   **Lead By** - Deepthi Shetty
*   **Zuletzt aktualisiert am/um** - Chetan Soni August 2023