# Oracle統合監査

## 概要

このワークショップでは、Oracle統合監査の機能を紹介します。これにより、ユーザーはこの機能を構成してデータベース・アクティビティを監査する方法を学習できます。

_推定ラボ時間:_ 35分

_この演習でテストしたバージョン:_ Oracle DB 19.19

### ビデオ・プレビュー

「_LiveLabs - Oracle Unified Auditing (May 2022)_」のプレビューを見る[](youtube:bK26Y0TZANY)

### 目標

*   データベースでの統合監査の有効化/無効化
    
*   異なる監査のユースケースを参照
    
    **ノート:**
    
    *   混合モード監査は、新しくインストールされたデータベースのデフォルトの監査です。混合モード監査では、従来の監査機能(リリース12cより前のリリースの監査機能)と新しい監査機能(統合監査)の両方を使用できます。
    *   混合モードは、統合監査を導入することを目的としています。これにより、統合監査がどのように機能するか、およびそのニュアンスとメリットを把握できます。混合モードでは、既存のアプリケーションおよびスクリプトを移行して統合監査を使用できます。純粋な統合監査の使用を決定したら、統合監査オプションを有効にしてoracleバイナリを再リンクし、それをOracleデータベースで実行する唯一の監査機能として有効にします。混合モードに戻す場合は、できます。
    *   この環境では、このOracle Databaseを純粋な統合監査モードにすでに移行しています。

### 前提条件

この演習では、次を想定しています。

*   Free Tier、有料またはLiveLabs Oracle Cloudアカウント
*   次を完了しました:
    *   演習: 設定の準備(_無料層_および_有料テナント_のみ)
    *   演習: 環境設定
    *   演習: 環境の初期化

### ラボのタイミング(推定)

| ステップ番号 | 機能 | 近似時間 |
| --- | --- | --- |
| 1 | 現在の監査設定を表示します | 5分 |
| 2 | 非アプリケーション使用の監査 | 10分 |
| 3 | データベース・ロール使用の監査 | <10分 |
| 4 | Data Pump使用の監査 | <10分 |

## タスク1: 現在の監査設定の表示

1.  OSユーザー**oracle**として、_DBSec-Lab_ VMでターミナル・セッションを開きます
    
        <copy>sudo su - oracle</copy>
        
    
    **ノート**: リモート・デスクトップ・セッションを使用している場合は、デスクトップの_ターミナル_・アイコンをダブルクリックしてセッションを起動します。
    
2.  scriptsディレクトリへ移動します
    
        <copy>cd $DBSEC_LABS/unified-auditing</copy>
        
3.  監査設定の表示
    
        <copy>./ua_current_audit_settings.sh</copy>
        
    *   純粋な統合監査モードではすでに環境が設定されているため、統合監査が**TRUE**に設定されていることがわかります。
        
        ![統合監査](./images/ua-001.png "監査設定の表示")
        
        **ノート**: これは、データベースが純粋な統合監査モードであり、従来の監査機能を使用していないことを意味します。
        
    *   2番目の問合せでは、統合監査ポリシーが存在する数と、各ポリシーに関連付けられている監査関連属性の数が示されます。
        
        ![統合監査](./images/ua-002.png "監査設定の表示")
        
    *   このスクリプトの3番目の問合せでは、どの統合監査ポリシーが**有効化**されているかが示されます。
        
        ![統合監査](./images/ua-003.png "監査設定の表示")
        
        **ノート:**
        
        *   ポリシーが前の問合せに存在するためだけに、ポリシーが有効であることを意味しません。
        *   統合監査ポリシーを使用するのは、次の2ステップのプロセスです。
            *   ポリシーの作成: 監査ポリシー<policy\_name>の作成...
            *   ポリシーの有効化: 監査ポリシー<policy\_name>
    *   4番目の問合せでは、コンテキストに基づく監査が表示されます。
        
        ![統合監査](./images/ua-004.png "監査設定の表示")
        
        **ノート:**
        
        *   `TICKET_ID`という名前の属性を取得する`TICKETINFO`というポリシーが1つあります。
        *   この情報は、`UNIFIED_AUDIT_TRAIL`ビューの`APPLICATION_CONTEXTS`列に表示されます。
