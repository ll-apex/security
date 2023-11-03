# Oracle Unified Auditing

## Einführung

In diesem Workshop werden die Funktionen von Oracle Unified Auditing vorgestellt. Es gibt dem Benutzer die Möglichkeit, zu lernen, wie dieses Feature zum Audit der Datenbankaktivität konfiguriert wird.

_Geschätzte Laborzeit:_ 35 Minuten

_In dieser Übung getestete Version:_ Oracle DB 19.19

### Videovorschau

Sehen Sie sich eine Vorschau von "_LiveLabs - Oracle Unified Auditing (Mai 2022)_" an[](youtube:bK26Y0TZANY)

### Ziele

*   Unified Auditing in der Datenbank aktivieren/deaktivieren
    
*   Verschiedene Auditing-Anwendungsfälle anzeigen
    
    **Hinweis**:
    
    *   Auditing im gemischten Modus ist das Standardauditing in einer neu installierten Datenbank. Das Auditing im gemischten Modus ermöglicht sowohl herkömmliche (d.h. die Auditfunktion aus Releases vor Release 12c) als auch neue Auditfunktionen (einheitliches Auditing).
    *   Der gemischte Modus soll ein einheitliches Auditing einführen, damit Sie ein Gefühl dafür haben können, wie es funktioniert und was seine Nuancen und Vorteile sind. Im gemischten Modus können Sie vorhandene Anwendungen und Skripte migrieren, um Unified Auditing zu verwenden. Nachdem Sie sich für das reine Unified Auditing entschieden haben, können Sie die oracle-Binärdatei mit aktivierter Unified Audit-Option neu verknüpfen und sie als einzige Auditfunktion aktivieren, die von der Oracle-Datenbank ausgeführt wird. Wenn Sie den gemischten Modus wiederherstellen möchten, können Sie dies tun.
    *   In dieser Umgebung haben wir diese Oracle Database bereits in den reinen Unified Auditing-Modus migriert.

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
| 1 | Zeigen Sie die aktuellen Auditeinstellungen an | 5 Minuten |
| 2 | Audit - Nicht-App-Nutzung | 10 Minuten |
| 3 | Verwendung der Auditdatenbankrolle | < 10 Minuten |
| 4 | Data Pump-Verwendung prüfen | < 10 Minuten |

## Aufgabe 1: Aktuelle Auditeinstellungen anzeigen

1.  Öffnen Sie eine Terminalsession auf Ihrer VM **DBSec-Lab** als BS-Benutzer _oracle_
    
        <copy>sudo su - oracle</copy>
        
    
    **Hinweis**: Wenn Sie eine Remote-Desktopsitzung verwenden, doppelklicken Sie auf das Symbol _Terminal_ auf dem Desktop, um eine Sitzung zu starten.
    
2.  Gehen Sie zum Skriptverzeichnis
    
        <copy>cd $DBSEC_LABS/unified-auditing</copy>
        
3.  Auditeinstellungen anzeigen
    
        <copy>./ua_current_audit_settings.sh</copy>
        
    *   Die Umgebung wurde bereits im reinen Unified Auditing-Modus festgelegt. Daher sollte Folgendes angezeigt werden: Unified Audit ist auf **TRUE** gesetzt
        
        ![Einheitliches Auditing](./images/ua-001.png "Auditeinstellungen anzeigen")
        
        **Hinweis**: Das bedeutet, dass sich unsere Datenbank im "reinen" Unified Auditing-Modus befindet und Sie die traditionellen Auditingfunktionen nicht mehr verwenden.
        
    *   Die 2. Abfrage zeigt an, wie viele Unified Audit Policys vorhanden sind und wie viele auditbezogene Attribute mit jeder Policy verknüpft sind
        
        ![Einheitliches Auditing](./images/ua-002.png "Auditeinstellungen anzeigen")
        
    *   Die 3. Abfrage dieses Skripts zeigt an, welche Unified Audit-Policys **aktiviert** sind
        
        ![Einheitliches Auditing](./images/ua-003.png "Auditeinstellungen anzeigen")
        
        **Hinweis**:
        
        *   Nur weil die Policy in der vorherigen Abfrage vorhanden ist, bedeutet dies nicht, dass sie aktiviert ist
        *   Die Verwendung einer Unified Audit Policy erfolgt in zwei Schritten:
            *   Policy erstellen: Auditrichtlinie erstellen <policy\_name> ...
            *   Policy aktivieren: Audit-Policy <policy\_name>
    *   Die 4. Abfrage zeigt das Auditing basierend auf dem Kontext an
        
        ![Einheitliches Auditing](./images/ua-004.png "Auditeinstellungen anzeigen")
        
        **Hinweis**:
        
        *   Es gibt eine Policy namens `TICKETINFO`, die ein Attribut namens `TICKET_ID` erfasst.
        *   Diese Informationen können in der Spalte `APPLICATION_CONTEXTS` der Ansicht `UNIFIED_AUDIT_TRAIL` angezeigt werden.
