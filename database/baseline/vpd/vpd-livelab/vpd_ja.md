# Oracle VPDを使用した行レベルおよび列レベルのセキュリティによるデータの保護

## 概要

Oracle Virtual Private Databaseは、データベース表、ビューまたはシノニムに対して直接、詳細なレベルのセキュリティを適用します。これらのデータベース・オブジェクトにセキュリティ・ポリシーを直接アタッチすると、ユーザーがデータにアクセスするたびにポリシーが自動的に適用されるため、セキュリティをバイパスできません。

説明: このラボでは、Oracle Virtual Private Database(VPD)の機能を紹介します。これにより、行および列レベルのセキュリティを実装するためにこの機能を構成する方法を学習できます。Oracle VPDは、行および列レベルでデータベース・アクセスを制御するセキュリティ・ポリシーを作成します。

_見積時間:_ 30分

_この演習でテストしたバージョン:_ Oracle DB 19.17

### 目標

*   VPDポリシーで使用するPL/SQLファンクションの作成方法の理解
*   セッション・ユーザーおよびクライアント識別子に基づいて返される行を制限します。
*   機密列データがSQL\*Plus問合せに表示されないようにします。

### 前提条件

この演習では、次を想定しています。

*   Free Tier、有料またはLiveLabs Oracle Cloudアカウント
*   次を完了しました:
    *   演習: 設定の準備(_無料層_および_有料テナント_のみ)
    *   演習: 環境設定
    *   演習: 環境の初期化

## タスク1: vpd.tarファイルをローカル・ディレクトリにダウンロードします。

1.  OSユーザー**oracle**として_DBSec-Lab_ VMでターミナル・セッションを開き、`cd`コマンドを使用してlivelabsディレクトリに移動します。
    
        <copy>cd livelabs</copy>
        
    
    **ノート**: リモート・デスクトップ・セッションを使用している場合は、デスクトップの_ターミナル_・アイコンをダブルクリックしてセッションを起動します。
    
2.  Linuxコマンド'wget'を使用して、ラボのコマンドのバンドル(圧縮)ファイルをダウンロードします。
    
        <copy>wget https://objectstorage.us-ashburn-1.oraclecloud.com/p/AC5TmVamZJxZfPG21L3cjuGER4BBS5lvMS80Du3DOwkkVtwoySpGfPIdo4Zf9knM/n/oradbclouducm/b/dbsec_rich/o/dbsec-livelabs-vpd.tar</copy>
        
3.  ダウンロードしたtarをアーカイブ解除して、ディレクトリおよびスクリプトを展開します。
    
        <copy>tar xvf dbsec-livelabs-vpd.tar</copy>
        
4.  `cd`コマンドを使用して、vpdディレクトリに移動します。
    

    ````
    <copy>cd vpd</copy>
    ````
    

5.  `ls`コマンドを使用してファイルをリストします。

    ````
    <copy>ls</copy>
    ````
    

## タスク2: 行ファンクションおよびポリシーの作成

1.  VPDポリシーがまだ作成されていないことを示す最初の問合せ。
    
        <copy>./vpd_query_policies.sh</copy>
        
    
    **出力:**
    
        Show current VPD policies...
        
        no rows selected
        
2.  `EMPLOYEESEARCH_PROD`および`DBA_DEBRA`が従業員関連のデータを表示できることを示す最初の問合せ。列に返される行数および機密データに注意してください。
    
        <copy>./vpd_query_employee_data.sh</copy>
        
    
    **`EMPLOYEESEARCH_PROD`の出力:**
    
    ![最初のクエリー](./images/vpd_initialquery.png " ")
    
    **`DBA_DEBRA`の出力:**
    
    ![初期問合せDBA_DEBRA](./images/vpd_initialquerydebra.png " ")
    
3.  VPDは、ビジネス・ロジックのPL/SQLファンクションに依存します。セッション・ユーザーがアプリケーション所有者でない場合は、問合せに`1=0`述語(where句)を適用するファンクション`EMPLOYEESEARCH_PROD`を作成します。
    
        <copy>./vpd_create_row_function.sh</copy>
        
4.  PL/SQLファンクションをコールし、`SELECT`問合せの行を制限するVPDポリシーを`EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES`表に適用します。
    
        <copy>./vpd_create_row_policy.sh</copy>
        
    
    **出力:**
    
    ![行ポリシーの作成](./images/vpd_createrowpolicy.png " ")
    
5.  問合せを再実行して従業員データを表示します。VPD行ポリシーが適用されると、`EMPLOYEESEARCH_PROD`は引き続きすべての行を表示しますが、`DBA_DEBRA`は従業員データを表示できなくなります。
    
        <copy>./vpd_query_employee_data.sh</copy>
        
    
    **`EMPLOYEESEARCH_PROD`の出力:**
    
    ![行ポリシー問合せ](./images/vpd_rerunquery.png " ")
    
    **`DBA_DEBRA`の出力:** ![行ポリシー問合せDBA_DEBRA](./images/vpd_rerunquerydebra.png " ")
    
6.  VPDを使用して返される行数を制限する方法を理解したので、行ポリシーを削除し、列値の保護に進みます。
    
        <copy>./vpd_drop_row_policy.sh</copy>
        

## タスク3: 列関数およびポリシーの作成