4.  `AUDIT_ADMIN`および`AUDIT_VIEWER`ロールを持つユーザーを表示します。
    
        <copy>./ua_who_audit_roles.sh</copy>
        
    
    ![統合監査](./images/ua-005.png "AUDIT_ADMINおよびAUDIT_VIEWERロールを持つユーザーを表示します。")
    
5.  接続先のデータベースの既存の監査レコードを表示します(デフォルトでは、スクリプトは問合せ対象のデータベースとして**pdb1**を選択します)。
    
        <copy>./ua_query_existing_audit_records.sh</copy>
        
    
    ![統合監査](./images/ua-006.png "既存の監査レコードを表示します")
    
6.  最後に、`DBMS_AUDIT_MGMT`パッケージの詳細の一部を示します。
    
        <copy>./ua_dbms_audit_mgmt_settings.sh</copy>
        
    
    ![統合監査](./images/ua-026.png "DBMS_AUDIT_MGMTパッケージの詳細の一部を示します")
    
    **ノート:**
    
    *   ファンクション`DBMS_AUDIT_MGMT.GET_AUDIT_COMMIT_DELAY`は、監査のコミット遅延時間を秒数で戻します。
    *   監査のコミット遅延時間は、監査レコードをデータベース監査証跡にコミットするのにかかる最大時間です。
    *   監査レコードのコミットに、監査のコミット遅延時間で定義された値よりも長い時間かかる場合、監査レコードのコピーがオペレーティング・システム(OS)監査証跡に書き込まれます。

## タスク2: 非アプリケーション使用の監査

この演習では、アプリケーションの外部で`EMPLOYEESEARCH_PROD`オブジェクトを使用しているユーザーを監査します。

1.  信頼できる接続を特定します。Glassfishアプリケーションからアクティビティを生成し、セッション関連情報を表示します。
    
        <copy>./ua_query_employeesearch_usage.sh</copy>
        
    
    ![統合監査](./images/ua-007.png "信頼できる接続を特定")
    
    **ノート**: プロンプトが表示されたら、次に示すように、Glassfishで調査を実行する前に**\[戻る\]**を押さないでください。
    
2.  並行して、Glassfishアプリケーションを使用してデータベースのアクティビティを生成します。
    
    *   _`http://dbsec-lab:8080/hr_prod_pdb1`_へのWebブラウザ・ウィンドウのオープン
        
        **ノート:**リモート・デスクトップを使用していない場合は、_`http://<YOUR_DBSEC-LAB_VM_PUBLIC_IP>:8080/hr_prod_pdb1`_に移動してこのページにアクセスすることもできます
        
    *   _`hradmin`_として、パスワード_`Oracle123`_でHRアプリケーションにログインします。
        
            <copy>hradmin</copy>
            
        
            <copy>Oracle123</copy>
            
        
        ![統合監査](./images/ua-008.png "HRアプリケーションへのログイン")
        
        ![統合監査](./images/ua-009.png "HRアプリケーションへのログイン")
        
    *   **「従業員の検索」**をクリックします。
        
        ![統合監査](./images/ua-010.png "従業員の検索")
        
    *   \[**検索**\]をクリックします
        
        ![統合監査](./images/ua-011.png "従業員の検索")
        
    *   基準の一部を変更し、再度検索してください
        
    *   十分なトラフィックを確保するために、**2-3回繰り返す**
        
3.  端末セッションに戻り、結果を表示する準備ができたら**\[return\]**を押します
    
    ![統合監査](./images/ua-012.png "結果を見る準備ができたら[return]を押します")
    
4.  次に、ホストOSで**SQL\*Plus**からトラフィックを生成する問合せを実行します。
    
        <copy>./ua_query_employeesearch.sh</copy>
        
    
    ![統合監査](./images/ua-013.png "トラフィックを生成")
    
