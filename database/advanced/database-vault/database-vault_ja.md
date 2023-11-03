# Oracle Database Vault(DV)

## 概要

このワークショップでは、Oracle Database Vault (DV)の様々な機能について説明します。これにより、権限のない特権ユーザーが機密データにアクセスできないようにこれらの機能を構成する方法を学習できます。

見積時間: 45分

_この演習でテストしたバージョン:_ Oracle DB 19.19

### ビデオ・プレビュー

_LiveLabs - Oracle Database Vault (2022年5月)_のプレビューを見る[](youtube:M5Kn-acUHRQ)

### 目標

*   コンテナおよび`PDB1`プラガブル・データベースでDatabase Vaultを有効化
*   Database Vaultレルムを使用した機密データの保護
*   トラステッドパスを使用してサービスアカウントを保護する
*   シミュレーション・モードでのDatabase Vaultコントロールのテスト
*   コンテナ管理者からのプラガブル・データベースの保護

### 前提条件

この演習では、次を想定しています。

*   Oracle Cloudアカウント
*   次を完了しました:
    *   演習: 設定の準備(有料テナントのみ)
    *   演習: 環境設定
    *   演習: 環境の初期化

### ラボのタイミング(推定)

| ステップ番号 | 機能 | 近似時間 |
| --- | --- | --- |
| 1 | DatabaseVaultの有効化 | 5分 |
| 2 | 単純なレルムの作成 | 10分 |
| 3 | トラステッドパス/マルチファクタ承認の作成 | 10分 |
| 4 | シミュレーション・モード | 10分 |
| 5 | 操作制御 | 5分 |
| 6 | DatabaseVaultの無効化 | 5分未満 |

## タスク1: Database Vaultの有効化

1.  OSユーザー**oracle**として、_DBSec-Lab_ VMでターミナル・セッションを開きます
    
        <copy>sudo su - oracle</copy>
        
    
    **ノート**: リモート・デスクトップ・セッションを使用している場合は、デスクトップの_ターミナル_・アイコンをダブルクリックしてセッションを起動します。
    
2.  scriptsディレクトリへ移動します
    
        <copy>cd $DBSEC_LABS/database-vault</copy>
        
3.  まず、コンテナ・データベース**cdb1**でDatabase Vaultを有効にします。
    
        <copy>./dv_enable_on_cdb.sh</copy>
        
    
    **ノート**: DB Vaultを有効にするには、データベースが再起動されます。
    
    ![DB Vault](./images/dv-001.png "DB Vaultの有効化")
    
4.  次に、プラガブル・データベースで有効にします。ここでは、**pdb1**で有効にします
    
        <copy>./dv_enable_on_pdb.sh pdb1</copy>
        
    
    次のようなステータスが表示されます:
    
    ![DB Vault](./images/dv-002.png "DB Vaultの有効化")
    
5.  これで、コンテナ・データベースおよびpdb1でDatabase Vaultが有効になりました。
    

## タスク2: 単純なレルムの作成

1.  _`http://dbsec-lab:8080/hr_prod_pdb1`_へのWebブラウザ・ウィンドウを開き、Glassfishアプリケーションにアクセスします
    
    **ノート:**リモート・デスクトップを使用していない場合は、_`http://<YOUR_DBSEC-LAB_VM_PUBLIC_IP>:8080/hr_prod_pdb1`_に移動してこのページにアクセスすることもできます
    
    ![DB Vault](./images/dv-029.png "HRアプリケーション- ログイン")
    
2.  _`hradmin`_として、パスワード_`Oracle123`_でアプリケーションにログインします。
    
        <copy>hradmin</copy>
        
    
        <copy>Oracle123</copy>
        
    
    ![DB Vault](./images/dv-030.png "HRアプリケーション- ログイン")
    
3.  **「従業員の検索」**をクリックします
    
    ![DB Vault](./images/dv-031.png "HRアプリケーション- 従業員の検索")
    
4.  \[**検索**\]をクリックします
    
    ![DB Vault](./images/dv-032.png "HRアプリケーション- 従業員の検索")
    
