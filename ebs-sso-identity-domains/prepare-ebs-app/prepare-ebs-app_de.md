# Setup vorbereiten

## Einführung

In dieser Übung erfahren Sie, wie Sie die erforderlichen Änderungen in der EBS-Anwendung vornehmen, um das Single Sign-On-Feature zu aktivieren. Außerdem erstellen wir einen Admin-Benutzer in der EBS-Anwendung und in der OCI-Identitätsdomain, der zum Testen des SSO-Ablaufs verwendet wird.

_Geschätzte Laborzeit:_ 20 Minuten

### Ziele

*   Systemadministrator von Oracle E-Business Suite in OCI IAM-Identitätsdomain erstellen
*   E-Mail-Adresse des Systemadministrator von Oracle E-Business Suite aktualisieren
*   Oracle E-Business Suite-Profile aktualisieren
*   EBS-Anwendung neu starten

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Sie haben die vorherigen Übungen erfolgreich durchgeführt
*   JDK 8 oder höher auf Ihrem lokalen System installiert

## Aufgabe 1: Systemadministrator von Oracle E-Business Suite in OCI-IAM-Identitätsdomain erstellen

Erstellen Sie einen Benutzer in der Oracle IAM-Identitätsdomain, der dem Systemadministrator in Ihrer Oracle E-Business Suite (EBS) entspricht. Andernfalls kann sich der Systemadministrator nicht bei der EBS-Konsole anmelden, nachdem EBS für die Authentifizierung mit der OCI IAM-Identitätsdomain konfiguriert wurde.

1.  Melden Sie sich bei Ihren OCI-IAM-Identitätsdomains an, um auf die **OCI-Konsole** zuzugreifen. Nachdem Sie sich angemeldet haben, navigieren Sie unter **Identität und Sicherheit** zu **Domains**.Wählen Sie jetzt die zuvor bereitgestellte **Identitätsdomain** aus.
    
    ![Bild 9](./images/image9.png "Bild 9")
    
2.  Klicken Sie auf **Benutzer**, geben Sie im **Fenster "Benutzer hinzufügen"** die folgenden Werte an, und klicken Sie auf **Erstellen**.
    
    1.  **First Name**: EBS
    2.  **Nachname**: Systemadministrator
    3.  Wählen Sie die E-Mail-Adresse als Benutzername aus.
    4.  **Benutzername**: Systemadministrator
    5.  **E-Mail**: Geben Sie die E-Mail-Adresse an, die für den SYSADMIN-Account in Oracle E-Business Suite festgelegt ist.
    
    ![Bild 10](./images/image10.png "Bild 10")
    

## Aufgabe 2: E-Mail-Adresse des Systemadministrators von Oracle E-Business Suite aktualisieren

Ändern Sie die E-Mail-Adresse des SYSADMIN-Benutzers in Oracle E-Business Suite in die E-Mail-Adresse, die Sie für den entsprechenden Benutzer in Oracle Identity Cloud Service angegeben haben.

1.  Melden Sie sich als Administrator (z.B. als Systemadministrator) bei der Oracle E-Business Suite-Anwendung an.
    
2.  Scrollen Sie auf der Oracle E-Business Suite-Homepage im **Navigator** nach unten, blenden Sie **Benutzermanagement** ein, und klicken Sie auf **Benutzer**.
    
3.  Suchen Sie auf der Seite **User Maintenance** nach dem Benutzernamen **SYSADMIN**, und klicken Sie auf das **Updatesymbol** für den **SYSADMIN**\-Benutzer.
    
    ![Bild 11](./images/image11.png "Bild 11")
    
4.  Aktualisieren Sie den Wert im Feld **E-Mail** mit der E-Mail-Adresse, die Sie bei der Erstellung des Systemadministratorbenutzers in der OCI-IAM-Identitätsdomain angegeben haben, und klicken Sie auf **Anwenden**.
    
    ![Bild 12](./images/image12.png "Bild 12")
    
5.  Schließen Sie die Anwendung der Oracle E-Business Suite.
    

## Aufgabe 3: Oracle E-Business Suite-Profile aktualisieren

_Führen Sie die folgenden Schritte aus, um Oracle E-Business Suite so zu konfigurieren, dass nicht von E-Business-Suite authentifizierte Benutzer zu E-Business Suite Asserter umgeleitet werden, anstatt die lokale Anmeldeseite von Oracle E-Business Suite zu verwenden._

1.  Blättern Sie auf der Oracle E-Business Suite-Homepage im Navigator nach unten, blenden Sie **Funktionsadministrator** ein, klicken Sie auf die Registerkarte **Coreservices**, und klicken Sie dann auf die Registerkarte **Profile**.
    
2.  Geben Sie im Feld "Suchen", "Profilwerte", "Code" den Wert **App%Agent%** ein, und klicken Sie auf **Suchen**
    
    ![Bild 18](./images/image18.png "Bild 18")
    
3.  Geben Sie auf der Seite "Profilwerte definieren: **Application Authenticate Agent**" **E-Business Suite Asserter's URL- https://ebsasserter.example.com:7004/ebs** in das Feld "Site Value" ein, und **speichern** Sie ihn.
    
4.  Zurück zur Registerkarte **Profile** geben Sie **%SSO%Type%** in die Suche ein, aktualisieren Sie den Codeeintrag **APPS\_SSO** von **SSWA zu SSWAw/SSO**, und **speichern** Sie das Profil.
    
    ![Bild 19](./images/image19.png "Bild 19")
    

5 Kehren Sie zur Registerkarte **Profile** zurück, geben Sie **%Oracle Applications Session%** in die Suche ein, aktualisieren Sie den Wert von **HOST** in **DOMAIN**, und **speichern** Sie das Profil.

![Bild 20](./images/image20.png "Bild 20")

**Hinweis** Um die EBS-Anwendung auszuführen und diese Profil-Vaules zu aktualisieren, muss JAVA in Ihrem lokalen System installiert sein.

## Aufgabe 4: Oracle E-Business Suite neu starten

Stellen Sie nach Abschluss der Profiländerungen eine SSH-Verbindung zum EBS-Server her, und führen Sie die folgenden Befehle aus

    $ sudo hostname demoebs.example.com
    # sudo su - oracle
    $ /u01/install/APPS/scripts/stopapps.sh
    $ /u01/install/APPS/scripts/startapps.sh
    
    

**Hinweis** Verwenden Sie bei Bedarf den oben genannten Hostnamen.

Sie können jetzt **mit der nächsten Übung fortfahren.**

## Danksagungen

*   **Autor** - Gautam Mishra, Aqib Bhat, Samratha S P
*   **Mitwirkender** - Chetan Soni, Sagar Takkar
*   **Unterstützt von** - Deepak Rao Narasimha Gajendragad
*   **Lead By** - Deepthi Shetty
*   **Zuletzt aktualisiert am/um** - Gautam Mishra Mai 2023