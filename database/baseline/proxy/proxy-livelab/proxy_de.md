# Wie Entwickler die Oracle Database-Proxyauthentifizierung verwenden können

## Einführung

In diesem Workshop wird die Funktionalität der Proxyauthentifizierung in der Oracle-Datenbank vorgestellt.

_Geschätzte Zeit:_ 30 Minuten

_In dieser Übung getestete Version:_ Oracle DB 19.17

### Informationen zum Produkt

Mit der Proxyauthentifizierung kann sich ein Benutzer (der Proxybenutzer) im Namen eines anderen Benutzers (der Zielbenutzer) bei der Datenbank anmelden. In diesem Workshop zeigen Entwickler die korrekte Verwendung, Konfiguration und Best Practices bei der Proxyauthentifizierung.

### Ziele

Entwickler können auf die Anwendungsdaten zugreifen, ohne das Kennwort des Anwendungsschemas zu kennen. Der erstellte Entwickler übernimmt die Berechtigungen des Anwendungsschemas.

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten (nur _Free-Tier_ und _bezahlte Mandanten_)
    *   Übung: Umgebungssetup
    *   Übung: Umgebung initialisieren

## Aufgabe 1: Laden Sie die Datei proxy.tar in das lokale Verzeichnis herunter.

1.  Öffnen Sie eine Terminalsession auf der VM **DBSec-Lab** als BS-Benutzer _oracle_, und verschieben Sie mit dem Befehl `cd` in das Verzeichnis "livelabs".
    
        <copy>cd livelabs</copy>
        
    
    **Hinweis**: Wenn Sie eine Remote-Desktopsitzung verwenden, doppelklicken Sie auf das Symbol _Terminal_ auf dem Desktop, um eine Sitzung zu starten.
    
2.  Mit dem Linux-Befehl "wget" können Sie eine gebündelte (zippte) Datei der Befehle für die Übung herunterladen.
    
        <copy>wget https://objectstorage.us-ashburn-1.oraclecloud.com/p/ZSKnVs6L8sGvA4HkZJjxv2sEFuf-30BYhE1F6jMHeltJ0icC-CBtLUKZzVwNObww/n/oradbclouducm/b/dbsec_rich/o/proxy.tar</copy>
        
3.  Entpacken Sie den heruntergeladenen tar, um das Verzeichnis und die Skripte zu erweitern.
    
        <copy>tar xvf proxy.tar</copy>
        
4.  Verwenden Sie den Befehl `cd`, um in das Proxyverzeichnis zu wechseln.
    

    ````
    <copy>cd proxy</copy>
    ````
    

5.  Verwenden Sie den `ls`\-Befehl, um Dateien aufzuführen.

    ````
    <copy>ls</copy>
    ````
    

## Aufgabe 2: Vertretung

1.  Erstellen Sie zunächst den Entwickleraccount `DEV_DAVE`. Beachten Sie, dass es keine Berechtigungen hat. Dieses Konto kann nur authentifiziert werden, wenn es als Proxy für ein anderes Konto fungiert.
    
        <copy>./proxy_create_dave.sh</copy>
        
    
    **Ausgabe:**
    
    ![DEV_DAVE erstellt](./images/proxy_createdev_dave.png " ")
    
2.  Autorisieren Sie den `DEV_DAVE`\-Account für den Proxy, als wäre er das Anwendungsschema `EMPLOYEESEARCH_PROD`.
    

    ````
    <copy>./proxy_auth_dave.sh</copy>
    ````
    **Output:**
    
    ![Authorize DEV_DAVE](./images/proxy_tasktwosteptwo.png " ")
    

3.  Erstellen Sie eine Unified Audit-Policy für das Audit, wenn Benutzer durch Proxy auf sensible Daten in der Tabelle `EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES` zugreifen.
    
        <copy>./proxy_create_audit_policy.sh</copy>
        
    
    **Ausgabe:**
    
    ![Unified Audit Policy erstellen](./images/proxy_createauditpolicy.png " ")
    
4.  Denken Sie daran, dass Sie nach dem Erstellen eine Unified Audit Policy aktivieren müssen. Auf diese Weise können Sie Policys im Voraus bereitstellen oder vorbereiten.
    

    ````
    <copy>./proxy_enable_audit_policy.sh</copy>
    ````
    **Output:**
    
    ![Enable Unified Audit Policy](./images/proxy_enableauditpolicy.png " ")
    

5.  Prüfen Sie, ob `DEV_DAVE` sich nicht authentifizieren kann. Dieser Account hat keine Berechtigungen. Er erbt Berechtigungen von `EMPLOYEESEARCH_PROD`, wenn er als dieser Benutzer einen Proxy verwendet.

    ````
    <copy>./proxy_test_as_dave.sh</copy>
    ````
    **Output:**
    
    ![Verify DEV_DAVE cannot authenticate](./images/proxy_proxytestasdave.png " ")
    

6.  Zeigen Sie jetzt, dass `DEV_DAVE` als Proxy `EMPLOYEESEARCH_PROD` fungieren kann. Dave muss das Kennwort `EMPLOYEESEARCH_PROD` nicht kennen, Dave verwendet sein eigenes Kennwort. Beachten Sie, dass in den Benutzerinformationen das Schema `EMPLOYEESEARCH_PROD` angezeigt wird. Wir können den Proxybenutzer prüfen, indem wir das Attribut SYS\_CONTEXT `proxy_user` abfragen.
    
        <copy>./proxy_query_employee_data.sh</copy>
        
    
    **Ausgabe:**
    
    ![DEV_DAVE Proxy](./images/proxy_dev_daveproxy.png " ")
    
    ![DEV_DAVE Proxy - Zweiter Teil](./images/proxy_dev_daveproxyparttwo.png " ")
    
