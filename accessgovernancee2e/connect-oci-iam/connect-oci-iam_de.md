# Oracle Access Governance mit OCI IAM integrieren

## Einführung

Als OCI-Mandantenadministratoren und Access Governance-Administratoren können sie lernen, Oracle Access Governance in OCI IAM zu integrieren.

*   Geschätzte Zeit: 15 Minuten
*   Persona: Administrator

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Setup-Policy, mit der Oracle Access Governance OCI verbinden kann
*   Neue OCI IAM Cloud Service-Verbindung in der Oracle Access Governance-Konsole konfigurieren

## Aufgabe 1: Policy einrichten, mit der Oracle Access Governance OCI verbinden kann

1.  Melden Sie sich bei der Identitätsdomain der OCI-Konsole an: ag-domain als Identitätsdomainadministrator.
    
2.  Klicken Sie in der OCI-Konsole in der oberen linken Ecke auf das Symbol "Navigationsmenü", um das Navigationsmenü anzuzeigen. Klicken Sie im Navigationsmenü auf Identität und Sicherheit. Wählen Sie Policen aus der Produktliste aus.
    
3.  Klicken Sie auf der Seite "Policys" im Root Compartment auf "Policy erstellen", um eine Policy zu erstellen: oci-iam-policy
    
        Name: oci-iam-policy
        Description: Allow Oracle Access Governance to connect OCI in tenancy
        Compartment: Ensure your root compartment is selected
        Policy Builder: Select the show manual editor checkbox
        
    
        <copy>allow resource accessgov-agent resource-scanner to read all-resources in tenancy
        allow resource accessgov-agent resource-manager to manage domains in tenancy
        allow resource accessgov-agent resource-manager to manage policies in tenancy
        </copy>
        
    
    Klicken Sie auf "Create".
    

## Aufgabe 2: Neue OCI IAM Cloud Service-Verbindung in der Oracle Access Governance-Konsole konfigurieren

1.  Navigieren Sie in einem Browser zur Homepage des Oracle Access Governance-Service, und melden Sie sich als Benutzer mit der Administratoranwendungsrolle an.

Geben Sie den Benutzernamen und das Kennwort für den Oracle Access Governance Campaign-Administrator ein (Pamela Green)

    **Username:**
    ```
    <copy>pamela.green</copy>
    ```
    
    **Password:**
    ```
    <copy>Oracl@123456</copy>
    ```
    

2.  Klicken Sie auf der Homepage des Oracle Access Governance-Service auf das Navigationsmenüsymbol, und wählen Sie **Serviceadministration → Verbundene Systeme** aus.
    
3.  Wählen Sie auf der Seite "Verbundene Systeme" die Schaltfläche **Verbundenes System hinzufügen**.
    
    ![Cloudserviceprovider auswählen](images/cloud-service-provider.png)
    
4.  Wählen Sie die Kachel **Möchten Sie eine Verbindung zu einem Cloud-Serviceprovider herstellen?** aus, indem Sie auf die Schaltfläche "Hinzufügen" klicken.
    
5.  Wählen Sie im Schritt **System auswählen** die Kachel **Oracle Cloud Infrastructure** aus, und klicken Sie auf **Weiter**.
    

![Cloudserviceprovider auswählen](images/select-oci.png)

6.  Geben Sie den Namen und eine Beschreibung des verbundenen Systems ein, und klicken Sie auf **Weiter**.

Name: OCI-IAM

Beschreibung: OCI-IAM

![OCI - Details eingeben](images/enter-oci-system-name.png)

7.  Geben Sie die Mandanten-OCID und die Regions-ID ein.

Um die Mandanten-OCID abzurufen, navigieren Sie in der oberen rechten Ecke zum Benutzerprofil, und klicken Sie auf "Mandant". Beachten Sie die Mandanten-OCID zur weiteren Verwendung.

![OCI - Details eingeben](images/navigate-tenancy.png)

![OCI - Details eingeben](images/tenancy-ocid.png)

Um die Regions-ID zu erhalten, lesen Sie den unten genannten Link.

https://docs.oracle.com/en/cloud/paas/access-governance/cagsi

![OCI - Details eingeben](images/oci-iam-details.png)

8.  Klicken Sie auf **Hinzufügen.** Klicken Sie auf "Verwalten", um den Status anzuzeigen. Wenn die Verbindungsdetails erfolgreich validiert wurden, wird der Status **Erfolgreich** für den Vorgang **Validieren** angezeigt. Der vollständige Dataload-Vorgang kann je nach den in Ihrem OCI-Mandanten verfügbaren Daten bis zu ein paar Minuten dauern. Der inkrementelle Dataload wird alle vier Stunden ausgeführt, damit dieses verbundene System die Daten synchronisiert.

![OCI-Verbindungsstatus](images/oci-connection-status.png)

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

*   [Oracle Access Governance - Zugriffsprüfungskampagne erstellen](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance - Produktseite](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governance - Produkttour](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access Governance - Häufig gestellte Fragen](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Bestätigungen

*   **Autoren** - Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Zuletzt aktualisiert am/um** - Anbu Anbarasu, Mai 2023