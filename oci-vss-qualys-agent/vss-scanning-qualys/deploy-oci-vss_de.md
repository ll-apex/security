# Vulnerability Scanning-Service bereitstellen und Konfiguration unterstützen

## Einführung

In dieser Übung erstellen Sie eine erforderliche dynamische Gruppe, Policy, Vault, Lizenz-Secret, Scanrezept und erforderliche Zielregeln, um das Scannen von Compute-Ressourcen in **VSS-VCN** zu aktivieren.

Geschätzte Zeit: 15 Minuten.

### Ziele

*   Dynamische Gruppe und Policy konfigurieren
*   Vault konfigurieren und Agent-Lizenz-Secret hinzufügen
*   Scanrezept und Ziel konfigurieren
*   Gescannte Ziele und Agent-Installation validieren

### Voraussetzungen

*   Oracle Cloud Infrastructure-Accountzugangsdaten (Benutzer, Kennwort, Mandant und Compartment)
*   Der Benutzer muss über die erforderlichen Berechtigungen und Quota verfügen, um Ressourcen bereitzustellen.

## Aufgabe 1: Dynamische Gruppe und erforderliche Policy erstellen

1.  Klicken Sie im Menü "OCI-Services" unter **Identität und Sicherheit** auf **Dynamische Gruppen**. Wählen Sie Ihre Region rechts auf dem Bildschirm aus:
    
    ![Homepage für dynamische Gruppen erstellen](../common/images/create-dynamic-group-home-page.png " ")
    
2.  Sie erstellen eine **dynamische Gruppe**, die **Instanz-OCIDs** oder **Compartment-OCIDs** enthält, die Sie scannen möchten. Basierend auf den folgenden Tabellen erstellen Sie eine dynamische Gruppe.
    
    | Ressource | Name | Kommentar |
    | --- | --- | --- |
    | Dynamische Gruppe | vss-Demo | Beispiel: Alle {instance.compartment.id} = Compartment ocid\_here eingeben |
    
    > **Hinweis:** Sie können auch den gesamten Mandanten angeben.
    
3.  Wählen Sie **Dynamische Gruppe erstellen**, und füllen Sie das Dialogfeld zum Erstellen einer dynamischen Gruppe aus:
    
    *   **Name**: Geben Sie einen Namen ein.
    *   **Beschreibung**: Geben Sie einen benutzerfreundlichen Namen ein.
    *   **Mit Instanz, die mit** übereinstimmt: Wählen Sie **"Alle der folgenden Optionen" aus:**
    *   **Instanzen abgleichen mit**: Wählen Sie die Compartment-OCID aus, und geben Sie die Compartment-OCID der Compute-Ressourcen ein.
    
    ![Dynamische Gruppe erstellen](../common/images/create-dynamic-group.png " ")
    
4.  Prüfen Sie alle Informationen, und klicken Sie auf **Regel hinzufügen** und **Erstellen**
    
5.  Dadurch wird eine dynamische Gruppe mit den folgenden Komponenten erstellt.
    
    _Neue dynamische Gruppe mit erforderlichen Compute-Compartment-OCIDs zum Scannen_
    
6.  Klicken Sie im Menü "OCI-Services" unter **Identität und Sicherheit** auf **Policys**. Wählen Sie Ihre Region rechts auf dem Bildschirm aus:
    
7.  Jetzt erstellen Sie eine **Policy**, die der dynamischen Gruppe die Berechtigung zum Zugriff auf Secrets erteilt und Daten von **Qualys** zurücksendet. Basierend auf den folgenden Tabellen erstellen Sie eine Policy.
    
    *   **Name**: Geben Sie den Policy-Namen ein.
    *   **Beschreibung**: Policy-Beschreibung eingeben
    *   **Compartment**: Stellen Sie sicher, dass das Root Compartment ausgewählt ist
        *   Sie können die Policy je nach Bedarf auf Root- oder erforderlicher Compartment-Ebene definieren.
        *   Bitte folgen Sie den offiziellen Dokumenten im Abschnitt **Weitere Informationen**, um mehr über die **Agent-basierten Qualys Richtlinien** zu erfahren.
    *   **Policy Builder**: Geben Sie die Policy ein
        *   **Einträge**: Stellen Sie sicher, dass Sie den richtigen Namen für die dynamische Gruppe eingeben, wenn es sich um **vss-demo handelt**
    
        <copy>
        Define tenancy ocivssprod as ocid1.tenancy.oc1..aaaaaaaa6zt5ejxod5pgthsq4apr5z2uzde7dmbpduc5ua3mic4zv3g5ttma
        Allow dynamic-group vss-demo to read vaults in tenancy
        Allow dynamic-group vss-demo to read keys in tenancy
        Allow dynamic-group vss-demo to read secret-family in tenancy
        Endorse dynamic-group vss-demo to read objects in tenancy ocivssprod
        </copy>
        
    
    ![Policy für dynamische Gruppe erstellen](../common/images/create-dynamic-group-policy.png " ")
    
