# EMPLOYEESEARCH\_PRODスキーマへの接続の識別

## 概要

この演習では、ATPインスタンスでのレルムの作成を確認します。レルムは**シミュレーション・モード**のままにします。これにより、レルムまたはコマンド・ルールの開発フェーズ中にエラーの記録を取得できるようになります。シミュレーション・モードを使用して、`EMPLOYEESEARCH_PROD`スキーマへの接続を識別します。

### 目標

この演習では、次のタスクを実行します。

*   Database Vaultレルムを作成します。
*   **シミュレーション・モード**を使用して、`EMPLOYEESEARCH_PROD`スキーマへの接続(アプリケーション接続と非アプリケーション接続)を識別します。

### 前提条件

この演習では、次を想定しています。

*   Oracle Cloud Infrastructure (OCI)テナンシ・アカウント
*   前の演習の完了: Autonomous Databaseインスタンスの構成、レガシーGlassfish HRアプリケーションへの接続、Glassfishアプリケーションのデータのロードと検証、Database Vaultの有効化およびHRアプリケーションの検証

## タスク1: Database Vaultレルムの作成

1.  **「データベース・アクションSQLワークシート」**に戻ります。右上の`ADMIN`のメニュー・バーで、**「サインアウト」**を選択します。ユーザー`sec_admin_owen`の下のデータベース・アクションにサインインし、**「開発」**の下の**「SQL」**を選択します。ワークシートがクリアされ、スキーマが**ADMIN**から`SEC_ADMIN_OWEN`に更新されていることを確認します。
    
    ![データベース・アクション・スキーマの変更](images/change-schema-dbactions.png)
    
2.  次のスクリプトをコピーしてSQLワークシートに貼り付け、`EMPLOYEESEARCH_PROD`を保護するためにDatabase Vaultレルム`PROTECT_MYHRAPP`を作成します。**「スクリプトの実行」**ボタンを選択してスクリプトを実行します。下部のスクリプト出力をチェックして、スクリプトが正常に実行されたことを確認します。
    
        	<copy>BEGIN
        			DVSYS.DBMS_MACADM.CREATE_REALM(
        				realm_name => 'PROTECT_MYHRAPP'
        				,description => 'A mandatory realm, in simulation mode, to show how EMPLOYEESEARCH_PROD would be protected'
        				,enabled => DBMS_MACUTL.G_SIMULATION
        				,audit_options => DBMS_MACUTL.G_REALM_AUDIT_FAIL
        				,realm_type => 1); 
        		END;
        		/</copy>
        
    
    ![dvレルムの作成](images/create-realm.png)
    
3.  SQLワークシートをクリアします。次の問合せをコピーして貼り付け、現在のDatabase Vaultレルムが作成されたことを示す出力を確認します。
    
        	<copy>SELECT name, description, enabled FROM dba_dv_realm WHERE id# >= 5000 ORDER BY 1;</copy>
        
    
    ![DVDレルムの表示](images/show-dv-realm.png)
    
4.  SQLワークシートをクリアします。次のスクリプトをコピーして貼り付け、`EMPLOYEESEARCH_PROD`スキーマとそのすべてのオブジェクトを`PROTECT_MYHRAPP` Database Vaultレルムに追加します。これは、`EMPLOYEESEARCH_PROD`スキーマ内に作成された既存のオブジェクトおよび新しいオブジェクトに適用されます。出力をチェックして、スクリプトが正常に実行されたことを確認します。
    
        <copy>BEGIN
           DVSYS.DBMS_MACADM.ADD_OBJECT_TO_REALM(
               realm_name   => 'PROTECT_MYHRAPP',
               object_owner => 'EMPLOYEESEARCH_PROD',
               object_name  => '%',
               object_type  => '%');
        END;
        /</copy>
        
    
    次のタスクでは、スキーマ自体によるアクセスを含め、`EMPLOYEESEARCH_PROD`スキーマ内のオブジェクトへのアクセスを取得します。これにより、使用可能なオブジェクトと取得元の場所を理解できます。
    
5.  **HR本番**アプリケーションに戻り、ログアウトしてから、**hradmin**として再度ログインします。データがまだ表示され、使用可能であることを確認します。従業員プロファイルのメニュー タブをクリックして、全てのデータがあることを確認します。
    
    ![従業員の検索](images/search-emp.png)
    
    ![従業員の選択](images/select-emp.png)
    
    ![従業員のナビゲート](images/navigate-emp.png)
    