5.  次に、**統合監査ポリシー**を作成します
    
        <copy>./ua_create_audit_policy.sh</copy>
        
    
    ![統合監査](./images/ua-014.png "統合監査ポリシーの作成")
    
    **ノート:**
    
    *   統合監査ポリシーにより、マシン関連の詳細が取得され、**WHEN**句が作成されます
    *   ここでは、基準として`SYS_CONTEXT`変数に基づいて監査ポリシー`AUDIT_EMPLOYEESEARCH_USAGE`を作成しました。
        *   `SESSION_USER = "EMPLOYEESEARCH_PROD"`
        *   **および**`OS_USER != "oracle"`
        *   **または**`MODULE != "JDBC Thin Client"`
        *   **または**`HOST != "dbsec-lab.dbsecvcn.oraclevcn.com"`
    *   この監査ポリシーは、安全でないパス(たとえば、公式Webアプリケーション以外のパス)から`EMPLOYEESEARCH_PROD.DEMO_HR_USERS`表および`EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES`表へのアクセスを試行するすべてのセッションを監査します
6.  統合監査ポリシーの作成後、それを有効にする必要があります。
    
        <copy>./ua_enable_audit_policy.sh</copy>
        
    
    ![統合監査](./images/ua-015.png "統合監査ポリシーの有効化")
    
7.  追加の問合せを実行してトラフィックを生成し、監査レコードが生成されるかどうかを確認します
    
    *   実行
        
            <copy>./ua_query_employeesearch_usage.sh</copy>
            
        
        ![統合監査](./images/ua-007.png "トラフィックを生成")
        
        **ノート**: プロンプトが表示されたら、次に示すように、Glassfishで調査を実行する前に**\[戻る\]**を押さないでください。
        
    *   Glassfishアプリケーションに戻り、**「従業員の検索」**をクリックして新しいアクティビティを生成します
        
        ![統合監査](./images/ua-010.png "Glassfishアプリケーションでの新規アクティビティの生成")
        
    *   \[**検索**\]をクリックします
        
        ![統合監査](./images/ua-011.png "従業員の検索")
        
    *   基準の一部を変更し、再度検索してください
        
    *   十分なトラフィックを確保するために、**2-3回繰り返す**
        
    *   次に、ターミナル・セッションに戻り、**\[return\]**を押します
        
        ![統合監査](./images/ua-016.png "カテゴリを停止")
        
    *   監査ポリシー`AUDIT_EMPLOYEESEARCH_USAGE`の監査出力の結果を表示します
        
            <copy>./ua_query_audit_records.sh</copy>
            
        
        ![統合監査](./images/ua-016b.png "監査出力の結果の表示")
        
        **ノート**: アプリケーション監査データを**除外**しているため、この統合監査ポリシーに基づいて生成しないでください
        
8.  ここで、「安全でない」アクセス・パスを使用して`EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES`表を再度問い合せて監査データを生成します。
    
    *   このSQL\*Plus問合せの実行
        
            <copy>./ua_query_employeesearch.sh</copy>
            
        
        ![統合監査](./images/ua-013.png "従業員の検索")
        
    *   ここで、監査ポリシー`AUDIT_EMPLOYEESEARCH_USAGE`の監査出力の結果を表示します
        
            <copy>./ua_query_audit_records.sh</copy>
            
        
        ![統合監査](./images/ua-017.png "監査出力の結果の表示")
        
        **ノート:**
        
        *   Glassfishアプリケーションからの問合せを取得せずに、SQL\*Plusの使用に対応するエントリがあることがわかります。
        *   アプリケーションが`EMPLOYEESEARCH_PROD`として問合せを実行することを信頼していますが、他のユーザーを信頼していません
        *   他のすべての人を監査したい
9.  この演習を完了したら、統合監査ポリシーを削除できます。
    
        <copy>./ua_delete_audit_policy.sh</copy>
        
    
    ![統合監査](./images/ua-027.png "統合監査ポリシーの削除")
    

## タスク3: データベース・ロール使用の監査

