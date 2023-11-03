# Verbindungen zum EMPLOYEESEARCH\_PROD-Schema identifizieren

## Einführung

In dieser Übung erstellen wir eine Realm in unserer ATP-Instanz. Wir halten unsere Realm im **Simulationsmodus**. Dadurch können wir wiederum eine Aufzeichnung von Fehlern während der Entwicklungsphase einer Realm oder Befehlsregel erfassen. Im Simulationsmodus identifizieren wir dann unsere Verbindungen zum Schema `EMPLOYEESEARCH_PROD`.

### Ziele

In dieser Übung führen Sie die folgenden Aufgaben aus:

*   Erstellen Sie eine Database Vault Realm.
*   Verwenden Sie den **Simulationsmodus**, um die Verbindungen zum Schema `EMPLOYEESEARCH_PROD` (App- und Nicht-App-Verbindungen) zu identifizieren.

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Oracle Cloud Infrastructure-(OCI-)Mandantenaccount
*   Abschluss der folgenden vorherigen Übungen: Autonomous Database-Instanz konfigurieren, Verbindung zur Legacy-Anwendung Glassfish HR herstellen, Daten in der Glassfish-Anwendung laden und prüfen, Database Vault aktivieren und HR-Anwendung prüfen

## Aufgabe 1: Database Vault-Realm erstellen

1.  Navigieren Sie zurück zum **SQL-Arbeitsblatt von Database Actions**. Wählen Sie in der Menüleiste für `ADMIN` oben rechts die Option **Abmelden**. Melden Sie sich unter dem Benutzer `sec_admin_owen` wieder bei Database Actions an, und wählen Sie unter **Development** die Option **SQL** aus. Stellen Sie sicher, dass das Arbeitsblatt gelöscht ist und Ihr Schema von **ADMIN** in `SEC_ADMIN_OWEN` aktualisiert wurde.
    
    ![DB-Aktionsschema ändern](images/change-schema-dbactions.png)
    
2.  Kopieren Sie das folgende Skript, und fügen Sie es in das SQL-Arbeitsblatt ein, um eine Database Vault-Realm `PROTECT_MYHRAPP` zu erstellen, um `EMPLOYEESEARCH_PROD` zu schützen. Wählen Sie die Schaltfläche **Skript ausführen**, um das Skript auszuführen. Prüfen Sie die Skriptausgabe unten, um sicherzustellen, dass das Skript erfolgreich ausgeführt wurde.
    
        	<copy>BEGIN
        			DVSYS.DBMS_MACADM.CREATE_REALM(
        				realm_name => 'PROTECT_MYHRAPP'
        				,description => 'A mandatory realm, in simulation mode, to show how EMPLOYEESEARCH_PROD would be protected'
        				,enabled => DBMS_MACUTL.G_SIMULATION
        				,audit_options => DBMS_MACUTL.G_REALM_AUDIT_FAIL
        				,realm_type => 1); 
        		END;
        		/</copy>
        
    
    ![DVD-Realm erstellen](images/create-realm.png)
    
3.  Löschen Sie das SQL-Arbeitsblatt. Kopieren Sie die folgende Abfrage, und fügen Sie sie ein. Prüfen Sie die Ausgabe, um die aktuelle Database Vault-Realm anzuzeigen, die erstellt wurde.
    
        	<copy>SELECT name, description, enabled FROM dba_dv_realm WHERE id# >= 5000 ORDER BY 1;</copy>
        
    
    ![DVD-Realm anzeigen](images/show-dv-realm.png)
    
4.  Löschen Sie das SQL-Arbeitsblatt. Kopieren Sie das folgende Skript, und fügen Sie es ein, um das Schema `EMPLOYEESEARCH_PROD` und alle seine Objekte zur Database Vault Realm `PROTECT_MYHRAPP` hinzuzufügen. Dies gilt für vorhandene und alle neuen Objekte, die im Schema `EMPLOYEESEARCH_PROD` erstellt wurden. Prüfen Sie die Ausgabe, um sicherzustellen, dass das Skript erfolgreich ausgeführt wurde.
    
        <copy>BEGIN
           DVSYS.DBMS_MACADM.ADD_OBJECT_TO_REALM(
               realm_name   => 'PROTECT_MYHRAPP',
               object_owner => 'EMPLOYEESEARCH_PROD',
               object_name  => '%',
               object_type  => '%');
        END;
        /</copy>
        
    
    In der nächsten Aufgabe erfassen wir den Zugriff auf Objekte im Schema `EMPLOYEESEARCH_PROD`, einschließlich des Zugriffs durch das Schema selbst. Dies hilft uns zu verstehen, welche Objekte verfügbar sind und wo sie abgerufen werden.
    
5.  Navigieren Sie zurück zur Anwendung **HR Production**, und melden Sie sich ab, und melden Sie sich dann als **hradmin** wieder an. Prüfen Sie, ob die Daten noch angezeigt werden und verfügbar sind. Klicken Sie auf die Menüregisterkarten für Mitarbeiterprofile, um sicherzustellen, dass alle Daten vorhanden sind.
    
    ![Mitarbeiter suchen](images/search-emp.png)
    
    ![Mitarbeiter auswählen](images/select-emp.png)
    
    ![Mitarbeiter navigieren](images/navigate-emp.png)
    

