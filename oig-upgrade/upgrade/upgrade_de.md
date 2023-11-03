# One-Hop-Upgrade

## Einführung

In dieser Übung werden die Schritte für das One-Hop-Upgrade von Oracle Identity Manager erläutert.

_Geschätzte Laborzeit_: 50 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Schemaupgrade auf 12c ausführen
*   Die aktualisierten Schemas des OIM 11g-Setups mit dem OIM 12c-Setup neu verdrahten

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten (nur _Free-Tier_ und _bezahlte Mandanten_)
    *   Übung: Umgebungssetup
    *   Übung: Umgebung initialisieren
    *   Übung: Pre-Upgrade-Anforderungen

## Aufgabe 1: Vorhandene Schemas identifizieren, die für das Upgrade verfügbar sind

1.  Stellen Sie als _sys_\-Benutzer eine Verbindung zur Datenbank her, und führen Sie die folgende SQL-Abfrage aus, um die Versionen der vorhandenen Domains zu prüfen
    
        <copy>sqlplus / as sysdba</copy>
        
    

SQL> SET LINE 120 COLUMN MRC\_NAME FORMAT A14 COLUMN COMP\_ID FORMAT A20 COLUMN VERSION FORMAT A12 COLUMN STATUS FORMAT A9 COLUMN UPGRADED FORMAT A8 SELECT MRC\_NAME, COMP\_ID, OWNER, VERSION, STATUS, UPGRADED FROM SCHEMA\_VERSION\_REGISTRY wobei Eigentümer wie '%DEV11G%' ORDER BY MRC\_NAME, COMP\_ID; \`\`\`\`

Sie können sehen, dass Version 11g angezeigt wird.

    ![](images/1-sql.png)
    
    ```
    <copy>exit</copy>
    ```
    

## Aufgabe 2: Führen Sie den Upgradeassistenten aus, um ein Schemaupgrade auszuführen

1.  Navigieren Sie in das Verzeichnis _oracle\_common/upgrade/bin_.
    
        <copy>cd /u01/oracle/middleware12c/oracle_common/upgrade/bin/</copy>
        
2.  Parameter für den Upgradeassistenten festlegen, um die JVM-Codierungsanforderung einzuschließen
    
        <copy>export UA_PROPERTIES="-Dfile.encoding=UTF-8"</copy>
        
3.  Upgradeassistent starten
    
        <copy>./ua</copy>
        

Der Upgradeassistent wird gestartet.

4.  Upgradetyp - _Alle von einer Domain verwendeten Schemas_. Navigieren Sie zum Home der 11g-Domain - _`/u01/oracle/middleware11g/user_projects/domains/iam11g_domain/`_
    
    ![](images/2-upgrade.png)
    
5.  Komponentenliste - Klicken Sie auf _Weiter_
    
6.  Voraussetzungsprüfung - Stellen Sie sicher, dass alle Voraussetzungen erfüllt sind.
    
    ![](images/3-upgrade.png)
    
7.  OPSS-Schema
    
        DBA Username: <copy>FMW</copy>
        
    
        DBA Password: <copy>Welcom#123</copy>
        
    
    ![](images/3a-upgrade.png)
    
8.  MDS-Schema - Derselbe Benutzername und dasselbe Kennwort werden automatisch aktualisiert. Klicken Sie auf _Weiter_
    
9.  UMS-Schema - Derselbe Benutzername und dasselbe Kennwort werden automatisch aktualisiert. Klicken Sie auf _Weiter_
    
10.  SOAINFRA-Schema - Derselbe Benutzername und dasselbe Kennwort werden automatisch aktualisiert. Klicken Sie auf _Weiter_
    
11.  OIM-Schema: Derselbe Benutzername und dasselbe Kennwort werden automatisch aktualisiert. Klicken Sie auf _Weiter_
    
12.  Schemas erstellen - Stellen Sie sicher, dass _Fehlende Schemas für angegebene Domain erstellen_ aktiviert ist. Klicken Sie auf _Gleiches Kennwort für alle Schemas verwenden_, und geben Sie das Schemakennwort als _Welcom#123_ ein
    
        Schema Password: <copy>Welcom#123</copy>
        
    
    ![](images/4-upgrade.png)
    
13.  Prüfen - Klicken Sie auf _Next_ (Weiter).
    
    ![](images/5-upgrade.png)
    
14.  Schemafortschritt erstellen: Nachdem Sie das Schema erstellt haben, klicken Sie auf _Upgrade_.
    
    ![](images/6-upgrade.png)
    
15.  Schließen Sie den Upgradeassistenten, nachdem das Upgrade erfolgreich abgeschlossen wurde.
    
    ![](images/7-upgrade.png)
    

## Aufgabe 3: Schemaupgrade prüfen

1.  Stellen Sie als _sys_\-Benutzer eine Verbindung zur Datenbank her, und führen Sie die folgende SQL-Abfrage aus, um die Version zu prüfen
    
        <copy>sqlplus / as sysdba</copy>
        
    

SQL> SET LINE 120 COLUMN MRC\_NAME FORMAT A14 COLUMN COMP\_ID FORMAT A20 COLUMN VERSION FORMAT A12 COLUMN STATUS FORMAT A9 COLUMN UPGRADED FORMAT A8 SELECT MRC\_NAME, COMP\_ID, OWNER, VERSION, STATUS, UPGRADED FROM SCHEMA\_VERSION\_REGISTRY wobei OWNER wie '%DEV11G%' ORDER BY MRC\_NAME, COMP\_ID; \`\` überprüft, ob das Schemaupgrade erfolgreich war, indem geprüft wird, ob die Schemaversion ordnungsgemäß auf 12c aktualisiert wurde.

    ![](images/8-sql.png)
    

## Aufgabe 4: Temporären Ordner bereinigen

1.  Da das Verzeichnis _/tmp_ für die JVM-Eigenschaft _java.io.tmpdir_ festgelegt ist, können alle unerwünschten Dateien im Ordner _/tmp_ den OIG-Upgradeprozess beeinträchtigen und zu einer MDS-Beschädigung führen. Löschen Sie daher den Ordner _/tmp_, bevor Sie den Upgradeprozess starten.
    
        <copy>rm -rf /tmp/*</copy>
        

## Aufgabe 5: 12c Managed Server stoppen

1.  Stoppen Sie die SOA, OIM Managed Server, bevor Sie die Domain neu verknüpfen. Stellen Sie sicher, dass der Admin-Server und die Datenbank hochgefahren und gestartet sind. Stoppen Sie zunächst den OIM-Server
    
        <copy>cd /u01/oracle/middleware12c/user_projects/domains/iam12c_domain/bin</copy>
        
    
        <copy>./stopManagedWebLogic.sh oim_server1</copy>
        
2.  Stoppen Sie nach dem Herunterfahren des OIM-Servers den SOA-Server.
    
        <copy>./stopManagedWebLogic.sh soa_server1</copy>
        
    
    Prüfen Sie in der Weblogic 12c-Konsole, ob alle Managed Server den Status "SHUTDOWN" aufweisen.
    
    ![](images/9-server12c.png)
    

## Aufgabe 6: Neuverkabelung der Domäne

1.  Prüfen Sie die Werte für die verschiedenen Eigenschaften in der Datei _oneHop.properties_
    
        <copy>vi /u01/Upgrade_Utils/oneHop.properties</copy>
        
2.  Kopieren Sie die Datei _oneHop.properties_ in das Verzeichnis _<12c\_ORACLE\_HOME>/idm/server/upgrade/oneHopUpgrade_.
    
        <copy>cp /u01/Upgrade_Utils/oneHop.properties /u01/oracle/middleware12c/idm/server/upgrade/oneHopUpgrade/</copy>
        
        
3.  Navigieren Sie zum Verzeichnis _<12c\_ORACLE\_HOME>/idm/server/upgrade/oneHopUpgrade_, um das Skript _oneHopUpgrade.sh_ aufzurufen.
    
        <copy>cd /u01/oracle/middleware12c/idm/server/upgrade/oneHopUpgrade/</copy>
        
    
        <copy>sh oneHopUpgrade.sh -p Welcom@123</copy>
        

Geben Sie zur Laufzeit die Kennwörter wie folgt an:

*   Admin-Server: **Welcom@123**
*   DEV11G\_OIM: **Welcom#123**
*   DEV11G\_SOAINFRA: **Welcom#123**
*   DEV11G\_STB: **Welcom#123**
*   DEV11G\_ORASDPM: **Welcom#123**
*   DEV11G\_WLS: **Welcom#123**
*   DEV11G\_MDS: **Welcom#123**
*   DEV11G\_IAU\_APPEND: **Welcom#123**
*   DEV11G\_IAU\_VIEWER: **Welcom#123**
*   DEV11G\_OPSS: **Welcom#123**
*   Kennwort, das zum Exportieren des OPSS-Verschlüsselungsschlüssels aus 11g verwendet wurde: **Welcom@123**
*   ".xldatabasekey"-Keystore-Kennwort von OIG12csp4 Setup: **Welcom@123**
*   Schlüsselkennwort des Keystores ".xldatabasekey" von Setup OIG12csp4: **Welcom@123**
*   ".xldatabasekey"-Keystore-Kennwort von OIG11gR2PS3 Setup: **Welcom@123**
*   Schlüsselkennwort des Keystores ".xldatabasekey" von Setup OIG11gR2PS3: **Welcom@123**

Die Neuverkabelung dauert etwa 10-15 Minuten. Warten Sie, bis die folgende Meldung angezeigt wird: "Neuverkabelung erfolgreich abgeschlossen"

      ![](images/10-rewire.png)
    

Die 12c-Domain ist jetzt mit dem upgegradeten 11g-Schema verbunden. Zu diesem Zeitpunkt werden alle Server der 12c-Domäne heruntergefahren. Fahren Sie mit der nächsten Übung fort, um alle Server neu zu starten und den Upgradeprozess abzuschließen.

Sie können jetzt [mit der nächsten Übung fortfahren](#next).

## Danksagungen

*   **Autor** - Keerti R, Brijith TG, Anuj Tripathi, NATD Solution Engineering
*   **Mitwirkende** - Keerti R, Brijith TG, Anuj Tripathi
*   **Zuletzt aktualisiert am/um** - Keerti R, NATD Solution Engineering, Juni 2021