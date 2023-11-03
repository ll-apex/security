# 開発者がOracle Databaseプロキシ認証を使用できる方法

## 概要

このワークショップでは、Oracleのデータベースでのプロキシ認証の機能について説明します。

_見積時間:_ 30分

_この演習でテストしたバージョン:_ Oracle DB 19.17

### 製品について

プロキシ認証では、ユーザー(プロキシ・ユーザー)が別のユーザー(ターゲット・ユーザー)にかわってデータベースに接続できます。このワークショップでは、開発者がプロキシ認証を使用して適切な使用方法、構成およびベスト・プラクティスを示します。

### 目標

開発者がアプリケーション・スキーマのパスワードを知らずにアプリケーション・データにアクセスできるようにします。作成した開発者は、アプリケーション・スキーマの権限を継承します。

### 前提条件

この演習では、次を想定しています。

*   Free Tier、有料またはLiveLabs Oracle Cloudアカウント
*   次を完了しました:
    *   演習: 設定の準備(_無料層_および_有料テナント_のみ)
    *   演習: 環境設定
    *   演習: 環境の初期化

## タスク1: proxy.tarファイルをローカル・ディレクトリにダウンロードします。

1.  OSユーザー**oracle**として_DBSec-Lab_ VMでターミナル・セッションを開き、`cd`コマンドを使用してlivelabsディレクトリに移動します。
    
        <copy>cd livelabs</copy>
        
    
    **ノート**: リモート・デスクトップ・セッションを使用している場合は、デスクトップの_ターミナル_・アイコンをダブルクリックしてセッションを起動します。
    
2.  Linuxコマンド'wget'を使用して、ラボのコマンドのバンドル(圧縮)ファイルをダウンロードします。
    
        <copy>wget https://objectstorage.us-ashburn-1.oraclecloud.com/p/ZSKnVs6L8sGvA4HkZJjxv2sEFuf-30BYhE1F6jMHeltJ0icC-CBtLUKZzVwNObww/n/oradbclouducm/b/dbsec_rich/o/proxy.tar</copy>
        
3.  ダウンロードしたtarをアーカイブ解除して、ディレクトリおよびスクリプトを展開します。
    
        <copy>tar xvf proxy.tar</copy>
        
4.  `cd`コマンドを使用して、プロキシ・ディレクトリに移動します。
    

    ````
    <copy>cd proxy</copy>
    ````
    

5.  `ls`コマンドを使用してファイルをリストします。

    ````
    <copy>ls</copy>
    ````
    

## タスク2: プロキシ・ラボ

1.  まず、開発者`DEV_DAVE`、アカウントを作成します。権限がないことに注意してください。このアカウントは、別のアカウントとしてプロキシされないかぎり認証できません。
    
        <copy>./proxy_create_dave.sh</copy>
        
    
    **出力:**
    
    ![DEV_DAVE作成済](./images/proxy_createdev_dave.png " ")
    
2.  `DEV_DAVE`アカウントを`EMPLOYEESEARCH_PROD`アプリケーション・スキーマであるかのようにプロキシに認可します。
    

    ````
    <copy>./proxy_auth_dave.sh</copy>
    ````
    **Output:**
    
    ![Authorize DEV_DAVE](./images/proxy_tasktwosteptwo.png " ")
    

3.  プロキシ・ユーザーが`EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES`表の機密データにアクセスするときに監査する統合監査ポリシーを作成します。
    
        <copy>./proxy_create_audit_policy.sh</copy>
        
    
    **出力:**
    
    ![統合監査ポリシーの作成](./images/proxy_createauditpolicy.png " ")
    
4.  統合監査ポリシーは、作成後に有効にする必要があることに注意してください。これにより、ポリシーを事前にステージングまたは準備できます。
    

    ````
    <copy>./proxy_enable_audit_policy.sh</copy>
    ````
    **Output:**
    
    ![Enable Unified Audit Policy](./images/proxy_enableauditpolicy.png " ")
    

5.  `DEV_DAVE`が認証できないことを確認します。このアカウントには権限がなく、そのユーザーとしてプロキシされるときに`EMPLOYEESEARCH_PROD`から権限を継承します。

    ````
    <copy>./proxy_test_as_dave.sh</copy>
    ````
    **Output:**
    
    ![Verify DEV_DAVE cannot authenticate](./images/proxy_proxytestasdave.png " ")
    

6.  ここで、`DEV_DAVE`が`EMPLOYEESEARCH_PROD`としてプロキシできることをデモンストレーションします。Daveは`EMPLOYEESEARCH_PROD`パスワードを知る必要はなく、Daveは自分のパスワードを使用します。ユーザー情報に`EMPLOYEESEARCH_PROD`スキーマが表示されます。SYS\_CONTEXT属性`proxy_user`を問い合せることで、プロキシ・ユーザーを確認できます。
    
        <copy>./proxy_query_employee_data.sh</copy>
        
    
    **出力:**
    
    ![DEV_DAVEプロキシ](./images/proxy_dev_daveproxy.png " ")
    
    ![DEV_DAVEプロキシ・パート2](./images/proxy_dev_daveproxyparttwo.png " ")
    