7.  Zeigen Sie die Auditdaten an, die vom Proxybenutzer `DEV_DAVE` erstellt wurden.
    
        <copy>./proxy_view_audit.sh</copy>
        
    
    **Ausgabe:**
    
    ![Auditdaten anzeigen](./images/proxy_viewauditdata.png " ")
    

## Aufgabe 3: Optionale Übung

In dieser Übung zeigen Sie, wie Sie zusätzliche Sicherheitsfaktoren hinzufügen können, je nachdem, ob der Datenbankbenutzer ein Proxybenutzer ist oder nicht. Wir verhindern, dass Proxying-Benutzer wie `DEV_DAVE` die Social-ID-Spalten (`SSN`, `SIN`, `NINO`) in `EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES` anzeigen. Das Schema ohne Proxybenutzer sollte diese sensiblen Spalten anzeigen können. Erstellen Sie eine Oracle Virtual Private Database-(VPD-)Policy, die manchmal als Sicherheit auf Zeilenebene oder fein granulierte Zugriffskontrolle bezeichnet wird, um zu verhindern, dass die Social-ID-Spalten an Proxybenutzer zurückgegeben werden. Sie werden als Nullwerte zurückgegeben.

1.  Zeigen Sie, dass die sensiblen Spaltendaten für `EMPLOYEESEARCH_PROD` und `DEV_DAVE` angezeigt werden.

    ````
    <copy>./proxy_query_employee_data.sh</copy>
    ````
    **Output:**
    
    ![Sensitive Column Data](./images/proxy_sensitivecolumndata.png " ")
    
    ![Sensitive Column Data Part Two](./images/proxy_sensitivecolumndataparttwo.png " ")
    

2.  Stellen Sie sicher, dass keine aktuellen Oracle Virtual Private Database-(VPD-)Policys in der Tabelle vorhanden sind.
    
        <copy>./proxy_vpd_query_policies.sh</copy>
        
    
    **Ausgabe:**
    
    ![Keine aktuellen VPD-Policys](./images/proxy_verifyvpdpolicies.png " ")
    
3.  Erstellen Sie eine PL/SQL-Funktion, um `EMPLOYEESEARCH_PROD` session\_user zu identifizieren, und ob der Sessionbenutzer ein Proxybenutzer ist. Wenn es sich nur um das Schema handelt, fügt die Funktion der SQL-Abfrage kein Prädikat hinzu. Wenn der Sessionbenutzer ein Proxybenutzer ist, fügt er der SQL-Abfrage `1=0` hinzu und gibt die Spalten `SSN`, `SIN` und `NINO` als Nullwerte zurück.
    
        <copy>./proxy_vpd_create_function.sh</copy>
        
4.  Erstellen Sie die Policy, die für die Tabelle `EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES` angewendet werden soll.
    
        <copy>./proxy_vpd_create_policy.sh</copy>
        
    
    **Ausgabe:**
    
    ![Policy erstellen](./images/proxy_createpolicy.png " ")
    
5.  Stellen Sie sicher, dass das Schema `EMPLOYEESEARCH_PROD` die Social-ID-Spalten anzeigen kann, der Proxybenutzer jedoch nicht.
    
        <copy>./proxy_query_employee_data.sh</copy>
        
    
    **Ausgabe:**
    
    ![EMPLOYEESEARCH_PROD-Schema prüfen](./images/proxy_verifyemployeesearch_prodschema.png " ")
    
    ![EMPLOYEESEARCH_PROD-Schema für DEV_DAVE prüfen](./images/proxy_verifyemployeesearch_prodschemadave.png " ")
    
6.  Zeigen Sie die Auditdaten für unsere Proxybenutzerabfragen an. Beachten Sie, dass die Oracle VPD-Prädikatdaten (`RLS_INFO`) im Unified Audit-Trail verfügbar sind. Sie können den Policy-Typ und das Policy-Prädikat anzeigen, die auf die SQL-Abfrage des Proxybenutzers angewendet wurden.
    
        <copy>./proxy_view_audit.sh</copy>
        
    
    **Ausgabe:**
    
    ![Auditdaten anzeigen](./images/proxy_viewauditdatarelatedtoproxy.png " ")
    

## Aufgabe 4: Bereinigen.

1.  Entfernen Sie die PL/SQL-Funktionen, und bereinigen Sie den Proxy.
    
        <copy>./proxy_cleanup.sh</copy>
        
    
    **Ausgabe:**
    
    ![Löschen](./images/proxy_cleanup.png " ")
    

## Weitere Informationen

Technische Dokumentation:

*   [Proxyberechtigung](https://docs.oracle.com/en/database/oracle/oracle-database/19/jjdbc/proxy-authentication.html#GUID-07E0AF7F-2C9A-42E9-8B99-F2716DC3B746)
    
*   [Oracle Database 19c - Sicherheitshandbuch](https://docs.oracle.com/en/database/oracle/oracle-database/19/dbseg/index.html#Oracle%C2%AE-Database)
    

## Danksagungen

*   **Autor** - Stephen Stuart & Noah Galloso, Solution Engineers, North America Specialist Hub
*   **Mitwirkende** - Richard C. Evans, Produktmanager für Datenbanksicherheit
*   **Zuletzt aktualisiert am/um** - Stephen Stuart & Noah Galloso, August 2023