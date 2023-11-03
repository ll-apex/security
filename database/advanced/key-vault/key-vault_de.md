# Oracle Key Vault (OKV)

## Einführung

In diesem Workshop werden die verschiedenen Features und Funktionen von Oracle Key Vault (OKV) vorgestellt. Der Benutzer kann lernen, wie Sie diese Appliance zur Verwaltung von Schlüsseln konfigurieren.

_Geschätzte Laborzeit:_ 55 Minuten

_In dieser Übung getestete Version:_ Oracle OKV 21.7

### Videovorschau

Sehen Sie sich eine Vorschau von "_LiveLabs - Oracle Key Vault (Mai 2022)_" an[](youtube:4VR1bbDpUIA)

### Ziele

*   Oracle DB (durch TDE verschlüsselt) mit OKV verbinden
*   Mit OKV das vorhandene DB Wallet verwalten
*   DB-Wallet migrieren und Onlineschlüssel von OKV verwalten

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten (nur _Free-Tier_ und _bezahlte Mandanten_)
    *   Übung: Umgebungssetup
    *   Übung: Umgebung initialisieren

### Laborzeit (geschätzt)

| Schritt Nr. | Merkmal | Ungefähr. Zeit | Details |
| --- | --- | --- | --- |
| 1 | (Erforderliche) TDE-Voraussetzungen | < 10 Minuten |  |
| 2 | Endpunkt hinzufügen | < 10 Minuten |  |
| 3 | Inhalt des virtuellen OKV-Wallets anzeigen | <5 Minuten |  |
| 4 | TDE-Wallet hochladen | 5 Minuten | So sichern Sie das Oracle Wallet in Oracle Key Vault |
| 5 | Zu Online-Masterschlüssel migrieren | 5 Minuten | So konfigurieren Sie die Datenbank für die direkte Kommunikation mit Oracle Key Vault neu |
| 6 | OKV SEPS Wallet erstellen | <5 Minuten |  |
| 7 | ReKey-Vorgang ausführen | 5 Minuten |  |
| 8 | SSH-Schlüsselverwaltung und Remote-Serverzugriffskontrolle mit OKV | 10 Minuten |  |
| 9 | Secret-Management mit OKV | 10 Minuten |  |
| 10 | OKV-Laborkonfiguration zurücksetzen | < 10 Minuten |  |

## Aufgabe 1: (Erforderliche) TDE-Voraussetzungen

**Bevor Sie mit dieser Übung beginnen**, stellen Sie sicher, dass Sie die Schritte 1 bis 4 der Transparent Data Encryption-(TDE-)Livelabs ausgeführt haben.

Wenn Sie sie noch nicht ausgeführt haben, führen Sie sie jetzt aus, indem Sie die folgenden Anweisungen befolgen:

1.  Öffnen Sie eine Terminalsession auf Ihrer VM **DBSec-Lab** als BS-Benutzer _oracle_
    
        <copy>sudo su - oracle</copy>
        
    
    **Hinweis**: Wenn Sie eine Remote-Desktopsitzung verwenden, doppelklicken Sie auf das Symbol _Terminal_ auf dem Desktop, um eine Sitzung zu starten.
    
2.  Zum Verzeichnis der TDE-Skripte wechseln
    
        <copy>cd $DBSEC_LABS/tde</copy>
        
3.  Stellen Sie sicher, dass ein Coldbackup der Datenbank vorhanden ist (**die DB wird neu gestartet!**)
    
        <copy>./tde_backup_db.sh</copy>
        
    
    ![Key Vault](../advanced-security/tde/images/tde-001.png "Backup-DB")
    
4.  Keystore-Verzeichnisse im Betriebssystem erstellen
    
        <copy>./tde_create_os_directory.sh</copy>
        
    
    ![Key Vault](../advanced-security/tde/images/tde-002.png "Keystore-Verzeichnisse erstellen")
    
5.  Verwenden Sie die Datenbankparameter, um TDE zu verwalten (**die DB wird neu gestartet!**)
    
        <copy>./tde_set_tde_parameters.sh</copy>
        
    
    ![Key Vault](../advanced-security/tde/images/tde-003.png "TDE-Parameter festlegen")
    
6.  **Oracle Wallet** für die Containerdatenbank erstellen
    
        <copy>./tde_create_wallet.sh</copy>
        
    
    ![Key Vault](../advanced-security/tde/images/tde-004.png "Software-Keystore erstellen")
    
7.  Erstellen Sie den TDE-Masterschlüssel der Containerdatenbank (**MEK**).
    
        <copy>./tde_create_mek_cdb.sh</copy>
        
    
    ![Key Vault](../advanced-security/tde/images/tde-005.png "TDE-Masterschlüssel der Containerdatenbank erstellen")
    
8.  Erstellen Sie den **pdb1**\-Masterschlüssel (MEK) der integrierbaren Datenbank.
    
        <copy>./tde_create_mek_pdb.sh pdb1</copy>
        
    
    ![Key Vault](../advanced-security/tde/images/tde-006.png "TDE-Masterschlüssel der integrierbaren Datenbank erstellen")
    
