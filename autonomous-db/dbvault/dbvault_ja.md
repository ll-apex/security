# Autonomous Database上のOracle Database Vault

## 概要

このワークショップでは、Oracle Database Vault (DV)の様々な機能について説明します。これにより、権限のない特権ユーザーが機密データにアクセスできないようにAutonomous Databaseでこれらの機能を構成する方法を学習できます。

マネージド・データベース・サービスは「管理スヌーピング」のリスクを冒し、特権ユーザー(特に権限のあるユーザー・アカウント)が機密データにアクセスできるようにします。Oracle Autonomous Database with Database Vaultは強力なセキュリティ制御を提供し、特権データベース・ユーザーによるアプリケーション・データへのアクセスを制限し、内部および外部の脅威のリスクを軽減し、一般的なコンプライアンス要件に対処します。

制御をデプロイして、アプリケーション・データへの特権アカウント・アクセスをブロックし、データベース内の機密操作を制御できます。信頼できるパスを使用して、許可されたデータ・アクセスおよびデータベースの変更にセキュリティ制御を追加できます。セキュリティを強化するために、IPアドレス、ユーザー名、クライアント・プログラム名およびその他の要因をOracle Database Vaultセキュリティ制御の一部として使用できます。**Oracle Database Vaultは、既存のデータベース環境を透過的に保護し、コストと時間のかかるアプリケーション変更を排除します。**

_見積時間:_ 35分

_このラボでテストされたバージョン:_ Oracle Autonomous Database 19c

### ビデオ・プレビュー

「_LiveLabs - Database VaultによるAutonomous Databasesでの不正なデータ・アクセスの防止(2022年5月)_」のプレビューを見る[](youtube:xKq0a_dwM1Y)

ラボのクイック・ウォークスルーについては、次のビデオをご覧ください。[Autonomous DatabaseでのOracle Database Vault](videohub:1_hpawkio9)

### 目標

Oracle Databaseボールトは、Autonomous Databaseに事前にインストールされています。この演習では、次のことを行います。

*   Autonomous DatabaseでのDatabase Vaultの有効化
*   Database Vaultレルムを使用した機密データの保護
*   レルム違反を取得するための監査ポリシーの作成
*   シミュレーション・モードでのDatabase Vaultコントロールのテスト

機密情報を含み、スキーマ所有者(**ユーザー`SH1`**)やDBA(**ユーザー`DBA_DEBRA`**)などの特権ユーザーから保護する必要がある`CUSTOMERS`表や`COUNTRIES`表などの複数の表を含む`SH1`スキーマを使用します。ただし、これらの表のデータは、アプリケーション・ユーザー(**ユーザー`APPUSER`**)が使用できる必要があります。

![](./images/adb-dbv_001.png " ")

**ノート:**

*   **このワークショップでは、DVの構成/有効化/無効化コマンド構文はAutonomous Database共有専用です。**
*   Autonomous Database専用、Exadata Cloud Service、データベース・システム、オンプレミス・データベースなどの他のOracle Databaseデプロイメントでは、若干異なる構文が使用されます。

### 前提条件

この演習では、次を想定しています。

*   Free Tier、有料またはLiveLabs Oracle Cloudアカウント
*   前に「環境の準備」ステップを完了しました

### ラボのタイミング(推定)

| タスク番号 | 機能 | 近似時間 |
| --- | --- | --- |
| 1 | DatabaseVaultの有効化 | 5分未満 |
| 2 | 職務分掌の有効化(SoD) | 5分未満 |
| 3 | 単純なレルムの作成 | 10分間 |
| 4 | レルム違反を取得するための監査ポリシー | 5分 |
| 5 | シミュレーション・モード | 10分間 |
| 6 | DatabaseVaultの無効化 | 5分未満 |

## タスク1: Database Vaultの有効化

まず、2つのDVユーザー・アカウントを作成します。

*   **Database Vault所有者(`SEC_ADMIN_OWEN`)**
    *   このユーザーはDVオブジェクトの所有者として必須です
    *   `SEC_ADMIN_OWEN`には`DV_OWNER`および`DV_ADMIN`ロールがあり、データベース・ボールト・ポリシーを構成できます
*   **Database Vaultアカウント・マネージャ(`ACCTS_ADMIN_ACE`)**
    *   このユーザーはオプションですが、推奨ロールです。
    *   `ACCTS_ADMIN_ACE`には`DV_ACCTMGR`ロールがあり、ユーザーを作成してユーザー・パスワードを変更できます。
*   DV所有者はDVアカウント・マネージャにもなることができますが、Oracleでは2つの異なるアカウントを使用して職務の分離を維持することをお薦めします。

