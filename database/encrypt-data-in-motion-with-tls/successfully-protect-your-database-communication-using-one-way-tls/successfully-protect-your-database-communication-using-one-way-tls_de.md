# Ihre Datenbankkommunikation erfolgreich mit 1-Wege-Transport Layer Security (TLS) schützen

## Einführung

In diesem Workshop wird die Funktionalität der Oracle Transport Layer Security-(TLS-)Netzwerkverschlüsselung vorgestellt. Es gibt dem Benutzer die Möglichkeit zu lernen, wie man diese Funktion konfiguriert, um seine Daten in Bewegung zu verschlüsseln und zu sichern.

Beschreibung: TLS ist der Branchenstandard zur Verschlüsselung von bewegten Daten. Da TLS eine einseitige oder wechselseitige Authentifizierung bereitstellt, minimiert es die Wahrscheinlichkeit einer Verletzung.

_Geschätzte Laborzeit:_ 30 Minuten

_In dieser Übung getestete Version:_ Oracle DB 19.17

### Ziele

*   Ihre Datenbankkommunikation erfolgreich mit 1-Wege-TLS schützen
*   Prüfen Sie vor der Konfiguration von TLS, ob der Netzwerkverkehr unverschlüsselt ist
*   Root-Wallet und selbstsigniertes Root-CA-Zertifikat erstellen
*   Wallet des Datenbankservers erstellen und Zertifikatanforderung erstellen
*   Datenbankzertifikat mit Root-CA-Zertifikat signieren
*   CA-Root-Zertifikat und Datenbankserverzertifikat dem Datenbank-Wallet hinzufügen
*   CA-Root-Zertifikat in Client-Truststore importieren (nur Linux, Windows)
*   Für TLS-Netzwerkverschlüsselung konfigurieren
*   Stellen Sie eine Verbindung mit TLS-Netzwerkverschlüsselung her, und prüfen Sie, ob der Datenverkehr verschlüsselt ist
*   Erstellen Sie einen neuen BS-Benutzer, und verschlüsseln Sie SQL-Traffic.
*   (Optional) Verschlüsselung deaktivieren

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten (nur _Free-Tier_ und _bezahlte Mandanten_)
    *   Übung: Umgebungssetup
    *   Übung: Umgebung initialisieren

## Aufgabe 1: Laden Sie die Datei tls.zip in das lokale Verzeichnis herunter.

1.  Öffnen Sie eine Terminalsession auf der VM **DBSec-Lab** als BS-Benutzer _oracle_, und verschieben Sie mit dem Befehl `cd` in das Verzeichnis "livelabs".
    
        <copy>cd livelabs</copy>
        
    
    **Hinweis**: Wenn Sie eine Remote-Desktopsitzung verwenden, doppelklicken Sie auf das Symbol _Terminal_ auf dem Desktop, um eine Sitzung zu starten.
    
2.  Mit dem Linux-Befehl "wget" können Sie eine gebündelte (zippte) Datei der Befehle für die Übung herunterladen.
    
        <copy>wget https://objectstorage.us-ashburn-1.oraclecloud.com/p/AjvzsV9DMydnARvKi9Y5j2sFZ2femVdGi5Ciz9r09V--QIRrorH12rLR3zrmKOeH/n/oradbclouducm/b/dbsec_rich/o/tls.zip</copy>
        
3.  Dekomprimieren Sie die heruntergeladene Datei tls.zip.
    
        <copy>unzip tls.zip</copy>
        
4.  TLS-ZIP entfernen.
    
        <copy>rm tls.zip</copy>
        
5.  Wechseln Sie in das Verzeichnis "tls".
    
        <copy>cd tls/</copy>
        
6.  Verwenden Sie den folgenden Befehl, um Ausführungsberechtigungen für die Shellskripte im TLS-Verzeichnis zu aktivieren.
    
        <copy>chmod +x *.sh</copy>
        
7.  Konvertiert Datei in Unix. Dies ist möglicherweise nicht erforderlich, schadet aber nicht den Dateien, um es auszuführen.
    
        <copy>dos2unix *</copy>
        

## Aufgabe 2: Prüfen Sie vor der Konfiguration von TLS, ob der Netzwerkverkehr unverschlüsselt ist.

1.  Verwenden Sie den folgenden Befehl, um die integrierbare Datenbank PDB1 zu pingen.
    
        <copy>tnsping pdb1</copy>
        
