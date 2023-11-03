# Oracle Transparent Data Encryption (TDE)

## Einführung

In diesem Workshop werden die verschiedenen Features und Funktionen der Oracle Transparent Data Encryption (TDE) vorgestellt. Es gibt dem Benutzer die Möglichkeit, zu lernen, wie man diese Funktionen konfiguriert, um sensible Daten zu verschlüsseln. Bei der Verwendung dieser LiveLab-Pinnwand erhalten wir auch verschlüsselte Tablespaces, auf die in der nächsten Übung die Komprimierung angewendet wird.

_Geschätzte Zeit:_ 45 Minuten

_In dieser Übung getestete Version:_ Oracle DB 19.17

### Videovorschau

Sehen Sie sich eine Vorschau von "_Livelabs - Oracle ASO (Transparent Data Encryption & Data Redaction) (Mai 2022)_" an[](youtube:JflshZKgxYs)

### Ziele

*   Offlinebackup der Datenbank erstellen, um DB-Wiederherstellung bei Bedarf zu aktivieren
*   Transparente Datenverschlüsselung in der Datenbank aktivieren
*   Daten mit Transparent Data Encryption verschlüsseln

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten (nur _Free-Tier_ und _bezahlte Mandanten_)
    *   Übung: Umgebungssetup
    *   Übung: Umgebung initialisieren

### Laborzeit (geschätzt)

| Aufgabennummer | Merkmal | Ungefähr. Zeit |
| --- | --- | --- |
| 1 | DB-Wiederherstellung zulassen | 5 Minuten |
| 2 | Keystore erstellen | <5 Minuten |
| 3 | Master-Schlüssel erstellen | <5 Minuten |
| 4 | Wallet mit automatischer Anmeldung erstellen | <5 Minuten |
| 5 | Vorhandenen Tablespace verschlüsseln TEST\_DATA | 5 Minuten |
| 6 | Alle neuen Tablespaces verschlüsseln | 5 Minuten |

## Aufgabe 1: DB-Wiederherstellung zulassen

1.  Öffnen Sie eine Terminalsession auf Ihrer VM **DBSec-Lab** als BS-Benutzer _oracle_
    
        <copy>sudo su - oracle</copy>
        
    
    **Hinweis**: Wenn Sie eine Remote-Desktopsitzung verwenden, doppelklicken Sie auf das Symbol _Terminal_ auf dem Desktop, um eine Sitzung zu starten.
    
2.  Gehen Sie zum Skriptverzeichnis
    
        <copy>cd $DBSEC_LABS/tde</copy>
        
3.  Führen Sie den Backup-Befehl aus:
    
        <copy>./tde_backup_db.sh</copy>
        
    
    ![TDE](./images/tde-001.png "TDE")
    
4.  Nach Abschluss wird der Container und die integrierbaren Datenbanken automatisch neu gestartet
    
    **Hinweis**:
    
    *   Wenn Sie dieses Skript zuvor ausgeführt haben und eine Backupdatei vorhanden ist, wird das Skript nicht abgeschlossen
    *   Sie müssen das vorhandene Backup manuell verwalten (löschen oder verschieben), bevor Sie dieses Skript erneut ausführen

## Aufgabe 2: Keystore erstellen

1.  Führen Sie dieses Skript aus, um die Keystore-Verzeichnisse im Betriebssystem zu erstellen
    
        <copy>./tde_create_os_directory.sh</copy>
        
    
    ![TDE](./images/tde-002.png "TDE")
    
2.  Verwenden Sie die Datenbankparameter, um TDE zu verwalten. Damit einer der Parameter wirksam wird, muss die Datenbank neu gestartet werden. Das Skript führt den Neustart für Sie aus.
    
        <copy>./tde_set_tde_parameters.sh</copy>
        
    
    ![TDE](./images/tde-003.png "TDE")
    
3.  Erstellen Sie den Software-Keystore (**Oracle Wallet**) für die Containerdatenbank. Das Statusergebnis wird von `NOT_AVAILABLE` zu `OPEN_NO_MASTER_KEY` angezeigt.
    
        <copy>./tde_create_wallet.sh</copy>
        
    
    ![TDE](./images/tde-004.png "TDE")
    
    **Hinweis:** Wir erstellen ein Secret für das Kennwort zur Verwaltung, um es für den nächsten Befehl auszublenden.
    
4.  Jetzt wurde Ihr Oracle Wallet erstellt!
    

## Aufgabe 3: Masterschlüssel erstellen

