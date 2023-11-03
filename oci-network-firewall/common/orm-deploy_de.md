# Übung mit Oracle Resource Manager bereitstellen

## Einführung

In dieser Übung stellen Sie mit Oracle Resource Manager erforderliche virtuelle Cloud-Netzwerke (VCNs), Subnetze in jedem VCN, dynamische Routinggateways (DRG), Routentabellen, Compute-Instanzen und OCI Network Firewall bereit, um den Traffic zwischen VCNs zu unterstützen.

> **Lesen**: Wenn Sie die Konfiguration manuell bereitstellen möchten, überspringen Sie **Lab0**, und fahren Sie mit **Lab1** fort.

Geschätzte Zeit: 45 Minuten.

### Ziele

*   Stack mit Oracle Resource Manager erstellen
*   Terraform-Plan validieren und anwenden
*   Verbindung mit Instanzen herstellen

### Voraussetzungen

*   Zugangsdaten für kostenpflichtigen Oracle Cloud Infrastructure-Account (Benutzer, Kennwort, Mandant und Compartment)
*   Der Benutzer muss über die erforderlichen Berechtigungen und Quota verfügen, um Ressourcen bereitzustellen.

## Aufgabe 1: Stack mit Resource Manager anmelden und erstellen

Sie erstellen Ihre Übungsumgebung mit Terraform.

1.  Klicken Sie auf den Link unten, um die ZIP-Datei herunterzuladen, die Sie zum Erstellen Ihrer Umgebung benötigen.
    
    *   Klicken Sie hier: [oci-network-firewall-live-labs.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/p/o_QTW8EWdtK1mMu-1WIjuKQ-fSz-ofIyGONl0S1oarXlN_Ohbkah99t3Byu7-uy8/n/partners/b/files/o/oci-network-firewall.zip)
        *   Anwendungsfall der verpackten Terraform-**OCI Network Firewall**.
        *   **PAR URL** ist bis zum **Dezember 2025** gültig.
    
    > **Lesen**: Sie können diesen ZIP-Ordner auch lokal herunterladen und die erforderlichen Ressourcen aktualisieren, um zusätzliche Anwendungsfälle zu unterstützen.
    
2.  Speichern Sie im Downloadordner Ihres lokalen Rechners.
    
3.  Öffnen Sie das Hamburger-Menü in der linken Ecke. Wählen Sie **Entwicklerservices > Stacks**. Klicken Sie auf **Stacks**:
    
    ![Homepage von Oracle Resource Manager](./images/orm-home-page.png " ")
    
4.  Wählen Sie in der Dropdown-Liste links das rechte Compartment und in der Dropdown-Liste oben rechts die entsprechende Region aus, und klicken Sie auf die Schaltfläche **Stack erstellen**.
    
    ![Oracle Resource Manager - Seite "Stack erstellen"](./images/create-stack-page.png " ")
    
5.  Wählen Sie **Meine Konfiguration** und dann **. Schaltfläche ZIP-Datei**, klicken Sie auf den Link **Durchsuchen**, und wählen Sie die heruntergeladene ZIP-Datei (oci-network-firewall-live-labs.ZIP) aus. Klicken Sie auf **Auswählen**.
    
    ![Oracle Resource Manager - Stackworkflow mit ZIP-Dateiupload erstellen](./images/myconfiguration-upload-zip-initial-configuration.png " ")
    
    Geben Sie die folgenden Informationen ein, und übernehmen Sie alle Standardwerte
    
    *   **Name**: Geben Sie einen benutzerfreundlichen Namen für den **Stack ein**
        
    *   **Compartment**: Wählen Sie das Compartment aus, in dem Sie den Stack erstellen möchten.
        
    *   **Terraform-Version**: Validierte Version für diesen Stack ist **1.0.x**
        
