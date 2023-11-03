# Laborumgebung zerstören

## Einführung

In dieser Übung zerstören Sie die Übungsumgebung, die Sie manuell oder mit Oracle Resource Manager erstellt haben. Manuelle Schritte umfassen das Löschen virtueller Cloud-Netzwerke (VCNs), Subnetze in jedem VCN, Routentabellen, Compute-Instanzen, Scanrezepte, Ziele, Secret und Vault.

> **Hinweis:** Wählen Sie das richtige Compartment aus, in dem Sie Ihre Ressourcen erstellt haben.

Geschätzte Zeit: 10 Minuten.

### Ziele

*   Löschen der Umgebung manuell
*   Umgebung mit Oracle Resource Manager zerstören

### Voraussetzungen

*   Oracle Cloud Infrastructure-Accountzugangsdaten (Benutzer, Kennwort, Mandant und Compartment)
*   Der Benutzer muss über die erforderlichen Berechtigungen und Quota verfügen, um Ressourcen bereitzustellen.

## Aufgabe 1: Umgebung manuell löschen

Stellen Sie beim manuellen Löschen der Umgebung sicher, dass eine Ressource nicht an eine andere Ressource gebunden ist.

1.  Klicken Sie im Menü "OCI-Services" unter **Compute** auf **Instanzen**, und löschen Sie Instanzen, die Sie in Ihrer Übungsumgebung erstellt haben.
    
2.  Klicken Sie im Menü "OCI-Services" unter **Identität und Sicherheit** auf **Scannen**. Löschen Sie das Scanning-Rezept und die zuvor erstellten Ziele, um Anwendungsfälle zu validieren.
    
3.  Klicken Sie im Menü "OCI-Services" unter **Identität und Sicherheit** auf **Vault**. Löschen Sie das Scanning-Rezept und die zuvor erstellten Ziele, um Anwendungsfälle zu validieren.
    
    > **Hinweis:** Das Löschen des Vaults kann über die geplante Zeit erfolgen. Sie müssen diese Zeit in Betracht ziehen, um sicherzustellen, dass die Umgebung erfolgreich gelöscht wird.
    
4.  Klicken Sie im Menü "OCI-Services" unter **Networking** auf **Virtuelle Cloud-Netzwerke**, und löschen Sie Routentabelleneinträge, Servicegateway, Routentabellen, Subnetze und VCN aus jedem **VSS-VCN**, das Sie in Ihrer Übungsumgebung erstellt haben.
    

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
3.  [Überblick über OCI Vulnerability Scanning Service](https://docs.oracle.com/en-us/iaas/scanning/home.htm)
4.  [OCI Vulnerability Scanning-Serviceseite](https://www.oracle.com/security/cloud-security/cloud-guard/)
5.  [Funktionen von OCI CloudGuard](https://www.oracle.com/security/cloud-security/cloud-guard/)

## Danksagungen

*   **Autor** - Arun Poonia, Principal Solutions Architect
*   **Angepasst von** - Oracle
*   **Mitwirkende** - nicht zutreffend
*   **Zuletzt aktualisiert am/um** - Arun Poonia, Aug 2023