4.  Anzeigen, wer die Rollen `AUDIT_ADMIN` und `AUDIT_VIEWER` hat
    
        <copy>./ua_who_audit_roles.sh</copy>
        
    
    ![Einheitliches Auditing](./images/ua-005.png "Anzeigen, wer die Rollen AUDIT_ADMIN und AUDIT_VIEWER hat")
    
5.  Zeigen Sie die vorhandenen Auditdatensätze für die Datenbank an, bei der Sie sich anmelden. (Standardmäßig wählt das Skript **pdb1** als abzufragende Datenbank aus.)
    
        <copy>./ua_query_existing_audit_records.sh</copy>
        
    
    ![Einheitliches Auditing](./images/ua-006.png "Vorhandene Auditdatensätze anzeigen")
    
6.  Schließlich werden einige Details des Packages `DBMS_AUDIT_MGMT` angezeigt
    
        <copy>./ua_dbms_audit_mgmt_settings.sh</copy>
        
    
    ![Einheitliches Auditing](./images/ua-026.png "Zeigt einige Details des Packages DBMS_AUDIT_MGMT an")
    
    **Hinweis**:
    
    *   Die Funktion `DBMS_AUDIT_MGMT.GET_AUDIT_COMMIT_DELAY` gibt die Verzögerungszeit des Audit-Commits als Anzahl von Sekunden zurück
    *   Die Audit COMMIT-Verzögerungszeit ist die maximale Zeit, die zum Speichern eines Auditdatensatzes im Datenbankaudittrail benötigt wird
    *   Wenn das Speichern eines Auditdatensatzes länger dauert als durch die Verzögerungszeit des Audit-Commits definiert, wird eine Kopie des Auditdatensatzes in den Audittrail des Betriebssystems (BS) geschrieben

## Aufgabe 2: Nicht-App-Verwendung prüfen

In dieser Übung prüfen Sie, wer die `EMPLOYEESEARCH_PROD`\-Objekte außerhalb der Anwendung verwendet.

1.  Identifizieren Sie die Verbindungen, denen wir vertrauen. Wir werden einige Aktivitäten aus der Glassfish-Anwendung generieren und die sitzungsbezogenen Informationen anzeigen
    
        <copy>./ua_query_employeesearch_usage.sh</copy>
        
    
    ![Einheitliches Auditing](./images/ua-007.png "Identifizieren Sie die Verbindungen, denen wir vertrauen")
    
    **Hinweis**: Wenn Sie dazu aufgefordert werden, **Drücken Sie NICHT \[Zurück\]**, bevor Sie eine Studie in Glassfish wie unten beschrieben durchführen.
    
2.  Verwenden Sie Ihre Glassfish App, um Aktivitäten in Ihrer Datenbank zu generieren:
    
    *   Öffnen Sie ein Webbrowserfenster, um _`http://dbsec-lab:8080/hr_prod_pdb1`_ zu öffnen
        
        **Anmerkungen:** Wenn Sie den Remotedesktop nicht verwenden, können Sie diese Seite auch über _`http://<YOUR_DBSEC-LAB_VM_PUBLIC_IP>:8080/hr_prod_pdb1`_ aufrufen.
        
    *   Melden Sie sich bei der HR-Anwendung als _`hradmin`_ mit dem Kennwort "_`Oracle123`_" an.
        
            <copy>hradmin</copy>
            
        
            <copy>Oracle123</copy>
            
        
        ![Einheitliches Auditing](./images/ua-008.png "Anmeldung bei der HR-Anwendung")
        
        ![Einheitliches Auditing](./images/ua-009.png "Anmeldung bei der HR-Anwendung")
        
    *   Klicken Sie auf **Mitarbeiter suchen**.
        
        ![Einheitliches Auditing](./images/ua-010.png "Mitarbeiter suchen")
        
    *   Klicken Sie auf \[**Suchen**\]
        
        ![Einheitliches Auditing](./images/ua-011.png "Mitarbeiter suchen")
        
    *   Ändern Sie einige der Kriterien, und wiederholen Sie die Suche
        
    *   **Wiederholen Sie 2-3 Mal**, um sicherzustellen, dass Sie genügend Traffic haben
        
