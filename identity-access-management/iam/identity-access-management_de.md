# Identity and Access Management

## Einführung

Mit Oracle Cloud Infrastructure (OCI) Identity and Access Management (IAM) Service können Sie kontrollieren, wer Zugriff auf Ihre Cloud-Ressourcen hat. Sie steuern die Zugriffstypen einer Benutzergruppe und die spezifischen Ressourcen. In dieser Übung erhalten Sie einen Überblick über die IAM Service-Komponenten und ein Beispielszenario, mit dem Sie verstehen, wie sie zusammenarbeiten.

Sehen Sie sich das Video unten an, um einen Überblick über das Labor zu erhalten.[](youtube:KahdJmhJlYI)

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
    
    Klicken Sie auf **Compartment erstellen**. ![Compartment erstellen](images/create-compartment.png)
    
2.  Benennen Sie das Compartment **Demo**, und geben Sie eine kurze Beschreibung an. Stellen Sie sicher, dass das Root Compartment als übergeordnetes Compartment angezeigt wird. Klicken Sie auf die blaue Schaltfläche **Compartment erstellen**, wenn Sie bereit sind.
    
    ![Compartment erstellen](images/compartment-details.png)
    
3.  Sie haben gerade ein Compartment für Ihre gesamte Arbeit in diesem Testlauf erstellt.
    

## Aufgabe 2: Benutzer, Gruppen und Policys zur Kontrolle des Zugriffs verwalten

Die Berechtigungen eines Benutzers für den Zugriff auf Services stammen aus den _Gruppen_, zu denen er gehört. Die Berechtigungen für eine Gruppe werden durch Policys definiert. Policys definieren, welche Aktionen Mitglieder einer Gruppe ausführen können und in welchen Compartments. Benutzer können auf Services zugreifen und Vorgänge basierend auf den Policys ausführen, die für die Gruppen festgelegt sind, deren Mitglieder sie sind.

Wir erstellen einen Benutzer, eine Gruppe und eine Sicherheitsrichtlinie, um das Konzept zu verstehen.

1.  Klicken Sie oben links auf das **Navigationsmenü**. Navigieren Sie zu **Identität und Sicherheit**, und wählen Sie **Gruppen** aus.
    
    ![](https://oracle-livelabs.github.io/common/images/console/id-groups.png " ")
    
2.  Klicken Sie auf **Gruppe erstellen**.
    
    Geben Sie im Dialogfeld **Gruppe erstellen** Folgendes ein:
    
    *   **Name:** Geben Sie einen eindeutigen Namen für die Gruppe ein, wie **oci-group**
        
        > **Hinweis:** Der Gruppenname darf keine Leerzeichen enthalten.
        
    *   **Beschreibung:** Geben Sie eine Beschreibung ein. Beispiel: **Neue Gruppe für OCI-Benutzer**
        
    *   Klicken Sie auf **Erstellen**.
        
    
    ![Gruppe erstellen](images/create-group.png)
    
3.  Klicken Sie auf Ihre neue Gruppe, um sie anzuzeigen. Ihre neue Gruppe wird angezeigt.
    
    ![Neue Gruppe wird angezeigt](images/image006.png)
    
4.  Jetzt erstellen wir eine Sicherheits-Policy, die Ihrer Gruppe Berechtigungen im zugewiesenen Compartment erteilt. Beispiel: Erstellen Sie eine Policy, die Mitgliedern der Gruppe **oci-group** im Compartment **Demo** die Berechtigung erteilt:
    
    a) Klicken Sie oben links auf das **Navigationsmenü**. Navigieren Sie zu **Identität und Sicherheit**, und wählen Sie **Policys** aus. ![](https://oracle-livelabs.github.io/common/images/console/id-policies.png " ")
    
    b) Wählen Sie auf der linken Seite das Compartment **Demo** aus. ![*Demo-Compartment auswählen](images/demo-compartment.png)
    
    > **Hinweis:** Möglicherweise müssen Sie auf das Pluszeichen neben Ihrem Haupt-Compartment-Namen klicken, um die _**Demo**_ des Unter-Compartments anzuzeigen. Wenn Sie dies tun und das Unter-Compartment immer noch nicht angezeigt wird, _**aktualisieren Sie Ihren Browser**_. Manchmal speichert der Browser die Compartment-Informationen im Cache und aktualisiert den internen Cache nicht.
    
    c) Nachdem Sie das Compartment **Demo** ausgewählt haben, klicken Sie auf **Policy erstellen**. ![](images/img0012.png)
    
    d) Geben Sie einen eindeutigen **Namen** für die Policy ein (Beispiel: "Policy-for-oci-group").
    
    > **Hinweis:** Der Name darf keine Leerzeichen enthalten.
    
    e) Geben Sie eine **Beschreibung** ein (Beispiel: "Policy für OCI-Gruppe").
    
    f) Wählen Sie **Demo** für Compartment aus.
    
    g) Klicken Sie auf **Manuel-Editor anzeigen**, und geben Sie die folgende **Anweisung** ein:
    
        <copy>Allow group oci-group to manage all-resources in compartment Demo</copy>
        
    
    h) Klicken Sie auf **Erstellen**.
    
    ![Erstellen](images/create-policy.png)
    