1.  _`ADMIN`_ユーザーとしてAutonomous DBでSQLワークシートを開きます
    
    *   Oracle Cloud Infrastructure (OCI)で、「環境の準備」ステップで作成した`ADB Security`データベースを選択します
        
        ![](./images/adb-dbv_002.png " ")
        
    *   `ADB Security`データベースの詳細ページで、**「ツール」**タブをクリックします。
        
        ![](../prepare-setup/images/adb-set_010.png " ")
        
    *   「ツール」ページでは、Autonomous Databaseのデータベース管理ツールおよび開発者ツール(データベース・アクション、Oracle Application Express、Oracle MLユーザー管理およびSODAドライバ)にアクセスできます。「データベース・アクション」ボックスで、\[**データベース・アクションを開く**\]をクリックします
        
        ![](../prepare-setup/images/adb-set_011.png " ")
        
    *   データベース・アクションのサインイン・ページが開きます。この演習では、データベース・インスタンスのデフォルトの管理者アカウント(ユーザー_`admin`_)を使用して\[**次へ**\]をクリックします。
        
        ![](../prepare-setup/images/adb-set_012.png " ")
        
    *   データベースの作成時に指定した管理パスワードを入力します(_`WElcome_123#`_を参照)
        
            <copy>WElcome_123#</copy>
            
    *   \[**サインイン**\]をクリックします
        
        ![](../prepare-setup/images/adb-set_013.png " ")
        
    *   「データベース・アクション」ページが開きます。「開発」ボックスで、\[**SQL**\]をクリックします。
        
        ![](../prepare-setup/images/adb-set_014.png " ")
        
2.  Database Vault所有者およびアカウント・マネージャ・ユーザーの作成
    
        <copy>
        -- Create DV owner
        CREATE USER sec_admin_owen IDENTIFIED BY WElcome_123#;
        GRANT CREATE SESSION TO sec_admin_owen;
        GRANT SELECT ANY DICTIONARY TO sec_admin_owen;
        GRANT AUDIT_ADMIN to sec_admin_owen;
        
        -- Create DV account manager
        CREATE USER accts_admin_ace IDENTIFIED BY WElcome_123#;
        GRANT CREATE SESSION TO accts_admin_ace;
        GRANT AUDIT_ADMIN to accts_admin_ace;
        
        -- Enable SQL Worksheet for the users just created
        BEGIN
           ORDS_ADMIN.ENABLE_SCHEMA(p_enabled => TRUE, p_schema => UPPER('sec_admin_owen'), p_url_mapping_type => 'BASE_PATH', p_url_mapping_pattern => LOWER('sec_admin_owen'), p_auto_rest_auth => TRUE);
           ORDS_ADMIN.ENABLE_SCHEMA(p_enabled => TRUE, p_schema => UPPER('accts_admin_ace'), p_url_mapping_type => 'BASE_PATH', p_url_mapping_pattern => LOWER('accts_admin_ace'), p_auto_rest_auth => TRUE);
        END;
        /
        </copy>
        
    
    **ノート:** - 次のSQL問合せをSQLワークシートにコピー/ペースト- \[**F5**\]を押すか、「スクリプトの実行」アイコンをクリックします- エラーがないことを確認します
    
        ![](./images/adb-dbv_003.png " ")
        
3.  Database Vaultユーザー・アカウントの構成
    
        <copy>EXEC DBMS_CLOUD_MACADM.CONFIGURE_DATABASE_VAULT('sec_admin_owen', 'accts_admin_ace');</copy>
        
    
    ![](./images/adb-dbv_004.png " ")
    
4.  Database Vaultが構成されているがまだ有効になっていないことを確認します。
    
        <copy>SELECT * FROM DBA_DV_STATUS;</copy>
        
    
    ![](./images/adb-dbv_005.png " ")
    
    **ノート:** `DV_CONFIGURE_STATUS`は**TRUEである必要があります**
    
5.  次に、Database Vaultを有効にします。
    
        <copy>EXEC DBMS_CLOUD_MACADM.ENABLE_DATABASE_VAULT;</copy>
        
    
        ![](./images/adb-dbv_006.png " ")
        
6.  Database Vaultの有効化プロセスを完了するには、データベースを再起動する必要があります。
    
    *   次に示すように、「その他のアクション」ドロップリストで**「再起動」**を選択して、コンソールからデータベースを再起動します。
        
        ![](./images/adb-dbv_007.png " ")
        
    *   再起動が完了したら、`ADMIN`ユーザーとしてSQLワークシートに戻り、DVが有効になっていることを確認します
        
            <copy>SELECT * FROM DBA_DV_STATUS;</copy>
            
        
        ![](./images/adb-dbv_008.png " ")
        
    
    **ノート:** `DV_ENABLE_STATUS`は**TRUEである必要があります**
    
