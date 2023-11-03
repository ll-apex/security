# Oracle Database Vault in Autonomous Database

## Einführung

In diesem Workshop werden die verschiedenen Features und Funktionen von Oracle Database Vault (DV) vorgestellt. Dadurch erhält der Benutzer die Möglichkeit, zu lernen, wie er diese Features in einer Autonomous Database konfiguriert, um zu verhindern, dass nicht autorisierte privilegierte Benutzer auf sensible Daten zugreifen.

Verwaltete Datenbankservices laufen Gefahr, "Admin-Snooping" durchzuführen, sodass privilegierte Benutzer - und insbesondere kompromittierte privilegierte Benutzerkonten - auf vertrauliche Daten zugreifen können. Oracle Autonomous Database mit Database Vault bietet leistungsstarke Sicherheitskontrollen, die den Zugriff auf Anwendungsdaten durch privilegierte Datenbankbenutzer einschränken, das Risiko von Insider- und Außenstehenden-Bedrohungen reduzieren und allgemeine Complianceanforderungen erfüllen.

Sie können Kontrollen bereitstellen, um den Zugriff privilegierter Accounts auf Anwendungsdaten zu blockieren und sensible Vorgänge in der Datenbank zu kontrollieren. Mit vertrauenswürdigen Pfaden können Sie autorisierten Datenzugriffen und Datenbankänderungen zusätzliche Sicherheitskontrollen hinzufügen. IP-Adressen, Benutzernamen, Clientprogrammnamen und andere Faktoren können im Rahmen von Oracle Database Vault-Sicherheitskontrollen zur Erhöhung der Sicherheit verwendet werden. **Oracle Database Vault sichert vorhandene Datenbankumgebungen transparent und eliminiert kostspielige und zeitaufwendige Anwendungsänderungen.**

_Geschätzte Zeit:_ 35 Minuten

_In dieser Übung getestete Version:_ Oracle Autonomous Database 19c

### Videovorschau

Vorschau von "_LiveLabs - Verhindern von unautorisiertem Datenzugriff in autonomen Datenbanken mit Database Vault (Mai 2022)_" ansehen[](youtube:xKq0a_dwM1Y)

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Oracle Database Vault in einer Autonomous Database](videohub:1_hpawkio9)

### Ziele

Oracle Database Vault ist mit Ihrer autonomen Datenbank vorinstalliert. In dieser Übung führen Sie folgende Schritte aus:

*   Database Vault in Autonomous Database aktivieren
*   Sensible Daten mit einer Database Vault-Realm schützen
*   Audit-Policy zur Erfassung von Realm-Verletzungen erstellen
*   Database Vault-Kontrollen im Simulationsmodus testen

Sie verwenden das Schema `SH1`, das mehrere Tabellen wie `CUSTOMERS`\- oder `COUNTRIES`\-Tabellen enthält, die sensible Informationen enthalten und vor privilegierten Benutzern wie dem Schemaeigentümer (**Benutzer `SH1`**) und DBA (**Benutzer `DBA_DEBRA`**) geschützt werden müssen. Die Daten in diesen Tabellen sollten jedoch für den Anwendungsbenutzer (**Benutzer `APPUSER`**) verfügbar sein.

![](./images/adb-dbv_001.png " ")

**Hinweis:**

*   **In diesem Workshop gilt die Befehlssyntax zum Konfigurieren/Aktivieren/Deaktivieren von DV nur für Autonomous Database Shared.**
*   Andere Oracle Database-Deployments, einschließlich Autonomous Database Dedicated, Exadata Cloud Service, Datenbanksysteme und On-Premise-Datenbanken, verwenden eine etwas andere Syntax.

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account
*   Sie haben zuvor den Schritt "Umgebung vorbereiten" abgeschlossen

### Laborzeit (geschätzt)

| Aufgabennummer | Merkmal | Ungefähr. Zeit |
| --- | --- | --- |
| 1 | Database Vault aktivieren | <5 Minuten |
| 2 | Aufgabentrennung aktivieren (SoD) | <5 Minuten |
| 3 | Einfache Realm erstellen | 10 Minuten |
| 4 | Audit-Policy zur Erfassung von Realm-Verletzungen | 5 Minuten |
| 5 | Simulationsmodus | 10 Minuten |
| 6 | Database Vault deaktivieren | <5 Minuten |

## Aufgabe 1: Database Vault aktivieren

Zunächst erstellen wir zwei DV-Benutzerkonten:

*   **Database Vault-Eigentümer (`SEC_ADMIN_OWEN`)**
    *   Dieser Benutzer ist als Eigentümer von DV-Objekten obligatorisch
    *   `SEC_ADMIN_OWEN` hat die Rollen `DV_OWNER` und `DV_ADMIN` und kann Datenbank-Vault-Policys konfigurieren
*   **Database Vault-Accountmanager (`ACCTS_ADMIN_ACE`)**
    *   Dieser Benutzer ist eine optionale, aber empfohlene Rolle
    *   `ACCTS_ADMIN_ACE` hat die Rolle `DV_ACCTMGR` und kann Benutzer erstellen und Benutzerkennwörter ändern.
*   Während der DV-Eigentümer auch DV-Accountmanager werden kann, empfiehlt Oracle, die Aufgabentrennung mit zwei verschiedenen Accounts beizubehalten

