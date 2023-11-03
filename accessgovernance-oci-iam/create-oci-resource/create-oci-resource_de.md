# OCI-Policys, -VCN, -Gruppen und -Compartments erstellen

## Einführung

Als Benutzer mit der Rolle **Identitätsdomainadministrator** in der Identitätsdomain können Sie OCI-Policys, -Gruppen und -Compartments erstellen. OCI-IP-Benutzer in der Übung **OCI** console.This zeigen Ihnen, wie Sie die OCI-Policys, das VCN, die Gruppen und die Compartments einrichten, die zur Ausführung dieser OCI-IAM-Policy-Prüfungen erforderlich sind.

*   Geschätzte Zeit: 15 Minuten
*   Persona: Identitätsdomainadministrator

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   OCI-Policys, -VCN, -Gruppen und -Compartments sowie OCI-IAM-Benutzer manuell erstellen
*   Hinweis: Alle in dieser Übung erstellten Ressourcen müssen im **ag-compartment** erstellt werden.
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
| Policys | Auditoren-Politik | Zugriffs-Policy für Auditoren |
|  | network-admins-policy | Zugriffs-Policy für Netzwerkadministratoren |
|  | Sicherheits-Admins-Policy | Zugriffs-Policy für Sicherheitsadministratoren |
| Virtual Cloud Network | ag-vcn | AG Virtuelles Cloud-Netzwerk testen |

## Aufgabe 1: Compartments erstellen

1.  Melden Sie sich bei der OCI-Konsolenidentitätsdomain an: ag-Domain als **Identitätsdomainadministrator**

![Bei OCI-Konsole anmelden](images/oci-console.png)

2.  Klicken Sie in der OCI-Konsole in der oberen linken Ecke auf das Symbol "Navigationsmenü", um das Navigationsmenü anzuzeigen. Klicken Sie im Navigationsmenü auf Identität und Sicherheit. Wählen Sie "Compartments" aus der Produktliste aus.

![Naviagte zu Compartment](images/navigate-compartment.png)

3.  Klicken Sie auf _Compartment erstellen._ Geben Sie die folgenden Details an, um die 3 Compartments zu erstellen: **Entwicklung, Qualitätssicherung und Tests**
    
    ![Compartment](images/create-compartment.png)
    

**Name:** Entwicklung

**Beschreibung:** Entwicklung

**Übergeordnetes Compartment:** Wählen Sie das Compartment **ag-compartment** aus.

Klicken Sie auf _Compartment erstellen_

![Compartment erstellen](images/create-compartment-tab.png)

**Name:** Qualitätssicherung

**Beschreibung:** Qualitätssicherung

**Übergeordnetes Compartment:** Wählen Sie das Compartment **ag-compartment** aus.

Klicken Sie auf _Compartment erstellen_

![Compartment erstellen](images/qa-compartment.png)

**Name:** Tests

**Beschreibung:** Tests

**Übergeordnetes Compartment:** Wählen Sie das Compartment **ag-compartment** aus.

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
        Compartment: Ensure ag-compartment is selected
        Policy Builder: Select the show manual editor checkbox
        
        
    
        <copy>Allow group ag-domain/Auditors to read instances in compartment ag-compartment
        Allow group ag-domain/Auditors to read audit-events in compartment ag-compartment
        Allow group ag-domain/Auditors to inspect vaults in compartment ag-compartment
        Allow group ag-domain/Auditors to inspect virtual-network-family in compartment ag-compartment
        Allow group ag-domain/Auditors to inspect keys in compartment ag-compartment
        Allow group ag-domain/Auditors to inspect vaults in compartment ag-compartment
        Allow group ag-domain/Auditors to inspect virtual-network-family in compartment ag-compartment
        Allow group ag-domain/Auditors to inspect keys in compartment Quality-Assurance
        Allow group ag-domain/Auditors to inspect vaults in compartment Development
        Allow group ag-domain/Auditors to inspect virtual-network-family in compartment Testing
        </copy>
        
    
    Klicken Sie auf _Erstellen_.
    
        Name: network_admins_policy
        
        Description: Access Policy for Network Administrators
        
        Compartment: Ensure ag-compartment is selected
        
        Policy Builder: Select the show manual editor checkbox
        
        
    
        <copy>Allow group ag-domain/NetworkAdmins to manage all-resources in compartment ag-compartment</copy>
        
    
    Klicken Sie auf _Erstellen_.
    
        Name: security_admins_policy
        Description: Access Policy for Security Admins
        Compartment: Ensure ag-compartment is selected
        Policy Builder: Select the show manual editor checkbox
        
        
    
        <copy>Allow group ag-domain/SecurityAdmins to manage virtual-network-family in compartment ag-compartment
        Allow group ag-domain/SecurityAdmins to manage vaults in compartment ag-compartment
        Allow group ag-domain/SecurityAdmins to manage secret-family in compartment ag-compartment
        Allow group ag-domain/SecurityAdmins to manage keys in compartment ag-compartment
        Allow group ag-domain/SecurityAdmins to inspect work-requests in compartment Quality-Assurance
        Allow group ag-domain/SecurityAdmins to manage keys in compartment Development
        Allow group ag-domain/SecurityAdmins to manage bastion in compartment Testing	
        </copy>
        
    
    Klicken Sie auf _Erstellen_.
    

