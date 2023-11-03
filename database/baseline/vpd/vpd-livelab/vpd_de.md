# Daten mit Sicherheit auf Zeilenebene und Spaltenebene mit Oracle VPD schützen

## Einführung

Oracle Virtual Private Database erzwingt die Sicherheit auf ein feines Maß an Granularität direkt in Datenbanktabellen, Views oder Synonymen. Da Sie Sicherheits-Policys direkt an diese Datenbankobjekte anhängen und die Policys automatisch angewendet werden, wenn ein Benutzer auf Daten zugreift, können Sie die Sicherheit nicht umgehen.

Beschreibung: In dieser Übung werden die Funktionen von Oracle Virtual Private Database (VPD) vorgestellt. Sie bietet dem Benutzer die Möglichkeit, zu lernen, wie Sie dieses Feature konfigurieren, um die Sicherheit auf Zeilen- und Spaltenebene zu implementieren. Oracle VPD erstellt Sicherheits-Policys zur Kontrolle des Datenbankzugriffs auf Zeilen- und Spaltenebene.

_Geschätzte Zeit:_ 30 Minuten

_In dieser Übung getestete Version:_ Oracle DB 19.17

### Ziele

*   PL/SQL-Funktion zur Verwendung in einer VPD-Policy erstellen
*   Begrenzen Sie die zurückgegebenen Zeilen basierend auf Sessionbenutzer- und Client-ID
*   Verhindern, dass sensible Spaltendaten in SQL\*Plus-Abfragen angezeigt werden

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten (nur _Free-Tier_ und _bezahlte Mandanten_)
    *   Übung: Umgebungssetup
    *   Übung: Umgebung initialisieren

## Aufgabe 1: Laden Sie die Datei vpd.tar in das lokale Verzeichnis herunter.

1.  Öffnen Sie eine Terminalsession auf der VM **DBSec-Lab** als BS-Benutzer _oracle_, und verschieben Sie mit dem Befehl `cd` in das Verzeichnis "livelabs".
    
        <copy>cd livelabs</copy>
        
    
    **Hinweis**: Wenn Sie eine Remote-Desktopsitzung verwenden, doppelklicken Sie auf das Symbol _Terminal_ auf dem Desktop, um eine Sitzung zu starten.
    
2.  Mit dem Linux-Befehl "wget" können Sie eine gebündelte (zippte) Datei der Befehle für die Übung herunterladen.
    
        <copy>wget https://objectstorage.us-ashburn-1.oraclecloud.com/p/AC5TmVamZJxZfPG21L3cjuGER4BBS5lvMS80Du3DOwkkVtwoySpGfPIdo4Zf9knM/n/oradbclouducm/b/dbsec_rich/o/dbsec-livelabs-vpd.tar</copy>
        
3.  Entpacken Sie den heruntergeladenen tar, um das Verzeichnis und die Skripte zu erweitern.
    
        <copy>tar xvf dbsec-livelabs-vpd.tar</copy>
        
4.  Verwenden Sie den `cd`\-Befehl zum Wechseln in das vpd-Verzeichnis.
    

    ````
    <copy>cd vpd</copy>
    ````
    

5.  Verwenden Sie den `ls`\-Befehl, um Dateien aufzuführen.

    ````
    <copy>ls</copy>
    ````
    

## Aufgabe 2: Zeilenfunktion und Policys erstellen

1.  Erste Abfrage, um anzuzeigen, dass noch keine VPD-Policys erstellt wurden.
    
        <copy>./vpd_query_policies.sh</copy>
        
    
    **Ausgabe:**
    
        Show current VPD policies...
        
        no rows selected
        
2.  Erste Abfrage, die zeigt, dass `EMPLOYEESEARCH_PROD` und `DBA_DEBRA` mitarbeiterbezogene Daten anzeigen können. Beachten Sie die Anzahl der Zeilen und die sensiblen Daten, die in den Spalten zurückgegeben werden.
    
        <copy>./vpd_query_employee_data.sh</copy>
        
    
    **Ausgabe für `EMPLOYEESEARCH_PROD`:**
    
    ![Ursprüngliche Abfrage](./images/vpd_initialquery.png " ")
    
    **Ausgabe für `DBA_DEBRA`:**
    
    ![Erste Abfrage DBA_DEBRA](./images/vpd_initialquerydebra.png " ")
    
