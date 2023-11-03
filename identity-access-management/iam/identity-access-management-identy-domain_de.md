# Identity and Access Management

## Einführung

Mit Oracle Cloud Infrastructure (OCI) Identity and Access Management (IAM) Service können Sie den Zugriff auf Ihre Cloud-Ressourcen kontrollieren. Sie steuern die Zugriffsarten für eine Benutzergruppe und für bestimmte Ressourcen. Im November wurden OCI IAM und Oracle IDCS unter Einbeziehung von Identitätsdomains zu einem einzigen Cloud-Service vereinheitlicht.

In dieser Übung erhalten Sie einen Überblick über die IAM-Servicekomponenten mit Identidy-Domains und ein Beispielszenario, damit Sie verstehen können, wie sie zusammenarbeiten.

Geschätzte Zeit: 30 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Compartment erstellen
*   Benutzer erstellen
*   Gruppe erstellen
*   Eine mit der Gruppe verknüpfte Policy erstellen
*   Benutzer zur Gruppe hinzufügen

### Voraussetzungen

*   Ein Oracle Cloud-Account - Auf der Landingpage LiveLabs dieses Workshops können Sie sehen, welche Umgebungen unterstützt werden.

> **Hinweis:** Wenn Sie über einen **kostenlosen Testaccount** verfügen, wird Ihr Account nach Ablauf der kostenlosen Testversion in einen Account vom Typ "**Immer kostenlos**" konvertiert. Sie können keine Free Tier-Workshops durchführen, es sei denn, die Umgebung vom Typ "Immer kostenlos" ist verfügbar.

**[Klicken Sie hier, um die FAQ-Seite zu Free Tier aufzurufen.](https://www.oracle.com/cloud/free/faq.html)**

## Aufgabe 1: Compartments erstellen

Ein Compartment ist eine Sammlung von Cloud-Assets, wie Compute-Instanzen, Load Balancer, Datenbanken usw. Standardmäßig wurde ein Root Compartment für Sie erstellt, als Sie Ihren Mandanten erstellt haben (d.h. als Sie sich für den Testaccount registriert haben). Es ist möglich, alle Elemente im Root Compartment zu erstellen. Oracle empfiehlt jedoch, Sub-Compartments zu erstellen, um Ihre Ressourcen effizienter zu verwalten.

1.  Klicken Sie oben links auf das **Navigationsmenü**, navigieren Sie zu **Identität und Sicherheit**, und wählen Sie **Compartments** aus.

![](https://oracle-livelabs.github.io/common/images/console/id-compartment.png " ")

1.  Klicken Sie auf **Compartment erstellen**. ![Compartment erstellen](images/create-compartment.png)
    
2.  Benennen Sie das Compartment **Demo**, und geben Sie eine kurze Beschreibung an. Stellen Sie sicher, dass das Root Compartment als übergeordnetes Compartment angezeigt wird. Klicken Sie auf die blaue Schaltfläche **Compartment erstellen**, wenn Sie bereit sind.
    
    ![Compartment erstellen](images/compartment-details.png)
    
3.  Sie haben gerade ein Compartment für Ihre gesamte Arbeit in diesem Testlauf erstellt.
    

## Aufgabe 2: Benutzer, Gruppen und Policys zur Kontrolle des Zugriffs verwalten

Die Berechtigungen eines Benutzers für den Zugriff auf Services stammen aus den _Gruppen_, zu denen er gehört. Die Berechtigungen für eine Gruppe werden durch Policys definiert. Policys definieren, welche Aktionen Mitglieder einer Gruppe ausführen können und in welchen Compartments. Benutzer können auf Services zugreifen und Vorgänge basierend auf den Policys ausführen, die für die Gruppen festgelegt sind, deren Mitglieder sie sind.

Wir erstellen einen Benutzer, eine Gruppe und eine Sicherheitsrichtlinie, um das Konzept zu verstehen.

Im Jahr 2022 hat OCI IAM Identitätsdomains eingeführt. Eine Identitätsdomain ist ein Container für die Verwaltung von Benutzern und Rollen, die Föderation und das Provisioning von Benutzern, die sichere Anwendungsintegration über Oracle Single Sign-On-(SSO-)Konfiguration und die OAuth-Administration.

1.  Klicken Sie oben links auf das **Navigationsmenü**. Navigieren Sie zu **Identität und Sicherheit**, und wählen Sie **Domains** aus.

Bei IAM mit Identitätsdomains befindet sich die zuvor als IAM-Benutzer und -Gruppen identifizierte Domain jetzt unter der Standarddomain.

![](images/id-domains.png)

1.  Standarddomain auswählen
    
    ![](images/id-domains-default.png)
    
2.  **Gruppen** auswählen
    
    ![](images/id-domains-groups.png)
    
3.  Klicken Sie auf **Gruppe erstellen**.
    
    Geben Sie im Dialogfeld **Gruppe erstellen** Folgendes ein:
    
    *   **Name:** Geben Sie einen eindeutigen Namen für die Gruppe ein, wie **oci-group**
        
        > **Hinweis:** Der Gruppenname darf keine Leerzeichen enthalten.
        
    *   **Beschreibung:** Geben Sie eine Beschreibung ein. Beispiel: **Neue Gruppe für OCI-Benutzer**
        
    *   Klicken Sie auf **Erstellen**.
        
    
    ![Gruppe erstellen](images/id-domains-create-group.png)
    
4.  Klicken Sie auf Ihre neue Gruppe, um sie anzuzeigen. Ihre neue Gruppe wird angezeigt.
    
    ![Neue Gruppe wird angezeigt](images/id-domains-group-detail.png)
    
5.  Neuen Benutzer erstellen
    
    a) Klicken Sie im Navigationspfad auf **Standarddomain**.
    
    ![Standarddomain im Brotkrümel auswählen](images/id-domains-bc-default-domain.png)
    
    Sie können auch oben links auf das **Navigationsmenü** klicken, zu **Identität und Sicherheit** navigieren und **Domains** auswählen, die Standarddomain auswählen und dann zu **Benutzer** gehen.
    
    b) Wählen Sie **Benutzer** aus.
    
    ![Benutzer auswählen](images/id-domains-users.png)
    
    c) Klicken Sie auf **Benutzer erstellen**.
    
    Geben Sie im Dialogfeld **Benutzer erstellen** Folgendes ein:
    
    *   **Fisrt Name**: Ihr Vorname
    *   **Last NameName**: Ihr Nachname.
    *   **E-Mail:** Verwenden Sie vorzugsweise eine persönliche E-Mail-Adresse, auf die Sie Zugriff haben (GMail, Yahoo usw.) und die sich von allen E-Mails unterscheidet, die bereits im Mandanten verwendet werden.
    *   **Verwenden Sie die E-Mail-Adresse als Benutzernamen:** Lassen Sie diese Option aktiviert, es sei denn, Sie möchten einen Benutzernamen verwenden, der nicht die E-Mail-Adresse ist. Sie kann verwendet werden, wenn Sie dieselbe E-Mail verwenden möchten, die bereits im Mandanten verwendet wird.
    *   **Cloud-Accountadministratorrolle zuweisen:** Lassen Sie das Kontrollkästchen deaktiviert.
    *   Aktivieren Sie das Kontrollkästchen neben **oci-group**
    
    Klicken Sie auf **Erstellen**.
    
    ![Neues Benutzerformular](images/id-domains-create-user.png)
    
    Nachdem Sie den Benutzer erstellt haben, werden Sie zu den Benutzerdetails weitergeleitet.
    
    Der neu erstellte Benutzer erhält eine E-Mail mit einem Aktivierungslink wie diesem:
    
    ![Aktivierungs-E-Mail](images/id-domains-activation-message.png)
    
