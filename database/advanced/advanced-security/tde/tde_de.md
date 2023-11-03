# Oracle Transparent Data Encryption (TDE)

## Einführung

In diesem Workshop werden die verschiedenen Features und Funktionen der Oracle Transparent Data Encryption (TDE) vorgestellt. Es gibt dem Benutzer die Möglichkeit, zu lernen, wie man diese Funktionen konfiguriert, um sensible Daten zu verschlüsseln.

_Geschätzte Laborzeit:_ 45 Minuten

_In dieser Übung getestete Version:_ Oracle DB 19.19

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

| Schritt Nr. | Merkmal | Ungefähr. Zeit |
| --- | --- | --- |
| 1 | DB-Wiederherstellung zulassen | 5 Minuten |
| 2 | Keystore erstellen | <5 Minuten |
| 3 | Lokalen Keystore für automatische Anmeldung erstellen | <5 Minuten |
| 4 | Master-Schlüssel erstellen | <5 Minuten |
| 5 | Vorhandene USERS-Tablespaces in CDB$ROOT verschlüsseln | 5 Minuten |
| 6 | Encyrpt-Zugangsdaten in CDB$ROOT | 5 Minuten |
| 7 | Tablespaces SYSTEM, SYSAUX und USERS in PDB verschlüsseln | 5 Minuten |
| 8 | Alle neuen Tablespaces anzeigen | 5 Minuten |
| 9 | Masterschlüssel neu erstellen | 5 Minuten |
| 10 | Keystore-Details anzeigen | 5 Minuten |
| 11 | Vor TDE wiederherstellen | 5 Minuten |

## Aufgabe 1: DB-Wiederherstellung zulassen

1.  Öffnen Sie eine Terminalsession auf Ihrer VM **DBSec-Lab** als BS-Benutzer _oracle_
    
        <copy>sudo su - oracle</copy>
        
    
    **Hinweis**: Wenn Sie eine Remote-Desktopsitzung verwenden, doppelklicken Sie auf das Symbol _Terminal_ auf dem Desktop, um eine Sitzung zu starten.
    
2.  Gehen Sie zum Skriptverzeichnis
    
        <copy>cd $DBSEC_LABS/tde</copy>
        
3.  Führen Sie den Backup-Befehl aus:
    
        <copy>./tde_backup_db.sh</copy>
        
    
    ![TDE](./images/tde-001.png "Backup-DB")
    
4.  Nach Abschluss wird der Container und die integrierbaren Datenbanken automatisch neu gestartet
    
    **Hinweis**:
    
    *   Wenn Sie dieses Skript zuvor ausgeführt haben und eine Backupdatei vorhanden ist, wird das Skript nicht abgeschlossen
    *   Sie müssen das vorhandene Backup manuell verwalten (löschen oder verschieben), bevor Sie dieses Skript erneut ausführen

## Aufgabe 2: Keystore erstellen

1.  Führen Sie dieses Skript aus, um die Keystore-Verzeichnisse im Betriebssystem zu erstellen
    
        <copy>./tde_create_os_directory.sh</copy>
        
    
    ![TDE](./images/tde-002.png "Keystore-Verzeichnisse erstellen")
    
2.  Verwenden Sie die Datenbankparameter, um TDE zu verwalten. Damit einer der Parameter wirksam wird, muss die Datenbank neu gestartet werden. Das Skript führt den Neustart für Sie aus.
    
        <copy>./tde_set_tde_parameters.sh</copy>
        
    
    ![TDE](./images/tde-003.png "TDE-Parameter festlegen")
    
3.  Erstellen Sie den Software-Keystore (**Oracle Wallet**) für die Containerdatenbank. Das Statusergebnis wird von `NOT_AVAILABLE` zu `OPEN_NO_MASTER_KEY` angezeigt.
    
        <copy>./tde_create_wallet.sh</copy>
        
    
    ![TDE](./images/tde-004.png "Software-Keystore erstellen")
    
    **Hinweis:** Wir erstellen ein Secret für das Kennwort zur Verwaltung, um es für den nächsten Befehl auszublenden.
    
4.  Jetzt wurde Ihr Oracle Wallet erstellt!
    

## Aufgabe 3: Masterschlüssel erstellen

1.  Um den TDE-Masterschlüssel (**MEK**) der Containerdatenbank zu erstellen, führen Sie den folgenden Befehl aus
    
        <copy>./tde_create_mek_cdb.sh</copy>
        
    
    ![TDE](./images/tde-005.png "TDE-Masterschlüssel der Containerdatenbank erstellen")
    
