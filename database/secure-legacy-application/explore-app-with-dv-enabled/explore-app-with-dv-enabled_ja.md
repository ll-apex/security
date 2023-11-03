# Database Vaultを有効にしたGlassfish HRアプリケーション機能の確認

## 概要

この演習では、Database Vaultレルムを**シミュレーション**・モードから**強制**モードにフリップします。これにより、レルムが本番にコミットされます。次に、レルムをテストに置き、特定のユーザーでアプリケーション・データを問い合せることができないことを示します。

### 目標

この演習では、次のタスクを実行します。

*   Database Vaultレルムを**シミュレーション**・モードから**強制**モードにフリップします
*   `EMPLOYEESEARCH_PROD`のデータを`DBA_DEBRA`および`ADMIN`で問い合せることができないことを示します。

### 前提条件

この演習では、次を想定しています。

*   Oracle Cloud Infrastructure (OCI)テナンシ・アカウント
*   前の演習の完了: Autonomous Databaseインスタンスの構成、レガシーGlassfish HRアプリケーションへの接続、Glassfishアプリケーションのデータのロードと検証、Database Vaultの有効化およびHRアプリケーションの検証、EMPLOYEESEARCH\_PRODスキーマへの接続の識別

## タスク1: Database Vaultレルムのシミュレーションから強制モードへのフリップ

1.  **データベース・アクション**内で、`sec_admin_owen`として、次の問合せ`dvsys.dba_dv_realm_auth`表を実行します。
    
        	<copy>select grantee from dvsys.dba_dv_realm_auth where realm_name = 'PROTECT_MYHRAPP';</copy>
        
    
    ![dbadvの問合せ](images/final-verification.png)
    
    これは、レルムが`EMPLOYEESEARCH_PROD`を保護していること、および`EMPLOYEESEARCH_PROD`がルール・セット(null)なしで**レルム所有者**として認可されていることの最終検証を示しています。
    
2.  `sec_admin_owen`として、次のコマンドをコピーして貼り付け、Database Vaultレルムを**シミュレーション**・モードから**強制**モードに切り替えます。出力をチェックして、プロシージャが正常に完了したことを確認します。
    
        	<copy>BEGIN
        		DVSYS.DBMS_MACADM.UPDATE_REALM(
        			realm_name => 'PROTECT_MYHRAPP'
        			,description => 'A mandatory realm, in simulation mode, to show how EMPLOYEESEARCH_PROD would be protected'
        			,enabled => DBMS_MACUTL.G_YES
        			,audit_options => DBMS_MACUTL.G_REALM_AUDIT_FAIL
        			,realm_type => 1); 
        	END;
        	/</copy>
        
    
    ![強制モード](images/enforcement-mode.png)
    

_ノート_: 更新が有効になるまで数分かかる場合があります。

## タスク2: DBA\_DEBRAおよびADMINを使用して`EMPLOYEESEARCH_PROD`のデータを問い合せることができないことを示します。

1.  データベース・アクションからログアウトし、`DBA_DEBRA`として再度ログインします。次のコマンドをコピーして貼り付け、`EMPLOYEESEARCH_PROD`内のオブジェクトを問い合せます。
    
        	<copy>alter session set current_schema = EMPLOYEESEARCH_PROD;</copy>
        
    
        	<copy>select a.USERID, a.FIRSTNAME, a.LASTNAME, a.EMAIL, a.PHONEMOBILE, a.PHONEFIX, a.PHONEFAX, a.EMPTYPE, a.POSITION, a.ISMANAGER, a.MANAGERID, a.DEPARTMENT, a.CITY, a.STARTDATE, a.ENDDATE, a.ACTIVE, a.COSTCENTER, b.FIRSTNAME as MGR_FIRSTNAME, b.LASTNAME as MGR_LASTNAME, b.USERID as MGR_USERID from DEMO_HR_EMPLOYEES a left outer join DEMO_HR_EMPLOYEES b on a.MANAGERID = b.USERID where 1=1 order by a.LASTNAME, a.FIRSTNAME</copy>
        
    
    ![Debraの権限が不足しています](images/debra-insufficient.png)
    
    `DBA_DEBRA`に、レルムによって保護され、`EMPLOYEESEARCH_PROD`によって所有されているデータベース・オブジェクトを問い合せる権限がなくなったことに注目してください。
    
2.  Cloud Shellを開きます。次のコマンドを使用して、`dv_query_employee_search.sh`スクリプトを`ADMIN`として実行します。
    
    _ノート: アクティブでないためにGlassfishインスタンスからログアウトしている場合は、次のコマンドを使用して再度ログインします。パブリックIPアドレスは、Oracle Cloudのインスタンス詳細ダッシュボードにあります。_
    
        	<copy>chmod 600 myhrappkey</copy>
        
    
        <copy>ssh -i myhrappkey opc@<PASTE INSTANCE PUBLIC IP ADDRESS HERE></copy>
        
    
    _dv\_query\_employee\_search.shを実行するためのコマンドを次に示します。_
    
        	<copy>cd secure-legacy-app-db-vault</copy>
        
    
        	<copy>./dv_query_employee_search.sh</copy>
        
    
    ![管理者には権限がありません](images/admin-insufficient.png)
    
    `ADMIN`でも、レルムによって保護され、`EMPLOYEESEARCH_PROD`によって所有されているデータベース・オブジェクトを問い合せる権限がなくなったことに注意してください。
    
3.  クラウド・シェルを最小化します。**「データベース・アクション」**を見つけて、`DBA_DEBRA`からログアウトします。`ADMIN`として再度ログインし、次のコマンドをコピーして貼り付けて、`EMPLOYEESEARCH_PROD`のオブジェクトを問い合せます。
    
        	<copy>alter session set current_schema = EMPLOYEESEARCH_PROD;</copy>
        
    
        	<copy>select a.USERID, a.FIRSTNAME, a.LASTNAME, a.EMAIL, a.PHONEMOBILE, a.PHONEFIX, a.PHONEFAX, a.EMPTYPE, a.POSITION, a.ISMANAGER, a.MANAGERID, a.DEPARTMENT, a.CITY, a.STARTDATE, a.ENDDATE, a.ACTIVE, a.COSTCENTER, b.FIRSTNAME as MGR_FIRSTNAME, b.LASTNAME as MGR_LASTNAME, b.USERID as MGR_USERID from DEMO_HR_EMPLOYEES a left outer join DEMO_HR_EMPLOYEES b on a.MANAGERID = b.USERID where 1=1 order by a.LASTNAME, a.FIRSTNAME</copy>
        
    
    ![管理データベース・アクション](images/admin-db-actions.png)
    
    `ADMIN`では、**データベース・アクション**内で`EMPLOYEESEARCH_PROD`によって保護されているオブジェクトを問い合せることもできません。
    

おめでとうございます!これで、Oracle CloudでレガシーHRアプリケーションを正常に移行して保護できました。

**次の演習に進みます。**

## 謝辞

*   **著者**\- 北米スペシャリスト・ハブ、Ethan Shmargad氏
*   **作成者** - シニア・プリンシパル・プロダクト・マネージャー、Richard Evans
*   **最終更新者/日付** - Ethan Shmargad、2022年9月