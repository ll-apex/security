# Blockchain-Tabellen verwalten und Zertifikats-GUID generieren

## Einführung

Blockchain-Tabellen sind Nur-Einfüge-Tabellen, die Zeilen in einer Reihe von Ketten organisieren. Vorhandene Zeilen dürfen nicht aktualisiert werden. Das Löschen von Zeilen ist aufgrund der Aufbewahrungszeit entweder unzulässig oder eingeschränkt. Zeilen in einer Blockchain-Tabelle werden manipulationssicher gemacht, indem jede eingefügte Zeile mit einem kryptografischen Hash mit der vorherigen Zeile in der Kette verkettet wird. Benutzer können prüfen, ob Zeilen nicht gelöscht oder manipuliert wurden. Blockchain-Tabellen stellen Unternehmen und Regierungen vor Herausforderungen beim Datenschutz, indem sie sich auf den Schutz von Daten vor Kriminellen, Hackern und Betrug konzentrieren. Blockchain-Tabellen bieten eine verbesserte Datensicherheit, indem sie unbefugte Änderungen oder Löschvorgänge von Daten verhindern, die wichtige Aktionen, Assets, Entitys und Dokumente aufzeichnen. Nicht autorisierte Änderungen wichtiger Datensätze können zu Vermögensverlusten, Geschäftsverlusten und möglichen rechtlichen Problemen führen. Verwenden Sie Blockchain-Tabellen, wenn die Unveränderlichkeit von Daten für Ihre Anwendungen von entscheidender Bedeutung ist und Sie ein manipulationssicheres Hauptbuch aktueller und historischer Transaktionen verwalten müssen.

Für einen verbesserten Betrugsschutz kann einer Zeile eine optionale Benutzersignatur hinzugefügt werden. Wenn Sie eine Blockchain-Tabellenzeile signieren, muss ein digitales Zertifikat verwendet werden. Bei der Überprüfung der Ketten in einer Blockchain-Tabelle benötigt die Datenbank das Zertifikat, um die Zeilensignatur zu überprüfen.

Blockchain-Tabellen können verwendet werden, um Blockchain-Anwendungen zu implementieren, bei denen die Teilnehmer verschiedene Datenbankbenutzer sind, die Oracle Database vertrauen, um eine überprüfbare, manipulationssichere Blockchain von Transaktionen aufrechtzuerhalten. Alle Teilnehmer müssen über Berechtigungen zum Einfügen von Daten in die Blockchain-Tabelle verfügen. Die Inhalte der Blockchain werden von der Anwendung definiert und verwaltet. Durch die Nutzung eines vertrauenswürdigen Providers mit überprüfbaren kryptosicheren Datenmanagementpraktiken können solche Anwendungen die verteilten Konsensanforderungen vermeiden. Dies bietet den größten Teil des Schutzes der verteilten Peer-to-Peer-Blockchains, jedoch mit viel höherem Durchsatz und geringerer Transaktionslatenz im Vergleich zu Peer-to-Peer-Blockchains unter Verwendung eines verteilten Konsenses. Blockchain-Tabellen können zusammen mit (normalen) Tabellen in Transaktionen und Abfragen verwendet werden. Datenbankbenutzer können weiterhin dieselben Tools und Übungen verwenden, die sie für die Entwicklung anderer Datenbankanwendungen verwenden würden. Weitere Informationen zu Blockchain-Tabellen finden Sie in der Dokumentation zu Oracle Database 19c oder 21c.

Diese Übung führt Sie durch die Schritte zum Erstellen einer Blockchain-Tabelle, zum Einfügen von Daten, zum Verwalten der Zeilen in der Tabelle, zum Verwalten der Blockchain-Tabelle und zum Prüfen der Zeilen in einer Blockchain-Tabelle ohne Signatur.

Geschätzte Zeit: 15 Minuten

Sehen Sie sich das Video unten an, um einen kurzen Spaziergang durch das Labor zu machen.

