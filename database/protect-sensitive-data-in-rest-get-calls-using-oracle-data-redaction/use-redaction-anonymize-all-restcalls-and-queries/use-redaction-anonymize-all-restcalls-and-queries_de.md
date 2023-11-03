# Verdeckung zum Anonymisieren aller REST-Aufrufe und -Abfragen verwenden

## Einführung

In dieser Übung zeigen wir eine Anwendung von Oracle Data Redaction mit REST Get Calls, um Mitarbeitertabellendaten vor und nach Anwendung einer Verdeckungs-Policy anzuzeigen.

Geschätzte Zeit: 15 Minuten

### Ziele

In dieser Übung führen Sie die folgenden Aufgaben aus:

*   REST aktivieren Sie die Tabelle.
*   Wenden Sie die Datenverdeckungs-Policy auf die Tabelle an.
*   Siehe Daten, die vom REST-Get-Aufruf verdeckt werden.

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Einen Oracle Cloud Infrastructure-(OCI-)Mandantenaccount
*   Alle vorherigen Übungen im Workshop **Redacting Restcalls with ORDS** LiveLab abgeschlossen

_Warnung: Das Beenden von Ressourcen kann einige Minuten dauern_

## Aufgabe 1: REST für Tabelle aktivieren

1.  Navigieren Sie im Database Actions-Launchpad für `EMPLOYEESEARCH_PROD` unter **Abschnitt Entwicklung** zu **SQL**.
    
    ![SQL aus Launchpad auswählen](images/launchpad-sql.png)
    
2.  Die **REST-Aktivierung der Tabelle** ist einfach. Suchen Sie dazu die gerade erstellte Tabelle mit dem Namen `DEMO_HR_EMPLOYEES` im **Navigator** links neben dem **SQL-Arbeitsblatt**. Klicken Sie mit der rechten Maustaste auf den Tabellennamen, und wählen Sie im Popup-Menü die Option **REST** und dann "Aktivieren".
    
    ![REST aktivieren](images/enable-rest.png)
    
3.  Der **REST-Schieberegler "Objekt aktivieren"** wird rechts auf der Seite angezeigt. Wir verwenden die Standardwerte für diese Seite, aber beachten Sie, und **kopieren** Sie die Vorschau-URL in eine Zwischenablage Ihrer Wahl. Diese URL wird für den **Zugriff auf die REST-fähige Tabelle** verwendet. Dieser Prozess aktiviert auch ORDS für die Tabelle. Wählen Sie die Option "Code anzeigen", um den Code für diese Aktion anzuzeigen.
    
    ![ORDS aktivieren](images/rest-ords.png)
    

Wenn Sie bereit sind, klicken Sie unten rechts im Schieberegler auf die Schaltfläche **Aktivieren**. _Warnung: Aktivieren Sie "Authentifizierung erforderlich" nicht. Dies erfordert, dass Benutzer einen zusätzlichen Authentifizierungsprozess durchlaufen._

3.  Das ist es! Ihre Tabelle ist **REST-fähig**. Öffnen Sie ein neues Browserfenster oder eine neue Registerkarte, und geben Sie **URL** ein, die im vorherigen Schritt kopiert wurde.
    
    ![REST-Aufruf vor Redaktion](images/pre-redaction-rest.png)
    

## Aufgabe 2: Data Redaction Policy auf die Tabelle anwenden

1.  Navigieren Sie zurück zur Seite "SQL-Entwicklung" für **Database Actions** für `ADMIN`. Gewähren Sie `EMPLOYEESEARCH_PROD` Zugriff auf das Package `DBMS_REDACT`, indem Sie den folgenden Text in das Arbeitsblatt einfügen.
    
        <copy>grant execute on sys.dbms_redact to EMPLOYEESEARCH_PROD</copy>   
        
    
    ![REST-Aufruf vor Redaktion](images/grant-red.png)
    
2.  Kehren Sie zur Seite **Database Actions** für die SQL-Entwicklung für `EMPLOYEESEARCH_PROD` zurück, und führen Sie die **erste Abfrage** aus. Zeigen Sie die ungenutzten Ergebnisse unter Abfrageergebnissen **unten auf der Seite** an.
    
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
        
    
    ![Abfrage](images/qry.png)
    
    Prüfen Sie auch die Tabellendaten in unserem Browserfenster aus der vorherigen Aufgabe.
    
    So sehen unsere Daten aus, bevor eine Verdeckungsrichtlinie angewendet wird.
    
