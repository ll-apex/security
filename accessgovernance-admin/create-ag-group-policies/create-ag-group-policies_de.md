# Gruppen und Policys für Access Governance erstellen

## Einführung

Policys für Access Governance erstellen.

*   Persona: Standarddomainadministrator

_Geschätzte Zeit_: 15 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Oracle Video Hub - Video ohne Größenanpassung](videohub:1_x0vzlnhh)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   **agcs-user** für Access Governance erstellen
*   **Gruppen** für Access Governance einrichten
*   **Policys** für Access Governance einrichten
*   **Policy** einrichten, damit Oracle Access Governance OCI verbinden kann
*   **Policy** für Domainadministratorzugriff einrichten

## Aufgabe 1: AGCS-Benutzer erstellen

1.  Melden Sie sich bei der **Standarddomain** der OCI-Konsole als **Standarddomainadministrator** an
    
2.  Klicken Sie in der OCI-Konsole in der oberen linken Ecke auf das Symbol "Navigationsmenü", um das _Navigationsmenü anzuzeigen._ Klicken Sie im _Navigationsmenü_ auf _Identität und Sicherheit_. Wählen Sie _Domains_ aus der Produktliste.
    
    ![Zu Domains navigieren](images/navigate-domains.png)
    
3.  Klicken Sie auf der Seite "Domains" auf _Standarddomain_. Stellen Sie sicher, dass das **Root Compartment** ausgewählt ist.
    
    ![Zu Domains navigieren](images/default-domain.png)
    
4.  Wählen Sie _Benutzer_ aus. Klicken Sie auf _Benutzer erstellen_.
    
    ![Benutzer erstellen](images/select-users.png)
    
    ![Benutzer erstellen](images/create-user.png)
    
    Geben Sie die folgenden Details ein, um den _agcs-Benutzer_ zu erstellen
    
        First name: agcs
        Last name: user
        Use the email address as the username: Uncheck the checkbox 
        Username: agcs-user
        Email: agcsuser@example.com
        
    
    ![Benutzer erstellen](images/createuser-tab.png)
    
    Klicken Sie auf _Erstellen_.
    
    Der _agcs-Benutzer_ wurde erstellt.
    

## Aufgabe 2: AG-Gruppe erstellen

1.  Melden Sie sich bei der **Standarddomain** der OCI-Konsole als **Standarddomainadministrator** an
    
2.  Klicken Sie in der OCI-Konsole in der oberen linken Ecke auf das Symbol "Navigationsmenü", um das _Navigationsmenü anzuzeigen._ Klicken Sie im _Navigationsmenü_ auf _Identität und Sicherheit_. Wählen Sie _Domains_ aus der Produktliste.
    
    ![Zu Domains navigieren](images/navigate-select-domain.png)
    
3.  Klicken Sie auf der Seite "Domains" auf _Standarddomain_. Stellen Sie sicher, dass das **Root Compartment** ausgewählt ist. Wählen Sie _Gruppen_ aus. Klicken Sie auf _Gruppe erstellen_.
    
    ![Identitätsdomain auswählen](images/default-domain.png)
    
    ![Gruppen wählen](images/select-group.png)
    
    Geben Sie die folgenden Details ein, um den _agcs-group_\-Benutzer zu erstellen und der Gruppe den **agcs-user**\-Benutzer zuzuweisen
    
        Name: agcs-group
        Description: Access governance group to manage users 
        Users: Select the agcs-user 
        
    
    Klicken Sie auf _Erstellen_.
    
    ![AG-Gruppe erstellen](images/creategroup-tab.png)
    
    Die _Gruppe_ wurde erfolgreich erstellt.
    

## Aufgabe 3: AG-Policys erstellen

1.  Klicken Sie in der OCI-Konsole in der oberen linken Ecke auf das Symbol "Navigationsmenü", um das _Navigationsmenü anzuzeigen._ Klicken Sie im _Navigationsmenü_ auf _Identität und Sicherheit_. Wählen Sie _Policys_ aus der Produktliste.
    
2.  Klicken Sie auf der Seite "Policys" auf _Policy erstellen_, um die Policy "ag-access-policy" im Root Compartment zu erstellen.
    
        Name: ag-access-policy
        Description: IAM policy for granting Domain_Administrators access to manage access governance instances
        Compartment: Ensure your  root compartment is selected
        Policy Builder: Select the show manual editor checkbox
        Statement :
        
    
        <copy>Allow group ag-domain/Domain_Administrators to manage agcs-instance in compartment ag-compartment
        Allow group ag-domain/Domain_Administrators to read objectstorage-namespace in tenancy</copy>
        
    
    Klicken Sie auf _Erstellen_.
    
    Klicken Sie auf der Seite "Policys" im Root Compartment auf "Policy erstellen", um eine Policy zu erstellen: agcs-policy
    
        Name: agcs-policy
        Description: Oracle Access Governance policy 
        Compartment: Ensure your root compartment is selected
        Policy Builder: Select the show manual editor checkbox
        
    
        <copy>ALLOW GROUP agcs-group to read all-resources IN TENANCY
        ALLOW GROUP agcs-group to manage policies IN TENANCY
        ALLOW GROUP agcs-group to manage domains IN TENANCY
        </copy>
        
    
    Klicken Sie auf "Create".
    
    Klicken Sie auf der Seite "Policys" im Root Compartment auf "Policy erstellen", um die Policy zu erstellen: domain-admin-policy
    
        Name: domain-admin-policy
        Description: IAM policy (domain-admin-policy) in the COMPARTMENT to give access to the Identity Domain admin for the compartment created
        Compartment: Ensure your root compartment is selected
        Policy Builder: Select the show manual editor checkbox
        Statement :
        
    
        <copy>Allow group ag-domain/Domain_Administrators to manage all-resources in compartment ag-compartment</copy>
        
    
    Klicken Sie auf _Erstellen_.
    

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

*   [Oracle Access Governance - Zugriffsprüfungskampagne erstellen](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance - Produktseite](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governance - Produkttour](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access Governance - Häufig gestellte Fragen](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Danksagungen

*   **Autoren** - Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Zuletzt aktualisiert am/um** - Anbu Anbarasu, Juni 2023