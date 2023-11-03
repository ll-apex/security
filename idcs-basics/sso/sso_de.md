# Einrichten der Umgebung

## Einführung

Oracle Identity Cloud Service (IDCS) bietet die Integration mit jedem Service, der über das _SAML_\-(Security Access Markup Language-)Protokoll integriert werden kann. Verwaltungen können Benutzer in verschiedenen Anwendungen über ein einziges Bedienfeld verwalten, und Endbenutzer können Anwendungen mit einem einzigen Klick aufrufen.

IDCS bietet Unterstützung für standardmäßige SAML 2.0-Browser-POST-Anmelde- und Abmeldeprofile.

Beachten Sie, dass, obwohl IDCS SAML und OpenID Connect/OAuth unterstützt, es viele Male gibt, wenn ein nicht-SAML-fähiges System SSO erfordert. IDCS App Gate unterstützt Nicht-SAML-Systeme, die Header-basiertes, Cookie-basiertes oder Formular-Fill-SSO verwenden. App Gate ist eine Nginx-basierte Software-Appliance, die in einer Proxykonfiguration bereitgestellt wird und auf AWS, OCI, OCI-C oder On Premise in VMWare oder Oracle Virtualbox bereitgestellt werden kann. Er wird über Identity Cloud Service bereitgestellt und ist für IDCS Standard-Kunden verfügbar.

Diese Übung soll föderiertes Single Sign-On (SSO) mit einer Anwendung SaaS der 3. Partei demonstrieren. Ziel ist es, den geschäftlichen Nutzen von IDCS als zentralem Identitätsprovider für Ihr wachsendes Portfolio an Produkten und Services von Oracle und Nicht-Oracle zu veranschaulichen.

Geschätzte Laborzeit: 90 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Salesforce-Anwendung konfigurieren
*   Anwendungen zu Gruppe zuweisen
*   Gruppenzugriff anfordern
*   SSO-Konfiguration überprüfen
*   Provisioning und Synchronisierung konfigurieren

### Voraussetzungen

*   Free Tier- oder kostenpflichtige Oracle-Accounts
*   Ein Google-Account
*   Ein Salesforce-Entwickleraccount

## Aufgabe 1: Salesforce-Anwendung konfigurieren

*   _Personen_:
    *   Administrator

In dieser praktischen Übung richten wir die Integration mit _Salesforce_ mit SAML ein. IDCS fungiert als _IdP_ (Identitätsprovider) und Salesforce-Organisation als _SP_ (Serviceprovider, auch als Relying Party bezeichnet)

1.  Laden Sie IDCS-Metadaten in eine lokale XML-Datei herunter. Metadaten sind im folgenden Verzeichnis verfügbar:
    
        https://<your tenant>/fed/v1/metadata
        
    
    Je nach verwendetem Browser können Sie dies am einfachsten tun, indem Sie mit der rechten Maustaste auf eine beliebige Stelle auf der Seite klicken und die Option "Speichern unter" auswählen, einen Namen angeben und als XML-Datei speichern. Verwenden Sie die Option Kopieren/Einfügen nicht, da die XML-Datei möglicherweise geändert wird.
    
    ![Bild](images/L2001.png)
    

**Die folgenden Schritte 2-8 wurden im Abschnitt "Workshop-Voraussetzungen" behandelt**

