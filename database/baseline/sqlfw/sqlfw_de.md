# Oracle SQL Firewall

## Einführung

In diesem Workshop werden die Funktionen von Oracle SQL Firewall vorgestellt. Es gibt dem Benutzer die Möglichkeit, zu lernen, wie man diese Funktionen zum Schutz vor Risiken konfiguriert, die auf Sicherheitslücken/Schwachstellen in datengesteuerten Webanwendungen abzielen, einschließlich SQL-Injection-Datenbankangriffen.

_Geschätzte Laborzeit:_ 30 Minuten

_In dieser Übung getestete Version:_ Oracle DB 23.2

### Videovorschau

Vorschau von "_Using SQL Firewall with Oracle Database 23c Free (Juni 2023)_" ansehen[](youtube:7yJv92FvLp4)

### Ziele

*   Erstellen einer SQL Firewall-Policy zum Schutz sensibler Daten
*   Bedrohungen durch gestohlenen Zugriff auf Zugangsdaten durch Insider erkennen
*   Reduzieren Sie das Risiko von SQL-Injection-Angriffen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten (nur _Free-Tier_ und _bezahlte Mandanten_)
    *   Übung: Umgebungssetup
    *   Übung: Umgebung initialisieren

### Laborzeit (geschätzt)

| Schritt Nr. | Merkmal | Ungefähr. Zeit |
| --- | --- | --- |
| 1 | SQL Firewall zum Schutz der Glassfish HR-Anwendung aktivieren | 10 Minuten |
| 2 | Erkennen Sie eine Insider-Bedrohung durch gestohlenen Zugangsdatenzugriff mit SQL Firewall | 10 Minuten |
| 3 | Erzwingen Sie zulässige SQL- und Zugriffsmuster mit SQL Firewall, um das Risiko von SQL-Injection-Angriffen zu minimieren | 10 Minuten |
| 4 | SQL Firewall Labs-Umgebung zurücksetzen | <5 Minuten |

## Aufgabe 1: SQL-Firewall zum Schutz der Glassfish HR-Anwendung aktivieren

In dieser Übung lernen Sie, wie der Administrator das System trainiert, um die autorisierten SQL-Anweisungen und die vertrauenswürdigen Verbindungspfade der HR-Anwendung zu erlernen. SQL Firewall Policy wird mit Ausnahmelisten generiert, die autorisierte SQL-Verbindungen und -Anweisungen darstellen, und auf dem Ziel bereitgestellt.

## Aufgabe 1a: SQL-Firewallumgebung einrichten

Hier ändern wir die standardmäßige Glassfish-Verbindung, um eine Oracle Database 23c als Ziel festzulegen, sodass wir SQL-Befehle überwachen und blockieren können

1.  Öffnen Sie eine Terminalsession auf Ihrer VM **DBSec-Lab** als BS-Benutzer _oracle_
    
        <copy>sudo su - oracle</copy>
        
    
    **Hinweis**: Wenn Sie eine Remote-Desktopsitzung verwenden, doppelklicken Sie auf das Symbol _Terminal_ auf dem Desktop, um eine Sitzung zu starten.
    
2.  Gehen Sie zum Skriptverzeichnis
    
        <copy>cd $DBSEC_LABS/sqlfw</copy>
        
3.  Verbindungszeichenfolge der Glassfish-Anwendung migrieren, um die 23c-Datenbank als Ziel festzulegen
    
        <copy>./sqlfw_glassfish_start_db23c.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-001.png "HR-App mit DB23c festlegen")
    
    **Hinweis**: Hier wird Glassfish mit der Datenbank **`FREEPDB1`** (DB 23c) auf der VM **`db23c`** verbunden.
    