2.  Sehen wir uns zunächst die vorhandene PDB1-Verbindung an, um zu sehen, dass die Daten nicht in Bewegung verschlüsselt werden.
    
        <copy>./tls_is_sess_encrypt.sh pdb1</copy>
        
    
    Ihre NETWORK\_PROTOCOL sollte nicht verschlüsselt sein und "tcp" lesen
    
3.  Auf diese Weise können Sie den Datenverkehr an Port 1521 abfangen und eine pcap-Datei (Packet Capture) generieren.
    
        <copy>./tls_tcpdump_traffic.sh pdb1</copy>
        
4.  Extrahiert lesbare Daten aus der pcap-Datei. In der Ausgabe wird die E-Mail-Adresse angezeigt.
    
        <copy>./tls_tcpdump_extract.sh pdb1</copy>
        

## Aufgabe 3: Root-Wallet und selbstsigniertes Root-CA-Zertifikat erstellen.

1.  In diesem Schritt erstellt das Skript das Wallet mit `orapki`, generiert das selbstsignierte Zertifikat und exportiert das vertrauenswürdige Zertifikat zur Verwendung durch den Client-Truststore.
    
        <copy>./tls_create_rootCA_wallet.sh</copy>
        

## Aufgabe 4: Datenbankserver-Wallet erstellen und Zertifikatanforderung erstellen.

1.  Als Nächstes erstellen Sie das DB-Wallet und die Zertifikatanforderung, die von rootCA signiert werden sollen. Mit diesem Schritt wird auch das vertrauenswürdige Zertifikat rootCA in das DB-Wallet importiert.
    
        <copy>./tls_create_DB_wallet.sh</copy>
        

## Aufgabe 5: Datenbankzertifikat mit Root-CA-Zertifikat signieren.

1.  Jetzt wird das Benutzerzertifikat des DB-Servers von rootCA signiert. Dadurch wird die Gültigkeit des Zertifikats gewährleistet. Wenn es nicht von einer öffentlichen Root oder einer Zwischenstelle, einer Certificate Authority (CA), einer Root oder einer Zwischenstelle einer Organisation signiert ist, ist das Zertifikat möglicherweise nicht vertrauenswürdig.
    
        <copy>./tls_sign_DB_cert.sh</copy>
        

## Aufgabe 6: CA-Root-Zertifikat und Datenbankserverzertifikat dem Datenbank-Wallet hinzufügen.

1.  Nachdem Sie das signierte DB-Benutzerzertifikat generiert haben, importieren Sie es in das DB-Wallet. In diesem Schritt sehen Sie, dass sich das Benutzerzertifikat des DB-Servers von einem "angeforderten Zertifikat" in ein "Benutzerzertifikat" ändert.
    
        <copy>./tls_import_signed_cert.sh</copy>
        

Hinweis: Vor dem Import des signierten Benutzerzertifikats sieht die DB-Wallet-Ausgabe folgendermaßen aus:

    Requested Certificates: 
    Subject:        CN=dbsec-lab,OU=dbsecdemo,O=LiveLabs,L=Austin,ST=Texas,C=US
    User Certificates:
    Trusted Certificates: 
    Subject:        C=US,CN=ROOT
    

Nach dem Import des signierten Zertifikats sieht die DB-Wallet-Ausgabe folgendermaßen aus:

    Requested Certificates: 
    User Certificates:
    Subject:        CN=dbsec-lab,OU=dbsecdemo,O=LiveLabs,L=Austin,ST=Texas,C=US
    Trusted Certificates: 
    Subject:        C=US,CN=ROOT
    

## Aufgabe 7: CA-Root-Zertifikat in Client-Truststore importieren (nur Linux, Windows)

1.  Nachdem Sie Ihr signiertes DB-Serverbenutzerzertifikat haben, stellen Sie es in Ihrem DB-Wallet-Root-Speicherort bereit. Die DB sucht mit dem Parameter `WALLET_ROOT` nach Wallet-bezogenen Informationen, einschließlich TDE und TLS. Mit diesem Schritt wird das DB-Wallet mit dem signierten Zertifikat sowohl in das TLS-Verzeichnis `WALLET_ROOT` der PDB als auch in das Standardverzeichnis kopiert, in dem ein ORACLE-Softwareclient nach dem Wallet `/etc/ORACLE/WALLETS/<user>` suchen würde. In diesem Fall wäre es `/etc/ORACLE/WALLETS/ORACLE`, da Sie `sqlplus` als Benutzer `ORACLE` verwenden.
    
        <copy>./tls_deploy_db_wallet.sh</copy>
        