## Aufgabe 2: Verwenden Sie den Simulationsmodus, um die Verbindungen zum Schema EMPLOYEESEARCH\_PROD zu identifizieren

1.  Kopieren Sie als `SEC_ADMIN_OWEN` die folgende Abfrage, und fügen Sie sie ein, um `dba_dv_simulation_log` anzuzeigen. Stellen Sie sicher, dass `EMPLOYEESEARCH_PROD` der **Objekteigentümer** ist. Beachten Sie, wie das Schema `EMPLOYEESEARCH_PROD` als **Realm-Verletzung** bezeichnet wird. Database Vault wird so konfiguriert, dass `EMPLOYEESEARCH_PROD` das einzige Schema ist, das auf die Daten zugreifen kann.
    
        	<copy>select * from dba_dv_simulation_log;</copy>
        
    
    ![Simulationslogs anzeigen](images/sec-view-log.png)
    
2.  Wählen Sie in der Menüleiste für `SEC_ADMIN_OWEN` oben rechts die Option **Abmelden**. Melden Sie sich unter dem Benutzer `dba_debra` wieder bei Database Actions an, und wählen Sie unter **Entwicklung** die Option **SQL** aus. Stellen Sie sicher, dass das Arbeitsblatt leer ist und Ihr Schema auf `DBA_DEBRA` aktualisiert wurde.
    
    ![Dba debra Anmeldung](images/dba-deb-login.png)
    
3.  Kopieren Sie die folgenden Befehle, und fügen Sie sie ein, um die Tabelle `DEMO_HR_EMPLOYEES` unter dem Schema `EMPLOYEESEARCH_PROD` abzufragen.
    
        	<copy>alter session set current_schema = EMPLOYEESEARCH_PROD;</copy>
        
    
        	<copy>select a.USERID, a.FIRSTNAME, a.LASTNAME, a.EMAIL, a.PHONEMOBILE, a.PHONEFIX, a.PHONEFAX, a.EMPTYPE, a.POSITION, a.ISMANAGER, a.MANAGERID, a.DEPARTMENT, a.CITY, a.STARTDATE, a.ENDDATE, a.ACTIVE, a.COSTCENTER, b.FIRSTNAME as MGR_FIRSTNAME, b.LASTNAME as MGR_LASTNAME, b.USERID as MGR_USERID from DEMO_HR_EMPLOYEES a left outer join DEMO_HR_EMPLOYEES b on a.MANAGERID = b.USERID where 1=1 order by a.LASTNAME, a.FIRSTNAME</copy>
        
    
    ![Abfrage mit DBA](images/dba-query.png)
    
    Beachten Sie, wie DBA Objekte im Schema EMPLOYEESEARCH\_PROD abfragen kann, d.h. dieselben Daten, die in der HR-Anwendung angezeigt werden. Dies ist ein Beispiel dafür, was beim Wechsel der Database Vault-Realm von der Simulation in den Enforcement-Modus untersagt wird. Dadurch wird auch der Zugriff für privilegierte Benutzer wie `ADMIN` und Benutzer mit der Rolle `PDB_DBA` deaktiviert
    
4.  Melden Sie sich von `DBA_DEBRA` ab und wieder als `SEC_ADMIN_OWEN` bei Datenbankaktionen an. Wählen Sie unter **Developemnt** die Option **SQL** aus. Kopieren Sie den folgenden Befehl, fügen Sie ihn ein, und führen Sie ihn aus, um die Tabelle `dba_dv_simulation_log` abzufragen.
    
        	<copy>select username, violation_type, realm_name, object_owner, object_name, client_ip, dv$_module, dv$_client_identifier from dba_dv_simulation_log;</copy>
        
    
    ![Query Sim-Logs](images/query-sim-log.png)
    
    Diese Abfrage zeigt die letzte Verwendung und Verbindungen zu Objekten unter der erstellten Datenbank-Vault-Realm an. Beachten Sie, dass Verbindungen von Benutzern wie `DBA_DEBRA, SEC_ADMIN_OWEN, and ADMIN` vorhanden sind. Diese Benutzer außer für `EMPLOYEESEARCH_PROD` sind untersagt, sobald wir Realm-Autorisierung für `EMPLOYEESEARCH_PROD` bereitstellen. Beachten Sie auch die Spalte `DV$_MODULE`. Dadurch werden die Verbindungsmethoden zu den `EMPLOYEESEARCH_PROD`\-Objekten angezeigt. Die einzige Verbindung, der wir vertrauen werden, sind die Verbindungen von `JDBC Thin Client`, der HR-Anwendungsverbindung. Alle anderen Verbindungen werden mit der erstellten Realm gesperrt.
    
5.  Löschen Sie die Tabelle `dba_dv_simulation_log` mit den folgenden Befehlen.
    
        	<copy>delete from dvsys.simulation_log$;</copy>
        
    
        	<copy>commit;</copy>
        
