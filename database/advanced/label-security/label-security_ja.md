# Oracle Label Security(OLS)

## 概要

このワークショップでは、Oracle Label Security (OLS)の様々な機能について説明します。これにより、ユーザーは、機密データを保護するためにこれらの機能を構成し、同意の追跡に役立ち、一般データ保護規則などの規制要件に基づく処理の制限を強制する方法を学習する機会を得ることができます。

_推定ラボ時間:_ 30分

_この演習でテストしたバージョン:_ Oracle DB 19.19

### ビデオ・プレビュー

_LiveLabs - Oracle Label Security(2022年5月)_のプレビューを見る[](youtube:7hBsg0ygZt4)

### 目標

この演習の目的は、Oracle Label Securityを使用して、一般データ保護規則の要件の下で同意を追跡し、処理の制限を適用する方法に関するガイダンスを提供することです。同様の機能を実現するために、異なるOLS戦略を使用できます。ここで説明する詳細は、単なる例として役立ちます。

### 前提条件

この演習では、次を想定しています。

*   Oracle Cloudアカウント
*   次を完了しました:
    *   演習: 設定の準備
    *   演習: 環境設定
    *   演習: 環境の初期化

### ラボのタイミング(推定)

| ステップ番号 | 機能 | 近似時間 |
| --- | --- | --- |
| 1 | シンプルなCRMアプリケーション | 10分間 |
| 2 | Glassfishアプリケーションの保護 | 20分 |

## タスク1: 単純なCRMアプリケーション

### **開始する前に**

アプリケーションごとに異なる目的があります。

*   **ユーザー・アプリケーション** - ユーザーがマーケティング、データ処理または忘れられる要求への同意をプリファレンスに設定するアプリケーション- ユーザー・ラベル: `NCNST::DP`で実行され、データベース・ユーザー: `APPPREFERENCE`を使用します
    
*   **電子メール・マーケティング** - データの処理および特に電子メール・マーケティングについて同意したユーザーのみにアクセスできるアプリケーション- ユーザー・ラベル: `CONS::EMAIL`で実行され、データベース・ユーザー: `APPMKT`を使用します
    
*   **ビジネス・インテリジェンス** - データの処理に同意したすべてのユーザーにアクセスできるアプリケーション- ユーザー・ラベルで実行: `CONS::DP`、データベース・ユーザー: `APPBI`
    
*   **匿名化** - ユーザー・レコードを匿名化し、データ・ラベルを`ANON::`に設定するバッチ・プロセス- ユーザー・ラベル: `FORGET::`で実行し、データベース・ユーザー: `APPFORGET`を使用します
    

### **ラボを歩く方法**

*   ラボ全体を最初から最後まで自動で実行するスクリプトを提供していますが、1つずつ開いてコード・ブロックを1つずつコピー/実行することを強くお薦めします。
*   これにより、この演習の構成要素をよりよく理解できるようになります。
*   スクリプトごとにスクリプトを実行する場合は、ログ・ファイル(.out)で詳細をいつでも確認できます。

### **ラボ**

1.  OSユーザー**oracle**として、_DBSec-Lab_ VMでターミナル・セッションを開きます
    
        <copy>sudo su - oracle</copy>
        
    
    **ノート**: リモート・デスクトップ・セッションを使用している場合は、デスクトップの_ターミナル_・アイコンをダブルクリックしてセッションを起動します。
    
2.  scriptsディレクトリへ移動します
    
        <copy>cd $DBSEC_LABS/label-security</copy>
        
3.  まず、Label Security環境を設定する必要があります。
    
        <copy>./ols_setup_env.sh</copy>
        
    
    ![オルス](./images/ols-001a.png "Label Security環境の設定") ![オルス](./images/ols-001b.png "Label Security環境の設定")
    
    **ノート:**
    
    *   このスクリプトは、`C##OSCAR_OLS`ユーザーを作成し、表を作成し、データをロードし、差異シナリオを示すために使用されるユーザーを作成し、OLSも構成および有効化します。
    *   このSQLスクリプトは、`load_crm_customer_data.sql`スクリプトを起動して、表`CRM_CUSTOMER`を`APPCRM`スキーマに作成し、**391行**を挿入します
    *   ステップごとに、実行したスクリプトの出力を確認できます(例: "`more ols_setup_env.out`")
