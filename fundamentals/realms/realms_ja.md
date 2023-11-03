# ローカル・ユーザーによる共通操作のブロックの防止- レルム

## 概要

このラボでは、共通ユーザーがPDB内の自分のスキーマのローカル・データにアクセスできないようにするOracle Database Vaultコントロールを、ローカル・ユーザーが共通ユーザー・オブジェクトに作成できないようにする方法を示します。PDBのローカルDatabase Vault所有者は、`DVSYS`や`CTXSYS`などの共通Oracleスキーマを中心にレルムを作成し、正しく機能しないようにできます。この演習では、この機能を表示するために、`C##TEST1`カスタム・スキーマがCDBルートに作成されます。

見積時間: 40分

### 目標

この演習では、次のことを行います。

*   環境の設定

### 前提条件

*   Oracle Account
*   SSHキー
*   DBCS VMデータベースの作成
*   21cの設定

## タスク1: CDBおよびPDBレベルでのDatabase Vaultの構成および有効化

1.  CDBルート・レベルおよびPDBレベルでDatabase Vaultを構成して有効にします。このスクリプトでは、ルート・コンテナに`HR.G_EMP`表を作成し、`PDB21`に`HR.L_EMP`表を作成します。
    
        $ <copy>cd /home/oracle/labs/M104781GC10</copy>
        
        $ <copy>/home/oracle/labs/M104781GC10/setup_DV.sh</copy>
        
        $ ./setup_DV_CDB.sh
        ...
        
        SQL> ADMINISTER KEY MANAGEMENT SET KEYSTORE OPEN IDENTIFIED BY <i>WElcome123##</i> container=all;
        keystore altered.
        
        ...
        
        SQL> create user c##sec_admin identified by <i>WElcome123##</i> container=ALL;
        User created.
        
        SQL> grant create session, set container, restricted session, DV_OWNER to c##sec_admin container=ALL;
        Grant succeeded.
        
        SQL> drop user c##accts_admin cascade;
        drop user c##accts_admin cascade
                  *
        ERROR at line 1:
        ORA-01918: user 'C##ACCTS_ADMIN' does not exist
        
        SQL> create user c##accts_admin identified by <i>WElcome123##</i> container=ALL;
        User created.
        
        SQL> grant create session, set container, DV_ACCTMGR to c##accts_admin container=ALL;
        Grant succeeded.
        
        SQL> grant select on sys.dba_dv_status to c##accts_admin container=ALL;
        Grant succeeded.
        
        SQL> EXIT
        
        ...
        
        Copyright (c) 1982, 2020, Oracle.  All rights reserved.
        Last Successful login time: Tue Feb 18 2020 08:26:21 +00:00
        
        SQL> DROP TABLE g_emp;
        Table dropped.
        
        SQL> CREATE TABLE g_emp(name CHAR(10), salary NUMBER) ;
        Table created.
        
        SQL> INSERT INTO g_emp values('EMP_GLOBAL',1000);
        1 row created.
        
        SQL> COMMIT;
        Commit complete.
        
        SQL> EXIT
        Copyright (c) 1982, 2020, Oracle.  All rights reserved.
        Last Successful login time: Tue Feb 18 2020 08:27:54 +00:00
        Connected to:
        
        SQL> DROP TABLE l_emp;
        Table dropped.
        
        SQL> CREATE TABLE l_emp(name CHAR(10), salary NUMBER);
        Table created.
        
        SQL> INSERT INTO l_emp values('EMP_LOCAL',2000);
        1 row created.
        
        SQL> COMMIT;
        Commit complete.
        
        SQL> EXIT
        
        Copyright (c) 1982, 2020, Oracle.  All rights reserved.
        Last Successful login time: Tue Feb 18 2020 08:27:54 +00:00
        Connected to:
        
        SQL> DROP TABLE l_tab;
        Table dropped.
        
        SQL> CREATE TABLE l_tab(code NUMBER);
        Table created.
        
        SQL> INSERT INTO l_tab values(1);
        1 row created.
        
        SQL> INSERT INTO l_tab values(2);
        1 row created.
        
        SQL> COMMIT;
        Commit complete.
        
        SQL> EXIT
        
        $
        

## タスク2: 共通オブジェクトに対するレルムのない表データのアクセシビリティのテスト