5.  ターミナル・セッションに戻り、コマンドを実行してGlassfishセッションの詳細を表示します
    
        <copy>./dv_query_employee_data.sh</copy>
        
    
    ![DB Vault](./images/dv-003.png "HRアプリケーション- 従業員データ")
    
6.  ここで、**レルム**`PROTECT_EMPLOYEESEARCH_PROD`を作成して、`EMPLOYEESEARCH_PROD`スキーマ内のオブジェクトを悪意のあるアクティビティから保護します。
    
        <copy>./dv_create_realm.sh</copy>
        
    
    ![DB Vault](./images/dv-004.png "レルムPROTECT_EMPLOYEESEARCH_PRODの作成")
    
7.  保護するオブジェクトをレルムに追加します(ここでは、スキーマのすべてのオブジェクトを追加します)。
    
        <copy>./dv_add_obj_to_realm.sh</copy>
        
    
    ![DB Vault](./images/dv-005.png "保護するオブジェクトをレルムに追加します。")
    
8.  レルムに認可されたユーザーがいることを確認します。このステップでは、レルム認可所有者として`EMPLOYEESEARCH_PROD`を追加します
    
        <copy>./dv_add_auth_to_realm.sh</copy>
        
    
    ![DB Vault](./images/dv-006.png "レルム認可所有者としてEMPLOYEESEARCH_PRODを追加します")
    
9.  SQL問合せを再実行して、`SYS`が**権限不足**エラー・メッセージを受信したことを示します。
    
        <copy>./dv_query_employee_data.sh</copy>
        
    
    ![DB Vault](./images/dv-007a.png "SYSユーザーが権限不足エラー・メッセージを受信しました")
    
10.  この演習を完了すると、レルムを削除できます。
    
        <copy>./dv_drop_realm.sh</copy>
        
    
    ![DB Vault](./images/dv-007b.png "レルムの削除")
    

## タスク3: トラステッド・パス/マルチファクタ認可の作成

1.  Glassfishアプリケーションに戻り、「**従業員の検索**」を再度クリックします
    
    ![DB Vault](./images/dv-031.png "HRアプリケーション- 従業員の検索")
    
2.  \[**検索**\]をクリックします
    
    ![DB Vault](./images/dv-032.png "HRアプリケーション- 検索")
    
3.  ターミナル・セッションに戻り、この問合せを実行して、Glassfishアプリケーションに関連付けられたセッション情報を表示します。
    
        <copy>./dv_query_employeesearch_usage.sh</copy>
        
    
    ![DB Vault](./images/dv-019.png "HRアプリケーションに関連付けられたセッション情報の表示")
    
4.  ここで、`EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES`表を所有者`EMPLOYEESEARCH_PROD`で問い合せて、アクセス可能であることを示します。
    
        <copy>./dv_query_employee_search.sh</copy>
        
    
    ![DB Vault](./images/dv-020.png "問合せ表")
    
5.  Database Vaultルールを作成して、アプリケーション資格証明の保護を開始します。
    
        <copy>./dv_create_rule.sh</copy>
        
    
    ![DB Vault](./images/dv-021.png "Database Vaultルールの作成")
    
    **ノート**: トラステッド・パス・アプリケーションとして認可するのは、スキーマ所有者`EMPLOYEESEARCH_PROD`を介したGlassfish Webアプリケーション(JDBC Thinクライアント)からのアクセスのみです。
    
6.  Database Vaultルールは、**DVルール・セット**に追加して使用します
    
    *   ルール・セットには1つ以上のルールを含めることができます。
    *   複数のルールがある場合、すべてのルールを評価するルール・セットをtrueにするか、`ANY`ルールをtrueにするかを選択できます。
    *   `IN`と`EXISTS`の違いのように考えてみます。`IN`は、1つの結果が一致すると`EXISTS`が停止する間、すべてを含みます。
    
        <copy>./dv_create_rule_set.sh</copy>
        
    
    ![DB Vault](./images/dv-022.png "Database Vaultルール・セットの作成")
    