2.  Registrieren Sie sich für ein [Salesforce-Entwicklerkonto](https://developer.salesforce.com).
    
    ![Bild](images/L2002.png)
    
    Prüfen Sie nach der Registrierung Ihre E-Mail-Adresse, prüfen Sie die Accountregistrierung, und melden Sie sich bei Salesforce an. Beachten Sie, dass Sie möglicherweise "Zu Salesforce Classic wechseln" auswählen müssen.
    
3.  Greifen Sie über die nach der Registrierung angegebene URL auf die Salesforce-Anwendung zu. Klicken Sie auf das Profilmenü gemäß dem folgenden Druckbildschirm, und wählen Sie die Option _Zu Salesforce Classic wechseln_ aus.
    
    ![Bild](images/L2003.png)
    
4.  Gehen Sie im oberen Registerkartenmenü neben dem Profil zu _Setup_.
    
    ![Bild](images/L2004.png)
    
5.  Registrieren Sie eine benutzerdefinierte Domain. Gehen Sie zu _Domainverwaltung_, wählen Sie _Meine Domain_ aus, und registrieren Sie eine Domain Ihrer Wahl. Dies soll eine benutzerdefinierte Anwendungs-URL für unseren Anwendungsfall bereitstellen.
    
6.  _Verfügbarkeit prüfen_ für Ihre neue Domain.
    
7.  _Registrieren_ Sie Ihre neue Domain.
    
    ![Bild](images/L2005.png)
    
8.  Salesforce aktualisiert seine Domain-Registrys mit Ihrer neuen. Anschließend erhalten Sie eine Bestätigungs-E-Mail. Es kann einige Minuten dauern, bis Ihre Domain verfügbar ist.
    
9.  Klicken Sie unter Domain Management auf Domains und dann auf Ihre neu erstellte Domain.
    
    ![Bild](images/L2006.png)
    
10.  Wenn Ihre neue Domain registriert ist, notieren Sie sich Ihre Domain-URL
    
    ![Bild](images/L2007.png)
    
11.  Melden Sie sich mit der oben angegebenen URL bei der neuen Domain an, gehen Sie zurück zu _Domainverwaltung_, klicken Sie auf _Domains_ und dann auf die zuletzt erstellte Domain.
    
12.  Klicken Sie auf _Für Benutzer bereitstellen_. Wenn das Popup-Fenster angezeigt wird, klicken Sie auf _OK_.
    
    ![Bild](images/L2008.png)
    
13.  Ihre Domain ist jetzt eingerichtet und für Benutzer bereitgestellt. Stellen Sie sicher, dass unter "Meine Domain" angezeigt wird, dass Schritt 4 abgeschlossen ist und _Für Benutzer bereitgestellte Domain_ angezeigt wird.
    
14.  Gehen Sie in der seitlichen Menüleiste zu _Sicherheitssteuerelemente_, und wählen Sie _Single Sign-On-Einstellungen_.
    
15.  Klicken Sie auf _Bearbeiten_, und aktivieren Sie die Option _Föderiertes Single Sign-On mit SAML_. Klicken Sie auf _Speichern_.
    
    ![Bild](images/L2010.png)
    
        We need to activate the SAML option on Salesforce in order to be able to integrate with IDCS via these open standards.  Also, the SAML metadata needs to be exchanged between IDCS and Salesforce
        
16.  Klicken Sie auf die Schaltfläche _Neu aus Metadatendatei_, um IDCS-Metadaten zu importieren. Wählen Sie die heruntergeladene Metadatendatei aus Schritt 1 mit der Schaltfläche _Datei auswählen_ aus. Klicken Sie auf _Erstellen_.
    
    ![Bild](images/L2011.png)
    
        IDCS metadata file contains information about IDCS endpoints for SSO, single logout, Entity ID, Issuer details, IDCS signing certificate etc which are needed by Salesforce in order to establish a federated trust with IDCS.
        
17.  Behalten Sie alle Standardinformationen bei, und klicken Sie auf _Speichern_.
    
    ![Bild](images/L2012.png)
    
    Im Folgenden finden Sie die Details zu diesem Beispiel. Beachten Sie, dass Ihre URLs und Domaininformationen unterschiedlich sind.
    
    ![Bild](images/L2013.png)
    
18.  Notieren Sie sich Folgendes:
    

*   Wert des Domainnamens, Beispiel: prodsys-dev-ed
    
*   (Optional) Wert der Organisations-ID. Beispiel: 00D4J000000DND9
    
    ![Bild](images/L2014.png)
    

Hinweis: Beachten Sie den folgenden Schritt, falls der Wert für die Organisations-ID nicht im Feld "Anmelde-URL" angezeigt wird.

19.  Wenn die Organisations-ID nicht in der Anmelde-URL angezeigt wird, klicken Sie auf _Unternehmensprofil_ und dann auf _Unternehmensinformationen_, um die _Organisations-ID_ abzurufen.
    
    ![Bild](images/L2015.png)
    

Salesforce wurde erfolgreich konfiguriert. Im nächsten Schritt konfigurieren Sie IDCS.

## Aufgabe 2: Salesforce IDCS-Anwendung erstellen

*   _Personen_:
    *   Administrator

1.  Melden Sie sich als Administrator bei der IDCS-Admin-Konsole an:
    
        https://<your tenant>/ui/v1/adminconsole
        
    
    ![Bild](images/L2016.png)
    
2.  Wählen Sie die Registerkarte "Anwendungen" im IDCS-Dashboard aus, das nach der Anmeldung angezeigt wird.
    
    ![Bild](images/L2017.png)
    
3.  Klicken Sie auf _Hinzufügen_, um eine neue Anwendung zu erstellen.
    
    ![Bild](images/L2018.png)
    
4.  Wählen Sie _App-Katalog_ aus.
    
    ![Bild](images/L2019.png)
    
        Note that IDCS provides an application template for Salesforce.
        The App Catalog is a collection of partially configured application templates that Oracle creates and maintains for you. You can use the templates to define the application, configure Single Sign-On, and enable and configure provisioning and synchronization. The App Catalog templates allow you to onboard applications quickly and securely.
        
        App templates simplify adding a new application by prepopulating all common attributes and values so that you just have to enter your tenant specific details.
        
5.  Suchen Sie im _App-Katalog_ nach _Salesforce_. Klicken Sie auf _Hinzufügen_.
    
    ![Bild](images/L2020.png)
    
6.  Geben Sie auf der ersten Seite des Konfigurationsbildschirms den Wert _Domainname_ an, den Sie in Salesforce notiert haben. Optional können Sie die _Organisations-ID_ hinzufügen. Klicken Sie auf _Weiter_.
    
7.  Klicken Sie erneut auf _Weiter_, wenn Sie sich im Fenster _SSO-Konfiguration_ befinden.
    
    ![Bild](images/L2021.png)
    
8.  Klicken Sie auf _Beenden_
    
    ![Bild](images/L2022.png)
    
9.  Anwendung aktivieren
    

    ![Image](images/L2023.png)
    

## Aufgabe 3: Apps zu Gruppe zuweisen

*   _Personen_:
    *   Administrator

In IDCS haben Sie die Möglichkeit, Benutzern den Zugriff direkt, durch direkte Zuweisung zu einer bestimmten Anwendung oder indirekt über dedizierte Gruppen zuzuweisen. Für den Anwendungsfall verwenden wir Gruppen, um Benutzern Zugriff auf die Salesforce-Anwendung zu gewähren.

1.  Klicken Sie auf das Schubfach "Navigation", und wählen Sie _Gruppen_.
    
2.  Fügen Sie eine Gruppe mit der Bezeichnung _Mitarbeiter_ hinzu. Aktivieren Sie das Kontrollkästchen _Benutzer kann Zugriff anfordern_.
    
    ![Bild](images/L2024.png)
    
3.  Klicken Sie auf _Beenden_
    
4.  Gehen Sie zur Registerkarte _Access_. Klicken Sie auf _Zuweisen_.
    
5.  Wählen Sie die Schaltfläche _Zuweisen_ in Übereinstimmung mit der _Salesforce_\-Anwendung, und klicken Sie auf _OK_.
    
    ![Bild](images/L2025.png)
    
        Note: you need to add a corresponding account in Salesforce for Federation to work correctly.  How accounts may be created in production are out of scope for this workshop but can be accomplished in a number of methods.  
        
6.  Melden Sie sich bei Ihrem [Salesforce-Entwickleraccount](https://developer.salesforce.com/) an, um einen entsprechenden Account für den IDCS-Account zu erstellen.
    
    ![Bild](images/L2026.png)
    
7.  Wenn Sie nicht Salesforce Classic sind, klicken Sie auf das Profilmenü gemäß dem folgenden Druckbildschirm, und wählen Sie die Option _Zu Salesforce Classic wechseln_ aus.
    
    ![Bild](images/L2027.png)
    
8.  Gehen Sie im oberen Registerkartenmenü neben dem Profil zu _Setup_.
    
    ![Bild](images/L2028.png)
    
9.  Klicken Sie im linken Bereich auf _Benutzer verwalten_, _Benutzer_ und dann auf _Neuer Benutzer_.
    
    ![Bild](images/L2029.png)
    
10.  Erstellen Sie einen Benutzer wie unten dargestellt. Stellen Sie sicher, dass Sie die E-Mail-Adresse als Anmeldung verwenden, und verwenden Sie eine E-Mail, die mit einem Account in IDCS übereinstimmt. Legen Sie die folgenden Parameter fest:
    
    | Parameter | Wert |
    | --- | --- |
    | Rolle | z.B. Marketingteam |
    | Benutzerlizenz | Salesforce-Plattform |
    | Profil | Standard-Plattformbenutzer |
    
    ![Bild](images/L2030.png)
    

Nachdem der Account in Salesforce verfügbar ist und IDCS den authentifizierten Account enthält, den Sie testen können.

## Aufgabe 4: Gruppenzugriff anfordern

*   _Personen_:
    *   Endbenutzer

Denken Sie daran, dass die Gruppe _Mitarbeiter_ in IDCS erstellt wurde, die Zugriff auf die Salesforce-Anwendung hat, ihr jedoch noch kein Benutzer zugewiesen ist. Da wir bei der Erstellung der Gruppe die Option _Benutzer kann Zugriff anfordern_ gewählt haben, sollten wir jetzt in der Lage sein, Zugriff darauf und implizit auf die Salesforce-Anwendung anzufordern.

1.  Schließen Sie den Browser, um den Cache zu löschen, und melden Sie sich dann bei der IDCS-Konsole an
    
        https://<yourtenant>/ui/v1/myconsole
        
    
        Please use the same user credentials that you have entered in the previous section in order to create a user account in Salesforce. If you have followed the conventions recommended by this guide this should be in the form of:     <username>+ui@<email_provider.com>
        
2.  Klicken Sie auf der Seite _Meine Apps_ auf die Schaltfläche für die Zugriffsanforderung _\+ Hinzufügen_.
    
    ![Bild](images/L2031.png)
    
3.  Wählen Sie auf der Registerkarte _Gruppen_ die Gruppe _Mitarbeiter_ aus, und wählen Sie das _+_\-Zeichen aus.
    
    ![Bild](images/L2032.png)
    
4.  Geben Sie auf der daraufhin angezeigten Popup-Seite eine _Begründung_ an. Klicken Sie auf _OK_. Dies ist eine automatisch genehmigte Anforderung, und der Zugriff sollte sofort ohne Eingriff des Administrators erteilt werden.
    
    ![Bild](images/L2033.png)
    
5.  Gehen Sie vom Menü oben rechts zum Abschnitt _Mein Zugriff_.
    
    ![Bild](images/L2034.png)
    
6.  Stellen Sie sicher, dass Salesforce-Anwendungen jetzt auf der Seite _Meine Apps_ angezeigt werden.
    
    ![Bild](images/L2035.png)
    

## Aufgabe 5: SSO-Konfiguration prüfen

*   _Personen_:
    *   Endbenutzer

1.  Klicken Sie in der IDCS-Konsole auf der Seite _Meine Apps_ auf die App _Salesforce Chatter_.
    
    Hinweis: Falls die Salesforce-Anwendungen nicht sichtbar sind, während Ihr Benutzer einer Gruppe zugewiesen ist, die Zugriff auf Salesforce gewährt, muss zuerst eine Synchronisierung mit Ihrem Benutzer in IDCS und Salesforce konfiguriert werden. In Schritt 6 wird erläutert, wie Benutzer zwischen IDCS und Salesforce synchronisiert werden.
    
    ![Bild](images/L2036.png)
    
2.  Stellen Sie sicher, dass der Benutzer automatisch bei Salesforce Chatter (SSO) angemeldet ist.
    
    ![Bild](images/L2037.png)
    

Sie sollten nun dieselben Benutzerprofilinformationen sehen, die Sie in IDCS in Salesforce gestartet haben, ohne sich bei Salesforce anmelden zu müssen.

    When you submit your IDCS user login credentials, in the background, IDCS prepares a SAML assertion and redirects the user’s browser to the Salesforce SaaS application.
    
    Salesforce consumes the SAML assertion and maps the user in its local identity store, based upon the federation agreements which had previously been configured.
    

## Aufgabe 6: Provisioning und Synchronisierung konfigurieren

*   _Personen_:
    *   Administrator

### Hostname, Organisations-ID und Domainname aus Salesforce abrufen

Ein Hostname, eine Organisations-ID und ein Domainname sind erforderlich, bevor Sie die Salesforce-App in Oracle Identity Cloud Service konfigurieren können. Diese Werte erhalten Sie von Salesforce.

1.  Suchen Sie im linken Navigationsmenü der Homepage, und klicken Sie auf _Single Sign-On-Einstellungen_. Die Seite "Single Sign-On-Einstellungen" wird als einziger gültiger Link angezeigt.
    
    ![Bild](images/L2038.png)
    
2.  Klicken Sie im Abschnitt _Single Sign-On-Einstellungen_ auf den Namen, den Sie für Ihren Identitätsprovider auf der Seite _Single Sign-On-Einstellungen_ angegeben haben.
    
3.  Notieren Sie sich den Hostnamen aus dem Wert im Feld "Entity-ID", und klicken Sie auf den entsprechenden Instanznamen.
    
    ![Bild](images/L2039.png)
    
        Note: Use this host name value while enabling user provisioning for the Salesforce app in Oracle Identity Cloud Service in the "Enabling Provisioning" section, and while verifying the SSO initiated from Salesforce in the "Verifying Service Provider Initiated SSO from Salesforce" section.
        
4.  Notieren Sie im Abschnitt _Endpunkte_ den Domainnamen und die Organisations-ID aus der Salesforce-Anmelde-URL:
    
        https://<Domain_Name>.my.salesforce.com?so=<Organization_ID>
        or
        https://<Domain_Name>.my.salesforce.com
        
    
    Der Domainname wird am Anfang und die Organisations-ID am Ende der URL angezeigt.
    
    ![Bild](images/L2040.png)
    

### Consumer Key und Consumer Secret aus Salesforce abrufen

1.  Wechseln Sie in Salesforce zur _Lightning Experience_.
    
    ![Bild](images/L2041.png)
    
2.  Klicken Sie auf das Zahnrad in der oberen rechten Ecke der Seite, und wählen Sie _Setup_ (Setup).
    
    ![Bild](images/L2042.png)
    
3.  Suchen Sie im linken Navigationsmenü der Homepage, und klicken Sie auf App Manager. Die Seite _App Manager_ von Lightning Experience wird angezeigt.
    
    ![Bild](images/L2043.png)
    
4.  Klicken Sie in der rechten Ecke der Seite auf _Neue verbundene App_. Die Seite "Neue verbundene App" wird angezeigt.
    
    ![Bild](images/L2044.png)
    
5.  Geben Sie im Abschnitt "Basisinformationen" den _Namen der verbundenen App_ der App ein, die Sie verbinden möchten.
    
6.  Geben Sie die _Kontakt-E-Mail_ des Administrators ein.
    
7.  Aktivieren Sie im Abschnitt "API" (OAuth-Einstellungen aktivieren) das Kontrollkästchen _OAuth-Einstellungen aktivieren_.
    
8.  Geben Sie im Feld _Callback-URL_ eine beliebige Public Domain-URL ein, die das Zugriffstoken vom Autorisierungsserver empfängt. Beispiel: https://login.salesforce.com.
    
9.  Wählen Sie im Feld _Ausgewählte OAuth-Geltungsbereiche_ in der Liste _Verfügbare OAuth-Geltungsbereiche_ die Option _Vollständiger Zugriff (vollständig)_ aus, und klicken Sie auf _Hinzufügen_, um vollständigen Zugriff zum Ändern der OAuth zu erhalten.
    
    ![Bild](images/L2045.png)
    
10.  Scrollen Sie nach unten, und klicken Sie auf _Speichern_.
    
11.  Warten Sie 2-10 Minuten, bis die Änderungen auf dem Server wirksam werden, bevor Sie die verbundene App verwenden, und klicken Sie dann auf _Weiter_.
    
12.  In den meisten Fällen wird die neu erstellte App-Seite automatisch angezeigt. Falls es nicht angezeigt wird, führen Sie bitte die folgenden Schritte aus. Suchen Sie im linken Navigationsmenü der Homepage, und klicken Sie auf _App-Manager_. Suchen Sie nach der neu erstellten App, klicken Sie auf den kleinen Pfeil rechts, und wählen Sie _Anzeigen_ aus.
    
    ![Bild](images/L2046.png)
    
13.  Klicken Sie im Abschnitt "API" (OAuth-Einstellungen aktivieren) neben dem Feld _Consumer Secret_ auf _Zum Anzeigen klicken_.
    
    ![Bild](images/L2047.png)
    
14.  Notieren Sie sich die Werte für _Consumer-Schlüssel_ und _Consumer Secret_.
    

### Ableiten des Administratorkennworts von Salesforce

**An das Administratorkennwort muss ein Sicherheitstoken angehängt werden**, bevor Sie das Provisioning und die Synchronisierung für die Salesforce-App aktivieren. Sie erhalten den Tokenwert aus Salesforce.

Der endgültige Wert sieht folgendermaßen aus:

    <yourSalesforceAdminPassword + securityToken(ex: LKdMzECdjFKYSj028WJhU1GG)>
    

1.  Klicken Sie in der oberen rechten Ecke der Salesforce-Homepage auf das Benutzersymbol und dann in der Dropdown-Liste auf "Einstellungen".
    
    ![Bild](images/L2048.png)  
     
    
2.  Suchen Sie im linken Navigationsmenü, und klicken Sie auf _Mein Sicherheitstoken zurücksetzen_.
    
    ![Bild](images/L2049.png)
    
3.  Klicken Sie auf der Seite _Mein Sicherheitstoken zurücksetzen_ auf _Mein Sicherheitstoken zurücksetzen_. Ein Sicherheitstoken wird an die E-Mail-Adresse des Administrators gesendet.
    
4.  Notieren Sie sich das Sicherheitstoken, und hängen Sie das Sicherheitstoken an das Administratorkennwort an.
    
        <yourSalesforceAdminPassword + securityToken(ex: LKdMzECdWJhU1GG)>
        
    
        NOTE: the <>, + and example must not be included
        

### Salesforce-Profil-ID abrufen

Die in Oracle Identity Cloud Service erstellten Benutzer müssen einem Salesforce-Profil zugewiesen sein. Der Profil-ID-Wert dieses Salesforce-Profils muss aus Salesforce erfasst werden, bevor Sie das Benutzer-Provisioning und die Synchronisierung für die Salesforce-App aktivieren.

1.  Klicken Sie auf _Zu Salesforce Classic wechseln_
    
    ![Bild](images/L2050.png)
    
2.  Klicken Sie auf _Setup_.
    
    ![Bild](images/L2051.png)
    
3.  Gehen Sie zu _Verwalten_, und wählen Sie _Benutzer und Profile verwalten_ aus.
    
    ![Bild](images/L2052.png)
    
4.  Klicken Sie auf der zweiten Seite auf _Standardplattformbenutzer_.
    
    ![Bild](images/L2053.png)
    

  5. Notieren Sie sich die URL des Profils. Die _Profil-ID_ ist die ID am Ende der URL.

    ![Image](images/L2054.png)
    

6.  Notieren Sie sich den Wert. Er wird später erneut verwendet.

### Provisioning und Synchronisierung für Salesforce aktivieren

Beim Provisioning werden die folgenden verfügbaren Vorgänge bereitgestellt:

*   _Account erstellen_: Erstellt automatisch einen Salesforce-Account, wenn dem entsprechenden Benutzer in Oracle Identity Cloud Service Salesforce-Zugriff erteilt wird.
*   _Account deaktivieren_: Deaktiviert oder aktiviert automatisch einen Salesforce-Account, wenn der Salesforce-Zugriff für den entsprechenden Benutzer in Oracle Identity Cloud Service deaktiviert oder aktiviert wird.
*   _Account löschen_: Entfernt automatisch einen Account aus Salesforce, wenn dem entsprechenden Benutzer in Oracle Identity Cloud Service der Salesforce-Zugriff entzogen wird.

Darüber hinaus können Sie Attribute zwischen den in Salesforce definierten Benutzeraccountfeldern und den entsprechenden in Oracle Identity Cloud Service definierten Feldern zuordnen.

1.  Gehen Sie zur IDCS-Administrationskonsole, und wählen Sie die Salesforce-Anwendungen aus, die Sie zuvor erstellt haben.
    
2.  Wählen Sie auf der Seite _Provisioning_ die Option _Enable Provisioning_ aus.
    
3.  Ein Fenster erscheint. Klicken Sie auf _Einwilligung erteilen_.
    
4.  Geben Sie die folgenden Parameter mit den Werten ein, die Sie in den vorherigen Schritten von Salesforce abgeleitet haben:
    
    ![Bild](images/L2055.png)
    
    | Parameter | Wert |
    | --- | --- |
    | Hostname | Geben Sie den Salesforce-Hostnamen ein, den Sie durch Ausführen der Schritte im Abschnitt "Hostnamen, Organisations-ID und Domainname aus Salesforce abrufen" erhalten haben. |
    | Administratorbenutzername | Geben Sie den Benutzernamen für den Administratoraccount ein. |
    | Kennwort des Administrators | Geben Sie das aktualisierte Kennwort ein, das Sie abgeleitet haben, indem Sie die Schritte im Abschnitt "Administratorkennwort aus Salesforce ableiten" ausführen. |
    | Client-ID | Geben Sie den Salesforce-Consumer-Schlüssel ein, den Sie durch Ausführen der Schritte im Abschnitt "Consumer-Schlüssel und Consumer Secret aus Salesforce abrufen" erhalten haben. |
    | Client Secret | Geben Sie das Salesforce Consumer Secret ein, das Sie durch Ausführen der Schritte im Abschnitt "Consumer Key und Consumer Secret aus Salesforce abrufen" abgerufen haben. |
    
5.  Klicken Sie auf _Konnektivität testen_, um die Verbindung zu Salesforce zu prüfen. Oracle Identity Cloud Service zeigt eine Bestätigungsmeldung an.
    
6.  Klicken Sie auf _Attributzuordnung_
    
    ![Bild](images/L2056.png)
    
7.  Setzen Sie die _Profil-ID_ auf den oben abgerufenen Wert.
    
    ![Bild](images/L2057.png)
    

  8. Scrollen Sie auf der Seite _Provisioning wird ausgeführt_ nach unten, und wählen Sie _Synchronisierung aktivieren_ aus.

    ![Image](images/L2058.png)
    

Mit dieser Option werden die vorhandenen Accountdetails aus Salesforce synchronisiert und mit den entsprechenden Oracle Identity Cloud Service-Benutzern verknüpft.  

### Synchronisierung in IDCS testen

1.  Wählen Sie auf der Anwendungsseite _Import_ aus.
    
2.  Klicken Sie auf _Importieren_, und warten Sie einen Moment.
    
    ![Bild](images/L2059.png)
    
3.  Klicken Sie auf _Aktualisieren_.
    
    ![Bild](images/L2060.png)
    
4.  Die importierten Benutzer, die aus IDCS importiert wurden, werden angezeigt.
    
    ![Bild](images/L2061.png)
    

  5. Der letzte Schritt besteht darin, die Benutzer zu bestätigen, die Sie zwischen IDCS und Salesforce verknüpfen möchten. Klicken Sie unter "Synchronisierungsstatus" auf _Bestätigen_. Eine erfolgreiche Verknüpfung zwischen einem Benutzer in IDCS und Salesforce führt zum Synchronisierungsstatus _Bestätigt_.

    ![Image](images/L2062.png)
    

    Every user that has status Confirmed in the Synchronization Status has access to Salesforce in My Apps. Also, the application Salesforce will be visible to all users in My Apps that have status Confirmed.
    

Sie können jetzt mit der nächsten Übung fortfahren

 

## Danksagungen

*   **Autor** - SEHub Sicherheits- und Verwaltbarkeitsteam
*   **Zuletzt aktualisiert am/um** - Lucian Ionescu, Principal Solution Engineer, 15.09.2020