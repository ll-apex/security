# すべてのPDBに対する最小パスワード長の強制

## 概要

このラボでは、データベース・ユーザー・プロファイルへのアクセスを制限することなく、CDB全体でデータベース・ユーザー・アカウントの最小パスワード長を強制する方法を示します。

見積時間: 15分

### 目標

この演習では、次のことを行います。

*   環境の設定

### 前提条件

*   Oracle Account
*   SSHキー
*   DBCS VMデータベースの作成
*   21cの設定

## タスク1: CDBルートでの必須プロファイルの作成

1.  `CDB21`のCDBルートに接続します。
    
        $ <copy>sqlplus sys@CDB21 AS SYSDBA</copy>
        
        SQL*Plus: Release 21.0.0.0.0 - Production on Wed Aug 12 09:45:45 2020
        Version 21.1.0.0.0
        Enter password: <i>WElcome123##</i>
        Connected to:
        Oracle Database 21c Enterprise Edition Release 21.0.0.0.0 - Development
        Version 21.1.0.0.0
        
        SQL>
        
2.  必須ルート・プロファイルを作成します。必須ルート・プロファイルは常時オンのユーザー・プロファイルとして機能します。ユーザーが割り当てられているプロファイルからの既存の制限に加えて、必須のプロファイル制限が適用されます。これにより、ユーザー・アカウントのプロファイルのパスワード複雑度スクリプト(存在する場合)の前に、必須プロファイルのパスワード複雑度検証スクリプトが実行されるという意味で、共用体効果が作成されます。
    
        SQL> <copy>COL resource_name FORMAT A30</copy>
        
        SQL> <copy>COL limit FORMAT A30</copy>
        
        SQL> <copy>CREATE MANDATORY PROFILE c##prof_min_pass_len
                              LIMIT PASSWORD_VERIFY_FUNCTION ora12c_stig_verify_function
                              CONTAINER=ALL;</copy>
        
        Profile created.
        
        SQL> <copy>SELECT resource_name, limit, mandatory FROM cdb_profiles
                    WHERE profile='C##PROF_MIN_PASS_LEN' AND resource_type='PASSWORD';</copy>
        
        RESOURCE_NAME                  LIMIT                          MAN
        ------------------------------ ------------------------------ ---
        PASSWORD_VERIFY_FUNCTION       FROM ROOT                      YES
        FAILED_LOGIN_ATTEMPTS                                         YES
        PASSWORD_LIFE_TIME                                            YES
        PASSWORD_REUSE_TIME                                           YES
        PASSWORD_REUSE_MAX                                            YES
        PASSWORD_LOCK_TIME                                            YES
        PASSWORD_GRACE_TIME                                           YES
        INACTIVE_ACCOUNT_TIME                                         YES
        PASSWORD_ROLLOVER_TIME                                        YES
        PASSWORD_VERIFY_FUNCTION       ORA12C_STIG_VERIFY_FUNCTION    YES
        FAILED_LOGIN_ATTEMPTS                                         YES
        PASSWORD_LIFE_TIME                                            YES
        PASSWORD_REUSE_TIME                                           YES
        PASSWORD_REUSE_MAX                                            YES
        PASSWORD_LOCK_TIME                                            YES
        PASSWORD_GRACE_TIME                                           YES
        INACTIVE_ACCOUNT_TIME                                         YES
        PASSWORD_ROLLOVER_TIME                                        YES
        
        18 rows selected.
        
        SQL>
        

## タスク2: `MANDATORY_USER_PROFILE`初期化パラメータの設定

1.  初期化パラメータの設定
    
        SQL> <copy>ALTER SYSTEM SET mandatory_user_profile=C##PROF_MIN_PASS_LEN;</copy>
        System altered.
        
        SQL> <copy>SHOW PARAMETER mandatory_user_profile</copy>
        
        NAME                                 TYPE        VALUE
        ------------------------------------ ----------- ------------------------------
        mandatory_user_profile               string      C##PROF_MIN_PASS_LEN
        SQL>
        
    
    _必須プロファイルのパスワード検証関数は、常に`CDB$ROOT`から強制されることが想定されます。つまり、パスワード・リソース制限は常に`CDB$ROOT`からフェッチされて実行され、`MANDATORY_USER_PROFILE`初期化パラメータに応じてCDB全体のPDBに強制されるということです。_
    

## タスク3: パスワード検証functio`n`を置き換えて、最小パスワード長を強制します。

