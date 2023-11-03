# Oracle Access Governance-Serviceinstanz einrichten und konfigurieren und Benutzer in OCI IAM erstellen

## Einführung

In dieser Übung richten wir die OAG-Serviceinstanz ein und machen Konfigurationen erforderlich, um diesen Workshop erfolgreich auszuführen.

_Geschätzte Laborzeit_: 30 Minuten

_Persona_: Identitätsdomainadministrator

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   AG-Serviceinstanz erstellen
*   URL der AG-Konsole aufrufen
*   Benutzer in OCI IAM erstellen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Ein gültiger Oracle OCI-Mandant mit OCI-Administratorberechtigungen.
*   Wählen Sie eine Region aus, in der Access Governance verfügbar ist.

## Aufgabe 1: AG-Serviceinstanz erstellen

Melden Sie sich bei der OCI-Konsole mit der Identitätsdomain an: ag-domain als **Identitätsdomainadministrator**, wenn sie derzeit nicht bei der Identitätsdomain angemeldet ist.

1.  Klicken Sie in der OCI-Konsole oben links auf das Symbol "Navigationsmenü", um das _Navigationsmenü anzuzeigen._ Klicken Sie im _Navigationsmenü_ auf _Identität und Sicherheit_. Wählen Sie _Access Governance_ aus der Produktliste aus. Wenn die Menüoption nicht angezeigt wird, prüfen Sie die ausgewählte Region, und stellen Sie sicher, dass Access Governance in dieser Region verfügbar ist.
    
    ![Instanz erstellen](images/oci-console.png)
    
2.  Wählen Sie auf der Seite "Access Governance" die Option _Serviceinstanzen_ aus.
    
        Name: ag-service-instance
        Description: Oracle Access Governance service instance
        Compartment: Ensure your ag-compartment is selected
        
    
    ![Instanz erstellen](images/create-service-instance.png)
    
3.  Wählen Sie den Lizenztyp aus: Access Governance für Oracle Workloads. Klicken Sie auf _Serviceinstanz erstellen_.
    
    ![Lizenztyp auswählen](images/license-type.png)
    
4.  Warten Sie, bis die Serviceinstanz den Status _Aktiv_ aufweist. Notieren Sie sich diese URL, da Sie sie in den weiteren Übungen verwenden werden.
    
    ![Serviceinstanz ist aktiv](images/ag-url.png)
    
5.  Klicken Sie auf die Serviceinstanz, um auf die URL zuzugreifen.
    
    ![Access Governance-Konsole](images/ag-console.png)
    

## Aufgabe 2: Benutzer in OCI IAM erstellen

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
        
7.  Melden Sie sich bei der Identitätsdomain der OCI-Konsole an: ag-domain als Identitätsdomainadministrator.
    
    *   Navigieren Sie in der OCI-Konsole zu "Identität -> Domains -> ag-Domain -> Oracle Cloud Services -> AG-Service-Instanz -> Anwendungsrolle".
        
    *   Beachten Sie, dass die Rolle _AG-Administrator_ und die Rolle _AG-Kampagnenadministrator_ aufgeführt sind. Klicken Sie auf den Abwärtspfeil in der rechten Ecke für jeden von ihnen.
        
    
    ![OIG-Identitätsrollen und -Zugriffs-Policys](images/user-approle.png)
    
    *   Click on _Assigned Users -> Manage_. Select _Pamela Green_ in _Available Users._ Click on _Assign_
    
    ![OIG-Identitätsrollen und -Zugriffs-Policys](images/user-approle-list.png)
    
    *   Der Benutzer Pamela Green ist jetzt unter _Zugewiesene Benutzer_ sichtbar.
    
    ![OIG-Identitätsrollen und -Zugriffs-Policys](images/user-approle-assign.png)
    
    *   Pamela Green wurde die Anwendungsrolle _AG-Administrator_ und _AG-Kampagnenadministrator_ zugewiesen. Sie können das Fenster jetzt schließen.
        
    *   Beachten Sie jetzt die _AG-Benutzerrolle_ und die _AG CloudAccessReviewer_\-Rolle aufgeführt. Klicken Sie auf den Abwärtspfeil in der rechten Ecke für jeden von ihnen.
        
        ![OIG-Identitätsrollen und -Zugriffs-Policys](images/aguser.png)
        
        ![OIG-Identitätsrollen und -Zugriffs-Policys](images/agreviewer.png)
        
    *   Click on _Assigned Users -> Manage_. Select _Mark Hernandez_ and _Harlan Bullard_ in _Available Users._ Click on _Assign_
        
    
    ![OIG-Identitätsrollen und -Zugriffs-Policys](images/ag-userassign.png)
    
    ![OIG-Identitätsrollen und -Zugriffs-Policys](images/ag-reviewerassign.png)
    
    *   Mark Hernandez und Harlan Bullard wurden jetzt mit der Anwendungsrolle _AG-Benutzer_ und _AG CloudAccessReviewer_ zugewiesen. Sie können das Fenster jetzt schließen.
    
    Sie können jetzt **mit der nächsten Übung fortfahren.**
    

## Weitere Informationen

*   [Oracle Access Governance - Zugriffsprüfungskampagne erstellen](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance - Produktseite](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governance - Produkttour](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access Governance - Häufig gestellte Fragen](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Bestätigungen

*   **Autoren** - Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Mitwirkende** - Edward Lu
*   **Zuletzt aktualisiert am/um** - Anbu Anbarasu, Mai 2023