1.  `C##SEC_ADMIN`としてCDBルートに接続し、`DV_ALLOW_COMMON_OPERATION`のステータスを確認します。これはデフォルトの動作で、ローカル・ユーザーは共通ユーザー・オブジェクトにDatabase Vaultコントロールを作成できます。
    
        $ <copy>sqlplus c##sec_admin</copy>
        
        Enter password: <i>WElcome123##</i>
        
    
        SQL> <copy>SELECT * FROM DVSYS.DBA_DV_COMMON_OPERATION_STATUS;</copy>
        
        NAME                      STATU
        ------------------------- -----
        DV_ALLOW_COMMON_OPERATION FALSE
        
        SQL>
        
2.  表の共通所有者の`C##TEST1`としてCDBルートに接続します。
    
        SQL> <copy>CONNECT c##test1</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
3.  別の共通ユーザーである`C##TEST2`としてCDBルートに接続します。
    
        SQL> <copy>CONNECT c##test2</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
4.  表の共通所有者である`C##TEST1`として`PDB21`に接続します。
    
        SQL> <copy>CONNECT c##test1@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
5.  別の共通ユーザーである`C##TEST2`として`PDB21`に接続します。
    
        SQL> <copy>CONNECT c##test2@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        

## タスク3: 共通オブジェクトに対する共通の通常または必須レルムを使用した表データのアクセシビリティのテスト

1.  CDBルートの`C##TEST1`表に共通標準レルムを作成します。
    
        SQL> <copy>CONNECT c##sec_admin</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>BEGIN
        DBMS_MACADM.CREATE_REALM(
          realm_name    => 'Root Test Realm',
          description   => 'Test Realm description',
          enabled       => DBMS_MACUTL.G_YES,
          audit_options => DBMS_MACUTL.G_REALM_AUDIT_FAIL,
          realm_type    => 0);
        END;
        /</copy>  2    3    4    5    6    7    8    9
        
        PL/SQL procedure successfully completed.
        
        SQL> <copy>BEGIN
        DBMS_MACADM.ADD_OBJECT_TO_REALM(
          realm_name   => 'Root Test Realm',
          object_owner => 'C##TEST1',
          object_name  => '%',
          object_type  => '%');
        END;
        /</copy>  2    3    4    5    6    7    8
        
        PL/SQL procedure successfully completed.
        
        SQL>
        
2.  表の共通所有者の`C##TEST1`としてCDBルートに接続します。
    
        SQL> <copy>CONNECT c##test1</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
3.  別の共通ユーザーである`C##TEST2`としてCDBルートに接続します。
    
        SQL> <copy>CONNECT c##test2</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        SELECT * FROM c##test1.g_emp
                               *
        ERROR at line 1:
        ORA-01031: insufficient privileges
        
        SQL>
        
4.  表の共通所有者である`C##TEST1`として`PDB21`に接続します。
    
        SQL> <copy>CONNECT c##test1@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
5.  別の共通ユーザーである`C##TEST2`として`PDB21`に接続します。
    
        SQL> <copy>CONNECT c##test2@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
6.  レルムを削除します。
    
        SQL> <copy>CONNECT c##sec_admin</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>EXEC DBMS_MACADM.DELETE_REALM_CASCADE('Root Test Realm')</copy>
        
        PL/SQL procedure successfully completed.
        
        SQL>
        
7.  CDBルートの`C##TEST1`表に共通必須レルムを作成します。
    
        SQL> <copy>BEGIN
        DBMS_MACADM.CREATE_REALM(
        realm_name    => 'Root Test Realm',
        description   => 'Test Realm description',
        enabled       => DBMS_MACUTL.G_YES,
        audit_options => DBMS_MACUTL.G_REALM_AUDIT_FAIL,
        realm_type    => 1);
        END;
        /</copy>  2    3    4    5    6    7    8    9
        
        PL/SQL procedure successfully completed.
        
    
        SQL> <copy>BEGIN
        DBMS_MACADM.ADD_OBJECT_TO_REALM(
        realm_name   => 'Root Test Realm',
        object_owner => 'C##TEST1',
        object_name  => '%',
        object_type  => '%');
        END;
        /</copy>  2    3    4    5    6    7    8
        
        PL/SQL procedure successfully completed.
        
        SQL>
        