4.  次に、ラベル・セキュリティ・ポリシーを作成します。ポリシーは、レベル、グループまたはコンパートメント(あるいはその両方)で構成されます。ポリシーの唯一の必須コンポーネントは少なくとも1つのレベルです
    
        <copy>./ols_create_policy.sh</copy>
        
    
    ![オルス](./images/ols-002.png "ラベル・セキュリティ・ポリシーの作成") ![オルス](./images/ols-002b.png "ラベル・セキュリティ・ポリシーの作成")
    
    **ノート:**
    
    *   このスクリプトは、ポリシー(レベル、グループおよびラベル)を作成し、ユーザーのレベルおよびグループを設定し、`APPCRM.CRM_CUSTOMER`表にポリシーを適用します
    *   ステップごとに、実行したスクリプトの出力を確認できます(例: "`more ols_create_policy.out`")
5.  次に、データにラベルを付ける必要があります...作成したポリシーを使用し、1つのレベル、オプションで1つ以上のコンパートメント、およびオプションで1つ以上のグループを適用します
    
        <copy>./ols_label_data.sh</copy>
        
    
    ![オルス](./images/ols-003.png "データのラベル付け")
    
    **ノート:**
    
    *   このスクリプトはデータ・ラベルを更新して、シナリオで使用されるラベルの多様性を作成します
    *   実際のシナリオでは、他の既存の表データ(他の列)に基づいてラベルを割り当てるラベル付け関数を作成することをお薦めします。
    *   ステップごとに、実行したスクリプトの出力を確認できます(例: "`more ols_label_data.out`")
6.  次に、ラベルのセキュリティの動作を確認します
    
        <copy>./ols_label_sec_in_action.sh</copy>
        
    
    ![オルス](./images/ols-004.png "「Label Security」を参照してください。")
    
    **ノート:**
    
    *   このスクリプトは、異なるアプリケーションが接続するように接続します
    *   各アプリケーションには、処理できるレコードのみが表示されます。
    *   例AppMKT (顧客の電子メール送信に使用されるアプリケーション)は、`CNST::EMAIL`というラベルのレコードのみを表示できます。`AppBI`は、`ANON`というラベルのレコードを表示でき、`CNST::ANALYTICS` (レベル`CNST`のラベルが付いた行、グループ分析の一部)は、`CNST::ANALYTICS,EMAIL`でも機能します)
    *   ステップごとに、実行したスクリプトの出力を確認できます(例: "`more ols_label_sec_in_action.out`")
7.  ここで、忘れられる **UserID(100)**のステータスを変更します
    
        <copy>./ols_to_be_forgotten.sh</copy>
        
    
    ![オルス](./images/ols-005.png "忘れられるようにUserID 100のステータスを変更します")
    
    **ノート:**
    
    *   このスクリプトは、忘れたレコードを処理するアプリケーションをシミュレートします
    *   Forgottenとマークされたレコードを表示するストアド・プロシージャを作成します(ラベルは`FRGT::`)。
    *   また、特定の顧客を忘れるために役立つプロシージャをAppPreferenceアプリケーション・スキーマの下に作成します。
    *   AppPreferenceはすべてのデータにアクセスでき、`forget_me(p_id)`プロシージャは特定のcustomerid行`FRGT::`にレコードを「同意」から「忘れられた」にラベル付けします。この例では、UserID(100)のステータスを変更します: `forget_me(100)`
    *   その後、このステータスが正しく「忘れられる」に変更されたことを確認します。
    *   ステップごとに、実行したスクリプトの出力を確認できます(例: "`more ols_to_be_forgotten.out`")