6.  Klicken Sie auf **Weiter**.
    
    ![Oracle Resource Manager - Stackworkflow mit Variablen erstellen](./images/myconfiguration-upload-zip-initial-configuration-step2.png " ")
    
    Geben Sie die folgenden Mindestinformationen ein bzw. wählen Sie sie aus. Einige Informationen sind möglicherweise bereits vorab ausgefüllt. Ändern Sie die vorab ausgefüllten Informationen nicht.
    
    **Compute Compartment**: Wählen Sie in der Dropdown-Liste die Option "Compute Compartment" aus, in der Sie Compute-Instanzen erstellen möchten.
    
    **Availability-Domain:** Wählen Sie die entsprechende AD in der Dropdown-Liste aus.
    
    **SSH-Public Key**: Fügen Sie die Public-Key-Zeichenfolge ein, mit der Sie VMs über Ihren Private Key verbinden möchten.
    
    **Netzwerk-Compartment**: Wählen Sie in der Dropdown-Liste das Netzwerk-Compartment aus, in dem Sie Netzwerkkomponenten erstellen möchten, z.B. VCN, Subnetze, Routentabellen, DRG usw.
    
    > **Hinweis:** Behalten Sie als Netzwerkstrategie den Standardwert **Neues VCN und Subnetz erstellen** bei. Wenn Sie den Code ändern möchten, können Sie vorhandene VCN-/Subnetzwerte unterstützen.
    
7.  Klicken Sie auf **Erstellen**, um Ihren Stack zu erstellen. Jetzt können Sie mit den nächsten Schritten fortfahren, um Ihre Umgebung zu erstellen.
    
    ![Oracle Resource Manager Create Stack Workflow mit Variablen prüfen](./images/myconfiguration-upload-zip-initial-configuration-step3.png " ")
    

## Aufgabe 2: Terraform planen und anwenden

Wenn Sie Resource Manager zum Bereitstellen einer Umgebung verwenden, müssen Sie einen terraform-**Plan** und eine **Anwendung** ausführen. Tun wir das jetzt.

1.  \[OPTIONAL\] Klicken Sie auf **Planen**, um die Konfiguration zu validieren. Das dauert etwa eine Minute, bitte haben Sie Geduld.
    
    ![Terraform-Planoption](./images/terraform-plan.png " ")
    
2.  Klicken Sie oben auf Ihrer Seite auf **Stackdetails**. Klicken Sie auf die Schaltfläche **Anwenden**. Dadurch werden Ihre Instanzen und die erforderliche Konfiguration erstellt.
    
    ![Terraform-Apply-Option](./images/terraform-apply.png " ")
    
3.  Sobald dieser Job erfolgreich ist, wird Ihre Umgebung erstellt! Zeit für die Anmeldung bei der Instanz, um die Konfiguration abzuschließen.
    
    ![Terraform - Erfolgreiche Ausgabe anwenden](./images/terraform-apply-success.png " ")
    
    > **Hinweis**: Das Deployment der **Netzwerkfirewall** dauert fast **30-35 Minuten**, und die Anwendung von terraform ist danach erfolgreich.
    
    > **Hinweis**: Stack stellt **Netzwerkfirewall** und die erforderlichen VMs bereit, um diesen Anwendungsfall zu unterstützen.
    

## Aufgabe 3: Verbindung zu Ihren Instanzen herstellen

1.  Wählen Sie je nach Laptopkonfiguration die entsprechenden Schritte aus, um eine Verbindung zu Ihren Instanzen herzustellen.
    
    ![Instanz mit Terraform erstellt](./images/final-instances.png " ")
    
2.  Die **Netzwerkfirewall** und die **Netzwerkfirewall-Policy** sollten erfolgreich erstellt werden.
    
    ![Netzwerkfirewall mit Terraform erstellt](./images/network-firewall.png " ")
    

> **Hinweis**: Es dauert einige Minuten, bis Sie eine Verbindung zum SSH-Daemon herstellen können. Wenn Sie keine Verbindung herstellen können, stellen Sie sicher, dass ein gültiger Schlüssel vorhanden ist. Warten Sie einige Minuten, und versuchen Sie es erneut.

_**Glückwunsch! Sie haben die Übung abgeschlossen.**_

> **Lesen**: Sie müssen **Übung 1 zu Lab2** jetzt überspringen und mit **Übung 3** fortfahren, d.h. **OCI-Netzwerkfirewall-Policy konfigurieren**.

Sie können jetzt **mit der nächsten Übung fortfahren**.

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