1.  検証関数の置換
    
        SQL> <copy>CREATE OR REPLACE FUNCTION ora12c_stig_verify_function
                  ( username VARCHAR2, password VARCHAR2, old_password VARCHAR2)
                  RETURN BOOLEAN
                  IS
                  BEGIN   
                   -- mandatory verify function will always be evaluated regardless of the  
                   -- password verify function that is associated to a particular profile/user
                   -- requires the minimum password length to be 10 characters
                   IF NOT ora_complexity_check(password, chars => 10) THEN return(false);   
                   END IF;
                   RETURN(true);
                   END;
                   /</copy>
        
        Function created.
        
        SQL>
        

## タスク4: テスト

1.  `PDB21`に新しいユーザー`JOHN`を作成します。
    
        SQL> <copy>CONNECT system@PDB21</copy>
        
        Enter password: <i>Welcome123##</i>
        Connected.
        
    
        SQL> <copy>CREATE USER john IDENTIFIED BY pass;</copy>
        CREATE USER john IDENTIFIED BY pass
        *
        ERROR at line 1:
        ORA-28219: password verification failed for mandatory profile
        ORA-20000: password length less than 10 characters
        
    
        SQL> <copy>CREATE USER john IDENTIFIED BY password123;</copy>
        User created.
        
    
        SQL> <copy>DROP USER john CASCADE;</copy>
        User dropped.
        
        SQL>
        

## タスク5: 構成のリセット

1.  ルート内の必須プロファイルを削除します。
    
        SQL> <copy>CONNECT sys@cdb21 AS SYSDBA</copy>
        Connected.
        
    
        SQL> <copy>DROP PROFILE c##prof_min_pass_len;</copy>
        DROP PROFILE c##prof_min_pass_len
        *
        ERROR at line 1:
        ORA-02381: cannot drop C##PROF_MIN_PASS_LEN profile
        
    
        SQL> <copy>!oerr ora 2381</copy>
        02381, 00000, "cannot drop %s profile"
        //  *Cause:  An attempt was made to drop PUBLIC_DEFAULT or a mandatory profile,
        //           which is not allowed due to following restrictions:
        //             * PUBLIC_DEFAULT profile can be dropped only when the database
        //               is in migration mode.
        //             * A mandatory profile can be dropped only if it is not set as a
        //               mandatory profile in root container (CDB$ROOT) of a multitenant
        //               container database (CDB) or in a Pluggable Database (PDB).
        //  *Action: If you are trying to drop the PUBLIC_DEFAULT profile, try dropping
        //           it during migration mode. If you are trying to drop a mandatory
        //           profile, check the MANDATORY_USER_PROFILE system parameter setting
        //           in the root container (CDB$ROOT) or in a Pluggable Database (PDB)
        //           and retry the operation after resetting the MANDATORY_USER_PROFILE
        //           system parameter by executing ALTER SYSTEM RESET DDL statement.
        
        SQL>
        
2.  最初に`MANDATORY_USER_PROFILE`初期化パラメータをリセットします。
    
        SQL> <copy>ALTER SYSTEM RESET mandatory_user_profile;</copy>
        System altered.
        
    
        SQL> <copy>SHOW PARAMETER mandatory_user_profile</copy>
        
        NAME                                 TYPE        VALUE
        ------------------------------------ ----------- ------------------------------
        mandatory_user_profile               string      C##PROF_MIN_PASS_LEN
        
    
        SQL> <copy>DROP PROFILE c##prof_min_pass_len;</copy>
        DROP PROFILE c##prof_min_pass_len
        *
        ERROR at line 1:
        ORA-02381: cannot drop C##PROF_MIN_PASS_LEN profile
        
        SQL><copy>exit;</copy>
        
3.  インスタンスを再起動します。
    
        <copy>/home/oracle/labs/M104784GC10/wallet.sh</copy>
        
4.  インスタンスに接続し、プロファイルを削除します
    
        SQL> <copy>sqlplus sys@cdb21 AS SYSDBA</copy>
        Connected.
        
    
        SQL> <copy>SHOW PARAMETER mandatory_user_profile</copy>
        
        NAME                                 TYPE        VALUE
        ------------------------------------ ----------- ------------------------------
        mandatory_user_profile               string
        
    
        SQL> <copy>DROP PROFILE c##prof_min_pass_len;</copy>
        Profile dropped.
        
        SQL> <copy>EXIT</copy>
        
        $
        

## 謝辞

*   **作成者** - Donna Keesling、データベースUAチーム
*   **コントリビュータ** - David Start、Kay Malcolm、Database Product Management
*   **最終更新者/日付** - David Start、2020年12月