5.  Neuen Benutzer erstellen
    
    a) Klicken Sie oben links auf das **Navigationsmenü**, navigieren Sie zu **Identität und Sicherheit**, und wählen Sie **Benutzer** aus.
    
    ![](https://oracle-livelabs.github.io/common/images/console/id-users.png " ")
    
    b) Klicken Sie auf **Benutzer erstellen**.
    
    Geben Sie im Dialogfeld **Benutzer erstellen** Folgendes ein:
    
    *   **Name:** Geben Sie einen eindeutigen Namen oder eine eindeutige E-Mail-Adresse für den neuen Benutzer ein (z.B. **User01**). _Dieser Wert ist der Anmeldename des Benutzers für die Konsole und muss für alle anderen Benutzer in Ihrem Mandanten eindeutig sein._
    *   **Beschreibung:** Geben Sie eine Beschreibung ein (z.B. **Neuer OCI-Benutzer**).
    *   **E-Mail:** Verwenden Sie vorzugsweise eine persönliche E-Mail-Adresse, auf die Sie Zugriff haben (GMail, Yahoo usw.).
    
    Klicken Sie auf **Erstellen**.
    
    ![Neues Benutzerformular](images/user-form.png)
    
6.  Legen Sie ein temporäres Kennwort für den neu erstellten Benutzer fest.
    
    a) Klicken Sie in der Liste der Benutzer auf den **Benutzer, den Sie erstellt haben**, um die zugehörigen Details anzuzeigen.
    
    b) Klicken Sie auf **Kennwort erstellen/zurichten**.
    
    ![Kennwort zurücksetzen](images/image009.png)
    
    c) Klicken Sie im Dialogfeld auf **Kennwort erstellen/ zurücksetzen**.
    
    ![Kennwort zurücksetzen](images/create-password.png)
    
    d) Das neue Einmalkennwort wird angezeigt. Klicken Sie auf den Link **Kopieren** und dann auf **Schließen**. Achten Sie darauf, dieses Passwort auf Ihren Notizblock zu kopieren.
    
    ![](images/copy-password.png)
    
    e) Klicken Sie im Benutzermenü auf **Abmelden**, und melden Sie sich vollständig vom Admin-Benutzeraccount ab.
    
    ![Abmelden](images/sign-out.png)
    
7.  Melden Sie sich als neuer Benutzer mit einem anderen Webbrowser oder einem Inkognito-Fenster an.
    
    a) Öffnen Sie einen unterstützten Browser, und rufen Sie die Konsolen-URL auf: [https://cloud.oracle.com](https://cloud.oracle.com).
    
    b) Klicken Sie im Browserfenster oben rechts auf das Porträtsymbol und dann auf **Bei Oracle Cloud anmelden**.
    
    c) Geben Sie den Namen Ihres Cloud-Accounts (den Namen Ihres Mandanten, nicht Ihren Benutzernamen) ein, und klicken Sie dann auf die Schaltfläche **Weiter**.
    
    ![Mandantennamen eingeben](images/cloud-account-name.png)
    
    d) Dieses Mal melden Sie sich mit dem von Ihnen erstellten Benutzer im Feld **Direkte Oracle Cloud Infrastructure-Anmeldung** an. Beachten Sie, dass der von Ihnen erstellte Benutzer nicht zu Identity Cloud Services gehört.
    
    e) Geben Sie das von Ihnen kopierte Passwort ein. Klicken Sie auf **Anmelden**.
    
    ![Kennwort eingeben](images/sign-in.png)
    
    > **Hinweis:** Da dies die erste Anmeldung ist, wird der Benutzer aufgefordert, das temporäre Kennwort zu ändern, wie im Screenshot unten gezeigt.
    
    f) Legen Sie das neue Passwort fest. Klicken Sie auf **Neues Kennwort speichern**. ![Neues Kennwort festlegen](images/image015.png)
    
8.  Prüfen Sie die Benutzerberechtigungen.
    
    a) Klicken Sie oben links auf das **Navigationsmenü**. Klicken Sie auf **Compute**, und klicken Sie dann auf **Instanzen**.
    
    ![](https://oracle-livelabs.github.io/common/images/console/compute-instances.png " ")
    
    b) Versuchen Sie, ein beliebiges Compartment im linken Menü auszuwählen.
    
    c) Die Meldung "**Sie haben keine Berechtigung zum Anzeigen dieser Ressourcen**" wird angezeigt. Dies ist normal, da Sie den Benutzer nicht der Gruppe hinzugefügt haben, der Sie die Policy zugeordnet haben. ![Fehlermeldung kann ignoriert werden](images/no-permission.png)
    
    d) Melden Sie sich von der Konsole ab.
    
9.  Benutzer zu einer Gruppe hinzufügen.
    
    a) Melden Sie sich mit dem _**admin**_\-Konto wieder an.
    
    b) Klicken Sie oben links auf das **Navigationsmenü**. Navigieren Sie zu **Identität und Sicherheit**, und wählen Sie **Benutzer** aus. Klicken Sie in der Liste **Benutzer** auf den gerade erstellten Benutzeraccount (z.B. `User01`), um zur Seite "Benutzerdetails" zu gelangen. ![](https://oracle-livelabs.github.io/common/images/console/id-users.png " ")
    
    c) Klicken Sie links im Menü **Ressourcen** auf **Gruppen**, falls diese noch nicht ausgewählt ist.
    
    d) Klicken Sie auf **Benutzer zu Gruppe hinzufügen**. ![](images/image020.png)
    
    e) Wählen Sie in der Dropdown-Liste **Gruppen** die von Ihnen erstellte **oci-group** aus.
    
    f) Klicken Sie auf **Hinzufügen**. ![Klicken Sie auf die Schaltfläche "Add (Hinzufügen)".](images/add-user-to-group.png)
    
    g) Bei der Oracle Cloud-Website abmelden.
    
10.  Prüfen Sie die Benutzerberechtigungen, wenn ein Benutzer zu einer bestimmten Gruppe gehört.
    
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

*   **Autor** - Rajeshwari Rai, Prasenjit Sarkar
*   **Mitwirkende** - Arabella Yao, Produktmanagerin, Datenbankproduktmanagement
*   **Zuletzt aktualisiert am/um** - Arabella Yao, Januar 2022