1.  Um den TDE-Masterschlüssel (**MEK**) der Containerdatenbank zu erstellen, führen Sie den folgenden Befehl aus
    
        <copy>./tde_create_mek_cdb.sh</copy>
        
    
    ![TDE](./images/tde-005.png "TDE")
    
2.  Um einen Masterschlüssel (MEK) für die integrierbare Datenbank **pdb1** zu erstellen, führen Sie den folgenden Befehl aus
    
        <copy>./tde_create_mek_pdb.sh pdb1</copy>
        
    
    ![TDE](./images/tde-006.png "TDE")
    
3.  Falls gewünscht, können Sie dasselbe für **pdb2**... Dies ist keine Anforderung, und es kann hilfreich sein, einige Datenbanken mit TDE und einige ohne
    
        <copy>./tde_create_mek_pdb.sh pdb2</copy>
        
    
    ![TDE](./images/tde-007.png "TDE")
    
4.  Jetzt haben Sie einen Masterschlüssel und können mit der Verschlüsselung von Tablespaces oder Spalten beginnen!
    

## Aufgabe 4: Wallet mit automatischer Anmeldung erstellen

1.  Führen Sie das Skript aus, um den Oracle Wallet-Inhalt im Betriebssystem anzuzeigen
    
        <copy>./tde_view_wallet_on_os.sh</copy>
        
    
    ![TDE](./images/tde-010.png "TDE")
    
2.  Sie können das Aussehen von Oracle Wallet in der Datenbank anzeigen
    
        <copy>./tde_view_wallet_in_db.sh</copy>
        
    
    ![TDE](./images/tde-011.png "TDE")
    
3.  Erstellen Sie jetzt das **Oracle Wallet automatisch anmelden**
    
        <copy>./tde_create_autologin_wallet.sh</copy>
        
    
    ![TDE](./images/tde-012.png "TDE")
    
4.  Führen Sie dieselben Abfragen aus, um den Oracle Wallet-Inhalt im Betriebssystem anzuzeigen
    
        <copy>./tde_view_wallet_on_os.sh</copy>
        
    
    ![TDE](./images/tde-013.png "TDE")
    
    **Hinweis**: Jetzt sollte die Datei **cwallet.sso** angezeigt werden.
    
5.  Und keine Änderungen am Oracle Wallet in der Datenbank
    
        <copy>./tde_view_wallet_in_db.sh</copy>
        
    
    ![TDE](./images/tde-014.png "TDE")
    
6.  Jetzt wird Ihr Autologin erstellt!
    

## Aufgabe 5: Vorhandenen Tablespace verschlüsseln TEST\_DATA

1.  Zeigen Sie die Daten in der Datendatei `test_data.dbf`, die mit dem Tablespace `TEST_DATA` verknüpft ist, mit dem Linux-Befehl und den Zeichenfolgen an
    
        <copy>
        sqlplus system/Oracle123@localhost:1521/pdb1
        set lines 110
        set pages 9999
        col algorithm       format a10
        col encrypted       format a10
        col file_name       format a45
        col pdb_name        format a20
        col online_status   format a15
        col tablespace_name format a30
        select file_name, online_status from dba_data_files where tablespace_name = 'TEST_DATA';
        </copy>
        
    
        <copy>
        HOST strings /u01/oradata/cdb1/pdb1/test_data.dbf | tail -20
        </copy>
        
    
    ![TDE](./images/tde-015-1.png "TDE")
    
    **Hinweis:**
    
    *   Die Daten werden angezeigt, und Sie sind nicht bei der Datenbank angemeldet.
    *   Dies ist ein Betriebssystembefehl, der die Datenbank zur Anzeige der Daten umgeht
    *   Dies wird als "Side-Channel-Angriff" bezeichnet, da die Datenbank sich dessen nicht bewusst ist.
2.  Als Nächstes **verschlüsseln Sie die Daten explizit**, indem Sie den gesamten Tablespace mit dem Verschlüsselungsalgorithmus AES256 verschlüsseln.
    
        <copy>
        select tablespace_name, encrypted from dba_tablespaces where tablespace_name = 'TEST_DATA';
        ALTER TABLESPACE test_data ENCRYPTION ONLINE USING 'AES256' ENCRYPT FILE_NAME_CONVERT = ('test_data','test_data_enc');
        </copy>
        
    
    ![TDE](./images/tde-016-1.png "TDE")
    
        <copy>
        select tablespace_name, encrypted from dba_tablespaces where tablespace_name = 'TEST_DATA';
        select file_name, online_status from dba_data_files where tablespace_name = 'TEST_DATA';
        
        select a.name pdb_name, b.name tablespace_name, c.ENCRYPTIONALG algorithm
        from v$pdbs a, v$tablespace b, v$encrypted_tablespaces c
        where a.con_id = b.con_id
        and b.con_id = c.con_id
        and b.ts# = c.ts#;
        exit
        </copy>
        
    
    ![TDE](./images/tde-016-2.png "TDE")
    
