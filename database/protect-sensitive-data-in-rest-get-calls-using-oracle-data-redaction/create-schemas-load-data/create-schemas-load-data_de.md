# Autonomous Database-Umgebung konfigurieren

## Einführung

In dieser Übung untersuchen wir Oracle Cloud Platform (OCI) und konfigurieren die Autonomous Transaction Processing-Datenbankinstanz (ATP). Danach erstellen wir mit dem Skript `employee_data_load.sql` den Benutzer `EMPLOYEESEARCH_PROD` und füllen dieses Datenbankschema mit HR-Mitarbeiterdaten mit **Oracle Database Actions** auf.

Weitere Informationen zur Autonomous Transaction Processing-Datenbank finden Sie [hier](https://www.oracle.com/autonomous-database/autonomous-transaction-processing/).

Weitere Informationen zu OCI Database Actions erhalten Sie [hier](https://www.oracle.com/database/sqldeveloper/technologies/db-actions/).

Geschätzte Zeit: 10 Minuten

### Ziele

In dieser Übung führen Sie die folgenden Aufgaben aus:

*   ATP-Datenbankinstanz erstellen
*   Verwenden Sie das Skript `employee_data_load.sql`, um den Benutzer `EMPLOYEESEARCH_PROD` zu erstellen und Daten hochzuladen.

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Oracle Cloud Infrastructure-(OCI-)Mandantenaccount
*   Alle vorherigen Übungen im Workshop **Sensible Daten in REST-GET-Aufrufen mit Oracle Data Redaction schützen** LiveLab abgeschlossen

## Aufgabe 1: ATP-Datenbankinstanz erstellen

1.  Navigieren Sie bei geöffneter OCI-Konsole zum ATP-Portal, indem Sie das Hamburger-Menü in der oberen linken Ecke auswählen. So können Sie **Oracle Database** und dann **Autonomous Transaction Processing** auswählen.
    
    ![Verfügbarkeitsprüfung im OCI-Menü auswählen](images/select-the-atp-menu.png)
    
2.  Wählen Sie **Autonomous Database erstellen** aus.
    
    ![Wählen Sie "Create Autonomous Database" aus.](images/create-autonomous-database.png)
    
3.  Verwenden Sie ein Compartment Ihrer Wahl, und geben Sie den Anzeigenamen und den Datenbanknamen **RedactionDB** ein.
    
    ![Datenbanknamen eingeben](images/db-name.png)
    
4.  Erstellen Sie ein Kennwort für die **ADMIN**\-Zugangsdaten.
    
    ![Administratorzugangsdaten eingeben](images/atp-password.png)
    
5.  Ändern Sie den Netzwerkzugriff auf **nur zulässige IPs und VCNs**, und ändern Sie den IP-Notationstyp in **CIDR-Block. Geben Sie den CIDR-Wert 0.0.0.0/0 in das leere Feld ein.** Stellen Sie sicher, dass die Option **Gegenseitige TLS-(mTLS-)Authentifizierung erforderlich bleibt** nicht aktiviert ist.
    
    ![Administratorzugangsdaten eingeben](images/secure-access.png)
    
6.  Wählen Sie die Lizenzierungsoption Ihrer Wahl aus, und wählen Sie unten **Autonomous Database erstellen** aus. _Hinweis: Das Hochfahren der ADB kann einige Minuten dauern._
    
    ![Schaltfläche "DB erstellen" unten](images/create-the-atp.png)
    

## Aufgabe 2: Verwenden Sie das Skript `employee_data_load.sql`, um den Benutzer `EMPLOYEESEARCH_PROD` zu erstellen und Daten hochzuladen.

1.  Wenn Autonomous Database grün und verfügbar ist, navigieren Sie zur oberen Menüleiste des Autonomous Database-Dashboards, und wählen Sie **Database Actions** aus.
    
    ![Gehe zu Datenbankaktionen](images/db-actions.png)
    
2.  Melden Sie sich mit den von Ihnen erstellten **ADMIN**\-Zugangsdaten bei **Database Actions** an.
    
    ![Bei DB-Aktionen anmelden](images/db-login.png)
    
3.  Wählen Sie im Abschnitt **Entwicklung** die Option **SQL** aus.
    
    ![SQL-Arbeitsblatt](images/sql-worksheet.png)
    
4.  Verwenden Sie die folgende URL, um das Skript `employee_data_load.sql` herunterzuladen und zu speichern:
    
        <copy>https://objectstorage.us-ashburn-1.oraclecloud.com/p/VEKec7t0mGwBkJX92Jn0nMptuXIlEpJ5XJA-A6C9PymRgY2LhKbjWqHeB5rVBbaV/n/c4u04/b/livelabsfiles/o/data-management-library-files/employee_data_load.sql</copy>   
        
5.  Wählen Sie oben in der Menüleiste das Ordnersymbol, um eine Datei zu öffnen.
    
    ![Datei öffnen](images/folder-icon.png)
    
6.  Wählen Sie **Datei öffnen**, und laden Sie das Skript `employee_data_load.sql` hoch.
    
    ![Datei auswählen](images/open-file.png)
    
7.  Nachdem das Skript in das **SQL-Arbeitsblatt** geladen wurde, wählen Sie das Skriptsymbol, um das SQL-Skript auszuführen. Prüfen Sie die Skriptausgabe unten, um sicherzustellen, dass keine Fehler empfangen wurden.
    
    ![Skript ausführen](images/run-script.png)
    
8.  Kehren Sie zum Haupt-Dashboard für **Database Actions** zurück, indem Sie das **Oracle**\-Logo oben links auf dem Bildschirm auswählen.
    
    ![Zurück zum Dashboard](images/return-to-dash.png)
    
9.  Scrollen Sie nach unten. Wählen Sie unter **Administration** die Option **DATABASE USERS** aus.
    
    ![Datenbankbenutzer auswählen](images/select-db-users.png)
    
10.  Wählen Sie unter dem Benutzer `EMPLOYEESEARCH_PROD` die Auslassungspunkte aus, und klicken Sie auf **Benutzer bearbeiten**.
    
    ![Benutzer bearbeiten](images/edit-user.png)
    
11.  Stellen Sie unter **Benutzer** sicher, dass **Webzugriff** **ein** aktiviert ist, und wählen Sie unten rechts im Popup-Menü die Option **Änderungen anwenden** aus.
    
    ![Webzugriff umschalten](images/web-access.png)
    
12.  Öffnen Sie das Portal **Database Actions** für `EMPLOYEESEARCH_PROD`.
    
    ![DB-Aktionen als Emp öffnen](images/db-actions-emp.png)
    
13.  Melden Sie sich mit den folgenden Zugangsdaten als `EMPLOYEESEARCH_PROD` bei **Database Actions** an:
    
        <copy>EMPLOYEESEARCH_PROD</copy>   
        
    
        <copy>Oracle123+Oracle123+</copy>
        

Sie können jetzt **mit der nächsten Übung fortfahren.**

## Danksagungen

*   **Autoren** - Alpha Diallo & Ethan Shmargad, North America Specialists Hub
*   **Ersteller** - Pedro Lopes, Produktmanager für Datenbanksicherheit
*   **Zuletzt aktualisiert am/um** - Alpha Diallo & Ethan Shmargad, Februar 2023