4.  Prüfen Sie als Nächstes die Anwendungsfunktionen wie erwartet.
    
    *   Öffnen Sie einen Webbrowser unter der URL _`http://dbsec-lab:8080/hr_prod_pdb1`_, um auf **Ihre Glassfish-App** zuzugreifen
        
        **Anmerkungen:** Wenn Sie den Remotedesktop nicht verwenden, können Sie diese Seite auch über _`http://<YOUR_DBSEC-LAB_VM_PUBLIC_IP>:8080/hr_prod_pdb1`_ aufrufen.
        
    *   Melden Sie sich bei der Anwendung als _`hradmin`_ mit dem Kennwort "_`Oracle123`_" an.
        
            <copy>hradmin</copy>
            
        
            <copy>Oracle123</copy>
            
        
        ![SQLFW](./images/sqlfw-002.png "HR-App - Anmeldung")
        
        ![SQLFW](./images/sqlfw-003.png "HR-App - Anmeldung")
        
    *   Klicken Sie in der oberen rechten Ecke der App auf den Link **HR-Administrator willkommen**, und Sie werden zu einer Seite mit Sessiondaten gesendet
        
        ![SQLFW](./images/sqlfw-004.png "HR-Anwendung - Einstellungen")
        
    *   Im Fenster **Sessiondetails** wird angezeigt, wie die Anwendung bei der Datenbank angemeldet ist. Diese Informationen stammen aus dem Namespace **userenv**, indem die Funktion `SYS_CONTEXT` ausgeführt wird.
        
        ![SQLFW](./images/sqlfw-005.png "HR-App - Sitzungsdetails")
        
    *   Jetzt sollte **FREEPDB1** als **`DB_NAME`** und **db23c** als **HOST** angezeigt werden.
        
        ![SQLFW](./images/sqlfw-006.png "HR-App - Zieldatenbank prüfen")
        
5.  Administrator (**`dba_tom`**) zur Verwaltung von SQL Firewall erstellen
    
        <copy>./sqlfw_crea_admin-user.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-007.png "SQL-Firewall-Admin-Benutzer erstellen")
    
6.  SQL Firewall aktivieren
    
        <copy>./sqlfw_enable.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-008.png "SQL Firewall aktivieren")
    
    **Hinweis**: Daraufhin wird `ENABLED` angezeigt.
    

## Aufgabe 1b: SQL-Firewall aktivieren, um autorisierten SQL-Traffic des HR-Anwendungsbenutzers zu erlernen

1.  Starten Sie die SQL-Workload-Erfassung des Anwendungsbenutzers EMPLOYEESEARCH\_PROD
    
        <copy>./sqlfw_capture_start.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-009.png "SQL-Workload Capture des Anwendungsbenutzers starten")
    
2.  Verwenden Sie jetzt Ihre Glassfish App, um Aktivitäten in Ihrer Datenbank zu generieren:
    
    *   Gehen Sie zurück zum Webbrowserfenster zu _`http://dbsec-lab:8080/hr_prod_pdb1`_
        
        **Anmerkungen:** Wenn Sie den Remotedesktop nicht verwenden, können Sie diese Seite auch über _`http://<YOUR_DBSEC-LAB_VM_PUBLIC_IP>:8080/hr_prod_pdb1`_ aufrufen.
        
    *   Klicken Sie auf **Mitarbeiter suchen**.
        
        ![SQLFW](./images/sqlfw-010.png "Mitarbeiter suchen")
        
    *   Klicken Sie auf \[**Suchen**\]
        
        ![SQLFW](./images/sqlfw-011.png "Mitarbeiter suchen")
        
    *   Ändern Sie einige der Kriterien, und wiederholen Sie die Suche
        
    *   **Wiederholen Sie 2-3 Mal**, um sicherzustellen, dass Sie genügend Traffic haben
        
3.  Kehren Sie zur Terminalsession zurück, um sicherzustellen, dass die SQL-Anweisungen und Verbindungen der Anwendungs-Workload ordnungsgemäß erfasst werden
    
        <copy>./sqlfw_capture_check.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-012.png "Sessions prüfen und Logs erfassen")
    
    **Hinweis:** Hier prüfen wir die Session und erfassen Logs
    
4.  Wenn Sie zufrieden sind, stoppen Sie die SQL-Workload-Erfassung
    
        <copy>./sqlfw_capture_stop.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-013.png "SQL-Workload Capture stoppen")
    

