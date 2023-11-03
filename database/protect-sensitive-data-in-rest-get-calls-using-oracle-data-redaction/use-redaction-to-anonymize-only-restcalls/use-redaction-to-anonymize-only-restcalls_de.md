# Nur REST-Aufrufe mit "Verdeckung" anonymisieren

## Einführung

In Übung 2 haben wir mit Oracle Data Redaction alle aus der Tabelle `DEMO_HR_EMPLOYEES` abgefragten Daten mit dem Ausdruck "1=1" verdeckt. In dieser Übung identifizieren wir das vom REST-Aufruf verwendete Modul und verdecken nur die Ergebnisse des REST-Aufrufs. Dadurch können Database Actions SQL und alle anderen Abfragen nicht wiederhergestellte Daten zurückgeben.

Geschätzte Zeit: 15 Minuten

### Ziele

In dieser Übung führen Sie die folgenden Aufgaben aus:

*   Identifizieren Sie die Sessioninformationen des REST-Aufrufs, und erstellen Sie eine Unified Audit Policy
*   Zeigen Sie die Mitarbeiterabfrage- und REST-Aufrufdaten an, bevor Sie die Verdeckungs-Policy ändern, und prüfen Sie Auditdatensätze
*   Verdeckungs-Policy aktualisieren und dann Mitarbeiterabfragedaten, REST-Aufrufdaten und Auditdatensätze prüfen
*   Audit-Policy und Verdeckungs-Policy löschen.

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Einen Oracle Cloud Infrastructure-(OCI-)Mandantenaccount
*   Alle vorherigen Übungen im Workshop **Sensible Daten in REST-GET-Aufrufen mit Oracle Data Redaction schützen** LiveLab abgeschlossen

_Warnung: Das Beenden von Ressourcen kann einige Minuten dauern_

## Aufgabe 1: Identifizieren Sie die Sessioninformationen des REST-Aufrufs, und erstellen Sie eine Unified Audit Policy.

1.  Zunächst müssen wir die Sessioninformationen des REST-Aufrufs identifizieren. Dazu werden alle Sessions auditiert, die eine SELECT-Anweisung für die `DEMO_HR_EMPLOYEES` ausführen. Dafür werden wir eine Unified Audit Policy erstellen und aktivieren. Navigieren Sie zum SQL-Fenster `ADMIN`, fügen Sie das folgende Skript ein, und führen Sie die Anweisung aus. Dieses Skript erstellt die Audit-Policy.
    
        <copy>CREATE AUDIT POLICY audit_hr_select ACTIONS SELECT on employeesearch_prod.demo_hr_employees;</copy>   
        
    
    ![Audit-Policy erstellen](images/create-audit-policy.png)
    
2.  Führen Sie das folgende Skript aus, um die Unified Audit-Policy zu aktivieren.
    
        <copy>audit policy audit_hr_select;</copy>   
        
    
    ![Audit-Policy aktivieren](images/enable-audit-policy.png)
    
3.  Jetzt möchten wir auch die Informationen zu dem Modul, das in der Abfrage verwendet wird, aufnehmen. Dadurch erhalten wir zusätzliche Informationen, um den REST-Aufruf einzugrenzen.
    
        <copy>audit context namespace userenv attributes module;</copy>   
        
    
    ![Auditkontext](images/audit-context.png)
    

## Aufgabe 2: Zeigen Sie die Abfrage- und REST-Aufrufdaten des Mitarbeiters an, bevor Sie die Verdeckungs-Policy ändern, und prüfen Sie Auditdatensätze

1.  Navigieren Sie zurück zu `EMPLOYEESEARCH_PROD`, und führen Sie die Abfrage erneut aus.
    
        <copy>SELECT
            USERID,
            FIRSTNAME,   
            LASTNAME,  --redact
            EMAIL, --redact
            PHONEMOBILE,
            STARTDATE, --redact
            SALARY, --redact
            MANAGERID,
            DEPARTMENT
        FROM
            DEMO_HR_EMPLOYEES;</copy>   
        
    
    Die Daten werden weiterhin als verdeckt angezeigt, da wir unsere Verdeckungsrichtlinie noch nicht geändert haben.
    
    ![Verdeckte Abfrage](./images/redacted-qry.png)
    
