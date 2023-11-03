# 環境の初期化

## 概要

この演習では、このワークショップを正常に実行するために必要なすべてのコンポーネントを確認して起動します。

見積時間: 10分。

### 目標

*   ワークショップ環境を初期化します。

### 前提条件

この演習では、次を想定しています。

*   Oracle Cloudアカウント
*   次を完了しました:
    *   演習: 設定の準備
    *   演習: 環境設定

## タスク1: 必要なプロセスが稼働中であることの検証

**ノート:**このワークショップで紹介したSSH端末タイプ・タスクのすべてのスクリーンショットは、このステップで説明するように、_MobaXterm_ SSHクライアントを使用して取得されました。そのため、グラフィカルリモートデスクトップセッション内からそのようなタスクを実行する場合は、_sudo su - oracle_を使用してユーザー _oracle_としてログインする必要のある手順をスキップします。これは、リモートデスクトップセッションがユーザー _oracle_の下にあるためです。

1.  リモート・デスクトップ・セッションにアクセスできるようになったら、次に示すように続行して環境を検証してから、後続の演習の実行を開始します。次のプロセスが稼働している必要があります。
    
    *   データベース・リスナー
    *   データベース・サーバー(emcdbおよびcdb1)
    *   Enterprise Manager - 管理サーバー(OMS)
    *   Enterprise Manager - 管理エージェント(emagent)
    *   Glassfishでの自分のHRアプリケーション
2.  右側のWebブラウザ・ウィンドウには、_Enterprise Manager_が事前ロードされたタブがあり、次の資格証明を使用してログインし、操作可能であることを検証します。リモートデスクトップへの初回ログイン時にログインページが表示されない場合は、リフレッシュして再ロードします。すべてのプロセスが完全に開始するには最大15分かかります。
    
        Username: <copy>sysman</copy>
        
    
        Password: <copy>Oracle123</copy>
        
    
    ![Enterprise Managerにログイン](images/em-login.png "Enterprise Managerにログイン")
    
3.  新しいブラウザ・タブを開き、次に示す_自分のHRアプリケーション_が正常にレンダリングされたことを確認します。
    
    *   PDB1
    
        Prod: <copy>http://dbsec-lab:8080/hr_prod_pdb1</copy>
        
    
        Dev: <copy>http://dbsec-lab:8080/hr_dev_pdb1</copy>
        
    
    *   PDB2
    
        Prod: <copy>http://dbsec-lab:8080/hr_prod_pdb2</copy>
        
    
        Dev: <copy>http://dbsec-lab:8080/hr_dev_pdb2</copy>
        
    
    すべてが成功すると、環境の準備が整います。
    
4.  前述のすべての_Enterprise Manager_およびすべてのリンクが正常にレンダリングされない場合は、ターミナル・セッションを開き、次に示すように続行してサービスを検証します。
    
    *   データベース・サービス(すべてのデータベースおよびStandard Listener)
        
            <copy>
            sudo systemctl status oracle-database
            </copy>
            
        
        ![DBサービスのステータス](images/db-service-status.png "DBサービスのステータス")
        
    *   DBSec-labサービス(Enterprise Manager 13cおよびMy HR Applications on Glassfish)
        
            <copy>
            sudo systemctl status oracle-dbsec-lab
            </copy>
            
        
        ![DBSecLabサービス・ステータス](images/dbsec-lab-service-status.png "DBSecLabサービス・ステータス")
        
5.  疑わしい出力、障害または停止コンポーネントが表示された場合は、対応するサービスを適宜再起動してください
    
    *   データベースとリスナー
        
            <copy>
            sudo systemctl restart oracle-database
            </copy>
            
    *   DBSec-labサービス
        
            <copy>
            sudo systemctl restart oracle-dbsec-lab
            </copy>
            

**次の演習に進む**ことができます。

## 付録1: 起動サービスの管理

1.  データベース・サービス(すべてのデータベースおよびStandard Listener)
    
    *   開始
    
        <copy>sudo systemctl start oracle-database</copy>
        
    
    *   停止
    
        <copy>sudo systemctl stop oracle-database</copy>
        
    
    *   ステータス
    
        <copy>sudo systemctl status oracle-database</copy>
        
    
    *   再起動
    
        <copy>sudo systemctl restart oracle-database</copy>
        
2.  DBSec-labサービス(Enterprise Manager 13cおよびMy HR Applications on Glassfish)
    
    *   開始
    
        <copy>sudo systemctl start oracle-dbsec-lab</copy>
        
    
    *   停止
    
        <copy>sudo systemctl stop oracle-dbsec-lab</copy>
        
    
    *   ステータス
    
        <copy>sudo systemctl status oracle-dbsec-lab</copy>
        
    
    *   再起動
    
        <copy>sudo systemctl restart oracle-dbsec-lab</copy>
        

## 付録2: 外部Webアクセス

ワークステーション/ラップトップなど、リモート・デスクトップ・セッションの外部の場所からログインする場合は、次の詳細を参照してください。

1.  Enterprise Manager 13cコンソール
    
        Username: <copy>sysman</copy>
        
    
        Password: <copy>Oracle123</copy>
        
    
        URL: <copy>http://<Your Instance public_ip>:7803/em</copy>
        
    
    *   _ノート:_次に示すように、「_接続がプライベートではありません_」というWebコンソールへのアクセス中にブラウザにエラーが表示されることがあります。無視して例外を追加して続行します。
    
    ![Enterprise Manager外部ログイン](images/login-em-external-1.png "Enterprise Manager外部ログイン") ![Enterprise Manager外部ログイン](images/login-em-external-2.png "Enterprise Manager外部ログイン")
    
2.  Glassfishでの自分のHRアプリケーション
    
    *   PDB1
        *   製品: `http://<YOUR_DBSECLAB-VM_PUBLIC-IP>:8080/hr_prod_pdb1`
        *   開発: `http://<YOUR_DBSECLAB-VM_PUBLIC-IP>:8080/hr_dev_pdb1` (bg: 赤)
    *   PDB2
        *   製品: `http://<YOUR_DBSECLAB-VM_PUBLIC-IP>:8080/hr_prod_pdb2` (メニュー: 赤)
        *   開発: `http://<YOUR_DBSECLAB-VM_PUBLIC-IP>:8080/hr_dev_pdb2` (bg: 赤とメニュー: 赤)

## 確認

*   **著者** - NA Technology、LiveLabs Platform Lead、Rene Fontcha氏
*   **貢献者** - Hakim Loumi
*   **最終更新者/日付** - 2022年4月、テクニカル・プログラム・マネージャ、Marion Smith