8.  Prüfen Sie alle Informationen, und klicken Sie auf **Erstellen**.
    
9.  Dadurch wird eine Policy mit den folgenden Komponenten erstellt.
    
    _Neue Policy im Root Compartment vorhanden, die mit einer zuvor erstellten dynamischen Gruppe verknüpft ist_
    

## Aufgabe 2: Vault und Secret erstellen

1.  Klicken Sie im Menü "OCI-Services" unter **Identität und Sicherheit** auf **Vault**. Wählen Sie Ihre Region rechts auf dem Bildschirm aus:
    
    ![Navigationsfenster für Vault](../common/images/vault-home.png " ")
    
2.  Die folgende Tabelle stellt dar, was Sie erstellen. Klicken Sie auf das Symbol **Vault erstellen**, um einen neuen **Vault** zu erstellen:
    
    | Ressource | Name | Kommentar |
    | --- | --- | --- |
    | Vault | Demo-Vault | Vault zum Speichern von Lizenz-Secrets |
    
    ![Schaltfläche "VM-Netzwerk erstellen"](../common/images/vault-create.png " ")
    
3.  Füllen Sie das Dialogfeld aus, und klicken Sie auf **Weiter**:
    
    *   **Vault-Name**: Geben Sie einen Namen an
    *   **Compartment**: Stellen Sie sicher, dass das Compartment ausgewählt ist
    
    ![Virtuelles VSS-Cloud-Netzwerk erstellen](../common/images/vault-create-details.png " ")
    
4.  Prüfen Sie alle Informationen, und klicken Sie auf **Vault erstellen**.
    
5.  Dadurch wird ein Vault mit den folgenden Komponenten erstellt.
    
    _Vault_
    
6.  Fahren Sie mit dem nächsten Schritt fort, nachdem **demo-vault** erfolgreich erstellt wurde. Dieser Vorgang dauert **einige Minuten**.
    
7.  Klicken Sie auf **demo-vault**, und navigieren Sie zum **Masterverschlüsselungsschlüssel**, um den Schlüssel zu erstellen. Sie erstellen einen Verschlüsselungsschlüssel in der folgenden Tabelle:
    
    | Ressource | Name | Schutzmodus | Kommentar |
    | --- | --- | --- | --- |
    | Master-Verschlüsselungsschlüssel | ms-Schlüssel | HSM | Schlüssel zum Verschlüsseln des Secrets. Sie können auch den Vault und den Schlüssel verwenden |
    
8.  Klicken Sie auf **Schlüssel erstellen**, und füllen Sie das Dialogfeld aus:
    
    *   **Compartment**: Stellen Sie sicher, dass das Compartment ausgewählt ist
    *   **Schutzmodus**: **HSM auswählen**
    *   **Name**: Geben Sie einen Namen an
    *   **Schlüsselausprägungsalgorithmus**: Wählen Sie **AWES** aus.
    
    ![Master-Verschlüsselungsschlüssel in Demo Vault erstellen](../common/images/create-master-encryption-key.png " ")
    
9.  Prüfen Sie alle Informationen, und klicken Sie auf **Schlüssel erstellen**.
    
10.  Dadurch wird der Masterverschlüsselungsschlüssel mit den folgenden Komponenten erstellt.
    
    _Master-Verschlüsselungsschlüssel mit ms-Schlüsselname_
    
11.  Klicken Sie in Ihrem **Vault** auf die Option **Secrets**, um ein Secret zu erstellen. Sie erstellen ein Secret mit dem Lizenzcode gemäß der folgenden Tabelle:
    
    | Ressource | Name | Verschlüsselungsschlüssel | Secret-Typvorlage | Kommentar |
    | --- | --- | --- | --- | --- |
    | Secret | Lizenzgeheimnis | ms-Schlüssel | Base64 | Geben Sie Ihren Lizenzcode in Secret Contents ein |
    
12.  Klicken Sie auf **Secret erstellen**, und füllen Sie das Dialogfeld aus:
    
    *   **Compartment**: Stellen Sie sicher, dass das Compartment ausgewählt ist
    *   **Name**: Geben Sie einen Namen an
    *   **Encryption Key**: Wählen Sie **mas-key aus**
    *   **Secret-Typvorlage**: Wählen Sie **Base64 aus**
    *   **Secret-Inhalt**: Geben Sie **Lizenzcodewert** ein.

![Lizenz-Secret in Demo Vault erstellen](../common/images/create-lic-secret.png " ")

14.  Prüfen Sie alle Informationen, und klicken Sie auf **Secret erstellen**.
    
15.  Dadurch wird der Masterverschlüsselungsschlüssel mit den folgenden Komponenten erstellt.
    
    _Secret mit Lizenzcodewert_
    

## Aufgabe 3: Scanrezept und Ziel erstellen

1.  Klicken Sie im Menü "OCI-Services" unter **Identität und Sicherheit** auf **Scannen**. Wählen Sie Ihre Region rechts auf dem Bildschirm aus:
    
    ![Navigationsfenster für Scannen](../common/images/scanning-home.png " ")
    
