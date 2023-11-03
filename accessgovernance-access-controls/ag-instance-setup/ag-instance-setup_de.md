# Oracle Access Governance-Serviceinstanz einrichten und konfigurieren

## Einführung

In dieser Übung richten wir die OAG-Serviceinstanz ein und machen Konfigurationen erforderlich, um diesen Workshop erfolgreich auszuführen.

Persona: Identitätsdomainadministrator

_Geschätzte Zeit_: 30 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Oracle Video Hub - Video ohne Größenanpassung](videohub:1_21nk0xhx)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   AG-Serviceinstanz erstellen
*   URL der AG-Konsole aufrufen
*   AG-Rollen Benutzern in OCI IAM zuweisen

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
    

## Aufgabe 2: Benutzern AG-Anwendungsrollen zuweisen

1.  Melden Sie sich bei der Identitätsdomain der OCI-Konsole an: ag-domain als Identitätsdomainadministrator.
    
2.  Navigieren Sie in der OCI-Konsole zu "Identität -> Domains -> ag-Domain -> Oracle Cloud Services -> AG-Service-Instanz -> Anwendungsrolle".
    
    *   Beachten Sie die Rolle _AG-Administrator_, und klicken Sie in der rechten Ecke auf den Pfeil nach unten.
    
    ![OIG-Identitätsrollen und -Zugriffs-Policys](images/user-approle.png)
    
    *   Click on _Assigned Users -> Manage_. Select _Pamela Green_ in _Available Users._ Click on _Assign_
    
    ![OIG-Identitätsrollen und -Zugriffs-Policys](images/user-approle-list.png)
    
    *   Der Benutzer Pamela Green ist jetzt unter _Zugewiesene Benutzer_ sichtbar.
    
    ![OIG-Identitätsrollen und -Zugriffs-Policys](images/user-approle-assign.png)
    
    *   Pamela Green wurde die Anwendungsrolle _AG-Administrator_ zugewiesen. Sie können das Fenster jetzt schließen.
3.  Beachten Sie jetzt die _AG-Benutzerrolle_ und die _AG CloudAccessReviewer_\-Rolle aufgeführt. Weisen Sie jedem Benutzer diese beiden Rollen zu: _Mark Hernandez_, _Harlan Bullard_ und _Jerry Poland_.
    
    ![OIG-Identitätsrollen und -Zugriffs-Policys](images/aguser.png)
    
    ![OIG-Identitätsrollen und -Zugriffs-Policys](images/agreviewer.png)
    
    *   Click on _Assigned Users -> Manage_. Select _Mark Hernandez_, _Harlan Bullard_ and _Jerry Poland_ in _Available Users._ Click on _Assign_. The Users appear under _Assigned Users_ :
    
    ![OIG-Identitätsrollen und -Zugriffs-Policys](images/ag-userassign.png)
    
    ![OIG-Identitätsrollen und -Zugriffs-Policys](images/ag-reviewerassign.png)
    
    *   Mark Hernandez, Harlan Bullard und Jerry Poland wurden jetzt mit den Anwendungsrollen _AG User_ und _AG CloudAccessReviewer_ zugewiesen. Sie können das Fenster jetzt schließen.
    
    Sie können jetzt **mit der nächsten Übung fortfahren.**
    

## Weitere Informationen

*   [Oracle Access Governance - Zugriffsprüfungskampagne erstellen](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance - Produktseite](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governance - Produkttour](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access Governance - Häufig gestellte Fragen](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Bestätigungen

*   **Autoren** - Anuj Tripathi, Anbu Anbarasu
*   **Zuletzt aktualisiert am/um** - Anuj Tripathi, Oktober 2023