8.  最後に、環境をクリーン・アップできます(OLSポリシーおよびユーザーを削除します)。
    
        <copy>./ols_clean_env.sh</copy>
        
    
    ![オルス](./images/ols-006.png "環境のクリーン・アップ")
    

## タスク2: Glassfishアプリケーションの保護

1.  まず、Glassfishアプリケーション環境を設定し、OLS変更がアプリケーションにまだデプロイされていないことを確認します。
    
        <copy>./ols_setup_glassfish_env.sh</copy>
        
    
    ![オルス](./images/ols-007.png "HRアプリケーション環境の設定")
    
2.  次に、GlassfishのOLSポリシーを設定します
    
        <copy>./ols_setup_glassfish_policy.sh</copy>
        
    
    ![オルス](./images/ols-008a.png "HRアプリケーションのOLSポリシーの設定") ![オルス](./images/ols-008b.png "HRアプリケーションのOLSポリシーの設定")
    
    **ノート:**
    
    *   このスクリプトはOLSを有効にするため、DBを再起動します
    *   Then, it creates the OLS policy named `OLS_DEMO_HR_APP` as well as the levels (`PUBLIC`, `CONFIDENTIAL`, `HIGHLY CONFIDENTIAL`), compartments (`HR`, `FIN`, `IP`, `IT`) and the OLS groups (`GLOBAL`, `USA`, `CANADA`, `LATAM`, `EU`, `GERMAN`)
    *   また、使用されるデータ・ラベルも生成されます。
    *   これにより、`label_tag`に番号を割り当てることができます。
    *   ステップごとに、実行したスクリプトの出力を確認できます(例: "`more ols_setup_glassfish_policy.out`")
3.  **EMPLOYEESEARCHアプリケーション**環境の作成
    
        <copy>./ols_config_employeesearch_app.sh</copy>
        
    
    ![オルス](./images/ols-009.png "EMPLOYEESEARCHアプリケーション環境の作成")
    
    **ノート:**
    
    *   このスクリプトは、アプリケーション・ユーザー・ラベル`EMPLOYEESEARCH_PROD.DEMO_HR_USER_LABELS`のカスタム表を作成し、`EMPLOYEESEARCH_PROD.DEMO_HR_USERS`のすべての行を移入します
    *   また、このスクリプトでは、`CAN_CANDY`、`EU_EVAN`など、この演習で使用するいくつかの追加ユーザーを作成し、すべてのアプリケーション・ユーザーに適切なOLSユーザー・ラベルを付与します。
4.  _`http://dbsec-lab:8080/hr_prod_pdb1`_へのWebブラウザ・ウィンドウを開き、Glassfishアプリケーションにアクセスします
    
    **ノート:**リモート・デスクトップを使用していない場合は、_`http://<YOUR_DBSEC-LAB_VM_PUBLIC_IP>:8080/hr_prod_pdb1`_に移動してこのページにアクセスすることもできます
    
5.  _`can_candy`_として、パスワード_`Oracle123`_でアプリケーションにログインします。
    
        <copy>can_candy</copy>
        
    
        <copy>Oracle123</copy>
        
    
    *   **「従業員の検索」**を選択し、\[**検索**\]をクリックします
    *   OLSポリシーを有効にする前に結果を参照してください
    
    ![オルス](./images/ols-017.png "CAN_CANDYとして従業員を検索します。")
    
6.  ログアウトし、パスワード「_`Oracle123`_」で _`eu_evan`_としてログインします
    
        <copy>eu_evan</copy>
        
    
        <copy>Oracle123</copy>
        
    
    *   **「従業員の検索」**を選択し、\[**検索**\]をクリックします
    *   地理的な制限のないすべての従業員データを表示できます。
    
    ![オルス](./images/ols-018.png "EU_EVANとして従業員を検索します。")
    
7.  ターミナル・セッションに戻り、OLSポリシーを`EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES`表に適用します。
    
        <copy>./ols_apply_policy.sh</copy>
        
    
    ![オルス](./images/ols-010.png "OLSポリシーの適用")
    
    **ノート**: OLSポリシーが表に適用されると、認可されたラベルを持つユーザー(OLS権限)のみがデータを表示できます。
    