8.  表の共通所有者の`C##TEST1`としてCDBルートに接続します。
    
        SQL> <copy>CONNECT c##test1</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        SELECT * FROM c##test1.g_emp
                               *
        ERROR at line 1:
        ORA-01031: insufficient privileges
        
        
        SQL>
        
9.  別の共通ユーザーである`C##TEST2`としてCDBルートに接続します。
    
        SQL> <copy>CONNECT c##test2</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        SELECT * FROM c##test1.g_emp
                               *
        ERROR at line 1:
        ORA-01031: insufficient privileges
        
        
        SQL>
        
10.  表の共通所有者である`C##TEST1`として`PDB21`に接続します。
    
        SQL> <copy>CONNECT c##test1@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
11.  別の共通ユーザーである`C##TEST2`として`PDB21`に接続します。
    
        SQL> <copy>CONNECT c##test2@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
12.  レルムを削除します。
    
        SQL> <copy>CONNECT c##sec_admin</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>EXEC DBMS_MACADM.DELETE_REALM_CASCADE('Root Test Realm')</copy>
        
        PL/SQL procedure successfully completed.
        
        SQL>
        

## タスク4: PDBの通常または必須レルムを含む共通オブジェクトに対する表データのアクセシビリティのテスト

1.  `PDB21`の`C##TEST1`表にPDB標準レルムを作成します。
    
        SQL> <copy>CONNECT sec_admin@PDB21</copy>
        
        Enter password: <i>WElcome123##</i>
        
        Connected.
        
    
        SQL> <copy>BEGIN
        DBMS_MACADM.CREATE_REALM(
          realm_name    => 'Test Realm',
          description   => 'Test Realm description',
          enabled       => DBMS_MACUTL.G_YES,
          audit_options => DBMS_MACUTL.G_REALM_AUDIT_FAIL,
          realm_type    => 0);
        END;
        /</copy>  2    3    4    5    6    7    8    9
        
        PL/SQL procedure successfully completed.
        
    
        
        SQL> <copy>BEGIN
        DBMS_MACADM.ADD_OBJECT_TO_REALM(
          realm_name   => 'Test Realm',
          object_owner => 'C##TEST1',
          object_name  => '%',
          object_type  => '%');
        END;
        /</copy>  2    3    4    5    6    7    8
        
        PL/SQL procedure successfully completed.
        
        SQL>
        
2.  表の共通所有者の`C##TEST1`としてCDBルートに接続します。
    
        SQL> <copy>CONNECT c##test1</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
3.  別の共通ユーザーである`C##TEST2`としてCDBルートに接続します。
    
        SQL> <copy>CONNECT c##test2</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
4.  表の共通所有者である`C##TEST1`として`PDB21`に接続します。
    
        SQL> <copy>CONNECT c##test1@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
5.  別の共通ユーザーである`C##TEST2`として`PDB21`に接続します。
    
        SQL> <copy>CONNECT c##test2@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        SELECT * FROM c##test1.l_emp
                               *
        ERROR at line 1:
        ORA-01031: insufficient privileges
        
        SQL>
        
6.  レルムを削除します。
    
        SQL> <copy>CONNECT c##sec_admin@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>EXEC DBMS_MACADM.DELETE_REALM_CASCADE('Test Realm')</copy>
        PL/SQL procedure successfully completed.
        
        SQL>
        
7.  `PDB21`の`C##TEST1`表にPDB必須レルムを作成します。
    
        SQL> <copy>CONNECT sec_admin@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>BEGIN
        DBMS_MACADM.CREATE_REALM(
          realm_name    => 'Test Realm',
          description   => 'Test Realm description',
          enabled       => DBMS_MACUTL.G_YES,
          audit_options => DBMS_MACUTL.G_REALM_AUDIT_FAIL,
          realm_type    => 1);
        END;
        /</copy>  2    3    4    5    6    7    8    9
        
        PL/SQL procedure successfully completed.
        
    
        SQL> <copy>BEGIN
        DBMS_MACADM.ADD_OBJECT_TO_REALM(
          realm_name   => 'Test Realm',
          object_owner => 'C##TEST1',
          object_name  => '%',
          object_type  => '%');
        END;
        /</copy>  2    3    4    5    6    7    8
        
        PL/SQL procedure successfully completed.
        
        SQL>
        
