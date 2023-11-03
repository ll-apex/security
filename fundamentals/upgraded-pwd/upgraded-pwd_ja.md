# アップグレードされたパスワード・ファイルで大/小文字が区別されるように強制

## 概要

このラボでは、Oracle Database 21cのパスワード・ファイルのパスワードで大/小文字が区別される方法を示します。以前のOracle Databaseリリースでは、パスワード・ファイルはデフォルトで元の大/小文字を区別しないベリファイアを保持しています。パスワード・ファイルの大/小文字を区別する`IGNORECASE`を有効または無効にするパラメータは削除されます。新しいパスワード・ファイル内のパスワードはすべて大文字と小文字が区別されます。

見積時間: 2分

### 目標

この演習では、次のことを行います。

*   環境の設定

### 前提条件

*   Oracle Account
*   SSHキー
*   DBCS VMデータベースの作成
*   21cの設定

## タスク1: パスワード・ファイル形式のCDB21の表示

1.  CDB21のパスワード・ファイル形式の表示
    
        	$ <copy>cd $ORACLE_BASE/dbs</copy>
        	$ <copy>ls -l orapwCDB21</copy>
        	-rw-r----- 1 oracle oinstall 2048 Mar  5 09:48 orapwCDB21
        	$ <copy>orapwd describe file=orapwCDB21</copy>
        	Password file Description : format=12
        	$
        

## タスク2: `SYS`パスワードを変更し、パスワードで大/小文字が区別されるようになったことの確認

1.  パスワード・ファイルで`SYS`ユーザーのパスワードを変更します。
    
        	$ <copy>orapwd file=$ORACLE_BASE/dbs/orapwCDB21 sys=Y force=Y format=12 ignorecase=Y</copy>
        
        	Usage 1: orapwd file=<fname> force={y|n} asm={y|n}
        				dbuniquename=<dbname> format={12|12.2}
        				delete={y|n} input_file=<input-fname>
        				'sys={y | password | external(<sys-external-name>)
        					| global(<sys-directory-DN>)}'
        				'sysbackup={y | password | external(<sysbackup-external-name>)
        							| global(<sysbackup-directory-DN>)}'
        				'sysdg={y | password | external(<sysdg-external-name>)
        						| global(<sysdg-directory-DN>)}'
        				'syskm={y | password | external(<syskm-external-name>)
        						| global(<syskm-directory-DN>)}'
        
        	Usage 2: orapwd describe file=<fname>
        		where
        		file   - name of password file (required),
        
        		password
        				- password for SYS will be prompted
        				if not specified at command line.
        				Ignored, if input_file is specified,
        
        		force  - whether to overwrite existing file, also clears
        				CRS resource if it already has password file
        				registered (optional),
        
        		asm    - indicates that the ASM instance password file is to
        				be stored in Automatic Storage Management (ASM)
        				disk group (optional),
        
        		dbuniquename
        				- unique database name used to identify database
        				password files residing in ASM diskgroup
        				or Exascale Vault.
        
        				Ignored when asm option is specified (optional),
        		format - use format=12 for new 12c features like SYSBACKUP, SYSDG
        				and SYSKM support, longer identifiers, SHA2 Verifiers etc.
        
        				use format=12.2 for 12.2 features like enforcing user
        				profile (password limits and password complexity) and
        				account status for administrative users.
        
        				If not specified, format=12.2 is default (optional),
        		delete - drops a password file. Must specify 'asm',
        				'dbuniquename' or 'file'. If 'file' is specified,
        				the file must be located on an ASM diskgroup
        				or Exascale Vault,
        
        		input_file
        				- name of input password file, from where old user
        				entries will be migrated (optional),
        
        		sys    - specifies if SYS user is password, externally or
        				globally authenticated.
        
        				For external SYS, also specifies external name.
        				For global SYS, also specifies directory DN.
        				SYS={y | password} specifies if SYS user password needs
        				to be changed when used with input_file,
        
        		sysbackup
        
        				- creates SYSBACKUP entry (optional).
        				Specifies if SYSBACKUP user is password, externally or
        				globally authenticated.
        				For external SYSBACKUP, also specifies external name.
        				For global SYSBACKUP, also specifies directory DN.
        				Ignored, if input_file is specified,
        
        		sysdg  - creates SYSDG entry (optional).
        				Specifies if SYSDG user is password, externally or
        				globally authenticated.
        
        				For external SYSDG, also specifies external name.
        				For global SYSDG, also specifies directory DN.
        				Ignored, if input_file is specified,
        
        		syskm  - creates SYSKM entry (optional).
        
        				Specifies if SYSKM user is password, externally or
        				globally authenticated.
        				For external SYSKM, also specifies external name.
        				For global SYSKM, also specifies directory DN.
        				Ignored, if input_file is specified,
        
        		describe
        
        				- describes the properties of specified password file
        				(required).
        
        		There must be no spaces around the equal-to (=) character.
        
        	$
        

_使用上のノートには、コマンドで使用できるすべてのパラメータが記述されています。`IGNORECASE`は非推奨になったため、記述されていません。_

2.  非推奨のパラメータを指定せずにコマンドを再入力します。
    
        	$ <copy>orapwd file=$ORACLE_BASE/dbs/orapwCDB21 sys=Y force=Y format=12</copy>
        	Enter password for SYS: <i>WElcome123##</i>
        
        	$
        
3.  `SYS`として`CDB21`にログオンします。次に、正しいケースを使用せずに、同じパスワードでログインしてみてください。
    
        	$ <copy>sqlplus sys@CDB21 AS SYSDBA</copy>
        	Enter password: <i>WElcome123##</i>
        
        	Connected to:
        
    
        	SQL> <copy>CONNECT sys@CDB21 AS SYSDBA</copy>
        	Enter password: <i>welcome123##</i>
        
        	ERROR:
        	ORA-01017: invalid username/password; logon denied
        	Warning: You are no longer connected to ORACLE.
        
        	SQL>
        
4.  ユーザーのリストを表示します。
    
        	SQL> <copy>CONNECT sys@CDB21 AS SYSDBA</copy>
        	Enter password: <i>WElcome123##</i>
        	Connected.
        
    
        	SQL> <copy>SET PAGES 100</copy>
        	SQL> <copy>COL username FORMAT A30</copy>
        	SQL> <copy>SELECT username, password_versions FROM dba_users ORDER BY 2,1;</copy>
        
        	USERNAME                       PASSWORD_VERSIONS
        	------------------------------ -----------------
        	SYS                            11G 12C
        	SYSTEM                         11G 12C
        	ANONYMOUS
        	APPQOSSYS
        	AUDSYS
        	CTXSYS
        	...
        	SQL> <copy>EXIT</copy>
        
        	$
        

## 確認

*   **作成者** - Donna Keesling、データベースUAチーム
*   **コントリビュータ** - データベース製品管理、Kay Malcolm
*   **最終更新者/日付** - Kay Malcolm、2020年11月