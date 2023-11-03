# Oracle Identity GovernanceからのOIRIへのデータのインポート

## 概要

データ・インポートは、データ収集とも呼ばれ、エンティティ・データをソースからOracle Identity Role Intelligence (OIRI)データベースにインポートするプロセスです。OIRIでは、データ収集コマンドライン・ツール(DING CLI)を使用して、サードパーティ・ソース(Oracle Identity Governance (OIG)データベースまたはフラット・ファイル)からエンティティ・データをフェッチおよびロードします。データ・インポート・プロセスの一環として、OIGデータベースやフラット・ファイルなどのソースのデータは、OIRIデータベースの`USERS`、`APPLICATIONS`、`ACCOUNTS`、`ENTITLEMENTS`、`ASSIGNED_ENTS`、`ROLES`、`ROLE_USER_MSHIP`、`ROLE_ENT_COMPOSI`、`ROLE_HIERARCHY`、`ORGANIZATIONS`の各表にロードされます。

_推定ラボ時間_: 30分

### 目標

この演習では、次のことを行います。

*   Oracle Identity Governanceデータベースからのデータ・インポートの構成
*   予行演習データ・インポートの実行
*   OIGデータベースからエンティティ・データを抽出してOIRIデータベース表にロードするための実際のデータ・インポート実行の実行
*   データ・インポート・プロセスの検証およびレビュー

### 前提条件

この演習では、次を想定しています。

*   Free Tier、有料またはLiveLabs Oracle Cloudアカウント
*   次を完了しました:
    *   演習: 設定の準備(_無料層_および_有料テナント_のみ)
    *   演習: 環境設定
    *   演習: 環境の初期化
    *   演習: KubernetesクラスタのデプロイとOIGサーバーの起動
    *   演習: ローカルKubernetesノードへのOIRIのデプロイ

## タスク1: データ・ロード・プロセスの開始

1.  K8Sマスターからca.crtをコピーします。
    
        <copy>cp /etc/kubernetes/pki/ca.crt /nfs/ding/</copy>
        
2.  ding-cli containerを実行し、既存のデータ・ロード構成を更新して、Oracle Identity Governanceデータベースからエンティティ・データをインポートします。
    
        <copy>docker exec -it ding-cli bash</copy>
        
    
        <copy>./updateDataIngestionConfig.sh --useoigdbforetl true --entityusersenabled true --entityuserssyncmode full --entityapplicationsenabled true --entityapplicationssyncmode full --useflatfileforetl false</copy>
        
    
    ![ding-cliコンテナを実行し、既存のデータ・ロード構成を更新してエンティティ・データをインポートするためのターミナルdockerコマンド](images/data-load.png)
    

## タスク2: ドライ・インポート実行の実行

1.  データ・インポート(またはデータ収集)の前に、予行演習を実行して、データがOIRIデータベースに適合しているかどうかを検証します。これにより、Oracle Identity Governanceデータベースからデータがフェッチされ、OIRIデータベースのメタデータに対して検証されます。
    
        <copy>ding-cli --config=/app/data/conf/config.yaml data-ingestion dry-run /app/data/conf/data-ingestion-config.yaml</copy>
        
2.  ドライ・データ・インポート・プロセスには、約8分から10分かかります。プロセスの実行中に、別の端末タブで次のコマンドを実行して、ネームスペースdingで生成されたポッドを確認できます。
    
        <copy>kubectl get pods -n ding</copy>
        
    
    ドライラン・タスクが完了すると、Dingドライバはステータス_「実行中」_から_「完了」_に移動します。
    
3.  次のステップに示すように、OIRIコンソールにサインインし、データ・インポート・プロセスをモニターします。
    

## タスク3: OIRIユーザー・インタフェースにサインインし、ドライ・データ・インポートを検証します。

1.  Identity Role Intelligenceユーザー・インタフェースにサインインします。ブラウザ・ウィンドウを起動し、次に示すURLを入力します。_「詳細」_をクリックし、「\*リスクを受け入れて続行」をクリックして警告メッセージを無視します。OIRIアカウントのサインイン・ページが表示されます。ユーザー名とパスワードを入力します。
    
        URL       https://oiri.livelabs.oraclevcn.com:30305/oiri/ui/v1/console/
        Username  xelsysadm
        Password  Welcome1
        
    
    ![OIRIコンソール・ホームページ](images/oiri.png)
    
