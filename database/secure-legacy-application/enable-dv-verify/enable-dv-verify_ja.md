# Database Vaultを有効にし、HRアプリケーションを確認します。

## 概要

この演習では、ATPインスタンスでDatabase Vaultを有効にし、ATPインスタンスを再起動してその変更を反映します。次に、Glassfishアプリケーションをもう一度見て、すべてが順調に機能していることを確認します。

### 目標

この演習では、次のタスクを実行します。

*   ATPインスタンスでDatabase Vaultを有効にします**(再起動が必要です)**。
*   HRアプリケーションが引き続き機能することを確認します。

### 前提条件

この演習では、次を想定しています。

*   Oracle Cloud Infrastructure (OCI)テナンシ・アカウント
*   前の演習の完了: Autonomous Databaseインスタンスの構成、レガシーGlassfish HRアプリケーションへの接続、Glassfishアプリケーションのデータのロードおよび検証

## タスク1: ATPインスタンスでのDatabase Vaultの有効化

1.  Cloud Shellコンソールを最小化します。右上のハンバーガー・メニューを使用して、**「Oracle Database」→「Autonomous Database」**に移動します。正しいコンパートメント内で、Glassfishアプリケーション用に作成したATPインスタンスを選択します。
    
2.  **「データベース・アクション」**を選択します。
    
    ![データベース・アクションを開く](images/database-actions.png)
    
3.  ATPインスタンスのプロビジョニング時に作成した**ADMIN**資格証明を使用して、データベース・アクションにログインします。
    
    ![ログイン・データベース・アクション](images/db-actions-login.png)
    
4.  **「開発」**セクションで、**「SQL」**を選択します。
    
    ![SQLの選択](images/select-sql.png)
    
5.  **ADMIN**スキーマで、次のコマンドをコピーして貼り付け、**Database Vault所有者**を作成します。**「スクリプトの実行」**ボタンを選択して、文を実行します。ページの下部にある**スクリプト出力**をチェックして、文が正常に実行されたことを確認します。
    
        <copy>
        CREATE USER sec_admin_owen IDENTIFIED BY WElcome_123#;
        GRANT CREATE SESSION TO sec_admin_owen;
        GRANT SELECT ANY DICTIONARY TO sec_admin_owen;
        GRANT AUDIT_ADMIN to sec_admin_owen;
        </copy>
        
    
    ![DVD所有者の作成](images/create-dv-owner.png)
    
    ![DVD実行のチェック](images/check-dv-execution.png)
    
6.  ワークシートをクリアします。**ADMIN**スキーマで、次のコマンドをコピーして貼り付け、**Database Vaultアカウント・マネージャ**を作成します。**「スクリプトの実行」**ボタンを選択して、文を実行します。ページの下部にある**スクリプト出力**をチェックして、文が正常に実行されたことを確認します。
    
        <copy>CREATE USER accts_admin_ace IDENTIFIED BY WElcome_123#;
        GRANT CREATE SESSION TO accts_admin_ace;
        GRANT AUDIT_ADMIN to accts_admin_ace;</copy>
        
7.  ワークシートをクリアします。**ADMIN**スキーマで、次のコマンドをコピーして貼り付け、DBAユーザー`dba_debra`を作成します。**「スクリプトの実行」**ボタンを選択して、文を実行します。ページの下部にある**スクリプト出力**をチェックして、文が正常に実行されたことを確認します。
    
        <copy>
        CREATE USER dba_debra IDENTIFIED by WElcome_123#;
        GRANT PDB_DBA to dba_debra;
        BEGIN
           ORDS_ADMIN.ENABLE_SCHEMA(p_enabled => TRUE, p_schema => UPPER('dba_debra'), p_url_mapping_type => 'BASE_PATH', p_url_mapping_pattern => LOWER('dba_debra'), p_auto_rest_auth => TRUE);
        END;
        /
        </copy>
        
