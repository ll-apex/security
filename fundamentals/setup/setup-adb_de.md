# Einrichtung

## Einführung

In der vorherigen Übung haben Sie eine ADB-Instanz erstellt. In dieser Übung stellen Sie über Oracle Cloud Shell eine Verbindung zur ADB-Instanz her.

Voraussichtliche Zeit: 20 Minuten

### Ziele

*   Bucket, Authentifizierungstoken und Oracle Wallet erstellen
*   ADB-Instanz laden
*   Benutzern Rollen und Berechtigungen erteilen
*   Datenbankzugangsdaten für die Benutzer erstellen

### Voraussetzungen

*   Übung: ADB bereitstellen

## Aufgabe 1: Bucket erstellen

1.  Melden Sie sich bei Oracle Cloud an, wenn Sie noch nicht angemeldet sind.
    
2.  Klicken Sie auf das Hamburger-Menü, und navigieren Sie zu **Speicher**, und klicken Sie auf **Buckets**.
    
    ![](./images/object_storage.png " ")
    
3.  Wählen Sie das Compartment aus, in dem die Verfügbarkeitsprüfung bereitgestellt wird, und klicken Sie auf **Bucket erstellen**.
    
    ![](./images/step1-3.png " ")
    
4.  Benennen Sie den Bucket **adb1**, und klicken Sie auf **Erstellen**.
    
    ![](./images/step1-4.png " ")
    
5.  Nachdem der Bucket erstellt wurde, klicken Sie auf den Bucket, und notieren Sie sich `bucket name` und `namespace`.
    
    ![](./images/step1-5.png " ")
    

## Aufgabe 2: Oracle Wallet in Cloud Shell erstellen

