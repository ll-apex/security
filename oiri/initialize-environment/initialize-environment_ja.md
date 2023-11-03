# 環境の初期化

## 概要

この演習では、このワークショップを正常に実行するために必要なすべてのコンポーネントを確認して起動します。

_推定ラボ時間_: 20分

### 製品・技術について

Oracle Identity Role Intelligence (OIRI)は、エンタープライズITリソース内のユーザーのアクセス権限を自動的に管理する、強力かつ柔軟なエンタープライズ・アイデンティティ管理システムです。

### 目標

この演習では、次のことを行います。

*   ワークショップ環境を初期化します。
*   データベースのステータスを確認します。
*   12cドメインのステータスを確認します。

### 前提条件

この演習では、次を想定しています。

*   Free Tier、有料またはLiveLabs Oracle Cloudアカウント
*   次を完了しました:
    *   演習: 設定の準備(_無料層_および_有料テナント_のみ)
    *   演習: 環境設定

_ノート: **無料トライアル**・アカウントがある場合、無料トライアルの期限が切れると、アカウントは**Always Free**アカウントに変換されます。Always Free環境が使用可能でないかぎり、Free Tierワークショップは実行できません。**[Free TierのFAQページはここをクリックしてください。](https://www.oracle.com/cloud/free/faq.html)**_

## タスク1: 必要なプロセスが稼働中であることの検証

1.  リモート・デスクトップ・セッションにアクセスできるようになったら、次に示すように続行して環境を検証してから、後続の演習の実行を開始します。次のプロセスが稼働している必要があります。
    
    *   データベース・リスナー
    *   データベース・サーバー
    *   管理サーバー(管理サーバーの起動には約3-4分かかります)
2.  事前ロードされた右側のWeblogic 12cコンソールの_「Webブラウザ」_ウィンドウで、_「ユーザー名」_フィールドをクリックし、ログインする保存済資格証明を選択します。これらの資格証明は_Webブラウザ_内に保存されており、参照用に次に示されています。
    
        Username  weblogic
        Password  Welcome1
        
    
    ![Weblogicコンソールのログイン・ページ](images/oiri-vnc.png " ")
    
3.  成功したログインを確認します。すべてのプロセスが完全に開始されるまで、インスタンス・プロビジョニングから約5分かかることに注意してください。
    
    *   Weblogicコンソールで、_「環境」_の下の_「サーバー」_をクリックし、管理サーバーがRUNNING状態であることを確認します。 ![「環境」の下のサーバーをクリックします](images/oiri-landing.png " ")
    
    成功すると、上のページが表示され、その結果、環境の準備ができました。
    
    [次の演習に進む](#next)ことができます。
    
4.  まだログインできない場合や、次に示すURLからリロードした後にログイン・ページが機能しない場合は、ターミナル・セッションを開き、次に示すように続行してサービスを検証します。
    
        ```
        
    
    URL http://oiri.livelabs.oraclevcn.com:7001/console/login/ユーザー名weblogicパスワード Welcome1 \`\`\`
    
    *   データベースとリスナー
        
            <copy>
            sudo systemctl status oracle-database
            </copy>
            
        
        ![データベース・ステータスを検証するターミナル・ウィンドウ・コマンド](images/db.png " ")
        
    *   WLS管理サーバーおよびノード・マネージャ
        
            <copy>
            sudo systemctl status oiri-weblogic.service
            </copy>
            
        
        ![WLS管理サーバー・ステータスを検証するためのターミナル・ウィンドウ・コマンド](images/oiri-wls-service.png " ")
        
            <copy>
            sudo systemctl status oiri-node.service
            </copy>
            
        
        ![ノード・マネージャのステータスを検証するためのターミナル・ウィンドウ・コマンド](images/oiri-node-service.png " ")
        
5.  疑わしい出力、障害または停止コンポーネントが表示された場合は、対応するサービスを適宜再起動してください
    
    *   データベースとリスナー
        
            <copy>
            sudo systemctl restart oracle-database
            </copy>
            
    *   WLS管理サーバーおよびノード・マネージャ
        
            <copy>
            sudo systemctl restart oiri-weblogic.service
            </copy>
            
        
            <copy>
            sudo systemctl restart oiri-node.service
            </copy>
            

[次の演習に進む](#next)ことができます。

## 付録1: 起動サービスの管理

1.  データベース・サービス(データベースおよびリスナー)。
    
    *   開始
    
        <copy>sudo systemctl start oracle-database</copy>
        
    
    *   停止
    
        <copy>sudo systemctl stop oracle-database</copy>
        
    
    *   ステータス
    
        <copy>sudo systemctl status oracle-database</copy>
        
    
    *   再起動
    
        <copy>sudo systemctl restart oracle-database</copy>
        
2.  OIRIサービス(WLS管理サーバー)
    
    *   開始
    
        <copy>sudo systemctl start  oiri-weblogic.service</copy>
        
    
    *   停止
    
        <copy>sudo systemctl stop oiri-weblogic.service</copy>
        
    
    *   ステータス
    
        <copy>sudo systemctl status oiri-weblogic.service</copy>
        
    
    *   再起動
    
        <copy>sudo systemctl restart oiri-weblogic.service</copy>
        
3.  OIRIサービス(ノード・マネージャ・サービス)
    
    *   開始
    
        <copy>sudo systemctl start oiri-node.service</copy>
        
    
    *   停止
    
        <copy>sudo systemctl stop oiri-node.service</copy>
        
    
    *   ステータス
    
        <copy>sudo systemctl status oiri-node.service</copy>
        
    
    *   再起動
    
        <copy>sudo systemctl restart oiri-node.service</copy>
        

## 謝辞

*   **著者** - NATDソリューション・エンジニアリング、Anuj Tripathi、Brijith TG、Keerti R氏
*   **貢献者** - Keerti R、Brijith TG、Anuj Tripathi
*   **最終更新者/日付** - NATDソリューション・エンジニアリング、Indiradarshni B氏