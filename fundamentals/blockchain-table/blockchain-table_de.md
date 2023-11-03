# Blockchain-Tabellen und -Zeilen verwalten

## Einführung

Blockchain-Tabellen sind reine Anhängen-Tabellen, in denen nur Einfügevorgänge zulässig sind. Das Löschen von Zeilen ist basierend auf der Zeit entweder unzulässig oder eingeschränkt. Zeilen in einer Blockchain-Tabelle werden durch spezielle Sequenzierungs- und Verkettungsalgorithmen manipulationssicher gemacht. Benutzer können prüfen, ob Zeilen nicht manipuliert wurden. Ein Hashwert, der Teil der Zeilenmetadaten ist, wird zum Verketten und Validieren von Zeilen verwendet.

Mit Blockchain-Tabellen können Sie ein zentralisiertes Ledger-Modell implementieren, bei dem alle Teilnehmer im Blockchainnetzwerk Zugriff auf dasselbe manipulationssichere Ledger haben.

Ein zentralisiertes Ledger-Modell reduziert den Verwaltungsaufwand für die Einrichtung eines dezentralen Ledger-Netzwerks, führt zu einer relativ geringeren Latenz im Vergleich zu dezentralen Ledgern, steigert die Entwicklerproduktivität, verkürzt die Time-to-Market und führt zu erheblichen Einsparungen für das Unternehmen. Datenbankbenutzer können weiterhin dieselben Tools und Übungen verwenden, die sie für die Entwicklung anderer Datenbankanwendungen verwenden würden.

Geschätzte Zeit: 30 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Blockchain-Tabelle erstellen
*   Zeilen einfügen und löschen
*   Blockchain-Tabelle löschen
*   Prüfen Sie die Gültigkeit von Zeilen in der Blockchain-Tabelle

### Voraussetzungen

*   Oracle-Account
*   SSH-Schlüssel
*   DBCS-VM-Datenbank erstellen
*   21c einrichten

## Aufgabe 1: Erstellen der Blockchain-Tabelle

1.  Erstellen Sie den AUDITOR-Benutzer, Eigentümer der Blockchain-Tabelle.
    
        $ <copy>cd /home/oracle/labs/M104781GC10</copy>
        $ <copy>/home/oracle/labs/M104781GC10/setup_user.sh</copy>
        SQL> host mkdir /u01/app/oracle/admin/CDB21/tde
        mkdir: cannot create directory '/u01/app/oracle/admin/CDB21/tde': File exists
        
        SQL>
        SQL> ADMINISTER KEY MANAGEMENT SET KEYSTORE CLOSE CONTAINER=ALL ;
        ADMINISTER KEY MANAGEMENT SET KEYSTORE CLOSE CONTAINER=ALL
        *
        ERROR at line 1:
        ORA-28389: cannot close auto login wallet
        
        SQL> ADMINISTER KEY MANAGEMENT SET KEYSTORE CLOSE IDENTIFIED BY <i>WElcome123##</i> CONTAINER=ALL;
        keystore altered.
        
        SQL> ALTER SYSTEM SET wallet_root = '/u01/app/oracle/admin/CDB21/tde'
          2         SCOPE=SPFILE;
        System altered.
        
        ...
        
        specify password for HR as parameter 1:
        specify default tablespeace for HR as parameter 2:
        specify temporary tablespace for HR as parameter 3:
        specify log path as parameter 4:
        PL/SQL procedure successfully completed.
        
        ...
        
        SQL> Disconnected from Oracle Database 21c Enterprise Edition Release 21.0.0.0.0 - Production
        Version 21.2.0.0.0
        
        SQL*Plus: Release 21.0.0.0.0 - Production on Mon Mar 9 05:34:16 2020
        Version 21.2.0.0.0
        
        Copyright (c) 1982, 2020, Oracle.  All rights reserved.
        Connected to:
        Oracle Database 21c Enterprise Edition Release 21.0.0.0.0 - Production
        Version 21.2.0.0.0
        
        SQL> DROP USER auditor CASCADE;
        DROP USER auditor CASCADE
                  *
        
        ERROR at line 1:
        ORA-01918: user 'AUDITOR' does not exist
        SQL> ALTER SYSTEM SET db_create_file_dest='/home/oracle/labs';
        System altered.
        
        SQL>
        SQL> DROP TABLESPACE ledgertbs INCLUDING CONTENTS AND DATAFILES cascade constraints;
        DROP TABLESPACE ledgertbs INCLUDING CONTENTS AND DATAFILES cascade constraints
        *
        ERROR at line 1:
        ORA-00959: tablespace 'LEDGERTBS' does not exist
        
        SQL> CREATE TABLESPACE ledgertbs;
        Tablespace created.
        
        SQL> CREATE USER auditor identified by password DEFAULT TABLESPACE ledgertbs;
        User created.
        
        SQL> GRANT create session, create table, unlimited tablespace TO auditor;
        Grant succeeded.
        
        SQL> GRANT execute ON sys.dbms_blockchain_table TO auditor;
        Grant succeeded.
        
        SQL> GRANT select ON hr.employees TO auditor;
        Grant succeeded.
        
        SQL>
        
        SQL> exit
        
        $
        
        