3.  Fügen Sie eine **Redaktions-Policy** hinzu, um den Nachnamen mit zufälligen Zeichen auszuführen.
    
        <copy>begin
                dbms_redact.add_policy(
                    object_schema => 'EMPLOYEESEARCH_PROD',
                    object_name   => 'demo_hr_employees',
                    column_name   => 'lastname',
                    policy_name   => 'redact_emp_info',
                    function_type => dbms_redact.random,
                    expression    => '1=1'
                );
        end;
        /</copy>   
        
    
    ![Nachname](images/last-name.png)
    
4.  Fügen Sie der Verdeckungs-Policy eine **E-Mail-Spalte** hinzu, und verdecken Sie sie mit standardmäßigen **Regex-Mustern**, die sie mit `X` anonymisieren.
    
        <copy>begin
                dbms_redact.alter_policy (
                object_schema => 'EMPLOYEESEARCH_PROD',
                object_name   => 'DEMO_HR_EMPLOYEES',
                column_name   => 'email',
                policy_name   => 'redact_emp_info',
                action              => dbms_redact.add_column,
                function_type          => DBMS_REDACT.REGEXP,
                function_parameters    => NULL,
                expression             => '1=1',
                regexp_pattern         => DBMS_REDACT.RE_PATTERN_EMAIL_ADDRESS,
                regexp_replace_string  => DBMS_REDACT.RE_REDACT_EMAIL_NAME,
                regexp_position        => DBMS_REDACT.RE_BEGINNING,
                regexp_occurrence      => DBMS_REDACT.RE_FIRST,
                regexp_match_parameter => DBMS_REDACT.RE_CASE_INSENSITIVE,
                policy_description     => 'Regular expressions to redact email address names',
                column_description     => 'email column contains customer emails');
            END;
        /</copy>   
        
    
    ![E-Mail](images/email.png)
    
5.  Fügen Sie der Verdeckungs-Policy die Spalte **Startdatum** hinzu, und verdecken Sie Tag und Monat.
    
        <copy>BEGIN
                DBMS_REDACT.alter_policy (
                object_schema       => 'EMPLOYEESEARCH_PROD',
                object_name         => 'DEMO_HR_EMPLOYEES',
                policy_name         => 'redact_emp_info',
                action              => DBMS_REDACT.add_column,
                column_name         => 'startdate',
                function_type       => DBMS_REDACT.partial,
                function_parameters => 'm1d1Y'
                );
            END;
        /</copy>   
        
    
    ![Startdatum](images/start-date.png)
    
6.  Fügen Sie der Verdeckungs-Policy die Spalte **Gehalt** hinzu, und verdecken Sie die ersten beiden Stellen, sodass sie 99 ist.
    
        <copy>BEGIN
                DBMS_REDACT.alter_policy (
                object_schema          => 'EMPLOYEESEARCH_PROD', 
                object_name            => 'DEMO_HR_EMPLOYEES', 
                column_name            => 'salary',
                column_description     => 'holds employee salaries',
                policy_name            => 'redact_emp_info', 
                policy_description     => 'Partially redacts the salary column',
                function_type          => DBMS_REDACT.PARTIAL,
                function_parameters    => '9,1,2',
                expression             => '1=1');
            END;
        /</copy>   
        
    
    ![Gehalt](images/salary.png)
    

## Aufgabe 3: Zeigen Sie an, welche Daten aus dem REST-Get-Aufruf verdeckt werden

1.  Zeigen Sie die Änderungen an der Tabelle an, indem Sie das Fenster **Browser neu laden**.
    
    ![Verdeckter REST](images/redacted-call.png)
    
2.  Führen Sie die **erste Abfrage der vorherigen Aufgabe** aus, und zeigen Sie die verdeckten Daten am **Unten der Seite** an.
    
    ![Verdeckter REST](images/redacted-qry.png)
    

Herzlichen Glückwunsch, Sie haben REST-Aufrufe mit ORDS erfolgreich verdeckt!

## Danksagungen

*   **Autoren** - Alpha Diallo & Ethan Shmargad, North America Specialists Hub
*   **Ersteller** - Pedro Lopes, Produktmanager für Datenbanksicherheit
*   **Zuletzt aktualisiert am/um** - Alpha Diallo & Ethan Shmargad, Februar 2023