## Aufgabe 1c: Zulassungslistenregeln für HR-Anwendungsbenutzer generieren und aktivieren

1.  Zulassungslistenregel generieren
    
        <copy>./sqlfw_allow_list_rule_gen.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-014.png "Ausnahmelistenregel generieren")
    
    **Hinweis:** Hier stehen 4 Anweisungen zur Verfügung.
    
2.  Vergleichen Sie diese Liste mit den erfassten Ereignissen
    
        <copy>./sqlfw_capture_count_events.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-015.png "Erfasste Ereignisse zählen")
    
    **Hinweis:** Die Anzahl entspricht der Anzahl der von uns erfassten eindeutigen Ereignisse.
    
3.  Prüfen Sie jetzt die SQL Firewall-Ausnahmelistenregeln für vertrauenswürdige Datenbankverbindungen und SQL-Anweisungen
    
        <copy>./sqlfw_allow_list_rule_exam.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-016.png "SQL Firewall-Ausnahmelistenregeln untersuchen")
    
    **Hinweis:** Hier werden nur Verbindungen von der Webanwendung (`JDBC ThinClient`) zugelassen, die vom Benutzer `oracle` auf dem Server `10.0.0.150` initiiert wurden
    
4.  Audit-Policys für SQL-Firewallverletzungen einrichten
    
        <copy>./sqlfw_setup_audit_policies.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-017.png "Audit-Policys für SQL-Firewallverletzungen einrichten")
    
5.  Aktivieren Sie die Ausnahmelistenregel für `EMPLOYEESEARCH_PROD` im **Beobachtungsmodus**
    
        <copy>./sqlfw_allow_list_rule_enable_monitor.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-018.png "Ausnahmelistenregel im Beobachtungsmodus aktivieren")
    
    **Hinweis:** Hier werden SQL-Firewallverletzungen beobachtet und nicht blockiert.
    

## Aufgabe 2: Erkennung einer Insider-Bedrohung durch gestohlenen Zugangsdatenzugriff mit SQL Firewall

Angenommen, es gibt einen böswilligen Insider, der Zugriff auf die gestohlenen Zugangsdaten des HR Apps-Benutzers `EMPLOYEESEARCH_PROD` hatte, und um die Autorisierung der HR-Anwendung zu umgehen, verwendet er SQL Developer, um Zugriff auf die sensiblen Mitarbeiterdaten zu erhalten.

1.  Lassen Sie uns zunächst prüfen, ob die normale SQL-Workload der Anwendung für die Datenbank zulässig ist
    
    *   Verwenden Sie Ihre Glassfish-App, um Aktivitäten in Ihrer Datenbank zu generieren und Ihre normalen Vorgänge auszuführen (abgeglichen, kein Verletzungslog):
        
        *   Gehen Sie zurück zum Webbrowserfenster zu _`http://dbsec-lab:8080/hr_prod_pdb1`_
            
            **Anmerkungen:** Wenn Sie den Remotedesktop nicht verwenden, können Sie diese Seite auch über _`http://<YOUR_DBSEC-LAB_VM_PUBLIC_IP>:8080/hr_prod_pdb1`_ aufrufen.
            
        *   Klicken Sie auf **Mitarbeiter suchen**.
            
            ![SQLFW](./images/sqlfw-010.png "Mitarbeiter suchen")
            
        *   Klicken Sie auf \[**Suchen**\]
            
            ![SQLFW](./images/sqlfw-011.png "Mitarbeiter suchen")
            
        *   Ändern Sie einige der Kriterien (die gleichen wie zuvor), und wiederholen Sie die Suche
            
        *   **Wiederholen Sie 2-3 Mal**, um sicherzustellen, dass Sie genügend Traffic haben
            
    *   Kehren Sie jetzt zur Terminalsession zurück, um Verletzungsprotokolle und Auditdatensätze zu prüfen
        
            <copy>./sqlfw_check_events.sh</copy>
            
        
        ![SQLFW](./images/sqlfw-019.png "Prüfen von Verletzungsprotokollen und Auditdatensätzen")
        
        **Hinweis:** Es wurden keine Datensätze gefunden, da diese Abfragen bereits als SQL-Anweisungen aufgeführt werden, die in der Datenbank zulässig sind.
        