9.  **Oracle Wallet für die automatische Anmeldung** beenden
    
        <copy>./tde_create_autologin_wallet.sh</copy>
        
    
    ![Key Vault](../advanced-security/tde/images/tde-012.png "Oracle Wallet zur automatischen Anmeldung erstellen")
    
10.  Alle diese Dateien, einschließlich der Datei **cwallet.sso**, sollten jetzt angezeigt werden.
    
        <copy>./tde_view_wallet_on_os.sh</copy>
        
    
    ![Key Vault](./images/okv-201.png "Oracle Wallet-Inhalt im BS anzeigen")
    
11.  Und das Wallet in der Datenbank, das so festgelegt und verfügbar sein soll
    
        <copy>./tde_view_wallet_in_db.sh</copy>
        
    
    ![Key Vault](./images/okv-202.png "Oracle Wallet-Inhalt in der Datenbank anzeigen")
    
12.  Jetzt ist Ihre Datenbank bereit für die OKV Labs!
    

## Aufgabe 2: Endpunkt hinzufügen

Zunächst müssen wir Oracle Key Vault über unseren Datenbankserver kennen. Dazu erstellen wir ihn als Endpunkt in OKV

1.  Öffnen Sie einen Webbrowser für _`https://kv`_, um auf die Oracle Key Vault-Konsole zuzugreifen
    
    **Anmerkungen:** Wenn Sie den Remotedesktop nicht verwenden, können Sie diese Seite auch über _`https://<OKV-VM_@IP-Public>`_ aufrufen.
    
2.  Melden Sie sich mit den folgenden Zugangsdaten bei der Oracle Key Vault-Webkonsole an
    
        <copy>KVRESTADMIN</copy>
        
    
        <copy>T06tron.</copy>
        
    
    ![Key Vault](./images/okv-001.png "Key Vault - Anmeldung")
    
3.  Gehen Sie zur Registerkarte **Endpunkte**
    
    ![Key Vault](./images/okv-002.png "Key Vault - Endpunkt")
    
4.  Sie werden sehen, dass keine Endpunkte verfügbar sind
    
    ![Key Vault](./images/okv-003.png "Key Vault - Endpunkt")
    
5.  Sie verwenden die Datei **OKVdeploy.tgz**, um das Utility zur Automatisierung der Prozesse bereitzustellen
    
    *   Öffnen Sie eine Terminalsession auf Ihrer VM **DBSec-Lab** als BS-Benutzer _oracle_
        
            <copy>sudo su - oracle</copy>
            
        
        **Hinweis**: Wenn Sie eine Remote-Desktopsitzung verwenden, doppelklicken Sie auf das Symbol _Terminal_ auf dem Desktop, um eine Sitzung zu starten.
        
    *   Gehen Sie zum Skriptverzeichnis
        
            <copy>cd $DBSEC_LABS/okv</copy>
            
    *   Dekomprimieren Sie die Binärdatei (wir haben die Datei bereits für Sie in die VM DBSecLab heruntergeladen)
        
            <copy>./okv_unpack_restservice.sh</copy>
            
        
        ![Key Vault](./images/okv-004.png "Key Vault-Binärdatei entpacken")
        
    *   OKV-Utilitykonfiguration erstellen
        
        *   Prüfen Sie die aktuelle OKV-Konfigurationsdatei **okvrestcli.ini**
            
        *   **okvrestcli.jar** herunterladen
            
        *   Erstellen Sie das automatisierte Skript **okv-ep.sh**, um den Endpunkt hinzuzufügen
            
                <copy>./okv_crea_config_script.sh</copy>
                
            
            ![Key Vault](./images/okv-005a.png "OKV-Konfigurationsskripte erstellen") ![Key Vault](./images/okv-005b.png "OKV-Konfigurationsskripte erstellen")
            
            **Hinweis**:
            
            *   Das Skript _`OKV-ep.sh`_ automatisiert den Prozess zum Erstellen des Endpunkts, des Oracle Wallet und zum Bereitstellen der OKV-Software.
            *   Außerdem wird die neueste Version des Serviceutilitys RESTful vom OKV-Server heruntergeladen
    *   Fügen Sie die **cdb1**\-Datenbank unter DBSec-Lab VM als Endpunkt hinzu
        
            <copy>./okv_add_endpoint.sh</copy>
            
        
        ![Key Vault](./images/okv-006.png "Endpunkt hinzufügen")
        
    *   Bevor Sie fertig sind, müssen Sie das Endpunktkennwort ändern
        
        *   Dies ist das Kennwort, das die OKV-Endpunktclientsoftware zur Kommunikation mit dem Key Vault-Server verwendet
            
        *   Ändern Sie das Standard-Wallet-Kennwort "_`change-on-install`_" durch das neue Kennwort "_`Oracle123`_".
            
                <copy>./okv_change_endpoint_pwd.sh</copy>
                
            
                <copy>change-on-install</copy>
                
            
                <copy>Oracle123</copy>
                
            
            ![Key Vault](./images/okv-007.png "Kennwort ändern")
            