[](youtube:ZEyDWdQVMhQ)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Blockchain-Tabelle erstellen und Zeilen in die Blockchain-Tabelle einfügen
*   Blockchain-Tabellen und ihre internen Spalten anzeigen
*   Blockchain-Tabellen und -Zeilen in einer Blockchain-Tabelle verwalten
*   Prüfen Sie die Zeilen in einer Blockchain-Tabelle ohne Signatur

### Voraussetzungen

*   LiveLabs Cloud-Account
*   Die vorherigen Übungen wurden erfolgreich abgeschlossen

## Aufgabe 1: Blockchain-Tabelle erstellen

1.  Die `CREATE BLOCKCHAIN TABLE`\-Anweisung erfordert zusätzliche Attribute. Die Klauseln `NO DROP`, `NO DELETE`, `HASHING USING` und `VERSION` sind obligatorisch. "NO DELETE LOCKED" bedeutet, dass keine Zeilen gelöscht werden können, und "LOCKED" bedeutet, dass diese Einstellung später nicht mit ALTER TABLE geändert werden kann.
    
    Kopieren Sie die Abfrage, fügen Sie sie in das SQL Developer Web-Arbeitsblatt ein, und führen Sie die Abfrage aus, um eine Blockchain-Tabelle namens `bank_ledger` zu erstellen, die ein manipulationssicheres Hauptbuch aktueller und historischer Transaktionen mit dem Hashing-Algorithmus SHA2\_512 verwaltet. Zeilen der Blockchain-Tabelle `bank_ledger` können nie gelöscht werden. Außerdem kann die Blockchain-Tabelle erst nach 16 Tagen Inaktivität gelöscht werden.
    
        	<copy>
        	CREATE BLOCKCHAIN TABLE bank_ledger (bank VARCHAR2(128), deposit_date DATE, deposit_amount NUMBER)
        	NO DROP UNTIL 16 DAYS IDLE
        	NO DELETE LOCKED
        	HASHING USING "SHA2_512" VERSION "v1";
        	</copy>
        
    
    ![](./images/task1-1.png " ")
    
2.  Klicken Sie in der Registerkarte {\\b Navigator} auf die Schaltfläche {\\b Refresh}, um anzuzeigen, dass die Tabelle erstellt wurde.
    
    ![](./images/task1-2.png " ")
    
3.  Führen Sie die Abfrage aus, um die Blockchain-Tabelle `bank_ledger` zu beschreiben und die Spalten anzuzeigen. Beachten Sie, dass in der Beschreibung nur die sichtbaren Spalten angezeigt werden.
    
        	<copy>
        	DESC bank_ledger;
        	</copy>
        
    
    ![](./images/task1-3.png " ")
    

## Aufgabe 2: Zeilen in die Blockchain-Tabelle einfügen

1.  Kopieren Sie das Code-Snippet unten, fügen Sie es in das Arbeitsblatt ein, und führen Sie es aus, um Datensätze in die Blockchain-Tabelle `bank_ledger` einzufügen.
    
        	<copy>
        	INSERT INTO bank_ledger VALUES ('999',to_date(sysdate,'dd-mm-yyyy'),100);
        	INSERT INTO bank_ledger VALUES ('999',to_date(sysdate,'dd-mm-yyyy'),200);
        	INSERT INTO bank_ledger VALUES ('999',to_date(sysdate,'dd-mm-yyyy'),500);
        	INSERT INTO bank_ledger VALUES ('999',to_date(sysdate,'dd-mm-yyyy'),-200);
        	INSERT INTO bank_ledger VALUES ('888',to_date(sysdate,'dd-mm-yyyy'),100);
        	INSERT INTO bank_ledger VALUES ('888',to_date(sysdate,'dd-mm-yyyy'),200);
        	INSERT INTO bank_ledger VALUES ('888',to_date(sysdate,'dd-mm-yyyy'),500);
        	INSERT INTO bank_ledger VALUES ('888',to_date(sysdate,'dd-mm-yyyy'),-200);
        	commit;
        	</copy>
        
    
    ![](./images/task2-1.png " ")
    