8.  表の共通所有者の`C##TEST1`としてCDBルートに接続します。
    
        SQL> <copy>CONNECT c##test1</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
9.  別の共通ユーザーである`C##TEST2`としてCDBルートに接続します。
    
        SQL> <copy>CONNECT c##test2</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
10.  表の共通所有者である`C##TEST1`として`PDB21`に接続します。
    
        SQL> <copy>CONNECT c##test1@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        SELECT * FROM c##test1.l_emp
                               *
        ERROR at line 1:
        ORA-01031: insufficient privileges
        
        SQL>
        
11.  別の共通ユーザーである`C##TEST2`として`PDB21`に接続します。
    
        SQL> <copy>CONNECT c##test2@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        SELECT * FROM c##test1.l_emp
                               *
        ERROR at line 1:
        ORA-01031: insufficient privileges
        
        SQL>
        
12.  レルムを削除します。
    
        SQL> <copy>CONNECT c##sec_admin@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>EXEC DBMS_MACADM.DELETE_REALM_CASCADE('Test Realm')</copy>
        PL/SQL procedure successfully completed.
        
        SQL>
        

## タスク5: ローカル・ユーザーによる共通オブジェクトに対するOracle Database Vaultコントロールの作成の制限

1.  ローカル・ユーザーによる共通オブジェクトに対するOracle Database Vaultコントロールの作成を制限します。
    
        SQL> <copy>CONNECT c##sec_admin</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM DVSYS.DBA_DV_COMMON_OPERATION_STATUS;</copy>
        
        NAME                      STATU
        ------------------------- -----
        DV_ALLOW_COMMON_OPERATION FALSE
        
        SQL> <copy>EXEC DBMS_MACADM.ALLOW_COMMON_OPERATION</copy>
        
        PL/SQL procedure successfully completed.
        
        SQL> <copy>SELECT * FROM DVSYS.DBA_DV_COMMON_OPERATION_STATUS;</copy>
        
        NAME                      STATU
        ------------------------- -----
        DV_ALLOW_COMMON_OPERATION TRUE
        
        SQL>
        

## タスク6: 共通オブジェクトに対する共通の通常または必須レルムを使用した表データのアクセシビリティのテスト

1.  CDBルートの`C##TEST1`表に共通標準レルムを作成します。
    
        SQL> <copy>CONNECT c##sec_admin</copy>
        
        Enter password: <i>WElcome123##</i>
        
        Connected.
        
    
        SQL> <copy>BEGIN
        DBMS_MACADM.CREATE_REALM(
          realm_name    => 'Root Test Realm',
          description   => 'Test Realm description',
          enabled       => DBMS_MACUTL.G_YES,
          audit_options => DBMS_MACUTL.G_REALM_AUDIT_FAIL,
          realm_type    => 0);
        END;
        /</copy>  2    3    4    5    6    7    8    9
        
        PL/SQL procedure successfully completed.
        
    
        SQL> <copy>BEGIN
        DBMS_MACADM.ADD_OBJECT_TO_REALM(
          realm_name   => 'Root Test Realm',
          object_owner => 'C##TEST1',
          object_name  => '%',
          object_type  => '%');
        END;
        /</copy>  2    3    4    5    6    7    8
        
        PL/SQL procedure successfully completed.
        
        SQL>
        
2.  表の共通所有者の`C##TEST1`としてCDBルートに接続します。
    
        SQL> <copy>CONNECT c##test1</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
3.  別の共通ユーザーである`C##TEST2`としてCDBルートに接続します。
    
        SQL> <copy>CONNECT c##test2</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        SELECT * FROM c##test1.g_emp
                               *
        ERROR at line 1:
        ORA-01031: insufficient privileges
        
        SQL>
        
4.  表の共通所有者である`C##TEST1`として`PDB21`に接続します。
    
        SQL> <copy>CONNECT c##test1@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
5.  別の共通ユーザーである`C##TEST2`として`PDB21`に接続します。
    
        SQL> <copy>CONNECT c##test2@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