3.  Kehren Sie zur Terminalsession zurück, und **drücken Sie \[Zurück\]**, wenn die Ergebnisse angezeigt werden sollen
    
    ![Einheitliches Auditing](./images/ua-012.png "Drücken Sie [Zurück], wenn Sie bereit sind, die Ergebnisse zu sehen")
    
4.  Führen Sie dann eine Abfrage aus, um Traffic von **SQL\*Plus** auf dem Host-BS zu generieren
    
        <copy>./ua_query_employeesearch.sh</copy>
        
    
    ![Einheitliches Auditing](./images/ua-013.png "Traffic generieren")
    
5.  Erstellen Sie jetzt die **Unified Audit Policy**.
    
        <copy>./ua_create_audit_policy.sh</copy>
        
    
    ![Einheitliches Auditing](./images/ua-014.png "Unified Audit Policy erstellen")
    
    **Hinweis**:
    
    *   Die Unified Audit Policy erfasst die maschinenbezogenen Details zum Erstellen der **WHEN**\-Klausel
    *   Hier haben wir die Audit-Policy `AUDIT_EMPLOYEESEARCH_USAGE` basierend auf den `SYS_CONTEXT`\-Variablen als Kriterien erstellt:
        *   `SESSION_USER = "EMPLOYEESEARCH_PROD"`
        *   **UND** `OS_USER != "oracle"`
        *   **ODER** `MODULE != "JDBC Thin Client"`
        *   **ODER** `HOST != "dbsec-lab.dbsecvcn.oraclevcn.com"`
    *   Diese Audit-Policy auditiert alle Sessions, die versuchen, über einen unsicheren Pfad auf die Tabellen `EMPLOYEESEARCH_PROD.DEMO_HR_USERS` und `EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES` zuzugreifen (z.B. einen anderen Pfad als die offizielle Webanwendung).
6.  Nachdem Sie die Unified Audit Policy erstellt haben, müssen Sie sie aktivieren.
    
        <copy>./ua_enable_audit_policy.sh</copy>
        
    
    ![Einheitliches Auditing](./images/ua-015.png "Unified Audit Policy aktivieren")
    
7.  Führen Sie zusätzliche Abfragen aus, um Traffic zu generieren und zu prüfen, ob Auditdatensätze generiert werden
    
    *   Ausführen
        
            <copy>./ua_query_employeesearch_usage.sh</copy>
            
        
        ![Einheitliches Auditing](./images/ua-007.png "Traffic generieren")
        
        **Hinweis**: Wenn Sie dazu aufgefordert werden, **Drücken Sie NICHT \[Zurück\]**, bevor Sie eine Studie in Glassfish wie unten beschrieben durchführen.
        
    *   Kehren Sie zur Glassfish-App zurück, um neue Aktivitäten zu generieren, indem Sie auf **Mitarbeiter suchen** klicken
        
        ![Einheitliches Auditing](./images/ua-010.png "Neue Aktivität in der Glassfish-App generieren")
        
    *   Klicken Sie auf \[**Suchen**\]
        
        ![Einheitliches Auditing](./images/ua-011.png "Mitarbeiter suchen")
        
    *   Ändern Sie einige der Kriterien, und wiederholen Sie die Suche
        
    *   **Wiederholen Sie 2-3 Mal**, um sicherzustellen, dass Sie genügend Traffic haben
        
    *   Kehren Sie anschließend zur Terminalsession zurück, und drücken Sie **\[return\]**
        
        ![Einheitliches Auditing](./images/ua-016.png "Stoppen Sie die Beschwörung")
        
    *   Zeigen Sie die Ergebnisse der Auditausgabe für die Audit-Policy an `AUDIT_EMPLOYEESEARCH_USAGE`
        
            <copy>./ua_query_audit_records.sh</copy>
            
        
        ![Einheitliches Auditing](./images/ua-016b.png "Zeigen Sie die Ergebnisse der Auditausgabe an")
        
        **Hinweis**: Sie sollten nicht basierend auf dieser Unified Audit Policy generiert werden, da Anwendungsauditdaten **ausgenommen** werden.
        
