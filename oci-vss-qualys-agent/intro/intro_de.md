# Einführung in OCI Vulnerability Scanning Service

Mit dem Oracle Cloud Infrastructure (OCI) Vulnerability Scanning Service (VSS) können Sie Ihren Sicherheitsstatus verbessern, indem Sie Hosts und Containerimages regelmäßig auf potenzielle Sicherheitslücken prüfen. Der Service bietet Entwicklern, Vorgängen und Sicherheitsadministratoren einen umfassenden Einblick in falsch konfigurierte oder anfällige Ressourcen und generiert Berichte mit Metriken und Details zu diesen Sicherheitslücken, einschließlich Korrekturinformationen. In diesem Workshop scannen Sie Ihre Workloads mit **OCI VSS Service**.

Dieser Workshop behandelt schrittweise (manuelle) und automatisierte Ansätze, die Sie befolgen können, um die erforderlichen Komponenten auf Oracle Cloud Infrastructure mit der **OCI Vulnerability Solution-Integration mit Qualys VMDR** bereitzustellen.

Geschätzte Zeit: 45 Minuten

## Lösung

Oracle hat sich mit **Qualys** zusammengetan, um Kunden eine VSS-Integrationslösung bereitzustellen, mit der sie ihre Vulnerability Management-, Detection- und Response-(**VMDR**\-)Lizenz nutzen können. Mit dieser Lösung können Sie Ihre auf OCI ausgeführten Workloads scannen. Dies bietet die folgenden **wichtigen Vorteile**:

*   **Einfach** - Ändern Sie Ihr VSS-Hostscanrezept, um die Qualys-Agents zu verwenden
*   **Verwaltet** - Wissen Sie, dass OCI diese Agents auf Ihren Compute-Instanzen installiert und aktualisiert.
*   **Sicherheitslücken**: Qualys VMDR gleicht die OCI-Compute-Instanzinformationen mit QIDs (CVEs) ab.
*   **Logging/Reporting**: Zeigen Sie Multi-Cloud-Ergebnisse im Qualys-Dashboard an.
*   **Suchen**: OCI-Ergebnisse in VSS und Cloud Guard anzeigen

Im Folgenden finden Sie eine Beispielarchitektur der Lösung:

![OCI Network Firewall-Workshop - Topologiearchitektur](../common/images/arch.png " ")

Diese Architektur zeigt zwei Compute-Instanzen, die in einem regionalen Compute-Subnetz in einem VCN ausgeführt werden. Mit dem OCI VSS-Service können Sie diese Compute-Ressourcen scannen. Sie können Ressourcen auch mit dem OCI-Service CloudGuard überwachen.

### Ziele

*   Provisioning der Infrastruktur mit Oracle Resource Manager, d.h. Terraform
*   Bereitstellung und Konfiguration der Infrastruktur manuell
*   Erfahren Sie, wie Sie die OCI Vulnerability Scanning Service-Integration mit Qualys bereitstellen
*   Scanning- und Sicherheitslückenberichte in der OCI-Konsole validieren und prüfen
*   Löschen Sie die Infrastruktur mit Oracle Resource Manager oder manuell.

### Voraussetzungen

*   Zugangsdaten für kostenpflichtigen Oracle Cloud Infrastructure-Account (Benutzer, Kennwort, Mandant und Compartment)
*   Der Benutzer muss über die erforderlichen Berechtigungen und Quota verfügen, um Ressourcen bereitzustellen.
*   Der Benutzer muss über einen gültigen Qualys VMDR Cloud Agent-Lizenzcode verfügen.

> **Bitte lesen**: Sie können sich für [**Qualys VMDR**](https://www.qualys.com/apps/vulnerability-management-detection-response/) registrieren und einen Lizenzcode mit der Option "Cloud-Installations-Agent" generieren.

![Qualys VMDR Lizenzcode erstellen](../common/images/qualys-vmdr-cloud-agent-license-code-key.png " ")

### Einführung

Sie können jetzt **mit der nächsten Übung fortfahren**.

### Weitere Informationen

1.  [OCI-Schulungen](https://www.oracle.com/cloud/iaas/training/)
2.  [Vertrautheit mit der OCI-Konsole](https://docs.us-phoenix-1.oraclecloud.com/Content/GSG/Concepts/console.htm)
3.  [Überblick über OCI Vulnerability Scanning Service](https://docs.oracle.com/en-us/iaas/scanning/home.htm)
4.  [OCI Vulnerability Scanning-Serviceseite](https://www.oracle.com/security/cloud-security/cloud-guard/)
5.  [Funktionen von OCI CloudGuard](https://www.oracle.com/security/cloud-security/cloud-guard/)

## Danksagungen

*   **Autor** - Arun Poonia, Principal Solutions Architect
*   **Angepasst von** - Oracle
*   **Mitwirkende** - nicht zutreffend
*   **Zuletzt aktualisiert am/um** - Arun Poonia, Aug 2023