1.  行ファンクションと同様に、PL/SQLファンクションは、アプリケーション・スキーマ所有者`EMPLOYEESEARCH_PROD`ではないユーザーに対して返される行数を制限します。また、アプリケーション・ユーザーであるファンクションは、ユーザーのセッション・コンテキストで`CLIENT_IDENTIFIER`値が設定されていることを確認します。
    
        <copy>./vpd_create_col_function.sh</copy>
        
2.  PL/SQL列ファンクションを使用してVPDポリシーを作成します。このポリシーは、`SSN`、`SIN`および`NINO`列に適用されます。
    
        <copy>./vpd_create_col_policy.sh</copy>
        
    
    **出力:**
    
    ![列ポリシー](./images/vpd_createvpdpolicy.png " ")
    
3.  `EMPLOYEESEARCH_PROD`がデータを問い合せると、9行が戻されますが、機密列の値は戻されません。これは、セッション・ユーザーと`CLIENT_IDENTIFIER`セッション・コンテキストの両方が満たされるまで、VPDポリシー・ファンクションがこれらの列の値を返さないためです。
    
        <copy>./vpd_query_employee_data.sh</copy>
        
    
    **`EMPLOYEESEARCH_PROD`の出力:** ![列ポリシー問合せ](./images/vpd_columnpolicyquery.png " ")
    
    **`DBA_DEBRA`の出力:**
    
    ![列ポリシー問合せDBA_DEBRA](./images/vpd_columnpolicyquerydebra.png " ")
    
4.  セッション・ユーザーと`CLIENT_IDENTIFIER`の両方が満たされた場合に結果を示すには、前の問合せに`hradmin`を追加します。機密列の値が表示されます。ただし、`DBA_DEBRA`はPL/SQLファンクションによって認可されていないため、このデータは表示されません。
    
        <copy>./vpd_query_employee_data.sh hradmin</copy>
        
    
    **`EMPLOYEESEARCH_PROD`の出力:**
    
    ![クライアント識別子問合せ](./images/vpd_clientidentifierquery.png " ")
    
    **`DBA_DEBRA`の出力:**
    
    ![クライアント識別子問合せDBA_DEBRA](./images/vpd_clientidentifierquerydebra.png " ")
    
5.  問合せを`hradmin`から`can_candy`に変更しても、機密列は表示されません。これは、PL/SQLファンクションが`can_candy`を`CLIENT_IDENTIFIER`として認識していないためです。
    
        <copy>./vpd_query_employee_data.sh can_candy</copy>
        
    
    **`EMPLOYEESEARCH_PROD`の出力:**
    
    ![Can_Candy識別子問合せ](./images/vpd_can_candyidentifierquery.png " ")
    
    **`DBA_DEBRA`の出力:**
    
    ![Can_Candy識別子問合せDBA_DEBRA](./images/vpd_can_candyidentifierquerydebra.png " ")
    
6.  PL/SQLファンクションを更新して`elsif`を追加し、`can_candy`がトロント・ベースの従業員の機密列を表示できるようにします。
    
        <copy>./vpd_update_col_function.sh</copy>
        
7.  `hradmin`には9つの行と機密列が表示されますが、`DBA_DEBRA`には表示されません。
    
        <copy>./vpd_query_employee_data.sh hradmin</copy>
        
    
    **`EMPLOYEESEARCH_PROD`の出力:**
    
    ![Hradmin識別子問合せ](./images/vpd_hradminidentifierquery.png " ")
    
    **`DBA_DEBRA`の出力:**
    
    ![Hradmin識別子問合せDBA_DEBRA](./images/vpd_hradminidentifierquerydebra.png " ")
    
8.  `can_candy`には、トロントに拠点を置く従業員の9行と機密列のみが表示されます。`DBA_DEBRA`には、機密列は表示されません。
    
        <copy>./vpd_query_employee_data.sh can_candy</copy>
        
    
    **`EMPLOYEESEARCH_PROD`の出力:**
    
    ![Can_Candy問合せの更新](./images/vpd_updatedcan_candyquery.png " ")
    
    **`DBA_DEBRA`の出力:**
    
    ![更新されたCan_Candy問合せDBA_DEBRA](./images/vpd_updatedcan_candyquerydebra.png " ")
    

## タスク4: クリーン・アップ

1.  PL/SQLファンクションを削除し、VPD関連のポリシーを削除します。
    
        <copy>./vpd_cleanup.sh</copy>
        
2.  `EMPLOYESEARCH_PROD`と`DBA_DEBRA`の両方が、VPDポリシーが設定されていない行および列へのフル・アクセス権を持っていることを確認します。
    
        <copy>./vpd_query_employee_data.sh</copy>
        
    
    **`EMPLOYEESEARCH_PROD`の出力:**
    
    ![クエリーをクリーンアップ](./images/vpd_cleanupquery.png " ")
    
    **`DBA_DEBRA`の出力:**
    
    ![問合せDBA_DEBRAのクリーンアップ](./images/vpd_cleanupquerydebra.png " ")
    

## さらに学ぶ

技術文書:

*   [Oracle Virtual Private Databaseを使用したデータ・アクセスの制御](https://docs.oracle.com/en/database/oracle/oracle-database/21/dbseg/using-oracle-vpd-to-control-data-access.html#GUID-06022729-9210-4895-BF04-6177713C65A7)

## 謝辞

*   **著者** - 北米スペシャリスト・ハブ、ソリューション・エンジニア、Stephen Stuart & Noah Galloso氏
*   **コントリビュータ** - データベース・セキュリティ製品マネージャ、Richard C. Evans氏
*   **最終更新者/日付** - Stephen Stuart & Noah Galloso、2023年8月