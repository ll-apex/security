# 環境の初期化

## 概要

この演習では、IAM 11.1.2.3を12.2.1.4バージョンに正常にアップグレードするために必要なすべてのコンポーネントを確認および設定します。

_推定ラボ時間_: 30分

### 目標

*   IAM 11.1.2.3ベースライン・ワークショップ環境を初期化します。

### 前提条件

この演習では、次を想定しています。

*   Free Tier、有料またはLiveLabs Oracle Cloudアカウント
*   次を完了しました:
    *   演習: 設定の準備(_無料層_および_有料テナント_のみ)
    *   演習: 環境設定

## タスク1: 必要なプロセスが稼働していることの検証

1.  リモート・デスクトップ・セッションにアクセスできるようになったら、次に示すように続行して環境を検証してから、後続の演習の実行を開始します。次のプロセスが稼働している必要があります。
    
    *   データベース・リスナー
        *   リスナー
    *   データベース・サーバー・インスタンス
        *   オーク
    
    ![リモートデスクトップのランディングページ](./images/login.png " ")
    
2.  _ターミナル_・セッションから次を実行して、予想されるプロセスが稼働していることを確認します。
    
    *   データベース・サービス
        
            <copy>
            systemctl status oracle-database.service
            </copy>
            
        
        ![端末データベースサービスのステータスチェック](./images/db-service-status.png " ")
        
    *   IAMサービス
        
            <copy>
            systemctl status oracle-iam.service
            </copy>
            
        
        ![端末iamサービスのステータスチェック](./images/iam-service-status.png " ")
        
    
    前述のように、期待されるすべてのプロセスが出力に表示される場合、環境は次のタスクに対応できます。
    
3.  次の資格証明を使用して、リモート・デスクトップ・セッションの_「Chrome」_ウィンドウで事前起動されている_Identity Manager管理コンソール_にアクセスして、ベース・インストールをテストします。
    
        URL: <copy>http://wsidmhost.idm.oracle.com:7778/oim</copy>
        
    
        User: <copy>xelsysadm</copy>
        
    
        Password: <copy>IAMUpgrade12c##</copy>
        
    
    ![Identity Manager管理コンソール・ログイン](./images/login1.png " ")
    
    ![Identity Manager管理コンソールのホーム・ページ](./images/oim-landing.png " ")
    

## タスク2: ReadMe.txtおよびバイナリの確認

1.  環境の詳細をレビューして、設定についてさらに学習します。次に示すようにファイル・ブラウザに移動し、_ReadMe.txt_を開きます。
    
    ![readme.txtを確認してください](./images/review.png " ")
    
2.  便宜上、ワークショップ全体で必要なすべてのソフトウェア・バイナリがインスタンスにステージングされています。確認するには、次の詳細を参照してください
    
    *   12cバイナリの確認
    
        <copy>ls -ltrh /home/oracle/Downloads/12cbits </copy>
        
    
    *   11gバイナリの確認
    
        <copy>ls -ltrh /home/oracle/Downloads/11gbits </copy>
        
    
    ![バイナリのフォルダ構造](./images/review2.png " ")
    

[次の演習に進む](#next)ことができます。

## 付録1: 起動サービスの管理

1.  データベース・サービス(データベースおよびStandardリスナー)。
    
    *   起動
    
        <copy>
        sudo systemctl start oracle-database
        </copy>
        
    
    *   停止
    
        <copy>
        sudo systemctl stop oracle-database
        </copy>
        
    
    *   ステータス
    
        <copy>
        systemctl status oracle-database
        </copy>
        
    
    *   再起動
    
        <copy>
        sudo systemctl restart oracle-database
        </copy>
        
2.  IAMサービス
    
    *   起動
    
        <copy>
        sudo systemctl start oracle-iam.service
        </copy>
        
    
    *   停止
    
        <copy>
        sudo systemctl stop oracle-iam.service
        </copy>
        
    
    *   ステータス
    
        <copy>
        systemctl status oracle-iam.service
        </copy>
        
    
    *   再起動
    
        <copy>
        sudo systemctl restart oracle-iam.service
        </copy>
        

## さらに学ぶ

Oracle Identity and Access Managementの詳細は、次のリンクを参照してください。

*   [Oracle Identity Management 12.2.1.1.4.0](https://docs.oracle.com/en/middleware/idm/suite/12.2.1.4/index.html)。

## 謝辞

*   **著者** - クラウド・プラットフォームCOE、ディレクタ、Anbu Anbarasu氏
*   **コントリビュータ** - Eric Pollard - 継続的なエンジニアリング、Ajith Puthan - IAMサポート、Rene Fontcha
*   **最終更新者/日付** - Sahaana Manavalan氏、LiveLabs Developer、NA Technology、2022年5月