6.  Wenn der Benutzer die E-Mail nicht erhalten hat, haben Sie in den Benutzerdetails eine Schaltfläche zum Zurücksetzen des Kennworts, mit der ein Link zum Zurücksetzen des Kennworts gesendet wird.
    
    ![Kennwort zurücksetzen](images/id-domains-user-resetpw.png)
    
    Nachdem Sie auf die Schaltfläche "Zurücksetzen" geklickt haben, werden Sie zur Bestätigung aufgefordert, bevor der Link zum Zurücksetzen gesendet wird.
    
7.  Überprüfen Sie die Messges in dem E-Mail-Konto, das Sie für den neuen Benutzer verwendet haben. Öffnen Sie den Aktivierungslink (das Zurücksetzen des Passworts erfolgt auf einen ähnlichen Bildschirm)
    
    ![Kennwort zurücksetzen](images/id-domains-resetpw.png)
    
8.  Jetzt erstellen wir eine Sicherheits-Policy, die Ihrer Gruppe Berechtigungen im zugewiesenen Compartment erteilt. Beispiel: Erstellen Sie eine Policy, die Mitgliedern der Gruppe **oci-group** im Compartment **Demo** die Berechtigung erteilt:
    
    a) Klicken Sie oben links auf das **Navigationsmenü**. Navigieren Sie zu **Identität und Sicherheit**, und wählen Sie **Policys** aus.
    
    ![IAM-Policy](images/iam-policies.png)
    
    b) Wählen Sie auf der linken Seite das Compartment **Demo** aus.
    
    ![*Demo-Compartment auswählen](images/id-domains-demo-compartment.png)
    
    > **Hinweis:** Möglicherweise müssen Sie auf das Pluszeichen neben Ihrem Haupt-Compartment-Namen klicken, um die _**Demo**_ des Unter-Compartments anzuzeigen. Wenn Sie dies tun und das Unter-Compartment immer noch nicht angezeigt wird, _**aktualisieren Sie Ihren Browser**_. Manchmal speichert der Browser die Compartment-Informationen im Cache und aktualisiert den internen Cache nicht.
    
    c) Nachdem Sie das Compartment **Demo** ausgewählt haben, klicken Sie auf **Policy erstellen**.
    
    ![](images/id-domain-create-policy.png)
    
    d) Geben Sie einen eindeutigen **Namen** für die Policy ein (Beispiel: "Policy-for-oci-group").
    
    > **Hinweis:** Der Name darf keine Leerzeichen enthalten.
    
    e) Geben Sie eine **Beschreibung** ein (Beispiel: "Policy für OCI-Gruppe").
    
    f) Wählen Sie **Demo** für Compartment aus.
    
    g) Klicken Sie auf **Manuellen Editor anzeigen**, und geben Sie die folgende **Anweisung** ein:
    
        <copy>Allow group default/oci-group to manage all-resources in compartment Demo</copy>
        
    
    Hinweis: Wenn Sie _identity\_domain\_name_ nicht vor _group\_name_ aufnehmen, wird die Policy-Anweisung ausgewertet, als ob die Gruppe zur Standardidentitätsdomain gehört.
    
    h) Klicken Sie auf **Erstellen**.
    
    ![Erstellen](images/create-policy.png)
    