2.  Um einen Masterschlüssel (MEK) für die integrierbare Datenbank **pdb1** zu erstellen, führen Sie den folgenden Befehl aus
    
        <copy>./tde_create_mek_pdb.sh pdb1</copy>
        
    
    ![TDE](./images/tde-006.png "TDE-Masterschlüssel der integrierbaren Datenbank erstellen")
    
3.  Falls gewünscht, können Sie dasselbe für **pdb2**... Dies ist keine Anforderung, und es kann hilfreich sein, einige Datenbanken mit TDE und einige ohne
    
        <copy>./tde_create_mek_pdb.sh pdb2</copy>
        
    
    ![TDE](./images/tde-007.png "TDE-Masterschlüssel der integrierbaren Datenbank erstellen")
    
4.  Jetzt haben Sie einen Masterschlüssel und können mit der Verschlüsselung von Tablespaces oder Spalten beginnen!
    

## Aufgabe 4: Lokales Wallet mit automatischer Anmeldung erstellen

1.  Führen Sie das Skript aus, um den Oracle Wallet-Inhalt im Betriebssystem anzuzeigen
    
        <copy>./tde_view_wallet_on_os.sh</copy>
        
    
    ![TDE](./images/tde-010.png "Oracle Wallet-Inhalt im BS anzeigen")
    
2.  Sie können das Aussehen von Oracle Wallet in der Datenbank anzeigen
    
        <copy>./tde_view_wallet_in_db.sh</copy>
        
    
    ![TDE](./images/tde-011.png "Oracle Wallet-Inhalt in der Datenbank anzeigen")
    
3.  Erstellen Sie jetzt das **Oracle Wallet automatisch anmelden**
    
        <copy>./tde_create_autologin_wallet.sh</copy>
        
    
    ![TDE](./images/tde-012.png "Oracle Wallet zur automatischen Anmeldung erstellen")
    
4.  Führen Sie dieselben Abfragen aus, um den Oracle Wallet-Inhalt im Betriebssystem anzuzeigen
    
        <copy>./tde_view_wallet_on_os.sh</copy>
        
    
    ![TDE](./images/tde-013.png "Oracle Wallet-Inhalt im BS anzeigen")
    
    **Hinweis**: Jetzt sollte die Datei **cwallet.sso** angezeigt werden.
    
5.  Und keine Änderungen am Oracle Wallet in der Datenbank
    
        <copy>./tde_view_wallet_in_db.sh</copy>
        
    
    ![TDE](./images/tde-014.png "Oracle Wallet-Inhalt in der Datenbank anzeigen")
    
6.  Jetzt wird Ihr Autologin erstellt!
    

## Aufgabe 5: Vorhandenen Tablespace verschlüsseln

1.  Zeigen Sie die Daten in der Datendatei `empdata_prod.dbf`, die mit dem Tablespace `EMPDATA_PROD` verknüpft ist, mit dem Linux-Befehl und den Zeichenfolgen an
    
        <copy>./tde_strings_data_empdataprod.sh</copy>
        
    
    ![TDE](./images/tde-015.png "Daten in der Datendatei anzeigen")
    
    **Hinweis:**
    
    *   Die Daten werden angezeigt, und Sie sind nicht bei der Datenbank angemeldet.
    *   Dies ist ein Betriebssystembefehl, der die Datenbank zur Anzeige der Daten umgeht
    *   Dies wird als "Side-Channel-Angriff" bezeichnet, da die Datenbank sich dessen nicht bewusst ist.
2.  Als Nächstes **verschlüsseln Sie die Daten explizit**, indem Sie den gesamten Tablespace verschlüsseln.
    
        <copy>./tde_encrypt_tbs.sh</copy>
        
    
    ![TDE](./images/tde-016.png "Daten explizit verschlüsseln")
    
    **Hinweis:** Standardmäßig verwendet die Syntax den Verschlüsselungsalgorithmus AES256.
    
3.  Versuchen Sie den Side-Channel-Angriff erneut
    
        <copy>./tde_strings_data_empdataprod.sh</copy>
        
    
    ![TDE](./images/tde-017.png "Versuchen Sie den Side-Channel-Angriff erneut")
    
4.  Sie sehen, dass alle Daten jetzt verschlüsselt und nicht mehr sichtbar sind!
    

## Aufgabe 6: Alle neuen Tablespaces verschlüsseln

1.  Prüfen Sie zunächst die vorhandenen Initialisierungsparameter
    
        <copy>./tde_check_init_params.sh</copy>
        
    
    ![TDE](./images/tde-018.png "Vorhandene Initialisierungsparameter prüfen")
    
