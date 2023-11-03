# Erforderliche Oracle Cloud Infrastructure-Komponenten konfigurieren

## Einführung

In dieser Übung erstellen Sie das erforderliche virtuelle Cloud-Netzwerk (VCN) mit Subnetzen, Internetgateway, NAT-Gateway, Servicegateways und Compute-Instanzen, die Sie mit **OCI VSS** scannen.

Geschätzte Zeit: 10 Minuten.

### Ziele

*   Starten von VSS-VCN und unterstützender Konfiguration demonstrieren
*   Starten Sie Compute-Instanzen im VSS-VCN.

### Voraussetzungen

*   Oracle Cloud Infrastructure-Accountzugangsdaten (Benutzer, Kennwort, Mandant und Compartment)
*   Der Benutzer muss über die erforderlichen Berechtigungen und Quota verfügen, um Ressourcen bereitzustellen.

## Aufgabe 1: VSS-VCN konfigurieren

1.  Klicken Sie im Menü "OCI-Services" unter **Networking** auf **Virtuelle Cloud-Netzwerke**. Wählen Sie Ihre Region rechts auf dem Bildschirm aus:
    
    ![Navigationsfenster für virtuelle Cloud-Netzwerke](../common/images/vcn-home.png " ")
    
2.  Die folgende Tabelle stellt dar, was Sie erstellen. Klicken Sie auf das Symbol **VCN-Assistenten starten**, um ein neues **virtuelles Cloud-Netzwerk** zu erstellen:
    
    | Ressource | Name | CIDR | Kommentar |
    | --- | --- | --- | --- |
    | Virtual Cloud Networks | vss-vcn | 10.10.0.0/16 | Virtuelles Cloud-Netzwerk zum Hosten von OCI-Workloads |
    | Öffentliches Subnetz | Öffentliches Subnetz-vss-vcn | 10.10.0.0/24 | \[Optional\] Sie können es in Compute-Subnetz umbenennen |
    | Privates Subnetz | Privates Subnetz-vss-vcn | 10.10.1.0/24 | Privates Subnetz zum Hosten von OCI-Workloads |
    
    ![Schaltfläche "VM-Netzwerk erstellen"](../common/images/vcn-create.png " ")
    
3.  Klicken Sie auf die Option **VCN mit Internetverbindung erstellen**:
    
    ![Virtuelles Cloud-Netzwerk mit Internetverbindung erstellen](../common/images/create-firewall-vcn-with-internet-connectivity.png " ")
    
4.  Füllen Sie das Dialogfeld aus, und klicken Sie auf **Weiter**:
    
    *   **VCN-Name**: Geben Sie einen Namen an.
    *   **Compartment**: Stellen Sie sicher, dass das Compartment ausgewählt ist
    *   **VCN-CIDR-Block**: Geben Sie einen CIDR-Block an (10.10.0.0/16)
    *   **CIDR-Block für öffentliches Subnetz**: Geben Sie einen CIDR-Block an (10.10.0.0/24)
    *   **CIDR-Block für privates Subnetz**: Geben Sie einen CIDR-Block an (10.10.1.0/24)
    
    ![Virtuelles VSS-Cloud-Netzwerk erstellen](../common/images/create-firewall-vcn.png " ")
    
5.  Prüfen Sie alle Informationen, und klicken Sie auf **Erstellen**.
    
    ![Details des virtuellen VSS-Cloud-Netzwerks bestätigen](../common/images/create-firewall-vcn-confirm-details.png " ")
    
6.  Dadurch wird ein VCN mit den folgenden Komponenten erstellt.
    
    _VCN, Standardroutentabellen, Standardsicherheitsliste, öffentliches Subnetz, privates Subnetz, NAT-Gateway, Servicegateway und Internetgateway_
    
7.  Klicken Sie auf **Virtuelles Cloud-Netzwerk anzeigen**, das Sie gerade erstellt haben, um die VCN-Details anzuzeigen.
    
    ![Virtuelles VSS-Cloud-Netzwerk anzeigen](../common/images/create-firewall-vcn-successfully.png " ")
    
8.  Navigieren Sie zur Seite "Details virtuelles Cloud-Netzwerk für **VSS-VCN**", um weitere Informationen zu VCN-Details zu erhalten:
    
    ![Info VSS Virtual Cloud Network](../common/images/created-firewall-vcn-info.png " ")
    