2.  Erstellen Sie die Blockchain-Tabelle mit dem Namen `AUDITOR.LEDGER_EMP`, die ein manipulationssicheres Ledger aktueller und historischer Transaktionen über `HR.EMPLOYEES` in `PDB21` verwaltet. Zeilen können in der Blockchain-Tabelle `AUDITOR.LEDGER_EMP` nie gelöscht werden. Außerdem kann die Blockchain-Tabelle erst nach 31 Tagen Inaktivität gelöscht werden.
    
        
        $ <copy>sqlplus auditor@PDB21</copy>
        Copyright (c) 1982, 2020, Oracle.  All rights reserved.
        
        Enter password:
        
    
        SQL> <copy>CREATE BLOCKCHAIN TABLE ledger_emp (employee_id NUMBER, salary NUMBER);</copy>
        CREATE BLOCKCHAIN TABLE ledger_emp (employee_id NUMBER, salary NUMBER)
                                                              *
        ERROR at line 1:
        ORA-00905: missing keyword
        SQL>
        
        
    
    *   _Beachten Sie, dass die `CREATE BLOCKCHAIN TABLE`\-Anweisung zusätzliche Attribute erfordert. Die Klauseln `NO DROP`, `NO DELETE`, `HASHING USING` und `VERSION` sind obligatorisch._
    
        
        SQL> <copy>CREATE BLOCKCHAIN TABLE ledger_emp (employee_id NUMBER, salary NUMBER)
                            NO DROP UNTIL 31 DAYS IDLE
                            NO DELETE LOCKED
                            HASHING USING "SHA2_512" VERSION "v1";</copy>
        Table created.
        SQL>
        
3.  Prüfen Sie die für die Blockchain-Tabelle festgelegten Attribute in der entsprechenden Data Dictionary View.
    
        SQL> <copy>SELECT row_retention, row_retention_locked,
                            table_inactivity_retention, hash_algorithm  
                      FROM   user_blockchain_tables
                      WHERE  table_name='LEDGER_EMP';</copy>
        
        ROW_RETENTION ROW TABLE_INACTIVITY_RETENTION HASH_ALG
        ------------- --- -------------------------- --------
                      YES                         31 SHA2_512
        SQL>
        
4.  Zeigen Sie die Beschreibung der Tabelle an.
    
        SQL> <copy>DESC ledger_emp</copy>
        Name                                      Null?    Type
        ----------------------------------------- -------- ----------------------------
        EMPLOYEE_ID                                        NUMBER
        SALARY                                             NUMBER
        SQL>
        

_Beachten Sie, dass in der Beschreibung nur die sichtbaren Spalten angezeigt werden._

5.  Verwenden Sie die Ansicht `USER_TAB_COLS`, um alle internen Spaltennamen anzuzeigen, die zum Speichern interner Informationen verwendet werden, wie die Benutzernummer und die Benutzersignatur.
    
        SQL> <copy>COL "Data Length" FORMAT 9999</copy>
        SQL> <copy>COL "Column Name" FORMAT A24</copy>
        SQL> <copy>COL "Data Type" FORMAT A28</copy>
        SQL> <copy>SELECT internal_column_id "Col ID", SUBSTR(column_name,1,30) "Column Name",
                            SUBSTR(data_type,1,30) "Data Type", data_length "Data Length"
                      FROM   user_tab_cols       
                      WHERE  table_name = 'LEDGER_EMP' ORDER BY internal_column_id;</copy>
        
            Col ID Column Name              Data Type                    Data Length
        ---------- ------------------------ ---------------------------- -----------
                1 EMPLOYEE_ID              NUMBER                                22
                2 SALARY                   NUMBER                                22
                3 ORABCTAB_INST_ID$        NUMBER                                22
                4 ORABCTAB_CHAIN_ID$       NUMBER                                22
                5 ORABCTAB_SEQ_NUM$        NUMBER                                22
                6 ORABCTAB_CREATION_TIME$  TIMESTAMP(6) WITH TIME ZONE           13
                7 ORABCTAB_USER_NUMBER$    NUMBER                                22
                8 ORABCTAB_HASH$           RAW                                 2000
                9 ORABCTAB_SIGNATURE$      RAW                                 2000
                10 ORABCTAB_SIGNATURE_ALG$  NUMBER                                22
                11 ORABCTAB_SIGNATURE_CERT$ RAW                                   16
                12 ORABCTAB_SPARE$          RAW                                 2000
        12 rows selected.
        SQL>
        