6.  Kehren Sie zur OKV-Konsole zurück, aktualisieren Sie den Bildschirm, und jetzt sollte der soeben hinzugefügte Endpunkt angezeigt werden
    
    ![Key Vault](./images/okv-008.png "Key Vault - Endpunkt")
    
7.  Klicken Sie auf den Endpunktnamen (hier _`CDB1_ON_DBSECLAB`_)
    
8.  Stellen Sie im Abschnitt **Standard-Wallet** sicher, dass das in OKV erstellte Wallet das Standard-Wallet für diesen Endpunkt ist
    
    ![Key Vault](./images/okv-009.png "Standard-Wallet-Abschnitt")
    
9.  Ihr Endpunkt wurde hinzugefügt!
    

## Aufgabe 3: Inhalt des virtuellen OKV-Wallets anzeigen

Sie können dieses Skript jederzeit nach dem Hinzufügen des Endpunkts zu diesem Host ausführen, um den Inhalt des virtuellen Wallets in Oracle Key Vault anzuzeigen

1.  Kehren Sie zur Terminalsession zurück, und zeigen Sie den Wallet-Inhalt im **Betriebssystem** an
    
        <copy>./okv_view_wallet_on_os.sh</copy>
        
    
    ![Key Vault](./images/okv-010.png "OKV-Wallet-Inhalt im BS anzeigen")
    
2.  ... innerhalb der **Datenbank** (in `V$ENCRYPTION_WALLET`)
    
        <copy>./okv_view_wallet_in_db.sh</copy>
        
    
    ![Key Vault](./images/okv-011.png "OKV-Wallet-Inhalt in der Datenbank anzeigen")
    
3.  ... und schließlich in **Key Vault**
    
        <copy>./okv_view_wallet_in_kv.sh</copy>
        
    
    ![Key Vault](./images/okv-012.png "OKV-Wallet-Inhalt in Key Vault anzeigen")
    

## Aufgabe 4: TDE-Wallet hochladen

In der Regel laden Benutzer zunächst ihre vorhandenen Oracle Wallets (**ewallet.p12**\-Dateien) in Oracle Key Vault hoch.

1.  Laden Sie das Oracle Wallet in Oracle Key Vault hoch (zur Erinnerung lautet das Kennwort "_`Oracle123`_")
    
        <copy>./okv_upload_wallet.sh</copy>
        
    
        <copy>Oracle123</copy>
        
    
    ![Key Vault](./images/okv-013.png "Oracle Wallet in OKV hochladen")
    
2.  Zeigen Sie jetzt den neuen Inhalt des virtuellen Wallets in der Datenbank an
    
        <copy>./okv_view_wallet_in_db.sh</copy>
        
    
    ![Key Vault](./images/okv-014.png "OKV-Wallet-Inhalt in der Datenbank anzeigen")
    
3.  ... und in Key Vault
    
        <copy>./okv_view_wallet_in_kv.sh</copy>
        
    
    ![Key Vault](./images/okv-015.png "OKV-Wallet-Inhalt in Key Vault anzeigen")
    
4.  Gehen Sie als _`KVRESTADMIN`_ zurück zur OKV-Webkonsole, um diese Informationen anzuzeigen
    
    ![Key Vault](./images/okv-001.png "KVRESTADMIN-Benutzer")
    
5.  Gehen Sie zur Registerkarte **Keys & Wallets**, und klicken Sie auf _`CDB1`_
    
    ![Key Vault](./images/okv-016.png "Abschnitt Schlüssel und Geldbörsen")
    
6.  Im Abschnitt **Wallet-Inhalt** können Sie **alle gerade hochgeladenen Wallet-Inhalte anzeigen**
    
    ![Key Vault](./images/okv-017.png "Abschnitt "Wallet-Inhalte"")
    
    **Hinweis:** Er entspricht genau dem, was Sie im Skript `okv_view_wallet_in_kv.sh` sehen können.
    

## Aufgabe 5: Zu Online-Masterschlüssel migrieren

Nachdem Sie die Oracle Wallet-Dateien in OKV Server hochgeladen haben, können Sie von der Speicherung unserer Masterschlüssel in Wallet-Dateien zur Abfrage aus Oracle Key Vault migrieren

1.  Kehren Sie zu Ihrer Terminalsession zurück, und migrieren Sie das virtuelle Wallet in den Online-Masterschlüssel. In diesem Schritt legen Sie die Initialisierungsparameter `TDE_CONFIGURATION` von `KEYSTORE_CONFIGURATION=FILE` auf `KEYSTORE_CONFIGURATION=OKV|FILE` fest. Dies ist ein dynamischer Parameter, sodass die Datenbank nicht neu gestartet werden muss.
    
        <copy>./okv_migrate_wallet_to_kv.sh</copy>
        
    
    ![Key Vault](./images/okv-018.png "Oracle Wallet in Online-Masterschlüssel migrieren")
    
