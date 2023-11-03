# Oracle Data Safeイベントを設定して、ターゲット・データベースのセキュリティ・ドリフトに関する通知を取得します

この演習では、ターゲット・データベースにセキュリティ・ドリフトがある場合に電子メールで通知するようにイベント・サービスを構成します。

推定ラボ時間: 20分

## 目標

この演習では、次のことを行います。

*   最新のユーザー評価の確認
*   最新ユーザー評価をベースラインとして設定
*   通知トピックとサブスクリプションの作成
*   イベント・サービスでルールを作成します
*   ターゲット・データベースでのアクティビティの生成
*   最新のユーザー評価のリフレッシュと結果の分析
*   ユーザー・アセスメントの比較レポートの生成
*   Eメール通知のレビュー

### 前提条件

この演習では、次を想定しています。

*   Oracle Cloudアカウントを取得し、Oracle Cloud Infrastructure Consoleにサインインします。
*   テナンシ内の_管理者_権限
*   このワークショップのための環境の準備([環境の準備](?lab=prepare-environment)を参照)
*   ターゲット・データベースをOracle Data Safeに登録しました([Autonomous DatabaseのOracle Data Safeへの登録](?lab=register-autonomous-database)を参照)
*   有効な電子メール・アドレス

### 前提

*   実際のデータ値は、スクリーンショットに表示されている値と異なる場合があります。

## タスク1: 最新のユーザー評価の確認

1.  Oracle Cloud Infrastructureのナビゲーション・メニューから**「Oracle Database」**、**「データ・セーフ- データベース・セキュリティ」**の順に選択します。
    
2.  **「セキュリティ・センター」**で、**「ユーザー評価」**をクリックします。
    
    ユーザー評価ダッシュボードが表示されます。
    
3.  **「ターゲットのサマリー」**タブをクリックします。
    
4.  **「最終評価日」**列で、**「レポートの表示」**をクリックして最新のユーザー評価を表示します。
    
5.  **「概要」**タブでチャートを確認します。
    
    ![最新ユーザー・アセスメントの概要タブ](images/latest-ua-overview-tab.png "最新ユーザー・アセスメントの概要タブ")
    
6.  下にスクロールして、**「ユーザー詳細」**セクションの情報を確認します。
    
    ![最新ユーザー・アセスメント・ユーザー詳細セクション](images/latest-ua-user-details-section.png "最新ユーザー・アセスメント・ユーザー詳細セクション")
    

## タスク2: 最新のユーザー評価をベースラインとして設定する

1.  最新のユーザー評価を表示している間に、**「ベースラインとして設定」**をクリックします。
    
    **「ベースラインとして設定しますか?」**ダイアログ・ボックスが表示され、確認を求められます。
    
2.  **「はい」**をクリックし、次のメッセージが表示されるまでページに留まります。
    
    `Baseline has been set.`
    
3.  **「履歴の表示」**をクリックします。コンパートメントにベースライン評価があることを確認します。
    
    ![評価履歴ページ](images/assessment-history.png "評価履歴ページ")
    

## タスク3: 通知トピックおよびサブスクリプションの作成

通知トピックを作成するには、テナンシ管理者が必要です。

1.  Oracle Cloud Infrastructureのナビゲーション・メニューで、**「開発者サービス」**を選択し、**「アプリケーション統合」**で**「通知」**を選択します。
    
    **「通知」**ページが表示されます。
    
2.  **「リスト範囲」**で、コンパートメントが選択されていることを確認します。
    
3.  **「トピックの作成」**をクリックします。
    
    **「トピックの作成」**パネルが表示されます。
    
4.  トピック名(**security-drift**など)を入力します。
    
5.  **「作成」**をクリックします。
    
6.  トピックの名前をクリックします。
    
    **トピックの詳細**ページが表示されます。
    
7.  **「サブスクリプションの作成」**をクリックします。
    
    **「サブスクリプションの作成」**パネルが表示されます。
    