8.  作成したユーザーに対して**SQLワークシート**権限を有効にします。次の各コマンドの実行後にワークシートをクリアし、文が正しく実行されたことを確認します。
    
        <copy>
        BEGIN
           ORDS_ADMIN.ENABLE_SCHEMA(p_enabled => TRUE, p_schema => UPPER('sec_admin_owen'), p_url_mapping_type => 'BASE_PATH', p_url_mapping_pattern => LOWER('sec_admin_owen'), p_auto_rest_auth => TRUE);
           ORDS_ADMIN.ENABLE_SCHEMA(p_enabled => TRUE, p_schema => UPPER('accts_admin_ace'), p_url_mapping_type => 'BASE_PATH', p_url_mapping_pattern => LOWER('accts_admin_ace'), p_auto_rest_auth => TRUE);
           ORDS_ADMIN.ENABLE_SCHEMA(p_enabled => TRUE, p_schema => UPPER('employeesearch_prod'), p_url_mapping_type => 'BASE_PATH', p_url_mapping_pattern => LOWER('employeesearch_prod'), p_auto_rest_auth => TRUE)
        END;
        /
        </copy>
        
9.  ワークシートをクリアします。ここでは、Oracle Database Vaultの構成および有効化プロシージャを実行します。このステップでは、DV関連のロールをデータベースに追加し、sec\_admin\_owenにDV\_OWNERロールを付与し、accts\_admin\_aceにDV\_ACCTMGRロールを付与します。Database Vaultの構成および有効化によるADBインスタンスに対する変更の詳細は、このラボの下部にある**詳細**の項を参照してください。
    
        <copy>EXEC DBMS_CLOUD_MACADM.CONFIGURE_DATABASE_VAULT('sec_admin_owen', 'accts_admin_ace');</copy>
        
    
        <copy>EXEC DBMS_CLOUD_MACADM.ENABLE_DATABASE_VAULT;</copy>
        
10.  `DBA_DV_STATUS`表を問い合せて、Database Vaultの構成ステータスが**trueだが有効化されていない**ことを確認します。ワークシートがクリアされていることを確認し、次の問合せをコピーしてワークシートに貼り付けます。コマンドを実行し、適切な出力を受信したことを確認します。
    

    <copy>SELECT * FROM DBA_DV_STATUS;</copy>
    

11.  「**Autonomous Databaseの詳細**」ページに戻ります。**「他のアクション」**で、**「再起動」**を選択します(これには数分かかる場合があります)。

![ATPの再起動](images/restart-atp.png)

12.  **データベース・アクションSQLワークシート**に戻り、ページをリフレッシュします。ワークシートが明確であることを確認し、次の問合せをコピー/貼り付けます。スクリプト出力をチェックして、`DV_CONFIGURE_STATUS`と`DV_ENABLE_STATUS`の両方が**TRUE**になったことを確認します。

    <copy>SELECT * FROM DBA_DV_STATUS;</copy>
    

## タスク2: HRアプリケーションが引き続き機能していることの確認

1.  Webブラウザを使用してGlassfishアプリケーションに戻り、**本番ページと開発ページの両方**をリフレッシュして、問題なく機能することを確認します。
    
    ![アプリを検証](images/front-page-prod.png)
    
    ![開発アプリを検証](images/front-page-dev.png)
    

**次の演習に進みます。**

## さらに学ぶ

*   [Oracle Database Vaultランディング・ページ](https://www.oracle.com/security/database-security/database-vault/)
*   [Autonomous DatabaseでのOracle Database Vaultの使用](https://docs.oracle.com/en/cloud/paas/autonomous-database/adbsa/autonomous-database-vault.html)

## 確認

*   **著者**\- 北米スペシャリスト・ハブ、Ethan Shmargad氏
*   **作成者** - シニア・プリンシパル・プロダクト・マネージャー、Richard Evans
*   **最終更新者/日付** - Ethan Shmargad、2022年9月