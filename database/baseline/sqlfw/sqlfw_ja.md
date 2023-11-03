# Oracle SQL Firewall

## 概要

このワークショップでは、Oracle SQL Firewallの機能を紹介します。ユーザーは、SQLインジェクション・データベース攻撃を含むデータドリブンWebアプリケーションのセキュリティ欠陥/脆弱性をターゲットとするリスクから保護するようにこれらの機能を構成する方法を学ぶことができます。

_推定ラボ時間:_ 30分

_この演習でテストしたバージョン:_ Oracle DB 23.2

### ビデオ・プレビュー

「_Oracle Database 23c FreeでのSQL Firewallの使用(2023年6月)_」のプレビューを見る[](youtube:7yJv92FvLp4)

### 目標

*   機密データを保護するSQLファイアウォール・ポリシーの作成
*   盗まれた資格証明アクセスの内部の脅威を検出します
*   SQLインジェクション攻撃のリスクを軽減

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
| 1 | Glassfish HRアプリケーションを保護するSQL Firewallの有効化 | 10分間 |
| 2 | SQL Firewallによる資格証明アクセスの盗難による内部脅威の検出 | 10分間 |
| 3 | 許可されたSQLおよびアクセス・パターンをSQL Firewallで強制し、SQLインジェクション攻撃のリスクを軽減 | 10分間 |
| 4 | SQL Firewall Labs環境のリセット | 5分未満 |

## タスク1: SQL Firewallの有効化によるGlassfish HRアプリケーションの保護

このラボでは、管理者がシステムをトレーニングして、認可されたSQL文およびHRアプリケーションの信頼できる接続パスを学習する方法を学習します。SQLファイアウォール・ポリシーは、認可されたSQL接続および文を表す許可リストを使用して生成され、ターゲットにデプロイされます。

## タスク1a: SQL Firewall環境の設定

ここでは、デフォルトのGlassfish接続を変更して、SQLコマンドを監視およびブロックできるようにOracle Database 23cをターゲットにします。

1.  OSユーザー**oracle**として、_DBSec-Lab_ VMでターミナル・セッションを開きます
    
        <copy>sudo su - oracle</copy>
        
    
    **ノート**: リモート・デスクトップ・セッションを使用している場合は、デスクトップの_ターミナル_・アイコンをダブルクリックしてセッションを起動します。
    
2.  scriptsディレクトリへ移動します
    
        <copy>cd $DBSEC_LABS/sqlfw</copy>
        
3.  23cデータベースをターゲットにするために、Glassfishアプリケーション接続文字列を移行します
    
        <copy>./sqlfw_glassfish_start_db23c.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-001.png "DB23cを使用したHRアプリケーションの設定")
    
    **ノート**: ここでは、**`db23c`** VM上のデータベース**`FREEPDB1`** (DB 23c)にGlassfishを接続します
    
4.  次に、アプリケーションが期待どおりに機能することを確認します。
    
    *   URL _`http://dbsec-lab:8080/hr_prod_pdb1`_でWebブラウザを開き、**Glassfishアプリケーション**にアクセスします
        
        **ノート:**リモート・デスクトップを使用していない場合は、_`http://<YOUR_DBSEC-LAB_VM_PUBLIC_IP>:8080/hr_prod_pdb1`_に移動してこのページにアクセスすることもできます
        
    *   _`hradmin`_として、パスワード_`Oracle123`_でアプリケーションにログインします。
        
            <copy>hradmin</copy>
            
        
            <copy>Oracle123</copy>
            
        
        ![SQLFW](./images/sqlfw-002.png "HRアプリケーション- ログイン")
        
        ![SQLFW](./images/sqlfw-003.png "HRアプリケーション- ログイン")
        
    *   アプリの右上隅にある**「ようこそHR管理者」**リンクをクリックすると、セッション・データを含むページが表示されます。
        
        ![SQLFW](./images/sqlfw-004.png "HRアプリケーション- 設定")
        
    *   **「セッション詳細」**画面で、アプリケーションがどのようにデータベースに接続されているかを確認します。この情報は、`SYS_CONTEXT`ファンクションを実行して**userenv**ネームスペースから取得されます。
        
        ![SQLFW](./images/sqlfw-005.png "HRアプリケーション- セッション詳細")
        
    *   これで、**FREEPDB1**が**`DB_NAME`**として、**db23c**が**HOST**として表示されます。
        
        ![SQLFW](./images/sqlfw-006.png "HRアプリケーション- ターゲット・データベースのチェック")
        
