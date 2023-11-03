# Glassfishアプリケーションのデータのロードおよび検証

## 概要

この演習では、Glassfishアプリケーションにデータを移入し、HRアプリケーションが引き続き適切に機能することを確認します。これには、ATPインスタンスへの`EMPLOYEESEARCH_PROD`スキーマ・オブジェクトのロードが含まれます。

### 目標

この演習では、次のタスクを実行します。

*   Glassfishアプリケーション・サーバーの`SQL*Plus`を使用して、`EMPLOYEESEARCH_PROD`スキーマを作成します。
*   接続文字列を更新します。
*   Glassfishアプリケーションを起動します。
*   Glassfishアプリケーションの**パブリックIP**を使用して、HRアプリケーション機能を確認します。

### 前提条件

この演習では、次を想定しています。

*   Oracle Cloud Infrastructure (OCI)テナンシ・アカウント
*   前の演習の完了: Autonomous Databaseインスタンスの構成、レガシーGlassfish HRアプリケーションへの接続

## タスク1: Glassfishアプリケーション・サーバーからSQL\*Plusを使用してEMPLOYEESEARCH\_PRODスキーマを作成し、Glassfishアプリケーションを起動します。

1.  `load_app_data.sh`スクリプトを実行して、ATPインスタンスにデータをロードします。
    
        <copy>./load_app_data.sh</copy>
        
    
    ![アプリケーション・データのロード](images/load-app-data.png)
    
2.  `update_app_connection_string.sh`スクリプトを使用して、アプリケーション接続文字列を更新します。
    
        <copy>./update_app_connection_string.sh</copy>
        
    
    ![接続文字列の更新](images/update-connection-string.png)
    
3.  `startGlassfish.sh`スクリプトを使用してGlassfishアプリケーションを起動します。
    
        <copy>./startGlassfish.sh</copy>
        
    
    ![Glassfishアプリケーションの起動](images/glassfish-start.png)
    
    _ノート: 「本番」または「開発」リンクをクリックすると、次のステップの後まではアクセスできず、ポート8080でイングレスが許可されます_
    

## タスク2: GlassfishアプリケーションのパブリックIPを使用したHRアプリケーション機能の確認

1.  **「コンピュート」→「インスタンス」**の下のハンバーガー・メニューを使用して、Cloud Shell端末を最小化し、OCIのGlassfishアプリケーション・インスタンスに戻ります。
    
    ![実行中のインスタンス](images/instance-running.png)
    
2.  **「プライマリVNIC」**セクションで、作成したサブネットを選択します。
    
    ![サブネットの検索](images/subnet.png)
    
3.  セキュリティ・リストで、サブネットの**デフォルト・セキュリティ・リスト**を選択します。
    
    ![欠陥SLの選択](images/default-list.png)
    
4.  「イングレス・ルール」で、**「イングレス・ルールの追加」**を選択します。
    
5.  次のイメージに従って情報を入力し、**「イングレス・ルールの追加」**を選択します。
    
    ![イングレス・ルールの追加](images/add-ingress.png)
    
6.  クラウド・シェル・ターミナルに戻ります。`startGlassfish.sh`スクリプトの出力を検索し、出力の最後に表示される**本番**と**開発**の両方のURLを見つけます。
    
    ![Glassfishアプリケーションの起動](images/glassfish-start.png)
    
    ![myhrappを開く](images/front-page-prod.png)
    
    ![myhrappを開く](images/front-page-dev.png)
    

**本番環境と開発環境の両方に次のステップを適用します。**

7.  ページ上部のメニュー・バーに移動して、**「ログイン」**を選択します。Glassfishアプリケーションにサインインするには、次のログイン情報を使用します。
    
        Username:<copy>hradmin</copy>
        
    
        Password:<copy>Oracle123</copy>
        
8.  ログインしたら、右側のメニュー・バーの**「従業員の検索」**に移動して、データがGlassfishアプリケーションにあることを確認します。検索パラメータで、**「アクティブ」**行を見つけて**「アクティブ」**を選択し、**「検索」**を選択してデータを表示します。
    
    ![従業員の検索](images/search-emp.png)
    
    ![アクティブな従業員の選択](images/select-active.png)
    
    ![myhrappを開く](images/verify-data.png)
    
9.  ページ上部のメニューを使用して、**「ホーム」**を選択してホームページに戻ります。
    

**次の演習に進みます。**

## 謝辞

*   **著者** - Ethan Shmargad氏、北米スペシャリスト・ハブ
*   **コントリビュータ** - シニア・プリンシパル・プロダクト・マネージャー、Richard Evans
*   **最終更新者/日付** - Ethan Shmargad、2022年9月