2.  Lassen Sie uns nun eine Insider-Bedrohung des gestohlenen Zugangs zu Zugangsdaten erkennen
    
    *   Der Insider erhält mit SQL\*Plus Zugriff auf die sensiblen Mitarbeiterdaten
        
            <copy>./sqlfw_select_sensitive_data.sh</copy>
            
        
        ![SQLFW](./images/sqlfw-020.png "Vertrauliche Mitarbeiterdaten auswählen")
        
    *   Überprüfen Sie erneut die Verletzungsprotokolle und Auditdatensätze
        
            <copy>./sqlfw_check_events.sh</copy>
            
        
        ![SQLFW](./images/sqlfw-021.png "Prüfen von Verletzungsprotokollen und Auditdatensätzen")
        
        **Hinweis:** Eine SQL-Firewall-Kontextverletzung wird ausgelöst, da SQL\*Plus nicht in der zulässigen BS-Programmzulassungsliste enthalten ist und die Aufmerksamkeit von Sicherheitsadministratoren auf sich zieht
        

## Aufgabe 3: Zulässige SQL- und Zugriffsmuster mit SQL Firewall durchsetzen, um die Risiken von SQL-Injection-Angriffen zu mindern

Mit dem verdächtigen Fall eines böswilligen Insiders aktiviert der Administrator die SQL-Firewall im Blockierungsmodus, um von der UN autorisierte Versuche, auf vertrauliche Mitarbeiterinformationen zuzugreifen, zu verhindern. Erfahren Sie, wie SQL Firewall zulässige Muster durchsetzen kann, einschließlich genehmigter SQL-Anweisungen und Datenbankverbindungspfade, sowie Alerts zu potenziellen SQL-Injection-Angriffen und anormalem Zugriff auf die DB von HR-Apps.

Hier wird die SQL-Firewall bei der Erkennung nicht autorisierter SQL-Verbindungen/Anweisungen blockieren

1.  Regel-Enforcement für Ausnahmelisten in **Blockierungsmodus** aktualisieren
    
        <copy>./sqlfw_allow_list_rule_enable_block.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-022.png "Ausnahmelistenregel in Sperrmodus aktualisieren")
    
    **Hinweis:** SQL Firewall kann jetzt SQL-Injection-Versuche blockieren
    
