# OCI-Policys, -VCN, -Gruppen und -Compartments erstellen

## Einführung

Als Benutzer mit der Rolle **Identitätsdomainadministrator** in der Identitätsdomain können Sie OCI-Policys, -Gruppen und -Compartments erstellen. OCI-IP-Benutzer in der Übung **OCI** console.This zeigen Ihnen, wie Sie die OCI-Policys, das VCN, die Gruppen und die Compartments einrichten, die zur Ausführung dieser OCI-IAM-Policy-Prüfungen erforderlich sind.

*   Persona: Identitätsdomainadministrator

_Geschätzte Zeit_: 15 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Oracle Video Hub - Video ohne Größenanpassung](videohub:1_wabc1y93)

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

## Aufgabe 1: Benutzer in OCI IAM erstellen

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
    
        First Name: Jerry
        Last Name: Poland
        Username: jerry.poland
        Email: Specify unique email-id to which you will be receiving activation mail for password reset for the user. 
        
    
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

## Danksagungen

*   **Autoren** - Anuj Tripathi, Anbu Anbarasu
*   **Zuletzt aktualisiert am/um** - Anuj Tripathi, Oktober 2023