2.  Fragen Sie die Blockchain-Tabelle `bank_ledger` ab, um die Datensätze anzuzeigen.
    
        	<copy>
        	select * from bank_ledger;
        	</copy>
        
    
    ![](./images/task2-2.png " ")
    

## Aufgabe 3: Blockchain-Tabellen und ihre internen Spalten anzeigen

1.  Führen Sie den Befehl aus, um alle Blockchain-Tabellen anzuzeigen.
    
        	<copy>
        	select * from user_blockchain_tables;
        	</copy>
        
    
    ![](./images/task3-1.png " ")
    
2.  Verwenden Sie die Ansicht `USER_TAB_COLS`, um alle internen Spaltennamen anzuzeigen, die zum Speichern interner Informationen verwendet werden, wie die Benutzernummer und die Benutzersignatur.
    
        	<copy>
        	SELECT table_name, internal_column_id "Col ID", SUBSTR(column_name,1,30) "Column Name", SUBSTR(data_type,1,30) "Data Type", data_length "Data Length"
        	FROM user_tab_cols
        	ORDER BY internal_column_id;
        	</copy>
        
    
    ![](./images/task3-2.png " ")
    
    Die zusätzlichen Spalten, die mit $ enden, werden von Oracle verwaltet, um die verkettete Sequenz, kryptografische Hash-Werte und die Benutzersignatur zu verwalten. Sie können diese Spalten in Ihre Abfragen aufnehmen, indem Sie sie explizit referenzieren.
    
3.  Fragen Sie die Blockchain-Tabelle `bank_ledger` ab, um alle Werte in der Blockchain-Tabelle einschließlich der Werte interner Spalten anzuzeigen.
    
        	<copy>
        	select bank, deposit_date, deposit_amount, ORABCTAB_INST_ID$,
        	ORABCTAB_CHAIN_ID$, ORABCTAB_SEQ_NUM$,
        	ORABCTAB_CREATION_TIME$, ORABCTAB_USER_NUMBER$,
        	ORABCTAB_HASH$, ORABCTAB_SIGNATURE$, ORABCTAB_SIGNATURE_ALG$,
        	ORABCTAB_SIGNATURE_CERT$ from bank_ledger;
        	</copy>
        
    
    ![](./images/task3-3.png " ")
    

## Aufgabe 4: Zeilen in einer Blockchain-Tabelle verwalten

Wenn Sie versuchen, die Zeilen mit Update, Delete und Truncate zu verwalten, wird der Fehler `operation not allowed on the blockchain table` angezeigt, wenn die Zeilen innerhalb des Aufbewahrungszeitraums liegen.

1.  Aktualisieren Sie einen Datensatz in der Blockchain-Tabelle `bank_ledger`, indem Sie deposit\_amount=0 festlegen.
    
        	<copy>
        	update bank_ledger set deposit_amount=0 where bank=999;
        	</copy>
        
    
    ![](./images/task4-1.png " ")
    
2.  Löschen Sie einen Datensatz in der Blockchain-Tabelle `bank_ledger`.
    
        	<copy>
        	delete from bank_ledger where bank=999;
        	</copy>
        
    
    ![](./images/task4-2.png " ")
    
3.  Die Tabelle `bank_ledger` wird abgeschnitten.
    
        	<copy>
        	truncate table bank_ledger;
        	</copy>
        
    
    ![](./images/task4-3.png " ")
    

## Aufgabe 5: Blockchain-Tabellen verwalten

Ähnlich wie bei der Verwaltung von Zeilen innerhalb des Aufbewahrungszeitraums löst die Verwaltung der Blockchain-Tabelle mit alter, drop einen Fehler aus.

1.  Löschen Sie die Tabelle `bank_ledger`. Wird erfolgreich gelöscht, wenn keine Zeile in der Tabelle vorhanden ist.
    
        	<copy>
        	drop table bank_ledger;
        	</copy>
        
    
    ![](./images/task5-1.png " ")
    