3.  VPD setzt für die Geschäftslogik auf PL/SQL-Funktionen. Erstellen Sie eine Funktion, die ein `1=0`\-Prädikat (Where-Klausel) auf die Abfrage anwendet, wenn der Sessionbenutzer nicht der Anwendungseigentümer ist, `EMPLOYEESEARCH_PROD`
    
        <copy>./vpd_create_row_function.sh</copy>
        
4.  Wenden Sie eine VPD-Policy auf die Tabelle `EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES` an, die die PL/SQL-Funktion aufruft und Zeilen für `SELECT`\-Abfragen beschränkt.
    
        <copy>./vpd_create_row_policy.sh</copy>
        
    
    **Ausgabe:**
    
    ![Zeilen-Policy erstellen](./images/vpd_createrowpolicy.png " ")
    
5.  Führen Sie die Abfrage erneut aus, um Mitarbeiterdaten anzuzeigen. Da die VPD-Zeilen-Policy gilt, werden in `EMPLOYEESEARCH_PROD` weiterhin alle Zeilen angezeigt, aber `DBA_DEBRA` kann keine Mitarbeiterdaten mehr anzeigen.
    
        <copy>./vpd_query_employee_data.sh</copy>
        
    
    **Ausgabe für `EMPLOYEESEARCH_PROD`:**
    
    ![Zeilen-Policy-Abfrage](./images/vpd_rerunquery.png " ")
    
    **Ausgabe für `DBA_DEBRA`:** ![Zeilen-Policy-Abfrage DBA_DEBRA](./images/vpd_rerunquerydebra.png " ")
    
6.  Nachdem Sie nun verstanden haben, wie Sie mit VPD die Anzahl der zurückgegebenen Zeilen begrenzen, wird die Zeilen-Policy gelöscht und die Spaltenwerte geschützt.
    
        <copy>./vpd_drop_row_policy.sh</copy>
        

## Aufgabe 3: Spaltenfunktion und Policys erstellen.

1.  Ähnlich wie die Zeilenfunktion begrenzt die PL/SQL-Funktion die Anzahl der zurückgegebenen Zeilen für Benutzer, die nicht der Eigentümer des Anwendungsschemas sind, `EMPLOYEESEARCH_PROD`. Darüber hinaus prüft die Funktion als Anwendungsbenutzer, ob der Wert `CLIENT_IDENTIFIER` im Sessionkontext des Benutzers festgelegt ist.
    
        <copy>./vpd_create_col_function.sh</copy>
        
2.  Erstellen Sie die VPD-Policy mit der PL/SQL-Spaltenfunktion. Diese Policy gilt für die Spalten `SSN`, `SIN` und `NINO`.
    
        <copy>./vpd_create_col_policy.sh</copy>
        
    
    **Ausgabe:**
    
    ![Spalten-Policy](./images/vpd_createvpdpolicy.png " ")
    
3.  Wenn `EMPLOYEESEARCH_PROD` Daten abfragt, werden 9 Zeilen zurückgegeben, die Werte für die sensiblen Spalten jedoch nicht. Dies liegt daran, dass die VPD-Policy-Funktion die Werte dieser Spalten erst zurückgibt, wenn der Sessionbenutzer und der Sessionkontext `CLIENT_IDENTIFIER` erfüllt sind.
    
        <copy>./vpd_query_employee_data.sh</copy>
        
    
    **Ausgabe für `EMPLOYEESEARCH_PROD`:** ![Spalten-Policy-Abfrage](./images/vpd_columnpolicyquery.png " ")
    
    **Ausgabe für `DBA_DEBRA`:**
    
    ![Spalten-Policy-Abfrage DBA_DEBRA](./images/vpd_columnpolicyquerydebra.png " ")
    
4.  Um die Ergebnisse zu demonstrieren, wenn sowohl der Sessionbenutzer als auch `CLIENT_IDENTIFIER` erfüllt sind, hängen Sie `hradmin` an die vorherige Abfrage an. Sensible Spaltenwerte werden angezeigt. `DBA_DEBRA` sieht diese Daten jedoch nie, weil sie nicht von der PL/SQL-Funktion autorisiert wurde.
    
        <copy>./vpd_query_employee_data.sh hradmin</copy>
        
    
    **Ausgabe für `EMPLOYEESEARCH_PROD`:**
    
    ![Client-ID-Abfrage](./images/vpd_clientidentifierquery.png " ")
    
    **Ausgabe für `DBA_DEBRA`:**
    
    ![Client-ID-Abfrage DBA_DEBRA](./images/vpd_clientidentifierquerydebra.png " ")
    