2.  ページの左上にある「アプリケーション・ナビゲーション」メニュー・アイコンをクリックし、_「データのインポート」_をクリックして、すべてのデータ・インポート・タスクのリストが表示された_「データ・インポートの管理」_ページを開きます。
    
    ![ページの左上にある「Application Navigation」メニュー・アイコンをクリックします。](images/data-import.png)
    
    ![「データ・インポート」をクリックして、「データ・インポートの管理」ページを開きます。](images/manage-data-import.png)
    
3.  予行演習の実行中に、タスクの横に赤い時計アイコンがあることを確認します。データ・インポートの実行が正常に完了すると、緑色のチェックが表示され、_「結果の表示」_ボタンが使用可能になります。Oracle Identity Governanceからのデータのインポート(実行)タスクに対して_「結果の表示」_をクリックします。または、データ・インポート・タスク名をクリックすることもできます。_「結果の表示」_ウィンドウに、Oracle Identity Governanceデータベースからのデータ・インポートの結果が表示されます。
    
    ![インポート・データに対して「結果の表示」をクリックします。](images/result-data-import.png)
    
4.  各エンティティを展開して、重複データ数、データセットが有効かどうか、無効なデータ型の数など、そのエンティティのデータ・インポートの詳細を確認します。
    
    ![結果の表示](images/viewresult-data-import.png)
    
    ![結果の表示- ロール](images/role-data-import.png)
    
    ![結果の表示- アプリケーション](images/application-data-import.png)
    
    ![結果の表示- 権限](images/entitlement-data-import.png)
    
    ![結果の表示- ユーザー](images/user-data-import.png)
    
5.  「取消」をクリックして「結果の表示」ウィンドウを閉じます。
    

## タスク4: Oracle Identity Governanceからのデータ・インポート

1.  実際のデータ インポート プロセスを実行します。
    
        <copy>ding-cli --config=/app/data/conf/config.yaml data-ingestion start /app/data/conf/data-ingestion-config.yaml</copy>
        
2.  データ・インポート・プロセスには、約8分から10分かかります。プロセスの実行中に、別の端末タブで次のコマンドを実行して、ネームスペースdingで生成されたポッドを確認できます。
    
        <copy>kubectl get pods -n ding</copy>
        
    
    ![名前空間dingで生成されるポッドを監視するTerminalコマンド](images/getpods-data-import.png)
    
    ドライラン・タスクが完了すると、Dingドライバはステータス_「実行中」_から_「完了」_に移動します。
    

## タスク5: データ・インポート・タスクの検証

1.  ブラウザ・ウィンドウからOIRIコンソールにアクセスします。
    
2.  ページの左上にある「Application Navigation」メニュー・アイコンをクリックし、_「Data Import」_をクリックして、すべてのデータ・インポート・タスクのリストが表示された「Manage Data Import」ページを開きます。
    
3.  データ・インポートの実行中に、タスクの横に赤い時計アイコンがあることを確認します。データ・インポートの実行が正常に完了すると、緑色のチェックが表示され、_「結果の表示」_ボタンが使用可能になります。Oracle Identity Governanceからのデータのインポート・タスクに対して_「結果の表示」_をクリックします。または、データ・インポート・タスク名をクリックすることもできます。_「結果の表示」_ウィンドウに、Oracle Identity Governanceデータベースからのデータ・インポートの結果が表示されます。
    
    ![OIRI - 「結果の表示」ウィンドウ](images/view-data-import.png)
    
4.  各エンティティを展開して、そのエンティティのデータ・インポートの詳細を確認します。
    
    ![OIRI - データ・インポートの詳細をレビューします](images/review-data-import.png)
    
5.  「取消」をクリックして「結果の表示」ウィンドウを閉じます。
    

[次の演習に進む](#next)ことができます。

## 確認

*   **著者** - NATDソリューション・エンジニアリング、Anuj Tripathi、Brijith TG、Keerti R氏
*   **貢献者** - Keerti R、Brijith TG、Anuj Tripathi
*   **最終更新者/日付** - Indiradarshni B、NATDソリューション・エンジニアリング、2022年12月