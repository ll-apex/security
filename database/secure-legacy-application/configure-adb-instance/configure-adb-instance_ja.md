# Autonomous Databaseインスタンスの構成

## 概要

この演習では、Oracle Cloud Platform (OCI)を確認し、Autonomous Transaction Processingデータベース・インスタンス(ATP)を構成します。

Autonomous Transaction Processingデータベースの詳細は、[ここ](https://www.oracle.com/autonomous-database/autonomous-transaction-processing/)をクリックしてください。

### 目標

この演習では、次のタスクを実行します。

*   ATPデータベース・インスタンスを作成します。

### 前提条件

この演習では、次を想定しています。

*   Oracle Cloud Infrastructure (OCI)テナンシ・アカウント

## タスク1: ATPデータベース・インスタンスの作成

1.  OCIを開いた状態で、左上隅にあるハンバーガー・メニューを選択してATPポータルに移動します。これにより、**「Oracle Database」**、**「Autonomous Transaction Processing」**の順に選択します。
    
    ![「OCI」メニューから「ATP」を選択します。](images/select-atp-menu.png)
    
2.  **「Autonomous Databaseの作成」**を選択します。
    
    ![「Create Autonomous Database」の選択](images/create-autonomous-database.png)
    
3.  選択したコンパートメントを使用し、表示名とデータベース名として**MyHRAppDB**を入力します。
    
    ![データベース名の入力](images/myhrapp-db-name.png)
    
4.  管理者資格証明のパスワードを作成します。
    
    ![管理資格証明の入力](images/atp-password.png)
    
5.  Change network access to **allowed IPs and VCNs only** and change IP notation type to **CIDR Block. Input the CIDR value of 0.0.0.0/0 into the blank field.** Make sure that the option for **requiring mutual TLS (mTLS) authentication remains unchecked**.
    
    ![管理資格証明の入力](images/secure-access.png)
    
6.  **「含まれるライセンス」**を選択し、下部にある**「Autonomous Databaseの作成」**を選択します。
    
    ![一番下にある「ADBの作成」ボタン](images/create-atp.png)
    
7.  作成したATPインスタンスに戻ります。ページ上部で、**「DB接続」**を選択します。
    
    ![DB接続に移動します](images/db-connection.png)
    
8.  メニューの**「接続文字列」**セクションまで下にスクロールします。「TLS認証」で、**「相互TLS」**ではなく**「TLS」**が選択されていることを確認します。次に、最初の接続文字列を任意のクリップボードにコピーします。この文字列は、次のステップの特定の場所に保存します。
    
    ![接続文字列のコピー](images/copy-connection-string.png)
    

**次の演習に進みます。**

## 確認

*   **著者** - Ethan Shmargad氏、北米スペシャリスト・ハブ
*   **作成者** - シニア・プリンシパル・プロダクト・マネージャー、Richard Evans
*   **最終更新者/日付** - Ethan Shmargad、2022年9月