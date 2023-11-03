# ブロックチェーン表および行の管理

## 概要

ブロックチェーン表は、挿入操作のみが許可される追加専用表です。行の削除は、時間に基づいて禁止または制限されます。ブロックチェーン表の行は、特別な順序付けおよび連鎖アルゴリズムによって改ざん防止されます。ユーザーは、行が改ざんされていないことを確認できます。行メタデータの一部であるハッシュ値は、行の連鎖および検証に使用されます。

ブロックチェーン表を使用すると、ブロックチェーン・ネットワーク内のすべての参加者が同じタンパー・レジスタント・レジャーにアクセスできる集中型レジャー・モデルを実装できます。

一元化された元帳モデルでは、分散化された元帳ネットワークを設定するための管理オーバーヘッドが軽減され、分散化された元帳に比べるとレイテンシが比較的低くなり、開発者の生産性が向上し、市場化までの時間が短縮され、組織にとって大幅な節約になります。データベース・ユーザーは、他のデータベース・アプリケーション開発に使用するのと同じツールおよびプラクティスを引き続き使用できます。

見積時間: 30分

### 目標

この演習では、次のことを行います。

*   ブロックチェーン表の作成
*   行の挿入および削除
*   ブロックチェーン表の削除
*   ブロックチェーン表の行の妥当性のチェック

### 前提条件

*   Oracle Account
*   SSHキー
*   DBCS VMデータベースの作成
*   21cの設定

## タスク1: ブロックチェーン表の作成

1.  AUDITORユーザー、ブロックチェーン表の所有者を作成します。
    
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
        
        
2.  `AUDITOR.LEDGER_EMP`という名前のブロックチェーン表を作成します。この表は、`PDB21`の`HR.EMPLOYEES`に関する現在および過去のトランザクションの改ざん防止性のある台帳を保持します。ブロックチェーン表`AUDITOR.LEDGER_EMP`の行は削除できません。さらに、ブロックチェーン表は、31日間の非アクティブ後にのみ削除できます。
    
        
        $ <copy>sqlplus auditor@PDB21</copy>
        Copyright (c) 1982, 2020, Oracle.  All rights reserved.
        
        Enter password:
        
    
        SQL> <copy>CREATE BLOCKCHAIN TABLE ledger_emp (employee_id NUMBER, salary NUMBER);</copy>
        CREATE BLOCKCHAIN TABLE ledger_emp (employee_id NUMBER, salary NUMBER)
                                                              *
        ERROR at line 1:
        ORA-00905: missing keyword
        SQL>
        
        
    
    *   _`CREATE BLOCKCHAIN TABLE`文に追加の属性が必要であることを確認します。`NO DROP`、`NO DELETE`、`HASHING USING`および`VERSION`句は必須です。_
    
        
        SQL> <copy>CREATE BLOCKCHAIN TABLE ledger_emp (employee_id NUMBER, salary NUMBER)
                            NO DROP UNTIL 31 DAYS IDLE
                            NO DELETE LOCKED
                            HASHING USING "SHA2_512" VERSION "v1";</copy>
        Table created.
        SQL>
        
3.  該当するデータ・ディクショナリ・ビューのブロックチェーン表に設定されている属性を確認します。
    
        SQL> <copy>SELECT row_retention, row_retention_locked,
                            table_inactivity_retention, hash_algorithm  
                      FROM   user_blockchain_tables
                      WHERE  table_name='LEDGER_EMP';</copy>
        
        ROW_RETENTION ROW TABLE_INACTIVITY_RETENTION HASH_ALG
        ------------- --- -------------------------- --------
                      YES                         31 SHA2_512
        SQL>
        
4.  表の説明を表示します。
    
        SQL> <copy>DESC ledger_emp</copy>
        Name                                      Null?    Type
        ----------------------------------------- -------- ----------------------------
        EMPLOYEE_ID                                        NUMBER
        SALARY                                             NUMBER
        SQL>
        

_表示可能な列のみが説明に表示されていることに注目します。_

5.  `USER_TAB_COLS`ビューを使用して、ユーザー番号やユーザー署名などの内部情報の格納に使用されるすべての内部列名を表示します。
    
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
        

## タスク2: ブロックチェーン表への行の挿入

1.  最初の行をブロックチェーン表に挿入します。
    
        SQL> <copy>INSERT INTO ledger_emp VALUES (106,12000);</copy>
        1 row created.
        
        SQL> <copy>COMMIT;</copy>
        Commit complete.
        SQL>
        
2.  チェーンの最初の行の内部値を表示します。
    
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
    

3.  `HR`として接続し、監査アプリケーションが実行するかのように、ブロックチェーン表に行を挿入します。まず、表に対する`INSERT`権限を`HR`に付与します。
    
        SQL> <copy>GRANT insert ON ledger_emp TO hr;</copy>
        Grant succeeded.
        
        SQL>
        
4.  `HR`として接続し、新しい行を挿入します。
    
        SQL> <copy>CONNECT hr@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>INSERT INTO  auditor.ledger_emp VALUES (106,24000);</copy>
        1 row created.
        
        SQL> <copy>COMMIT;</copy>
        Commit complete.
        
        SQL>
        