8.  Fragen Sie jetzt die Tabelle `EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES` erneut ab, indem Sie einen "unsicheren" Zugriffspfad zum Generieren von Auditdaten verwenden
    
    *   Diese SQL\*Plus-Abfrage ausführen
        
            <copy>./ua_query_employeesearch.sh</copy>
            
        
        ![Einheitliches Auditing](./images/ua-013.png "Mitarbeiter suchen")
        
    *   Zeigen Sie jetzt die Ergebnisse der Auditausgabe für die Audit-Policy `AUDIT_EMPLOYEESEARCH_USAGE` an
        
            <copy>./ua_query_audit_records.sh</copy>
            
        
        ![Einheitliches Auditing](./images/ua-017.png "Zeigen Sie die Ergebnisse der Auditausgabe an")
        
        **Hinweis**:
        
        *   Sie können sehen, dass wir einen Eintrag haben, der unserer Verwendung von SQL\*Plus entspricht, ohne Abfragen aus der Glassfish-Anwendung zu erfassen
        *   Wir vertrauen der Anwendung, um Abfragen als `EMPLOYEESEARCH_PROD` auszuführen, aber wir vertrauen niemandem.
        *   Wir wollen alle anderen auditieren
9.  Wenn Sie diese Übung abgeschlossen haben, können Sie die Unified Audit-Policy entfernen
    
        <copy>./ua_delete_audit_policy.sh</copy>
        
    
    ![Einheitliches Auditing](./images/ua-027.png "Unified Audit-Policy entfernen")
    

## Aufgabe 3: Datenbankrollenverwendung prüfen

Wenn Sie eine Rolle auditieren, prüft Oracle Database alle Systemberechtigungen, die der Rolle direkt erteilt wurden. Sie können jede Rolle prüfen, einschließlich benutzerdefinierter Rollen. Wenn Sie eine gemeinsame einheitliche Audit-Policy für Rollen mit der ROLES-Auditoption erstellen, dürfen Sie nur allgemeine Rollen in der Rollenliste angeben.

Wenn eine solche Policy aktiviert ist, prüft Oracle Database alle Systemberechtigungen, die allgemein und direkt der gemeinsamen Rolle erteilt werden. Die Systemberechtigungen, die der gemeinsamen Rolle lokal erteilt werden, werden nicht geprüft. Um zu ermitteln, ob eine Rolle allgemein erteilt wurde, fragen Sie die Data Dictionary View `DBA_ROLES` ab. Um zu ermitteln, ob die Berechtigungen, die der Rolle erteilt wurden, allgemein erteilt wurden, fragen Sie die View `ROLE_SYS_PRIVS` ab.

1.  Erstellen Sie die Rolle `MGR_ROLE`, und erteilen Sie ihr die Systemberechtigung `CREATE TABLESPACE`. Anschließend erteilt er dem Datenbankbenutzer die Rolle `DBA_NICOLE`
    
        <copy>./ua_create_role.sh</copy>
        
    
    ![Einheitliches Auditing](./images/ua-018.png "Rolle MGR_ROLE erstellen")
    
2.  Erstellen Sie die Audit-Policy `AUD_ROLE_POL`, um die Verwendung der Rolle `MGR_ROLE` zu auditieren
    
        <copy>./ua_create_role_audit_policy.sh</copy>
        
    
    ![Einheitliches Auditing](./images/ua-019.png "Erstellen Sie die Audit-Policy AUD_ROLE_POL")
    