## タスク2: シミュレーション・モードを使用して、EMPLOYEESEARCH\_PRODスキーマへの接続を識別します。

1.  `SEC_ADMIN_OWEN`として、次の問合せをコピーして貼り付け、`dba_dv_simulation_log`を確認します。`EMPLOYEESEARCH_PROD`が**オブジェクト所有者**であることを確認します。スキーマ`EMPLOYEESEARCH_PROD`に**レルム違反**というラベルが付けられていることに注意してください。`EMPLOYEESEARCH_PROD`がデータにアクセスできる唯一のスキーマになるように、Database Vaultを構成します。
    
        	<copy>select * from dba_dv_simulation_log;</copy>
        
    
    ![シミュレーション・ログの表示](images/sec-view-log.png)
    
2.  右上の`SEC_ADMIN_OWEN`のメニュー・バーで、**「サインアウト」**を選択します。ユーザー`dba_debra`でデータベース・アクションにサインインし、**「開発」**で**「SQL」**を選択します。ワークシートがクリアされ、スキーマが`DBA_DEBRA`に更新されていることを確認します。
    
    ![DBAデブラ・ログイン](images/dba-deb-login.png)
    
3.  次のコマンドをコピーして貼り付け、`EMPLOYEESEARCH_PROD`スキーマの下の`DEMO_HR_EMPLOYEES`表を問い合せます。
    
        	<copy>alter session set current_schema = EMPLOYEESEARCH_PROD;</copy>
        
    
        	<copy>select a.USERID, a.FIRSTNAME, a.LASTNAME, a.EMAIL, a.PHONEMOBILE, a.PHONEFIX, a.PHONEFAX, a.EMPTYPE, a.POSITION, a.ISMANAGER, a.MANAGERID, a.DEPARTMENT, a.CITY, a.STARTDATE, a.ENDDATE, a.ACTIVE, a.COSTCENTER, b.FIRSTNAME as MGR_FIRSTNAME, b.LASTNAME as MGR_LASTNAME, b.USERID as MGR_USERID from DEMO_HR_EMPLOYEES a left outer join DEMO_HR_EMPLOYEES b on a.MANAGERID = b.USERID where 1=1 order by a.LASTNAME, a.FIRSTNAME</copy>
        
    
    ![dbaを使用した問合せ](images/dba-query.png)
    
    DBAが、HRアプリケーションに表示されるのと同じデータであるEMPLOYEESEARCH\_PRODスキーマにあるオブジェクトを問い合せる方法に注意してください。これは、Database Vaultレルムをシミュレーションから強制モードに切り替えるときに禁止する例です。これにより、`ADMIN`などの特権ユーザー、および`PDB_DBA`ロールを持つユーザーのアクセスも無効になります。
    
4.  `DBA_DEBRA`からログアウトし、`SEC_ADMIN_OWEN`としてデータベース・アクションに再度ログインします。**「開発者」**で、**「SQL」**を選択します。次のコマンドをコピーして貼り付け、実行し、`dba_dv_simulation_log`表を問い合せます。
    
        	<copy>select username, violation_type, realm_name, object_owner, object_name, client_ip, dv$_module, dv$_client_identifier from dba_dv_simulation_log;</copy>
        
    
    ![問合せSimログ](images/query-sim-log.png)
    
    この問合せでは、作成されたデータベース・ボールト・レルム内のオブジェクトに対する最新の使用状況および接続がすべて表示されます。`DBA_DEBRA, SEC_ADMIN_OWEN, and ADMIN`のようなユーザーからの接続があることに注意してください。`EMPLOYEESEARCH_PROD`以外のユーザーは、`EMPLOYEESEARCH_PROD`にレルム認可を提供すると禁止されます。`DV$_MODULE`列にも注意してください。これは、`EMPLOYEESEARCH_PROD`オブジェクトへの接続メソッドを示しています。信頼できる唯一の接続は、HRアプリケーション接続である`JDBC Thin Client`からの接続です。他のすべての接続は、作成されたレルムを使用して禁止されます。
    
5.  次のコマンドを使用して、`dba_dv_simulation_log`表をクリアします。
    
        	<copy>delete from dvsys.simulation_log$;</copy>
        
    
        	<copy>commit;</copy>
        
