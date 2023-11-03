# Anwendungs-Onboarding

## Einführung

In dieser Übung werden die Schritte zum Onboarding einer Anwendung in Oracle Identity Governance (OIG) mit dem Flat File-Connector erläutert.

_Geschätzte Zeit_: 25 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Anwendung mit Flat File Connector in OIG integrieren

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten (nur _Free-Tier_ und _bezahlte Mandanten_)
    *   Übung: Umgebungssetup
    *   Übung: Umgebung initialisieren

## Aufgabe 1: OIG-Server starten

1.  Klicken Sie im Webbrowserfenster auf der rechten Seite, das mit der _Weblogic-Admin-Konsole_ vorab geladen wurde, auf das Feld _Benutzername_, und wählen Sie die gespeicherten Zugangsdaten aus, oder melden Sie sich mit den folgenden Zugangsdaten an.
    
    *   Benutzername
    
        <copy>weblogic</copy>
        
    
    *   Kennwort
    
        <copy>Welcome1</copy>
        
    
    ![](images/3-weblogic.png)
    
2.  Klicken Sie unter _Umgebung_ auf _Server_.
    
    ![](images/4-weblogic.png)
    
3.  Klicken Sie auf die Registerkarte {\\b Control}. Wählen Sie oim\_server1 und soa\_server1, und klicken Sie auf _Starten_, um die Server zu starten. Dies kann etwa 5-8 Minuten dauern.
    
    ![](images/5-weblogic.png)
    
    ![](images/6-weblogic.png)
    
4.  Beachten Sie, dass sich die Server im Status _Wird ausgeführt_ befinden.
    
    ![](images/7-weblogic.png)
    

## Aufgabe 2: Flat Files in das Verzeichnis kopieren

1.  Öffnen Sie eine Terminalsession als Benutzer "oracle", und kopieren Sie die Berechtigungsdatei.
    
        <copy>
        cd ~
        unzip /u01/files/target/access/archived/documents_16-06-2021_09-51-31.zip -d /u01/files/target/access/
        ls -latr /u01/files/target/access
        </copy>
        
    
    ![](images/8-files.png)
    
2.  Benutzeraccountdatei kopieren
    
        <copy>
        unzip /u01/files/target/accounts/archived/accounts_16-06-2021_09-53-03.zip -d /u01/files/target/accounts/
        ls -latr /u01/files/target/accounts
        </copy>
        
    
    ![](images/9-files.png)
    

## Aufgabe 3: Bildschirm "Anwendung erstellen"

1.  Öffnen Sie im Browserfenster auf der rechten Seite, das mit der _Weblogic-Admin-Konsole_ vorab geladen wurde, die URL unten in einer neuen Registerkarte, klicken Sie dann auf das Feld _Benutzername_, und wählen Sie die gespeicherten Zugangsdaten aus, oder melden Sie sich mit dem folgenden Befehl bei der _OIG Identity-Konsole_ an
    
    *   Adresse
    
        <copy>http://oiri.livelabs.oraclevcn.com:14000/identity/faces/signin</copy>
        
    
    *   Benutzername
    
        <copy>xelsysadm</copy>
        
    
    *   Kennwort
    
        <copy>Welcome1</copy>
        
    
    ![](images/10-oig.png)
    
2.  Aktivieren Sie das Feld "Anwendung" auf der Registerkarte "Verwalten".
    
    ![](images/11-application.png) ![](images/11a-application.png)
    
3.  Klicken Sie auf der Seite "Anwendungen" in der Symbolleiste auf das Menü "Erstellen", und wählen Sie dann die Option _Ziel_, um eine Zielanwendung zu erstellen.
    
    ![](images/12-application.png)
    

## Aufgabe 4: Grundlegende Informationen bereitstellen

1.  Stellen Sie auf der Seite "Basisinformationen" sicher, dass die Option _Connector-Package_ ausgewählt ist.
    
2.  Wählen Sie aus der Dropdown-Liste "Bundle auswählen" die Option _Flat File Connector 12.2.1.3.0_ aus.
    
3.  Geben Sie den Anwendungsnamen, den Anzeigenamen und die Beschreibung für die Anwendung ein.
    
        Application Name : <copy>DMS</copy>
        
    
        Display Name : <copy>Document Management System</copy>
        
    
    ![](images/1-app.png)
    
4.  Blenden Sie den Abschnitt "Erweiterte Einstellungen" ein, und geben Sie einen Wert für den Parameter _flatFileLocation_ ein.
    
        flatFileLocation : <copy>/u01/files/target/accounts/accounts.csv</copy>
        
    
    ![](images/3-app.png)
    
5.  Klicken Sie auf _Header parsen_, um die Header der Flat File zu parsen.
    
6.  In der Tabelle mit den Flat-File-Schemaeigenschaften.
    
    *   Markieren Sie `document_access` als Mehrfachwert, indem Sie das entsprechende Kontrollkästchen in der Spalte "MVA" aktivieren.
    *   Wählen Sie die Spalte "Name" für das Attribut "Benutzername".
    *   Ändern Sie den Datentyp des Attributs `start_date`, indem Sie den Datentyp "Datum" aus der Spalte "Datentyp" auswählen.
    *   Wählen Sie die UID-Spalte für das ID-Attribut aus.

![](images/4-app.png)

7.  Klicken Sie auf "Weiter", um zur Seite "Schema" zu gelangen.

## Aufgabe 5: Schemainformationen aktualisieren

