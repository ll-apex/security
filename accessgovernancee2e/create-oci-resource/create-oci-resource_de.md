# OCI-Policys, -Gruppen und -Compartments erstellen

## Einführung

Als Benutzer mit der Rolle **Administrator** in der Identitätsdomain können Sie OCI-Policys, -Gruppen und -Compartments in der Übung **OCI** console.This erstellen. In dieser Übung erfahren Sie, wie Sie die OCI-Policys, -Gruppen und -Compartments einrichten, die zur Ausführung dieser OCI-IAM-Policy-Prüfungen erforderlich sind.

*   Geschätzte Zeit: 15 Minuten
*   Persona: Identitätsdomainadministrator

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   OCI-Policys, -Gruppen und -Compartments manuell erstellen
*   In dieser Übung erstellen wir die folgenden Ressourcen:

| Ressourcenart | Ressource | Beschreibung |
| :-- | :-: | :-: |
| Compartment | Entwicklung | Entwicklung |
|  | Qualitätssicherung | Qualitätssicherung |
|  | Testen | Testen |
| Benutzer | demouser1 | demouser1 gehört zu Gruppen - SecurityAdmins |
|  | demouser2 | demouser2 gehört zu den Gruppen SecurityAdmins und NetworkAdmins |
|  | demouser3 | demouser3 gehört zu Gruppen - SecurityAdmins und Auditoren |
| Gruppen | SecurityAdmins | SecurityAdmins |
|  | NetworkAdmins | NetworkAdmins |
|  | Auditoren | Auditoren |
| Policys | tf1-Auditors-Policy | Zugriffs-Policy für Auditoren |
|  | tf2-network-admins-policy | Zugriffs-Policy für Netzwerkadministratoren |
|  | tf3-security-admins-policy | Zugriffs-Policy für Sicherheitsadministratoren |

## Aufgabe 1: Compartments erstellen

1.  Melden Sie sich bei der Identitätsdomain der OCI-Konsole an: ag-domain als Identitätsdomainadministrator.

![Bei OCI-Konsole anmelden](images/oci-console.png)

2.  Klicken Sie in der OCI-Konsole in der oberen linken Ecke auf das Symbol "Navigationsmenü", um das Navigationsmenü anzuzeigen. Klicken Sie im Navigationsmenü auf Identität und Sicherheit. Wählen Sie "Compartments" aus der Produktliste aus.

![Naviagte zu Compartment](images/navigate-compartment.png)

3.  Klicken Sie auf _Compartment erstellen._ Geben Sie die folgenden Details an, um die 3 Compartments zu erstellen: **Entwicklung, Qualitätssicherung und Tests**
    
    ![Compartment](images/create-compartment.png)
    

**Name:** Entwicklung

**Beschreibung:** Entwicklung

**Übergeordnetes Compartment:** Wählen Sie das Root Compartment aus

Klicken Sie auf _Compartment erstellen_

![Compartment erstellen](images/create-compartment-tab.png)

**Name:** Qualitätssicherung

**Beschreibung:** Qualitätssicherung

**Übergeordnetes Compartment:** Wählen Sie das Root Compartment aus

Klicken Sie auf _Compartment erstellen_

![Compartment erstellen](images/qa-compartment.png)

**Name:** Tests

**Beschreibung:** Tests

**Übergeordnetes Compartment:** Wählen Sie das Root Compartment aus

Klicken Sie auf _Compartment erstellen_

![Compartment erstellen](images/testing-compartment.png)

Die _Compartments_ wurden erfolgreich erstellt.

## 2\. Aufgabe: Gruppen erstellen

1.  Navigieren Sie zu Identity -> Domains -> ag-domain -> Groups.
    
    ![Gruppen erstellen](images/create-group.png)
    
2.  Klicken Sie auf _Gruppe erstellen_. Geben Sie die folgenden Details ein, um 3 Gruppen zu erstellen: **SecurityAdmins, NetworkAdmins and Auditors**
    

**Name:** SecurityAdmins

**Beschreibung:** SecurityAdmins

Klicken Sie auf _Erstellen_.

![Gruppen erstellen](images/securityadmins-group.png)

**Name:** NetworkAdmins

**Beschreibung:** NetworkAdmins

Klicken Sie auf _Erstellen_.

![Gruppen erstellen](images/networkadmins-group.png)

**Name:** Auditoren

**Beschreibung:** Auditoren

Klicken Sie auf _Erstellen_.

![Gruppen erstellen](images/auditors-group.png)

Die _Gruppen_ wurden erfolgreich erstellt.

## Aufgabe 3: Beispielbenutzer erstellen

1.  Navigieren Sie zu Identity -> Domains -> ag-domain -> Users
    
    ![Benutzer erstellen](images/create-user.png)
    