1.  Öffnen Sie ein SQL-Arbeitsblatt in der autonomen DB als Benutzer _`ADMIN`_
    
    *   Wählen Sie in Oracle Cloud Infrastructure (OCI) die Datenbank "`ADB Security`", die im Schritt "Umgebung vorbereiten" erstellt wurde.
        
        ![](./images/adb-dbv_002.png " ")
        
    *   Klicken Sie auf der Detailseite der Datenbank "`ADB Security`" auf die Registerkarte **Extras**.
        
        ![](../prepare-setup/images/adb-set_010.png " ")
        
    *   Auf der Seite "Tools" können Sie auf Datenbankadministration und Entwicklertools für Autonomous Database zugreifen: Database Actions, Oracle Application Express, Oracle ML User Administration und SODA-Treiber. Klicken Sie im Feld "Datenbankaktionen" auf \[**Datenbankaktionen öffnen**\]
        
        ![](../prepare-setup/images/adb-set_011.png " ")
        
    *   Für Database Actions wird eine Anmeldeseite geöffnet. In dieser Übung verwenden Sie einfach den Standardadministratoraccount der Datenbankinstanz (Benutzer _`admin`_), und klicken Sie auf \[**Weiter**\]
        
        ![](../prepare-setup/images/adb-set_012.png " ")
        
    *   Geben Sie das Admin-Kennwort ein, das Sie beim Erstellen der Datenbank angegeben haben (hier: _`WElcome_123#`_)
        
            <copy>WElcome_123#</copy>
            
    *   Klicken Sie auf \[**Anmelden**\]
        
        ![](../prepare-setup/images/adb-set_013.png " ")
        
    *   Die Seite "Datenbankaktionen" wird geöffnet. Klicken Sie im Feld "Entwicklung" auf \[**SQL**\]
        
        ![](../prepare-setup/images/adb-set_014.png " ")
        
2.  Database Vault-Eigentümer- und Accountmanagerbenutzer erstellen
    
        <copy>
        -- Create DV owner
        CREATE USER sec_admin_owen IDENTIFIED BY WElcome_123#;
        GRANT CREATE SESSION TO sec_admin_owen;
        GRANT SELECT ANY DICTIONARY TO sec_admin_owen;
        GRANT AUDIT_ADMIN to sec_admin_owen;
        
        -- Create DV account manager
        CREATE USER accts_admin_ace IDENTIFIED BY WElcome_123#;
        GRANT CREATE SESSION TO accts_admin_ace;
        GRANT AUDIT_ADMIN to accts_admin_ace;
        
        -- Enable SQL Worksheet for the users just created
        BEGIN
           ORDS_ADMIN.ENABLE_SCHEMA(p_enabled => TRUE, p_schema => UPPER('sec_admin_owen'), p_url_mapping_type => 'BASE_PATH', p_url_mapping_pattern => LOWER('sec_admin_owen'), p_auto_rest_auth => TRUE);
           ORDS_ADMIN.ENABLE_SCHEMA(p_enabled => TRUE, p_schema => UPPER('accts_admin_ace'), p_url_mapping_type => 'BASE_PATH', p_url_mapping_pattern => LOWER('accts_admin_ace'), p_auto_rest_auth => TRUE);
        END;
        /
        </copy>
        
    
    **Hinweis:** - Kopieren Sie die folgenden SQL-Abfragen, bzw. fügen Sie sie in das SQL-Arbeitsblatt ein. Drücken Sie \[**F5**\], oder klicken Sie auf das Symbol "Run Scripts". Prüfen Sie, ob keine Fehler vorliegen.
    
        ![](./images/adb-dbv_003.png " ")
        
3.  Database Vault-Benutzeraccounts konfigurieren
    
        <copy>EXEC DBMS_CLOUD_MACADM.CONFIGURE_DATABASE_VAULT('sec_admin_owen', 'accts_admin_ace');</copy>
        
    
    ![](./images/adb-dbv_004.png " ")
    
4.  Prüfen Sie, ob Database Vault konfiguriert, aber noch nicht aktiviert ist
    
        <copy>SELECT * FROM DBA_DV_STATUS;</copy>
        
    
    ![](./images/adb-dbv_005.png " ")
    
    **Hinweis:** `DV_CONFIGURE_STATUS` muss **TRUE sein**
    
5.  Aktivieren Sie jetzt Database Vault
    
        <copy>EXEC DBMS_CLOUD_MACADM.ENABLE_DATABASE_VAULT;</copy>
        
    
        ![](./images/adb-dbv_006.png " ")
        
6.  Sie müssen die Datenbank neu starten, um den Database Vault-Aktivierungsprozess abzuschließen
    
    *   Starten Sie die Datenbank über die Konsole neu, indem Sie in der Dropdown-Liste "Weitere Aktionen" die Option "**Neu starten**" auswählen (siehe Abbildung).
        
        ![](./images/adb-dbv_007.png " ")
        
    *   Wenn der Neustart abgeschlossen ist, kehren Sie als Benutzer `ADMIN` zum SQL Worksheet zurück, und prüfen Sie, ob DV aktiviert ist.
        
            <copy>SELECT * FROM DBA_DV_STATUS;</copy>
            
        
        ![](./images/adb-dbv_008.png " ")
        
    
    **Hinweis:** `DV_ENABLE_STATUS` muss **TRUE sein**
    