Es gibt mehrere Möglichkeiten, ein Oracle Wallet für ADB zu erstellen. Wir verwenden Oracle Cloud Shell, da dies nicht im Mittelpunkt dieses Workshops steht. Weitere Informationen zu Oracle Wallets und deren Erstellung über die Benutzeroberfläche finden Sie in der Übung in diesem Workshop: [Daten mit ADB analysieren - Übung 6](https://apexapps.oracle.com/pls/apex/dbpm/r/livelabs/view-workshop?p180_id=553)

1.  Melden Sie sich bei Oracle Cloud an, wenn Sie noch nicht angemeldet sind.
    
2.  Klicken Sie auf das Cloud Shell-Symbol, um Cloud Shell zu starten ![](./images/cloud-shell.png " ")
    
3.  Klicken Sie beim Starten der Cloud Shell auf das Hamburger-Menü -> **Autonomous Transaction Processing**. ![](https://oracle-livelabs.github.io/common/images/console/database-atp.png " ")
    
4.  Klicken Sie auf den **Anzeigenamen**, um zur ADB-Hauptseite zu wechseln.
    
    ![](./images/step2-4.png " ")
    
5.  Suchen und kopieren Sie die **OCID** (Oracle Cloud-ID), die Sie in wenigen Minuten benötigen.
    
    ![](./images/locate-ocid.png " ")
    
6.  Verwenden Sie autonomous\_database\_ocid, um das Oracle Wallet zu erstellen. Zur einfacheren Verwendung legen Sie das Wallet-Kennwort auf denselben Wert wie das ADB-Admin-Kennwort fest: _WElcome123##_ Hinweis: Dies ist keine empfohlene Übung und wird nur für die Zwecke dieser Übung verwendet.
    
7.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in Cloud Shell ein. Drücken Sie die Eingabetaste noch nicht.
    
        <copy>
        cd ~
        oci db autonomous-database generate-wallet --password WElcome123## --file 21c-wallet.zip --autonomous-database-id  </copy> ocid1.autonomousdatabase.oc1.iad.xxxxxxxxxxxxxxxxxxxxxx
        
    
    ![](./images/wallet.png " ")
    
8.  Klicken Sie auf "Kopieren", um die OCID aus Schritt 5 zu kopieren und die OCID der autonomen Datenbank auszufüllen, die im Ausgabeabschnitt Ihres terraforms aufgeführt ist. Stellen Sie sicher, dass ein Leerzeichen zwischen der Phrase --autonomous-database-id und der OCID vorhanden ist. Klicken Sie auf **enter**. Sei geduldig, es dauert etwa 20 Sekunden.
    
9.  Die Wallet-Datei wird in das cloud shell-Dateisystem in /home/yourtenancyname heruntergeladen.
    
10.  Geben Sie den Listenbefehl in der cloud shell unten ein, um zu prüfen, ob _21c-wallet.zip_ erstellt wurde
    
        ls
        
    
    ![](./images/21cwallet.png " ")
    

## Aufgabe 3: Authentifizierungstoken erstellen

1.  Klicken Sie auf das Personensymbol in der oberen rechten Ecke.
    
2.  **Benutzereinstellungen** auswählen
    
    ![](./images/select-user.png " ")
    
3.  Kopieren Sie den **Benutzernamen**.
    
    ![](./images/copy-username.png " ")
    
4.  Klicken Sie auf der Registerkarte **Benutzerinformationen** auf die Schaltfläche **Kopieren**, um die **OCID** des Benutzers zu kopieren.
    
    ![](./images/copy-user-ocid.png " ")
    
5.  Erstellen Sie das Authentifizierungstoken mit der Beschreibung `adb1`. Verwenden Sie dazu den folgenden Befehl, indem Sie die tatsächliche _Benutzer-OCID_ durch die unten angegebene Benutzer-ID ersetzen. _Hinweis: Wenn Sie bereits ein Authentifizierungstoken haben, wird möglicherweise ein Fehler angezeigt, wenn Sie versuchen, mehr als 2 pro Benutzer zu erstellen_
    
        <copy>
         oci iam auth-token create --description adb1 --user-id </copy> ocid1.user.oc1..axxxxxxxxxxxxxxxxxxxxxx
        
    
    ![](./images/token.png " ")
    
6.  Identifizieren Sie die Zeile in der Ausgabe, die mit **"token"** beginnt.
    
7.  Kopieren Sie den Wert für das **Token** an einem sicheren Ort. Sie benötigen ihn in den folgenden Schritten.
    

## Aufgabe 4: ADB-Instanz mit Anwendungsschemas laden

1.  Kehren Sie zur cloud shell zurück, und starten Sie die cloud shell, wenn sie noch nicht ausgeführt wird
    
2.  Führen Sie den wget-Befehl aus, um das Skript load\_21c.sh aus dem Objektspeicher herunterzuladen.
    
        <copy>
        cd $HOME
        pwd
        wget https://objectstorage.us-ashburn-1.oraclecloud.com/p/VEKec7t0mGwBkJX92Jn0nMptuXIlEpJ5XJA-A6C9PymRgY2LhKbjWqHeB5rVBbaV/n/c4u04/b/livelabsfiles/o/data-management-library-files/load-21c.sh
        chmod +x load-21c.sh
        export PATH=$PATH:/usr/lib/oracle/19.10/client64/bin
        </copy>
        
3.  Führen Sie das Load-Skript aus, das die beiden Argumente aus Ihrem Notizblock, Ihrem Admin-Kennwort und dem Namen der ATP-Instanz, weiterleitet. Mit diesem Skript werden alle Daten für Ihre Anwendung in Ihre ATP-Instanz importiert und SQL Developer Web für jedes Schema eingerichtet. Dieses Skript wird als opc-Benutzer ausgeführt. Der ATP-Name muss der Name der ADB-Instanz sein. Im folgenden Beispiel wurde _adb1_ verwendet. Die Ausführung dieses Load-Skripts dauert etwa 3 Minuten. _Hinweis: Wenn Sie einen anderen ADB-Namen verwenden, ersetzen Sie adb1 durch den Namen Ihrer ADB-Instanz_
    
        <copy> 
        ./load-21c.sh WElcome123## adb1 2>&1 > load-21c.out</copy>
        
    
    ![](./images/load21c-1.png " ")
    

## Aufgabe 5: Benutzern Rollen und Berechtigungen erteilen

1.  Gehen Sie zurück zur Autonomous Database-Homepage.
    
    ![](./images/step4-0.png " ")
    
    ![](./images/step4-1.png " ")
    
2.  Klicken Sie auf die Registerkarte **Tools**.
    
    ![](./images/step4-tools.png " ")
    
3.  Klicken Sie auf **Datenbankaktionen**.
    
    ![](./images/step4-database.png " ")
    
4.  Wählen Sie **admin** für Ihren Benutzernamen.
    
    ![](./images/step4-admin.png " ")
    
5.  Passwort: **WElcome123##**.
    
    ![](./images/step4-password.png " ")
    
6.  Wählen Sie unter "Administration" die Option **Datenbankbenutzer** aus.
    
    ![](./images/step4-databaseuser.png " ")
    
7.  Klicken Sie für den Benutzer **HR** auf **3 Punkte**, um das Menü einzublenden, und wählen Sie **Bearbeiten** aus.
    
    ![](./images/step4-edit.png " ")
    
8.  Aktivieren Sie die Schieberegler **REST aktivieren** und **Autorisierung erforderlich**.
    
    ![](./images/step4-enable-rest.png " ")
    
9.  Klicken Sie oben auf die Registerkarte **Erteilte Rollen**.
    
    ![](./images/step4-roles.png " ")
    
10.  Scrollen Sie nach unten **DWROLE**, und stellen Sie sicher, dass die Kontrollkästchen **1.** und **3.** aktiviert sind.
    
    ![](./images/step4-dwrole.png " ")
    
11.  Scrollen Sie ganz nach unten, und klicken Sie auf **Änderungen anwenden**.
    
    ![](./images/step4-apply.png " ")
    
12.  Klicken Sie in der Suchleiste auf das **X**, um alle Benutzer erneut anzuzeigen.
    
    ![](./images/step4-cancel-search.png " ")
    
13.  Wiederholen Sie die Schritte 7-12 für den Benutzer **OE**.
    
    ![](./images/step5-13a.png " ")
    
    ![](./images/step5-13b.png " ")
    
    ![](./images/step5-13c.png " ")
    
    ![](./images/step5-13d.png " ")
    
    ![](./images/step5-13e.png " ")
    
14.  Wiederholen Sie die Schritte 7-12 für den Benutzer **BERICHTEN**.
    
    ![](./images/step5-14a.png " ")
    
    ![](./images/step5-14b.png " ")
    
    ![](./images/step5-14c.png " ")
    
    ![](./images/step5-14d.png " ")
    
    ![](./images/step5-14e.png " ")
    

## Aufgabe 6: Bei SQL Developer Web anmelden

1.  Testen Sie, ob Ihre Daten geladen wurden, indem Sie sich bei SQL Developer Web anmelden.
    
2.  Wählen Sie oben links die **Hamburger-Schaltfläche**, und blenden Sie die Registerkarte **Development** ein. Wählen Sie **SQL**.
    
    ![](./images/step4-sql.png " ")
    
3.  Klicken Sie auf das **X**, um das Popup-Fenster zu schließen.
    
    ![](./images/step4-sql-x.png " ")
    
4.  Führen Sie das Code-Snippet unten aus, und prüfen Sie, ob 665 Elemente vorhanden sind.
    
        <copy>
        select count(*) from oe.order_items;
        </copy>
        
    
    ![](./images/step4-run.png " ")
    

## Aufgabe 7: Datenbankzugangsdaten für Benutzer erstellen

Um auf Daten im Objektspeicher zuzugreifen, müssen Sie Ihrem Datenbankbenutzer die Authentifizierung beim Objektspeicher mit Ihrem OCI-Objektspeicheraccount und Authentifizierungstoken ermöglichen. Dazu erstellen Sie ein privates CREDENTIAL-Objekt für Ihren Benutzer, das diese Informationen verschlüsselt in Autonomous Transaction Processing speichert. Diese Informationen können nur für Ihr Benutzerschema verwendet werden.

1.  Kopieren Sie das Code-Snippet, und fügen Sie es in das SQL Developer-Arbeitsblatt ein. Geben Sie die Zugangsdaten für den Oracle Cloud Infrastructure Object Storage-Service an, indem Sie `<username>` und `<token>` durch den folgenden Benutzernamen und das folgende Kennwort ersetzen:
    
    *   Zugangsdatenname: Beschreibung des Authentifizierungstokens. In diesem Beispiel wird das Authentifizierungstoken mit der Beschreibung `adb1` aus Schritt 1 erstellt.
    *   Benutzername: Der Benutzername entspricht dem **OCI-Benutzernamen**, den Sie in Schritt 3 notiert haben
    *   Kennwort: Das Kennwort ist das OCI Object Store-Authentifizierungs-**Token**, das Sie in Schritt 3 generiert haben.
    
        	<copy>
        	BEGIN
        		DBMS_CLOUD.CREATE_CREDENTIAL(
        		credential_name => 'adb1',
        		username => '<username>',
        		password => '<token>'
        		);
        	END;
        	/
        	</copy>
        
    
    ![](./images/step7-1.png " ")
    
    Jetzt können Sie Daten aus dem Objektspeicher laden.
    
2.  Klicken Sie auf den Abwärtspfeil neben dem Wort **ADMIN** und **Abmelden**.
    
    ![](./images/step4-signout.png " ")
    

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Danksagungen

*   **Autoren** - Kay Malcolm, Senior Director, Database Product Management
*   **Mitwirkende** - Anoosha Pilli, Didi Han, Datenbankproduktmanagement
*   **Zuletzt aktualisiert am/um** - Didi Han, April 2021