2.  Ändern Sie als Nächstes den Initialisierungsparameter `TABLESPACE_ENCRYPTION` in "`AUTO_ENABLE`", um immer **implizit alle neuen Tablespaces zu verschlüsseln**, und den verborgenen Initialisierungsparameter `_tablespace_encryption_default_algorithm`, um "`AES256`" als Standardverschlüsselungsalgorithmus zu verwenden
    
        <copy>./tde_set_encrypt_all_new_tbs.sh</copy>
        
    
    ![TDE](./images/tde-019.png "Initialisierungsparameter ändern")
    
    **Hinweis**:
    
    *   Der Parameter `TABLESPACE_ENCRYPTION` kann nicht geändert werden. Daher **muss die Datenbank neu gestartet werden**!
    *   Dieser Parameter wird **in Oracle Database-Version 19.16 eingeführt** als Alternative zum Parameter `ENCRYPT_NEW_TABLESPACES`
    *   Wie bei `ENCRYPT_NEW_TABLESPACES` können Sie mit diesem Parameter angeben, ob neu erstellte Benutzer-Tablespaces verschlüsselt werden sollen
    *   Wenn das durch die Einstellung `ENCRYPT_NEW_TABLESPACES` angegebene Verhalten mit dem durch die Einstellung `TABLESPACE_ENCRYPTION` angegebenen Verhalten in Konflikt steht, hat das Verhalten `TABLESPACE_ENCRYPTION` Vorrang.
    *   Daher wird `ENCRYPT_NEW_TABLESPACES` automatisch auf `ALWAYS` gesetzt, wenn `TABLESPACE_ENCRYPTION` auf `AUTO_ENABLE` gesetzt ist.
3.  Erstellen und löschen Sie schließlich einen Tablespace-TEST, um den Effekt zu prüfen
    
        <copy>./tde_create_new_tbs.sh</copy>
        
    
    ![TDE](./images/tde-020.png "Tablespace-TEST erstellen und löschen, um den Effekt zu prüfen")
    
    **Hinweis**: Obwohl der Tablespace **TEST** ohne Angabe von Verschlüsselungsparametern erstellt wurde, wird er standardmäßig mit dem Verschlüsselungsalgorithmus AES256 verschlüsselt
    
4.  Jetzt werden alle neuen Tablespaces standardmäßig verschlüsselt!
    

## Aufgabe 7: Masterschlüssel neu erstellen

1.  Um den TDE-Masterschlüssel (MEK) der Containerdatenbank neu einzugeben, führen Sie den folgenden Befehl aus
    
        <copy>./tde_rekey_mek_cdb.sh</copy>
        
    *   Sehen Sie sich den CDB-Schlüssel an, bevor Sie eine neue Eingabe vornehmen...

![TDE](./images/tde-021.png "Vor der erneuten Eingabe des TDE-Masterschlüssels (MEK) der Containerdatenbank")

    - ...and after
    
    ![TDE](./images/tde-022.png "After rekeying the container database TDE Master Key (MEK)")
    
    - You can see the new key generated for the container
    

2.  Um einen Masterschlüssel (MEK) für die integrierbare Datenbank **pdb1** neu einzugeben, führen Sie den folgenden Befehl aus
    
        <copy>./tde_rekey_mek_pdb.sh pdb1</copy>
        
    
    *   Überprüfen Sie die pdb1-Taste, bevor Sie die Eingabe erneut vornehmen...
    
    ![TDE](./images/tde-023.png "Vor der Neuerstellung des TDE-Masterschlüssels (MEK) der integrierbaren Datenbank")
    
    *   ...und nach
    
    ![TDE](./images/tde-024a.png "Nach erneuter Eingabe des TDE-Masterschlüssels (MEK) der integrierbaren Datenbank")
    
    *   Der neue Schlüssel, der für die integrierbare Datenbank generiert wurde, wird angezeigt
3.  Wenn Sie möchten, können Sie dieselbe Aktion für **pdb2** ausführen
    
        <copy>./tde_rekey_mek_pdb.sh pdb2</copy>
        
    
    **Hinweis**:
    
    *   Dies ist jedoch keine Anforderung
    *   Es könnte hilfreich sein, einige Datenbanken mit TDE und einige ohne
4.  Nachdem Sie nun einen Masterschlüssel haben, können Sie mit der Verschlüsselung von Tablespaces oder Spalten beginnen.
    

## Aufgabe 8: Keystore-Details anzeigen

1.  Sobald Sie einen Keystore haben, können Sie eines dieser Skripte ausführen. Sie werden feststellen, dass mehrere Kopien der Datei **ewallet.p12** vorhanden sind. Jedes Mal, wenn Sie eine Änderung vornehmen, einschließlich Erstellen oder Neuerstellen, wird die Datei ewallet.p12 gesichert. Außerdem wird der Inhalt der Oracle Wallet-Datei mit **orapki** angezeigt
    
    *   BS-Dateien für den Keystore anzeigen
    
        <copy>./tde_view_wallet_on_os.sh</copy>
        
    
    ![TDE](./images/tde-024b.png "BS-Dateien für den Keystore anzeigen")
    
    *   Keystore-Daten in der Datenbank anzeigen
    
        <copy>./tde_view_wallet_in_db.sh</copy>
        
    
    ![TDE](./images/tde-024c.png "Keystore-Daten in der Datenbank anzeigen")
    