5.  SQL Firewallを管理するための管理者(**`dba_tom`**)の作成
    
        <copy>./sqlfw_crea_admin-user.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-007.png "SQL Firewall管理ユーザーの作成")
    
6.  SQLファイアウォールの有効化
    
        <copy>./sqlfw_enable.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-008.png "SQLファイアウォールの有効化")
    
    **ノート**: `ENABLED`を参照する必要があります
    

## タスク1b: SQL Firewallを有効にして、HRアプリケーション・ユーザーの認可されたSQLトラフィックを学習

1.  アプリケーション・ユーザーEMPLOYEESEARCH\_PRODのSQLワークロード取得を開始します。
    
        <copy>./sqlfw_capture_start.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-009.png "アプリケーション・ユーザーのSQLワークロード取得を開始します。")
    
2.  次に、Glassfishアプリケーションを使用して、データベースでアクティビティを生成します。
    
    *   Webブラウザ・ウィンドウに戻り、_`http://dbsec-lab:8080/hr_prod_pdb1`_にアクセスします
        
        **ノート:**リモート・デスクトップを使用していない場合は、_`http://<YOUR_DBSEC-LAB_VM_PUBLIC_IP>:8080/hr_prod_pdb1`_に移動してこのページにアクセスすることもできます
        
    *   **「従業員の検索」**をクリックします。
        
        ![SQLFW](./images/sqlfw-010.png "従業員の検索")
        
    *   \[**検索**\]をクリックします
        
        ![SQLFW](./images/sqlfw-011.png "従業員の検索")
        
    *   基準の一部を変更し、再度検索してください
        
    *   十分なトラフィックを確保するために、**2-3回繰り返す**
        
3.  ターミナル・セッションに戻り、アプリケーション・ワークロードのSQL文および接続が適切に取得されていることを確認します。
    
        <copy>./sqlfw_capture_check.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-012.png "セッションの確認とログの取得")
    
    **ノート:**ここでは、セッションおよび取得ログを確認します
    
4.  問題がない場合は、SQLワークロードの取得を停止します。
    
        <copy>./sqlfw_capture_stop.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-013.png "SQLワークロード取得の停止")
    

## タスク1c: HRアプリケーション・ユーザーの許可リスト・ルールの生成および有効化

1.  許可リスト・ルールの生成
    
        <copy>./sqlfw_allow_list_rule_gen.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-014.png "許可リスト・ルールの生成")
    
    **ノート:**ここでは、4つの文があります
    
2.  このリストをキャプチャしたイベントと比較
    
        <copy>./sqlfw_capture_count_events.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-015.png "取得されたイベントのカウント")
    
    **ノート:**カウントは、取得した個別イベントの数と一致します
    
3.  次に、信頼できるデータベース接続およびSQL文のSQL Firewall許可リスト・ルールを確認します。
    
        <copy>./sqlfw_allow_list_rule_exam.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-016.png "SQL Firewall許可リスト・ルールの確認")
    
    **ノート:**ここでは、サーバー`10.0.0.150`上のユーザー`oracle`によって開始されたWebアプリケーション(`JDBC ThinClient`)からの接続のみを許可します
    
4.  SQL Firewall違反の監査ポリシーの設定
    
        <copy>./sqlfw_setup_audit_policies.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-017.png "SQL Firewall違反の監査ポリシーの設定")
    