## Aufgabe 2: Zeilen in die Blockchain-Tabelle einfügen

1.  Fügen Sie eine erste Zeile in die Blockchain-Tabelle ein.
    
        SQL> <copy>INSERT INTO ledger_emp VALUES (106,12000);</copy>
        1 row created.
        
        SQL> <copy>COMMIT;</copy>
        Commit complete.
        SQL>
        
2.  Zeigen Sie die internen Werte der ersten Zeile der Kette an.
    
        SQL> <copy>COL "Chain date" FORMAT A17</copy>
        SQL> <copy>COL "Chain ID" FORMAT 99999999</copy>
        SQL> <copy>COL "Seq Num" FORMAT 99999999</copy>
        SQL> <copy>COL "User Num" FORMAT 9999999</copy>
        SQL> <copy>COL "Chain HASH" FORMAT 99999999999999</copy>
        SQL> <copy>SELECT ORABCTAB_CHAIN_ID$ "Chain ID", ORABCTAB_SEQ_NUM$ "Seq Num",
                    to_char(ORABCTAB_CREATION_TIME$,'dd-Mon-YYYY hh-mi') "Chain date",
                    ORABCTAB_USER_NUMBER$ "User Num", ORABCTAB_HASH$ "Chain HASH"
            FROM   ledger_emp;</copy>
        
        Chain ID   Seq Num Chain date        User Num
        --------- --------- ----------------- --------
        Chain HASH
        --------------------------------------------------------------------------------
              14         1 06-Apr-2020 12-26      119
        

5812238B734B019EE553FF8A7FF573A14CFA1076AB312517047368D600984CFAB001FA1FF2C98B139AB03DDCCF8F6C14ADF16FFD678756572F102D43420E69B3

    SQL>
    ```
    

3.  Stellen Sie eine Verbindung als `HR` her, und fügen Sie eine Zeile in die Blockchain-Tabelle ein, als ob die Auditinganwendung dies tun würde. Erteilen Sie zunächst `HR` die Berechtigung `INSERT` für die Tabelle.
    
        SQL> <copy>GRANT insert ON ledger_emp TO hr;</copy>
        Grant succeeded.
        
        SQL>
        
4.  Melden Sie sich als `HR` an, und fügen Sie eine neue Zeile ein.
    
        SQL> <copy>CONNECT hr@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>INSERT INTO  auditor.ledger_emp VALUES (106,24000);</copy>
        1 row created.
        
        SQL> <copy>COMMIT;</copy>
        Commit complete.
        
        SQL>
        
5.  Melden Sie sich als `AUDITOR` an, und zeigen Sie die internen und externen Werte der Blockchain-Tabellenzeilen an.
    
        SQL> <copy>CONNECT auditor@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT ORABCTAB_CHAIN_ID$ "Chain ID", ORABCTAB_SEQ_NUM$ "Seq Num",
                      to_char(ORABCTAB_CREATION_TIME$,'dd-Mon-YYYY hh-mi') "Chain date",
                      ORABCTAB_USER_NUMBER$ "User Num", ORABCTAB_HASH$ "Chain HASH",
                      employee_id, salary
                FROM   ledger_emp;</copy>
        
        Chain ID   Seq Num Chain date        User Num
        --------- --------- ----------------- --------
        Chain HASH
        --------------------------------------------------------------------------------
        EMPLOYEE_ID     SALARY
        ----------- ----------
              14         1 06-Apr-2020 12-26      119
        
        5812238B734B019EE553FF8A7FF573A14CFA1076AB312517047368D600984CFAB001FA1FF2C98B13
        9AB03DDCCF8F6C14ADF16FFD678756572F102D43420E69B3
        
                106      12000    14         2 06-Apr-2020 12-28      118
        
        BBCDACC41B489DFBD8E28244841411937BD716F987BE750146572C555311E377D6DBA28D392C61E7
        D75BA47BFCB3A2F4920A2C149409E89FBA63E10549DF4F47
                106      24000
        
        SQL>
        

_Beachten Sie, dass die Benutzernummer unterschiedlich ist. Dieser Wert entspricht dem Wert der Spalte `V$SESSION.USER#`._

