# Funktionen der Glassfish HR-Anwendung mit aktiviertem Database Vault kennenlernen

## Einführung

In dieser Übung wechseln wir jetzt unsere Database Vault Realm vom **Simulationsmodus** in den **Durchsetzungsmodus**. Dies wird unser Reich in die Produktion bringen. Anschließend testen wir unsere Realm und demonstrieren die Unfähigkeit, die Anwendungsdaten unter einem bestimmten Benutzer abzufragen.

### Ziele

In dieser Übung führen Sie die folgenden Aufgaben aus:

*   Flip der Database Vault-Realm vom **Simulations** in den **Durchsetzungs**modus
*   Zeigen Sie, dass die Daten in `EMPLOYEESEARCH_PROD` nicht mit `DBA_DEBRA` und `ADMIN` abgefragt werden können.

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Oracle Cloud Infrastructure-(OCI-)Mandantenaccount
*   Abschluss der folgenden vorherigen Übungen: Autonomous Database-Instanz konfigurieren, Verbindung zur Legacy-Anwendung Glassfish HR herstellen, Daten in der Glassfish-Anwendung laden und prüfen, Database Vault aktivieren und HR-Anwendung prüfen, Verbindungen zum EMPLOYEESEARCH\_PROD-Schema identifizieren

## Aufgabe 1: Database Vault-Realm von der Simulation in den Enforcement-Modus umwandeln

1.  Führen Sie unter **Database Actions** als `sec_admin_owen` die folgende Abfrage der Tabelle `dvsys.dba_dv_realm_auth` aus.
    
        	<copy>select grantee from dvsys.dba_dv_realm_auth where realm_name = 'PROTECT_MYHRAPP';</copy>
        
    
    ![Abfrage-dbadv](images/final-verification.png)
    
    Dadurch wird abschließend geprüft, ob die Realm `EMPLOYEESEARCH_PROD` schützt und ob `EMPLOYEESEARCH_PROD` als **Realm-Eigentümer** ohne Regelsets autorisiert ist (Null).
    
2.  Kopieren Sie als `sec_admin_owen` den folgenden Befehl, und fügen Sie ihn ein, um die Database Vault-Realm von **Simulation** in den **Durchsetzungsmodus** zu wechseln. Prüfen Sie in der Ausgabe, ob die Prozedur erfolgreich abgeschlossen wurde.
    
        	<copy>BEGIN
        		DVSYS.DBMS_MACADM.UPDATE_REALM(
        			realm_name => 'PROTECT_MYHRAPP'
        			,description => 'A mandatory realm, in simulation mode, to show how EMPLOYEESEARCH_PROD would be protected'
        			,enabled => DBMS_MACUTL.G_YES
        			,audit_options => DBMS_MACUTL.G_REALM_AUDIT_FAIL
        			,realm_type => 1); 
        	END;
        	/</copy>
        
    
    ![Durchsetzungsmodus](images/enforcement-mode.png)
    

_Hinweis_: Es kann einige Minuten dauern, bis die Aktualisierung wirksam wird.

## Aufgabe 2: Zeigen Sie, dass die Daten in `EMPLOYEESEARCH_PROD` nicht mit DBA\_DEBRA und ADMIN abgefragt werden können.

1.  Melden Sie sich von Datenbankaktionen ab, und melden Sie sich wieder als `DBA_DEBRA` an. Kopieren Sie die folgenden Befehle, und fügen Sie sie ein, um Objekte in `EMPLOYEESEARCH_PROD` abzufragen.
    
        	<copy>alter session set current_schema = EMPLOYEESEARCH_PROD;</copy>
        
    
        	<copy>select a.USERID, a.FIRSTNAME, a.LASTNAME, a.EMAIL, a.PHONEMOBILE, a.PHONEFIX, a.PHONEFAX, a.EMPTYPE, a.POSITION, a.ISMANAGER, a.MANAGERID, a.DEPARTMENT, a.CITY, a.STARTDATE, a.ENDDATE, a.ACTIVE, a.COSTCENTER, b.FIRSTNAME as MGR_FIRSTNAME, b.LASTNAME as MGR_LASTNAME, b.USERID as MGR_USERID from DEMO_HR_EMPLOYEES a left outer join DEMO_HR_EMPLOYEES b on a.MANAGERID = b.USERID where 1=1 order by a.LASTNAME, a.FIRSTNAME</copy>
        
    
    ![Debra unzureichende Berechtigungen](images/debra-insufficient.png)
    
    Beachten Sie, dass `DBA_DEBRA` jetzt nicht mehr autorisiert ist, Datenbankobjekte abzufragen, die von der Realm geschützt sind und deren Eigentümer `EMPLOYEESEARCH_PROD` ist.
    