1.  Blenden Sie das Attribut _`document_access`_ ein, und ändern Sie den Anzeigenamen.
    
        Display Name : <copy>DMS Access</copy>
        
    
    ![](images/5-app.png)
    
2.  Klicken Sie auf das Symbol "Erweiterte Einstellung" für das Attribut _`document_access`_.
    
    *   Aktivieren Sie das Kontrollkästchen "Lookup und Berechtigung".
        
    *   Geben Sie diese Details an
        
            List of values : <copy>lookup.dms.access</copy>
            
        
            Length : <copy>15</copy>
            

![](images/5a-app.png)

![](images/6-app.png)

3.  Wählen Sie die Spalte "Groß-/Kleinschreibung ignorieren" für die Attribute Benutzername, ID und `document_access` aus.
    
    ![](images/7-app.png)
    
4.  Klicken Sie auf "Weiter", um zur Seite "Einstellungen" zu gelangen.
    

## Aufgabe 6: Einstellungsinformationen angeben

1.  Klicken Sie auf der Seite Einstellungen auf Vorschaueinstellungen, um eine Vorschau der Einstellungen anzuzeigen.
    
2.  Wählen Sie auf der Registerkarte "Provisioning" in der Dropdown-Liste den Accountnamen als _Benutzername_ aus.
    
    ![](images/8-app.png)
    
3.  Blenden Sie auf der Registerkarte "Abstimmung" die Abstimmungsjobs ein.
    
    ![](images/9-app.png)
    
    ![](images/10-app.png)
    
4.  Löschen Sie die Jobs unter "Flat File Diff Sync", "Flat File Delete Sync" und "Flat File Delete", da diese Jobs für diesen Workshop nicht erforderlich sind.
    
    ![](images/11-app.png)
    
5.  Blenden Sie den Loader für DMS-Flat File-Berechtigungen unter dem Job "Flat File-Berechtigung" ein, und füllen Sie diese Details aus.
    
        Flat File directory : <copy>/u01/files/target/access/</copy>
        
    
        Lookup Name : <copy>lookup.dms.access</copy>
        
    
        Code Key Attribute : <copy>ID</copy>
        
    
        Decode Attribute : <copy>Access</copy>
        
    
    ![](images/13-app.png)
    
6.  Blenden Sie den DMS Flat File Accounts Loader unter dem Job "Flat File Full" ein, und geben Sie diese Details ein.
    
        Flat File directory : <copy>/u01/files/target/accounts/</copy>
        
    
    ![](images/14-app.png)
    
7.  Klicken Sie auf "Next", um mit der Seite "Finish" fortzufahren.
    
    ![](images/15-app.png)
    

## Aufgabe 7: Antragsdetails prüfen und weiterleiten

1.  Prüfen Sie auf der Seite "Fertigstellen" die Bewerbungsübersicht, und klicken Sie auf "Fertigstellen", um den Antrag weiterzuleiten.
    
    ![](images/16-app.png)
    
2.  Klicken Sie auf "Ja", um das Standardanforderungsformular zu erstellen.
    
    ![](images/17-app.png)
    
3.  Klicken Sie auf der Seite "Anwendung" auf das Symbol "Suchen". Beachten Sie, dass die von uns erstellte DMS-Anwendung aufgelistet ist.
    
    ![](images/18-app.png)
    
4.  Melden Sie sich ab und wieder beim Identity Self Service an.
    

## Aufgabe 8: Abstimmung durchführen

1.  Wählen Sie das Feld "Anwendungen" auf der Registerkarte "Verwalten".
    
2.  Klicken Sie auf das Symbol "Suchen", und wählen Sie die DMS-Anwendungszeile.
    
3.  Wählen Sie jetzt "Jobs verwalten".
    
    ![](images/19-app.png)
    
4.  Anspruchsabstimmung ausführen
    
    *   Blenden Sie die Flat File-Berechtigung ein, und erweitern Sie dann den DMS-Loader für Flat File-Berechtigungen.
    *   Klicken Sie auf "Run now", und klicken Sie mehrmals auf das Symbol "Refresh", bis das Ergebnis "Stopped::Success" unter "Job history" angezeigt wird.
    
    ![](images/20-app.png)
    
    ![](images/21-app.png)
    
5.  Vollständige Abstimmung ausführen
    
    *   Blenden Sie Flat File Full ein, und blenden Sie dann den DMS Flat File Accounts Loader ein.
    *   Klicken Sie auf "Jetzt ausführen" und klicken Sie mehrmals auf das Symbol "Aktualisieren", bis das Ergebnis "Gestoppt::Erfolg" unter "Jobhistorie" angezeigt wird
    
    ![](images/22-app.png)
    
    ![](images/23-app.png)
    
6.  Gehen Sie auf der Registerkarte Verwalten zum Feld Benutzer und klicken Sie auf einen beliebigen Benutzer.
    
    ![](images/24-app.png)
    
    ![](images/25-app.png)
    
7.  Klicken Sie auf die Registerkarte "Berechtigungen" und "Accounts", und beachten Sie, dass dem Benutzer die entsprechende Berechtigung für die "DMS"-Anwendung bereitgestellt wird.
    
    ![](images/26-app.png)
    
    ![](images/27-app.png)
    

## Danksagungen

*   **Autor** - Keerti R, Brijith TG, Vineeth Boopathy, NATD Solution Engineering
*   **Mitwirkende** - Keerti R, Brijith TG, Vineeth Boopathy
*   **Zuletzt aktualisiert am/um** - Rene Fontcha, LiveLabs Platform Lead, NA Technology, November 2021