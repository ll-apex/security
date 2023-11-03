# Gruppen und Policys für Access Governance erstellen

## Einführung

Policys für Access Governance erstellen.

*   Geschätzte Zeit: 15 Minuten
*   Persona: Standarddomainadministrator

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   **Policys** für den Domainadministratorzugriff einrichten

## Aufgabe 1: AG-Policys erstellen

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

## Bestätigungen

*   **Autoren** - Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Zuletzt aktualisiert am/um** - Anbu Anbarasu, Juni 2023