8.  **「プロトコル」**で、**「電子メール」**を選択したままにします。
    
9.  **「電子メール」**に、電子メール・アドレスを入力します。
    
10.  **「作成」**をクリックします。
    
    サブスクリプションの状態は**「保留中」**です。
    
11.  電子メール・アプリケーションを開き、Oracleから電子メールを見つけます。電子メールで、**「サブスクリプションの確認」**をクリックします。
    
    ブラウザに**「サブスクリプション確認済」**ページが表示されます。
    
12.  **「トピックの詳細」**ページをリフレッシュします。サブスクリプションの状態が**「アクティブ」**に設定されていることを確認します。
    
    ![アクティブ・サブスクリプション](images/active-subscription.png "アクティブ・サブスクリプション")
    

## タスク4: イベント・サービスでのルールの作成

1.  Oracle Cloud Infrastructureのナビゲーション・メニューから**「監視および管理」**、**「イベント・サービス」**の順に選択します。
    
2.  **「リスト範囲」**で、コンパートメントが選択されていることを確認します。
    
3.  **「ルールの作成」**をクリックします。
    
4.  **「表示名」**に、**「セキュリティ・ドリフト」**と入力します。
    
5.  **「説明」**に、**「ユーザー評価がベースライン評価と比較され、差異が見つかった場合の電子メール通知の送信」**と入力します。
    
6.  **「ルール条件」**セクションで、条件として**「イベント・タイプ」**を選択したままにします。
    
7.  **「サービス名」**で、**「データ・セーフ」**を選択します。
    
8.  **「イベント・タイプ」**で、**「ベースラインからのユーザー評価ドリフト」**を選択します。
    
9.  **「サンプル・イベント(JSON)の表示」**をクリックし、ルール・ロジックを確認します。これは、Eメールで受信する情報です。「**Cancel**」をクリックしてパネルを閉じます。
    
10.  **「アクション・タイプ」**で、**「通知」**を選択します。
    
11.  **「通知コンパートメント」**で、コンパートメントを選択します。
    
12.  **「トピック」**で、作成したトピック(**security-drift**など)を選択します。
    
    ![ルールの作成ページ](images/create-rule-page.png "ルールの作成ページ")
    
13.  **「ルールの作成」**をクリックします。
    
    **「セキュリティ・ドリフト」**ページが表示されます。
    
    ![セキュリティ ドリフト ページ](images/security-drift-page.png "セキュリティ ドリフト ページ")
    

## タスク5: ターゲット・データベースでのアクティビティの生成

このタスクでは、`PDB_DBA`ロールを持つユーザーをターゲット・データベースに作成します。

1.  データベース・アクションのSQLワークシートにアクセスします。セッションが期限切れになった場合は、`ADMIN`ユーザーとして再度サインインします。
    
2.  必要に応じて、ワークシートおよび**「スクリプト出力」**タブをクリアします。
    
3.  ワークシートで次のコマンドを入力します。
    
        <copy>CREATE USER joe_smith identified by Oracle123_Oracle123;
        GRANT PDB_DBA to joe_smith;</copy>
        
4.  ツールバーで、**「文の実行」**ボタン(緑色の円で白い矢印)をクリックします。
    
    ![「文の実行」ボタン](images/run-statement-button.png "「文の実行」ボタン")
    

## タスク6: 最新のユーザー評価のリフレッシュと結果の分析

1.  Oracle Cloud Infrastructureのブラウザ・タブに戻ります。
    
2.  ナビゲーション・メニューから**「Oracle Database」**、**「データ・セーフ- データベース・セキュリティ」**の順に選択します。
    
3.  **「セキュリティ・センター」**で、**「ユーザー評価」**をクリックします。
    
4.  **「ターゲットのサマリー」**タブをクリックします。
    
5.  **「レポートの表示」**をクリックして、最新のユーザー評価を表示します。
    
6.  最新のユーザー評価の最上部で、**「今すぐリフレッシュ」**をクリックして最新データを取得します。
    
    **「今すぐリフレッシュ」**パネルが表示されます。
    