2.  Klicken Sie auf _Benutzer erstellen_. Geben Sie die folgenden Details ein, um 3 Beispielbenutzer zu erstellen: **demouser1, demouser2 und demouser3**
    

**First Name:** Demo

**Nachname:** user1

**Benutzername:** demouser1

**E-Mail:** demouser1@example.com

**Gruppen:** Aktivieren Sie das Kontrollkästchen **SecurityAdmins**

Klicken Sie auf _Erstellen_.

![Benutzer erstellen](images/demo-user1.png)

![Benutzer erstellen](images/createuser-securityadmin.png)

**First Name:** Demo

**Nachname:** user2

**Benutzername:** demouser2

**E-Mail:** demouser2@example.com

**Gruppen:** Aktivieren Sie das Kontrollkästchen **SecurityAdmins** und **NetworkAdmins**

Klicken Sie auf _Erstellen_.

![Benutzer erstellen](images/demo-user2.png)

![Benutzer erstellen](images/user-group-assign.png)

**First Name:** Demo

**Nachname:** user3

**Benutzername:** demouser3

**E-Mail:** demouser3@example.com

**Gruppen:** Aktivieren Sie das Kontrollkästchen **SecurityAdmins** und **Auditoren**

Klicken Sie auf _Erstellen_.

![Benutzer erstellen](images/demo-user3.png)

![Benutzer erstellen](images/user-group.png)

Die _Benutzer_ wurden erfolgreich erstellt.

## Aufgabe 4: Policys erstellen

1.  Klicken Sie in der OCI-Konsole in der oberen linken Ecke auf das Symbol "Navigationsmenü", um das _Navigationsmenü anzuzeigen._ Klicken Sie im _Navigationsmenü_ auf _Identität und Sicherheit_. Wählen Sie _Policys_ aus der Produktliste.

![Policys auswählen](images/policy-page.png)

2.  Klicken Sie auf der Seite "Policys" jedes Mal auf _Policy erstellen_, um 3 Policys zu erstellen: **auditors-policy, network-admins-policy and security-admins-policy**
    
        Name: auditors_policy
        Description: Access Policy for Auditors
        Compartment: Ensure your root compartment is selected
        Policy Builder: Select the show manual editor checkbox
        
        
    
        <copy>Allow group Auditors to inspect all-resources in tenancy
        Allow group Auditors to read instances in compartment Quality-Assurance
        Allow group Auditors to read audit-events in compartment Quality-Assurance </copy>
        
    
    Klicken Sie auf _Erstellen_.
    

![Policy erstellen](images/create-policy-auditor.png)

    ```
    Name: network_admins_policy
    
    Description: Access Policy for Network Administrators
    
    Compartment: Ensure your root compartment is selected
    
    Policy Builder: Select the show manual editor checkbox
    
    ```
    
      ```
      <copy>Allow group NetworkAdmins to manage virtual-network-family in tenancy	
      Allow group NetworkAdmins to manage load-balancers in compartment Quality-Assurance	
      Allow group NetworkAdmins to manage load-balancers in compartment Development	
      Allow group NetworkAdmins to manage load-balancers in compartment Testing	
      Allow group NetworkAdmins to manage instances in compartment Quality-Assurance	
      Allow group NetworkAdmins to manage instances in compartment Development	
      Allow group NetworkAdmins to manage instances in compartment Testing</copy>
      ```  
    
    Click *Create*
    
    ![Create Policy](images/create-policy-networkadmin.png)
    
    
    ```
    Name: security_admins_policy
    Description: Access Policy for Security Admins
    Compartment: Ensure your root compartment is selected
    Policy Builder: Select the show manual editor checkbox
    
    ```
    
     ```
    <copy>Allow group SecurityAdmins to manage bastion in compartment Quality-Assurance
    Allow group SecurityAdmins to manage bastion-session in compartment Quality-Assurance
    Allow group SecurityAdmins to read instance-family in compartment Quality-Assurance
    Allow group SecurityAdmins to read instance-agent-plugins in compartment Quality-Assurance
    Allow group SecurityAdmins to inspect work-requests in compartment Quality-Assurance
    Allow group SecurityAdmins to manage bastion in compartment Testing
    Allow group SecurityAdmins to manage bastion-session in compartment Testing
    Allow group SecurityAdmins to manage virtual-network-family in compartment Testing
    Allow group SecurityAdmins to read instance-family in compartment Testing
    Allow group SecurityAdmins to read instance-agent-plugins in compartment Testing
    Allow group SecurityAdmins to inspect work-requests in compartment Testing</copy>
    ```  
    
    Click *Create*
    

![Policy erstellen](images/create-policy-securityadmin.png)

Die _Policys_ wurden erfolgreich erstellt.

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