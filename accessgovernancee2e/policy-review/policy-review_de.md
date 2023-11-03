# Identitätsorchestrierung zwischen Oracle Acccess Governance und OCI IAM einrichten

## Einführung

Als OCI-Mandantenadministratoren und Access Governance-Administratoren können sie lernen, Oracle Access Governance in OCI IAM zu integrieren.

*   Geschätzte Zeit: 15 Minuten
*   Persona: Administrator

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   API-Schlüssel und Oracle Cloud-ID (OCID) für einen Identitätsbenutzer generieren
*   Neue OCI IAM Cloud Service-Verbindung in der Oracle Access Governance-Konsole konfigurieren

## Aufgabe 1: Policy einrichten, mit der Oracle Access Governance OCI verbinden kann

1.  Klicken Sie in der OCI-Konsole in der oberen linken Ecke auf das Symbol "Navigationsmenü", um das Navigationsmenü anzuzeigen. Klicken Sie im Navigationsmenü auf Identität und Sicherheit. Wählen Sie Policen aus der Produktliste aus.
    
2.  Klicken Sie auf der Seite "Policys" im Root Compartment auf "Policy erstellen", um eine Policy zu erstellen: oci-iam-policy
    
        Name: oci-iam-policy
        Description: Allow Oracle Access Governance to connect OCI in tenancy
        Compartment: Ensure your root compartment is selected
        Policy Builder: Select the show manual editor checkbox 
        Statement 1: allow resource accessgov-agent resource-scanner to read all-resources in tenancy
        Statement 2: allow resource accessgov-agent resource-manager to manage domains in tenancy
        Statement 3: allow resource accessgov-agent resource-manager to manage policies in tenancy
        
    
    Klicken Sie auf "Create".
    

## Aufgabe 2: Neue OCI IAM Cloud Service-Verbindung in der Oracle Access Governance-Konsole konfigurieren

1.  Navigieren Sie in einem Browser zur Homepage des Oracle Access Governance-Service, und melden Sie sich als Benutzer mit der Administratoranwendungsrolle an.
    
2.  Klicken Sie auf der Homepage des Oracle Access Governance-Service auf das Navigationsmenüsymbol, und wählen Sie **Serviceadministration → Verbundene Systeme** aus.
    
3.  Wählen Sie auf der Seite "Verbundene Systeme" die Schaltfläche **Verbundenes System hinzufügen**.
    
    ![Cloudserviceprovider auswählen](images/cloud-service-provider.png)
    
4.  Wählen Sie die Kachel **Möchten Sie eine Verbindung zu einem Cloud-Serviceprovider herstellen?** aus, indem Sie auf die Schaltfläche "Hinzufügen" klicken.
    
5.  Wählen Sie im Schritt **System auswählen** die Kachel **Oracle Cloud Infrastructure** aus, und klicken Sie auf **Weiter**.
    

![Cloudserviceprovider auswählen](images/select-oci.png)

6.  Geben Sie den Namen und eine Beschreibung des verbundenen Systems ein, und klicken Sie auf **Weiter**.

![OCI - Details eingeben](images/enter-oci-system-name.png)

7.  Geben Sie die Mandanten-OCID und Regionsdetails ein.

![OCI - Details eingeben](images/oci-iam-details.png)

8.  Klicken Sie auf **Hinzufügen.** Wenn die Verbindungsdetails erfolgreich validiert wurden, wird der Status **Erfolgreich** für den Vorgang **Validieren** angezeigt. Der vollständige Dataload-Vorgang kann je nach den in Ihrem OCI-Mandanten verfügbaren Daten bis zu ein paar Minuten dauern. Der inkrementelle Dataload wird alle vier Stunden ausgeführt, damit dieses verbundene System die Daten synchronisiert.

![OCI-Verbindungsstatus](images/oci-connection-status.png)

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

*   [Oracle Access Governance - Zugriffsprüfungskampagne erstellen](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance - Produktseite](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governance - Produkttour](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access Governance - Häufig gestellte Fragen](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Bestätigungen

*   **Autoren** - Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Mitwirkende** - Abhishek Juneja
*   **Zuletzt aktualisiert am/um** - Anbu Anbarasu, Cloud Platform COE, Januar 2023