5.  **監視モード**で`EMPLOYEESEARCH_PROD`の許可リスト・ルールを有効にします
    
        <copy>./sqlfw_allow_list_rule_enable_monitor.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-018.png "監視モードでの許可リスト・ルールの有効化")
    
    **ノート:**ここでは、SQLファイアウォール違反を監視し、ブロックしません。
    

## タスク2: SQL Firewallによる資格証明アクセスの盗難に対する内部脅威の検出

HR Appsユーザー`EMPLOYEESEARCH_PROD`の盗難資格証明にアクセスした悪意のある内部者が存在し、HRアプリケーション認可をバイパスしてSQL Developerを使用して機密従業員データにアクセスするとします。

1.  まず、通常のアプリケーションSQLワークロードがデータベースに対して許可されていることを検証します。
    
    *   Glassfishアプリケーションを使用して、データベースでアクティビティを生成し、通常の操作(一致、違反ログなし)を実行します。
        
        *   Webブラウザ・ウィンドウに戻り、_`http://dbsec-lab:8080/hr_prod_pdb1`_にアクセスします
            
            **ノート:**リモート・デスクトップを使用していない場合は、_`http://<YOUR_DBSEC-LAB_VM_PUBLIC_IP>:8080/hr_prod_pdb1`_に移動してこのページにアクセスすることもできます
            
        *   **「従業員の検索」**をクリックします。
            
            ![SQLFW](./images/sqlfw-010.png "従業員の検索")
            
        *   \[**検索**\]をクリックします
            
            ![SQLFW](./images/sqlfw-011.png "従業員の検索")
            
        *   基準の一部(以前と同じ)を変更し、再度検索します
            
        *   十分なトラフィックを確保するために、**2-3回繰り返す**
            
    *   ターミナル・セッションに戻り、違反ログおよび監査レコードを確認します
        
            <copy>./sqlfw_check_events.sh</copy>
            
        
        ![SQLFW](./images/sqlfw-019.png "違反ログおよび監査レコードのチェック")
        
        **ノート:**これらの問合せは、データベースへのSQL文としてすでにリストされているため、レコードは見つかりません。
        
2.  次に、盗まれた資格証明アクセスの内部脅威を検出します
    
    *   内部者は、SQL\*Plusを使用して機密性の高い従業員データにアクセスします。
        
            <copy>./sqlfw_select_sensitive_data.sh</copy>
            
        
        ![SQLFW](./images/sqlfw-020.png "機密性の高い従業員データの選択")
        
    *   違反ログおよび監査レコードの再確認
        
            <copy>./sqlfw_check_events.sh</copy>
            
        
        ![SQLFW](./images/sqlfw-021.png "違反ログおよび監査レコードのチェック")
        
        **ノート:** SQL\*Plusが許可されたOSプログラム許可リストにないため、セキュリティ管理者の注意を引いてSQL Firewallコンテキスト違反が発生します。
        

## タスク3: SQLファイアウォールで許可されたSQLおよびアクセス・パターンを適用して、SQLインジェクション攻撃のリスクを軽減

管理者は、悪意のある内部者が疑わしい場合、ブロック・モードのSQL Firewallを有効にして、機密性の高い従業員情報へのUN認可によるアクセスを禁止します。SQL Firewallが、承認されたSQL文やデータベース接続パスなどの許可されたパターンを強制し、潜在的なSQLインジェクション攻撃やHRアプリケーションDBの異常なアクセスについて警告する方法について学習します。

ここでは、許可されていないSQL接続/文の検出時にSQL Firewallをブロックできるようにします

1.  許可リスト・ルールの強制を**ブロッキング・モード**に更新します
    
        <copy>./sqlfw_allow_list_rule_enable_block.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-022.png "許可リスト・ルールをブロック・モードに更新します")
    
    **ノート:** SQL FirewallでSQLインジェクションの試行をブロックできるようになりました
    