7.  Jetzt ist Database Vault aktiviert!
    

## Aufgabe 2: Aufgabentrennung aktivieren (SoD)

In der autonomen DB verfügt der Benutzer `ADMIN` über alle Berechtigungen, einschließlich der Berechtigungen, die zum Verwalten von Database Vault-Sicherheits-Policys erforderlich sind. Im wirklichen Leben können Sie die Sicherheitsadministration, die Benutzerverwaltung und die Datenbankadministration in verschiedene Konten aufteilen. Ab jetzt verwenden wir einen DBA-Account anstelle eines `ADMIN`\-Benutzers, weil Sie in der Produktion genau das tun müssen.

Im Schritt "Prepare your environment" haben Sie den Benutzer `DBA_DEBRA` erstellt. Dieser Benutzer hat die Rolle `DBA` in der autonomen DB

1.  Um die Auswirkungen von DB Vault SoD auf einen DBA-Account zu demonstrieren, öffnen Sie das SQL Worksheet als Benutzer _`DBA_DEBRA`_ (zur Erinnerung: Das Kennwort lautet `WElcome_123#`).
    
        <copy>WElcome_123#</copy>
        
2.  `DBA_DEBRA`\-Rollen anzeigen
    
        <copy>SELECT * FROM session_roles ORDER BY 1;</copy>
        
    
        ![](./images/adb-dbv_009.png " ")
        
    
    **Hinweis:** Beachten Sie, dass DBA\_DEBRA mehrere Rollen hat, einschließlich `PDB_DBA` (die DBA-Rolle in einer autonomen DB)
    
3.  Testbenutzer `DEMO1` erstellen
    
        <copy>CREATE USER demo1;</copy>
        
    
        ![](./images/adb-dbv_010.png " ")
        
    
    **Hinweis:** Beachten Sie, dass `DBA_DEBRA` trotz der Rolle `DBA` keinen Benutzer erstellen kann, **weil Database Vault aktiviert ist**!
    
4.  Versuchen wir, den Benutzer `APPUSER` zu ändern
    
        <copy>ALTER USER appuser IDENTIFIED BY WElcome_123456#;</copy>
        
    
        ![](./images/adb-dbv_011.png " ")
        
        **Note:** `DBA_DEBRA` can no longer change a user's passwords!
        
5.  **Sobald DV aktiviert ist, beginnt es sofort mit der Durchsetzung der Aufgabentrennung** - der Benutzer `DBA_DEBRA` verliert seine Fähigkeit, DB-Benutzeraccounts zu erstellen/ändern/löschen, und diese Berechtigung wird dann mit der Rolle "DV-Accountmanager" erteilt.
    
6.  Wenn Sie mit der Übung fortfahren, verwenden Sie `SEC_ADMIN_OWEN` und `ACCTS_ADMIN_ACE` für alle Datenbank-Vault-Aktionen. Die Aufgaben der Datenbankadministration (durch `DBA_DEBRA` erfüllt) sind jetzt getrennt von den Aufgaben der Benutzerverwaltung (`ACCTS_ADMIN_ACE`) und der Sicherheitsadministration (`SEC_ADMIN_OWEN`).
    

## Aufgabe 3: Einfache Realm erstellen

Als Nächstes erstellen wir eine Realm, um die Tabelle `SH1.CUSTOMERS` vor dem Zugriff durch `DBA_DEBRA` und `SH1` (Tabellenbesitzer) zu schützen und nur Zugriff auf `APPUSER` zu erteilen.

Eine Realm ist eine geschützte Zone innerhalb der Datenbank, in der Datenbankschemas, -objekte und -rollen gesichert werden können. Beispiel: Sie können eine Reihe von Schemas, Objekten und Rollen sichern, die sich auf Buchhaltung, Vertrieb oder Personalwesen beziehen. Nachdem Sie diese in einer Realm gesichert haben, können Sie mit der Realm die Verwendung von System- und Objektberechtigungen durch bestimmte Accounts oder Rollen kontrollieren. Auf diese Weise können Sie kontextabhängige Zugriffskontrollen für alle Benutzer erzwingen, die diese Schemas, Objekte und Rollen verwenden möchten.

1.  Um die Auswirkungen dieser Realm zu demonstrieren, müssen Sie vor und nach dem Erstellen der Realm dieselbe SQL-Abfrage dieser 3 Benutzer ausführen:
    
    *   Um fortzufahren, **öffnen Sie das SQL-Arbeitsblatt in 3 Webbrowser-Seiten**, die mit einem anderen Benutzer verbunden sind (_`DBA_DEBRA`_, _`SH1`_ und _`APPUSER`_), wie in Aufgabe 1 zuvor gezeigt
        
        **Hinweis:** Achtung: Es kann nur eine SQL Worksheet-Sitzung gleichzeitig in einem Standardbrowserfenster geöffnet werden. Daher **öffnen Sie jede Ihrer Sitzungen in einem neuen Webbrowserfenster (Mozilla, Chrome, Edge, Safari usw.) oder verwenden Sie den "Incognito-Modus"!** - Zur Erinnerung: Das Passwort dieser Benutzer ist identisch (hier `WElcome_123#`)
        
               ````
               <copy>WElcome_123#</copy>
               ````
            
    *   Kopieren/Einfügen und Ausführen der folgenden Abfrage
        
            <copy>
               SELECT cust_id, cust_first_name, cust_last_name, cust_email, cust_main_phone_number
                 FROM sh1.customers
                WHERE rownum < 10;
            </copy>
            
        
        *   als Benutzer "**`DBA_DEBRA`**"
            
            ![](./images/adb-dbv_012.png " ")
            
        *   als Benutzer "**`SH1`**"
            
            ![](./images/adb-dbv_013.png " ")
            
        *   als Benutzer "**`APPUSER`**"
            
            ![](./images/adb-dbv_014.png " ")
            
        
        **Hinweis:** - **Diese 3 Benutzer können die Tabelle `SH1.CUSTOMERS` anzeigen!** - `SH1`, weil `SH1` Eigentümer der Tabelle ist - `DBA_DEBRA`, weil sie die DBA-Rolle hat - `APPUSER`, weil sie die Systemberechtigung "`READ ANY TABLE`" aufweist
        