7.  プロキシ・ユーザー`DEV_DAVE`によって作成された監査データを表示します。
    
        <copy>./proxy_view_audit.sh</copy>
        
    
    **出力:**
    
    ![監査データの表示](./images/proxy_viewauditdata.png " ")
    

## タスク3: オプション・ラボ

この演習では、データベース・ユーザーがプロキシ・ユーザーかどうかに基づいて、セキュリティ要因を追加する方法を示します。`DEV_DAVE`などのプロキシ・ユーザーが`EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES`のソーシャル識別子列(`SSN`、`SIN`、`NINO`)を表示できないようにします。プロキシ・ユーザーのないスキーマは、これらの機密列を表示できる必要があります。Oracle Virtual Private Database (VPD)ポリシー(行レベル・セキュリティまたはファイングレイン・アクセス制御とも呼ばれる)を作成して、ソーシャル識別子列がプロキシ・ユーザーに返されないようにします。null値として返されます。

1.  `EMPLOYEESEARCH_PROD`と`DEV_DAVE`の両方について、機密列データが表示されることを示します。

    ````
    <copy>./proxy_query_employee_data.sh</copy>
    ````
    **Output:**
    
    ![Sensitive Column Data](./images/proxy_sensitivecolumndata.png " ")
    
    ![Sensitive Column Data Part Two](./images/proxy_sensitivecolumndataparttwo.png " ")
    

2.  表に現在のOracle Virtual Private Database (VPD)ポリシーがないことを確認します。
    
        <copy>./proxy_vpd_query_policies.sh</copy>
        
    
    **出力:**
    
    ![現在のVPDポリシーがありません](./images/proxy_verifyvpdpolicies.png " ")
    
3.  `EMPLOYEESEARCH_PROD` session\_userを識別するPL/SQLファンクションを作成し、セッション・ユーザーがプロキシ・ユーザーかどうかを指定します。スキーマのみの場合、ファンクションはSQL問合せに述語を追加しません。セッション・ユーザーがプロキシ・ユーザーの場合、SQL問合せに`1=0`を追加し、`SSN`、`SIN`および`NINO`列をNULL値として返します。
    
        <copy>./proxy_vpd_create_function.sh</copy>
        
4.  `EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES`表に適用するポリシーを作成します。
    
        <copy>./proxy_vpd_create_policy.sh</copy>
        
    
    **出力:**
    
    ![ポリシーの作成](./images/proxy_createpolicy.png " ")
    
5.  `EMPLOYEESEARCH_PROD`スキーマがソーシャル識別子列を表示できるが、プロキシ・ユーザーが表示できないことを確認します。
    
        <copy>./proxy_query_employee_data.sh</copy>
        
    
    **出力:**
    
    ![EMPLOYEESEARCH_PRODスキーマの検証](./images/proxy_verifyemployeesearch_prodschema.png " ")
    
    ![DEV_DAVEのEMPLOYEESEARCH_PRODスキーマの検証](./images/proxy_verifyemployeesearch_prodschemadave.png " ")
    
6.  プロキシ・ユーザーの問合せに関連する監査データを表示します。Oracle VPD述語(`RLS_INFO`)データが統合監査証跡で使用できることに注意してください。プロキシ・ユーザーのSQL問合せに適用されたポリシー・タイプおよびポリシー述語を確認できます。
    
        <copy>./proxy_view_audit.sh</copy>
        
    
    **出力:**
    
    ![監査データの表示](./images/proxy_viewauditdatarelatedtoproxy.png " ")
    

## タスク4: クリーン・アップ

1.  PL/SQLファンクションを削除し、プロキシをクリーン・アップします。
    
        <copy>./proxy_cleanup.sh</copy>
        
    
    **出力:**
    
    ![クリーンアップ](./images/proxy_cleanup.png " ")
    

## さらに学ぶ

技術文書:

*   [プロキシ認証](https://docs.oracle.com/en/database/oracle/oracle-database/19/jjdbc/proxy-authentication.html#GUID-07E0AF7F-2C9A-42E9-8B99-F2716DC3B746)
    
*   [Oracle Database 19cセキュリティ・ガイド](https://docs.oracle.com/en/database/oracle/oracle-database/19/dbseg/index.html#Oracle%C2%AE-Database)
    

## 確認

*   **著者** - 北米スペシャリスト・ハブ、ソリューション・エンジニア、Stephen Stuart & Noah Galloso氏
*   **コントリビュータ** - データベース・セキュリティ製品マネージャ、Richard C. Evans氏
*   **最終更新者/日付** - Stephen Stuart & Noah Galloso、2023年8月