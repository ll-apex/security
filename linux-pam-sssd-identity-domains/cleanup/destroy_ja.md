# 環境クリーンアップ

## 概要

このラボでは、ライブ・ラボ全体のクリーン・アップ・アクティビティを実行する方法について説明します。

_予想時間:_ 10分

### 目標

*   アプリケーションおよびアイデンティティ・ドメインの手動による非アクティブ化
*   リソースのクリーンアップを実行するためにスタック1および2を破棄します。

## タスク1: 機密アプリケーションとアイデンティティ・ドメインの非アクティブ化

このタスクでは、両方のスタックを破棄する前に前提条件を実行します。

1.  ドメインに存在する_機密アプリケーション_の両方を手動で_非アクティブ化_します。
    
2.  OCIコンソールでアイデンティティ・ドメインを_非アクティブ化_します。
    
    ![クライアント・アプリケーション](./images/client-app.png "クライアント・アプリケーション")
    
    ![機密アプリ](./images/confidential-app.png "機密アプリ")
    
    ![非アクティブ化](./images/deactivate.png "非アクティブ化")
    

## タスク2: スタック1の破棄- リソースのクリーンアップを実行するために破棄します。

このタスクでは、**「デプロイ」**ラボの一部として作成されたすべてのリソースを削除します。

1.  Oracle Cloudへのログイン
    
2.  左隅のハンバーガーメニューを開きます。**「開発者サービス」**をクリックし、**「リソース・マネージャ」→「スタック」**を選択します。
    
    ![スタック](./images/stacks.png "スタック")
    
3.  **スタック1- デプロイ**を作成したコンパートメントを選択し、選択します。
    
    ![スタック](./images/stack.png "スタック")
    
4.  「**Destroy**」をクリックし、右下のプロンプトに従って再度確認します。
    
    ![破棄](./images/destroy.png "破棄")
    
5.  ジョブが完了するのを待機し、出力を確認します。
    
    ![ジョブ](./images/job.png "ジョブ") ![完了](./images/complete.png "完了")
    

## タスク3: スタック2の破棄- リソースのクリーンアップを実行するための構成。

これで、ワークショップ用にプロビジョニングされたすべてのリソースが正常に破棄されたので、**Stack -2 Configure**を安全に削除して、環境を元の状態に戻すことができます。

1.  左上にあるブレッドクラム・リンクに従って、**「スタックの詳細」**、**「その他のアクション」→「スタックの削除」**をクリックします。
    
    ![削除スタック](./images/delete-stack.png "削除スタック")
    
    ![削除](./images/delete.png "削除")
    

これでワークショップは終了です。

## 確認

*   **著者** - Gautam Mishra、Aqib Bhat
*   **貢献者** - Deepthi Shetty
*   **最終更新者/日付** - Gautam Mishra 2023年7月