Hinweis: Um den Initialisierungsparameter WALLET\_ROOT festzulegen, muss die DB neu gestartet werden. Das Skript startet die Datenbank automatisch für Sie neu.

## Aufgabe 8: Konfiguration für TLS-Netzwerkverschlüsselung.

1.  Fügen Sie einen neuen tnsnames.ora-Eintrag für die Verbindungszeichenfolge pdb1\_tls hinzu. Dadurch wird die vorhandene pdb1-Verbindungszeichenfolge kopiert und geändert, sodass TCPS-Protokoll und Port 1522 anstelle von TCP und 1521 verwendet werden.
    
        <copy>./tls_update_tnsnames_ora.sh</copy>
        
2.  In diesem Schritt aktualisieren wir sqlnet.ora der Datenbank, sodass der Parameter SSL\_CLIENT\_AUTHENTICATION als "false" aufgenommen wird. Bei 'Falsch' kann einseitige TLS vom Client verwendet werden. Bei 'Wahr' muss gegenseitige TLS (mTLS) verwendet werden.
    
        <copy>./tls_update_sqlnet_ora.sh</copy>
        
3.  Dieser Schritt stoppt Oracle Listener und aktualisiert listener.ora so, dass es für TCPS-(TLS-)Verbindungen auf Port 1522 verfügbar ist. Nachdem Oracle Listener gestartet wurde, registriert er die vorhandene CDB und PDBs dynamisch.
    
        <copy>./tls_update_listener_ora.sh</copy>
        
4.  Stellen Sie sicher, dass Sie weiterhin tnsping verwenden können, um eine Verbindung zum unverschlüsselten Listener-Alias für PDB1 herzustellen.
    
        <copy>tnsping pdb1</copy>
        
5.  Da unser Listener jetzt für TCPS-(TLS-)Verbindungen auf Port 1522 verfügbar ist, können wir die Konnektivität mit tnsping überprüfen.
    
        <copy>tnsping pdb1_tls</copy>
        

## Aufgabe 9: Stellen Sie eine Verbindung mit TLS-Netzwerkverschlüsselung her, und prüfen Sie, ob der Traffic verschlüsselt ist.

1.  Wie bei der unverschlüsselten Verbindung zu PDB1 wird der Schritt erneut mit dem tnsnames-Alias pdb1\_tls ausgeführt. Diese Verbindung ist jetzt eine TLS-verschlüsselte Verbindung zu PDB1 auf Port 1522 anstelle von Port 1521.
    
        <copy>./tls_is_sess_encrypt.sh pdb1_tls</copy>
        
    
    Die Ausgabe sollte stattdessen "tcps" lesen.
    
2.  Auch hier werden SQL-Abfragen aus sqlplus mit tcpdump erfasst. Die Abfragen werden auf unserem Bildschirm angezeigt, aber die Paketerfassungsdatei (pcap) enthält keine unverschlüsselten E-Mails.
    
        <copy>./tls_tcpdump_traffic.sh pdb1_tls</copy>
        
3.  Wenn wir Daten aus der pcap-Datei extrahieren, werden wir keine E-Mail-Adressen mehr sehen, da der von uns erfasste Datenverkehr verschlüsselt wurde.
    
        <copy>./tls_tcpdump_extract.sh pdb1_tls</copy>
        
    
    Sie haben erfolgreich Daten in Bewegung zwischen Oracle Database und dem Oracle SQL\*Plus-Client verschlüsselt.
    

## Aufgabe 10: Neuen BS-Benutzer erstellen und SQL-Traffic verschlüsseln.

1.  Im nächsten Schritt erstellen Sie einen separaten Betriebssystembenutzer und stellen sicher, dass die Konnektivität zwischen dem neuen Benutzer und der Oracle Database ebenfalls verschlüsselt ist. Erstellen Sie den BS-Benutzer "dba\_dan".
    
        <copy>./tls_useradd_dba_dan.sh</copy>
        
2.  Installieren Sie die Oracle Instant Client- und SQL\*Plus-RPMs. Hinweis: Für diesen Schritt muss Ihre VM über Internetzugang verfügen, um zwei RPMs herunterladen zu können.
    
        <copy>./tls_install_oracle_ic.sh</copy>
        
3.  Erstellen Sie ein Verzeichnis tns\_admin und ein Verzeichnis sqlnet.ora für "dba\_dan" im Home-Verzeichnis von Dan.
    
        <copy>./tls_dba_dan_sqlnet_ora.sh</copy>
        
4.  Erstellen Sie ein Verzeichnis tns\_admin und eine Datei tnsnames.ora für "dba\_dan" im Home-Verzeichnis von Dan. Die Datei tnsnames.ora enthält nur einen Eintrag für den Alias pdb1\_tls.
    
        <copy>./tls_dba_dan_tnsnames_ora.sh</copy>
        
5.  Da Oracle Database ein selbstsigniertes Zertifikat verwendet, muss entweder "dba\_dan" aus der rootCA ein Wallet mit dem vertrauenswürdigen Zertifikat rootCA verwalten, oder das vertrauenswürdige Zertifikat rootCA muss der Liste der vertrauenswürdigen Root-Zertifikate von Linux hinzugefügt werden. In diesem Schritt fügen Sie das vertrauenswürdige Zertifikat rootCA zur Liste der vertrauenswürdigen Linux-Root-Zertifikate hinzu, sodass alle Benutzer im BS es verwenden können, um über TCPS auf Port 1522 eine Verbindung zu PDB1 herzustellen. In einer Unternehmensumgebung wäre dies die effizienteste Methode zum Verwalten interner, selbstsignierter Zertifikate für Verbindungen zu Oracle-Datenbanken, die TLS verwenden. Unter Windows installieren Sie dieses Zertifikat mit der Microsoft Management Console (MMC).
    
        <copy>./tls_install_linux_cert.sh</copy>
        

Hinweis: Die Datei rootCA.crt wurde in das Linux-Verzeichnis "/etc/pki/ca-trust/source/anchor" kopiert und in die Liste der vertrauenswürdigen Zertifikate geladen. Dieser Speicherort kann je nach Linux-Distribution variieren, die Sie in Ihrer Umgebung verwenden.

6.  Testen Sie die Konnektivität als Linux-Benutzer "dba\_dan".
    
        <copy>sudo su - dba_dan</copy>
        
7.  Der Oracle Instant Client muss wissen, wo die tns-bezogenen Parameter zu finden sind. Dies ist der Alias, pdb1\_tls, und die Verbindung SSL\_CLIENT\_AUTHENTICATION, die TLS anstelle von mTLS angibt.
    
        <copy>export TNS_ADMIN=$HOME/tns_admin</copy>
        
8.  Stellen Sie mit SQL\*Plus eine Verbindung zur DB mit dem verschlüsselten tcps-Listener her.
    
        <copy>sqlplus system/Oracle123@pdb1_tls</copy>
        
9.  Zeigen Sie an, dass das Netzwerkprotokoll "tcps" lautet.
    
        <copy>SELECT sys_context('USERENV', 'NETWORK_PROTOCOL') as network_protocol FROM dual;</copy>
        

## Aufgabe 11: (Optional) Verschlüsselung deaktivieren

1.  Beenden Sie SQL\*Plus.
    
        <copy>exit</copy>
        
2.  Beenden Sie DBA Dan zurück zum Oracle-Benutzer.
    
        <copy>exit</copy>
        
3.  Mit diesem Schritt wird die TLS-Verschlüsselung für den Listener deaktiviert, die Parameter aus der Datei sqlnet.ora und tnsnames.ora entfernt und die TLS-Wallet-Dateien gelöscht.
    
        <copy>./tls_disable.sh</copy>
        

## **Anhang**: Informationen zum Produkt

### **Überblick**

Oracle Database bietet sowohl native Datennetzwerkverschlüsselung als auch TLS-basierte Verschlüsselung, um sicherzustellen, dass Daten in Bewegung sicher sind, während sie im Netzwerk übertragen werden.

![Netzwerkverschlüsselung](./images/nne-concept.png "Netzwerkverschlüsselung")

## Weitere Informationen

Technische Dokumentation:

*   [Transport Layer Security-Authentifizierung konfigurieren](https://docs.oracle.com/en/database/oracle/oracle-database/19/dbseg/configuring-secure-sockets-layer-authentication.html)

## Danksagungen

*   **Autor** - Stephen Stuart & Alpha Diallo, Solution Engineers, North America Specialist Hub
*   **Mitwirkende** - Richard C. Evans, Produktmanager für Datenbanksicherheit
*   **Zuletzt aktualisiert am/um** - Stephen Stuart & Alpha Diallo, April 2023