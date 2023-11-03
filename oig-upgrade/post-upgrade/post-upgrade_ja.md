# アップグレード後のタスク

## 概要

このラボでは、サーバーを再起動してワンホップ・アップグレード・プロセスを完了するステップについて説明します。

_推定ラボ時間_: 20分

### 目標

この演習では、次のことを行います。

*   サーバーの再起動によるアップグレードの完了
*   アップグレード・プロセスの検証

### 前提条件

この演習では、次を想定しています。

*   Free Tier、有料またはLiveLabs Oracle Cloudアカウント
*   次を完了しました:
    *   演習: 設定の準備(_無料層_および_有料テナント_のみ)
    *   演習: 環境設定
    *   演習: 環境の初期化
    *   演習: アップグレード前の要件
    *   演習: ワンホップ・アップグレード

## タスク1: 12c管理サーバーおよび管理対象サーバーの起動

1.  12cドメイン・ディレクトリに移動し、次に示す順序でサーバーを起動します。
    
        <copy>cd /u01/oracle/middleware12c/user_projects/domains/iam12c_domain/bin</copy>
        
2.  管理サーバーの起動
    
        <copy>nohup ./startWebLogic.sh &</copy>
        
3.  管理サーバーが起動したら、ブラウザからWeblogicコンソールにアクセスします。ブックマーク_「ワークショップ・リンク」_をクリックし、_WLS12c_「または」をクリックして、次のURLをブラウザに貼り付けます。
    
        <copy>http://onehopiam:7005/console</copy>
        
    
        Username: <copy>weblogic</copy>
        
    
        Password: <copy>Welcom@123</copy>
        
4.  Weblogicコンソールで、_「環境」_の下の_「サーバー」_をクリックし、すべてのサーバー(OIM、SOA)がSHUTDOWN状態であることを確認します。
    
5.  ターミナルで別のタブを開きます。_`/u01/oracle/middleware12c/user_projects/domains/iam12c_domain/bin`_パスに移動し、SOAサーバーを起動します。
    
        <copy>nohup ./startManagedWebLogic.sh soa_server1 t3://onehopiam:7005 -Dbpm.enabled=true &</copy>
        
    
    Weblogicコンソールをリフレッシュし、SOAサーバーがRUNNING状態になったことを確認します。SOAサーバーの起動には約5分から8分かかる場合があります。
    
6.  ターミナルで別のタブを開きます。_`/u01/oracle/middleware12c/user_projects/domains/iam12c_domain/bin`_パスに移動し、OIMサーバーを起動します。
    
        <copy>nohup ./startManagedWebLogic.sh oim_server1 t3://onehopiam:7005 &</copy>
        
    
    これにより、OIMブートストラップ・プロセスが実行され、ブートストラップに成功した後、OIM管理対象サーバーが自動的に停止されます。
    
7.  OIMサーバーが自動的に停止したら、次のコマンドを発行して、OIMプロセスが実行されていないことを確認します。
    
        <copy>ps -ef |grep oim_server1</copy>
        
8.  SOAサーバーを停止します。_`/u01/oracle/middleware12c/user_projects/domains/iam12c_domain/bin`_パスに移動し、SOAサーバーを停止します。
    
        <copy>./stopManagedWebLogic.sh soa_server1</copy>
        
9.  管理サーバーの停止
    
        <copy>./stopWebLogic.sh</copy>
        

## タスク2: アップグレード・プロセスの検証

1.  _startDomain12c.sh_スクリプトを実行して、12cドメインを再起動します。管理サーバーの起動には約3-4分かかります。SOAおよびOIMサーバーの起動には約10分かかる場合があります。
    
        <copy>cd /u01/scripts</copy>
        
    
        <copy>./startDomain12c.sh</copy>
        
2.  ブラウザから12c Weblogicコンソールにアクセスします。ブックマーク_「ワークショップ・リンク」_をクリックし、_WLS12c_「または」をクリックして、次のURLをブラウザに貼り付けます。
    
        <copy>http://onehopiam:7005/console</copy>
        
    
        Username: <copy>weblogic</copy>
        
    
        Password: <copy>Welcom@123</copy>
        
    
    Weblogicコンソールで、_「環境」_の下の_「サーバー」_をクリックし、すべてのサーバー(OIM、SOA)がRUNNING状態であることを確認します。
    
3.  Identity Self Serviceコンソールにアクセスします。ブックマーク_「ワークショップ・リンク」_をクリックし、_OIG12c_「または」をクリックして、次のURLをブラウザに貼り付けます。
    
        <copy>http://onehopiam:14005/identity</copy>
        
    
        Username: <copy>xelsysadm</copy>
        
    
        Password: <copy>Welcom@123</copy>
        
4.  右上隅にある _xelsysadm_をクリックし、ドロップダウンから「_About_」をクリックします。OIMバージョンが12cであることを確認します。
    
    ![](images/1-identity.png)
    
5.  右上隅にある_「管理」_をクリックします。次に、_「ユーザー」_をクリックし、3人のユーザー_TUSER1、TUSER2、TUSER3_が11gから12cに移行されていることを確認します。
    
    ![](images/2-users.png)
    

Oracle Identity Manager 12cへのワンホップ・アップグレードが完了しました。

## 謝辞

*   **著者** - NATDソリューション・エンジニアリング、Anuj Tripathi、Brijith TG、Keerti R氏
*   **貢献者** - Keerti R、Brijith TG、Anuj Tripathi
*   **最終更新者/日付** - 2021年6月、NATDソリューション・エンジニアリング、Keerti R氏