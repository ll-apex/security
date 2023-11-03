# Autonomous Database環境の構成

## 概要

この演習では、Oracle Cloud Platform (OCI)を確認し、Autonomous Transaction Processingデータベース・インスタンス(ATP)を構成します。その後、`employee_data_load.sql`スクリプトを使用して`EMPLOYEESEARCH_PROD`ユーザーを作成し、**Oracle Database Actions**を使用してそのデータベース・スキーマにHR従業員データを移入します。

Autonomous Transaction Processingデータベースの詳細は、[ここ](https://www.oracle.com/autonomous-database/autonomous-transaction-processing/)をクリックしてください。

OCIデータベース・アクションの詳細は、[ここ](https://www.oracle.com/database/sqldeveloper/technologies/db-actions/)をクリックしてください。

見積時間: 10分

### 目標

この演習では、次のタスクを実行します。

*   ATPデータベース・インスタンスを作成します。
*   `employee_data_load.sql`スクリプトを使用して、`EMPLOYEESEARCH_PROD`ユーザーを作成し、データをアップロードします。

### 前提条件

この演習では、次を想定しています。

*   Oracle Cloud Infrastructure (OCI)テナンシ・アカウント
*   **Oracle Data Redactionを使用したREST GETコールの機密データの保護**のLiveLabワークショップでこれまでのすべてのラボを完了

## タスク1: ATPデータベース・インスタンスの作成

1.  OCIコンソールを開いた状態で、左上隅のハンバーガー・メニューを選択してATPポータルにナビゲートします。これにより、**「Oracle Database」**、**「Autonomous Transaction Processing」**の順に選択します。
    
    ![「OCI」メニューから「ATP」を選択します。](images/select-the-atp-menu.png)
    
2.  **「Autonomous Databaseの作成」**を選択します。
    
    ![「Create Autonomous Database」の選択](images/create-autonomous-database.png)
    
3.  選択したコンパートメントを使用し、表示名とデータベース名として**RedactionDB**を入力します。
    
    ![データベース名の入力](images/db-name.png)
    
4.  **ADMIN**資格証明のパスワードを作成します。
    
    ![管理資格証明の入力](images/atp-password.png)
    
5.  Change network access to **allowed IPs and VCNs only** and change IP notation type to **CIDR Block. Input the CIDR value of 0.0.0.0/0 into the blank field.** Make sure that the option for **requiring mutual TLS (mTLS) authentication remains unchecked**.
    
    ![管理資格証明の入力](images/secure-access.png)
    
6.  選択したライセンス・オプションを選択し、下部にある**「Autonomous Databaseの作成」**を選択します。_ノート: ADBのスピン・アップには数分かかる場合があります。_
    
    ![一番下にある「ADBの作成」ボタン](images/create-the-atp.png)
    

## タスク2: `employee_data_load.sql`スクリプトを使用して、`EMPLOYEESEARCH_PROD`ユーザーを作成し、データをアップロードします。

1.  Autonomous Databaseが緑色で使用可能になったら、Autonomous Databaseダッシュボードの上部メニュー・バーに移動し、**「データベース・アクション」**を選択します。
    
    ![データベース・アクションに移動](images/db-actions.png)
    
2.  作成した**ADMIN**資格証明を使用して、**データベース・アクション**にログインします。
    
    ![dbアクションにログインします。](images/db-login.png)
    
3.  **「開発」**セクションで、**「SQL」**を選択します。
    
    ![SQLワークシート](images/sql-worksheet.png)
    
4.  次のURLを使用して、`employee_data_load.sql`スクリプトをダウンロードして保存します。
    
        <copy>https://objectstorage.us-ashburn-1.oraclecloud.com/p/VEKec7t0mGwBkJX92Jn0nMptuXIlEpJ5XJA-A6C9PymRgY2LhKbjWqHeB5rVBbaV/n/c4u04/b/livelabsfiles/o/data-management-library-files/employee_data_load.sql</copy>   
        
5.  メニュー・バーの上部で、フォルダ・アイコンを選択してファイルを開きます。
    
    ![ファイルを開く](images/folder-icon.png)
    
6.  **「ファイルを開く」**を選択し、`employee_data_load.sql`スクリプトをアップロードします
    
    ![ファイルの選択](images/open-file.png)
    
7.  スクリプトが**SQLワークシート**にロードされたら、SQLスクリプトを実行するスクリプト・アイコンを選択します。下部のスクリプト出力をチェックして、エラーを受信しなかったことを確認します。
    
    ![スクリプトの実行](images/run-script.png)
    
8.  画面の左上にある**Oracle**ロゴを選択して、**データベース・アクション**のメイン・ダッシュボードに戻ります
    
    ![ダッシュボードに戻る](images/return-to-dash.png)
    
9.  下にスクロールします。**「管理」**で**「DATABASE USERS」**を選択します。
    
    ![データベース・ユーザーの選択](images/select-db-users.png)
    
10.  ユーザー`EMPLOYEESEARCH_PROD`で、省略記号を選択して**「ユーザーの編集」**をクリックします。
    
    ![ユーザーの編集](images/edit-user.png)
    
11.  **「ユーザー」**で、ポップアップ・メニューの右下にある**「変更の適用」**の選択が**オン**に**「Webアクセス」**が選択されていることを確認します。
    
    ![Webアクセスの切替え](images/web-access.png)
    
12.  `EMPLOYEESEARCH_PROD`の**データベース・アクション**・ポータルを開きます。
    
    ![DB処理を従業員としてオープン](images/db-actions-emp.png)
    
13.  次の資格証明を使用して、`EMPLOYEESEARCH_PROD`として**データベース・アクション**にログインします:
    
        <copy>EMPLOYEESEARCH_PROD</copy>   
        
    
        <copy>Oracle123+Oracle123+</copy>
        

**次の演習に進みます。**

## 確認

*   **著者** - Alpha Diallo & Ethan Shmargad、北米スペシャリスト・ハブ
*   **作成者** - Pedro Lopes、データベース・セキュリティ製品マネージャー
*   **最終更新者/日付** - Alpha Diallo & Ethan Shmargad、2023年2月