2.  現在、ハッカーがGlassfishアプリケーションにログインしてSQLインジェクション攻撃を実行しています
    
    *   GlassfishアプリケーションのWebページに戻り、ログアウトして、パスワード_`Oracle123`_で_`hradmin`_としてログインします。
        
    *   **「従業員の検索」**をクリックします
        
        ![SQLFW](./images/sqlfw-010.png "従業員の検索")
        
    *   \[**検索**\]をクリックします
        
        ![SQLFW](./images/sqlfw-011.png "従業員の検索")
        
        **ノート**: すべての行が戻されます...通常、リマーバですべてが許可されているためです。
        
    *   次に、**チェック・ボックス「デバッグ」**を選択して、このフォームの背後にあるSQL問合せを確認します
        
        ![SQLFW](./images/sqlfw-023.png "フォームの背後に実行されたSQL問合せの確認")
        
    *   「**検索**」を再度クリックします
        
        ![SQLFW](./images/sqlfw-024.png "従業員の検索")
        
        **ノート:**
        
        *   これで、このフォームで実行された公式SQL問合せが表示され、結果が表示されます。
        *   この問合せでは、リクエストされた列の数、列の名前、データ型およびその関係に関する情報が提供されます。
    *   この情報に基づいて、UNIONベースのSQLインジェクション問合せを作成して、フォームから直接抽出するすべての機密データを表示できます。ここでは、この問合せを使用して、`DEMO_HR_SUPPLEMENTAL_DATA`表から`USER_ID', 'MEMBER_ID', 'PAYMENT_ACCT_NO`および`ROUTING_NUMBER`を抽出します。
        
            <copy>
            ' UNION SELECT userid, ' ID: '|| member_id, 'SQLi', '1', '1', '1', '1', '1', '1', 0, 0, payment_acct_no, routing_number, sysdate, sysdate, '0', 1, '1', '1', 1 FROM demo_hr_supplemental_data --
            </copy>
            
    *   SQLインジェクション問合せをコピーし、「検索」フォームの**「位置」フィールドに直接貼り付け**、**「デバッグ」チェック・ボックスを選択します**
        
        ![SQLFW](./images/sqlfw-025.png "SQLインジェクション問合せのコピー/貼付け")
        
        **ノート:**
        
        *   SQL句"LIKE"を閉じるには、UNIONキーワードの前に"`'`"を忘れないでください
        *   問合せの残りの部分を無効にするには、最後に`--`を忘れないでください。
    *   \[**検索**\]をクリックします
        
        ![SQLFW](./images/sqlfw-026.png "従業員の検索")
        
        **ノート:**
        
        *   出力は、これらの試行でORA-failuresを返す必要があります
        *   これは、SQL FirewallポリシーのAllow-listにUNION問合せが追加されていないためです。
3.  違反ログと監査レコードを確認
    
        <copy>./sqlfw_check_events.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-027.png "違反ログおよび監査レコードのチェック")
    
    **ノート:**セキュリティ管理者に注意して、SQL違反が発生します。
    

## タスク4: SQL Firewall Labs環境のリセット

1.  SQL Firewallの概念に慣れたら、環境をリセットできます。
    
        <copy>./sqlfw_reset_env.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-050.png "SQL Firewall Labs環境のリセット")
    
2.  デフォルト・データベースをターゲットにするために、Glassfishアプリケーション接続文字列を移行します(**pdb1**)
    
        <copy>./sqlfw_glassfish_stop_db23c.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-051.png "PDB1を使用したHRアプリケーションの設定")
    
    **ノート**: ここでは、**`dbsec-lab`** VM上のデータベース**`PDB1`** (DB 19c)にGlassfishを接続します
    

次の演習に進むことができます。

## **付録**: 製品について

### **概要**

SQLファイアウォールは、Oracle Databaseカーネルに組み込まれているデータベース・セキュリティ機能で、すべての受信SQL文を検査し、SQLファイアウォール許可リスト・ポリシーに含まれないSQL文/接続をログに記録またはブロックできます。SQLファイアウォールは、明示的に認可されたSQLのみが実行されるようにします。SQLインジェクション攻撃や侵害されたアカウントなどの一般的なデータベース・リスクに対するクラス最高の保護を提供します。