3.  Erstellen Sie als Nächstes den Benutzer `DBA_JUNIOR`, dem die Rolle `DBA` erteilt wird.
    
        <copy>./ua_create_junior_dba.sh</copy>
        
    
    ![Einheitliches Auditing](./images/ua-020.png "Erstellen Sie den Benutzer DBA_JUNIOR.")
    
4.  Erstellen Sie die Policy, die mit dem Auditing der Verwendung der Rolle `DBA` verknüpft ist
    
        <copy>./ua_create_dba_audit_policy.sh</copy>
        
    
    ![Einheitliches Auditing](./images/ua-021.png "Verknüpfte Policy erstellen")
    
5.  Aktivieren Sie die Audit-Policys für die Verwendung der Rollen `MGR_ROLE` und `DBA`
    
        <copy>./ua_enable_audit_policies.sh</copy>
        
    
    ![Einheitliches Auditing](./images/ua-022.png "Audit-Policys aktivieren")
    
6.  Audit-Policys anzeigen, die aktiviert sind
    
        <copy>./ua_view_audit_policies.sh</copy>
        
    
    ![Einheitliches Auditing](./images/ua-023.png "Prüfen Sie die Audit-Policys.")
    
7.  SQL-Anweisungen ausführen, die im Unified Audit Trail angezeigt werden
    
        <copy>./ua_generate_audits.sh</copy>
        
    
    ![Einheitliches Auditing](./images/ua-024.png "Audits generieren")
    
8.  Zeigen Sie die Ausgabe des Unified Audittrails an, die den beiden Audit-Policys zugeordnet ist
    
        <copy>./ua_review_generated_audits.sh</copy>
        
    
    ![Einheitliches Auditing](./images/ua-025.png "Zeigen Sie die Unified Audit Trail-Ausgabe an")
    
9.  Wenn Sie diese Übung abgeschlossen haben, können Sie die Unified Audit-Policys für die Rollenverwendung entfernen
    
        <copy>./ua_delete_role_audit_policy.sh</copy>
        
    
    ![Einheitliches Auditing](./images/ua-025b.png "Unified Audit-Policys für Rollennutzung entfernen")
    

## Aufgabe 4: Data Pump-Verwendung prüfen

In dieser Übung konfigurieren Sie den Unified Audit Trail und prüfen ein Audit des Oracle Data Pump-Exports. Dieses Feature von Unified Audit ist im herkömmlichen Auditing nicht verfügbar.

1.  Erstellen Sie die Unified Audit Policy "`DP_POL`", um Data Pump-Aktivitäten zu auditieren
    
        <copy>./ua_audit_datapump_export.sh</copy>
        
    
    ![Einheitliches Auditing](./images/ua-028.png "Unified Audit Policy erstellen DP_POL")
    
2.  Führen Sie zwei Data Pump-Exporte der Tabelle `EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES` aus...
    
        <copy>./ua_datapump_export_hr_table.sh</copy>
        
    
    *   ...als autorisierter Benutzer (`SYSTEM`): **Erfolgreich** ... und die Exportdatei `$DBSEC_LABS/unified-auditing/HR_table.dmp` wurde erstellt!
        
        ![Einheitliches Auditing](./images/ua-029a.png "Data Pump-Exporte")
        
    *   ...und als nicht autorisierter Benutzer (`DBSAT_ADMIN`): **Fehlerhaft!**
        
        ![Einheitliches Auditing](./images/ua-029b.png "Data Pump-Exporte")
        
    
    **Hinweis**: Nur der erfolgreiche Export ist verfügbar.
    
3.  Unified Audit Trail für die Data Pump-Aktivität prüfen
    
        <copy>./ua_review_datapump_audit_events.sh</copy>
        
    
    ![Einheitliches Auditing](./images/ua-030.png "Einheitlichen Audit Trail prüfen")
    
4.  Wenn Sie die Übung abgeschlossen haben, können Sie die Policy "Data Pump Unified Audit" entfernen.
    
        <copy>./ua_delete_dp_audit_policy.sh</copy>
        
    
    ![Einheitliches Auditing](./images/ua-031.png "Policy für Unified Audit von Data Pump entfernen")
    

Sie können jetzt mit der nächsten Übung fortfahren!

## **Anhang**: Informationen zum Produkt

### **Überblick**