3.  Versuchen Sie den Side-Channel-Angriff erneut
    
        <copy>
        strings /u01/oradata/cdb1/pdb1/test_data_enc.dbf | tail -20
        </copy>
        
    
    ![TDE](./images/tde-017.png "TDE")
    
4.  Sie sehen, dass alle Daten jetzt verschlüsselt und nicht mehr sichtbar sind!
    

## Aufgabe 6: Alle neuen Tablespaces verschlüsseln

1.  Prüfen Sie zunächst die vorhandenen Initialisierungsparameter
    
        <copy>./tde_check_init_params.sh</copy>
        
    
    ![TDE](./images/tde-018.png "TDE")
    
2.  Ändern Sie als Nächstes den Initialisierungsparameter `TABLESPACE_ENCRYPTION` in "`AUTO_ENABLE`", um immer **implizit alle neuen Tablespaces zu verschlüsseln**, und den verborgenen Initialisierungsparameter `_tablespace_encryption_default_algorithm`, um "`AES256`" als Standardverschlüsselungsalgorithmus zu verwenden
    
        <copy>./tde_set_encrypt_all_new_tbs.sh</copy>
        
    
    ![TDE](./images/tde-019.png "TDE")
    
    **Hinweis**:
    
    *   Der Parameter `TABLESPACE_ENCRYPTION` kann nicht geändert werden. Daher **muss die Datenbank neu gestartet werden**!
    *   Dieser Parameter wird **in Oracle Database-Version 19.16 eingeführt** als Alternative zum Parameter `ENCRYPT_NEW_TABLESPACES`
    *   Wie bei `ENCRYPT_NEW_TABLESPACES` können Sie mit diesem Parameter angeben, ob neu erstellte Benutzer-Tablespaces verschlüsselt werden sollen
    *   Wenn das durch die Einstellung `ENCRYPT_NEW_TABLESPACES` angegebene Verhalten mit dem durch die Einstellung `TABLESPACE_ENCRYPTION` angegebenen Verhalten in Konflikt steht, hat das Verhalten `TABLESPACE_ENCRYPTION` Vorrang.
    *   Daher wird `ENCRYPT_NEW_TABLESPACES` automatisch auf `ALWAYS` gesetzt, wenn `TABLESPACE_ENCRYPTION` auf `AUTO_ENABLE` gesetzt ist.
3.  Erstellen Sie schließlich ein gzip-Backup der verschlüsselten `test_data_enc`\-Beispieldatendatei.
    
        <copy>
        cp /u01/oradata/cdb1/pdb1/test_data_enc.dbf /u01/oradata/test_data_enc.dbf
        du -hs /u01/oradata/test_data_enc.dbf
        gzip /u01/oradata/test_data_enc.dbf
        du -hs /u01/oradata/test_data_enc.dbf.gz 
        </copy>
        
    
    ![TDE](./images/tde-020-1.png "TDE")
    
    **Hinweis**: Wie Sie aus dem obigen Ergebnis sehen können, sind die Vorteile der Speicherkomprimierung nicht mehr vorhanden, wenn wir unseren Tablespace verschlüsseln. Die GZIP-Größe der verschlüsselten Datendatei ist mit der unkomprimierten Datendatei identisch. Dies liegt daran, dass die Speicherkomprimierung nach der Datenverschlüsselung aktiviert ist. Mit Oracle Advanced Compression können wir die Daten komprimieren, bevor wir die Verschlüsselung aktivieren, damit wir sicherstellen können, dass wir bei der Sicherung unserer Daten keine Speicherauswirkungen haben.  
    
4.  Jetzt werden alle neuen Tablespaces standardmäßig verschlüsselt!
    

Sie können jetzt mit der nächsten Übung fortfahren!

## **Anhang**: Informationen zum Produkt

### **Überblick**

Eingebettet in den Kernel der Oracle Database ermöglicht Ihnen der TDE-Teil (Transparent Data Encryption) der Advanced Security Option (ASO) die Verschlüsselung von Daten, sodass diese nur vom RDBMS entschlüsselt werden können.

Schützen Sie vertrauliche Daten in einer potenziell ungeschützten Umgebung, wie z. B. Daten, die Sie auf Sicherungsmedien gespeichert haben, die an einen externen Speicherort gesendet werden. Sie können einzelne Spalten in einer Datenbanktabelle verschlüsseln oder einen gesamten Tablespace verschlüsseln.