Die _Policys_ wurden erfolgreich erstellt.

## Aufgabe 5: VCN erstellen

1.  Navigieren Sie zu "Networking" -> "Virtuelle Cloud-Netzwerke".
    
    ![Zu VCN navigieren](images/navigate-vcn.png)
    
2.  Stellen Sie sicher, dass das **ag-compartment** ausgewählt ist. Klicken Sie auf **VCN-Assistenten starten**
    

![Zu VCN navigieren](images/start-wizard.png)

3.  Aktivieren Sie das Kontrollkästchen **VCN mit Internetverbindung erstellen**. Klicken Sie auf **VCN-Assistenten starten**.

![Zu VCN navigieren](images/wizard-starts.png)

4.  Geben Sie unter Konfiguration die folgenden Details an:

**VCN-Name:** ag-VCN

**Compartment:** Wählen Sie das Ag-Compartment

    ![Navigate to VCN](images/enter-vcn-details.png)
    

5.  Klicken Sie auf **Weiter**
    
    ![Zu VCN navigieren](images/click-next-wizard.png)
    
6.  Prüfen Sie alle Details. Klicken Sie auf **Erstellen**.
    
    ![Zu VCN navigieren](images/click-create-wizard.png)
    
    Das _VCN_ wurde erfolgreich erstellt.
    

## Aufgabe 6: Benutzer in OCI IAM erstellen

1.  Klicken Sie in der oberen linken Ecke auf das Symbol "Navigationsmenü", um das Menü "Navigation" anzuzeigen. Klicken Sie im Navigationsmenü auf Identität und Sicherheit. Wählen Sie Domains aus der Produktliste.
    
    ![Zu Domains navigieren](images/navigate-select-domain.png)
    
2.  Klicken Sie auf der Seite "Domains" auf die von Ihnen erstellte Identitätsdomain: _ag-domain_.
    
    ![Zu Identitätsdomains navigieren](images/open-domains.png)
    
    Wählen Sie _Benutzer_ aus. Klicken Sie auf _Benutzer erstellen_.
    
    ![Zu Benutzern navigieren](images/navigate-to-users.png)
    
3.  Deaktivieren Sie "Verwenden Sie die E-Mail-Adresse als Benutzernamen"
    
4.  Geben Sie die folgenden Details ein, um 3 Benutzer zu erstellen: Pamela Green (Kampagnenadministrator), Harlan Bullard (Manager), Mark Hernandez (Mitarbeiterbenutzer) in IAM. Stellen Sie sicher, dass Sie verschiedene E-Mail-IDs für verschiedene Benutzer verwenden.
    
        First Name: Pamela
        Last Name: Green
        Username: pamela.green
        Email: Specify unique email-id to which you will be receiving activation mail for password reset for the user. 
        
    
    ![Benutzer erstellen](images/user-create-pamela.png)
    
    Klicken Sie auf _Erstellen_.
    
        First Name: Harlan
        Last Name: Bullard
        Username: harlan.bullard
        Email: Specify unique email-id to which you will be receiving activation mail for password reset for the user. 
        
    
    ![Benutzer erstellen](images/user-create-harlan.png)
    
    Klicken Sie auf _Erstellen_.
    
        First Name: Mark
        Last Name: Hernandez
        Username: mhernandez
        Email: Specify unique email-id to which you will be receiving activation mail for password reset for the user. 
        
    
    ![Benutzer erstellen](images/user-create-mark.png)
    
    Klicken Sie auf _Erstellen_.
    
5.  Melden Sie sich von der Cloud-Konsole ab.
    
6.  Für jeden erstellten Benutzer wird eine Aktivierungs-E-Mail an die E-Mail-ID gesendet, die in _Aufgabe 3: Schritt 4_ angegeben wird. Setzen Sie das Kennwort für die 3 Benutzer mit der _Aktivierungs-E-Mail_ zurück, die für jeden Benutzer eingegangen ist. Passwort auf das unten genannte Passwort zurücksetzen:
    
    **Passwort:**
    
        <copy>Oracl@123456</copy>
        

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