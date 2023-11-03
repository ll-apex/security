# Database Vault aktivieren und HR-Anwendung prüfen

## Einführung

In dieser Übung aktivieren wir Database Vault auf unserer ATP-Instanz und starten die ATP-Instanz neu, um diese Änderung widerzuspiegeln. Dann werden wir einen weiteren Blick auf die Glassfish-Anwendung werfen und überprüfen, ob alles noch akkurat funktioniert.

### Ziele

In dieser Übung führen Sie die folgenden Aufgaben aus:

*   Aktivieren Sie Database Vault auf der ATP-Instanz **(Neustart erforderlich)**.
*   Prüfen Sie, ob die HR-Anwendung weiterhin funktioniert.

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Oracle Cloud Infrastructure-(OCI-)Mandantenaccount
*   Abschluss der folgenden vorherigen Übungen: Autonomous Database-Instanz konfigurieren, Verbindung zur Legacy-Anwendung Glassfish HR herstellen, Daten in der Glassfish-Anwendung laden und prüfen

## Aufgabe 1: Database Vault auf der ATP-Instanz aktivieren

1.  Minimieren Sie die Cloud Shell-Konsole. Navigieren Sie über das Hamburger-Menü oben rechts zu **Oracle Database>Autonomous Database**. Wählen Sie im richtigen Compartment die ATP-Instanz aus, die Sie für die Glassfish-Anwendung erstellt haben.
    
2.  Wählen Sie **Datenbankaktionen** aus.
    
    ![Datenbankaktionen öffnen](images/database-actions.png)
    
3.  Melden Sie sich bei Database Actions mit den **ADMIN**\-Zugangsdaten an, die Sie beim Provisioning der ATP-Instanz erstellt haben.
    
    ![Aktionen in Anmeldedatenbank](images/db-actions-login.png)
    
4.  Wählen Sie im Abschnitt **Entwicklung** die Option **SQL** aus.
    
    ![SQL auswählen](images/select-sql.png)
    
5.  Kopieren Sie unter dem **ADMIN**\-Schema die folgenden Befehle, und fügen Sie sie ein, um den **Database Vault-Eigentümer** zu erstellen. Klicken Sie auf die Schaltfläche **Skript ausführen**, um die Anweisungen auszuführen. Prüfen Sie die **Skriptausgabe** unten auf der Seite, um sicherzustellen, dass die Anweisungen erfolgreich ausgeführt wurden.
    
        <copy>
        CREATE USER sec_admin_owen IDENTIFIED BY WElcome_123#;
        GRANT CREATE SESSION TO sec_admin_owen;
        GRANT SELECT ANY DICTIONARY TO sec_admin_owen;
        GRANT AUDIT_ADMIN to sec_admin_owen;
        </copy>
        
    
    ![DVD-Eigentümer erstellen](images/create-dv-owner.png)
    
    ![DVD-Ausführung prüfen](images/check-dv-execution.png)
    
6.  Löschen Sie das Arbeitsblatt. Kopieren Sie unter dem **ADMIN**\-Schema die folgenden Befehle, und fügen Sie sie ein, um den **Database Vault Account Manager** zu erstellen. Klicken Sie auf die Schaltfläche **Skript ausführen**, um die Anweisungen auszuführen. Prüfen Sie die **Skriptausgabe** unten auf der Seite, um sicherzustellen, dass die Anweisungen erfolgreich ausgeführt wurden.
    
        <copy>CREATE USER accts_admin_ace IDENTIFIED BY WElcome_123#;
        GRANT CREATE SESSION TO accts_admin_ace;
        GRANT AUDIT_ADMIN to accts_admin_ace;</copy>
        
7.  Löschen Sie das Arbeitsblatt. Kopieren Sie unter dem **ADMIN**\-Schema die folgenden Befehle, und fügen Sie sie ein, um den DBA-Benutzer `dba_debra` zu erstellen. Klicken Sie auf die Schaltfläche **Skript ausführen**, um die Anweisungen auszuführen. Prüfen Sie die **Skriptausgabe** unten auf der Seite, um sicherzustellen, dass die Anweisungen erfolgreich ausgeführt wurden.
    
        <copy>
        CREATE USER dba_debra IDENTIFIED by WElcome_123#;
        GRANT PDB_DBA to dba_debra;
        BEGIN
           ORDS_ADMIN.ENABLE_SCHEMA(p_enabled => TRUE, p_schema => UPPER('dba_debra'), p_url_mapping_type => 'BASE_PATH', p_url_mapping_pattern => LOWER('dba_debra'), p_auto_rest_auth => TRUE);
        END;
        /
        </copy>
        
