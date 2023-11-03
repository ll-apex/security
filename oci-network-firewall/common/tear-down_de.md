# Laborumgebung zerstören

## Einführung

In dieser Übung zerstören Sie die Übungsumgebung, die Sie manuell oder mit Oracle Resource Manager erstellt haben. Manuelle Schritte umfassen das Löschen virtueller Cloud-Netzwerke (VCNs), Subnetze in jedem VCN, dynamische Routinggateways (DRG), Routentabellen, Compute-Instanzen und Netzwerkfirewall.

> **Hinweis**: Wählen Sie das richtige Compartment aus, in dem Sie Ihre Ressourcen erstellt haben.

Geschätzte Zeit: 10 Minuten.

### Ziele

*   Löschen der Umgebung manuell
*   Umgebung mit Oracle Resource Manager zerstören

### Voraussetzungen

*   Zugangsdaten für kostenpflichtigen Oracle Cloud Infrastructure-Account (Benutzer, Kennwort, Mandant und Compartment)
*   Der Benutzer muss über die erforderlichen Berechtigungen und Quota verfügen, um Ressourcen bereitzustellen.

## Aufgabe 1: Umgebung manuell löschen

Stellen Sie beim manuellen Löschen der Umgebung sicher, dass eine Ressource nicht an eine andere Ressource gebunden ist.

1.  Klicken Sie im Menü "OCI-Services" unter **Compute** auf **Instanzen**, und löschen Sie Instanzen, die Sie in Ihrer Übungsumgebung erstellt haben.
    
2.  Klicken Sie im Menü "OCI-Services" unter **Identität und Sicherheit** auf **Netzwerkfirewall und -Policy**. Löschen Sie die zuvor erstellte Netzwerkfirewall-Policy und Netzwerkfirewall zur Validierung von Anwendungsfällen.
    
3.  Klicken Sie im Menü "OCI-Services" unter **Networking > Kundenkonnektivität** auf **Dynamisches Routinggateway**, und löschen Sie das in Ihrer Übungsumgebung erstellte **firewall-drg**\-DRG.
    
4.  Klicken Sie im Menü "OCI-Services" unter **Networking** auf **Virtuelle Cloud-Netzwerke**, und löschen Sie Routentabelleneinträge, Servicegateway, Routentabellen, Subnetze und VCN aus jedem **firewall-vcn, Spoke-vcn**, das Sie in Ihrer Übungsumgebung erstellt haben.
    
5.  Klicken Sie im Menü "OCI-Services" unter **Speicher** auf **Buckets**. Löschen Sie das zuvor erstellte Objekt und den Bucket, um den Object Storage-Netzwerktraffic zu validieren.
    

## Aufgabe 2: Umgebung mit Oracle Resource Manager löschen

Wenn Sie Resource Manager zum Löschen der Umgebung verwenden, müssen Sie ein **terraform destroy** ausführen und anwenden. Tun wir das jetzt.

1.  Öffnen Sie das Hamburger-Menü in der linken Ecke. Wählen Sie **Entwicklerservices > Stacks**. Klicken Sie auf **Stacks**, und navigieren Sie zu dem Stack, den Sie in **Lab0** erstellt haben.
    
2.  Klicken Sie oben auf Ihrer Seite auf Stack Details. Klicken Sie auf die Schaltfläche **Zerstören**. Dadurch werden Ihre Instanzen und die erforderliche Konfiguration gelöscht.
    
    ![Umgebung mit Terraform zerstören](./images/terraform-destroy.png " ")
    
3.  Sobald dieser Job erfolgreich ist, wird Ihre Umgebung zerstört! Zeit für eine Tasse Kaffee :)
    
    ![Fenster "Terraform erfolgreich zerstört"](./images/terraform-destroy-success.png " ")
    

_**Glückwunsch! Sie haben die Übungen abgeschlossen.**_

## Weitere Informationen

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