2.  Jetzt erstellen wir eine Realm zum Sichern von `SH1`\-Tabellen, indem wir diese Abfrage unten als Benutzer _`SEC_ADMIN_OWEN`_ ausführen. Also **öffnen Sie bitte ein 4. Webbrowser-Fenster**
    
        <copy>
        -- Create the "PROTECT_SH1" DV realm
           BEGIN
              DVSYS.DBMS_MACADM.CREATE_REALM(
                  realm_name => 'PROTECT_SH1'
                  ,description => 'A mandatory realm to protect SH1 tables'
                  ,enabled => DBMS_MACUTL.G_YES
                  ,audit_options => DBMS_MACUTL.G_REALM_AUDIT_FAIL
                  ,realm_type => 1); 
           END;
           /
        
        -- Show the current DV realm
        SELECT name, description, enabled FROM dba_dv_realm WHERE id# >= 5000 ORDER BY 1;
        </copy>
        
    
        ![](./images/adb-dbv_015.png " ")
        
    
    **Hinweis:** - Jetzt ist die Realm `PROTECT_SH1` **als obligatorisch erstellt und aktiviert**! - Der Unterschied zwischen einer **obligatorischen und regulären Realm** besteht darin, dass reguläre Realms Systemberechtigungen blockieren (und direkte Objektberechtigungen zulassen), während obligatorische Realms direkte Objektberechtigungen (auch vom Objekteigentümer) zusätzlich zu Systemberechtigungen blockieren.
    