7.  `EMPLOYEESEARCH_PROD`ユーザーを保護するためのコマンド・ルールを**CONNECT**に作成します。
    
        <copy>./dv_create_command_rule.sh</copy>
        
    
    ![DB Vault](./images/dv-023.png "CONNECTでのコマンド・ルールの作成")
    
    **ノート**: `CONNECT`は、作成したルール・セットと一致する場合にのみ`EMPLOYEESEARCH_PROD`として指定できます。
    
8.  Glassfishアプリケーションに戻り、数回リフレッシュし、\[**検索**\]をクリックして問合せを実行し、従業員データを調査します
    
    **ノート**: Glassfishアプリケーションをトラステッド・パス・アプリケーションとして使用しているため、データにアクセスできます。
    
9.  ターミナル・セッションに戻り、アプリケーションの使用状況の問合せを再実行して、まだ機能していることを確認します
    
        <copy>./dv_query_employeesearch_usage.sh</copy>
        
    
    ![DB Vault](./images/dv-024.png "従業員の検索")
    
10.  ここで、所有者`EMPLOYEESEARCH_PROD`を使用して`EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES`表を問い合せてみます...**ブロックします**!
    
        <copy>./dv_query_employee_search.sh</copy>
        
    
    ![DB Vault](./images/dv-025.png "問合せ表")
    
    **ノート**: 「Trusted Path」以外のアプリケーションを使用して問合せを行うため、データにアクセスできません。
    
11.  演習を正常に完了したら、Database Vaultから**「コマンド・ルール」**、**「ルール・セット」**および**「ルール」**を削除できます。
    
        <copy>./dv_del_trusted_path.sh</copy>
        
    
    ![DB Vault](./images/dv-026.png "トラステッドパスの削除")
    

## タスク4: シミュレーション・モード

1.  まず、シミュレーション・ログを問い合せて、現在の値がないことを確認します。
    
        <copy>./dv_query_simulation_logs.sh</copy>
        
    
    ![DB Vault](./images/dv-008.png "シミュレーション・ログの問合せ")
    
2.  次に、データベースへのすべての接続のブロックをシミュレートするコマンド・ルールを作成します。これは、誰が接続しているのか、どこから接続しているのかを識別するための簡単な方法です。
    
        <copy>./dv_command_rule_sim_mode.sh</copy>
        
    
    ![DB Vault](./images/dv-009.png "コマンド・ルールの作成")
    
3.  スクリプトを実行してDB接続を作成し、ログ・エントリを生成します
    
        <copy>./dv_run_queries.sh</copy>
        
    
    ![DB Vault](./images/dv-010.png "トラフィックを生成")
    
4.  次に、シミュレーション・ログを再度問い合せて、新しいエントリを確認します。ユーザー接続のブロックをシミュレートするコマンド・ルールを作成したことを覚えておいてください。
    
        <copy>./dv_query_simulation_logs.sh</copy>
        
    
    ![DB Vault](./images/dv-011.png "シミュレーション・ログの問合せ")
    
    ログには、ルールによって接続され、ブロックされる可能性があるすべてのユーザーが表示されます。また、接続元の場所と、接続に使用したクライアントも表示されます。
    
5.  このスクリプトを実行して、シミュレーション・ログに存在する個別のユーザー名のリストを取得します
    
        <copy>./dv_distinct_users_sim_logs.sh</copy>
        
    
    ![DB Vault](./images/dv-012a.png "シミュレーション・ログに存在する個別のユーザー名のリスト")
    
6.  **CONNECT**ルールではシミュレーション・モードのみを使用していましたが、これをレルムで使用して、どのような違反があったかを示すことができました。
    
7.  次の演習に進む前に、シミュレーション・ログをクリーン・アウトし、コマンド・ルールを削除します
    
        <copy>./dv_purge_sim_logs.sh</copy>
        
    
    ![DB Vault](./images/dv-012b.png "シミュレーション・ログのパージ")
    
        <copy>./dv_drop_command_rule.sh</copy>
        
    
    ![DB Vault](./images/dv-012c.png "コマンド・ルールの削除")
    

## タスク5: 操作制御