2.  Ändern Sie die Tabelle `bank_ledger`, um die Zeilen erst 20 Tage nach dem Einfügen zu löschen. Kopieren Sie die folgende Abfrage, und fügen Sie sie in das Arbeitsblatt ein. Markieren Sie die Abfrage, und führen Sie sie aus.
    
        	<copy>
        	ALTER TABLE bank_ledger NO DELETE UNTIL 20 DAYS AFTER INSERT;
        	</copy>
        
    
    ![](./images/task5-2.png " ")
    
3.  Erstellen Sie eine weitere Tabelle `bank_ledger_2`. Klicken Sie auf die Schaltfläche "Aktualisieren", um die neue Tabelle anzuzeigen.
    
        	<copy>
        	CREATE BLOCKCHAIN TABLE bank_ledger_2 (bank VARCHAR2(128), deposit_date DATE, deposit_amount NUMBER)
        	NO DROP UNTIL 16 DAYS IDLE
        	NO DELETE UNTIL 16 DAYS AFTER INSERT
        	HASHING USING "SHA2_512" VERSION "v1";
        	</copy>
        
    
    ![](./images/task5-3.png " ")
    
4.  ALTER kann verwendet werden, um die Aufbewahrungsfrist zu erhöhen, aber nicht, um sie zu reduzieren. Beispiel: "Mit NO DELETE bis 10 Tage nach Einfügen ändern" schlägt mit der Fehlermeldung "ORA-05732: Aufbewahrungswert kann nicht gesenkt werden" fehl.
    
    Ändern Sie die Tabelle `bank_ledger_2`, indem Sie angeben, dass die Zeilen erst 20 Tage nach dem Einfügen gelöscht werden können. Kopieren Sie die folgende Abfrage, und fügen Sie sie in das Arbeitsblatt ein. Markieren Sie die Abfrage, und führen Sie sie aus.
    
        	<copy>
        	ALTER TABLE bank_ledger_2 NO DELETE UNTIL 20 DAYS AFTER INSERT;
        	</copy>
        
    
    ![](./images/task5-4.png " ")
    
5.  Führen Sie den Befehl aus, um alle Blockchain-Tabellen anzuzeigen.
    
        	<copy>
        	select * from user_blockchain_tables;
        	</copy>
        
    
    ![](./images/task5-5.png " ")
    

## Aufgabe 6: Zeilen ohne Signatur prüfen

Sie können die Integrität von Blockchain-Tabellen überprüfen, indem Sie überprüfen, ob die Kettenintegrität nicht beeinträchtigt wurde. Oracle stellt die Prozedur DBMS\_BLOCKCHAIN\_TABLE.VERIFY\_ROWS bereit, die alle Zeilen in allen anwendbaren Ketten auf Integrität des HASH-Spaltenwerts und optional auf den SIGNATURE-Spaltenwert für Zeilen prüft, die im Bereich von low\_timestamp bis high\_timestamp erstellt wurden. Eine entsprechende Ausnahme wird ausgelöst, wenn die Integrität der Ketten beeinträchtigt wird.

1.  Prüfen Sie die Zeilen in der Blockchain-Tabelle mit DBMS\_BLOCKCHAIN\_TABLE.VERIFY\_ROWS.
    
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
        
    
    ![](./images/task6-1.png " ")
    

Sie können jetzt [mit der nächsten Übung fortfahren](#next).

## Weitere Informationen

*   Weitere Informationen zum Validieren einer Blockchain-Tabelle und anderer Blockchain-Tabellenprozeduren finden Sie in der [DBMS\_BLOCKCHAIN\_TABLE](https://docs.oracle.com/en/database/oracle/oracle-database/21/arpls/dbms_blockchain_table.html)\-Dokumentation.

## Danksagungen

*   **Autor** - Rayes Huang, Mark Rakhmilevich, Anoosha Pilli
*   **Mitwirkende** - Anoosha Pilli, Brianna Ambler, Produktmanagerin, Oracle Database
*   **Zuletzt aktualisiert am/um** - Marion Smith, April 2022