ロールを監査すると、Oracle Databaseは、そのロールに直接付与されているすべてのシステム権限を監査します。ユーザー定義ロールを含む任意のロールを監査できます。ROLES監査オプションでロールに対して共通の統合監査ポリシーを作成する場合、ロール・リストで共通ロールのみを指定する必要があります。

このようなポリシーが有効になっている場合、Oracle Databaseは、共通ロールに共通かつ直接付与されているすべてのシステム権限を監査します。共通ロールにローカルに付与されたシステム権限は監査されません。ロールが共通に付与されたかどうかを確認するには、`DBA_ROLES`データ・ディクショナリ・ビューを問い合せます。ロールに付与された権限が共通に付与されたかどうかを確認するには、`ROLE_SYS_PRIVS`ビューを問い合せます。

1.  ロール`MGR_ROLE`を作成し、`CREATE TABLESPACE`システム権限を付与します。次に、そのロールをデータベース・ユーザー`DBA_NICOLE`に付与します。
    
        <copy>./ua_create_role.sh</copy>
        
    
    ![統合監査](./images/ua-018.png "ロールMGR_ROLEを作成します。")
    
2.  監査ポリシー`AUD_ROLE_POL`を作成して、ロール`MGR_ROLE`の使用を監査します。
    
        <copy>./ua_create_role_audit_policy.sh</copy>
        
    
    ![統合監査](./images/ua-019.png "監査ポリシーの作成 AUD_ROLE_POL")
    
3.  次に、`DBA`ロールを付与される`DBA_JUNIOR`ユーザーを作成します。
    
        <copy>./ua_create_junior_dba.sh</copy>
        
    
    ![統合監査](./images/ua-020.png "DBA_JUNIORユーザーの作成")
    
4.  `DBA`ロールの使用の監査に関連付けられたポリシーを作成します。
    
        <copy>./ua_create_dba_audit_policy.sh</copy>
        
    
    ![統合監査](./images/ua-021.png "関連付けられたポリシーの作成")
    
5.  `MGR_ROLE`および`DBA`ロールの使用に対する監査ポリシーの有効化
    
        <copy>./ua_enable_audit_policies.sh</copy>
        
    
    ![統合監査](./images/ua-022.png "監査ポリシーを有効にします。")
    
6.  有効になっている監査ポリシーの表示
    
        <copy>./ua_view_audit_policies.sh</copy>
        
    
    ![統合監査](./images/ua-023.png "監査ポリシーの確認")
    
7.  統合監査証跡に表示されるSQL文の実行
    
        <copy>./ua_generate_audits.sh</copy>
        
    
    ![統合監査](./images/ua-024.png "監査の生成")
    
8.  2つの監査ポリシーに関連付けられた統合監査証跡出力の表示
    
        <copy>./ua_review_generated_audits.sh</copy>
        
    
    ![統合監査](./images/ua-025.png "統合監査証跡出力の表示")
    
9.  この演習を完了したら、ロール使用の統合監査ポリシーを削除できます。
    
        <copy>./ua_delete_role_audit_policy.sh</copy>
        
    
    ![統合監査](./images/ua-025b.png "ロール使用の統合監査ポリシーの削除")
    

## タスク4: Data Pump使用の監査

この演習では、統合監査証跡を構成し、Oracle Data Pumpエクスポートの監査を確認します。これは、従来の監査では使用できない統合監査の機能です。

1.  Data Pumpアクティビティを監査するための統合監査ポリシー`DP_POL`の作成
    
        <copy>./ua_audit_datapump_export.sh</copy>
        
    
    ![統合監査](./images/ua-028.png "統合監査ポリシーの作成 DP_POL")
    
2.  表`EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES`の2つのData Pumpエクスポートを実行します...
    
        <copy>./ua_datapump_export_hr_table.sh</copy>
        
    
    *   ...認可されたユーザー(`SYSTEM`): **成功**...およびエクスポート・ファイル`$DBSEC_LABS/unified-auditing/HR_table.dmp`が作成されました。
        
        ![統合監査](./images/ua-029a.png "Data Pumpエクスポート")
        
    *   ...および認可されていないユーザー(`DBSAT_ADMIN`): **In Failure!**
        
        ![統合監査](./images/ua-029b.png "Data Pumpエクスポート")
        
    
    **ノート**: 成功したエクスポートのみを使用できます。
    