7.  これで、Database Vaultが有効になりました。
    

## タスク2: 職務の分離の有効化(SoD)

Autonomous DBでは、`ADMIN`ユーザーには、Database Vaultセキュリティ・ポリシーの管理に必要な権限を含むすべての権限があります。実際の状況では、セキュリティ管理、ユーザー管理、およびデータベース管理を異なるアカウントに分離できます。これからは、`ADMIN`ユーザーのかわりにDBAアカウントを使用します。これは、本番環境で実行する必要があるためです。

「環境の準備」ステップで、ユーザー`DBA_DEBRA`を作成しました。このユーザーは、Autonomous DBで`DBA`ロールを持っています

1.  DBAアカウントに対するDB Vault SoDの影響を示すには、_`DBA_DEBRA`_ユーザーとしてSQLワークシートを開きます(リマインダとして、パスワードは`WElcome_123#`です)。
    
        <copy>WElcome_123#</copy>
        
2.  `DBA_DEBRA`のロールの表示
    
        <copy>SELECT * FROM session_roles ORDER BY 1;</copy>
        
    
        ![](./images/adb-dbv_009.png " ")
        
    
    **ノート:** DBA\_DEBRAには、`PDB_DBA` (Autonomous DBのDBAロール)を含む複数のロールがあることに注意してください
    
3.  テスト・ユーザー`DEMO1`を作成します。
    
        <copy>CREATE USER demo1;</copy>
        
    
        ![](./images/adb-dbv_010.png " ")
        
    
    **ノート:** `DBA_DEBRA`は、`DBA`ロールを持っているにもかかわらず、**Database Vaultが有効になっているため**ユーザーを作成できないことに注意してください。
    
4.  ユーザー`APPUSER`を変更してみましょう
    
        <copy>ALTER USER appuser IDENTIFIED BY WElcome_123456#;</copy>
        
    
        ![](./images/adb-dbv_011.png " ")
        
        **Note:** `DBA_DEBRA` can no longer change a user's passwords!
        
5.  実際、**DVを有効にすると、すぐに職務分離の強制が開始されます** - ユーザー`DBA_DEBRA`はDBユーザー・アカウントを作成/変更/削除する機能を失い、その権限はDVアカウント・マネージャ・ロールを使用します。
    
6.  演習を続行する場合は、すべてのデータベース・ボールト・アクションに`SEC_ADMIN_OWEN`および`ACCTS_ADMIN_ACE`を使用します。データベース管理(`DBA_DEBRA`で実行)の職務は、ユーザー管理(`ACCTS_ADMIN_ACE`)およびセキュリティ管理(`SEC_ADMIN_OWEN`)の職務とは別のものになりました。
    

## タスク3: 単純なレルムの作成

次に、`DBA_DEBRA`および`SH1` (表所有者)によるアクセスから`SH1.CUSTOMERS`表を保護し、`APPUSER`へのアクセスのみを付与するレルムを作成します。

レルムは、データベース・スキーマ、オブジェクトおよびロールを保護できるデータベース内の保護ゾーンです。たとえば、会計、販売または人事に関連するスキーマ、オブジェクトおよびロールのセットを保護できます。これらをレルムに保護した後、そのレルムを使用して、特定のアカウントまたはロールによるシステム権限およびオブジェクト権限の使用を制御できます。これにより、これらのスキーマ、オブジェクトおよびロールを使用するすべてのユーザーに、コンテキスト依存のアクセス制御を適用できます。