## Aufgabe 9: Vor TDE wiederherstellen

1.  Führen Sie zunächst dieses Skript aus, um die PFILE wiederherzustellen
    
        <copy>./tde_restore_init_parameters.sh</copy>
        
    
    ![TDE](./images/tde-025.png "PFILE wiederherstellen")
    
2.  Zweitens Datenbank wiederherstellen (dies kann einige Zeit dauern)
    
        <copy>./tde_restore_db.sh</copy>
        
    
    ![TDE](./images/tde-026.png "Datenbank zurückschreiben")
    
3.  Drittens die zugehörigen Oracle Wallet-Dateien löschen
    
        <copy>./tde_delete_wallet_files.sh</copy>
        
    
    ![TDE](./images/tde-027.png "Verknüpfte Oracle Wallet-Dateien löschen")
    
4.  Viertens: Container und integrierbare Datenbanken starten
    
        <copy>./tde_start_db.sh</copy>
        
    
    ![TDE](./images/tde-028.png "Datenbanken starten")
    
    **Hinweis**: Dadurch hätte Ihre Datenbank in den Zustand vor TDE zurückgesetzt.
    
5.  Prüfen Sie schließlich, ob die Initialisierungsparameter nichts über TDE sagen
    
        <copy>./tde_check_init_params.sh</copy>
        
    
    ![TDE](./images/tde-029.png "Prüfen Sie die Initialisierungsparameter")
    
6.  Jetzt wird Ihre Datenbank bis zu dem Zeitpunkt wiederhergestellt, bevor Sie TDE aktivieren, und Sie können Ihr Datenbankbackup entfernen (optional).
    
        <copy>./tde_delete_backup_db.sh</copy>
        
    
    ![TDE](./images/tde-030.png "Entfernen Sie Ihr Datenbank-Backup (optional)")
    

Sie können jetzt mit der nächsten Übung fortfahren!

## **Anhang**: Informationen zum Produkt

### **Überblick**

Diese Features sind fest im Kernprodukt von Oracle Database codiert und Teil der _Advanced Security Option (ASO)_

TDE Ermöglicht die Verschlüsselung von Daten, sodass nur ein autorisierter Empfänger sie lesen kann.

Schützen Sie vertrauliche Daten in einer potenziell ungeschützten Umgebung, wie z. B. Daten, die Sie auf Sicherungsmedien gespeichert haben, die an einen externen Speicherort gesendet werden. Sie können einzelne Spalten in einer Datenbanktabelle verschlüsseln oder einen gesamten Tablespace verschlüsseln.

Nachdem die Daten verschlüsselt wurden, werden diese Daten für autorisierte Benutzer oder Anwendungen transparent entschlüsselt, wenn sie auf diese Daten zugreifen. TDE schützt Daten, die auf Medien gespeichert sind (auch als ruhende Daten bezeichnet), falls das Speichermedium oder die Datendatei gestohlen wird.

Oracle Database verwendet Authentifizierungs-, Autorisierungs- und Auditingmechanismen, um Daten in der Datenbank zu sichern, jedoch nicht in den Betriebssystemdatendateien, in denen Daten gespeichert werden. Zum Schutz dieser Datendateien stellt Oracle Database Transparent Data Encryption (TDE) bereit. TDE verschlüsselt sensible Daten, die in Datendateien gespeichert sind. Um eine nicht autorisierte Entschlüsselung zu verhindern, speichert TDE die Verschlüsselungsschlüssel in einem Sicherheitsmodul außerhalb der Datenbank, das als Keystore bezeichnet wird.

Sie können Oracle Key Vault als Teil der TDE-Implementierung konfigurieren. Auf diese Weise können Sie TDE-Keystores (in Oracle Key Vault als TDE-Wallets bezeichnet) in Ihrem Unternehmen zentral verwalten. Beispiel: Sie können einen Software-Keystore in Oracle Key Vault hochladen und dann den Inhalt dieses Keystores anderen TDE-fähigen Datenbanken zur Verfügung stellen.

![TDE](./images/aso-concept-tde.png "TDE-Konzept")

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

*   **Autor** - Hakim Loumi, PM für Datenbanksicherheit
*   **Mitwirkende** - Peter Wahl
*   **Zuletzt aktualisiert am/um** - Hakim Loumi, Datenbanksicherheit PM - November 2023