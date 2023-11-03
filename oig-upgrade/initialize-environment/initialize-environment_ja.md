# 環境の初期化

## 概要

この演習では、このワークショップを正常に実行するために必要なすべてのコンポーネントを確認して起動します。

_推定ラボ時間_: 20分

### 製品・技術について

Oracle Identity Governance(OIG)は、エンタープライズITリソース内のユーザーのアクセス権限を自動的に管理する、強力かつ柔軟なエンタープライズ・アイデンティティ管理システムです。

### 目標

この演習では、次のことを行います。

*   ワークショップ環境を初期化します。
*   データベースのステータスを確認します。
*   アップグレード可能な11gドメインのステータスを確認します。
*   12cドメインのステータスを確認します。

### 前提条件

この演習では、次を想定しています。

*   Free Tier、有料またはLiveLabs Oracle Cloudアカウント

_ノート: **無料トライアル**・アカウントがある場合、無料トライアルの期限が切れると、アカウントは**Always Free**アカウントに変換されます。Always Free環境が使用可能でないかぎり、Free Tierワークショップは実行できません。**[Free TierのFAQページはここをクリックしてください。](https://www.oracle.com/cloud/free/faq.html)**_

## タスク1: 必要なプロセスが稼働中であることの検証

1.  リモート・デスクトップ・セッションにアクセスできるようになったら、次に示すように続行して環境を検証してから、後続の演習の実行を開始します。次のプロセスが稼働している必要があります。
    
    *   データベース・リスナー
    *   データベース・サーバー
    *   管理サーバー(管理サーバーの起動には約3-4分かかります)
    *   OIMおよびSOAサーバー(SOAおよびOIMサーバーの起動に10分から12分かかる)
2.  右側の事前ロードされたWeblogic 11gコンソールの_「Firefox」_ウィンドウで、ページをリフレッシュするか、2-3分待って管理サーバーを起動してから、_「ユーザー名」_フィールドをクリックし、ログインする保存済資格証明を選択します。これらの資格情報は _Firefox_内に保存されており、参照用に以下に記載されています。
    
        Username: <copy>weblogic</copy>
        
    
        Password: <copy>Welcom@123</copy>
        
    
    ![](images/oig-vnc.png " ")
    
3.  管理サーバーの起動には約3-4分かかります。SOAおよびOIMサーバーの起動には約10分から12分かかる場合があります。
    
    *   Weblogic 11gコンソールで、_「環境」_の下の_「サーバー」_をクリックし、すべてのサーバー(OIM、SOA)がRUNNING状態であることを確認します。 ![](images/oig-vnc2.png " ")
        
    *   右側の事前ロードされた3番目のタブの_「Firefox」_ウィンドウには、Weblogic 12cコンソールがあります。
        
            Username: <copy>weblogic</copy>
            
        
            Password: <copy>Welcom@123</copy>
            
    
    ![](images/oig-vnc3.png " ")
    
    *   _「環境」_の下の_「サーバー」_をクリックし、すべてのサーバー(OIM、SOA)がRUNNING状態であることを確認します。 ![](images/oig-vnc4.png " ")
    
    成功すると、上のページが表示され、すべてのサーバーが実行され、環境の準備ができました。
    
4.  まだログインできないか、_「ワークショップ・リンク」_ブックマーク・フォルダからリロードした後にログイン・ページが機能しない場合は、端末セッションを開き、次に示すように続行してサービスを検証します。
    
    *   データベースとリスナー
        
            <copy>
            sudo systemctl status oracle-database
            </copy>
            
        
        ![](images/1-database.png " ")
        
    *   WLS管理サーバー、OIMサーバー
        
            <copy>
            sudo systemctl status oig-11g.service
            </copy>
            
        
        ![](images/oig-11gservice.png " ")
        
            <copy>
            sudo systemctl status oig-12c.service
            </copy>
            
        
        ![](images/oig-12cservice.png " ")
        

## タスク2: 11gおよび12cドメインの確認

1.  Identity Self Serviceコンソールにアクセスします。ブックマーク_「ワークショップ・リンク」_をクリックし、_OIG11g_「または」をクリックして、次のURLをブラウザに貼り付けます。
    
        <copy>http://onehopiam:14000/identity</copy>
        
    
        Username: <copy>xelsysadm</copy>
        
    
        Password: <copy>Welcom@123</copy>
        
    
    ![](images/5-identity-console.png)
    
2.  右上隅にある _xelsysadm_をクリックし、ドロップダウンから「_About_」をクリックします。OIMバージョンが11gであることを確認します。
    
    ![](images/6-identity-console.png)
    
3.  右上隅にある_「管理」_をクリックします。次に、_「ユーザー」_をクリックし、3人のテスト・ユーザーが作成されたことを確認します(_TUSER1、TUSER2、TUSER3_)。
    
    ![](images/7-users.png)
    
    ![](images/8-users.png)
    

4.  Identity Self Service OIG-12cコンソールにアクセスします。ブックマーク_「ワークショップ・リンク」_をクリックし、_OIG12c_「または」をクリックして、次のURLをブラウザに貼り付けます。
    
        <copy>http://onehopiam:14005/identity</copy>
        
    
        Username: <copy>xelsysadm</copy>
        
    
        Password: <copy>Welcom@123</copy>
        
    
    ![](images/11-oim12c.png)
    
5.  右上隅にある _xelsysadm_をクリックし、ドロップダウンから「_About_」をクリックします。OIMバージョンが12cであることを確認します。
    
    ![](images/12-oim12c.png)
    
6.  右上隅にある_「管理」_をクリックします。次に、_「ユーザー」_をクリックし、新規ユーザーが作成されていないことを確認します。
    
    ![](images/13-oim12c.png)
    

[次の演習に進む](#next)ことができます。

## 付録1: 起動サービスの管理

1.  データベース・サービス(データベースおよびリスナー)。
    
        Start: <copy>sudo systemctl start oracle-database</copy>
        
    
        Stop: <copy>sudo systemctl stop oracle-database</copy>
        
    
        Status: <copy>sudo systemctl status oracle-database</copy>
        
    
        Restart: <copy>sudo systemctl restart oracle-database</copy>
        
2.  OIGサービス- 11gバージョン(WLS管理サーバーOIGサーバー)
    
        Start: <copy>sudo systemctl start oig-11g.service</copy>
        
    
        Stop: <copy>sudo systemctl stop oig-11g.service</copy>
        
    
        Status: <copy>sudo systemctl status oig-11g.service</copy>
        
    
        Restart: <copy>sudo systemctl restart oig-11g.service</copy>
        
3.  OIGサービス- 12cバージョン(WLS管理サーバーOIGサーバー)
    
        Start: <copy>sudo systemctl start oig-12c.service</copy>
        
    
        Stop: <copy>sudo systemctl stop oig-12c.service</copy>
        
    
        Status: <copy>sudo systemctl status oig-12c.service</copy>
        
    
        Restart: <copy>sudo systemctl restart oig-12c.service</copy>
        

## 確認

*   **著者** - NATDソリューション・エンジニアリング、Anuj Tripathi、Brijith TG、Keerti R氏
*   **貢献者** - Keerti R、Brijith TG、Anuj Tripathi
*   **最終更新者/日付** - Ashish Kumar、NATDソリューション・エンジニアリング、2021年6月