8.  Aktivieren Sie **SQL Worksheet**\-Berechtigungen für die gerade erstellten Benutzer. Löschen Sie das Arbeitsblatt, nachdem Sie die folgenden Befehle ausgeführt haben, und stellen Sie sicher, dass die Anweisungen ordnungsgemäß ausgeführt wurden.
    
        <copy>
        BEGIN
           ORDS_ADMIN.ENABLE_SCHEMA(p_enabled => TRUE, p_schema => UPPER('sec_admin_owen'), p_url_mapping_type => 'BASE_PATH', p_url_mapping_pattern => LOWER('sec_admin_owen'), p_auto_rest_auth => TRUE);
           ORDS_ADMIN.ENABLE_SCHEMA(p_enabled => TRUE, p_schema => UPPER('accts_admin_ace'), p_url_mapping_type => 'BASE_PATH', p_url_mapping_pattern => LOWER('accts_admin_ace'), p_auto_rest_auth => TRUE);
           ORDS_ADMIN.ENABLE_SCHEMA(p_enabled => TRUE, p_schema => UPPER('employeesearch_prod'), p_url_mapping_type => 'BASE_PATH', p_url_mapping_pattern => LOWER('employeesearch_prod'), p_auto_rest_auth => TRUE)
        END;
        /
        </copy>
        
9.  Löschen Sie das Arbeitsblatt. Jetzt führen Sie die Konfigurations- und Aktivierungsprozeduren für Oracle Database Vault aus. Mit diesen Schritten werden der Datenbank DV-bezogene Rollen hinzugefügt, sec\_admin\_owen die Rolle DV\_OWNER erteilt und accts\_admin\_ace die Rolle DV\_ACCTMGR erteilt. Weitere Informationen zu den Änderungen, die Database Vault-Konfiguration und -Aktivierung an Ihrer ADB-Instanz vornehmen, finden Sie im Abschnitt **Weitere Informationen** am Ende dieser Übung.
    
        <copy>EXEC DBMS_CLOUD_MACADM.CONFIGURE_DATABASE_VAULT('sec_admin_owen', 'accts_admin_ace');</copy>
        
    
        <copy>EXEC DBMS_CLOUD_MACADM.ENABLE_DATABASE_VAULT;</copy>
        
10.  Stellen Sie sicher, dass der Database Vault-Konfigurationsstatus **true, aber nicht aktiviert** ist, indem Sie die Tabelle `DBA_DV_STATUS` abfragen. Stellen Sie sicher, dass das Arbeitsblatt leer ist. Kopieren Sie anschließend die folgende Abfrage, und fügen Sie sie in das Arbeitsblatt ein. Führen Sie den Befehl aus, und stellen Sie sicher, dass Sie eine entsprechende Ausgabe erhalten haben.
    

    <copy>SELECT * FROM DBA_DV_STATUS;</copy>
    

11.  Navigieren Sie zurück zur Seite **Autonomous Database-Details**. Wählen Sie unter **Weitere Aktionen** die Option **Neu starten** aus (dies kann einige Minuten dauern).

![atp neu starten](images/restart-atp.png)

12.  Navigieren Sie zurück zum **SQL-Arbeitsblatt von Database Actions**, und aktualisieren Sie die Seite. Stellen Sie sicher, dass das Arbeitsblatt leer ist, und kopieren Sie die Abfrage unten, bzw. fügen Sie sie ein. Prüfen Sie die Skriptausgabe, um sicherzustellen, dass `DV_CONFIGURE_STATUS` und `DV_ENABLE_STATUS` jetzt **TRUE** sind.

    <copy>SELECT * FROM DBA_DV_STATUS;</copy>
    

## Aufgabe 2: Prüfen, ob die HR-Anwendung weiterhin funktioniert

1.  Navigieren Sie mit Ihrem Webbrowser zurück zur Glassfish-App, und aktualisieren Sie **sowohl die Produktions- als auch die Entwicklungsseiten**, um sicherzustellen, dass sie problemlos funktioniert.
    
    ![App verifizieren](images/front-page-prod.png)
    
    ![Geräte-App überprüfen](images/front-page-dev.png)
    

Sie können jetzt **mit der nächsten Übung fortfahren.**

## Weitere Informationen

*   [Oracle Database Vault-Landingpage](https://www.oracle.com/security/database-security/database-vault/)
*   [Oracle Database Vault mit Autonomous Database verwenden](https://docs.oracle.com/en/cloud/paas/autonomous-database/adbsa/autonomous-database-vault.html)

## Danksagungen

*   **Autor** - Ethan Shmargad, North America Specialists Hub
*   **Ersteller** - Richard Evans, Senior Principle Product Manager
*   **Zuletzt aktualisiert am/um** - Ethan Shmargad, September 2022