6.  Stellen Sie sicher, dass das SQL-Arbeitsblatt leer ist. Kopieren Sie den folgenden PL\*SQL-Block, fügen Sie ihn ein, und führen Sie ihn aus, um Realm `EMPLOYEESEARCH_PROD` als autorisierten Teilnehmer in der Realm `PROTECT_MYHRAPP` hinzuzufügen. Dadurch kann das Schema `EMPLOYEESEARCH_PROD` auf seine eigenen Objekte zugreifen, ohne dass es in den Simulationslogs als Realm-Verletzung angezeigt wird, und stellt sicher, dass es auf die Objekte zugreifen kann, wenn wir vom Simulationsmodus in den Enforcement-Modus wechseln.
    
        	<copy>
        	begin
        		DVSYS.DBMS_MACADM.ADD_AUTH_TO_REALM(
        			realm_name => 'PROTECT_MYHRAPP'
        			, grantee => 'EMPLOYEESEARCH_PROD'
        			, rule_set_name => null
        			, auth_options => DBMS_MACUTL.G_REALM_AUTH_OWNER); 
        	end;
        	/</copy>
        
    
    ![Realm-Berechtigung hinzufügen](images/add-auth.png)
    
7.  Navigieren Sie zurück zur **HR-Produktionsanwendung**, und melden Sie sich ab, und melden Sie sich dann wieder als **hradmin** an. Werfen Sie einen weiteren Blick auf die Mitarbeiterdaten, um sicherzustellen, dass sie noch vorhanden sind und die Anwendung weiterhin ordnungsgemäß funktioniert.
    
    ![Mitarbeiter suchen](images/search-emp.png)
    
    ![Mitarbeiter auswählen](images/select-emp.png)
    
8.  Öffnen Sie **Database Actions** in Oracle Cloud als Backup. Kopieren Sie als `sec_admin_owen` den folgenden Befehl, fügen Sie ihn ein, und führen Sie ihn aus, um die Tabelle `dba_dv_simulation_log` abzufragen.
    
        	<copy>select * from dba_dv_simulation_log;</copy>
        
    
    ![Mitarbeiter suchen](images/log-query.png)
    
    Jetzt, da wir Realm-Autorisierung zu `EMPLOYEESEARCH_PROD` hinzugefügt haben. Eingehende Verbindungen vom **JDBC-Client** oder der HR-Anwendung gelten nicht mehr als Realm-Verletzung, da sie jetzt ein vertrauenswürdiger Benutzer und eine vertrauenswürdige Verbindung sind.
    
9.  Speichern Sie die cloud shell-Konsole um. Verwenden Sie die folgenden te-Befehle, um das Skript `dv_query_employee_search.sh` als **ADMIN** auszuführen.
    
    _Hinweis: Wenn Sie aufgrund von Inaktivität von Ihrer Glassfish-Instanz abgemeldet wurden, melden Sie sich mit dem folgenden Befehl erneut an. Die öffentliche IP-Adresse finden Sie im Instanzdetail-Dashboard in Oracle Cloud:_
    
        	<copy>chmod 600 myhrappkey</copy>
        
    
        <copy>ssh -i myhrappkey opc@<PASTE INSTANCE PUBLIC IP ADDRESS HERE></copy>
        
    
    Im Folgenden werden die Befehle zum Ausführen von `dv_query_employee_search.sh` aufgeführt:
    
        	<copy>cd cd secure-legacy-app-db-vault</copy>
        
    
        	<copy>./dv_query_employee_search.sh</copy>
        
    
    ![Abfrage mit Admin](images/bash-query.png)
    
10.  Minimieren Sie Cloud Shell, und navigieren Sie zurück zur Registerkarte **Database Actions** in Oracle Cloud. Kopieren Sie als `sec_admin_owen` den folgenden Befehl, fügen Sie ihn ein, und führen Sie ihn aus, um die Tabelle `dba_dv_simulation_log` erneut abzufragen.
    
        <copy>select * from dba_dv_simulation_log;</copy>
        
    
    ![Abfrage nach Admin](images/post-admin-query.png)
    

Da `ADMIN` auf `EMPLOYEESEARCH_PROD`\-Datenbankobjekte zugegriffen hat, wird es im Database Vault-Simulationslog als Realm-Verletzung angezeigt. Als Nächstes verschieben wir die Realm von **Simulation in Enforcement-Modus** und stellen fest, dass privilegierte Benutzer wie `ADMIN` und `DBA_DEBRA` nicht mehr über ausreichende Berechtigungen zum Abfragen von `EMPLOYEESEARCH_PROD`\-Objekten verfügen.

Sie können jetzt **mit der nächsten Übung fortfahren.**

## Weitere Informationen

*   [Database Vault Realms konfigurieren](https://docs.oracle.com/database/121/DVADM/cfrealms.htm#DVADM003)
*   [Simulationsmodus für Oracle Databse Vault](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/dvadmusing-training-mode-to-log-realm-and-command-rule-activities.html)

## Danksagungen

*   **Autor** - Ethan Shmargad, North America Specialists Hub
*   **Ersteller** - Richard Evans, Senior Principle Product Manager
*   **Zuletzt aktualisiert am/um** - Ethan Shmargad, September 2022