2.  Jetzt meldet sich ein Hacker bei der Glassfish-Anwendung an, um einen SQL-Injection-Angriff auszuführen
    
    *   Gehen Sie zurück zur Glassfish App-Webseite, melden Sie sich ab und als _`hradmin`_ mit dem Kennwort "_`Oracle123`_" an.
        
    *   Klicken Sie auf **Mitarbeiter suchen**.
        
        ![SQLFW](./images/sqlfw-010.png "Mitarbeiter suchen")
        
    *   Klicken Sie auf \[**Suchen**\]
        
        ![SQLFW](./images/sqlfw-011.png "Mitarbeiter suchen")
        
        **Hinweis**: Alle Zeilen werden zurückgegeben... normal, weil Sie alles erlaubt haben.
        
    *   Aktivieren Sie jetzt das **Kontrollkästchen "Debuggen"**, um die SQL-Abfrage hinter diesem Formular anzuzeigen
        
        ![SQLFW](./images/sqlfw-023.png "SQL-Abfrage anzeigen, die hinter der Form ausgeführt wird")
        
    *   Klicken Sie erneut auf \[**Search**\].
        
        ![SQLFW](./images/sqlfw-024.png "Mitarbeiter suchen")
        
        **Hinweis:**
        
        *   Jetzt sehen Sie die offizielle SQL-Abfrage, die von dieser Form ausgeführt wird und die Ergebnisse anzeigt.
        *   Diese Abfrage enthält die Informationen zur Anzahl der angeforderten Spalten, ihren Namen, ihren Datentyp und ihre Beziehung
    *   Basierend auf diesen Informationen können Sie jetzt unsere "UNION-basierte" SQL-Injection-Abfrage erstellen, um alle sensiblen Daten anzuzeigen, die Sie direkt aus dem Formular extrahieren möchten. Hier verwenden wir diese Abfrage, um `USER_ID', 'MEMBER_ID', 'PAYMENT_ACCT_NO` und `ROUTING_NUMBER` aus der Tabelle `DEMO_HR_SUPPLEMENTAL_DATA` zu extrahieren.
        
            <copy>
            ' UNION SELECT userid, ' ID: '|| member_id, 'SQLi', '1', '1', '1', '1', '1', '1', 0, 0, payment_acct_no, routing_number, sysdate, sysdate, '0', 1, '1', '1', 1 FROM demo_hr_supplemental_data --
            </copy>
            
    *   Kopieren Sie die SQL-Injection-Abfrage, **fügen Sie sie direkt in das Feld "Position"** im Suchformular ein, und **kreuzen Sie das Kontrollkästchen "Debuggen" an**
        
        ![SQLFW](./images/sqlfw-025.png "SQL-Injection-Abfrage kopieren/einfügen")
        
        **Hinweis:**
        
        *   Vergessen Sie nicht das "`'`" vor dem UNION-Schlüsselwort, um die SQL-Klausel "LIKE" zu schließen
        *   Vergessen Sie nicht die "`--`" am Ende, um den Rest der Abfrage zu deaktivieren
    *   Klicken Sie auf \[**Suchen**\]
        
        ![SQLFW](./images/sqlfw-026.png "Mitarbeiter suchen")
        
        **Hinweis:**
        
        *   Die Ausgabe sollte bei diesen Versuchen einen ORA-Fehler zurückgeben
        *   Denken Sie daran, dass die UNION-Abfrage nicht in die Ausnahmeliste der SQL Firewall-Policy aufgenommen wurde... so einfach wie das!
3.  Prüfen Sie jetzt die Verletzungsprotokolle und Auditdatensätze
    
        <copy>./sqlfw_check_events.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-027.png "Prüfen von Verletzungsprotokollen und Auditdatensätzen")
    
    **Hinweis:** SQL-Verletzungen werden ausgelöst, um die Aufmerksamkeit von Sicherheitsadministratoren zu erregen!
    

## Aufgabe 4: SQL Firewall Labs-Umgebung zurücksetzen

1.  Sobald Sie mit dem SQL Firewall-Konzept vertraut sind, können Sie die Umgebung zurücksetzen
    
        <copy>./sqlfw_reset_env.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-050.png "SQL Firewall Labs-Umgebung zurücksetzen")
    
2.  Migrieren Sie die Verbindungszeichenfolge der Glassfish-Anwendung, um die Standarddatenbank als Ziel festzulegen (**pdb1**)
    
        <copy>./sqlfw_glassfish_stop_db23c.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-051.png "HR-App mit PDB1 festlegen")
    
    **Hinweis**: Jetzt verbinden wir Glassfish mit der Datenbank **`PDB1`** (DB 19c) auf der VM **`dbsec-lab`**.
    

Sie können jetzt mit der nächsten Übung fortfahren!

## **Anhang**: Informationen zum Produkt

### **Überblick**

SQL Firewall ist ein in den Oracle Database-Kernel integriertes Datenbanksicherheitsfeature, das alle eingehenden SQL-Anweisungen prüft und SQL-Anweisungen/Verbindungen protokollieren oder blockieren kann, die nicht in die Ausnahmelisten-Policy der SQL-Firewall fallen. SQL Firewall stellt sicher, dass nur explizit autorisiertes SQL ausgeführt wird. Es bietet erstklassigen Schutz vor häufigen Datenbankrisiken wie SQL-Injection-Angriffen und kompromittierten Konten.

SQL-Injection ist ein häufiges Datenbankangriffsmuster für datengesteuerte Webanwendungen. Angreifer können Schwachstellen und Sicherheitslücken in Webanwendungen ausnutzen und potenziell Datenbankinformationen ändern, auf sensible Daten zugreifen, Admin-Aufgaben in der Datenbank ausführen, Zugangsdaten stehlen und seitlich verschieben, um auf andere sensible Systeme zuzugreifen.