8.  ここで、`EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES`表を更新して、`OLSLABEL`列に適切なOLS数値ラベルを移入します。
    
        <copy>./ols_set_row_labels.sh</copy>
        
    
    ![オルス](./images/ols-011.png "表の更新")
    
    **ノート:**
    
    *   これは、表の`CITY`列に基づいて行います。
    *   たとえば、"`Berlin`"は、GERMANYグループに属するため、`P::GER`のOLSラベルを受け取ります
9.  ポリシー出力がどのように表示されるかを確認します(各ステップについて、実行したスクリプトの出力を確認できます。たとえば、"`more ols_verify_our_policy.out`")。
    
        <copy>./ols_verify_our_policy.sh</copy>
        
    
    ![オルス](./images/ols-012.png "OLSポリシーの確認")
    
    ...データを調べて、様々なデータ・ラベルと、それにアクセスしているアプリケーション・ユーザーに基づいてどのように表示されるかを示します。
    
    *   DB USerおよびスキーマ所有者`EMPLOYEESEARCH_PROD`用
    
    ![オルス](./images/ols-013.png "EMPLOYEESEARCH_PRODとして問い合せます。")
    
    *   アプリケーション・ユーザー`HRADMIN`用
    
    ![オルス](./images/ols-014.png "HRADMINとして問合せ")
    
    *   アプリケーション・ユーザー`EU_EVAN`用
    
    ![オルス](./images/ols-015.png "EU_EVANとして問い合せます。")
    
    *   アプリケーション・ユーザー`CAN_CANDY`用
    
    ![オルス](./images/ols-016.png "CAN_CANDYとして問い合せます。")
    
10.  最後に、Glassfishアプリケーション構成ファイルを変更してOLSポリシーを適用します...このスクリプトは、作成する必要があるすべての追加をステップ実行します
    
        <copy>./ols_upd_glassfish.sh</copy>
        
    
    ![オルス](./images/ols-019.png "HRアプリケーションの設定")
    
11.  Glassfishアプリケーションに戻り、パスワード_`Oracle123`_で_`can_candy`_としてログインします
    
        <copy>can_candy</copy>
        
    
        <copy>Oracle123</copy>
        
    
    *   **「従業員の検索」**を選択し、\[**検索**\]をクリックします
    *   これで、OLSポリシーを有効にした後、違いがあることがわかります。`CAN_CANDY`では、**カナダ・ラベル付きユーザー**のみを表示できます。
    
    ![オルス](./images/ols-020.png "CAN_CANDYとして従業員を検索します。")
    
12.  ログアウトし、パスワード「_`Oracle123`_」で _`eu_evan`_としてログインします
    
        <copy>eu_evan</copy>
        
    
        <copy>Oracle123</copy>
        
    
    *   **「従業員の検索」**を選択し、\[**検索**\]をクリックします
    *   `EU_EVAN`には、**EUラベル付きユーザー**のみが表示されます。
    
    ![オルス](./images/ols-021.png "EU_EVANとして従業員を検索します。")
    
13.  ログアウトし、パスワード「_`Oracle123`_」で _`hradmin`_としてログインします
    
        <copy>hradmin</copy>
        
    
        <copy>Oracle123</copy>
        
    
    *   **「従業員の検索」**を選択し、\[**検索**\]をクリックします
    *   それに応じて、`HRADMIN`は**すべてのユーザー**を表示できることに注意してください。
    
    ![オルス](./images/ols-022.png "HRADMINとして従業員を検索")
    
14.  演習が完了したら、ポリシーを削除し、Glassfish JSPファイルを元の状態にリストアできます。
    
        <copy>./ols_restore_glassfish_env.sh</copy>
        
    
    ![オルス](./images/ols-023a.png "envの復元") ![オルス](./images/ols-023b.png "envの復元")
    

次の演習に進むことができます。

## **付録**: 製品について

### **概要**

OLSは、行ラベルとユーザーのラベル認可を比較して、認可されたユーザーのみに簡単に機密情報を制限できるようにします。