2.  Zeigen Sie jetzt die Inhalte des Wallets an
    
    *   ... in der Datenbank
        
            <copy>./okv_view_wallet_in_db.sh</copy>
            
        
        ![Key Vault](./images/okv-019.png "OKV-Wallet-Inhalt in der Datenbank anzeigen")
        
        **Hinweis:** Jetzt werden Zeilen für OKV angezeigt.
        
    *   ... und in Key Vault
        
            <copy>./okv_view_wallet_in_kv.sh</copy>
            
        
        ![Key Vault](./images/okv-020.png "OKV-Wallet-Inhalt in Key Vault anzeigen")
        
        **Hinweis:** Jetzt werden Zeilen für TDE MEK migriert (Zeilen mit MKID) angezeigt.
        
3.  Sobald Sie damit einverstanden sind, können Sie die vorhandenen Wallet-Dateien in `$TDE_HOME` löschen.
    
        <copy>./okv_delete_wallet_files.sh</copy>
        
    
    ![Key Vault](./images/okv-021.png "Oracle Wallet löschen")
    
    **Hinweis**:
    
    *   Zur Sicherheit erstellen wir ein temporäres Backupverzeichnis in `$TDE_HOME/backup` und verschieben die Wallet-bezogenen Dateien in das Verzeichnis.
    *   Wenn Sie es wirklich löschen möchten, nachdem Sie bestätigt haben, dass alles erfolgreich war, können Sie
4.  Gehen Sie als _`KVRESTADMIN`_ zurück zur OKV-Webkonsole, um diese Informationen anzuzeigen
    
    ![Key Vault](./images/okv-001.png "KVRESTADMIN-Benutzer")
    
5.  Gehen Sie zur Registerkarte **Keys & Wallets**, und klicken Sie auf _`CDB1`_
    
    ![Key Vault](./images/okv-016.png "Abschnitt Schlüssel und Geldbörsen")
    
6.  Im Abschnitt **Wallet-Inhalt** können Sie **alle gerade migrierten Wallet-Inhalte anzeigen**
    
    ![Key Vault](./images/okv-022.png "Abschnitt "Wallet-Inhalte"")
    
    **Hinweis:**
    
    *   Es ist genau das gleiche wie das, was Sie aus dem Skript `okv_view_wallet_in_kv.sh` sehen können.
    *   In der rechten unteren Ecke sehen Sie, dass diese 2 neuen Zeilen zu den 9 vorhandenen Zeilen hinzugefügt wurden

## Aufgabe 6: OKV SEPS Wallet erstellen

Häufig müssen Sie Verbindungen zur Datenbank über Shell-Skripte im Dateisystem herstellen. Dies kann ein großes Sicherheitsproblem sein, wenn diese Skripte die Details der Datenbankverbindung enthalten. Eine Lösung besteht darin, die BS-Authentifizierung zu verwenden. Oracle bietet Ihnen die Möglichkeit, einen **Secure External Password Store (SEPS)** zu verwenden, in dem die Oracle-Anmeldedaten in einem clientseitigen Oracle Wallet gespeichert sind. Dadurch können Aufgaben zwischen DBAs, die das OKV-Kennwort nicht mehr kennen müssen, und den OKV-Administratoren getrennt werden!

1.  Legen Sie das OKV-Endpunktpasswort in das SEPS-Wallet ab
    
        <copy>./okv_add_kv_pwd_to_seps.sh</copy>
        
    
    ![Key Vault](./images/okv-023.png "Legen Sie das OKV-Endpunktpasswort in das SEPS-Wallet ab")
    
    **Hinweis:** Das OKV-Kennwort wurde jetzt im SEPS-Wallet (`${SEPS_WALLET_DIR}/cwallet.sso`) gespeichert
    
2.  Fertigstellen, um das SEPS-Wallet in der Datenbank festzulegen, indem Secret für OKV-Keystore für automatische Anmeldung hinzugefügt wird
    
        <copy>./okv_setup_external_store.sh</copy>
        
    
    ![Key Vault](./images/okv-024.png "SEPS-Wallet in der Datenbank festlegen")
    
    **Hinweis:** Das Datum des TDE-Wallets auto\_login (`${TDE_HOME}/cwallet.sso`) wird angezeigt. Nun wurde das OKV-Kennwort gespeichert.
    
3.  Jetzt können Sie den Keystore verwalten, indem Sie sich über den externen Speicher anmelden und das Kennwort nicht angeben
    

## Aufgabe 7: Neueingabevorgang ausführen

Sie müssen einen Masterschlüssel für die Containerdatenbank erstellen, bevor Sie fortfahren können. Jede integrierbare Datenbank muss ebenfalls einen eigenen Masterschlüssel haben (außer `PDB$SEED`)

1.  Kehren Sie zur Terminalsession zurück, und geben Sie den TDE-Masterschlüssel der **Containerdatenbank** neu ein
    
        <copy>./okv_online_cdb_rekey.sh</copy>
        
    
    ![Key Vault](./images/okv-025.png "Schlüssel für TDE-Masterschlüssel der Containerdatenbank neu eingeben")
    
    **Hinweis:**
    
    *   Nachdem Sie das SEPS-Wallet in der vorherigen Übung erstellt haben, können Sie sich jetzt über den Befehl "Externer Speicher" anmelden
    *   Vergessen Sie nicht, ein explizites Tag zu setzen, um Ihren Schlüssel einfacher zu finden