Während andere in Oracle Database eingebettete Datenbanksicherheitsfunktionen verschiedene Sicherheitskontrollen zur Überwachung/Verhütung solcher Webanwendungsangriffe bieten, ist die SQL-Firewall die einzige, die alle eingehenden SQL-Anweisungen prüft und nur autorisierte SQL zulässt. Es protokolliert und blockiert die Ausführung nicht autorisierter SQL-Abfragen in der Datenbank.

SQL Firewall wird im Oracle Database-Kernel entsprechend allen eingehenden SQL-Anweisungen unabhängig vom Ursprung ausgeführt. SQL Firewall kann SQL-Traffic zulassen, protokollieren und optional blockieren, wenn sie eine Verletzung seiner Regeln erkennt.

![SQLFW](./images/sqlfw-concept.png "SQL Firewall-Konzept")

Abbildung 1: In Oracle Database-Kernel integrierte Oracle SQL Firewall

Oracle SQL Firewall-Policys funktionieren auf Datenbankkontoebene, unabhängig davon, ob es sich um einen Application Service-Account oder einen direkten Datenbankbenutzer handelt, wie einen Berichtsbenutzer oder einen Datenbankadministrator. Die SQL-Firewall-Policy für jeden Datenbankaccount umfasst Ausnahmelisten mit autorisierten SQL-Anweisungen und zugehörigen vertrauenswürdigen Datenbankverbindungspfaden. Der Ansatz für die Ausnahmeliste bietet einen höheren Schutz vor Risiken wie SQL-Injection-Angriffen und gefährdeten Konten. Sie stellt sicher, dass nur autorisierte SQL-Anweisungen aus vertrauenswürdigen Datenbankverbindungen zur Ausführung in der Oracle-Datenbank zugelassen sind, während unbefugte Versuche, auf vertrauliche Daten zuzugreifen, die darin gespeichert sind, alarmiert/blockiert werden. Im Gegensatz zu signaturbasierten Schutzmechanismen kann SQL Firewall nicht durch Codierung der SQL-Anweisung oder Referenzierung von Synonymen oder dynamisch generierten Objektnamen getäuscht werden.

Mit PL/SQL-Prozeduren im Package `SYS.DBMS_SQL_FIREWALL` können Sie die SQL-Firewallkonfiguration in Oracle Database verwalten und verwalten. Oracle SQL Firewall ist nur für Oracle Database Enterprise Edition (Version 23c und höher) verfügbar. Oracle SQL Firewall muss zur Verwendung lizenziert sein. Es gibt zwei Wege zur Lizenz:

*   Oracle SQL Firewall ist in Oracle Database Vault enthalten. Database Vault ist eine zusätzliche Kostenoption.
*   Oracle SQL Firewall ist in Oracle Audit Vault and Database Firewall (AVDF) enthalten. AVDF ist ein separates Produkt und erfordert eine Lizenz.

### **Vorteile der Verwendung von Oracle SQL Firewall**

*   Bietet Echtzeitschutz vor häufigen Datenbankangriffen, indem der Datenbankzugriff nur auf autorisierte SQL-Anweisungen/Verbindungen beschränkt wird.
*   Reduziert Risiken durch SQL-Injection-Angriffe, anomalen Zugriff und Diebstahl/Missbrauch von Zugangsdaten.
*   Setzen Sie vertrauenswürdige Datenbankverbindungspfade durch.

## Sie möchten mehr erfahren?

Technische Dokumentation:

*   [Oracle SQL Firewall 23c](https://docs.oracle.com/en/database/oracle/oracle-database/23/dbseg/using-sql-firewall.html#GUID-F53EAE01-CE78-47F4-80AD-A0091BA3C434)

## Danksagungen

*   **Autor** - Hakim Loumi, PM für Datenbanksicherheit
*   **Mitwirkende** - Angeline Dhanarani
*   **Zuletzt aktualisiert am/um** - Hakim Loumi, Datenbanksicherheit PM - November 2023