6.  SQLワークシートが明確であることを確認します。次のPL\*SQLブロックをコピー、貼付けおよび実行して、レルム`EMPLOYEESEARCH_PROD`を`PROTECT_MYHRAPP`レルムの認可された参加者として追加します。これにより、`EMPLOYEESEARCH_PROD`スキーマは、シミュレーション・ログにレルム違反として表示せずに独自のオブジェクトにアクセスでき、シミュレーション・モードから強制モードに切り替えると、オブジェクトにアクセスできるようになります。
    
        	<copy>
        	begin
        		DVSYS.DBMS_MACADM.ADD_AUTH_TO_REALM(
        			realm_name => 'PROTECT_MYHRAPP'
        			, grantee => 'EMPLOYEESEARCH_PROD'
        			, rule_set_name => null
        			, auth_options => DBMS_MACUTL.G_REALM_AUTH_OWNER); 
        	end;
        	/</copy>
        
    
    ![レルム認可の追加](images/add-auth.png)
    
7.  **HR本番アプリケーション**に戻り、ログアウトしてから**hradmin**として再度ログインします。従業員データがまだ存在し、アプリケーションが正しく機能していることを確認するために、従業員データをもう一度確認してください。
    
    ![従業員の検索](images/search-emp.png)
    
    ![従業員の選択](images/select-emp.png)
    
8.  Oracle Cloudで**データベース・アクション**を開きます。`sec_admin_owen`として、`dba_dv_simulation_log`表を問い合せるには、次のコマンドをコピー、貼付けおよび実行します。
    
        	<copy>select * from dba_dv_simulation_log;</copy>
        
    
    ![従業員の検索](images/log-query.png)
    
    これで、レルム認可を`EMPLOYEESEARCH_PROD`に追加しました。**JDBCクライアント**またはHRアプリケーションからの着信接続は、現在は信頼できるユーザーおよび接続であるため、レルム違反とみなされなくなりました。
    
9.  クラウド・シェル・コンソールを再配置します。次のコマンドを使用して、`dv_query_employee_search.sh`スクリプトを**ADMIN**として実行します。
    
    _ノート: アクティブでないためにGlassfishインスタンスからログアウトしている場合は、次のコマンドを使用して再度ログインします。パブリックIPアドレスは、Oracle Cloudのインスタンス詳細ダッシュボードにあります。_
    
        	<copy>chmod 600 myhrappkey</copy>
        
    
        <copy>ssh -i myhrappkey opc@<PASTE INSTANCE PUBLIC IP ADDRESS HERE></copy>
        
    
    `dv_query_employee_search.sh`を実行するためのコマンドを次に示します。
    
        	<copy>cd cd secure-legacy-app-db-vault</copy>
        
    
        	<copy>./dv_query_employee_search.sh</copy>
        
    
    ![adminを使用した問合せ](images/bash-query.png)
    
10.  クラウド・シェルを最小化し、Oracle Cloudの**「データベース・アクション」**タブに戻ります。`sec_admin_owen`として、コピー、貼付けおよび次のコマンドを実行して、`dba_dv_simulation_log`表を再度問い合せます。
    
        <copy>select * from dba_dv_simulation_log;</copy>
        
    
    ![問合せポスト管理](images/post-admin-query.png)
    

`ADMIN`は`EMPLOYEESEARCH_PROD`データベース・オブジェクトにアクセスしたため、Database Vaultシミュレーション・ログにレルム違反として表示されます。次に、レルムを**シミュレーション・モードから強制モード**に移動し、`ADMIN`や`DBA_DEBRA`などの特権ユーザーには、`EMPLOYEESEARCH_PROD`オブジェクトを問い合せるための十分な権限がなくなることを確認します。

**次の演習に進みます。**

## さらに学ぶ

*   [Database Vaultレルムの構成](https://docs.oracle.com/database/121/DVADM/cfrealms.htm#DVADM003)
*   [Oracle Databse Vaultシミュレーション・モード](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/dvadmusing-training-mode-to-log-realm-and-command-rule-activities.html)

## 確認

*   **著者**\- 北米スペシャリスト・ハブ、Ethan Shmargad氏
*   **作成者** - シニア・プリンシパル・プロダクト・マネージャー、Richard Evans
*   **最終更新者/日付** - Ethan Shmargad、2022年9月