3.  Objekte zur zu schützenden Realm hinzufügen (hier die Tabelle `CUSTOMERS`)
    
        <copy>
        -- Set SH1 objects as protected by the DV realm "PROTECT_SH1"
           BEGIN
               DVSYS.DBMS_MACADM.ADD_OBJECT_TO_REALM(
                   realm_name   => 'PROTECT_SH1',
                   object_owner => 'SH1',
                   object_name  => 'CUSTOMERS',
                   object_type  => 'TABLE');
           END;
           /
        
        -- Show the objects protected by the DV realm PROTECT_SH1
        SELECT realm_name, owner, object_name, object_type
          FROM dvsys.dba_dv_realm_object
         WHERE realm_name IN (SELECT name FROM dvsys.dv$realm WHERE id# >= 5000);
        </copy>
        
    
    ![](./images/adb-dbv_016.png " ")
    
        **Note:** Now the table `CUSTOMERS` is protected and no one can access on it!
        
4.  Auswirkungen dieser Realm prüfen
    
    *   Führen Sie die folgende Abfrage im SQL Worsheet der 3 Benutzer erneut aus (_`DBA_DEBRA`_, _`SH1`_ und _`APPUSER`_)
    
        <copy>
           SELECT cust_id, cust_first_name, cust_last_name, cust_email, cust_main_phone_number
             FROM sh1.customers
            WHERE rownum < 10;
        </copy>
        
    
        - as user "**`DBA_DEBRA`**"
        
           ![](./images/adb-dbv_017.png " ")
        
        - as user "**`SH1`**"
        
           ![](./images/adb-dbv_018.png " ")
        
        - as user "**`APPUSER`**"
        
           ![](./images/adb-dbv_019.png " ")
        
        - **Objects in the realm cannot be accessed by any database users**, including the DBA (`DBA_DEBRA`) and the schema owner (`SH1`)!
        
5.  Kehren Sie jetzt als Benutzer _`SEC_ADMIN_OWEN`_ zu SQL Worksheet zurück, und stellen Sie sicher, dass Sie einen autorisierten Anwendungsbenutzer (`APPUSER`) in der Realm haben, indem Sie diese Abfrage ausführen
    
        <copy>
        -- Grant access to APPUSER only for the DV realm "PROTECT_SH1"
           BEGIN
               DVSYS.DBMS_MACADM.ADD_AUTH_TO_REALM(
                   realm_name   => 'PROTECT_SH1',
                   grantee      => 'APPUSER');
           END;
           /
        </copy>
        
    
    ![](./images/adb-dbv_020.png " ")
    
6.  Führen Sie die SQL-Abfrage erneut aus, um anzuzeigen, dass nur `APPUSER` die Daten jetzt lesen kann
    
        <copy>
           SELECT cust_id, cust_first_name, cust_last_name, cust_email, cust_main_phone_number
             FROM sh1.customers
            WHERE rownum < 10;
        </copy>
        
    
        - as user "**`DBA_DEBRA`**"
        
           ![](./images/adb-dbv_017.png " ")
        
        - as user "**`SH1`**"
        
           ![](./images/adb-dbv_018.png " ")
        
        - as user "**`APPUSER`**"
        
           ![](./images/adb-dbv_014.png " ")
        
        - **`APPUSER` must be the only user who has access to the table from now!**
        

## Aufgabe 4: Audit-Policy zur Erfassung von Realm-Verletzungen erstellen

Möglicherweise möchten Sie auch einen Audittrail von nicht autorisierten Zugriffsversuchen auf Ihre Realm-Objekte erfassen. Da Autonomous Database Unified Auditing enthält, erstellen wir eine Policy zum Audit von Datenbank-Vault-Aktivitäten

1.  Öffnen Sie ein SQL-Arbeitsblatt als Benutzer _`ACCTS_ADMIN_ACE`_ (als Erinnerung lautet das Kennwort `WElcome_123#`).
    
        <copy>WElcome_123#</copy>
        
2.  Prüfen, ob kein Audittraillog vorhanden ist
    
        <copy>
        -- Display the audit trail log
        SELECT os_username, dbusername, event_timestamp, action_name, sql_text 
          FROM unified_audit_trail
         WHERE DV_ACTION_NAME='Realm Violation Audit' order by 3;
        </copy>
        
    
    ![](./images/adb-dbv_021.png " ")
    
    **Hinweis:** Die Abfrage sollte keine Zeilen zurückgeben.
    
3.  Erstellen Sie eine Audit-Policy für die DV-Realm `PROTECT_SH1`, die zuvor in Schritt 2 erstellt wurde
    
        <copy>
        -- Create the Audit Policy
           CREATE AUDIT POLICY dv_realm_sh1
              ACTIONS SELECT, UPDATE, DELETE
              ACTIONS COMPONENT=DV Realm Violation ON "PROTECT_SH1";
        
        -- Enable the Audit Policy
           AUDIT POLICY dv_realm_sh1;
        </copy>
        
    
    ![](./images/adb-dbv_022.png " ")
    
4.  Wie in Schritt 2 sehen wir die Auswirkungen der Prüfung
    
    *   Um fortzufahren, **führen Sie dieselbe SQL-Abfrage in 3 verschiedenen SQL-Arbeitsblättern erneut aus, die in 3 Webbrowserfenstern geöffnet sind**, die mit einem anderen Benutzer verbunden sind (_`DBA_DEBRA`_, _`SH1`_ und _`APPUSER`_)
        
        **Hinweis:** Achtung: Es kann nur eine SQL Worksheet-Sitzung gleichzeitig in einem Standardbrowserfenster geöffnet werden. Daher **öffnen Sie jede Ihrer Sitzungen in einem neuen Webbrowserfenster (Mozilla, Chrome, Edge, Safari usw.) oder verwenden Sie den "Incognito-Modus"!** - Zur Erinnerung: Das Passwort dieser Benutzer ist identisch (hier `WElcome_123#`)
        
               ````
               <copy>WElcome_123#</copy>
               ````
            
    *   Kopieren/Einfügen und Ausführen der folgenden Abfrage
        
            <copy>
               SELECT cust_id, cust_first_name, cust_last_name, cust_email, cust_main_phone_number
                 FROM sh1.customers
                WHERE rownum < 10;
            </copy>
            
        
        *   als Benutzer "**`DBA_DEBRA`**"
        
        ![](./images/adb-dbv_017.png " ")
        
        *   als Benutzer "**`SH1`**"
        
        ![](./images/adb-dbv_018.png " ")
        
        *   als Benutzer "**`APPUSER`**"
        
        ![](./images/adb-dbv_014.png " ")
        
        *   `DBA_DEBRA` **und** `SH1` **Benutzer können nicht auf die** `SH1.CUSTOMERS` **Tabelle zugreifen und müssen einen Auditdatensatz über den nicht erfolgreichen Versuch generieren, die Policy zu verletzen.**
5.  Kehren Sie als Benutzer _`ACCTS_ADMIN_ACE`_ zum SQL-Arbeitsblatt zurück, um den Audittrail für Realm-Verletzungen zu prüfen
    
        <copy>
        -- Display the audit trail log
        SELECT os_username, dbusername, event_timestamp, action_name, sql_text 
          FROM unified_audit_trail
         WHERE DV_ACTION_NAME='Realm Violation Audit' order by 3;
        </copy>
        
    
    ![](./images/adb-dbv_023.png " ")
    
    **Hinweis:** Die nicht erfolgreichen Versuche mit `DBA_DEBRA` und `SH1` sollten angezeigt werden.
    
6.  Wenn Sie diese Übung abgeschlossen haben, melden Sie sich als Benutzer _`SEC_ADMIN_OWEN`_ an, um die Auditeinstellungen zurückzusetzen
    
        <copy>
        -- Show the current DV realm
        SELECT name, description, enabled FROM dba_dv_realm WHERE id# >= 5000 order by 1;
        
        -- Purge the DB Vault audit trail logs
        DELETE FROM DVSYS.AUDIT_TRAIL$;
        
        -- Purge the audit trail logs
        BEGIN
            DBMS_AUDIT_MGMT.CLEAN_AUDIT_TRAIL(
               audit_trail_type         =>  DBMS_AUDIT_MGMT.AUDIT_TRAIL_UNIFIED,
               use_last_arch_timestamp  =>  FALSE);
        END;
        /
        
        -- Display the audit trail log
        SELECT os_username, dbusername, event_timestamp, action_name, sql_text 
          FROM unified_audit_trail
         WHERE DV_ACTION_NAME='Realm Violation Audit' order by 3;
        
        -- Disable the audit policy
        NOAUDIT POLICY dv_realm_sh1;
        
        -- Drop the audit policy
        DROP AUDIT POLICY dv_realm_sh1;
        
        </copy>
        
    
    ![](./images/adb-dbv_024.png " ")
    
7.  Jetzt haben Sie keine Audit-Policy und DV-Realm mehr!
    

## Aufgabe 5: Simulationsmodus

Wir verwenden den Simulationsmodus, um die Faktoren zu finden, die für unsere "vertrauenswürdige Pfad"-Verbindung zur Tabelle `SH1.COUNTRIES` verwendet werden sollen. Wir werden dies tun, indem wir den Zugriff auf die Tabelle vollständig deaktivieren - aber dann die Realm Policy in den Simulationsmodus versetzen. Da der Simulationsmodus die eigentlichen SQL-Befehle nicht blockiert, funktionieren die SQL-Befehle. Wenn der SQL-Befehl jedoch von der neuen Befehlsregel blockiert worden sein sollte, wird ein Eintrag im Simulationsmodus erstellt. Anschließend können Sie das Simulationsprotokoll prüfen, um festzustellen, ob die korrekten Verletzungen sowie die Faktoren und zugehörigen Regeln erfasst wurden.

1.  Öffnen Sie ein SQL-Arbeitsblatt als Benutzer _`SEC_ADMIN_OWEN`_ (als Erinnerung lautet das Kennwort `WElcome_123#`).
    
        <copy>WElcome_123#</copy>
        
2.  Fragen Sie zunächst das Simulationsprotokoll ab, um zu zeigen, dass es keine aktuellen Werte enthält
    
        <copy>
           SELECT violation_type, username, machine, object_owner, object_name, command, dv$_module
           FROM dba_dv_simulation_log;
        </copy>
        
    
    ![](./images/adb-dbv_025.png " ")
    
3.  Erstellen Sie als Nächstes eine **Befehlsregel**, die das Blockieren aller `SELECT` aus der Tabelle `SH1.COUNTRIES` simuliert.
    
        <copy>
        BEGIN
            DBMS_MACADM.CREATE_COMMAND_RULE(
               command 	 => 'SELECT',
               rule_set_name   => 'Disabled',
               object_name       => 'COUNTRIES',
               object_owner       => 'SH1',
               enabled         => DBMS_MACUTL.G_SIMULATION);
        END;
        /
        </copy>
        
    
    ![](./images/adb-dbv_026.png " ")
    
4.  Wie in Schritt 2, sehen wir jetzt die Auswirkungen der Simulation
    
    *   Um fortzufahren, **führen Sie dieselbe SQL-Abfrage in 3 verschiedenen SQL-Arbeitsblättern erneut aus, die auf 3 Webbrowser-Seiten geöffnet sind**, die mit einem anderen Benutzer verbunden sind (_`DBA_DEBRA`_, _`SH1`_ und _`APPUSER`_)
        
        **Hinweis:** Achtung: Es kann nur eine SQL Worksheet-Sitzung gleichzeitig in einem Standardbrowserfenster geöffnet werden. Daher **öffnen Sie jede Ihrer Sitzungen in einem neuen Webbrowserfenster (Mozilla, Chrome, Edge, Safari usw.) oder verwenden Sie den "Incognito-Modus"!** - Zur Erinnerung: Das Passwort dieser Benutzer ist identisch (hier `WElcome_123#`)
        
               ````
               <copy>WElcome_123#</copy>
               ````
            
    *   Kopieren Sie die folgende SELECT-Abfrage, fügen Sie sie ein, und führen Sie sie mehrmals in SH1 aus. Tabelle COUNTRIES
        
            <copy>
               SELECT * FROM sh1.countries WHERE rownum < 20;
            </copy>
            
        
        *   als Benutzer "**`DBA_DEBRA`**"
        
        ![](./images/adb-dbv_027.png " ")
        
        *   als Benutzer "**`SH1`**"
        
        ![](./images/adb-dbv_028.png " ")
        
        *   als Benutzer "**`APPUSER`**"
        
        ![](./images/adb-dbv_029.png " ")
        
        *   **Alle Benutzer können auf die** `SH1.CUSTOMERS` **Tabelle zugreifen.**
5.  Kehren Sie nun als Benutzer _`SEC_ADMIN_OWEN`_ zum SQL Worksheet zurück, um zu sehen, welche neuen Einträge vorhanden sind. Denken Sie daran, dass wir eine Befehlsregel erstellt haben, um blockierende Benutzerauswahl zu simulieren!
    
        <copy>
           SELECT violation_type, username, machine, object_owner, object_name, command, dv$_module
           FROM dba_dv_simulation_log;
        </copy>
        
    
    ![](./images/adb-dbv_030.png " ")
    
    **Hinweis:**
    
    *   Obwohl jeder Benutzer die Ergebnisse sehen kann, zeigt das Log alle ausgewählten Benutzer an, die von der Regel blockiert worden wären
    *   Außerdem wird angezeigt, von wo aus sie eine Verbindung hergestellt haben und welchen Client sie verwendet haben, um eine Verbindung herzustellen.
6.  Bevor Sie zur nächsten Übung wechseln, werden die Simulationsprotokolle bereinigt und die Befehlsregel entfernt
    
        <copy>
        -- Purge simulation logs
        DELETE FROM DVSYS.SIMULATION_LOG$;
        
        -- Current simulation logs after purging
        SELECT count(*) FROM dba_dv_simulation_log;
        </copy>
        
    
    ![](./images/adb-dbv_031.png " ")
    
        <copy>
        -- Delete the Command Rule
        BEGIN
            DBMS_MACADM.DELETE_COMMAND_RULE(
               command        => 'SELECT', 
               object_owner   => 'SH1', 
               object_name    => 'COUNTRIES',
               scope          => DBMS_MACUTL.G_SCOPE_LOCAL);
        END;
        /
        </copy>
        
    
    ![](./images/adb-dbv_032.png " ")
    

## Aufgabe 6: Database Vault deaktivieren

1.  Melden Sie sich als Benutzer _`SEC_ADMIN_OWEN`_ an, und löschen Sie die vorhandene DV-Realm.
    
        <copy>
        -- Drop the "PROTECT_SH1" DV realm
        BEGIN
            DVSYS.DBMS_MACADM.DELETE_REALM_CASCADE(realm_name => 'PROTECT_SH1');
        END;
        /
        
        -- Show the current DV realm
        SELECT name, description, enabled FROM dba_dv_realm WHERE id# >= 5000 order by 1;
        
        </copy>
        
    
    ![](./images/adb-dbv_060.png " ")
    
2.  Deaktivieren Sie jetzt DB Vault in Autonomous Database
    
        <copy>EXEC DBMS_CLOUD_MACADM.DISABLE_DATABASE_VAULT;</copy>
        
    
    ![](./images/adb-dbv_061.png " ")
    
3.  Sie müssen die Datenbank neu starten, um den Database Vault-Aktivierungsprozess abzuschließen
    
    *   Starten Sie die Datenbank über die Konsole neu, indem Sie in der Dropdown-Liste "Weitere Aktionen" die Option "**Neu starten**" auswählen (siehe Abbildung).
        
        ![](./images/adb-dbv_007.png " ")
        
    *   Melden Sie sich nach Abschluss des Neustarts als Benutzer _`DBA_DEBRA`_ bei SQL Worksheet an, und prüfen Sie, ob DV aktiviert ist
        
            <copy>SELECT * FROM DBA_DV_STATUS;</copy>
            
        
        ![](./images/adb-dbv_062.png " ")
        
    
    **Hinweis:** `DV_ENABLE_STATUS` muss **FALSE sein**
    
4.  Löschen Sie jetzt den Database Vault-Eigentümer- und Accountmanagerbenutzer
    
        <copy>
        DROP USER sec_admin_owen;
        DROP USER accts_admin_ace;
        </copy>
        
    
        ![](./images/adb-dbv_063.png " ")
        
    
    **Hinweis:** Da DB Vaut deaktiviert ist, ist SoD ebenfalls automatisch deaktiviert, und Sie können Benutzer jetzt mit dem Benutzer `DBA_DEBRA` löschen.
    
5.  Jetzt ist Database Vault korrekt deaktiviert!
    

Sie können jetzt mit der nächsten Übung fortfahren!

## **Anhang**: Informationen zum Produkt

### **Überblick**

Oracle Database Vault bietet Kontrollen, um zu verhindern, dass nicht autorisierte privilegierte Benutzer auf sensible Daten zugreifen. Außerdem werden nicht autorisierte Datenbankänderungen verhindert.

Die Oracle Database Vault-Sicherheitskontrollen schützen Anwendungsdaten vor unbefugtem Zugriff und unterstützen Sie bei der Einhaltung von Datenschutz- und gesetzlichen Anforderungen.

![](./images/dv-concept.png " ")

Sie können Kontrollen bereitstellen, um den Zugriff privilegierter Accounts auf Anwendungsdaten zu blockieren und sensible Vorgänge in der Datenbank mit Database Vault zu kontrollieren.

Durch die Analyse von Berechtigungen und Rollen können Sie die Sicherheit vorhandener Anwendungen erhöhen, indem Sie Best Practices mit den geringsten Berechtigungen verwenden.

Oracle Database Vault schützt vorhandene Datenbankumgebungen transparent und eliminiert kostspielige und zeitaufwendige Anwendungsänderungen.

Mit Oracle Database Vault können Sie eine Gruppe von Komponenten erstellen, um die Sicherheit für Ihre Datenbankinstanz zu verwalten.

Diese Komponenten sind:

*   **Bereiche**

Eine Realm ist eine Schutzzone innerhalb der Datenbank, in der Datenbankschemas, -objekte und -rollen gesichert werden können. Beispiel: Sie können eine Reihe von Schemas, Objekten und Rollen sichern, die sich auf Buchhaltung, Vertrieb oder Personalwesen beziehen. Nachdem Sie diese in einer Realm gesichert haben, können Sie mit der Realm die Verwendung von System- und Objektberechtigungen für bestimmte Accounts oder Rollen kontrollieren. Auf diese Weise können Sie kontextbezogene Zugriffskontrollen für alle Benutzer bereitstellen, die diese Schemas, Objekte und Rollen verwenden möchten.

*   **Befehlsregeln**

Eine Befehlsregel ist eine spezielle Sicherheits-Policy, die Sie erstellen können, um zu steuern, unter welchen Bedingungen Benutzer fast jede SQL-Anweisung ausführen können, einschließlich SELECT-, ALTER SYSTEM-, DDL- und DML-Anweisungen (Data Manipulation Language). Befehlsregeln arbeiten mit Regelgruppen, um zu bestimmen, ob die Anweisung zulässig ist.

*   **Faktoren**

Ein Faktor ist eine benannte Variable oder ein Attribut, z.B. ein Benutzerspeicherort, eine Datenbank-IP-Adresse oder ein Sessionbenutzer, die Oracle Database Vault erkennen und verwenden kann, um Entscheidungen zur Zugriffskontrolle zu treffen. Sie verwenden Faktoren in Regeln, um Aktivitäten wie die Autorisierung von Datenbankaccounts für die Anmeldung bei der Datenbank oder die Ausführung eines bestimmten Datenbankbefehls zu steuern, um die Sichtbarkeit und Verwaltbarkeit von Daten einzuschränken. Jeder Faktor kann eine oder mehrere Identitäten aufweisen. Eine Identität ist der tatsächliche Wert eines Faktors. Ein Faktor kann abhängig von der Factor-Abrufmethode oder seiner Identity Mapping-Logik mehrere Identitäten aufweisen.

*   **Regel**

Die Regel innerhalb eines Regelsets ist ein PL/SQL-Ausdruck, dessen Auswertung true oder false ergibt. Sie können dieselbe Regel in mehreren Regelsets verwenden.

*   **Regelsets**

Ein Regelset besteht aus einer Sammlung von Regeln, die Sie mit einer Realm-Autorisierung, Befehlsregel, Faktorzuweisung oder sicheren Anwendungsrolle verknüpfen können. Das Regelset wird basierend auf der Auswertung jeder enthaltenen Regel und dem Auswertungstyp (alle True oder jede True) als wahr oder falsch ausgewertet.

*   **Sichere Anwendungsrollen**

Eine sichere Database Vault-Anwendungsrolle ist eine spezielle Oracle Database-Rolle, die basierend auf der Auswertung eines Oracle Database Vault-Regelsets aktiviert werden kann.

Oracle Database Vault stellt eine Reihe von PL/SQL-Schnittstellen und -Packages bereit, mit denen Sie diese Komponenten konfigurieren können. Im Allgemeinen besteht der erste Schritt darin, eine Realm zu erstellen, die aus den Datenbankschemas oder Datenbankobjekten besteht, die Sie sichern möchten. Sie können Ihre Datenbank weiter sichern, indem Sie Regeln, Regelsets, Befehlsregeln, Faktoren, Identitäten und sichere Anwendungsrollen erstellen. Darüber hinaus können Sie Berichte zu den Aktivitäten ausführen, die diese Komponenten überwachen und schützen.

### **Vorteile der Verwendung von Database Vault**

*   Erfüllt Compliance-Vorschriften, um den Zugriff auf Daten zu minimieren
*   Schützt Daten vor missbräuchlichen oder kompromittierten privilegierten Benutzerkonten
*   Flexible Sicherheits-Policys für Ihre Datenbank entwerfen und durchsetzen
*   Behebt Bedenken hinsichtlich der Datenbankkonsolidierung und Cloud-Umgebungen, um Kosten zu senken und das Risiko sensibler Anwendungsdaten für diejenigen zu reduzieren, die keine echte Wissensnotwendigkeit haben
*   Schützen Sie den Zugriff auf Ihre sensiblen Daten, indem Sie einen vertrauenswürdigen Pfad erstellen (weitere Informationen finden Sie in der [Übung zu Full Database Vault](https://apexapps.oracle.com/pls/apex/dbpm/r/livelabs/view-workshop?wid=682&clear=180&session=4531599220675))

## Sie möchten mehr erfahren?

Technische Dokumentation:

*   [Oracle Database Vault 19 c](https://docs.oracle.com/en/database/oracle/oracle-database/19/dvadm/introduction-to-oracle-database-vault.html#GUID-0C8AF1B2-6CE9-4408-BFB3-7B2C7F9E7284)

Video:

*   _Oracle Database Vault (März 2019)_[](youtube:oVidZw7yWIQ)
*   _Oracle Database Vault - Anwendungsfälle (Part1) (Oktober 2019)_[](youtube:aW9YQT5IRmA)
*   _Oracle Database Vault - Anwendungsfälle (Part2) (November 2019)_[](youtube:hh-cX-ubCkY)
*   _Oracle Database Vault - Einführung (Mai 2021)_[](youtube:vSVr7avZ4Hg)
*   _Oracle Database Vault auf ADB - Kurze Einführung in die Livelabs_[](youtube:O_Hi2-vZ-zU)

## Danksagungen

*   **Autor** - Hakim Loumi, PM für Datenbanksicherheit
*   **Mitwirkende** - Alan Williams, Rene Fontcha
*   **Zuletzt aktualisiert am/um** - Hakim Loumi, Datenbanksicherheit PM - Mai 2022