## Aufgabe 3: Zeilen aus der Blockchain-Tabelle löschen

1.  Löschen Sie die von `HR` eingefügte Zeile.
    
        SQL> <copy>DELETE FROM ledger_emp WHERE ORABCTAB_USER_NUMBER$ = 119;</copy>
        DELETE FROM ledger_emp WHERE ORABCTAB_USER_NUMBER$ = 106
                  *
        ERROR at line 1:
        ORA-05715:operation not allowed on the blockchain table
        SQL>
        

_Sie können keine Zeilen in einer Blockchain-Tabelle mit dem DML-Befehl `DELETE` löschen. Sie müssen das Package `DBMS_BLOCKCHAIN_TABLE` verwenden._

    ```
    SQL> <copy>SET SERVEROUTPUT ON</copy>
    
    SQL> <copy>DECLARE
      NUMBER_ROWS NUMBER;
    BEGIN
      DBMS_BLOCKCHAIN_TABLE.DELETE_EXPIRED_ROWS('AUDITOR','LEDGER_EMP', null, NUMBER_ROWS);
      DBMS_OUTPUT.PUT_LINE('Number of rows deleted=' || NUMBER_ROWS);
    END;
    /</copy>    2    3    4    5    6    7
    
    Number of rows deleted=0
    PL/SQL procedure successfully completed.
    SQL>
    ```
    

\*Sie können Zeilen in einer Blockchain-Tabelle nur mit dem Package `DBMS_BLOCKCHAIN_TABLE` und nur Zeilen löschen, die außerhalb des Aufbewahrungszeitraums liegen. Aus diesem Grund wird die Prozedur erfolgreich abgeschlossen, ohne eine Zeile zu löschen.

Wenn das installierte Oracle Database-Release 20.0.0 ist, lautet die zu verwendende Prozedur `DBMS_BLOCKCHAIN_TABLE.DELETE_ROWS` und nicht `DBMS_BLOCKCHAIN_TABLE.DELETE_EXPIRED_ROWS`.\*

2.  Schneiden Sie die Tabelle ab.
    
        SQL> <copy>TRUNCATE TABLE ledger_emp;</copy>
        TRUNCATE TABLE ledger_emp
                      *
        ERROR at line 1:
        ORA-05715: operation not allowed on the blockchain table
        SQL>
        
3.  Geben Sie jetzt an, dass Zeilen erst 15 Tage nach ihrer Erstellung gelöscht werden können.
    
        SQL> <copy>ALTER TABLE ledger_emp NO DELETE UNTIL 15 DAYS AFTER INSERT;</copy>
        ALTER TABLE ledger_emp NO DELETE UNTIL 15 DAYS AFTER INSERT
        *
        ERROR at line 1:
        ORA-05731: blockchain table LEDGER_EMP cannot be altered
        SQL>
        

_Warum können Sie dieses Attribut nicht ändern? Sie haben die Tabelle mit dem Attribut `NO DELETE LOCKED` erstellt. Die `LOCKED`\-Klausel gibt an, dass Sie die Zeilenaufbewahrung nie nachträglich ändern können._

## Aufgabe 4: Löschen der Blockchain-Tabelle

1.  Löschen Sie die Tabelle.
    
        SQL> <copy>DROP TABLE ledger_emp;</copy>
        DROP TABLE ledger_emp
                  *
        ERROR at line 1:
        ORA-05723: drop blockchain table LEDGER_EMP not allowed
        SQL>
        

\*Beachten Sie, dass die Fehlermeldung etwas anders ist. Die Fehlermeldung aus dem Befehl `TRUNCATE TABLE` erklärte, dass der Vorgang in einer Blockchain-Tabelle nicht möglich war. Die aktuelle Fehlermeldung erklärt, dass `DROP TABLE` nicht möglich ist, jedoch in dieser `LEDGER_EMP`\-Tabelle.

Die Blockchain-Tabelle wurde so erstellt, dass sie nicht vor 31 Tagen Inaktivität gelöscht werden kann.\*

