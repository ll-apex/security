# Compartment und Identitätsdomain erstellen

## Einführung

Compartment erstellen

*   Persona: Standarddomainadministrator

_Geschätzte Zeit_: 15 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Oracle Video Hub - Video ohne Größenanpassung](videohub:1_8rvbi8pv)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   **Compartment** erstellen
*   **Identitätsdomain** erstellen
*   Account aktivieren

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Ein gültiger Oracle OCI-Mandant mit OCI-Administratorberechtigungen.

## Aufgabe 1: Compartment erstellen

1.  Melden Sie sich bei OCI als **Standarddomainadministrator** an. Öffnen Sie das Hamburger-Menü oben links. Klicken Sie auf Identity and Security, und wählen Sie Identity > Compartments.
    
    ![Zu Compartments navigieren](images/navigate-comp.png)
    
2.  Klicken Sie auf _Compartment erstellen_
    
    ![Compartment erstellen auswählen](images/create-compartment.png)
    
3.  Geben Sie die Details der zu erstellenden Identitätsdomain ein. Klicken Sie auf _Compartment erstellen_
    
        Name: ag-compartment
        Description: Oracle Access Governance Compartment
        Parent Compartment: Ensure your root compartment is selected
        
    
    ![Neues Compartment erstellen](images/new-compartment.png)
    
4.  Sie haben jetzt das Compartment **ag-compartment** erstellt
    

## Aufgabe 2: Identitätsdomain erstellen

1.  Melden Sie sich bei der OCI-Konsole als **Standarddomainadministrator** an. Öffnen Sie das Hamburger-Menü oben links. Klicken Sie auf Identität und Sicherheit, und wählen Sie Identität > Domains.
    
    ![Zu Domains navigieren](images/navigate-to-domains.png)
    
2.  Wählen Sie das _ag-compartment_ aus, in dem Sie die Identitätsdomain erstellen. Klicken Sie auf _Domain erstellen_.
    
    ![Identitätsdomain erstellen](images/create-domains.png)
    
3.  Geben Sie die Details der zu erstellenden Identitätsdomain ein. Klicken Sie auf _Erstellen_.
    
        Display Name: ag-domain
        Description: Oracle Access Governance Identity Domain
        Domaintype: Free
        Domain Administrator: Select the checkbox for Create an administrative user for this domain 
        Administrator first name: Enter administrator  first name 
        Administrator last name: Enter administrator last name 
        Administrator username/email: Enter the administrator email id
        Compartment: Ensure ag-compartment compartment is selected
        
    
    ![Klicken Sie auf "Create Identity Domain".](images/click-create-domain.png)
    
    ![Erstellung der Identitätsdomain abschließen](images/complete-creation-domain.png)
    
4.  Sie haben jetzt die Identitätsdomain erstellt.
    
    ![Aktive Identitätsdomain](images/active-identity-domain.png)
    

## Aufgabe 3: Account aktivieren

1.  Gehen Sie zu Ihrer Administrator-E-Mail, und klicken Sie auf **Account aktivieren**
    
    ![E-Mail aktivieren](images/activate-email.png)
    
2.  Geben Sie das Kennwort auf dem nächsten Bildschirm ein, und leiten Sie es weiter
    
    Sie können jetzt **mit der nächsten Übung fortfahren.**
    

## Weitere Informationen

*   [Oracle Access Governance - Zugriffsprüfungskampagne erstellen](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance - Produktseite](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governance - Produkttour](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access Governance - Häufig gestellte Fragen](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Danksagungen

*   **Autoren** - Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu