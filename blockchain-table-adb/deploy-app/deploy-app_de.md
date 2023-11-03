# Erweiterte Übung - Node.js in Compute-Instanz installieren und Anwendung bereitstellen

## Einführung

In der vorherigen Übung haben wir die SSH-Schlüssel generiert, die für die Compute-Instanz erforderlich sind, die für die Anwendung node.js verwendet wird, um die kryptografische Signatur der Benutzerdaten außerhalb der Datenbank bereitzustellen.

In dieser Übung installieren wir Node.js in der Compute-Instanz. Zur Vorbereitung der Datensignatur generieren Sie dann ein Private/Public Key-Paar und ein PKI-Zertifikat in der Compute-Instanz, das Ihren Public Key enthält. Danach speichern Sie das Zertifikat in der Oracle Autonomous Transaction Processing-Datenbankinstanz und speichern die zurückgegebene Zertifikat-GUID. Stellen Sie schließlich die Anwendung bereit, und signieren Sie eine Zeile in der Datenbank aus der cloud shell.

Geschätzte Zeit: 25 Minuten

Sehen Sie sich das Video unten an, um einen kurzen Spaziergang durch das Labor zu machen.

[](youtube:2QHI3kH8H0o)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Oracle Linux-Compute-Instanz bereitstellen und SSH in der Instanz bereitstellen
*   Zertifikat generieren und Zertifikat in der Datenbank speichern
*   Installieren Sie Node.js in der Compute-Instanz
*   Offene Firewall für Ports
*   Anwendung herunterladen und bereitstellen
*   Zeile signieren und alle Zeilen in der Blockchain-Tabelle prüfen

### Voraussetzungen

Dieser Workshop geht davon aus, dass Sie:

*   LiveLabs Cloud-Account
*   Die vorherigen Übungen wurden erfolgreich abgeschlossen

## Aufgabe 1: Compute-Instanz bereitstellen

1.  Klicken Sie in der Oracle Cloud-Konsole auf das Navigationsmenü, suchen Sie nach **Compute**, und wählen Sie unter "Compute" die Option **Instanzen** aus.
    
    ![](./images/lab5-task1-1.png " ")
    
2.  Stellen Sie sicher, dass Sie sich in derselben Region und demselben Compartment wie die Oracle Autonomous Database-Instanz befinden, und klicken Sie auf **Instanz erstellen**.
    
    ![](./images/lab5-task1-2.png " ")
    
3.  Geben Sie der Instanz einen Namen. In dieser Übung verwenden wir den Namen **DEMOVM**.
    
    ![](./images/lab5-task1-3.png " ")
    
4.  Wählen Sie unter Platzierung, Bild und Form Folgendes aus:
    
    *   **Availability-Domain**: Lassen Sie für diese Übung die Standardinstanzplatzierung auf "Immer kostenlos" berechtigt, oder klicken Sie auf **Bearbeiten**, und wählen Sie eine Availability-Domain (AD) aus.
    *   **Image und Ausprägung** - Übernehmen Sie für diese Übung die Standardressource "Auswählbare Ressource vom Typ "Immer kostenlos", oder klicken Sie auf **Bearbeiten**, um das Image und die Ausprägung zu ändern.
    
    ![](./images/lab5-task1-4.png " ")
    
5.  Wählen Sie unter "SSH-Schlüssel hinzufügen" die Option **Public Keys einfügen** aus, fügen Sie den in der vorherigen Übung notierten Public Key ein, und klicken Sie auf **Erstellen**.
    
    ![](./images/lab5-task1-5.png " ")
    
6.  Ihre Instanz beginnt mit dem Provisioning. In wenigen Minuten ändert sich der Status von "Provisioning wird ausgeführt". Jetzt ist Ihre Compute-Instanz einsatzbereit! Sehen Sie sich die Details Ihrer Instanz an, und kopieren Sie die **öffentliche IP-Adresse**, um sie später zu verwenden.
    
    ![](./images/lab5-task1-61.png " ") ![](./images/lab5-task1-62.png " ")
    

## Aufgabe 2: Verbindung zur Compute-Instanz herstellen

Es gibt mehrere Möglichkeiten, eine Verbindung zu Ihrer Cloud-Instanz herzustellen. Wählen Sie aus, wie Sie eine Verbindung zu Ihrer Cloud-Instanz herstellen möchten, die dem generierten SSH-Schlüssel entspricht. \*(Wenn Sie Ihre SSH-Schlüssel in der cloud shell erstellt haben, wählen Sie cloud shell aus.)\*

*   Oracle Cloud-Shell
*   MAC oder Windows CYGWIN Emulator
*   Windows mit Putty

### Oracle Cloud-Shell

1\. Wenn Sie von der cloud Shell abgemeldet sind oder die Oracle Cloud-Shell neu starten möchten, klicken Sie in der Cloud-Konsole rechts neben der Region auf das cloud shell-Symbol. \*Hinweis: Stellen Sie sicher, dass Sie sich in der Ihnen zugewiesenen Region befinden\*

    ![](./images/lab5-task2-1.png " ")
    

2.  Ersetzen Sie im folgenden Befehl "sshkeyname" durch den tatsächlichen SSH-Schlüsselnamen aus Übung 1, und ersetzen Sie "Your Compute Instance Public IP Address" durch die Adresse, die Sie in Aufgabe 1 dieser Übung kopiert haben. Geben Sie den bearbeiteten Befehl ein, um sich bei Ihrer Instanz anzumelden.
    
        ssh -i ~/.ssh/<sshkeyname> opc@<Your Compute Instance Public IP Address>
        
    
    _Hinweis: Die spitzen Klammern <> sollten nicht in Ihrem Befehl angezeigt werden._
    
    ![](./images/lab5-task2-4.png " ")
    
3.  Wenn Sie dazu aufgefordert werden, geben Sie **Ja** ein, um mit der Verbindung fortzufahren.
    
    ![](./images/lab5-task2-5.png " ")
    
4.  Fahren Sie mit der nächsten Aufgabe im linken Menü fort.
    

### MAC oder Windows CYGWIN Emulator

1.  Gehen Sie zu **Compute**, klicken Sie auf **Instanz**, und wählen Sie die erstellte Instanz aus. (Stellen Sie sicher, dass Sie das richtige Compartment auswählen.)
    
2.  Suchen Sie auf der Instanzhomepage die öffentliche IP-Adresse für Ihre Instanz.
    
3.  Öffnen Sie als opc-Benutzer einen Terminal-(MAC-) oder Cygwin-Emulator. Geben Sie bei entsprechender Aufforderung "Ja" ein.
    
        ssh -i ~/.ssh/<sshkeyname> opc@<Your Compute Instance Public IP Address>
        
    
    ![](./images/lab5-cloudshellssh.png " ")
    
    ![](./images/lab5-cloudshelllogin.png " ")
    
    _Hinweis: Die spitzen Klammern <> sollten nicht in Ihrem Befehl angezeigt werden._
    
4.  Fahren Sie nach der erfolgreichen Anmeldung mit der nächsten Aufgabe fort.
    

### Windows mit Putty

1.  Öffnen Sie Kitt und erstellen Sie eine neue Verbindung.
    
        ssh -i ~/.ssh/<sshkeyname> opc@<Your Compute Instance Public IP Address>
        
    
    ![](./images/lab5-ssh-first-time.png " ")
    
    _Hinweis: Die spitzen Klammern <> sollten nicht in Ihrem Befehl angezeigt werden._
    
2.  Geben Sie einen Namen für die Session ein, und klicken Sie auf **Speichern**.
    
    ![](./images/lab5-putty-setup.png " ")
    
3.  Klicken Sie im linken Navigationsbereich auf **Verbindung** und dann auf **Daten**, und setzen Sie den Benutzernamen für die automatische Anmeldung auf "root".
    
4.  Klicken Sie im linken Navigationsbereich auf **Verbindung**, **SSH**, **Auth**, und konfigurieren Sie den zu verwendenden SSH-Private Key, indem Sie unter "Private Key-Datei zur Authentifizierung" auf "Durchsuchen" klicken.
    
5.  Navigieren Sie zu dem Speicherort, in dem Sie die SSH-Private-Key-Datei gespeichert haben, wählen Sie die Datei aus, und klicken Sie auf "Open". HINWEIS: Sie können keine Verbindung herstellen, während Sie sich auf VPN oder im Oracle-Büro auf Clear-Corporate befinden (wählen Sie "Clear-Internet").
    
    ![](./images/lab5-putty-auth.png " ")
    
6.  Der Dateipfad für die SSH-Private-Key-Datei wird jetzt in der Private-Key-Datei für das Authentifizierungsfeld angezeigt.
    
7.  Klicken Sie im linken Navigationsbereich auf Session und dann im Laden auf Speichern, Speichern oder Löschen einer gespeicherten Session STEP.
    
8.  Klicken Sie auf "Öffnen", um die Session mit der Instanz zu beginnen. Herzlichen Glückwunsch! Jetzt wird eine voll funktionsfähige Linux-Instanz auf Oracle Cloud Compute ausgeführt.
    

## Aufgabe 3: Zertifikat generieren

Generieren wir das Schlüsselpaar X509. Wir generieren das Schlüsselpaar und ein X509-Zertifikat, das später für die Datensignatur verwendet wird.

1.  Nachdem wir nun mit SSH eine Verbindung zur Compute-Instanz in der Oracle cloud shell hergestellt haben, laden wir die Datei nodejs.zip herunter.
    
        <copy>
        	cd ~
        wget https://objectstorage.us-ashburn-1.oraclecloud.com/p/VEKec7t0mGwBkJX92Jn0nMptuXIlEpJ5XJA-A6C9PymRgY2LhKbjWqHeB5rVBbaV/n/c4u04/b/livelabsfiles/o/data-management-library-files/blockchain/nodejs.zip
        </copy>
        
    
    ![](./images/lab5-task7-2.png " ")
    
2.  Dekomprimieren Sie die nodejs-Datei.
    
        <copy>
        unzip nodejs.zip
        </copy>
        
    
    ![](./images/lab5-task7-3.png " ")
    
3.  Navigieren Sie zum Ordner nodejs.
    
        <copy>
        cd nodejs
        </copy>
        
    
    ![](./images/lab5-task7-4.png " ")
    
4.  Führen Sie den Befehl aus, um das x509-Schlüsselpaar _user01.key_, _user01.pem_ im nodejs-Ordner zu generieren.
    
    Geben Sie bei entsprechender Aufforderung die Details für jeden Parameter ein, und drücken Sie die Eingabetaste - Ländername, Bundesland, Ortsname, Organisationsname, Allgemeiner Name, E-Mail-Adresse
    
        	<copy>
        	openssl req -x509 -sha256 -nodes -newkey rsa:4096 -keyout user01.key -days 730 -out user01.pem
        	</copy>
        
    
    ![](./images/lab5-task7-5.png " ")
    
5.  Listen Sie die Dateien auf, und beachten Sie, dass das Schlüsselpaar _user01.key_ und _user01.pem_ erstellt wurde.
    
        <copy>ls</copy>
        
    
    ![](./images/lab5-task7-6.png " ")
    
6.  `cat` den Schlüssel _user01.pem_.
    
        	<copy>cat user01.pem</copy>
        
    
    ![](./images/lab5-task7-7.png " ")
    

## Aufgabe 4: Zertifikat in der Datenbank speichern

1.  Navigieren Sie zur Registerkarte mit SQL Developer Web in Ihrem Browser, kopieren Sie die folgende Prozedur, und fügen Sie sie in SQL Worksheet ein. Führen Sie die Prozedur noch nicht aus.
    
        	<copy>
        	set serveroutput on
        	DECLARE
        		amount NUMBER := 32767;
        		cert_guid RAW(16);
        		cert clob := '-----BEGIN CERTIFICATE----MIIFcjCCA1oCCQC+Rsk9wAYlzDANBgkqhkiG9w0BAQsFADB7MQswCQYDVQQGEwJV
        		………
        		-----END CERTIFICATE-----';
        	BEGIN
        
        		DBMS_USER_CERTS.ADD_CERTIFICATE(
          		utl_raw.cast_to_raw(cert), cert_guid);
        		DBMS_OUTPUT.PUT_LINE('Certificate GUID = ' || cert_guid);
        	END;
        	/
        	</copy>
        
    
    Kopieren Sie in der vorherigen Registerkarte in der Oracle cloud shell den PEM-Schlüssel, und ersetzen Sie "-----BEGIN CERTIFICATE----MIIFcjCCA1oCCQC+Rsk9wAYlzDAN.........-----END CERTIFICATE-----" durch den PEM-Schlüssel, der im SQL Developer Web-Arbeitsblatt kopiert wurde. Stellen Sie sicher, dass sich der PEM-Schlüssel innerhalb der Apostrophe befindet, und führen Sie den Vorgang aus.
    
    Das Verfahren sollte wie folgt aussehen
    
        	set serveroutput on
        	DECLARE
        		amount NUMBER := 32767;
        		cert_guid RAW(16);
        		cert clob := '-----BEGIN CERTIFICATE-----
        	MIIFlTCCA32gAwIBAgIJAPyKGld/4jwSMA0GCSqGSIb3DQEBCwUAMGExCzAJBgNV
        	BAYTAlVTMQswCQYDVQQIDAJOSjELMAkGA1UEBwwCTEExCzAJBgNVBAoMAlRBMQsw
        	CQYDVQQLDAJQQTELMAkGA1UEAwwCSEExETAPBgkqhkiG9w0BCQEWAkFKMB4XDTIx
        	MDcxNDAxNTcwM1oXDTIzMDcxNDAxNTcwM1owYTELMAkGA1UEBhMCVVMxCzAJBgNV
        	BAgMAk5KMQswCQYDVQQHDAJMQTELMAkGA1UECgwCVEExCzAJBgNVBAsMAlBBMQsw
        	CQYDVQQDDAJIQTERMA8GCSqGSIb3DQEJARYCQUowggIiMA0GCSqGSIb3DQEBAQUA
        	A4ICDwAwggIKAoICAQDAlBMNqLDDprxCCFACf2v3oKaFmes1uSc0WfFPfblNDn7K
        	kvvNYIAkcAxCsv6fvt/xg1ixpDEokwFMm9mf2L8uYZiqx7TnwOsWOABRrkMpnlQ5
        	bVIiFnukb2hxrnehrM/PEkhCxTTXFkDHneNQVkrekYuETpLXK3t06+1eQCGRugZ4
        	q0vcpAES3eNoSf3YS9aXqzcF8zp/qe71QFqdI0CoCUCJ5LN/7sCL+5hzZ80kiC9p
        	1N7AR+LpYURSnFnYSeIk8pSCKx3u2oxRAmrhF+VrLGFUsM4D9uW+pTHQz4PN+VUs
        	ylQati7pH9HRZ7NoGiBJWdRsUkRpS6ylwXzNCl1HmHWU7NbR5IPCuBbrKUfIK9iy
        	mcUQECAHGV+M8hN2obE/MZFdySDpPt37Y7Z/B89GA7As6hUVpX7jUtl4oQhWDVCu
        	6Ah40RvrAVmMI7knhv78+ZFrOlBTyVLxFxazNAzpSmAGQtKmdb68YJetBEB96eto
        	hn4c9HCoUApQDT2AR98qWyQMd9gXQadsd0GmR2RgKtplaRdqVMZBaec1/59reyWT
        	qfohfKpBJbXMLGD1pmkAFwtUiHHXm8NBBgjQNN92U3URVKEy6FEXyvzP2agnIvH4
        	QvzDWPRzyoY2vzn3b7rWX3Srvk3EHCI1+ryYmfJsSKXrrvnDJja+2tpxL9IrxwID
        	AQABo1AwTjAdBgNVHQ4EFgQUCKHo9yn9x3hp8hrl2HauGEzxJYQwHwYDVR0jBBgw
        	FoAUCKHo9yn9x3hp8hrl2HauGEzxJYQwDAYDVR0TBAUwAwEB/zANBgkqhkiG9w0B
        	AQsFAAOCAgEAfK7+UjY9XKvY1GpTMBi57SHc6QWhVZRhdtvd1ak4vBgwrqqmkV3U
        	Uv7IGbG0uqGG1s00I3I8RJbQl5ebTUhdxtuo1XQUQ4Uz9InoUikVSsTWwylaS05d
        	0YwL6i/D6A66Z9oMPxosDHkJLHL6DfyDq2SH4GCzynXx/G2B2uu3Id7jCOYbH4RZ
        	Fm7ftpvsiIelJO99s7r2yLI3eyAMiKCYhRLJ3308/f8TMKs7Pd8xuNzVjxY1lugC
        	u944OinKAgAiHRutwpmEyXgKacRiq8W3NA3dpCudTiRiqpIBaSBvPLyS1oIWP0O+
        	FAl+ak/9UI5K0DD8OOU4Y4pxIbS/NvlHcxG3Sxt1wsunxwV4ujEo1dHRoC9Op8Pk
        	SCpr8hf48AG1PYdufUA8kTvRdd9La6p1fL+nWJ+QuzDFDj0SG92WxQUC6gMRLzlA
        	A7HPcDOG+04AduvMPfpcpkFOtnlJz1Ln1gDUsq0WHIrlfq7whawcJhgS9V9mHOen
        	iw1H2yizZi8/d3y2WK4xJr1m7frIlEkXoemVXAJMwQLh14rdFU/kzcViZm7eQj/p
        	PPEpEcdKfSgRraSNKjT3UdyGXTImRJat/XvjMHWokZPd4Zry7NS5hCqOhZgtUGjr
        	P5ztpVj2DIAxPrrH8JOpUwvGsXOtCxoa0INzkWwckS9WImkJFy2QGfA=
        	-----END CERTIFICATE-----';
        	BEGIN
        		DBMS_USER_CERTS.ADD_CERTIFICATE(
        			utl_raw.cast_to_raw(cert), cert_guid);
        		DBMS_OUTPUT.PUT_LINE('Certificate GUID = ' || cert_guid);
        	END;
        	/
        
    
    ![](./images/lab5-task8-1.png " ")
    
2.  Kopieren Sie den Wert der Zertifikats-GUID. Sie wird nicht mehr angezeigt.
    
    > **Hinweis:** Führen Sie die Prozedur nicht erneut aus, um eine weitere Zertifikats-GUID zu generieren.
    
    Diese Ausgabe sieht folgendermaßen aus:
    
        	Certificate GUID = C8D2C1F00236AD7CE0533D11000AE2FC
        
3.  Führen Sie diesen Befehl aus, um Ihr Zertifikat anzuzeigen.
    
        	<copy>
        	SELECT * FROM USER_CERTIFICATES;
        	</copy>
        
    
    ![](./images/lab5-task8-3.png " ")
    

## Aufgabe 5: Node.js in der Compute-Instanz installieren

Jetzt wird erläutert, wie Sie Node.js in der Compute-Instanz für die Anwendung Node.js installieren, um mit den Restpunkten der autonomen Oracle-Datenbank zu interagieren.

1.  Navigieren Sie zurück zur Registerkarte mit der Oracle Cloud-Konsole. Wenn Sie von der cloud shell abgemeldet sind, klicken Sie oben rechts auf der Seite auf das cloud shell-Symbol, um die Oracle Cloud-Shell zu starten und SSH mit diesem Befehl in der Instanz auszuführen. Andernfalls können Sie mit dem Schritt fortfahren.
    
        <copy>
        ssh -i ~/.ssh/<sshkeyname> opc@<Your Compute Instance Public IP Address>
        </copy>
        
    
    ![](./images/lab5-task7-1.png " ")
    
2.  Nodejs wird als AppStream-Modul für Oracle Linux 8 veröffentlicht. Das Repository ol8\_appstream ist auf Oracle Linux 8 standardmäßig über sudo aktiviert. Dies dauert etwa eine Minute und wird sagen "Vollständig!" wenn fertig. Führen Sie den folgenden Befehl aus, um Nodejs zu aktivieren.
    
        <copy>
        sudo dnf config-manager --set-enabled ol8_appstream
        </copy>
        
    
    ![](./images/task1-2.png " ")
    
3.  Führen Sie diesen Befehl aus, um Node.js zu installieren und die Laufzeitumgebung einzurichten. Geben Sie `y` ein, wenn Sie dazu aufgefordert werden. Nach der Installation wird "Complete!" gedruckt.
    
        <copy>
        sudo dnf module install nodejs
        </copy>
        
    
    ![](./images/task1-3.png " ")
    

## Aufgabe 6: Firewall für Ports öffnen

Um über die virtuelle Maschine eine Verbindung zur Oracle Autonomous Database-Instanz herzustellen, müssen Sie Firewallports öffnen. Für die interne Firewall der Oracle linux Compute-Instanz ist standardmäßig kein Port aktiviert. Wir müssen einen Port aktivieren.

1.  Führen Sie diesen sudo-Befehl aus, um Port 8080 dauerhaft unter der öffentlichen Zone hinzuzufügen.
    
        <copy>
        sudo firewall-cmd --permanent --zone=public --add-port=8080-8080/tcp
        </copy>
        
    
    ![](./images/task2-1.png " ")
    
2.  Laden Sie die Firewall neu, um sicherzustellen, dass der Port hinzugefügt wird.
    
        <copy>
        sudo firewall-cmd --reload
        </copy>
        
    
    ![](./images/task2-2.png " ")
    
3.  Listen Sie alle Ports auf, um sicherzustellen, dass Port 8080 verfügbar ist. Wenn 8080/TCP angezeigt wird, bedeutet dies, dass die Virtual-Machine-Firewall, die standardmäßig mit Oracle linux geliefert wird, den 8080-Port im TCP-Protokoll aktiviert hat.
    
        <copy>
        sudo firewall-cmd --permanent --zone=public --list-ports
        </copy>
        
    
    ![](./images/task2-3.png " ")
    

## Aufgabe 7: Anwendung bereitstellen

Da Node.js auf der virtuellen Oracle Linux-Maschine installiert und die Ports aktiviert sind, laden wir die Anwendung herunter und stellen sie bereit.

1.  Navigieren Sie zum Ordner nodejs.
    
        <copy>
        cd nodejs
        </copy>
        
    
    ![](./images/task3-1.png " ")
    
2.  Nun ändern wir die Datei index.js mit der URL der Oracle Autonomous Transaction Processing-Datenbankinstanz und der Zertifikats-GUID. Kopieren Sie den folgenden Befehl, und fügen Sie ihn in Notepad ein. Ersetzen Sie `<paste your atp url>` im folgenden Befehl durch die URL der Oracle Autonomous Transaction Processing-Datenbank, und drücken Sie `Enter`.
    
        sed -i 's,atp-url,<paste your atp url>,g' index.js
        
    
    Der Befehl sollte folgendermaßen aussehen:
    
        sed -i 's,atp-url,https://fw8mxn5ftposwuj-demoatp.adb.us-ashburn-1.oraclecloudapps.com,g' index.js
        
3.  Kopieren Sie den geänderten Befehl, fügen Sie ihn in die cloud shell ein, und drücken Sie `Enter`. Dieser Befehl sucht in der Datei index.js nach der Zeichenfolge `atp-url` und wird durch die Oracle Autonomous Transaction Processing-Datenbank-URL ersetzt.
    
    ![](./images/task3-3.png " ")
    
4.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in Notepad ein. Ersetzen Sie `<paste your certificate guid>` im folgenden Befehl durch die zuvor angegebene Zertifikats-GUID.
    
        sed -i 's,cert-guid,<paste your certificate guid>,g' index.js
        
    
    Der Befehl sollte folgendermaßen aussehen:
    
        sed -i 's,cert-guid,C8D2C1F00236AD7CE0533D11000AE2FC,g' index.js
        
5.  Kopieren Sie den geänderten Befehl, fügen Sie ihn in die cloud shell ein, und drücken Sie `Enter`. Dieser Befehl sucht in der Datei index.js nach der Zeichenfolge `cert-guid` und wird durch die Zertifikats-GUID ersetzt.
    
    ![](./images/task3-5.png " ")
    
6.  Kopieren Sie die aktuelle Registerkarte im Browser.
    
    ![](./images/task3-6.png " ")
    
7.  Melden Sie sich erneut bei der Compute-Instanz an. Klicken Sie oben rechts auf der Seite auf das cloud shell-Symbol, um die Oracle Cloud-Shell zu starten und mit dem Befehl eine SSH-Verbindung zur Instanz herzustellen.
    
        ssh -i ~/.ssh/<sshkeyname> opc@<Your Compute Instance Public IP Address>
        
    
    ![](./images/task1-1.png " ")
    
8.  Navigieren Sie zum Ordner nodejs, und führen Sie den Befehl aus, um die Anwendung bereitzustellen. Nachdem Sie den `node bin/www`\-Befehl ausgeführt haben, wird die Anwendung Node.js ausgeführt und horcht auf Port 8080.
    
        <copy>
        cd nodejs
        node bin/www
        </copy>
        
    
    Wenn sich der Cursor im Leerlauf befindet, wird die Anwendung nodejs ausgeführt.
    
    ![](./images/task3-7.png " ")
    

## Aufgabe 8: Zeile signieren

Verwenden wir den curl-Befehl, um eine REST-Anforderung an die gerade gestartete Anwendung node.js zu senden. Dies führt dazu, dass die Anwendung:

*   Rufen Sie den Inhalt der Zeile mit DBMS\_BLOCKCHAIN\_TABLE.GET\_BYTES\_FOR\_ROW\_SIGNATURE-Prozeduren ab
*   Rufen Sie den Befehl openssl auf, um eine Signatur mit Ihrem Private Key zu generieren
*   Fügen Sie der Zeile die Signatur mit der Prozedur DBMS\_BLOCKCHAIN\_TABLE.SIGN\_ROW hinzu. Zu diesem Zeitpunkt verwendet die Datenbank Ihre Zertifikats-GUID, um das Zertifikat abzurufen, den Public Key zu extrahieren und zu prüfen, ob die mit Ihrem Private Key in der Anwendung node.js generierte Signatur basierend auf den in der Zeile vorhandenen Daten und dem Public Key in Ihrem X.509-Zertifikat gültig ist

Wenn die Signatur und die Daten mit dem Public Key verifiziert werden, wird das Signaturfeld in der Tabelle gespeichert, und die Anwendung gibt den Status 200 und eine Erfolgsmeldung zurück. Wenn dies nicht erfolgreich ist, wird der Fehlerstatus 400 zurückgegeben.

1.  Navigieren Sie zurück zum vorherigen cloud shell-Fenster, in dem die Anwendung Node.js nicht ausgeführt wird.
    
    Wenn Sie von der cloud shell abgemeldet sind, klicken Sie oben rechts auf der Seite auf das cloud shell-Symbol, um die Oracle Cloud-Shell zu starten, eine SSH-Verbindung zur Instanz herzustellen und zum nodejs-Ordner zu navigieren. Andernfalls können Sie mit dem Schritt fortfahren.
    
        <copy>
        ssh -i ~/.ssh/<sshkeyname> opc@<Your Compute Instance Public IP Address>
        cd nodejs
        </copy>
        
    
    ![](./images/lab5-task7-1.png " ")
    
2.  Ersetzen Sie die Zahl 1 für instanceId, chainId und seqId, und aktualisieren Sie sie mit den angegebenen Werten für instanceId, chainId und seqId im folgenden Befehl, und drücken Sie die Eingabetaste.
    
        curl --location --request POST 'http://localhost:8080/transactions/row' --header 'Content-Type: application/json' --data '{"instanceId":1,"chainId":1,"seqId":1}'
        
    
    Nachdem die Werte instanceId, chainId und seqId im Befehl ersetzt wurden, sollte es folgendermaßen aussehen:
    
        curl --location --request POST 'http://localhost:8080/transactions/row' --header 'Content-Type: application/json' --data '{"instanceId":1,"chainId":5,"seqId":1}'
        
3.  Beachten Sie, dass die JSON-Nachricht mit dem Status 200 und der angezeigten Meldung `Signature has been added to the row successfully` lautet. Dies bedeutet, dass die Zeile erfolgreich signiert wurde.
    
    ![](./images/task4-3.png " ")
    
4.  Navigieren Sie zur Prüfung zurück zur Registerkarte mit der Blockchain APEX-Anwendung mit der Transaktionsliste, und aktualisieren Sie die Registerkarte. Beachten Sie, dass in der Zeile mit den Werten Instanz-ID - , Ketten-ID - und Folgenummer - `IS Signed` ein grünes Häkchen angezeigt werden muss, das angibt, dass die Zeile erfolgreich signiert wurde.
    
    ![](./images/task4-4.png " ")
    
5.  Navigieren Sie zurück zu SQL Developer Web, und fragen Sie alle Spalten in der Tabelle `bank_ledger` ab, um die tatsächlichen Werte in den Signatur- und GUID-Spalten anzuzeigen.
    
    Scrollen Sie zur Ausgabe nach rechts, und beachten Sie die Werte in den Spalten orabctab\_signature$, \_signature\_alg$ und \_signature\_cert$.
    
        	<copy>
        	select bank, deposit_date, deposit_amount, ORABCTAB_INST_ID$,
        	ORABCTAB_CHAIN_ID$, ORABCTAB_SEQ_NUM$,
        	ORABCTAB_CREATION_TIME$, ORABCTAB_USER_NUMBER$,
        	ORABCTAB_HASH$, ORABCTAB_SIGNATURE$, ORABCTAB_SIGNATURE_ALG$,
        	ORABCTAB_SIGNATURE_CERT$ from bank_ledger;
        	</copy>
        
    
    ![](./images/task4-5.png " ")
    

## Aufgabe 9: Zeilen mit Signatur prüfen

1.  Kopieren Sie im SQL-Arbeitsblatt die folgende Prozedur, um die Zeilen in der Blockchain-Tabelle mit DBMS\_BLOCKCHAIN\_TABLE.VERIFY\_ROWS zu prüfen.
    
    > _Hinweis: Es wird erwartet, dass jede Blockchain-Tabelle unterschiedliche Instanz-ID-Werte aufweist. Machen Sie sich keine Sorgen, wenn in der Ausgabe nicht dieselben Werte wie im Screenshot angezeigt werden. Wenn die PL/SQL-Prozedur erfolgreich abgeschlossen wurde, wird die Blockchain-Tabelle erfolgreich verifiziert._
    
        	<copy>
        	DECLARE
        		verify_rows NUMBER;
        		instance_id NUMBER;
        	BEGIN
        		FOR instance_id IN 1 .. 4 LOOP
        			DBMS_BLOCKCHAIN_TABLE.VERIFY_ROWS('DEMOUSER','BANK_LEDGER',
        	NULL, NULL, instance_id, NULL, verify_rows);
        		DBMS_OUTPUT.PUT_LINE('Number of rows verified in instance Id '||
        	instance_id || ' = '|| verify_rows);
        		END LOOP;
        	END;
        	/
        	</copy>
        
    
    ![](./images/lab5-task6-1.png " ")
    

## Danksagungen

*   **Autor** - Mark Rakhmilevich, Anoosha Pilli
*   **Mitwirkende** - Anoosha Pilli, Salim Hlayel, Brianna Ambler, Produktmanager, Oracle Database
*   **Zuletzt aktualisiert am/um** - Marion Smith, April 2022