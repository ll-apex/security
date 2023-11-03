# Verwaltung des Mitarbeiterlebenszyklus

## Einführung

In dieser Übung werden mehrere Anwendungsfälle im Zusammenhang mit Mitarbeiter-Onboarding, Benutzerlebenszyklus, Versetzungen, Managergenehmigungen und Mitarbeiterkündigungen mit **Meine HR-Anwendung** als maßgebliche Quelle in Oracle Identity Manager ausgeführt.

"Meine HR-Anwendung" ist eine Beispielanwendung, die in Oracle APEX entwickelt und in der Oracle-Datenbank gehostet wird. Seine Hauptfunktion besteht darin, Mitarbeiterdaten in Tabellen im HR-Datenbankschema zu verwalten und zu speichern. Der **DBAT-Connector** von Oracle Identity Manager wird für die Schnittstelle zu Datenbanktabellen verwendet, die das Onboarding und Management von Mitarbeiterdatensätzen in Oracle Identity Manager erleichtern.

![Meine HR-Anwendung](./images/img-myhr-app-menu.png " ")

Abbildung 1. Meine HR-Anwendung

Folgende Anwendungsfälle sind verfügbar:

*   Mitarbeiter-Onboarding und Benachrichtigungen
*   Mitarbeitertransfer und Rollenaktualisierungen
*   Self-Service, Zugriffsanforderung, aktionsunterstützende Benachrichtigungen und Genehmigungen
*   Mitarbeiterkündigung

_Geschätzte Laborzeit_: 60 Minuten

### Ziele

*   Machen Sie sich mit dem Mitarbeiterlebenszyklusmanagement vertraut

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten (nur _Free-Tier_ und _bezahlte Mandanten_)
    *   Übung: Umgebungssetup
    *   Übung: Umgebung initialisieren

## Aufgabe 1: Zugriff auf erforderliche Komponenten und Anwendungen validieren

1.  Validieren Sie den Zugriff auf erforderliche Komponenten und Anwendungen. Weitere Informationen finden Sie unter _Übung 1: Umgebung initialisieren_.
    
    Oracle Identity Manager-Admin-Konsole:
    
        URL         http://secureoracle.oracledemo.com:14000/sysadmin
        User        xelsysadm
        Password    Oracle123
        
    
    Oracle Identity Manager Self Service -
    
        URL         http://secureoracle.oracledemo.com:14000/identity
        User        xelsysadm
        Password    Oracle123
        
    
    E-Mail-Webclient (Roundcube):
    
        URL         http://secureoracle.oracledemo.com/roundcubemail-1.4.1/
        User        admin
        Password    Oracle123
        Server      secureoracle.oracledemo.com
        
    
    E-Mail-Server-Administrationskonsole (Hedwig Mail Server):
    
        URL         http://secureoracle.oracledemo.com:7001/hedwig-web-0.6/
        User        admin
        Password    Oracle123
        
    
    APEX-HR-Workspace
    
        URL         http://secureoracle.oracledemo.com:7001/ords/
        Workspace   HRSPACE
        User        hradmin
        Pass        Oracle123
        
    
    Meine HR-Anwendung:
    
        URL         http://secureoracle.oracledemo.com:7001/ords/f?p=100
        User        hradmin
        Password    Oracle123
        

## Aufgabe 2: Mitarbeiter-Onboarding und Benachrichtigungen

1.  Öffnen Sie den APEX-Workspace **HRSPACE** in einem Browser, und importieren Sie neue Mitarbeiter in "Meine HR-Anwendung".
    
    Beispiel: Verwenden Sie den folgenden Link und die folgenden Zugangsdaten:
    
        URL          http://secureoracle.oracledemo.com:7001/ords/
        Workspace    hrspace
        User         hradmin
        Password     Oracle123
        