Nachdem die Daten verschlüsselt wurden, werden diese Daten für autorisierte Benutzer oder Anwendungen transparent entschlüsselt, wenn sie auf diese Daten zugreifen. TDE schützt Daten, die auf Medien gespeichert sind (auch als ruhende Daten bezeichnet), falls das Speichermedium oder die Datendatei gestohlen wird.

Oracle Database verwendet Authentifizierungs-, Autorisierungs- und Auditingmechanismen, um Daten in der Datenbank zu sichern, jedoch nicht in den Betriebssystemdatendateien, in denen Daten gespeichert werden. Zum Schutz dieser Datendateien stellt Oracle Database Transparent Data Encryption (TDE) bereit. TDE verschlüsselt sensible Daten, die in Datendateien gespeichert sind. Um eine nicht autorisierte Entschlüsselung zu verhindern, speichert TDE die Verschlüsselungsschlüssel in einem Sicherheitsmodul außerhalb der Datenbank, das als Keystore bezeichnet wird.

Sie können Oracle Key Vault als Teil der TDE-Implementierung konfigurieren. Auf diese Weise können Sie TDE-Keystores (in Oracle Key Vault als TDE-Wallets bezeichnet) in Ihrem Unternehmen zentral verwalten. Beispiel: Sie können einen Software-Keystore in Oracle Key Vault hochladen und dann den Inhalt dieses Keystores anderen TDE-fähigen Datenbanken zur Verfügung stellen.

![TDE](./images/aso-concept-tde.png "TDE")

### **Vorteile der transparenten Datenverschlüsselung**

*   Als Sicherheitsadministrator können Sie sicher sein, dass die sensiblen Daten verschlüsselt und somit sicher sind, falls das Speichermedium oder die Datendatei gestohlen wird.
*   Mit TDE können Sie sicherheitsbezogene Compliance-Probleme beheben
*   Sie müssen keine Auxiliary-Tabellen, Trigger oder Views erstellen, um Daten für den autorisierten Benutzer oder die autorisierte Anwendung zu entschlüsseln. Daten aus Tabellen werden für den Datenbankbenutzer und die Anwendung transparent entschlüsselt. Eine Anwendung, die sensible Daten verarbeitet, kann TDE verwenden, um eine starke Datenverschlüsselung mit wenig oder keiner Änderung der Anwendung bereitzustellen
*   Daten werden für Datenbankbenutzer und Anwendungen, die auf diese Daten zugreifen, transparent entschlüsselt. Datenbankbenutzer und -anwendungen müssen nicht wissen, dass die Daten, auf die sie zugreifen, verschlüsselt gespeichert werden
*   Sie können Daten ohne Ausfallzeit auf Produktionssystemen mit `Online Table Redefinition` verschlüsseln oder sie während Wartungsperioden offline verschlüsseln (weitere Informationen zu `Online Table Redefinition` finden Sie unter `Oracle Database Administrator’s Guide`)
*   Sie müssen Ihre Anwendungen nicht ändern, um die verschlüsselten Daten zu verarbeiten. Die Datenbank verwaltet die Datenverschlüsselung und -entschlüsselung
*   Oracle Database automatisiert TDE-Masterverschlüsselungsschlüssel- und Keystore-Verwaltungsvorgänge. Der Benutzer oder die Anwendung muss keine TDE-Masterverschlüsselungsschlüssel verwalten

## Sie möchten mehr erfahren?

Technische Dokumentation

*   [Transparente Datenverschlüsselung (TDE) 19c](https://docs.oracle.com/en/database/oracle/oracle-database/19/asoag/asopart2.html)

Video:

*   _Oracle Transparent Data Encryption (TDE) - Part1 (Januar 2020)_[](youtube:avNWykLpic4)
*   _Oracle Transparent Data Encryption (TDE) - Part2 (Februar 2020)_[](youtube:aUfwG5MIMNU)
*   _Zurück zu den Grundlagen mit Transparent Data Encryption (TDE) (März 2021)_[](youtube:JflshZKgxYs)

## Danksagungen

*   **Autor**
    *   Hakim Loumi, Datenbanksicherheit PM
    *   Royce Fu, Principal Database und O&M Solution Architect
    *   Noah Galloso, Solution Engineer, North America Specialist Hub
*   **Beitragende**
    *   Rene Fontcha
    *   Richard Evans, Produktmanagement für Datenbanksicherheit
    *   Gregg Christman, Database Advanced Compression Product Management
*   **Zuletzt aktualisiert am/um** - Royce Fu, - Juni 2023