1.  Database Vault and Operations Controlのステータスの確認
    
        <copy>./dv_status.sh</copy>
        
    
    ![DB Vault](./images/dv-013.png "Database Vaultのステータスを確認します。")
    
    **ノート**: まだ構成されていません。
    
2.  次に、プラガブル・データベース**pdb1**と**pdb2**の両方と同じ問合せを実行します...
    
    *   ... `DBA_DEBRA`として
    
        <copy>./dv_query_with_debra.sh</copy>
        
    
    ![DB Vault](./images/dv-014.png "DBA DEBRAとして問合せ")
    
    *   ... `C##SEC_DBA_SAL`として
    
        <copy>./dv_query_with_sal.sh</copy>
        
    
    ![DB Vault](./images/dv-015.png "DBA SALとして問合せ")
    
    **ノート:**
    
    *   問合せ結果は同じ
    *   共通ユーザー`C##SEC_DBA_SAL`は、pdb管理者と同様に、プラガブル・データベースのデータにアクセスできます。
3.  Database Vault 19cの**操作制御**を有効にし、問合せを再度実行します
    
    **ノート**: 誰が`EMPLOYEESEARCH_PROD`スキーマ・データを問い合せることができるか、誰が問合せできないかに注意してください。`SAL`は、データにアクセスできなくなります。
    
        <copy>./dv_enable_ops_control.sh</copy>
        
    
    ![DB Vault](./images/dv-016a.png "OPS制御の有効化")
    
        <copy>./dv_status.sh</copy>
        
    
    ![DB Vault](./images/dv-016b.png "Database Vaultのステータスを確認します。")
    
        <copy>./dv_query_with_debra.sh</copy>
        
    
    ![DB Vault](./images/dv-017.png "DBA DEBRAとして問合せ")
    
        <copy>./dv_query_with_sal.sh</copy>
        
    
    ![DB Vault](./images/dv-018a.png "DBA SALとして問合せ")
    
4.  この演習を完了したら、「Ops Control」を無効にします。
    
        <copy>./dv_disable_ops_control.sh</copy>
        
    
    ![DB Vault](./images/dv-018b.png "OPSコントロールの無効化")
    

## タスク6: Database Vaultの無効化

1.  プラガブル・データベース**pdb1**の無効化
    
        <copy>./dv_disable_on_pdb.sh pdb1</copy>
        
    
    **ノート**: pdb1の`DV_ENABLE_STATUS`は**FALSEである必要があります**
    
    ![DB Vault](./images/dv-027.png "DatabaseVaultの無効化")
    
2.  次に、コンテナ・データベース**cdb1**でDatabase Vaultを無効にします。
    
        <copy>./dv_disable_on_cdb.sh</copy>
        
    
    ![DB Vault](./images/dv-028.png "DatabaseVaultの無効化")
    
    **ノート:**
    
    *   DB Vaultを無効にするには、データベースが再起動されます。
    *   cdbの`DV_ENABLE_STATUS`は**FALSE**である必要があります
3.  これで、コンテナ・データベースおよびpdb1でDatabase Vaultが無効になりました。
    

次の演習に進むことができます。

## **付録**: 製品について

### **概要**

Oracle Database Vaultは、認可されていない特権ユーザーが機密データにアクセスできないようにし、認可されていないデータベース変更を防ぐための制御を提供します。

Oracle Database Vaultのセキュリティ管理機能は、アプリケーション・データを不正アクセスから守り、プライバシー要件や法定要件を満たします。

![DB Vault](./images/dv-concept.png "DB Vaultの概念")

アプリケーション・データへの特権アカウント・アクセスをブロックしたり、トラステッド・パス認可を使用してデータベース内の機密操作を制御するためのコントロールをデプロイできます。

権限およびロールの分析により、最小権限のベスト・プラクティスを使用することで、既存のアプリケーションのセキュリティを強化できます。

Oracle Database Vaultは、既存のデータベース環境を透過的に保護するため、コストと時間がかかります。

Oracle Database Vaultを使用して一連のコンポーネントを作成し、データベース・インスタンスのセキュリティを管理できます。

これらのコンポーネントは次のとおりです。

*   **レルム**