2.  Öffnen Sie Cloud Shell. Verwenden Sie die folgenden te-Befehle, um das Skript `dv_query_employee_search.sh` als `ADMIN` auszuführen.
    
    _Hinweis: Wenn Sie aufgrund von Inaktivität von Ihrer Glassfish-Instanz abgemeldet wurden, melden Sie sich mit dem folgenden Befehl erneut an. Die öffentliche IP-Adresse finden Sie im Instanzdetail-Dashboard in Oracle Cloud:_
    
        	<copy>chmod 600 myhrappkey</copy>
        
    
        <copy>ssh -i myhrappkey opc@<PASTE INSTANCE PUBLIC IP ADDRESS HERE></copy>
        
    
    _Im Folgenden werden die Befehle zum Ausführen von dv\_query\_employee\_search.sh aufgeführt:_
    
        	<copy>cd secure-legacy-app-db-vault</copy>
        
    
        	<copy>./dv_query_employee_search.sh</copy>
        
    
    ![Keine Admin-Berechtigungen](images/admin-insufficient.png)
    
    Beachten Sie, dass `ADMIN` jetzt auch nicht mehr autorisiert ist, Datenbankobjekte abzufragen, die von der Realm geschützt sind und deren Eigentümer `EMPLOYEESEARCH_PROD` ist.
    
3.  Minimieren Sie Cloud Shell. Suchen Sie **Database Actions**, und melden Sie sich von `DBA_DEBRA` ab. Melden Sie sich wieder als `ADMIN` an, kopieren Sie die folgenden Befehle, und fügen Sie sie in die Abfrage von Objekten in `EMPLOYEESEARCH_PROD` ein.
    
        	<copy>alter session set current_schema = EMPLOYEESEARCH_PROD;</copy>
        
    
        	<copy>select a.USERID, a.FIRSTNAME, a.LASTNAME, a.EMAIL, a.PHONEMOBILE, a.PHONEFIX, a.PHONEFAX, a.EMPTYPE, a.POSITION, a.ISMANAGER, a.MANAGERID, a.DEPARTMENT, a.CITY, a.STARTDATE, a.ENDDATE, a.ACTIVE, a.COSTCENTER, b.FIRSTNAME as MGR_FIRSTNAME, b.LASTNAME as MGR_LASTNAME, b.USERID as MGR_USERID from DEMO_HR_EMPLOYEES a left outer join DEMO_HR_EMPLOYEES b on a.MANAGERID = b.USERID where 1=1 order by a.LASTNAME, a.FIRSTNAME</copy>
        
    
    ![Datenbankaktionen verwalten](images/admin-db-actions.png)
    
    `ADMIN` ist jetzt auch nicht in der Lage, Objekte abzufragen, die durch `EMPLOYEESEARCH_PROD` in **Database Actions** geschützt sind.
    

Herzlichen Glückwunsch! Sie haben jetzt erfolgreich eine Legacy-HR-Anwendung in Oracle Cloud verschoben und gesichert!

Sie können jetzt **mit der nächsten Übung fortfahren.**

## Danksagungen

*   **Autor** - Ethan Shmargad, North America Specialists Hub
*   **Ersteller** - Richard Evans, Senior Principle Product Manager
*   **Zuletzt aktualisiert am/um** - Ethan Shmargad, September 2022