Beim einheitlichen Auditing erfasst der einheitliche Audittrail Auditinformationen aus einer Vielzahl von Quellen.

Mit Unified Auditing können Sie Auditdatensätze aus den folgenden Quellen erfassen:

*   Auditdatensätze (einschließlich SYS-Auditdatensätze) aus einheitlichen AUDIT-Policys und AUDIT-Einstellungen
*   Detaillierte Auditdatensätze aus dem PL/SQL-Paket `DBMS_FGA`
*   Oracle Database Real Application Security-Auditdatensätze
*   Oracle Recovery Manager-Auditdatensätze
*   Oracle Database Vault-Auditdatensätze
*   Oracle Label Security-Auditdatensätze
*   Oracle Data Mining-Datensätze
*   Oracle Data Pump
*   Oracle SQL\*Loader-Direktladevorgänge

Der einheitliche Audittrail, der sich in einer schreibgeschützten Tabelle im AUDSYS-Schema im SYSAUX-Tablespace befindet, stellt diese Informationen in einem einheitlichen Format in der Data Dictionary View `UNIFIED_AUDIT_TRAIL` zur Verfügung und ist sowohl in Einzelinstanz- als auch in Oracle Database Real Application Clusters-Umgebungen verfügbar. Zusätzlich zum Benutzer SYS können Benutzer, denen die Rollen `AUDIT_ADMIN` und `AUDIT_VIEWER` erteilt wurden, diese Views abfragen. Wenn Ihre Benutzer nur die Ansichten abfragen, aber keine Audit-Policys erstellen müssen, erteilen Sie ihnen die Rolle `AUDIT_VIEWER`.

Wenn die Datenbank schreibgeschützt ist, werden Auditdatensätze in den einheitlichen Audittrail geschrieben. Wenn die Datenbank nicht schreibgeschützt ist, werden Auditdatensätze in Betriebssystemdateien im neuen Format im Verzeichnis `$ORACLE_BASE/audit/$ORACLE_SID` geschrieben.

### **Vorteile des einheitlichen Audit Trails**

*   Nachdem Unified Auditing aktiviert wurde, hängt es nicht mehr von den Initialisierungsparametern ab, die in früheren Releases verwendet wurden.
*   Die Auditdatensätze, einschließlich Datensätze aus dem SYS-Audittrail, für alle auditierten Komponenten der Oracle Database-Installation werden an einem Speicherort und in einem Format abgelegt, anstatt an verschiedenen Stellen nach Audittrails in verschiedenen Formaten suchen zu müssen.
*   Die Verwaltung und Sicherheit des Audittrails wird ebenfalls verbessert, indem er in einem einzigen Audittrail gespeichert wird.
*   Die allgemeine Auditing-Performance wird erheblich verbessert. Standardmäßig werden die Auditdatensätze automatisch in eine interne relationale Tabelle im AUDSYS-Schema geschrieben.
*   Sie können benannte Audit-Policys erstellen, mit denen Sie die am Anfang dieses Abschnitts aufgeführten unterstützten Komponenten sowie SYS-Benutzer mit Administratorrechten auditieren können. Darüber hinaus können Sie Bedingungen und Ausschlüsse in Ihren Policys erstellen.
*   Wenn Sie eine Oracle Audit Vault and Database Firewall-Umgebung verwenden, erleichtert der einheitliche Audittrail die Erfassung von Auditdaten erheblich, da alle diese Daten von einem Speicherort stammen.

## Sie möchten mehr erfahren?

Technische Dokumentation:

*   [Auditing - Einführung](https://docs.oracle.com/en/database/oracle/oracle-database/19/dbseg/introduction-to-auditing.html)
*   [Datenbankaktivität mit Auditing überwachen](https://docs.oracle.com/en/database/oracle/oracle-database/19/dbseg/part_6.html)

Video:

*   _Understanding Unified Auditing (Februar 2019)_[](youtube:8spLhyj3iC0)

## Danksagungen

*   **Autor** - Hakim Loumi, PM für Datenbanksicherheit
*   **Mitwirkende** - Angeline Dhanarani
*   **Zuletzt aktualisiert am/um** - Hakim Loumi, Datenbanksicherheit PM - November 2023