5.  Wenn Sie die Abfrage von `hradmin` in `can_candy` ändern, werden keine der sensiblen Spalten angezeigt, weil die PL/SQL-Funktion `can_candy` noch nicht als `CLIENT_IDENTIFIER` erkennt.
    
        <copy>./vpd_query_employee_data.sh can_candy</copy>
        
    
    **Ausgabe für `EMPLOYEESEARCH_PROD`:**
    
    ![Can_Candy-ID-Abfrage](./images/vpd_can_candyidentifierquery.png " ")
    
    **Ausgabe für `DBA_DEBRA`:**
    
    ![Can_Candy ID-Abfrage DBA_DEBRA](./images/vpd_can_candyidentifierquerydebra.png " ")
    
6.  Aktualisieren Sie die PL/SQL-Funktion, um eine `elsif` aufzunehmen, damit `can_candy` die sensiblen Spalten für Toronto-basierte Mitarbeiter anzeigen kann.
    
        <copy>./vpd_update_col_function.sh</copy>
        
7.  Zeigen Sie, dass `hradmin` 9 Zeilen und sensible Spalten enthält, `DBA_DEBRA` jedoch nicht.
    
        <copy>./vpd_query_employee_data.sh hradmin</copy>
        
    
    **Ausgabe für `EMPLOYEESEARCH_PROD`:**
    
    ![Hradmin-ID-Abfrage](./images/vpd_hradminidentifierquery.png " ")
    
    **Ausgabe für `DBA_DEBRA`:**
    
    ![Hradmin-ID-Abfrage DBA_DEBRA](./images/vpd_hradminidentifierquerydebra.png " ")
    
8.  Demonstrieren Sie, dass für `can_candy` 9 Zeilen und nur die sensiblen Spalten für Mitarbeiter mit Sitz in Toronto angezeigt werden. Für `DBA_DEBRA` werden weiterhin keine sensiblen Spalten angezeigt.
    
        <copy>./vpd_query_employee_data.sh can_candy</copy>
        
    
    **Ausgabe für `EMPLOYEESEARCH_PROD`:**
    
    ![Aktualisierte Can_Candy-Abfrage](./images/vpd_updatedcan_candyquery.png " ")
    
    **Ausgabe für `DBA_DEBRA`:**
    
    ![Aktualisierte Can_Candy-Abfrage DBA_DEBRA](./images/vpd_updatedcan_candyquerydebra.png " ")
    

## Aufgabe 4: Bereinigen.

1.  Entfernen Sie die PL/SQL-Funktionen, und löschen Sie die VPD-bezogenen Policys.
    
        <copy>./vpd_cleanup.sh</copy>
        
2.  Stellen Sie sicher, dass sowohl `EMPLOYESEARCH_PROD` als auch `DBA_DEBRA` vollständigen Zugriff auf Zeilen und Spalten haben, ohne dass die VPD-Policys vorhanden sind.
    
        <copy>./vpd_query_employee_data.sh</copy>
        
    
    **Ausgabe für `EMPLOYEESEARCH_PROD`:**
    
    ![Abfrage bereinigen](./images/vpd_cleanupquery.png " ")
    
    **Ausgabe für `DBA_DEBRA`:**
    
    ![Abfrage DBA_DEBRA bereinigen](./images/vpd_cleanupquerydebra.png " ")
    

## Weitere Informationen

Technische Dokumentation:

*   [Datenzugriff mit Oracle Virtual Private Database steuern](https://docs.oracle.com/en/database/oracle/oracle-database/21/dbseg/using-oracle-vpd-to-control-data-access.html#GUID-06022729-9210-4895-BF04-6177713C65A7)

## Danksagungen

*   **Autor** - Stephen Stuart & Noah Galloso, Solution Engineers, North America Specialist Hub
*   **Mitwirkende** - Richard C. Evans, Produktmanager für Datenbanksicherheit
*   **Zuletzt aktualisiert am/um** - Stephen Stuart & Noah Galloso, August 2023