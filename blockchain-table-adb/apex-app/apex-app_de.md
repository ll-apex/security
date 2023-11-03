# Webanwendung importieren und Blockchain-Tabellenfeatures testen

## Einführung

In der Übung erstellen Sie einen APEX-Workspace, definieren die restlichen Endpunkte und aktivieren ORDS für den Workspace. Importieren Sie anschließend die APEX-Anwendung, und führen Sie die Anwendung aus, um die Blockchain-Funktionalität zu testen.

Geschätzte Zeit: 20 Minuten

Sehen Sie sich das Video unten an, um einen kurzen Spaziergang durch das Labor zu machen.

[](youtube:0vNYCULNhnI)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   APEX- Workspace erstellen
*   REST-Endpunkte für den Workspace definieren
*   APEX-Anwendung importieren
*   APEX-Anwendung ausführen und Blockchain-Funktionalität testen

### Voraussetzungen

Dieser Workshop geht davon aus, dass Sie:

*   LiveLabs Cloud-Account
*   Die vorherigen Übungen wurden erfolgreich abgeschlossen

## Aufgabe 1: APEX-Workspace erstellen

1.  Navigieren Sie zurück zur Registerkarte mit der Oracle Cloud-Konsole. Klicken Sie auf das Navigationsmenü, suchen Sie nach **Oracle Database**, und klicken Sie auf **Autonomous Transaction Processing**.
    
    ![](./images/task1-1.png " ")
    
2.  Klicken Sie auf den Anzeigenamen der Oracle Autonomous Database-Instanz, um zur Detailseite der Oracle Autonomous Database-Instanz zu navigieren. Klicken Sie in dieser Übung auf die bereitgestellte **DEMOATP**\-Instanz.
    
    ![](./images/task1-2.png " ")
    
3.  In Ihrer Datenbank ist APEX noch nicht konfiguriert. Wenn Sie also das erste Mal auf APEX zugreifen, müssen Sie sich als APEX-Instanzadministrator anmelden, um einen Workspace zu erstellen.
    
    Klicken Sie auf die Registerkarte **Tools**. Klicken Sie auf **APEX öffnen**. ![](./images/task1-3.png " ")
    
4.  Geben Sie das Kennwort für die Administration Services ein, und klicken Sie auf **Bei Administration anmelden**. Das Kennwort entspricht dem Kennwort, das beim Erstellen der Oracle Autonomous Transaction Processing-Instanz für den ADMIN-Benutzer eingegeben wurde.
    
    Geben Sie in der Übung das **Kennwort - _WElcome123##_** für den ADMIN-Benutzer an, den Sie beim Provisioning der Oracle Autonomous Database-Instanz erstellt haben, und klicken Sie auf **Bei Administration anmelden**, um sich bei APEX Workspace anzumelden. ![](./images/task1-4.png " ")
    
5.  Klicken Sie auf die Schaltfläche "Create Workspace".
    

![](images/task1-5.png)

4.  "Vorhandenes Schema" auswählen

![](images/task1-6.png)

5.  Klicken Sie im Menü "Datenbankbenutzer" auf

![](images/task1-7.png)

6.  Suchen Sie nach dem in Lab1 erstellten **DEMOUSER**.

![](images/task1-8.png)

7.  Geben Sie das **Kennwort - _WElcome123##_** ein, das für DEMOUSER in Lab1 eingerichtet wurde

![](images/task1-9.png)

7.  Auf die Konsole zugreifen

![](images/task1-10.png)

## Aufgabe 2: REST-Endpunkte definieren und Schema aktivieren

Die APEX-Anwendung interagiert mit den Blockchain-Tabellen über die REST-API über Oracle REST Data Services (ORDS). Dazu muss das DB-Schema bei ORDS registriert sein. Standardmäßig ist das Schema nicht bei ORDS registriert. Definieren Sie REST-Endpunkte für das Schema bank\_ledger, und aktivieren Sie die Verwendung des Schemas als Teil der Datensignatur für die APEX-Anwendung. Diese wird in der nächsten Aufgabe importiert.