7.  デフォルトの評価名はそのままにして、**「今すぐリフレッシュ」**をクリックします。ステータスが**SUCCEEDED**と表示されるのを待ちます。
    
    *   このアクションは、ターゲット・データベースの最新のユーザー評価のデータを更新し、アセスメント履歴にアセスメントのコピーを保存します。
    *   リフレッシュ操作には約1分かかります。
8.  **「履歴の表示」**をクリックします。
    
9.  ベースライン・アセスメントと生成した新しいアセスメントの間のリスク値を比較します。何か違いがありますか?
    
    ![次の後のアセスメント履歴](images/assessment-history-after.png "次の後のアセスメント履歴")
    
10.  「**Close**」をクリックします。
    

## タスク7: ユーザー評価の比較レポートの生成

比較レポートを生成した後、セキュリティ・ドリフト(特権ユーザーを追加したため)がある場合は、イベント・サービスによって電子メール通知がトリガーされます。

1.  最新のユーザー評価が表示された状態で、左側の**「リソース」**で、**「ベースラインとの比較」**をクリックします。Oracle Data Safeでは、比較の処理が自動的に開始されます。
    
2.  比較操作が完了したら、**比較**レポートを確認します。詳細を表示するには、**「詳細を開く」**をクリックします。
    
    ![「比較詳細」パネル](images/comparison-details-panel.png "「比較詳細」パネル")
    

## タスク8: Eメール通知のレビュー

1.  電子メール・アプリケーションを開きます。
    
2.  Oracleからの電子メール通知を見つけて開きます。メッセージには、次のようなテキストが含まれます。
    
        <copy>{
        "eventType" : "com.oraclecloud.datasafe.userassessmentdriftfrombaseline",
        "cloudEventsVersion" : "0.1",
        "eventTypeVersion" : "2.0",
        "source" : "DataSafe",
        "eventTime" : "2023-02-16T19:54:52Z",
        "contentType" : "application/json",
        "data" : {
            "compartmentId" : "ocid1.compartment.oc1...",
            "compartmentName" : "compartment-name",
            "resourceName" : "userAssessment",
            "resourceId" : "not applicable",
            "availabilityDomain" : "ad3",
            "additionalDetails" : {
            "targetName" : "ATP2000",
            "comparedWith" : "ocid1.datasafeuserassessment.oc1..."
            }
        },
        "eventID" : "e46fad7b-ac11...",
        "extensions" : {
            "compartmentId" : "ocid1.compartment.oc1..."
        }
        }
        
        --
        You are receiving notifications as a subscriber to the topic: security-drift (Topic OCID: ocid1.onstopic.oc1.eu-frankfurt-1.aaaaa...). 
        To stop receiving notifications from this topic, unsubscribe: https://cell1.notification.eu-frankfurt-1.oci.oraclecloud.com/20181201/subscriptions/ocid1.onssubscription.oc1.eu-frankfurt-1.aaaaa.../unsubscription?token=YVpsOE4weTU4TTdKSGxoTkVwR3kyaU8...==&protocol=EMAIL
        
        Please do not reply directly to this email. If you have any questions or comments regarding this email, contact your account administrator.
        
        Oracle Corporation - Worldwide Headquarters
        2300 Oracle Way, Austin, Texas 78741 USA</copy>
        

## さらに学ぶ

*   [Oracle Data Safeのイベント・タイプ](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=ADMDS-GUID-A8D65EBC-9A53-43EC-B335-0DA0E2F9CDC8)
*   [Oracle Cloud Infrastructure内のイベント](https://docs.oracle.com/en-us/iaas/Content/Events/home.htm)

## 確認

*   **著者** - データベース開発、コンサルティング・ユーザー・アシスタンス開発者、Jody Glover
*   **貢献者** - Bettina Schaeumer
*   **最終更新者/日付** - Jody Glover、2023年4月11日