6.  レルムを削除します。
    
        SQL> <copy>CONNECT c##sec_admin</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>EXEC DBMS_MACADM.DELETE_REALM_CASCADE('Root Test Realm')</copy>
        
        PL/SQL procedure successfully completed.
        
        SQL>
        
7.  CDBルートの`C##TEST1`表に共通必須レルムを作成します。
    
        SQL> <copy>BEGIN
        DBMS_MACADM.CREATE_REALM(
        realm_name    => 'Root Test Realm',
        description   => 'Test Realm description',
        enabled       => DBMS_MACUTL.G_YES,
        audit_options => DBMS_MACUTL.G_REALM_AUDIT_FAIL,
        realm_type    => 1);
        

終了; / 2 3 4 5 6 7 8 9

PL/SQLプロシージャが正常に完了しました。

SQL> BEGIN DBMS\_MACADM.ADD\_OBJECT\_TO\_REALM( realm\_name => 'Root Test Realm', object\_owner => 'C##TEST1', object\_name => '%', object\_type => '%'); END; / 2 3 4 5 6 7 8

    PL/SQL procedure successfully completed.
    
    SQL>
    ```
    

8.  表の共通所有者の`C##TEST1`としてCDBルートに接続します。
    
        SQL> <copy>CONNECT c##test1</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        SELECT * FROM c##test1.g_emp
                               *
        ERROR at line 1:
        ORA-01031: insufficient privileges
        
        
        SQL>
        
9.  別の共通ユーザーである`C##TEST2`としてCDBルートに接続します。
    
        SQL> <copy>CONNECT c##test2</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        SELECT * FROM c##test1.g_emp
                               *
        ERROR at line 1:
        ORA-01031: insufficient privileges
        
        
        SQL>
        
10.  表の共通所有者である`C##TEST1`として`PDB21`に接続します。
    
        SQL> <copy>CONNECT c##test1@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
11.  別の共通ユーザーである`C##TEST2`として`PDB21`に接続します。
    
        SQL> <copy>CONNECT c##test2@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
12.  レルムを削除します。
    
        SQL> <copy>CONNECT c##sec_admin</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>EXEC DBMS_MACADM.DELETE_REALM_CASCADE('Root Test Realm')</copy>
        
        PL/SQL procedure successfully completed.
        
        SQL>
        

## タスク7: PDBの通常または必須レルムを含む共通オブジェクトに対する表データのアクセシビリティのテスト

1.  `PDB21`の`C##TEST1`表にPDB標準レルムを作成します。
    
        SQL> <copy>CONNECT sec_admin@PDB21</copy>
        
        Enter password: <i>WElcome123##</i>
        
        Connected.
        
    
        SQL> <copy>BEGIN
        DBMS_MACADM.CREATE_REALM(
        realm_name    => 'Test Realm1',
        description   => 'Test Realm description',
        enabled       => DBMS_MACUTL.G_YES,
        audit_options => DBMS_MACUTL.G_REALM_AUDIT_FAIL,
        realm_type    => 0);
        END;
        /</copy>  2    3    4    5    6    7    8    9
        
        PL/SQL procedure successfully completed.
        
    
        SQL> <copy>BEGIN
        DBMS_MACADM.ADD_OBJECT_TO_REALM(
        realm_name   => 'Test Realm1',
        object_owner => 'C##TEST1',
        object_name  => '%',
        object_type  => '%');
        END;
        /</copy>  2    3    4    5    6    7    8
        
        BEGIN
        
        *
        
        ERROR at line 1:
        ORA-47286: cannot add %, C##TEST1.%  to a realm
        ORA-06512: at "DVSYS.DBMS_MACADM", line 1059
        ORA-06512: at line 2
        
        SQL> <copy>!oerr ora 47286</copy>
        47286, 00000, "cannot add %s, %s.%s  to a realm"
        // *Cause: When ALLOW COMMON OPERATION was set to TRUE, a smaller scope user was not allowed to add a larger scope user's object or a larger scope role to a realm.
        // *Action: When ALLOW COMMON OPERATION is TRUE, do not add a larger scope user's object or a larger scope role to a realm.
        SQL>
        
2.  表の共通所有者の`C##TEST1`としてCDBルートに接続します。
    
        SQL> <copy>CONNECT c##test1</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
3.  別の共通ユーザーである`C##TEST2`としてCDBルートに接続します。
    
        SQL> <copy>CONNECT c##test2</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