2.  Klicken Sie im APEX-Workspace auf die Kachel **SQL Workshop** und dann auf **`Utilities -> Data Workshop`**. Klicken Sie auf der Seite "Data Workshop" auf **Daten laden**, wählen Sie **Datei auswählen** aus, und geben Sie den Speicherort der Datei **`import-new-employees.csv`** ein.
    
    **Hinweis**: Die Datei **`import-new-employees.csv`** kann aus **`/home/oracle/demo/sample-data` kopiert werden.**
    
3.  Nachdem die Datei analysiert wurde, wird der Abschnitt **Vorschau** angezeigt. Klicken Sie auf die Schaltfläche **Vorschau**, um eine vollständige Ansicht aller zu importierenden Spalten und Zeilen anzuzeigen. Fahren Sie fort, um das Fenster zu schließen.
    
4.  Geben Sie auf der Seite "Daten laden" die erforderlichen Informationen ein.
    
    Beispiel: Wählen Sie die folgenden Daten aus, bzw. geben Sie sie ein:
    
        Load To        Existing Table
        Table Owner    HR
        Table Name     EMPLOYEES
        Update Method  Append
        
5.  Klicken Sie auf die Schaltfläche **Daten laden**, um den Importprozess zu starten. Wenn der Import neuer Zeilen erfolgreich war, schließen Sie die Seite "Daten laden".
    
6.  Melden Sie sich vom APEX-Workspace ab, indem Sie in der oberen rechten Ecke auf den Avatar **HRADMIN** klicken und **Abmelden** auswählen.
    
7.  Öffnen Sie eine weitere Registerkarte, und melden Sie sich als Benutzer **hradmin** mit dem Kennwort **Oracle123** bei "Meine HR-Anwendung" an.
    
    Verwenden Sie z.B. den folgenden Link und die Zugangsdaten:
    
        URL         http://secureoracle.oracledemo.com:7001/ords/f?p=100
        User        hradmin
        Password    Oracle123
        
8.  Klicken Sie auf die Kachel **Mitarbeiter**, um die Seite "Mitarbeiter" zu öffnen und zu prüfen, ob Mitarbeiter in der folgenden Tabelle hinzugefügt wurden.
    
    Beispiel: Die folgenden neuen Mitarbeiter sollten nach Abschluss des vorherigen Importprozesses hinzugefügt werden:
    
        FULL NAME     USER ID   EMAIL                    DEARTMENT  TITLE       HIRE DATE    END DATE     MANAGER
        Denny Coby    DCOBY     dcoby@oracledemo.com     Sales      Sales Rep   26-Sep-2018               MGRAFF
        Gene Marton   GMARTON   gmarton@oracledemo.com   Finance    Accountant  26-Sep-2018               JSMITH
        Ray Lauria    RLAURIA   rlauria@oracledemo.com   Finance    Accountant  24-Sep-2018               JSMITH
        Robin Mainor  RMAINOR   rmainor@oracledemo.com   Finance    Accountant  03-Sep-2018  28-Sep-2019  JSMITH    
        
    
    **Hinweis:** E-Mail-Accounts für die oben genannten Mitarbeiter wurden bereits erstellt. Daher müssen Sie die E-Mail-Accounts nicht erstellen. Löschen Sie vorhandene Mitarbeiter nicht, es sei denn, Sie werden dazu aufgefordert, vorhandene OIM-Benutzer zu kündigen, die zu anderen Anwendungsfällen in der Demoumgebung gehören.
    
9.  Optional können Sie neue Mitarbeiter manuell erstellen, indem Sie auf die Schaltfläche **Erstellen** klicken. Beachten Sie jedoch, dass nur Mitarbeiter, die zu den Abteilungen **Finanzen** oder **Vertrieb** gehören, erfolgreich über die Abstimmungsaufgabe in OIM importiert werden sollten.
    
    Beispiel: Sie müssen mindestens die folgenden Daten angeben, um einen neuen Mitarbeiter zu erstellen:
    
        FIRST NAME, LAST NAME, USER NAME, STATUS, EMAIL, JOB, HIRE DATE, DEPARTMENT
        
    
    **Hinweis:** Bevor Sie einen neuen Mitarbeiter hinzufügen, müssen Sie zuerst einen E-Mail-Account erstellen, damit der neue Mitarbeiter eine Benachrichtigung erhält, nachdem er in OIM integriert wurde. Greifen Sie als Admin-Benutzer auf die **Administrationskonsole für E-Mail-Server** zu, um neue E-Mail-Accounts zu erstellen.
    

