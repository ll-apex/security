# デフォルトの表領域暗号化アルゴリズムの設定

## 概要

このラボでは、Oracle Database 21cのパスワード・ファイルのパスワードで大/小文字が区別される方法を示します。以前のOracle Databaseリリースでは、パスワード・ファイルはデフォルトで元の大/小文字を区別しないベリファイアを保持しています。パスワード・ファイルの大/小文字を区別する`IGNORECASE`を有効または無効にするパラメータは削除されます。新しいパスワード・ファイル内のパスワードはすべて大文字と小文字が区別されます。

見積時間: 5分

### 目標

この演習では、次のことを行います。

*   環境の設定

### 前提条件

*   Oracle Account
*   SSHキー
*   DBCS VMデータベースの作成
*   21cの設定

## タスク1: デフォルトの表領域暗号化アルゴリズムの設定

1.  CDBルートに接続し、デフォルトの表領域暗号化アルゴリズムを表示します。
    
        	$ <copy>sqlplus / AS SYSDBA</copy>
        	Connected to:
        
        	Oracle Database 21c Enterprise Edition Release 21.0.0.0.0 - Production
        	Version 21.2.0.0.0
        
    
        	SQL> <copy>SHOW PARAMETER TABLESPACE_ENCRYPTION_DEFAULT_ALGORITHM</copy>
        
        	NAME                                       TYPE   VALUE
        	------------------------------------------ ------ -----------------------
        	tablespace_encryption_default_algorithm    string AES128
        
        	SQL>
        
2.  表領域暗号化アルゴリズムを変更します。
    
        	SQL> <copy>ALTER SYSTEM SET TABLESPACE_ENCRYPTION_DEFAULT_ALGORITHM=AES192;</copy>
        	System altered.
        
        	SQL> <copy>EXIT</copy>
        
        	$
        
3.  PDBに接続し、`PDBTEST`に新しい表領域を作成します。
    
        	$ <copy>sqlplus sys@PDB21 AS SYSDBA</copy>
        	Enter password: <b><i>WElcome123##</i></b>
        
        	Connected.
        

    SQL> <copy>CREATE TABLESPACE tbstest DATAFILE '/u02/app/oracle/oradata/pdb21/test01.dbf' SIZE 2M;</copy>
    Tablespace created.
    
    SQL>
    ```
    

## タスク2: 使用されている表領域暗号化アルゴリズムの検証

1.  使用されている表領域暗号化アルゴリズムを確認します。
    
        	SQL> <copy>SELECT name, encryptionalg
        				FROM v$tablespace t, v$encrypted_tablespaces v
        				WHERE t.ts#=v.ts#;</copy>
        
        	NAME                           ENCRYPT
        	------------------------------ -------
        	USERS                          AES128
        	TBSTEST                        AES192
        
        	SQL> <copy>EXIT</copy>
        
        	$
        

## 確認

*   **作成者** - Donna Keesling、データベースUAチーム
*   **コントリビュータ** - David Start、Kay Malcolm、Database Product Management
*   **最終更新者/日付** - David Start、2020年12月