## Aufgabe 2: Compute-Instanzen im VSS-VCN starten

1.  Starten Sie **Cloud Shell**, indem Sie auf das Symbol neben dem Regionsnamen oben rechts in der OCI-Konsole klicken. ("<="-Symbol)
    
2.  Sobald die Cloud Shell gestartet wurde. Geben Sie den Befehl **ssh-keygen** ein, und drücken Sie die Eingabetaste für alle Eingabeaufforderungen. Dadurch wird ein SSH-Schlüsselpaar erstellt. Geben Sie den Befehl ein.
    
        <copy>
        bash
        cd .ssh
        cat id_rsa.pub
        </copy>
        
    
    Kopieren Sie den angezeigten Schlüssel. Dies wird beim Erstellen der Compute-Instanz verwendet.
    
3.  Klicken Sie im Menü "OCI-Services" unter **Compute** auf **Instanzen**.
    
4.  Wählen Sie in der linken Randleiste das **Compartment** aus, in dem Sie Ihr **VSS-VCN** unter **Listengeltungsbereich** platziert haben. Klicken Sie dann auf **Instanz erstellen**. Sie erstellen **2** Instanzen gemäß der folgenden Tabelle:
    
    | Name | Position | Bild | Form | Netzwerk | Subnetz | SSH-Schlüssel hinzufügen | Öffentliche IP zuweisen |
    | --- | --- | --- | --- | --- | --- | --- | --- |
    | Compute-vm-1 | AD1 | Standard: Oracle Linux | Standardwert | vss-vcn | Öffentliches Compute-Subnetz | Ihr/CloudShell Public Key | Ja |
    | Compute-vm-2 | AD2 oder AD1 | Standard: Oracle Linux | Standardwert | vss-vcn | Öffentliches Compute-Subnetz | Ihr/CloudShell Public Key | Nicht anwendbar |
    
5.  Geben Sie einen **Namen** für Ihre Instanz und das **Compartment** ein, in dem Sie Ihr **VSS-VCN** platziert haben.
    
6.  Füllen Sie das Dialogfeld aus. Übernehmen Sie als Standardwerte **Image oder Betriebssystem** und **Availability-Domain**.
    
7.  Übernehmen Sie für die Ausprägung den Standardwert **Ausprägung**.
    
8.  Scrollen Sie nach unten zu **Networking**, und prüfen Sie Folgendes.
    
    *   Ihr Compartment ist ausgewählt
    *   Das erstellte VCN wird aufgefüllt: **VSS-VCN**
    *   Das erstellte Subnetz wird aufgefüllt:
        *   Wählen Sie **Öffentliches Compute-Subnetz** für Compute-VM-1-VM aus.
        *   Wählen Sie **Öffentliches Compute-Subnetz** für Compute-VM-1-VM aus.
9.  Stellen Sie sicher, dass unter **SSH-Schlüssel hinzufügen** die Option **PASTE PUBLIC KEYS** ausgewählt ist. Fügen Sie den zuvor kopierten PUBLIC Key ein.
    
    > **Hinweis:** Wenn der Fehler "Servicelimit" angezeigt wird, wählen Sie eine andere Ausprägung aus als VM.Standard.E4. Flex, VM.Standard2.1, VM.Standard.E2.1, VM.Standard1.1, VM.Standard.B1.1 ODER wählen Sie eine andere AD.
    
    > **Hinweis:** Wenn Ihr SSH-Schlüssel bereits verfügbar ist, können Sie das Kopieren aus Cloud-Shell überspringen, Ihren Public Key einfügen und den damit verknüpften Private Key für den Zugriff auf die Instanz verwenden.
    
10.  Klicken Sie auf **Erstellen**, und warten Sie, bis sich die Instanz im Status **Wird ausgeführt** befindet.
    
11.  Stellen Sie sicher, dass die erforderlichen Instanzen im **VSS-VCN** den Status **Wird ausgeführt** aufweisen.
    

![Laufende Instanz in VSS-VCN erstellt](../common/images/final-manual-instances.png " ")

_**Glückwunsch! Sie haben die Übung abgeschlossen.**_

Sie können jetzt **mit der nächsten Übung fortfahren**.

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