SQLインジェクションは、データ駆動型Webアプリケーションの一般的なデータベース攻撃パターンです。Webアプリケーションの脆弱性とセキュリティ上の欠陥を悪用すると、攻撃者はデータベース情報の変更、機密データへのアクセス、データベースの管理タスクの実行、資格証明の盗難、その他の機密システムへのアクセスを後から移動する可能性があります。

Oracle Databaseに組み込まれている他のデータベース・セキュリティ機能は、このようなWebアプリケーション攻撃を監視/防止するための様々なセキュリティ制御を提供しますが、SQL Firewallは、すべての受信SQL文を検査し、認可されたSQLのみを許可する唯一のセキュリティ制御です。権限のないSQL問合せがデータベースで実行されないようにログに記録およびブロックします。

SQLファイアウォールは、起点に関係なく、すべての受信SQL文に沿ってOracle Databaseカーネル内で動作します。SQL Firewallは、ルール違反を検出したときにSQLトラフィックを許可、記録およびオプションでブロックできます。

![SQLFW](./images/sqlfw-concept.png "SQLファイアウォールの概念")

図1: Oracle Databaseカーネルに組み込まれたOracle SQL Firewall

Oracle SQL Firewallポリシーは、アプリケーション・サービス・アカウントか、レポート・ユーザーやデータベース管理者などの直接データベース・ユーザーかに関係なく、データベース・アカウント・レベルで機能します。すべてのデータベース・アカウントのSQLファイアウォール・ポリシーは、認可されたSQL文および関連する信頼できるデータベース接続パスの許可リストで構成されます。許可リスト・アプローチは、SQLインジェクション攻撃や侵害されたアカウントなどのリスクに対するより高いレベルの保護を提供します。これにより、信頼できるデータベース接続からの認可されたSQL文のみがOracleデータベース内での実行を許可され、その中に格納されている機密データへのアクセスに対する認可されていない試行のアラート/ブロックが行われます。署名ベースの保護メカニズムとは異なり、SQL文をエンコードしたり、シノニムを参照したり、動的に生成されたオブジェクト名を参照して、SQL Firewallをフールすることはできません。

`SYS.DBMS_SQL_FIREWALL`パッケージのPL/SQLプロシージャを使用すると、Oracle Database内でSQL Firewall構成を管理および管理できます。Oracle SQL Firewallは、Oracle Database Enterprise Edition (バージョン23c以降)でのみ使用できます。使用するには、Oracle SQL Firewallのライセンスが必要です。ライセンスへのパスは2つあります。

*   Oracle SQL Firewallは、Oracle Database Vaultに含まれています。Database Vaultは追加コストのオプションです。
*   Oracle SQL Firewallは、Oracle Audit Vault and Database Firewall (AVDF)に含まれています。AVDFは独立した製品であり、ライセンスが必要です。

### **Oracle SQL Firewallを使用する利点**

*   承認されたSQL文/接続にのみデータベース・アクセスを制限することで、一般的なデータベース攻撃に対するリアルタイムの保護を提供します。
*   SQLインジェクション攻撃、異常なアクセス、資格証明の盗難/不正使用によるリスクを軽減します。
*   信頼できるデータベース接続パスを強制します。

## 詳細

技術文書:

*   [Oracle SQL Firewall 23c](https://docs.oracle.com/en/database/oracle/oracle-database/23/dbseg/using-sql-firewall.html#GUID-F53EAE01-CE78-47F4-80AD-A0091BA3C434)

## 謝辞

*   **著者** - Database Security PM、Hakim Loumi氏
*   **貢献者** - Angeline Dhanarani
*   **最終更新者/日付** - Hakim Loumi、データベース・セキュリティPM - 2023年11月