3.  Data Pumpアクティビティの統合監査証跡の確認
    
        <copy>./ua_review_datapump_audit_events.sh</copy>
        
    
    ![統合監査](./images/ua-030.png "統合監査証跡の確認")
    
4.  演習が完了したら、Data Pump統合監査ポリシーを削除できます。
    
        <copy>./ua_delete_dp_audit_policy.sh</copy>
        
    
    ![統合監査](./images/ua-031.png "Data Pump統合監査ポリシーの削除")
    

次の演習に進むことができます。

## **付録**: 製品について

### **概要**

統合監査では、統合監査証跡によって様々なソースから監査情報が取得されます。

統合監査では、次のソースから監査レコードを取得できます。

*   統合監査ポリシーおよびAUDIT設定による監査レコード
*   `DBMS_FGA` PL/SQLパッケージによるファイングレイン監査レコード
*   Oracle Database Real Application Security監査レコード
*   Oracle Recovery Manager監査レコード
*   Oracle Database Vault監査レコード
*   Oracle Label Security監査レコード
*   Oracle Data Miningのレコード
*   Oracle Data Pump
*   Oracle SQL\*Loaderダイレクト・ロード

統合監査証跡は、SYSAUX表領域のAUDSYSスキーマの読取り専用表に存在し、この情報を`UNIFIED_AUDIT_TRAIL`データ・ディクショナリ・ビューで均一形式で使用可能にし、単一インスタンス環境とOracle Database Real Application Clusters環境の両方で使用できます。ユーザーSYSに加えて、`AUDIT_ADMIN`および`AUDIT_VIEWER`ロールを付与されているユーザーは、これらのビューを問い合せることができます。ユーザーがビューを問い合せるだけで監査ポリシーを作成する必要がない場合は、`AUDIT_VIEWER`ロールを付与します。

データベースが書込み可能な場合、監査レコードは統合監査証跡に書き込まれます。データベースが書込みできない場合、監査レコードは`$ORACLE_BASE/audit/$ORACLE_SID`ディレクトリの新しい形式のオペレーティング・システム・ファイルに書き込まれます。

### **統合監査証跡の利点**

*   統合監査の有効化後、以前のリリースで使用されていた初期化パラメータに依存しません。
*   SYS監査証跡のレコードを含む、Oracle Databaseインストールのすべての監査コンポーネントの監査レコードは、1つの場所に1つの形式で配置されるため、様々な場所を参照して様々な形式の監査証跡を探す必要はありません。
*   監査証跡の管理とセキュリティは、監査証跡を単一にすることでも改善されます。
*   全体的な監査パフォーマンスが大幅に向上します。デフォルトでは、監査レコードがAUDSYSスキーマの内部リレーショナル表に自動的に書き込まれます。
*   サポートされているコンポーネント(この項の最初に記載)およびSYS管理ユーザーを監査できる名前付きの監査ポリシーを作成できます。さらに、ポリシーに条件および除外を作成できます。
*   Oracle Audit Vault and Database Firewall環境を使用している場合は、このデータがすべて1つの場所から取得されるため、統合監査証跡により、監査データの収集が大幅に容易になります。

## 詳細

技術文書:

*   [監査の概要](https://docs.oracle.com/en/database/oracle/oracle-database/19/dbseg/introduction-to-auditing.html)
*   [監査を使用したデータベース・アクティビティの管理](https://docs.oracle.com/en/database/oracle/oracle-database/19/dbseg/part_6.html)

ビデオ:

*   _統合監査の理解(2019年2月)_[](youtube:8spLhyj3iC0)

## 確認

*   **著者** - Database Security PM、Hakim Loumi氏
*   **貢献者** - Angeline Dhanarani
*   **最終更新者/日付** - Hakim Loumi、データベース・セキュリティPM - 2023年11月