これにより、異なる承認レベル(マネージャや営業担当など)を持つユーザーは、表内の特定のデータ行にアクセスできます。OLSポリシーは、1つ以上のアプリケーション表に適用できます。OLSの設計は、Oracle Virtual Private Database (VPD)に似ています。ただし、OLSはVPDとは異なり、アクセス調整関数、データ・ディクショナリ表およびポリシーベースのアーキテクチャをすぐに提供するため、カスタマイズされたコーディングが不要になり、複数のアプリケーションで使用できる一貫性のあるラベル・ベースのアクセス制御モデルが提供されます。

![オルス](./images/ols-concept.png "OLSコンセプト")

OLSは、政府および防衛機関にあるマルチレベル・セキュリティ(MLS)要件に基づいています。OLSソフトウェアはデフォルトでインストールされますが、自動的には有効になりません。OLSは、SQLPlusまたはOracle Database Configuration Assistant (DBCA)を使用して有効にできます。OLSのデフォルト管理者は、ユーザー`LBACSYS`です。OLSを管理するには、コマンドライン・レベルまたはOracle Enterprise Manager Cloud Controlで一連のPL/SQLパッケージとスタンドアロン・ファンクションを使用できます。OLSポリシーに関する情報を検索するには、`ALL_SA_*`、`DBA_SA_*`または`USER_SA_*`データ・ディクショナリ・ビューを問い合せます。

OLSポリシーには、次のようなコンポーネントの標準セットがあります。

*   **ラベル**: データおよびユーザー、ユーザーおよびプログラム・ユニットの認証のラベルは、保護されている特定のオブジェクトへのアクセスを制御します。ラベルは次のもので構成されます。
    *   **レベル**: レベルは、行に割り当てる機密性のタイプを指定します(たとえば、`SENSITIVE`や`HIGHLY SENSITIVE`など)。レベルは必須です。
    *   **コンパートメント(オプション)**: データは、同じレベル(たとえば、`PUBLIC`、`CONFIDENTIAL`および`SECRET`)でも、会社内の別のプロジェクト(たとえば、`ACME Merger`および`IT Security`)に属すことができます。コンパートメントはこの例のプロジェクトを表し、より正確なアクセス制御の定義に役立ちます。政府の環境でよく使われます。
    *   **グループ(オプション)**: グループは、データを所有またはアクセスする組織(`UK`、`US`、`Asia`、`Europe`など)を識別します。グループは、商用環境でも政府環境でも使用され、その柔軟性のためにコンパートメントの代わりによく使用されます。
*   **ポリシー**: ポリシーは、ラベル、ルール、認可および保護された表に関連付けられた名前です。

### **OLSを使用する利点**

OLSには、行レベル管理を制御するためのいくつかの利点があります。

*   行レベルのデータ分類を有効にし、データ分類とユーザー・ラベル認可またはセキュリティ・クリアランスに基づいて即時利用可能なアクセス調整を提供します。
*   ラベル認可またはセキュリティ・クリアをデータベース・ユーザーとアプリケーション・ユーザーの両方に割り当てることができます。
*   データ分類ラベルとユーザー・ラベル認可を定義および格納するためのAPIとグラフィカル・ユーザー・インタフェースの両方を提供します。
*   Oracle Database VaultおよびOracle Advanced Security Data Redactionと統合され、セキュリティ・クリアランスをDatabase Vaultコマンド・ルールとData Redactionポリシー定義の両方で使用できるようにします。

## 詳細

技術文書:

*   [Oracle Label Security 19c](https://docs.oracle.com/en/database/oracle/oracle-database/19/olsag/part1.html)

ビデオ:

*   _Oracle Label Securityについて(2020年4月)_[](youtube:o4-XpUQWfaM)

## 確認

*   **著者** - Database Security PM、Hakim Loumi氏
*   **貢献者** - Alan Williams
*   **最終更新者/日付** - Hakim Loumi、データベース・セキュリティPM - 2023年11月