2.  Ändern Sie das Verhalten der Tabelle, um eine geringere Aufbewahrung zu ermöglichen.
    
        SQL> <copy>ALTER TABLE ledger_emp NO DROP UNTIL 1 DAYS IDLE;</copy>
        ALTER TABLE auditor.ledger_emp NO DROP UNTIL 1 DAYS IDLE
        *
        ERROR at line 1:
        ORA-05732: retention value cannot be lowered
        SQL> <copy>ALTER TABLE ledger_emp NO DROP UNTIL 40 DAYS IDLE;</copy>
        Table altered.
        
        SQL>
        

_Sie können den Aufbewahrungswert nur erhöhen. Dies verbietet die Möglichkeit, historische Informationen, die aus Sicherheitsgründen aufbewahrt werden müssen, zu löschen und zu entfernen._

## Aufgabe 5: Überprüfen der Gültigkeit von Zeilen in der Blockchain-Tabelle

1.  Erstellen Sie eine weitere Blockchain-Tabelle `AUDITOR.LEDGER_TEST`. Zeilen können erst 5 Tage nach dem Einfügen gelöscht werden, sodass Zeilen gelöscht werden können. Darüber hinaus kann die Blockchain-Tabelle erst nach 1 Tag Inaktivität gelöscht werden.
    
        SQL> <copy>CREATE BLOCKCHAIN TABLE auditor.ledger_test (id NUMBER, label VARCHAR2(2))
              NO DROP UNTIL 1 DAYS IDLE
              NO DELETE UNTIL 5 DAYS AFTER INSERT
              HASHING USING "SHA2_512" VERSION "v1";</copy>
        
        2    3    4  CREATE BLOCKCHAIN TABLE auditor.ledger_test (id NUMBER, label VARCHAR2(2))
        *
        ERROR at line 1:
        ORA-05741: minimum retention time too low, should be at least 16 days
        
        SQL> <copy>CREATE BLOCKCHAIN TABLE auditor.ledger_test (id NUMBER, label VARCHAR2(2))
              NO DROP UNTIL 16 DAYS IDLE
              NO DELETE UNTIL 16 DAYS AFTER INSERT
              HASHING USING "SHA2_512" VERSION "v1";</copy>
        Table created.
        
        SQL>
        
2.  Stellen Sie eine Verbindung als `HR` her, und fügen Sie eine Zeile in die Blockchain-Tabelle ein, als ob die Auditinganwendung dies tun würde. Erteilen Sie zunächst `HR` die Berechtigung `INSERT` für die Tabelle.
    
        SQL> <copy>GRANT insert ON auditor.ledger_test TO hr;</copy>
        Grant succeeded.
        
        SQL>
        
3.  Melden Sie sich als `HR` an, und fügen Sie eine neue Zeile ein.
    
        SQL> <copy>CONNECT hr@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>INSERT INTO auditor.ledger_test VALUES (1,'A1');</copy>
        1 row created.
        
        SQL> <copy>COMMIT;</copy>
        Commit complete.
        
        SQL>
        
4.  Melden Sie sich als `AUDITOR` an, und zeigen Sie die eingefügte Zeile an.
    
        SQL> <copy>CONNECT auditor@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM auditor.ledger_test;</copy>
                ID LA
        ---------- --
                1 A1
        
        SQL>
        
5.  Prüfen Sie, ob der Inhalt der Zeilen noch gültig ist. Verwenden Sie `DBMS_BLOCKCHAIN_TABLE.VERIFY_ROWS`.
    
        SQL> <copy>CONNECT auditor@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        
        Connected.
        
    
        SQL> <copy>SET SERVEROUTPUT ON</copy>
        
        SQL> <copy>DECLARE
          row_count NUMBER;
          verify_rows NUMBER;
          instance_id NUMBER;
        BEGIN
          FOR instance_id IN 1 .. 2 LOOP
            SELECT COUNT(*) INTO row_count FROM auditor.ledger_test WHERE ORABCTAB_INST_ID$=instance_id;
            DBMS_BLOCKCHAIN_TABLE.VERIFY_ROWS('AUDITOR','LEDGER_TEST', NULL, NULL, instance_id, NULL, verify_rows);
            DBMS_OUTPUT.PUT_LINE('Number of rows verified in instance Id '|| instance_id || ' = '|| row_count);
          END LOOP;
        END;
        /</copy>
        
        Number of rows verified in instance Id 1 = 1
        Number of rows verified in instance Id 2 = 0
        PL/SQL procedure successfully completed.
        SQL> <copy>EXIT</copy>
        
        $
        

## Danksagungen

*   **Autor** - Donna Keesling, Datenbank-UA-Team
*   **Mitwirkende** - David Start, Kay Malcolm, Database Product Management
*   **Zuletzt aktualisiert am/um** - David Start, Dezember 2020