1.  このレルムの効果を示すには、レルムの作成前と作成後に、これらの3人のユーザーから同じSQL問合せを実行することが重要です。
    
    *   続行するには、前のタスク1に示すように、別のユーザー(_`DBA_DEBRA`_、_`SH1`_および_`APPUSER`_)と接続された**3つのWebブラウザ・ページでSQLワークシートを開きます**。
        
        **注:** - 標準のブラウザーウィンドウで同時に開けるSQLワークシートセッションは1つだけなので、**各セッションを新しいWebブラウザウィンドウ(Mozilla、Chrome、Edge、Safariなど)で開くか、もしくは「認知モード」で開くことができます!** - リマインダとして、これらのユーザーのパスワードは同じです(`WElcome_123#`を参照)
        
               ````
               <copy>WElcome_123#</copy>
               ````
            
    *   次の問合せのコピー/貼付けおよび実行
        
            <copy>
               SELECT cust_id, cust_first_name, cust_last_name, cust_email, cust_main_phone_number
                 FROM sh1.customers
                WHERE rownum < 10;
            </copy>
            
        
        *   ユーザー"**`DBA_DEBRA`**"として
            
            ![](./images/adb-dbv_012.png " ")
            
        *   ユーザー"**`SH1`**"として
            
            ![](./images/adb-dbv_013.png " ")
            
        *   ユーザー"**`APPUSER`**"として
            
            ![](./images/adb-dbv_014.png " ")
            
        
        **Note:** - **These 3 users can see the `SH1.CUSTOMERS` table!** - `SH1` because `SH1` owns it - `DBA_DEBRA` because it has the DBA role - `APPUSER` because it have the "`READ ANY TABLE`" system privilege
        
2.  次に、_`SEC_ADMIN_OWEN`_ユーザーとして次の問合せを実行して、`SH1`表を保護するレルムを作成します。そのため、**第4のWebブラウザ・ウィンドウを開く**ようにしてください
    
        <copy>
        -- Create the "PROTECT_SH1" DV realm
           BEGIN
              DVSYS.DBMS_MACADM.CREATE_REALM(
                  realm_name => 'PROTECT_SH1'
                  ,description => 'A mandatory realm to protect SH1 tables'
                  ,enabled => DBMS_MACUTL.G_YES
                  ,audit_options => DBMS_MACUTL.G_REALM_AUDIT_FAIL
                  ,realm_type => 1); 
           END;
           /
        
        -- Show the current DV realm
        SELECT name, description, enabled FROM dba_dv_realm WHERE id# >= 5000 ORDER BY 1;
        </copy>
        
    
        ![](./images/adb-dbv_015.png " ")
        
    
    **ノート:** - レルム`PROTECT_SH1`が**必須として作成され、有効になりました**! - **必須レルムと通常のレルム**の違いは、通常のレルム・ブロック・システム権限(および直接オブジェクト権限を許可)ですが、必須レルムはシステム権限に加えて、直接オブジェクト権限(オブジェクト所有者でも)をブロックします。
    
