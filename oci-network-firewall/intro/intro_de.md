# Einführung in OCI Network Firewall

Oracle Cloud Infrastructure (OCI) bietet erstklassige Sicherheitstechnologie und betriebliche Prozesse zur Sicherung seiner Cloud-Services für Unternehmen. Die Sicherheit in der Cloud basiert jedoch auf einem Modell der gemeinsamen Verantwortung. Oracle ist für die Sicherheit der zugrunde liegenden Infrastruktur wie Rechenzentrumseinrichtungen, Hardware und Software zur Verwaltung von Cloud-Vorgängen und -Services verantwortlich. Kunden sind dafür verantwortlich, ihre Workloads zu sichern und ihre Services und Anwendungen sicher zu konfigurieren, um ihre Compliance-Verpflichtungen zu erfüllen. In diesem Workshop verwenden Sie die **OCI Network Firewall**\-Lösung, um Ihre Workloads zu sichern.

Dieser Workshop behandelt schrittweise (manuelle) und automatisierte Ansätze, die Sie befolgen können, um die erforderlichen Komponenten in Oracle Cloud Infrastructure mit der OCI Network Firewall-Lösung bereitzustellen.

Geschätzte Zeit: 120 Minuten

## Lösung

Oracle hat sich mit **Palo Alto Networks** zusammengetan, um Oracle Cloud Infrastructure Network Firewall bereitzustellen, einen verwalteten Netzwerkfirewallservice der nächsten Generation für Ihr OCI-VCN, der von Palo Alto Networks unterstützt wird. Der Oracle Cloud Infrastructure Network Firewall-Service bietet die folgenden **Sicherheitsfeatures**:

*   **Zustandsfähige Netzwerkfilterung**: Erstellen Sie Regeln zur zustandslosen Netzwerkfilterung, die Netzwerktraffic basierend auf Quell-IP (IPv4 und IPv6), Ziel-IP (IPv4 und IPv6), Port und Protokoll zulassen oder ablehnen.
*   **Benutzerdefinierte URL- und FQDN-Filterung**: Begrenzen Sie Ingress- und Egress-Traffic auf eine angegebene Liste von vollqualifizierten Domainnamen (FQDNs), einschließlich Platzhaltern und benutzerdefinierten URLs.
*   **Intrusion Detection and Prevention (IDPS)**: Überwachen Sie Ihr Netzwerk auf böswillige Aktivitäten. Protokollieren, melden oder blockieren Sie die Aktivität.
*   **SSL-Prüfung**: Entschlüsseln und prüfen Sie TLS-verschlüsselten Traffic mit ESNI-(Encrypted Server Name Indication-)Unterstützung auf Sicherheitslücken. ESNI ist eine TLSv1.3-Erweiterung, die den Server Name Indication (SNI) im TLS-Handshake verschlüsselt.
*   **Logging**: Netzwerkfirewall ist in Oracle Cloud Infrastructure Logging integriert. Aktivieren Sie Logs basierend auf den Policy-Regeln Ihrer Firewall.
*   **Metriken**: Netzwerkfirewall ist in Oracle Cloud Infrastructure Monitoring integriert. Aktivieren Sie Alerts basierend auf Metriken wie der Anzahl blockierter Anforderungen mit Monitoring-Servicefunktionen.
*   **Prüfung des Intra-VCN-Subnetztraffics**: Leiten Sie den Traffic über eine Netzwerkfirewall zwischen zwei Subnetzen innerhalb eines VCN weiter.
*   **Prüfung des Inter-VCN-Traffics**: Leiten Sie den Traffic über eine Netzwerkfirewall zwischen VCNs weiter.

Sie können **Netzwerkfirewall** in zwei verschiedenen Architekturen bereitstellen:

*   **Verteilte Netzwerkfirewall**: Netzwerkfirewall wird in ihrem dedizierten VCN bereitgestellt. Dies wird empfohlen, damit Sie eine Firewall in jedem VCN bereitstellen und Ihre Workloads sichern können.
*   **Netzwerkfirewall übertragen**: Netzwerkfirewall wird in einem Firewall-VCN bereitgestellt und über ein dynamisches Routinggateway mit Spoke-VCNs verbunden.

In diesem Workshop werden verschiedene Sicherheitsschlüsselfeatures behandelt, darunter Sicherheitsregeln, Eindringschutz, Eindringerkennung, URL-Filterung und mehr, in denen Sie die Lösung bereitstellen und Ihre OCI-Workloads sichern können.

Im Folgenden finden Sie eine Beispielarchitektur der Lösung basierend auf **Transitnetzwerkfirewall**, die Ihnen die Vertrautheit mit dem **Hub- und Spoke**\-Modell und dem Traffic vermittelt, der über die Netzwerkfirewall in **firewall-vcn** geleitet wird:

![OCI Network Firewall-Workshop - Topologiearchitektur](../common/images/arch.png " ")

### Ziele

*   Provisioning der Infrastruktur mit Oracle Resource Manager, d.h. Terraform
*   Bereitstellung und Konfiguration der Infrastruktur manuell
*   Deployment von OCI Network Firewall kennenlernen
*   Konfigurieren Sie die OCI Network Firewall zur Unterstützung verschiedener Sicherheitsfunktionen.
*   Traffic über OCI Network Firewall validieren und prüfen
*   Löschen Sie die Infrastruktur mit Oracle Resource Manager oder manuell.

### Voraussetzungen

*   Zugangsdaten für kostenpflichtigen Oracle Cloud Infrastructure-Account (Benutzer, Kennwort, Mandant und Compartment)
*   Der Benutzer muss über die erforderlichen Berechtigungen und Quota verfügen, um Ressourcen bereitzustellen.

> **Lesen**: Sie benötigen einen gültigen kostenpflichtigen **Oracle Cloud Infrastructure-Account**, mit dem Sie **OCI Network Firewall** und zugehörige Komponenten bereitstellen können. **Hinweis**: Der Abschnitt **Erste Schritte** enthält Anweisungen zum Erstellen eines neuen kostenlosen Testkontos. Konvertieren Sie dieses Konto wie gezahlt, damit Sie Ressourcen bereitstellen können.

### Einführung

Sie können jetzt **mit der nächsten Übung fortfahren**.

### Weitere Informationen

1.  [OCI-Schulungen](https://www.oracle.com/cloud/iaas/training/)
2.  [Vertrautheit mit der OCI-Konsole](https://docs.us-phoenix-1.oraclecloud.com/Content/GSG/Concepts/console.htm)
3.  [Networking - Überblick](https://docs.us-phoenix-1.oraclecloud.com/Content/Network/Concepts/overview.htm)
4.  [Überblick über OCI Network Firewall](https://docs.oracle.com/en-us/iaas/Content/network-firewall/overview.htm)
5.  [OCI Network Firewall Cloud - Seite "Sicherheit"](https://www.oracle.com/security/cloud-security/network-firewall/)
6.  [OCI-Intra-VCN-Routingfunktionen](https://docs.oracle.com/en-us/iaas/Content/Network/Tasks/managingroutetables.htm)

## Danksagungen

*   **Autor** - Arun Poonia, Principal Solutions Architect
*   **Angepasst von** - Oracle
*   **Mitwirkende** - nicht zutreffend
*   **Zuletzt aktualisiert am/um** - Arun Poonia, Aug 2023