レルムは、データベース・スキーマ、オブジェクトおよびロールを保護できるデータベース内の保護ゾーンです。たとえば、会計、販売または人事に関連するスキーマ、オブジェクトおよびロールのセットを保護できます。これらをレルムに保護した後、そのレルムを使用して、特定のアカウントまたはロールに対するシステム権限およびオブジェクト権限の使用を制御できます。これにより、これらのスキーマ、オブジェクトおよびロールを使用するすべてのユーザーにきめ細かいアクセス制御を提供できます。

*   **コマンド・ルール**

コマンド・ルールとは、SELECT、ALTER SYSTEM、データベース定義言語(DDL)文およびデータ操作言語(DML)文などのほぼすべてのSQL文をユーザーが実行する方法を制御するために作成する特別なセキュリティ・ポリシーです。コマンド・ルールはルール・セットと連携して、文を許可するかどうかを決定する必要があります。

*   **ファクタ**

ファクタは、Oracle Database Vaultが信頼できるパスとして認識して使用できる名前付き変数または属性(ユーザー・ロケーション、データベースIPアドレス、セッション・ユーザーなど)です。ルールのファクタを使用して、データベースに接続するためのデータベース・アカウントの認可や特定のデータベース・コマンドの実行などのアクティビティを制御し、データの可視性と管理性を制限できます。各ファクターには、1つ以上のIDを指定できます。アイデンティティは、ファクタの実際の値です。ファクターは、ファクター検索方法またはそのIDマッピング ロジックに応じて、複数のIDを持つことができます。

*   **ルール・セット**

ルール・セットは、レルム認可、コマンド・ルール、ファクタ割当てまたはセキュア・アプリケーション・ロールに関連付けることができる1つ以上のルールの集合です。ルール・セットは、含まれる各ルールの評価および評価タイプ(「すべてTrue」または「いずれかTrue」)に基づいてtrueまたはfalseに評価されます。ルール・セット内のルールは、trueまたはfalseに評価されるPL/SQL式です。複数のルール・セットで同じルールを使用できます。

*   **アプリケーション・ロールの保護**

セキュア・アプリケーション・ロールは、Oracle Database Vaultルール・セットの評価に基づいて有効にできる特別なOracle Databaseロールです。

これらのコンポーネントを強化するために、Oracle Database Vaultには一連のPL/SQLインタフェースおよびパッケージが用意されています。一般に、最初に実行するステップは、保護するデータベース・スキーマまたはデータベース・オブジェクトで構成されるレルムを作成することです。ルール、コマンド・ルール、ファクタ、アイデンティティ、ルール・セットおよびセキュア・アプリケーション・ロールを作成することで、レルムをさらに保護できます。また、これらのコンポーネントが監視および保護するアクティビティに関するレポートを実行できます。

### **Database Vaultを使用する利点**

*   セキュリティ意識向上のためのコンプライアンス規制への対応
*   特権ユーザー・アカウントを多くのセキュリティ侵害から保護し、外部と内部の両方でデータを盗む
*   データベースの柔軟なセキュリティ・ポリシーの設計を支援します。
*   データベース統合とクラウド環境の懸念に対処して、コストを削減し、機密アプリケーション・データの露出を、真の知る必要のない人にも低減します。
*   マルチテナント環境で機能し、統合のセキュリティを強化

## 詳細

技術文書:

*   [Oracle Database Vault](https://docs.oracle.com/en/database/oracle/oracle-database/19/dvadm/introduction-to-oracle-database-vault.html#GUID-0C8AF1B2-6CE9-4408-BFB3-7B2C7F9E7284)

ビデオ:

*   _Oracle Database Vault - ユースケース(Part1)(2019年10月)_[](youtube:aW9YQT5IRmA)
*   _Oracle Database Vault - ユースケース(Part2)(2019年11月)_[](youtube:hh-cX-ubCkY)
*   _Oracle Database Vaultの理解(2019年3月)_[](youtube:oVidZw7yWIQ)

## 確認

*   **著者** - Database Security PM、Hakim Loumi氏
*   **貢献者** - Richard Evans
*   **最終更新者/日付** - Hakim Loumi、データベース・セキュリティPM - 2023年11月