2.  Prüfen Sie als `ADMIN`, ob ein neuer Auditdatensatz für unsere neu erstellte Unified Audit Policy im Unified Audit Trail vorhanden ist.
    
        <copy>SELECT dbusername, dbproxy_username, client_program_name, sql_text, APPLICATION_CONTEXTS
            FROM unified_audit_trail
        WHERE application_contexts is not null
            AND regexp_like(unified_audit_policies,'AUDIT_HR_SELECT');</copy>  
        
    
    ![Neuer Auditdatensatz](images/new-audit-rec.png)
    
3.  Führen Sie jetzt den REST-Aufruf in Ihrem Browser aus, indem Sie die Browserregisterkarte aktualisieren. Die Daten werden verdeckt. ![Verdeckter REST-Aufruf](./images/redacted-call.png)
    
4.  Auch hier sollten Sie als `ADMIN` zusätzliche Auditdatensätze im Unified Audit Trail anzeigen. Diese neuen Auditdatensätze sollten Unterschiede in den Werten der Spalte application\_contexts aufweisen. Beispiel: Database Actions SQL zeigt den Wert "/\_/sql/" an, während Oracle Rest Data Services CALL den Wert der Spalte application\_contexts wie folgt anzeigt: "/demo\_hr\_employees/" ![Weitere Auditdatensätze](images/add-audit-rec.png)
    

## Aufgabe 3: Verdeckungs-Policy aktualisieren und Mitarbeiterabfragedaten, REST-Aufrufdaten und Auditdatensätze prüfen

1.  Als Nächstes aktualisieren wir als `EMPLOYEESEARCH_PROD` den Oracle Data Redaction Policy-Parameterausdruck von "1=1" auf das, was unser REST-Aufruf verwendet.
    
        <copy>BEGIN
        DBMS_REDACT.ALTER_POLICY  (
            OBJECT_SCHEMA => 'EMPLOYEESEARCH_PROD'
            ,object_name => 'DEMO_HR_EMPLOYEES'
            ,policy_name => 'redact_emp_info'
            ,action => DBMS_REDACT.MODIFY_EXPRESSION
            ,expression => 'sys_context(''userenv'',''module'') = ''/demo_hr_employees/''');
        END;
        /</copy> 
        
    
    ![Verdeckungs-Policy ändern](images/change-red-pol.png)
    
2.  Führen Sie die Abfrage jetzt als `EMPLOYEESEARCH_PROD` in Database Actions SQL erneut aus. Die Daten sollten nicht mehr verdeckt werden.
    
        <copy>SELECT
            USERID,
            FIRSTNAME,   
            LASTNAME,  --redact
            EMAIL, --redact
            PHONEMOBILE,
            STARTDATE, --redact
            SALARY, --redact
            MANAGERID,
            DEPARTMENT
        FROM
            DEMO_HR_EMPLOYEES;</copy>   
        
    
    ![Weitere Abfrage ausführen](images/re-run-qry.png)
    
3.  Führen Sie auch den REST-Aufruf erneut aus. Die Daten sollten weiterhin verdeckt werden. ![Weitere Abfrage ausführen](./images/redacted-call.png)
    

## Aufgabe 4: Audit-Policy und dann die Verdeckungs-Policy löschen.

1.  Da unsere Unified Audit-Policy ihren Zweck erfüllt hat, können wir sie löschen, da nicht jede einzelne SELECT-Anweisung auditiert werden muss. Führen Sie als `ADMIN` das folgende Skript aus:
    
        <copy>noaudit policy audit_hr_select;
        drop AUDIT POLICY audit_hr_select;</copy>  
        
    
    ![Audit-Policy löschen](images/drop-aud-pol.png)
    
2.  Navigieren Sie zurück zum **SQL-Fenster** für `EMPLOYEESEARCH_PROD`, und **werfen Sie die Verdeckungs-Policy**.
    
        <copy>BEGIN
                dbms_redact.drop_policy (
                object_schema => 'EMPLOYEESEARCH_PROD',
                object_name   => 'DEMO_HR_EMPLOYEES',
                policy_name   => 'redact_emp_info'
                );
            end;
        /</copy>   
        
    
    ![Löschen](images/drop.png)
    

Sie können jetzt **mit der nächsten Übung fortfahren.**

## Danksagungen

*   **Autoren** - Alpha Diallo & Ethan Shmargad, North America Specialists Hub
*   **Ersteller** - Pedro Lopes, Produktmanager für Datenbanksicherheit
*   **Zuletzt aktualisiert am/um** - Alpha Diallo & Ethan Shmargad, Februar 2023