3.  保護するレルムにオブジェクトを追加します(ここでは、`CUSTOMERS`表)。
    
        <copy>
        -- Set SH1 objects as protected by the DV realm "PROTECT_SH1"
           BEGIN
               DVSYS.DBMS_MACADM.ADD_OBJECT_TO_REALM(
                   realm_name   => 'PROTECT_SH1',
                   object_owner => 'SH1',
                   object_name  => 'CUSTOMERS',
                   object_type  => 'TABLE');
           END;
           /
        
        -- Show the objects protected by the DV realm PROTECT_SH1
        SELECT realm_name, owner, object_name, object_type
          FROM dvsys.dba_dv_realm_object
         WHERE realm_name IN (SELECT name FROM dvsys.dv$realm WHERE id# >= 5000);
        </copy>
        
    
    ![](./images/adb-dbv_016.png " ")
    
        **Note:** Now the table `CUSTOMERS` is protected and no one can access on it!
        
4.  このレルムの影響を確認してください
    
    *   3人のユーザー(_`DBA_DEBRA`_、_`SH1`_および_`APPUSER`_)ごとにSQL Worsheetで次の問合せを再度実行します。
    
        <copy>
           SELECT cust_id, cust_first_name, cust_last_name, cust_email, cust_main_phone_number
             FROM sh1.customers
            WHERE rownum < 10;
        </copy>
        
    
        - as user "**`DBA_DEBRA`**"
        
           ![](./images/adb-dbv_017.png " ")
        
        - as user "**`SH1`**"
        
           ![](./images/adb-dbv_018.png " ")
        
        - as user "**`APPUSER`**"
        
           ![](./images/adb-dbv_019.png " ")
        
        - **Objects in the realm cannot be accessed by any database users**, including the DBA (`DBA_DEBRA`) and the schema owner (`SH1`)!
        
5.  ここで、_`SEC_ADMIN_OWEN`_ユーザーとしてSQLワークシートに戻り、この問合せを実行して、レルムに認可されたアプリケーション・ユーザー(`APPUSER`)があることを確認します。
    
        <copy>
        -- Grant access to APPUSER only for the DV realm "PROTECT_SH1"
           BEGIN
               DVSYS.DBMS_MACADM.ADD_AUTH_TO_REALM(
                   realm_name   => 'PROTECT_SH1',
                   grantee      => 'APPUSER');
           END;
           /
        </copy>
        
    
    ![](./images/adb-dbv_020.png " ")
    
6.  SQL問合せを再実行して、`APPUSER`のみがデータを読み取ることができるようになったことを示します。
    
        <copy>
           SELECT cust_id, cust_first_name, cust_last_name, cust_email, cust_main_phone_number
             FROM sh1.customers
            WHERE rownum < 10;
        </copy>
        
    
        - as user "**`DBA_DEBRA`**"
        
           ![](./images/adb-dbv_017.png " ")
        
        - as user "**`SH1`**"
        
           ![](./images/adb-dbv_018.png " ")
        
        - as user "**`APPUSER`**"
        
           ![](./images/adb-dbv_014.png " ")
        
        - **`APPUSER` must be the only user who has access to the table from now!**
        

## タスク4: レルム違反を取得するための監査ポリシーの作成

レルム・オブジェクトへの不正アクセス試行の監査証跡を取得することもできます。Autonomous Databaseには統合監査が含まれているため、データベース・ボールト・アクティビティを監査するポリシーを作成します

1.  _`ACCTS_ADMIN_ACE`_ユーザーとしてSQLワークシートを開きます(リマインダとして、パスワードは`WElcome_123#`です)。
    
        <copy>WElcome_123#</copy>
        
2.  監査証跡ログが存在しないことを確認します。
    
        <copy>
        -- Display the audit trail log
        SELECT os_username, dbusername, event_timestamp, action_name, sql_text 
          FROM unified_audit_trail
         WHERE DV_ACTION_NAME='Realm Violation Audit' order by 3;
        </copy>
        
    
    ![](./images/adb-dbv_021.png " ")
    
    **ノート:**問合せでは行が戻されません。
    
3.  ステップ2で前に作成したDVレルム`PROTECT_SH1`に監査ポリシーを作成します。
    
        <copy>
        -- Create the Audit Policy
           CREATE AUDIT POLICY dv_realm_sh1
              ACTIONS SELECT, UPDATE, DELETE
              ACTIONS COMPONENT=DV Realm Violation ON "PROTECT_SH1";
        
        -- Enable the Audit Policy
           AUDIT POLICY dv_realm_sh1;
        </copy>
        
    
    ![](./images/adb-dbv_022.png " ")
    
4.  ステップ2と同様に、監査の影響を確認します。
    
    *   続行するには、別のユーザー(_`DBA_DEBRA`_、_`SH1`_および_`APPUSER`_)と接続された**3つのWebブラウザ・ウィンドウで開いた3つの異なるSQLワークシートで同じSQL問合せを再実行します**
        
        **注:** - 標準のブラウザーウィンドウで同時に開けるSQLワークシートセッションは1つだけなので、**各セッションを新しいWebブラウザウィンドウ(Mozilla、Chrome、Edge、Safariなど)で開くか、もしくは「認知モード」で開くことができます!** - リマインダとして、これらのユーザーのパスワードは同じです(`WElcome_123#`を参照)
        
               ````
               <copy>WElcome_123#</copy>
               ````
            
    *   次の問合せのコピー/貼付けおよび実行
        
            <copy>
               SELECT cust_id, cust_first_name, cust_last_name, cust_email, cust_main_phone_number
                 FROM sh1.customers
                WHERE rownum < 10;
            </copy>
            
        
        *   ユーザー"**`DBA_DEBRA`**"として
        
        ![](./images/adb-dbv_017.png " ")
        
        *   ユーザー"**`SH1`**"として
        
        ![](./images/adb-dbv_018.png " ")
        
        *   ユーザー"**`APPUSER`**"として
        
        ![](./images/adb-dbv_014.png " ")
        
        *   `DBA_DEBRA`**および**`SH1`**ユーザーは**`SH1.CUSTOMERS`**表にアクセスできず、ポリシーに違反しようとして失敗した監査レコードを生成する必要があります。**
5.  レルム違反監査証跡を確認するには、_`ACCTS_ADMIN_ACE`_ユーザーとしてSQLワークシートに戻ります。
    
        <copy>
        -- Display the audit trail log
        SELECT os_username, dbusername, event_timestamp, action_name, sql_text 
          FROM unified_audit_trail
         WHERE DV_ACTION_NAME='Realm Violation Audit' order by 3;
        </copy>
        
    
    ![](./images/adb-dbv_023.png " ")
    
    **ノート:** `DBA_DEBRA`および`SH1`の失敗した試行が表示されます。
    
6.  この演習を完了したら、_`SEC_ADMIN_OWEN`_ユーザーとしてサインインして監査設定をリセットします。
    
        <copy>
        -- Show the current DV realm
        SELECT name, description, enabled FROM dba_dv_realm WHERE id# >= 5000 order by 1;
        
        -- Purge the DB Vault audit trail logs
        DELETE FROM DVSYS.AUDIT_TRAIL$;
        
        -- Purge the audit trail logs
        BEGIN
            DBMS_AUDIT_MGMT.CLEAN_AUDIT_TRAIL(
               audit_trail_type         =>  DBMS_AUDIT_MGMT.AUDIT_TRAIL_UNIFIED,
               use_last_arch_timestamp  =>  FALSE);
        END;
        /
        
        -- Display the audit trail log
        SELECT os_username, dbusername, event_timestamp, action_name, sql_text 
          FROM unified_audit_trail
         WHERE DV_ACTION_NAME='Realm Violation Audit' order by 3;
        
        -- Disable the audit policy
        NOAUDIT POLICY dv_realm_sh1;
        
        -- Drop the audit policy
        DROP AUDIT POLICY dv_realm_sh1;
        
        </copy>
        
    
    ![](./images/adb-dbv_024.png " ")
    
7.  これで、ポリシーとDVレルムを監査できなくなりました。
    

## タスク5: シミュレーション・モード

シミュレーション・モードを使用して、`SH1.COUNTRIES`表へのトラステッド・パス接続に使用するファクタを検索します。これを行うには、表へのアクセスを完全に無効にし、レルム・ポリシーをシミュレーション・モードにします。シミュレーション・モードでは実際のSQLコマンドはブロックされないため、SQLコマンドは機能します。ただし、SQLコマンドが新しいコマンド・ルールによってブロックされている場合は、シミュレーション・モードでエントリが作成されます。次に、シミュレーション・ログを確認して、正しい違反とファクタおよび関連するルールを取得したかどうかを確認できます。

1.  _`SEC_ADMIN_OWEN`_ユーザーとしてSQLワークシートを開きます(リマインダとして、パスワードは`WElcome_123#`です)。
    
        <copy>WElcome_123#</copy>
        
2.  まず、シミュレーション・ログを問い合せて、現在の値がないことを確認します。
    
        <copy>
           SELECT violation_type, username, machine, object_owner, object_name, command, dv$_module
           FROM dba_dv_simulation_log;
        </copy>
        
    
    ![](./images/adb-dbv_025.png " ")
    
3.  次に、`SH1.COUNTRIES`表からのすべての`SELECT`のブロックをシミュレートする**コマンド・ルール**を作成します
    
        <copy>
        BEGIN
            DBMS_MACADM.CREATE_COMMAND_RULE(
               command 	 => 'SELECT',
               rule_set_name   => 'Disabled',
               object_name       => 'COUNTRIES',
               object_owner       => 'SH1',
               enabled         => DBMS_MACUTL.G_SIMULATION);
        END;
        /
        </copy>
        
    
    ![](./images/adb-dbv_026.png " ")
    
4.  ステップ2のように、シミュレーションの効果を見てみましょう
    
    *   続行するには、別のユーザー(_`DBA_DEBRA`_、_`SH1`_および_`APPUSER`_)と接続された**3つのWebブラウザ・ページで開かれた3つの異なるSQLワークシートで同じSQL問合せを再実行します**
        
        **注:** - 標準のブラウザーウィンドウで同時に開けるSQLワークシートセッションは1つだけなので、**各セッションを新しいWebブラウザウィンドウ(Mozilla、Chrome、Edge、Safariなど)で開くか、もしくは「認知モード」で開くことができます!** - リマインダとして、これらのユーザーのパスワードは同じです(`WElcome_123#`を参照)
        
               ````
               <copy>WElcome_123#</copy>
               ````
            
    *   次のSELECT問合せをSH1にコピー/貼り付けて、何度か実行します。COUNTRIES表
        
            <copy>
               SELECT * FROM sh1.countries WHERE rownum < 20;
            </copy>
            
        
        *   ユーザー"**`DBA_DEBRA`**"として
        
        ![](./images/adb-dbv_027.png " ")
        
        *   ユーザー"**`SH1`**"として
        
        ![](./images/adb-dbv_028.png " ")
        
        *   ユーザー"**`APPUSER`**"として
        
        ![](./images/adb-dbv_029.png " ")
        
        *   **すべてのユーザーが**`SH1.CUSTOMERS`**表にアクセスできます。**
5.  次に、_`SEC_ADMIN_OWEN`_ユーザーとしてSQLワークシートに戻り、新しいエントリを確認します。ブロッキング・ユーザー選択をシミュレートするコマンド・ルールを作成したことを覚えておいてください。
    
        <copy>
           SELECT violation_type, username, machine, object_owner, object_name, command, dv$_module
           FROM dba_dv_simulation_log;
        </copy>
        
    
    ![](./images/adb-dbv_030.png " ")
    
    **ノート:**
    
    *   各ユーザーが結果を表示できますが、ログには、ルールによって選択され、ブロックされる予定のすべてのユーザーが表示されます。
    *   また、接続元の場所と、接続に使用したクライアントも表示されます。
6.  次の演習に進む前に、シミュレーション・ログをクリーン・アウトし、コマンド・ルールを削除します
    
        <copy>
        -- Purge simulation logs
        DELETE FROM DVSYS.SIMULATION_LOG$;
        
        -- Current simulation logs after purging
        SELECT count(*) FROM dba_dv_simulation_log;
        </copy>
        
    
    ![](./images/adb-dbv_031.png " ")
    
        <copy>
        -- Delete the Command Rule
        BEGIN
            DBMS_MACADM.DELETE_COMMAND_RULE(
               command        => 'SELECT', 
               object_owner   => 'SH1', 
               object_name    => 'COUNTRIES',
               scope          => DBMS_MACUTL.G_SCOPE_LOCAL);
        END;
        /
        </copy>
        
    
    ![](./images/adb-dbv_032.png " ")
    

## タスク6: Database Vaultの無効化

1.  _`SEC_ADMIN_OWEN`_ユーザーとしてログを記録し、既存のDVレルムを削除します
    
        <copy>
        -- Drop the "PROTECT_SH1" DV realm
        BEGIN
            DVSYS.DBMS_MACADM.DELETE_REALM_CASCADE(realm_name => 'PROTECT_SH1');
        END;
        /
        
        -- Show the current DV realm
        SELECT name, description, enabled FROM dba_dv_realm WHERE id# >= 5000 order by 1;
        
        </copy>
        
    
    ![](./images/adb-dbv_060.png " ")
    
2.  次に、Autonomous DatabaseでDB Vaultを無効にします
    
        <copy>EXEC DBMS_CLOUD_MACADM.DISABLE_DATABASE_VAULT;</copy>
        
    
    ![](./images/adb-dbv_061.png " ")
    
3.  Database Vaultの有効化プロセスを完了するには、データベースを再起動する必要があります。
    
    *   次に示すように、「その他のアクション」ドロップリストで**「再起動」**を選択して、コンソールからデータベースを再起動します。
        
        ![](./images/adb-dbv_007.png " ")
        
    *   再起動が完了したら、_`DBA_DEBRA`_ユーザーとしてSQLワークシートにログインし、DVが有効になっていることを確認します
        
            <copy>SELECT * FROM DBA_DV_STATUS;</copy>
            
        
        ![](./images/adb-dbv_062.png " ")
        
    
    **ノート:** `DV_ENABLE_STATUS`は**FALSEである必要があります**
    
4.  次に、Database Vaultの所有者およびアカウント・マネージャのユーザーを削除します。
    
        <copy>
        DROP USER sec_admin_owen;
        DROP USER accts_admin_ace;
        </copy>
        
    
        ![](./images/adb-dbv_063.png " ")
        
    
    **ノート:** DB Vautが無効になっているため、SoDも自動的に無効になり、`DBA_DEBRA`ユーザーを持つユーザーを削除できます。
    
5.  Database Vaultが正しく無効になりました。
    

次の演習に進むことができます。

## **付録**: 製品について

### **概要**

Oracle Database Vaultは、認可されていない特権ユーザーが機密データにアクセスできないようにするための制御を提供します。また、認可されていないデータベース変更も防止します。

Oracle Database Vaultのセキュリティ制御は、アプリケーション・データを不正アクセスから守るとともに、プライバシおよび規制の要件への準拠に役立ちます。

![](./images/dv-concept.png " ")

制御をデプロイして、特権アカウントによるアプリケーション・データへのアクセスをブロックしたり、Database Vaultを使用してデータベース内の機密操作を制御できます。

権限およびロールの分析により、最小権限のベスト・プラクティスを使用することで、既存のアプリケーションのセキュリティを強化できます。

Oracle Database Vaultは、既存のデータベース環境を透過的に保護するため、コストと時間がかかります。

Oracle Database Vaultを使用して一連のコンポーネントを作成し、データベース・インスタンスのセキュリティを管理できます。

コンポーネントは次のとおりです。

*   **レルム**

レルムは、データベース・スキーマ、オブジェクトおよびロールを保護できるデータベース内の保護ゾーンです。たとえば、会計、販売または人事に関連するスキーマ、オブジェクトおよびロールのセットを保護できます。これらをレルムに保護した後、そのレルムを使用して、特定のアカウントまたはロールに対するシステム権限およびオブジェクト権限の使用を制御できます。これにより、これらのスキーマ、オブジェクトおよびロールを使用するすべてのユーザーにコンテキスト対応のアクセス制御を提供できます。

*   **コマンド・ルール**

コマンド・ルールとは、SELECT、ALTER SYSTEM、データベース定義言語(DDL)文およびデータ操作言語(DML)文などのほぼすべてのSQL文をユーザーが実行できる条件を制御するために作成する特別なセキュリティ・ポリシーです。コマンド・ルールでは、ルール・セットを使用して文が許可されるかどうかを決定します。

*   **ファクタ**

ファクタは、ユーザーの場所、データベースIPアドレス、セッション・ユーザーなどの名前付き変数または属性で、Oracle Database Vaultはアクセス制御の決定のために認識して使用できます。ルールのファクタを使用して、データベースに接続するためのデータベース・アカウントの認可や、データの可視性と管理性を制限する特定のデータベース・コマンドの実行などのアクティビティを制御します。各ファクターには、1つ以上のIDを指定できます。アイデンティティは、ファクタの実際の値です。ファクターは、ファクター検索方法またはそのIDマッピング ロジックに応じて、複数のIDを持つことができます。

*   **ルール**

ルール・セット内のルールは、trueまたはfalseに評価されるPL/SQL式です。複数のルール・セットで同じルールを使用できます。

*   **ルール・セット**

ルール・セットは、レルム認可、コマンド・ルール、ファクタ割当てまたはセキュア・アプリケーション・ロールに関連付けることができる1つ以上のルールの集合です。ルール・セットは、含まれる各ルールの評価および評価タイプ(「すべてTrue」または「いずれかTrue」)に基づいてtrueまたはfalseに評価されます。

*   **アプリケーション・ロールの保護**

Database Vaultセキュア・アプリケーション・ロールは、Oracle Database Vaultルール・セットの評価に基づいて有効にできる特別なOracle Databaseロールです。

Oracle Database Vaultには、これらのコンポーネントを構成できる一連のPL/SQLインタフェースおよびパッケージが用意されています。一般に、最初に実行するステップは、保護するデータベース・スキーマまたはデータベース・オブジェクトで構成されるレルムを作成することです。ルール、ルール・セット、コマンド・ルール、ファクタ、アイデンティティおよびセキュア・アプリケーション・ロールを作成することで、データベースをさらに保護できます。また、これらのコンポーネントが監視および保護するアクティビティに関するレポートを実行できます。

### **Database Vaultを使用する利点**

*   データへのアクセスを最小限に抑えるためにコンプライアンス規制に対応
*   不正使用または侵害された特権ユーザー・アカウントからデータを保護します。
*   データベースに対する柔軟なセキュリティ・ポリシーの設計と適用
*   データベース統合とクラウド環境の懸念に対処して、コストを削減し、機密アプリケーション・データの露出を、本当の知る必要のない人だけ削減します。
*   信頼できるパスを作成して機密データへのアクセスを保護します(詳細は、[完全なDatabase Vaultラボ](https://apexapps.oracle.com/pls/apex/dbpm/r/livelabs/view-workshop?wid=682&clear=180&session=4531599220675)を実行してください)

## 詳細

技術文書:

*   [Oracle Database Vault](https://docs.oracle.com/en/database/oracle/oracle-database/19/dvadm/introduction-to-oracle-database-vault.html#GUID-0C8AF1B2-6CE9-4408-BFB3-7B2C7F9E7284)

ビデオ:

*   _Oracle Database Vaultの理解(2019年3月)_[](youtube:oVidZw7yWIQ)
*   _Oracle Database Vault - ユースケース(Part1)(2019年10月)_[](youtube:aW9YQT5IRmA)
*   _Oracle Database Vault - ユースケース(Part2)(2019年11月)_[](youtube:hh-cX-ubCkY)
*   _Oracle Database Vaultの概要(2021年5月)_[](youtube:vSVr7avZ4Hg)
*   _ADB上のOracle Database Vault - Livelabsのクイック・ウォークスルー_[](youtube:O_Hi2-vZ-zU)

## 謝辞

*   **著者** - Database Security PM、Hakim Loumi氏
*   **貢献者** - Alan Williams、Rene Fontcha
*   **最終更新者/日付** - Hakim Loumi、Database Security PM - 2022年5月