## Aufgabe 3: Neue Mitarbeiter einbinden

1.  Melden Sie sich als Benutzer **xelsysadm** mit dem Passwort **Oracle123** bei der [OIM Admin Console](http://secureoracle.oracledemo.com:14000/sysadmin) an. Klicken Sie auf die Option **Scheduler**, und gehen Sie zur Registerkarte "Systemverwaltung", und geben Sie **HRData** in das Suchfeld ein, oder klicken Sie auf das Pfeilsymbol, um Daten abzurufen.
    
2.  Doppelklicken Sie in der Ergebnisliste auf den Job **HRData DBAT Trusted Resource User Reconciliation** ![Suchergebnisse für OIM Admin Console Scheduler](./images/hrdata.png " ")
    
3.  Klicken Sie auf der Registerkarte "Jobdetails" auf **Jetzt ausführen**, und überwachen Sie den Fortschritt, indem Sie auf **Aktualisieren** klicken und unten unter "Jobhistorie" nachsehen.
    
4.  Nachdem der Job mit Jobstatus **Stopped::Success** abgeschlossen wurde, ![HRData DBAT Trusted Resource User Reconciliation-Servicedetails](./images/stopped.png " ")
    
5.  Klicken Sie oben auf die Registerkarte "Eventmanagement", und klicken Sie auf das Symbol **Pfeil**, um nach Abstimmungsereignissen zu suchen. Eine Liste der Ereignisse mit dem Profilnamen **HRData** sollte als letzte Ereignisse aufgeführt werden, die darauf hinweisen, dass Datensätze verarbeitet wurden. Klicken Sie auf eine der Ereigniskennungen, um die Details anzuzeigen. Wenn im aktuellen Status für alle Ereignisse **Erstellung erfolgreich** angezeigt wird, war der Import erfolgreich. Melden Sie sich von der Admin-Konsole ab. ![Registerkarte "Veranstaltung"](./images/creation.png " ")
    
    **Notizen**: Der Abstimmungsjob importiert neue Mitarbeiter, die nicht in OIM vorhanden sind. Optional können Sie eine gefilterte Abstimmung durchführen, indem Sie auf der Seite "Jobdetails" im Abschnitt **Parameter** einen Filterausdruck in das Feld **Filter** eingeben.
    
    Beispiel: Der folgende DBAT Connector-Filterausdruck gibt Mitarbeiter mit dem Titel **Buchhalter** zurück:
    
        contains('JOB_TITLE','Accountant')
        
    
    Die folgenden Spalten aus der Mitarbeitertabelle können in einem Filterausdruck verwendet werden:
    
        USERNAME, DEPARTMENT_NAME, FIRST_NAME, LAST_NAME, JOB_TITLE, PHONE_NUMBER, MANAGER, EMAIL, HIRE_DATE, END_DATE.
        

## Aufgabe 4: Integrierte Mitarbeiter prüfen

1.  Melden Sie sich als Benutzer bei der [OIM Self Service Console](http://secureoracle.oracledemo.com:14000/identity) an: **xelsysadm** mit dem Passwort: **Oracle123**. Klicken Sie auf **`Manage -> Users`**, und prüfen Sie, ob die integrierten Mitarbeiter auf der Seite "Benutzer" als Benutzer aufgelistet sind.
    
    ![OIM-Selfservicekonsole](./images/oim_users.png " ")
    
2.  Beachten Sie, dass **RMAINOR** für die Benutzeranmeldung auf der Seite "Benutzer" aufgeführt ist. Klicken Sie dann auf die Benutzeranmeldung, um die Seite "Benutzerdetails" zu öffnen. Wählen Sie die Registerkarte **Attribute** aus, und prüfen Sie das Attribut **Enddatum** für diesen Benutzer. Sein Enddatum lag nach dem aktuellen Datum. Dies bedeutet, dass dieser Benutzer bei der nächsten Jobausführung in **`Disable/Delete User After End Date`** deaktiviert und gelöscht wird. Schließen Sie nicht die Seite "Benutzerdetails", um mit dem nächsten Schritt fortzufahren.
    
3.  Öffnen Sie eine weitere Registerkarte in Ihrem Browser, und melden Sie sich bei der OIM-Administrationskonsole an.
    
4.  Klicken Sie auf **Scheduler**, und suchen Sie in der Registerkarte **Systemverwaltung** nach Job **`Disable/Delete User After End Date`**. Öffnen Sie die Seite "Jobdetails", und klicken Sie auf die Schaltfläche **Jetzt ausführen**, um den Job auszuführen.
    
    **Hinweis**: Standardmäßig ist die Ausführung dieses Jobs für jeden Tag geplant.
    
5.  Wenn der Job abgeschlossen ist, kehren Sie zur vorherigen Browserregisterkarte zur Selfservicekonsole zurück. Klicken Sie auf der Seite "Benutzerdetails" auf die Schaltfläche **Aktualisieren**, und prüfen Sie das Attribut **Status** der Benutzeranmeldung **RMAINOR**. Es sollte jetzt **Deaktiviert** angezeigt werden.
    
6.  Wenn Sie die Seite "Benutzerdetails" schließen und zur Seite "Benutzer" zurückkehren, um alle Benutzer aufzulisten, sollte die Benutzeranmeldung **RMAINOR** jetzt weg sein, da sie nicht mehr als Benutzer aufgeführt wurde.
    
7.  Melden Sie sich von der Selfservicekonsole ab. Dadurch wird auch die Session für die Admin-Konsole geschlossen.
    

## Aufgabe 5: Benachrichtigung und neue Zugangsdaten prüfen (optional)

1.  Optional können Sie sich beim E-Mail-Webclient **Roundcube** als Mitarbeiter **DCOBY** mit **Oracle123** als Kennwort anmelden.
    
    Beispiel: Anmeldung bei Roundcube Email Client:
    
        URL         http://secureoracle.oracledemo.com/roundcubemail-1.4.1/
        User        DCOBY
        Password    Oracle123
        Server      secureoracle.oracledemo.com
        
2.  Prüfen Sie den Posteingang. Der Mitarbeiter muss mindestens eine Benachrichtigung mit seinen neuen Zugangsdaten (UserID) und einen Link zum Zurücksetzen des Kennworts erhalten haben.
    
3.  Klicken Sie auf den Link **Klicken Sie hier, um das Kennwort zurückzusetzen**, um die Seite "OIM-Kennwortverwaltung" zu öffnen.
    
    **Hinweis**: Beachten Sie, dass der Link zum Zurücksetzen des Kennworts in der E-Mail-Benachrichtigung nur für einen Tag gültig ist.
    
4.  Geben Sie ein neues Kennwort ein, und beantworten Sie die geheimen Kennwortfragen. Geben Sie **Oracle123** als neues Kennwort ein.
    
5.  Klicken Sie auf die Schaltfläche **Weiterleiten**. Anschließend werden Sie zur Selfservicekonsole umgeleitet. Die Benutzerdetails werden in der Kachel **Meine Informationen** ausführlich erläutert.
    
6.  Melden Sie sich von der Selfservicekonsole ab.
    

## Aufgabe 6: Mitarbeitertransfer und Rollenaktualisierungen

1.  Melden Sie sich als Benutzer **hradmin** mit dem Kennwort **Oracle123** bei "Meine HR-Anwendung" an.
    
2.  Klicken Sie auf die Kachel **Mitarbeiter**, um die Seite "Mitarbeiter" zu öffnen. Wählen Sie einen Mitarbeiter aus, z.B. **DCOBY**, und klicken Sie auf das Symbol **Stift**, um die Mitarbeiterdetails zu bearbeiten.
    
    ![Meine HR-Anwendung](./images/img-myhr-app.png " ")
    
    Abbildung 2. Mitarbeiterdetails bearbeiten
    
3.  Fahren Sie fort, um die Abteilung von **Vertrieb** in **Finanzen** zu ändern, und klicken Sie auf **Änderungen anwenden**.
    
4.  Melden Sie sich als **xelsysadm** mit dem Passwort **Oracle123** bei der OIM-Administrationskonsole an. Klicken Sie auf die Option **Scheduler** und suchen Sie in der Registerkarte **Systemverwaltung** nach **`HRData*`**.
    
5.  Wählen Sie in der Ergebnisliste den Job **HRData DBAT Trusted Resource User Reconciliation** aus.
    
6.  Klicken Sie auf der Registerkarte "Jobdetails" auf **Jetzt ausführen**, und überwachen Sie den Fortschritt, indem Sie auf **Aktualisieren** klicken und unten unter "Jobhistorie" nachsehen.
    
7.  Nachdem der Job mit dem Jobstatus **Stopped::Success** abgeschlossen wurde, schließen Sie das Scheduler-Fenster, und melden Sie sich von der Admin-Konsole ab.
    

## Aufgabe 7: Prüfung von Abteilungswechsel und Rollenaktualisierungen

1.  Melden Sie sich als Benutzer **xelsysadm** mit dem Passwort **Oracle123** bei der OIM Self Service Console an. Klicken Sie auf **`Manage -> Users`**, und klicken Sie auf die Benutzeranmeldung **DCOBY**.
    
2.  Klicken Sie auf der Seite "Benutzerdetails" auf die Registerkarte **Attribute**, und prüfen Sie, ob der Organisationsname von "Vertrieb" in "Finanzen" geändert wurde.
    
3.  Da "Meine HR-Anwendung" eine zuverlässige Quelle ist, sind keine Genehmigungen für die Änderung der Organisation/Abteilung erforderlich.
    
4.  Klicken Sie auf die Registerkarte **Rollen**, und beachten Sie, dass dem Mitarbeiter jetzt die **Finanzrolle** zugewiesen werden muss.
    
5.  Melden Sie sich von der Selfservicekonsole ab.
    

## Aufgabe 8: Selfservice, Zugriffsanforderungen und Genehmigungen

1.  Melden Sie sich beim E-Mail-Webclient **Roundcube** als Mitarbeiter **RLAURIA** mit dem Kennwort **Oracle123** an, um die OIM-Benutzerzugangsdaten (UserID) abzurufen, und verknüpfen Sie den Link, um das Kennwort und geheime Kennwortfragen festzulegen.
    
2.  Nachdem der vorherige Schritt abgeschlossen ist, melden Sie sich bei der OIM-Selfservicekonsole an. Klicken Sie auf **`Request Access -> Request for Self`**.
    
3.  Wählen Sie auf der Anforderungsseite die Registerkarte **Katalog**, und klicken Sie anschließend auf die Optionsschaltfläche **Anwendung**.
    
4.  Geben Sie **Meine** in das Suchfeld ein, und klicken Sie auf **Suchen**.
    
5.  Wählen Sie in der Ergebnisliste **Mein OUD-Verzeichnis** aus, und klicken Sie auf die Schaltfläche **Zu Warenkorb hinzufügen**.
    
6.  Klicken Sie oben auf der Seite auf die Schaltfläche **Weiter**.
    
7.  Blenden Sie den Abschnitt **Warenkorbartikel** ein, wählen Sie das Element **Mein OUD-Verzeichnis**, und klicken Sie unter "Anforderungsdetails" auf das Symbol **Bleistift**.
    
8.  Geben Sie die Anwendungsdetails als genehmigt ein.
    
    Beispiel: Geben Sie die folgenden Daten für den Benutzer **RLAURIA** ein
    
        	ATTRIBUTE       VALUE
        	Container DN    OUDDirectory~iam
        
    
    **Hinweis:** Der Container-DN entspricht einem Speicherort im OUD-Verzeichnis, der von OAM zum Speichern von Benutzeraccounts verwendet wird. Das bedeutet, dass der Benutzer durch Provisioning dieses Accounts einen gültigen Account in OAM haben würde. Die verbleibenden Attribute wie **Benutzer-ID**, **Kennwort**, **Vorname**, **Nachname**, **E-Mail** und **Allgemeiner Name** werden während des Provisioning-Vorgangs automatisch von OIM aufgefüllt.
    
9.  Klicken Sie auf **Aktualisieren**, **Weiterleiten** oben auf der Seite, um die Zugriffsanforderung weiterzuleiten.
    
10.  Gehen Sie zurück zur Registerkarte **Selfservice**, und klicken Sie auf die Kachel **Anforderungen verfolgen**. Wählen Sie auf der Seite "Anforderung verfolgen" im Feld **Suchen** die Option **Status** und im zweiten Feld die Option **Anforderung mit ausstehender Genehmigung**, und klicken Sie auf das Symbol **Lupe**.
    
11.  Die vom Benutzer weitergeleitete Anforderung sollte aufgeführt werden. Klicken Sie auf den Link **Anforderungs-ID**, um die Details anzuzeigen.
    
12.  Klicken Sie auf der Seite mit den Anforderungsdetails auf die Registerkarte **Genehmigungsdetails**, um den **Aufgabenstatus** und die **Bearbeiter** zu prüfen. Der Beauftragte sollte der Manager von RLAURIA **JSMITH** sein.
    
13.  Schließen Sie die Seite, und melden Sie sich von der Selfservicekonsole ab.
    

## Aufgabe 9: Anforderung mit umsetzbaren Benachrichtigungen genehmigen

1.  Melden Sie sich beim E-Mail-Webclient **Roundcube** als Manager **JSMITH** mit **Oracle123** als Kennwort an.
    
    Beispiel: Anmeldung beim Roundcube Email Client:
    
        	URL         http://secureoracle.oracledemo.com/roundcubemail-1.4.1/
        	User        JSMITH
        	Password    Oracle123
        	Server      secureoracle.oracledemo.com
        
2.  Prüfen Sie den Posteingang. Der Manager sollte eine Benachrichtigung erhalten haben, in der er um Genehmigung für die Zugriffsanforderung gebeten wurde, die vom Mitarbeiter **RLAURIA** weitergeleitet wurde.
    
3.  Öffnen Sie die E-Mail, und prüfen Sie, ob der Anforderer **Ray Lauria** ist. Klicken Sie anschließend auf den Link **Genehmigen**.
    
    ![Roundcube-Benachrichtigungs-Mail zur Genehmigung](./images/uc03-action-notification.png " ")
    
    Abbildung 3. Benachrichtigung über Maßnahmen - Genehmigungen
    
4.  Geben Sie in der Antwortbenachrichtigung eine Notiz zwischen Klammern im Abschnitt **Kommentare** ein. Beispiel: **Anforderung genehmigt**. Klicken Sie dann auf die Schaltfläche **Senden**, um die E-Mail zu senden.
    
5.  Beachten Sie, dass die Zieladresse **`SOA@oracledemo.com`** lautet. Dies bedeutet, dass die Benachrichtigung vom SOA-Server als Benachrichtigung verarbeitet und dann an OIM weitergeleitet wird, um die Anforderung zu aktualisieren.
    
6.  Melden Sie sich vom E-Mail-Webclient ab.
    

## Aufgabe 10: Prüfen des angeforderten Anwendungszugriffs

1.  Melden Sie sich als Benutzer **RLAURIA** bei der OIM-Selfservicekonsole an. Klicken Sie auf die Kachel **Mein Zugriff**.
    
2.  Klicken Sie auf meiner Zugriffsseite auf die Registerkarte **Accounts**, und bestätigen Sie, ob die Anwendung **Mein OUD-Verzeichnis** aufgeführt ist.
    
3.  Wählen Sie die Zeile **Mein OUD-Verzeichnis** aus, und prüfen Sie die Details auf der Registerkarte **Detailinformationen**.
    
4.  Melden Sie sich über die Selfservicekonsole ab.
    
    ![OIM-Selfservice](./images/uc03-account-provisioned.png " ")
    
    Abbildung 4. Bereitgestelltes Konto - Zugriffsanforderung
    

## Aufgabe 11: Account in der Zielanwendung prüfen (optional)

1.  Dieser Schritt ist optional und erfordert, dass der OAM-Server ausgeführt wird, um den bereitgestellten Account zu testen. Da Sie die OIM-Komponenten bereits gestartet haben, benötigen Sie mindestens 48 GB Gesamtspeicher in Ihrer Umgebung, um die OAM-Serverkomponenten zusätzlich zu OIM starten zu können.
    
    Beispiel: Melden Sie sich als Benutzer **oracle** an, und führen Sie den folgenden Befehl aus, um OAM zu starten:
    
        <copy>sc start oam</copy>
        
    
    **Hinweis**: Die Startzeit der OAM-Komponenten variiert zwischen 10 und 15 Minuten.
    
2.  Nachdem OAM gestartet wurde, melden Sie sich bei der OAM-Konsole an, indem Sie die Zugangsdaten für den Benutzer **RLAURIA** eingeben.
    
    Beispiel: Anmeldung bei der Oracle Access Manager-Administrationskonsole:
    
        	URL         http://secureoracle.oracledemo.com:8001/oamconsole
        	User        RLAURIA
        	Password    Oracle123
        
    
    **Hinweis**: Wir verwenden dasselbe Kennwort wie beim Provisioning der Zugriffsanforderung. OIM hat diesen Wert aus dem OIM-Benutzerprofil in den neuen Account kopiert.
    
3.  Wenn die Accountzugangsdaten korrekt sind, sollte dem Benutzer eine leere OAM-Seite angezeigt werden. Dies liegt daran, dass der Benutzeraccount keine Administratorberechtigungen für die Konsole hat.
    
4.  Melden Sie sich von der OAM-Konsole ab.
    

## Aufgabe 12: Mitarbeiterkündigung

1.  Melden Sie sich als Benutzer **hradmin** mit dem Kennwort **Oracle123** bei "Meine HR-Anwendung" an.
    
2.  Klicken Sie auf die Kachel **Mitarbeiter**, um die Seite "Mitarbeiter" zu öffnen. Wählen Sie einen Mitarbeiter aus, z.B. **Denny Coby**, und klicken Sie auf das Symbol **Stift**, um die Mitarbeiterdetails zu bearbeiten.
    
3.  Klicken Sie zum Löschen des Benutzers auf die Schaltfläche **Löschen**, und klicken Sie auf **OK**, um den Löschvorgang zu bestätigen.
    
4.  Melden Sie sich als Benutzer **xelsysadm** mit dem Passwort **Oracle123** bei der OIM-Administrationskonsole an. Klicken Sie auf die Option **Scheduler**, und suchen Sie auf der Registerkarte "Systemverwaltung" nach **`HRData*`**.
    
5.  Wählen Sie in der Ergebnisliste den Job **HRData DBAT Trusted Resource User Delete Reconciliation** aus.
    
6.  Klicken Sie auf der Registerkarte "Jobdetails" auf **Jetzt ausführen**, und überwachen Sie den Fortschritt, indem Sie auf **Aktualisieren** klicken und unten unter "Jobhistorie" nachsehen.
    
7.  Nachdem der Job mit dem Jobstatus **`Stopped::Success`** abgeschlossen wurde, klicken Sie oben auf die Registerkarte "Eventmanagement", und klicken Sie auf das Symbol **Pfeil**, um nach Abstimmungsereignissen zu suchen.
    
8.  Eine Liste der Ereignisse mit dem Profilnamen **HRData** sollte als letzte Ereignisse aufgeführt werden, die darauf hinweisen, dass Datensätze verarbeitet wurden. Klicken Sie auf die letzte Ereignis-ID, um die Details anzuzeigen. Wenn im aktuellen Status **Löschen erfolgreich** für das Ereignis angezeigt wird, wurde der Mitarbeiter erfolgreich gelöscht. Melden Sie sich von der Admin-Konsole ab.
    

## Aufgabe 13: Gekündigte Mitarbeiter prüfen

1.  Melden Sie sich als Benutzer **xelsysadm** mit dem Passwort **Oracle123** bei der OIM Self Service Console an. Klicken Sie auf **`Manage -> Users`**, und prüfen Sie, ob der gekündigte Mitarbeiter nicht als Benutzer auf der Benutzerseite aufgeführt ist.
    
2.  Melden Sie sich von der Selfservicekonsole ab.
    

## **Anhang**: Informationen zu Genehmigungsworkflows

1.  Die Generierung und Genehmigung von OIM-Anforderungen hängt von der Verwendung und Konfiguration von Workflowregeln ab. Workflow-Regeln bestimmen Folgendes:
    
    *   Gibt an, ob Genehmigungen für einen Vorgang erforderlich sind
    *   Welcher Workflow für einen bestimmten Vorgang aufgerufen werden muss
2.  Zur Demonstration von Managergenehmigungen in **`UC03. Self Service, Access Request and Approvals`** haben wir dem Vorgang **Provisioning ApplicationInstance** in OIM eine Workflowregel hinzugefügt.
    
    Beispiel: Die folgende Regel wurde hinzugefügt und als erste festgelegt, um vor der Standardregel ausgewertet zu werden:
    
        	RULE NAME   Provision Account Organization Rule
        	CONDITION   IF user.Organization Name Equal Sales OR user.Organization Name Equal Finance
        	            THEN workflow Equal default/RequesterManagerApproval!3.0
        
3.  Bei der Auswertung von Workflow-Regeln muss Folgendes berücksichtigt werden:
    
    *   Genehmigungsworkflowregeln, die dem ausgeführten Vorgang zugeordnet sind, werden einzeln in der Reihenfolge ausgewertet, in der sie konfiguriert sind.
    *   Die Auswertung der Genehmigungsworkflowregeln wird bei der ersten Abgleichsregel gestoppt. Dies ist die Regel, die als "true" ausgewertet wird, und das Ergebnis dieser Regel wird als Ergebnis zurückgegeben.
    *   Wenn bei allen Nicht-Massenvorgängen keine der Regeln übereinstimmt, wird die in **defaultOperationApprovalComposite** von **SOAConfig** konfigurierte SOA-Composite Application implizit zurückgegeben. Dies führt in der Regel dazu, dass eine Genehmigungsanforderung an `SYSTEM ADMINISTRATOR` gesendet wird.
4.  Workflowregeln können aus einer Quellumgebung, z.B. einer Testumgebung, exportiert und mit Deployment Manager in eine Zielumgebung, z.B. eine Produktionsumgebung, importiert werden.
    
5.  Weitere Informationen zu den vom OIM-System definierten Vorgängen und Regeln finden Sie unter dem folgenden [Link](https://docs.oracle.com/en/middleware/idm/identity-governance/12.2.1.3/omadm/managing-workflows.html#GUID-F21C1DC9-49A3-48F1-820C-2422CB24678A).
    

Sie können jetzt _mit der nächsten Übung fortfahren_.

## Weitere Informationen

Über diese Links erhalten Sie weitere Informationen zu Oracle Identity and Access Management:

*   [Oracle Identity Management-Website](https://docs.oracle.com/en/middleware/idm/suite/12.2.1.4/index.html)
*   [Oracle Identity Governance-Dokumentation](https://docs.oracle.com/en/middleware/idm/identity-governance/12.2.1.4/index.html)
*   [Oracle Access Management-Dokumentation](https://docs.oracle.com/en/middleware/idm/access-manager/12.2.1.4/books.html)

## Danksagungen

*   **Autor** - Ricardo Gutierrez, Solution Engineering - Sicherheit und Management
*   **Mitwirkende** - Rene Fontcha
*   **Zuletzt aktualisiert am/um** - Sahaana Manavalan, LiveLabs Developer, NA Technology, März 2022