1.  Klicken Sie auf das Dropdown-Menü **SQL Workshop**.
    
    ![](./images/task2-1.png " ")
    
2.  Wählen Sie **RESTful Services**.
    
    ![](./images/task2-2.png " ")
    
3.  Beachten Sie, dass der Schemaalias `DEMOUSER` lautet.
    
    ![](./images/task2-31.png " ")
    
    Beachten Sie auch, dass das Klicken auf Module zeigt, dass keine Module definiert sind. ![](./images/task2-32.png " ")
    
4.  Klicken Sie [hier](https://objectstorage.us-ashburn-1.oraclecloud.com/p/VEKec7t0mGwBkJX92Jn0nMptuXIlEpJ5XJA-A6C9PymRgY2LhKbjWqHeB5rVBbaV/n/c4u04/b/livelabsfiles/o/data-management-library-files/blockchain/ORDS-REST-Blockchain.sql), um die Datei ORDS-REST-Blockchain.SQL herunterzuladen, die das SQL-Skript zum REST-Aktivieren dieses Schemas enthält, und um Module für die Tabelle bank\_ledger mit den entsprechenden Handlern zu erstellen.
    
5.  Importieren Sie die Module, indem Sie auf **Importieren** klicken.
    
    ![](./images/task2-5.png " ")
    
6.  Klicken Sie auf **Datei auswählen**, und laden Sie die gerade heruntergeladene ORDS-REST-Blockchain-Datei hoch. Klicken Sie auf **Importieren**.
    
    ![](./images/task2-6.png " ")
    
7.  Blenden Sie die Registerkarte **Module** ein, und beachten Sie, dass jetzt das Modul `blockchain` erstellt wurde.
    
    ![](./images/task2-7.png " ")
    
8.  Blenden Sie die Registerkarte des Blockchain-Moduls ein, um die Vorlagen `rowdata` und `signdata` anzuzeigen.
    
    ![](./images/task2-8.png " ")
    
9.  Blenden Sie die Registerkarte `rowdata` ein, und wählen Sie dann POST.
    
    ![](./images/task2-9.png " ")
    
10.  Beachten Sie, dass die PL/SQL-Prozedur unter dem Feld "Quelle" die Eingabeparameter seqId, instanceId und chainId annimmt und die Zeilendaten als Ausgabeantwort angibt, wenn Sie eine POST-Anforderung ausführen.
    
    ![](./images/task2-10.png " ")
    
    Zeigen Sie die PL/SQL-Prozedur rowdata an, die seqId, instanceId, chainId als Eingabeparameter verwendet und die Zeilendaten als Ausgabeantwort für die POST-Anforderung angibt:
    
        DECLARE
            row_data BLOB;
            buffer RAW(4000);
            inst_id BINARY_INTEGER;
            chain_id BINARY_INTEGER;
            sequence_no BINARY_INTEGER;
            row_len BINARY_INTEGER;
        BEGIN
            SELECT ORABCTAB_INST_ID$, ORABCTAB_CHAIN_ID$,
            ORABCTAB_SEQ_NUM$
            INTO inst_id, chain_id, sequence_no
            FROM BANK_LEDGER
            WHERE ORABCTAB_INST_ID$=:instanceId and
            ORABCTAB_CHAIN_ID$=:chainId and ORABCTAB_SEQ_NUM$=:seqId;
            DBMS_BLOCKCHAIN_TABLE.GET_BYTES_FOR_ROW_SIGNATURE('DEMOUSER','BANK_LEDGER',inst_id, chain_id, sequence_no, 1, row_data);
            row_len := DBMS_LOB.GETLENGTH(row_data);
            DBMS_LOB.READ(row_data, row_len, 1, buffer);
            :rowhex := RAWTOHEX(buffer);
            :status := 200;
        END;
        
11.  Nach Erhalt der Rowdata verwendet die Anwendung Node.js, die in der nächsten Übung installiert wird, diese Zeilendaten, um die Signatur mit dem anderen Restpunkt auszuführen - POST-Methode unter den signdata.
    
12.  Blenden Sie die Registerkarte "Zeichendaten" ein, und wählen Sie **POST** aus. Beachten Sie, dass die PL/SQL-Prozedur unter dem Feld "Quelle" die Eingabeparameter cert\_guid, chainId, instanceId und seqId zusammen mit den Zeilendaten zum Signieren der Zeile verwendet.
    
    ![](./images/task2-12.png " ")
    
    Zeigen Sie die PL/SQL-Prozedur der Zeichenzeile an, die cert\_guid,seqId, instanceId, chainId als Eingabeparameter verwendet, und ergibt `status - 200` mit einem `message - Signature has been added to the row successfully.`, wenn die Zeile erfolgreich signiert wurde. Andernfalls wird `status - 400` mit dem `message - Error adding the signature to blockchain table.` angezeigt
    
        BEGIN
            DBMS_BLOCKCHAIN_TABLE.SIGN_ROW('DEMOUSER','BANK_LEDGER', :instanceId
            , :chainId, :seqId, NULL, HEXTORAW(:signature), HEXTORAW(:cert_guid),
            DBMS_BLOCKCHAIN_TABLE.SIGN_ALGO_RSA_SHA2_256);
            :signature := :signature;
            :message := 'Signature has been added to the row successfully.';
            :status := 200;
        exception
        when others then
            :message := HEXTORAW(:signature)||'Error adding the signature to blockchain table.';
            :status := 400;
        END;
        

## Aufgabe 3: APEX-Anwendung importieren und ausführen

Jetzt haben wir das Blockchain-Modul, die Handler und die Vorlagen definiert. Wir importieren die Apex-Anwendung.

1.  Klicken Sie [hier](https://objectstorage.us-ashburn-1.oraclecloud.com/p/VEKec7t0mGwBkJX92Jn0nMptuXIlEpJ5XJA-A6C9PymRgY2LhKbjWqHeB5rVBbaV/n/c4u04/b/livelabsfiles/o/data-management-library-files/blockchain/Blockchain-APEX-Application.sql), um die Blockchain-APEX-Application.sql herunterzuladen.
    
2.  Klicken Sie auf das Dropdown-Menü **App Builder**, und wählen Sie **Importieren** aus.
    
    ![](./images/task3-2.png " ")
    
3.  Ziehen Sie auf der Importseite die Datei Blockchain-APEX-Application.sql, die Sie gerade heruntergeladen haben, per Drag-and-Drop, oder klicken Sie auf das Symbol **Drag-and-Drop**.
    
    Behalten Sie den Standarddateityp "Datenbankanwendung", "Seite" oder "Komponentenexport" bei, und klicken Sie auf **Weiter**. ![](./images/task3-3.png " ")
    
4.  Klicken Sie auf **Weiter**.
    
    ![](./images/task3-4.png " ")
    
5.  Beachten Sie, dass auf der Seite "Datenbankanwendung installieren" das Schema DEMOUSER verwendet wird. Übernehmen Sie die Standardwerte, und klicken Sie auf **Anwendung installieren**.
    
    ![](./images/task3-5.png " ")
    
6.  Klicken Sie nach der Installation der Anwendung auf **Anwendung ausführen**, um die Anwendung auszuführen.
    
    ![](./images/task3-6.png " ")
    
7.  Geben Sie auf der Anmeldeseite der Blockchain APEX-Anwendung den **Benutzernamen - DEMOUSER**, das **Kennwort - _W3lcome123##_** ein, und klicken Sie auf **Anmelden**.
    
    ![](./images/task3-7.png " ")
    

## Aufgabe 4: Blockchain-Funktionalität in APEX-Anwendung testen

1.  Klicken Sie auf **Liste der Transaktionen**.
    
    ![](./images/task4-1.png " ")
    
2.  Auf der Seite "Transaktionsliste" werden alle vorhandenen Transaktionen aus der Datenbank angezeigt. Die Transaktionen mit dem leeren Schildsymbol in der Spalte "Ist signiert" implizieren, dass die Datensätze nicht signiert sind. Da die Zeilen nicht geprüft werden, wird die Meldung `No Verification Process has been run!` angezeigt.
    
    ![](./images/task4-2.png " ")
    
3.  Klicken Sie auf die Schaltfläche **Transaktion erstellen**, um eine neue Transaktion zu erstellen. Ein Dialogfeld "Bankbuch" wird angezeigt.
    
    ![](./images/task4-3.png " ")
    
4.  Geben Sie im Dialogfeld "Bankbuch" die gewünschten Werte in die Felder "Bank", "Einzahlungsdatum" und "Einzahlungsbetrag" ein, oder geben Sie die Werte `Bank - 999`, `Deposit Date - 7/7/21` und `Deposit Amount - 1000` ein. Klicken Sie dann auf **Transaktion einfügen**, um eine neue Transaktion zu erstellen.
    
    ![](./images/task4-4.png " ")
    
5.  Beachten Sie, dass eine neue Zeile mit den angegebenen Details erstellt wird, die nicht signiert sind und in der Transaktionsliste angezeigt werden.
    
    ![](./images/task4-5.png " ")
    
6.  Um einen Datensatz zu aktualisieren, klicken Sie auf das Stiftsymbol einer Zeile Ihrer Wahl, aktualisieren einen Wert, und klicken Sie auf **Transaktion speichern**. Es wird einen Fehler auslösen.
    
    In diesem Beispiel versuchen wir, den gerade erstellten Datensatz zu bearbeiten. Ändern Sie den Wert für "Bank" in 998, und klicken Sie auf **Transaktion speichern**.
    
    ![](./images/task4-61.png " ")
    
    Beachten Sie den Fehler, und klicken Sie auf **Abbrechen**.
    
    ![](./images/task4-62.png " ")
    
7.  Um einen Datensatz zu löschen, klicken Sie auf das Stiftsymbol einer Zeile Ihrer Wahl, klicken Sie auf **Löschen**, und bestätigen Sie den Löschvorgang, indem Sie auf **OK** klicken. Es wirft einen Fehler -
    
    Versuchen wir in diesem Beispiel, denselben Datensatz zu löschen. Klicken Sie auf den Bleistift, auf **Löschen**, **OK**.
    
    ![](./images/task4-71.png " ")
    
    Beachten Sie den Fehler, und klicken Sie auf **Abbrechen**.
    
    ![](./images/task4-72.png " ")
    
8.  Klicken Sie auf die Schaltfläche **Zeilen prüfen**, und klicken Sie im Dialogfeld auf **OK**, um alle Zeilen in der Blockchain-Tabelle zu prüfen.
    
    ![](./images/task4-8.png " ")
    
9.  Nach Abschluss der Verifizierung wird in der _Zeilennachricht verifizieren_ jetzt die Gesamtanzahl der Zeilen und die Anzahl der erfolgreich verifizierten Datensätze angezeigt. Wenn die Werte für "Zeilen gesamt" und "Überprüfte Zeilen" identisch sind, werden alle Zeilen in der Tabelle erfolgreich geprüft.
    
    ![](./images/task4-9.png " ")
    
10.  Wählen Sie eine beliebige Zeile aus, notieren Sie sich die Werte für Instanz-ID, Ketten-ID und Folge-ID dieser Zeile, und speichern Sie diese erforderlichen Parameterwerte, da Sie sie bei der Abzeichnungsaufgabe in der nächsten Übung eingeben müssen.
    
    > _Hinweis: Es wird erwartet, dass jede Blockchain-Tabelle unterschiedliche Werte für Instanz-ID, Ketten-ID und Folge-ID aufweist. Bitte machen Sie sich keine Sorgen, wenn Sie nicht die gleichen Werte in Ihrer Tabelle wie in der Demo sehen._
    
    In der Demo werden wir die Zeile mit Instanz-ID - 1, Ketten-ID - 5 und Folge-ID - 1 unterzeichnen.
    
    ![](./images/task4-10.png " ")
    

## Danksagungen

*   **Autor** - Mark Rakhmilevich, Anoosha Pilli
*   **Mitwirkende** - Anoosha Pilli, Salim Hlayel, Brianna AmblerProduct Manager, Oracle Database
*   **Zuletzt aktualisiert am/um** - Marion Smith, April 2022