5.  `AUDITOR`として接続し、ブロックチェーン表の行の内外の値を表示します。
    
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
        

_ユーザー番号が異なることを確認します。この値は、`V$SESSION.USER#`列と同じ値です。_

## タスク3: ブロックチェーン表からの行の削除

1.  `HR`が挿入した行を削除します。
    
        SQL> <copy>DELETE FROM ledger_emp WHERE ORABCTAB_USER_NUMBER$ = 119;</copy>
        DELETE FROM ledger_emp WHERE ORABCTAB_USER_NUMBER$ = 106
                  *
        ERROR at line 1:
        ORA-05715:operation not allowed on the blockchain table
        SQL>
        

_DML `DELETE`コマンドでブロックチェーン表の行を削除することはできません。`DBMS_BLOCKCHAIN_TABLE`パッケージを使用する必要があります。_

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
    

\*ブロックチェーン表の行は、`DBMS_BLOCKCHAIN_TABLE`パッケージの使用によってのみ削除でき、保存期間外の行しか削除できません。これは、行を削除せずにプロシージャが正常に完了した理由です。

インストールされているOracle Databaseリリースが20.0.0の場合、使用するプロシージャは`DBMS_BLOCKCHAIN_TABLE.DELETE_ROWS`であり、`DBMS_BLOCKCHAIN_TABLE.DELETE_EXPIRED_ROWS`ではありません。\*

2.  表を切り詰めます。
    
        SQL> <copy>TRUNCATE TABLE ledger_emp;</copy>
        TRUNCATE TABLE ledger_emp
                      *
        ERROR at line 1:
        ORA-05715: operation not allowed on the blockchain table
        SQL>
        
3.  今度は、作成後15日経過するまで行を削除できないように指定します。
    
        SQL> <copy>ALTER TABLE ledger_emp NO DELETE UNTIL 15 DAYS AFTER INSERT;</copy>
        ALTER TABLE ledger_emp NO DELETE UNTIL 15 DAYS AFTER INSERT
        *
        ERROR at line 1:
        ORA-05731: blockchain table LEDGER_EMP cannot be altered
        SQL>
        

_この属性を変更できないのはなぜですか。`NO DELETE LOCKED`属性を使用して表を作成しました。`LOCKED`句は、その後行保存を変更できないことを示します。_

## タスク4: ブロックチェーン表の削除

1.  表を削除します。
    
        SQL> <copy>DROP TABLE ledger_emp;</copy>
        DROP TABLE ledger_emp
                  *
        ERROR at line 1:
        ORA-05723: drop blockchain table LEDGER_EMP not allowed
        SQL>
        

\*エラー・メッセージが若干異なることを確認します。`TRUNCATE TABLE`コマンドからのエラー・メッセージは、ブロックチェーン表で操作できなかったことを示しています。現在のエラー・メッセージは、この`LEDGER_EMP`表に対しては`DROP TABLE`を実行できないことを示しています。

このブロックチェーン表は、非アクティブな状態で31日経過する前に削除できません。\*

2.  保存期間が短縮されるように表の動作を変更します。
    
        SQL> <copy>ALTER TABLE ledger_emp NO DROP UNTIL 1 DAYS IDLE;</copy>
        ALTER TABLE auditor.ledger_emp NO DROP UNTIL 1 DAYS IDLE
        *
        ERROR at line 1:
        ORA-05732: retention value cannot be lowered
        SQL> <copy>ALTER TABLE ledger_emp NO DROP UNTIL 40 DAYS IDLE;</copy>
        Table altered.
        
        SQL>
        

_保存値は増やすことができます。これにより、セキュリティのために保持する必要がある履歴情報は削除できなくなります。_

## タスク5: ブロックチェーン表の行の妥当性のチェック

1.  別のブロックチェーン表`AUDITOR.LEDGER_TEST`を作成します。行は、挿入されてから5日後に削除できず、行を削除できます。さらに、ブロックチェーン表は、1日間の非アクティブ後にのみ削除できます。
    
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
        
2.  `HR`として接続し、監査アプリケーションが実行するかのように、ブロックチェーン表に行を挿入します。まず、表に対する`INSERT`権限を`HR`に付与します
    
        SQL> <copy>GRANT insert ON auditor.ledger_test TO hr;</copy>
        Grant succeeded.
        
        SQL>
        
3.  `HR`として接続し、新しい行を挿入します。
    
        SQL> <copy>CONNECT hr@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>INSERT INTO auditor.ledger_test VALUES (1,'A1');</copy>
        1 row created.
        
        SQL> <copy>COMMIT;</copy>
        Commit complete.
        
        SQL>
        
4.  `AUDITOR`として接続し、挿入された行を表示します。
    
        SQL> <copy>CONNECT auditor@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM auditor.ledger_test;</copy>
                ID LA
        ---------- --
                1 A1
        
        SQL>
        
5.  行の内容がまだ有効であることを確認します。`DBMS_BLOCKCHAIN_TABLE.VERIFY_ROWS`を使用します。
    
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
        

## 確認

*   **作成者** - Donna Keesling、データベースUAチーム
*   **コントリビュータ** - David Start、Kay Malcolm、Database Product Management
*   **最終更新者/日付** - David Start、2020年12月