4.  表の共通所有者である`C##TEST1`として`PDB21`に接続します。
    
        SQL> <copy>CONNECT c##test1@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
5.  別の共通ユーザーである`C##TEST2`として`PDB21`に接続します。
    
        SQL> <copy>CONNECT c##test2@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
6.  レルムを削除します。
    
        SQL> <copy>CONNECT c##sec_admin@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>EXEC DBMS_MACADM.DELETE_REALM_CASCADE('Test Realm1')</copy>
        
        PL/SQL procedure successfully completed.
        
        SQL>
        
7.  `PDB21`の`C##TEST1`表にPDB必須レルムを作成します。
    
        SQL> <copy>CONNECT sec_admin@PDB21</copy>
        
        Enter password: <i>WElcome123##</i>
        
        Connected.
        
    
        SQL> <copy>BEGIN
        DBMS_MACADM.CREATE_REALM(
          realm_name    => 'Test Realm1',
          description   => 'Test Realm description',
          enabled       => DBMS_MACUTL.G_YES,
          audit_options => DBMS_MACUTL.G_REALM_AUDIT_FAIL,
          realm_type    => 1);
        END;
        /</copy>  2    3    4    5    6    7    8    9
        
        PL/SQL procedure successfully completed.
        
    
        SQL> <copy>BEGIN
        DBMS_MACADM.ADD_OBJECT_TO_REALM(
          realm_name   => 'Test Realm1',
          object_owner => 'C##TEST1',
          object_name  => '%',
          object_type  => '%');
        END;
        /</copy>  2    3    4    5    6    7    8
        
        BEGIN
        
        *
        
        ERROR at line 1:
        
        ORA-47286: cannot add %, C##TEST1.%  to a realm
        
        ORA-06512: at "DVSYS.DBMS_MACADM", line 1059
        
        ORA-06512: at line 2
        
        SQL>
        
8.  表の共通所有者の`C##TEST1`としてCDBルートに接続します。
    
        SQL> <copy>CONNECT c##test1</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
9.  別の共通ユーザーである`C##TEST2`としてCDBルートに接続します。
    
        SQL> <copy>CONNECT c##test2</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
10.  表の共通所有者である`C##TEST1`として`PDB21`に接続します。
    
        SQL> <copy>CONNECT c##test1@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
11.  別の共通ユーザーである`C##TEST2`として`PDB21`に接続します。
    
        SQL> <copy>CONNECT c##test2@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
12.  レルムを削除します。
    
        SQL> <copy>CONNECT sec_admin@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>EXEC DBMS_MACADM.DELETE_REALM_CASCADE('Test Realm1')</copy>
        
        PL/SQL procedure successfully completed.
        
        SQL> <copy>EXIT</copy>
        $
        

## タスク8: 概要

`DV_ALLOW_COMMON_OPERATION`値を切り替えたときのPDBの共通ユーザー・オブジェクトに対するデータ・アクセスの動作をまとめてみましょう。

*   通常または必須のレルムをCDBルートに作成し、通常または必須のPDBレルムを作成し、`DV_ALLOW_COMMON_OPERATION`が`TRUE`の場合は、共通ユーザー・オブジェクトのデータにアクセスできます。
    
*   `DV_ALLOW_COMMON_OPERATION`が`FALSE`に設定されたときにローカル・レルムが作成されていた場合は、新しいコントロール後もローカル・レルムは存在しますが、強制は無視されます。
    

## タスク9: PDBとCDBルートの両方でのDatabase Vaultの無効化

1.  `disable_DV.sh`スクリプトを実行して、PDBとCDBルートの両方でDatabase Vaultを無効にします。
    
        $ <copy>/home/oracle/labs/M104781GC10/disable_DV.sh</copy>
        ...
        
        SQL> ADMINISTER KEY MANAGEMENT SET KEY IDENTIFIED BY <i>WElcome123##</i> WITH BACKUP CONTAINER=CURRENT;
        keystore altered.
        
        SQL> exit
        $
        

## 確認

*   **作成者** - Donna Keesling、データベースUAチーム
*   **コントリビュータ** - David Start、Kay Malcolm、Database Product Management
*   **最終更新者/日付** - David Start、2020年12月