2.  Erstellen Sie jetzt einen neuen Masterschlüssel für die integrierbare Datenbank **pdb1**
    
        <copy>./okv_online_pdb_rekey.sh pdb1</copy>
        
    
    ![Key Vault](./images/okv-026.png "Schlüssel der integrierbaren Datenbank-TDE-Masterschlüssel neu eingeben")
    
3.  Falls gewünscht, können Sie dasselbe für **pdb2** tun. Dies ist keine Anforderung und es könnte hilfreich sein, einige Datenbanken mit TDE und einige ohne anzuzeigen!
    
        <copy>./okv_online_pdb_rekey.sh pdb2</copy>
        
4.  Zeigen Sie jetzt die neuen Inhalte des virtuellen Wallets in Key Vault an
    
        <copy>./okv_view_wallet_in_kv.sh</copy>
        
    
    ![Key Vault](./images/okv-027.png "OKV-Wallet-Inhalt in Key Vault anzeigen")
    
5.  Gehen Sie als _`KVRESTADMIN`_ zurück zur OKV-Webkonsole, um diese Informationen anzuzeigen
    
    ![Key Vault](./images/okv-001.png "KVRESTADMIN-Benutzer")
    
6.  Gehen Sie zur Registerkarte **Keys & Wallets**, und klicken Sie auf _`CDB1`_
    
    ![Key Vault](./images/okv-016.png "Abschnitt Schlüssel und Geldbörsen")
    
7.  Im Abschnitt **Wallet-Inhalt** werden die neu eingegebenen Masterschlüssel für **cdb1** und **pdb1** (und pdb2) angezeigt.
    
    ![Key Vault](./images/okv-028.png "Abschnitt "Wallet-Inhalte"")
    
    **Hinweis:**
    
    *   Es ist genau das gleiche wie das, was Sie aus dem Skript `okv_view_wallet_in_kv.sh` sehen können.
    *   In der rechten unteren Ecke sehen Sie, dass diese 2 neuen Zeilen zu den 11 vorhandenen Zeilen hinzugefügt wurden
8.  Klicken Sie auf die Schaltfläche "**Weiter**", um die 2. Ergebnisseite anzuzeigen
    
    ![Key Vault](./images/okv-029.png "Abschnitt "Wallet-Inhalte"")
    
9.  Jetzt haben Sie den Masterschlüssel für den Container und die integrierbaren Datenbanken neu eingegeben!
    

## Aufgabe 8: SSH-Schlüsselverwaltung und Remote-Serverzugriffskontrolle

In dieser Übung führen wir Remote-Serverzugriffskontrollen durch die zentrale Verwaltung von Public Keys für Benutzer ein. Im zweiten Teil werden wir die privaten Schlüssel der Benutzer in OKV verwalten, so dass diese privaten Schlüssel nicht extrahierbar sind.

1.  ...

## Aufgabe 9: Secret-Management mit OKV

In dieser Übung rufen wir ein Kennwort für den Datenbankaccount aus OKV On-Demand ab

1.  Neuen Endpunkt für die Secret-Verwaltung erstellen
    
        <copy>./okv_add_endpoint_secret.sh</copy>
        
    
    ![Key Vault](./images/okv-030.png "Neuen Endpunkt für die Secret-Verwaltung erstellen")
    
    **Hinweis**:
    
    *   Wir erstellen ein Verzeichnis für eine Nicht-DB EndPoint. Hier ein Endpunkt für den DB-Account
    *   Wir stellen EndPoint ohne Kennwort bereit und ändern die Clientkonfiguration in `$OKV_RESTHOME/conf/okvrestcli.ini` so, dass sie auf das Secret EndPoint-Wallet-Verzeichnis verweist.
2.  Erstellen Sie das Secret-Kennwort, und laden Sie es in OKV hoch
    
        <copy>./okv_crea_secret_pwd.sh</copy>
        
    
    ![Key Vault](./images/okv-031.png "Secret-Kennwort in OKV erstellen")
    
    **Hinweis**:
    
    *   Dieses Skript generiert eine JSON-Datei (`$OKV_RESTHOME/sec-reg.JSON`), um das Secret zu registrieren
    *   Nach der Generierung wird das Secret-Kennwort in OKV hochgeladen
    *   OKV antwortet mit der eindeutigen ID des Secret-Passworts... **Kopieren Sie es bitte zur späteren Verwendung**!
    *   Da sich das Kennwort jetzt in OKV befindet, benötigen wir keine temporäre Datei mehr, die das Secret-Kennwort enthält. Daher wird es vom Skript gelöscht