9.  Prüfen Sie die Benutzerberechtigungen.
    
    a) Klicken Sie oben links auf das **Navigationsmenü**. Klicken Sie auf **Compute**, und klicken Sie dann auf **Instanzen**.
    
    ![](https://oracle-livelabs.github.io/common/images/console/compute-instances.png " ")
    
    b) Versuchen Sie, ein beliebiges Compartment im linken Menü auszuwählen.
    
    c) Die Meldung "**Sie haben keine Berechtigung zum Anzeigen dieser Ressourcen**" wird angezeigt. Dies ist normal, da Sie den Benutzer nicht der Gruppe hinzugefügt haben, der Sie die Policy zugeordnet haben. ![Fehlermeldung kann ignoriert werden](images/no-permission.png)
    
    d) Melden Sie sich von der Konsole ab.
    
10.  Benutzer zu einer Gruppe hinzufügen.
    
    a) Melden Sie sich mit dem _**admin**_\-Konto wieder an.
    
    b) Klicken Sie oben links auf das **Navigationsmenü**. Navigieren Sie zu **Identität und Sicherheit**, und wählen Sie **Benutzer** aus. Klicken Sie in der Liste **Benutzer** auf den gerade erstellten Benutzeraccount (z.B. `User01`), um zur Seite "Benutzerdetails" zu gelangen. ![](https://oracle-livelabs.github.io/common/images/console/id-users.png " ")
    
    c) Klicken Sie links im Menü **Ressourcen** auf **Gruppen**, falls diese noch nicht ausgewählt ist.
    
    d) Klicken Sie auf **Benutzer zu Gruppe hinzufügen**. ![](images/image020.png)
    
    e) Wählen Sie in der Dropdown-Liste **Gruppen** die von Ihnen erstellte **oci-group** aus.
    
    f) Klicken Sie auf **Hinzufügen**. ![Klicken Sie auf die Schaltfläche "Add (Hinzufügen)".](images/add-user-to-group.png)
    
    g) Bei der Oracle Cloud-Website abmelden.
    
11.  Prüfen Sie die Benutzerberechtigungen, wenn ein Benutzer zu einer bestimmten Gruppe gehört.
    
    a) Melden Sie sich mit dem von Ihnen erstellten lokalen **User01**\-Account an. Denken Sie daran, das letzte Passwort zu verwenden, das Sie diesem Benutzer zugewiesen haben.
    
    b) Klicken Sie auf das **Navigationsmenü**. Klicken Sie auf **Compute** und dann auf **Instanzen**.
    
    c) Wählen Sie in der Liste der Compartments auf der linken Seite die Compartment-**Demo** aus.
    
    ![Demo auswählen](images/select-demo.png)
    
    d) Es gibt keine Meldung zu Berechtigungen, und Sie können neue Instanzen erstellen.
    
    e) Klicken Sie auf das **Navigationsmenü**. Klicken Sie auf **Identität und Sicherheit**, und wählen Sie **Gruppen** aus.
    
    f) Die Meldung **"Autorisierung fehlgeschlagen oder angeforderte Ressource nicht gefunden"** wird angezeigt. Dies wird erwartet, da Ihr Benutzer keine Berechtigung zum Ändern von Gruppen hat.
    
    > **Hinweis:** Stattdessen wird die Meldung "Ein unerwarteter Fehler ist aufgetreten" angezeigt. Das ist auch in Ordnung.
    
    ![](images/group-error.png)
    
    g) Melden Sie sich ab.
    

_Glückwunsch! Sie haben die Übung erfolgreich abgeschlossen._

## Danksagungen

*   **Autor** - Orlando Gentil
*   **Zuletzt aktualisiert am/um** - Orlando Gentil, März 2022