2.  Die folgende Tabelle stellt dar, was Sie erstellen. Klicken Sie auf das Symbol **Erstellen**, um ein neues **Scanrezept** zu erstellen:
    
    | Ressource | Zu verwendender Agent | Vault | Secret | Kommentar |
    | --- | --- | --- | --- | --- |
    | Scanrezept | Qualys | Vault auswählen | Wählen Sie Ihr Lizenz-Secret aus | Wählen Sie Ihre Planung entsprechend aus |
    
    ![Scanrezept erstellen](../common/images/scanning-create.png " ")
    
3.  Füllen Sie das Dialogfeld aus:
    
    *   **Typ**: Compute auswählen
    *   **Name**: Geben Sie einen Namen an
    *   **Agent-basierter Scan**: **Qualitäten auswählen**
    *   **Vault**: Wählen Sie Ihren Vault aus.
    *   **Secret**: Secret auswählen
    
    ![Scanrezept mit Details erstellen](../common/images/scanning-create-details.png " ")
    
4.  Prüfen Sie alle Informationen, und klicken Sie auf **Scanrezept erstellen**.
    
5.  Dadurch wird ein Scanrezept mit den folgenden Komponenten erstellt.
    
    _Qualys Agent-basiertes Scanrezept_
    
6.  Klicken Sie unter **Hosts** auf **Ziele**, um das Scannen mit dem Scanrezept zu aktivieren. Sie erstellen ein Ziel basierend auf der folgenden Tabelle:
    
    | Ressource | Name | Scanrezept | Kommentar |
    | --- | --- | --- | --- |
    | Hostziel | Qualys-Validation-Ziele | Wählen Sie das zuvor erstellte qualys-validation-recipe. | Wählen Sie das Ziel-Compartment aus, in dem Compute-Hosts ausgeführt werden, und Sie haben die erforderlichen Policys |
    
    ![Scanninghostziele erstellen](../common/images/create-target-page.png " ")
    
7.  Klicken Sie auf **Erstellen**, und füllen Sie das Dialogfeld aus:
    
    *   **Name**: Geben Sie einen Namen an
    *   **Compartment**: Stellen Sie sicher, dass das Compartment ausgewählt ist
    *   **Beschreibung**: Geben Sie eine benutzerfreundliche Beschreibung ein.
    *   **Scanrezept**: Wählen Sie Ihr Scanrezept aus
    *   **Ziel-Compartment**: Stellen Sie sicher, dass das Compartment ausgewählt ist
    *   **Ziele**: Stellen Sie sicher, dass die Instanzen Ihres Compartments oder Compartments ausgewählt sind, die Sie scannen möchten.
    
    ![Scanninghostziele mit Details erstellen](../common/images/target-create-details.png " ")
    
8.  Prüfen Sie alle Informationen, und klicken Sie auf **Ziel erstellen**.
    
9.  Dadurch wird ein Hostziel mit den folgenden Komponenten erstellt.
    
    _Hostziel, um das Scannen nach Compute-Ressourcen zu aktivieren_
    

## Aufgabe 4: Gescannte Ziele prüfen

1.  Klicken Sie im Menü "OCI-Services" unter **Compute** auf **Instanzen**. Wählen Sie Ihre Region rechts auf dem Bildschirm aus.
    
2.  Navigieren Sie zu den Instanzen von **compute-vm-1** und/oder **compute-vm-2**, um die Installation des **Qualys**\-Agents zu prüfen.
    
3.  In der Meldung neben der Registerkarte **Vulnerability Scanning** sollte **...Plug-in erfolgreich gestartet, Installation von Qualys-Agent abgeschlossen** angezeigt werden, was darauf hinweist, dass der erforderliche Agent auf Ihrem Compute-Host erfolgreich installiert wurde und auf Qualys VMDR sichtbar sein sollte.
    
4.  \[Optional\] Sie können auch das Plug-in für **Vulnerability Scanning** prüfen, wenn es nicht aktiviert ist. Aktivieren Sie es mit der Schaltfläche **Aktiviert** in der OCI-Konsole.
    
5.  Das folgende Bild zeigt die **compute-vm-1**\-Instanzdetails:
    
    ![Installation von Compute Instance1 Qualys Agent prüfen](../common/images/verify-scanned-targets-compute-instance-1.png " ")
    
6.  Das folgende Bild zeigt die **compute-vm-2**\-Instanzdetails:
    
    ![Installation von Compute Instance1 Qualys Agent prüfen](../common/images/verify-scanned-targets-compute-instance-2.png " ")
    
7.  \[Optional\] Sie können zusätzliche Compute-Ressourcen bereitstellen und prüfen, ob der Qualys-Agent erfolgreich installiert wurde.
    
8.  Sie sollten in der Lage sein, **Assets**, die erfolgreich in Ihrem **Qualys VMDR** gemeldet wurden, wie folgt anzuzeigen:
    
    ![Lizenz-Secret in Demo Vault erstellen](../common/images/qualys-vmdr-assets.png " ")
    

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