3.  Definieren Sie jetzt die benutzerdefinierten Attribute für das Secret-Kennwort (**fügen Sie die eindeutige ID des zuvor kopierten Secrets als Parameter ein**)
    
        <copy>./okv_add_secret_attributes.sh <SECRET_UNIQUE_ID></copy>
        
    
    ![Key Vault](./images/okv-032.png "Benutzerdefinierte Attribute für das Secret-Kennwort definieren")
    
    **Hinweis**:
    
    *   Wir fügen den Benutzernamen des DB-Benutzers (hier `REFRESH_DWH)` und die Verbindungszeichenfolge zur Datenbank hinzu (hier "`dbsec-lab:1521/pdb1`")
    *   Eine abschließende Prüfung bestätigt, dass alle benutzerdefinierten Attribute korrekt festgelegt sind
4.  Testen Sie schließlich die Secret-Konfiguration, indem Sie sich mit dem Secret-Kennwort bei der Datenbank anmelden (mit dem DB-Benutzer "_`REFRESH_DWH`_" und der Verbindungszeichenfolge "_`dbsec-lab:1521/pdb1`_" als Parametern)
    
        <copy>./okv_login_with_secret.sh REFRESH_DWH dbsec-lab:1521/pdb1</copy>
        
    
    ![Key Vault](./images/okv-033.png "Secret-Konfiguration testen")
    
    **Hinweis**:
    
    *   Wie Sie sehen können, können Sie sich bei der Ziel-DB anmelden, ohne das Kennwort zu kennen oder es einzugeben, da sich dieses Secret jetzt in OKV befindet!
    *   Nach 3 Sekunden wird die SQL-Session durch das Skript unterbrochen und automatisch beendet.
5.  Wenn Sie mit diesem Konzept zufrieden sind, setzen Sie die Secret-Konfiguration zurück
    
        <copy>./okv_clean_endpoint_secret.sh</copy>
        
    
    ![Key Vault](./images/okv-034.png "Secret-Konfiguration zurücksetzen")
    
6.  Herzlichen Glückwunsch, jetzt wissen Sie, wie Sie ein Geheimnis mit OKV verwenden und verwalten!
    

\-->

## Aufgabe 10: OKV-Laborkonfiguration zurücksetzen

1.  Endpunkt und Wallet löschen, die in OKV während dieser Übung erstellt wurden
    
        <copy>./okv_reset_config.sh</copy>
        
    
    ![Key Vault](./images/okv-050.png "OKV-Konfiguration zurücksetzen")
    
2.  OKV-Binärdateien zurücksetzen
    
        <copy>
        rm -Rf $OKV_HOME
        rm -Rf $OKV_RESTHOME/!(*.tgz)
        ll $OKV_RESTHOME
        </copy>
        
    
    ![Key Vault](./images/okv-051.png "OKV-Binärdateien zurücksetzen")
    
3.  Hochgeladene Schlüssel in Key Vault löschen
    
    *   Zurück zur OKV-Webkonsole als _`KVRESTADMIN`_
        
        ![Key Vault](./images/okv-001.png "Hochgeladene Schlüssel in Key Vault löschen")
        
    *   Gehen Sie zur Registerkarte **Schlüssel und Wallets**, und wählen Sie das Untermenü **Schlüssel und Secrets** aus.
        
        ![Key Vault](./images/okv-052.png "Abschnitt "Schlüssel und Secrets"")
        
    *   Wählen Sie ALLE Elemente, und klicken Sie auf \[**Löschen**\]
        
        ![Key Vault](./images/okv-053.png "Alle Artikel löschen")
        
    *   Bestätigen Sie das Löschen, indem Sie auf \[**OK**\] klicken
        
        ![Key Vault](./images/okv-054.png "Alle Artikel löschen")
        
    *   Jetzt wurden Ihre hochgeladenen Schlüssel entfernt
        
        ![Key Vault](./images/okv-055.png "Alle Artikel löschen")
        
4.  DB wie before-TDE wiederherstellen
    
    *   Zum Verzeichnis der TDE-Skripte wechseln
        
            <copy>cd $DBSEC_LABS/tde</copy>
            
    *   Führen Sie zunächst dieses Skript aus, um die PFILE wiederherzustellen
        
            <copy>./tde_restore_init_parameters.sh</copy>
            
        
        ![Key Vault](../advanced-security/tde/images/tde-025.png "PFILE wiederherstellen")
        
    *   Zweitens Datenbank wiederherstellen (dies kann einige Zeit dauern)
        
            <copy>./tde_restore_db.sh</copy>
            
        
        ![Key Vault](../advanced-security/tde/images/tde-026.png "Datenbank zurückschreiben")
        
    *   Drittens die zugehörigen Oracle Wallet-Dateien löschen
        
            <copy>./tde_delete_wallet_files.sh</copy>
            
        
        ![Key Vault](../advanced-security/tde/images/tde-027.png "Verknüpfte Oracle Wallet-Dateien löschen")
        
    *   Viertens: Container und integrierbare Datenbanken starten
        
            <copy>./tde_start_db.sh</copy>
            
        
        ![Key Vault](../advanced-security/tde/images/tde-028.png "Datenbanken starten")
        
        **Hinweis**: Dadurch hätte Ihre Datenbank in den Zustand vor TDE zurückgesetzt.
        
    *   Prüfen Sie schließlich, ob die Initialisierungsparameter nichts über TDE sagen
        
            <copy>./tde_check_init_params.sh</copy>
            
        
        ![Key Vault](../advanced-security/tde/images/tde-029.png "Prüfen Sie die Initialisierungsparameter")
        
    *   Kehren Sie zum OKV-Skriptsverzeichnis zurück, und zeigen Sie den Oracle Wallet-Inhalt in der **Datenbank** an
        
            <copy>$DBSEC_LABS/okv/okv_view_wallet_in_db.sh</copy>
            
        
        ![Key Vault](./images/okv-056.png "Oracle Wallet-Inhalte in der Datenbank anzeigen")
        
5.  **Jetzt können Sie diese Übung aus Aufgabe 1 erneut durchführen** (Ihre Datenbank wird bis zu dem Zeitpunkt wiederhergestellt, zu dem Sie TDE aktivieren).
    

Sie können jetzt mit der nächsten Übung fortfahren!

## **Anhang**: Informationen zum Produkt

### **Überblick**

Oracle Key Vault ist eine sicherheitserprobte Full-Stack-Software Appliance, mit der Sie Schlüssel und Sicherheitsobjekte im Unternehmen zentral verwalten können.

Oracle Key Vault ist eine robuste, sichere und standardkonforme Schlüsselverwaltungsplattform, auf der Sie Ihre Sicherheitsobjekte speichern, verwalten und freigeben können.

![Key Vault](./images/okv-concept.png "Key Vault-Konzept")

Zu den Sicherheitsobjekten, die Sie mit Oracle Key Vault verwalten können, gehören Verschlüsselungsschlüssel, Oracle Wallets, Java Keystores (JKS), Java Cryptography Extension Keystores (JCEKS) und Zugangsdatendateien.

Oracle Key Vault zentralisiert den Verschlüsselungsschlüsselspeicher in Ihrem Unternehmen schnell und effizient. Die zentralisierte, hochverfügbare und skalierbare Sicherheitslösung von Oracle Key Vault, die auf Oracle Linux, Oracle Database, Oracle Database und Oracle GoldenGate-Sicherheitsfunktionen wie Oracle Transparent Data Encryption, Oracle Database Vault, Oracle Virtual Private Database und Oracle GoldenGate basiert, hilft dabei, die größten Herausforderungen bei der Schlüsselverwaltung zu meistern, mit denen Unternehmen heute konfrontiert sind. Mit Oracle Key Vault können Sie Ihre Sicherheitsobjekte beibehalten, sichern und wiederherstellen, ihren versehentlichen Verlust verhindern und ihren Lebenszyklus in einer geschützten Umgebung verwalten.

Oracle Key Vault ist für den Oracle Stack (Datenbank, Middleware, Systeme) und die transparente Datenverschlüsselung (TDE) für erweiterte Sicherheit optimiert. Darüber hinaus entspricht es dem Branchenstandard OASIS Key Management Interoperability Protocol (KMIP) für die Kompatibilität mit KMIP-basierten Clients.

Mit Oracle Key Vault können Sie eine Vielzahl anderer Endpunkte verwalten, wie MySQL TDE-Verschlüsselungsschlüssel.

Ab Oracle Key Vault Release 18.1 ist ein neuer Multi-Master-Clustermodus verfügbar, mit dem die Verfügbarkeit erhöht und die geografische Verteilung unterstützt wird.

Die Multi-Master-Clusterknoten bieten High Availability, Disaster Recovery, Lastverteilung und geografische Verteilung in einer Oracle Key Vault-Umgebung.

Ein Multi-Master-Cluster von Oracle Key Vault bietet einen Mechanismus zum Erstellen von Paaren von Oracle Key Vault-Knoten für maximale Verfügbarkeit und Zuverlässigkeit.

![Key Vault](./images/okv-cluster-concept.png "Key Vault - Multi-Master-Konzept")

Oracle Key Vault unterstützt zwei Modultypen für Clusterknoten: schreibgeschützter eingeschränkter Modus oder Lese-/Schreibmodus.

*   **Schreibgeschützter eingeschränkter Modus**
    
    In diesem Modus können nur nicht kritische Daten aktualisiert oder dem Knoten hinzugefügt werden. Kritische Daten können in diesem Modus nur durch Replikation aktualisiert oder hinzugefügt werden. Es gibt zwei Situationen, in denen sich ein Knoten im schreibgeschützten eingeschränkten Modus befindet:
    
    *   Ein Knoten ist schreibgeschützt und verfügt noch nicht über einen Peer mit Lese-/Schreibzugriff.
    *   Ein Knoten ist Teil eines Lese-/Schreibpaars, aber die Kommunikation mit seinem Lese-/Schreib-Peer wurde unterbrochen oder es ist ein Knotenfehler aufgetreten. Wenn einer der beiden Knoten nicht betriebsbereit ist, wird der verbleibende Knoten auf den schreibgeschützten eingeschränkten Modus gesetzt. Wenn ein Lese-/Schreibknoten wieder mit seinem Lese-/Schreib-Peer kommunizieren kann, kehrt der Knoten vom schreibgeschützten eingeschränkten Modus in den Lese-/Schreibmodus zurück.
*   **Lese- und Schreibmodus**
    

In diesem Modus können sowohl kritische als auch nicht kritische Informationen auf einen Knoten geschrieben werden. Ein Lese-/Schreibknoten sollte immer im Lese-/Schreibmodus arbeiten.

Sie können dem Cluster schreibgeschützte Oracle Key Vault-Knoten hinzufügen, um Endpunkten, die Oracle-Wallets, Verschlüsselungsschlüssel, Java-Keystores, Zertifikate, Zugangsdatendateien und andere Objekte benötigen, noch mehr Verfügbarkeit zu bieten.

Ein Oracle Key Vault-Multimastercluster ist eine miteinander verbundene Gruppe von Oracle Key Vault-Knoten. Jeder Knoten im Cluster wird automatisch für die Verbindung mit allen anderen Knoten in einem vollständig verbundenen Netzwerk konfiguriert. Die Knoten können geografisch verteilt sein, und Oracle Key Vault-Endpunkte interagieren mit jedem Knoten im Cluster.

Diese Konfiguration repliziert Daten auf allen anderen Knoten und reduziert so das Risiko eines Datenverlusts. Um Datenverlust zu vermeiden, müssen Sie Knotenpaare konfigurieren, die als Lese-/Schreibpaare bezeichnet werden, um die bidirektionale synchrone Replikation zu aktivieren. Mit dieser Konfiguration kann ein Update auf einen Knoten auf den anderen Knoten repliziert werden, und prüft dies auf dem anderen Knoten, bevor das Update als erfolgreich betrachtet wird. Kritische Daten können nur innerhalb der Lese-/Schreibpaare hinzugefügt oder aktualisiert werden. Alle hinzugefügten oder aktualisierten Daten werden asynchron auf den Rest des Clusters repliziert.

Nachdem Sie den Upgradeprozess abgeschlossen haben, muss sich jeder Knoten im Oracle Key Vault-Cluster in Oracle Key Vault Release 18.1 oder höher und innerhalb eines Releaseupdates aller anderen Knoten befinden. Jeder neue Oracle Key Vault-Server, der dem Cluster beitreten soll, muss sich auf derselben Releaseebene wie das Cluster befinden.

Die Uhren auf allen Knoten des Clusters müssen synchronisiert werden. Daher müssen für alle Knoten des Clusters die NTP-(Network Time Protocol-)Einstellungen aktiviert sein.

Jeder Knoten im Cluster kann Endpunkte aktiv und unabhängig bedienen und gleichzeitig ein identisches Dataset durch kontinuierliche Replikation im gesamten Cluster verwalten. Die kleinste Konfiguration ist ein 2-Knoten-Cluster, und die größte Konfiguration kann bis zu 16 Knoten mit mehreren Paaren haben, die über mehrere Data Center verteilt sind.

### **Vorteile von Oracle Key Vault**

*   Oracle Key Vault unterstützt Sie bei der Bekämpfung von Sicherheitsbedrohungen, der Zentralisierung des Schlüsselspeichers und der Zentralisierung des Schlüssellebenszyklusmanagements
*   Durch das Deployment von Oracle Key Vault in Ihrer Organisation können Sie Folgendes erreichen:
*   Verwalten Sie den Lebenszyklus für Endpunktsicherheitsobjekte und -schlüssel, einschließlich Schlüsselerstellung, Rotation, Deaktivierung und Entfernung
*   Verhindern Sie den Verlust von Schlüsseln und Wallets durch vergessene Passwörter oder versehentliches Löschen
*   Schlüssel sicher zwischen autorisierten Endpunkten im gesamten Unternehmen freigeben
*   Einfache Registrierung und Bereitstellung von Endpunkten mit einem einzigen Softwarepackage, das alle erforderlichen Binärdateien, Konfigurationsdateien und Endpunktzertifikate für gegenseitig authentifizierte Verbindungen zwischen Endpunkten und Oracle Key Vault enthält
*   Arbeiten Sie zusätzlich zu Transparent Data Encryption (TDE) mit anderen Oracle-Produkten und -Features wie Oracle Real Application Clusters (Oracle RAC), Oracle Data Guard, integrierbaren Datenbanken und Oracle GoldenGate zusammen. Oracle Key Vault erleichtert das Verschieben verschlüsselter Daten mit Oracle Data Pump und Transportable Tablespaces, einem wichtigen Feature von Oracle Database
*   Oracle Key Vault Multi-Master-Cluster bietet zusätzliche Vorteile wie:
*   Maximale Schlüsselverfügbarkeit durch Angabe mehrerer Oracle Key Vault-Knoten, aus denen Daten abgerufen werden können
*   Keine Endpunktausfallzeit bei Oracle Key Vault-Clusterwartung mit mehreren Mastern

## Sie möchten mehr erfahren?

Technische Dokumentation:

*   [Oracle Key Vault 21](https://docs.oracle.com/en/database/oracle/key-vault/21.3/index.html)
*   [Oracle Key Vault 21 - Multimaster](https://docs.oracle.com/en/database/oracle/key-vault/21.3/okvag/multimaster_concepts.html)

Video:

*   _Einführung in Oracle Key Vault 21 (Januar 2021)_[](youtube:SfXQEwziyw4)

## Danksagungen

*   **Autor** - Hakim Loumi, PM für Datenbanksicherheit
*   **Mitwirkende** - Peter Wahl
*   **Zuletzt aktualisiert am/um** - Hakim Loumi, Datenbanksicherheit PM - November 2023