# 環境の準備

## 概要

このラボでは、Oracle Cloud Infrastructureで環境を準備します。LiveLabsサンドボックスには、テナンシ、コンパートメント、LiveLabsテナンシ内のOracle Cloudアカウント、および事前にプロビジョニングされたAutonomous Databaseが用意されています。

推定ラボ時間: 5分

### 目標

この演習では、次のことを行います。

*   LiveLabs予約情報を表示し、サインインします
*   Oracle Database Actionsへのアクセス
*   サンプル・データをデータベースにロードします。

### 前提条件

この演習では、次を想定しています。

*   Oracle Cloudアカウントを取得し、Oracle Cloud Infrastructure Console (`https://cloud.oracle.com`)にサインインします。

## タスク1: LiveLabsサンドボックス予約情報の表示とサインイン

1.  演習手順ページ(このページ)の左上隅で、**「ログイン情報の表示」**リンクをクリックします。
    
    **「予約情報」**パネルが表示されます。
    
2.  情報を確認します。Oracle Cloud Infrastructureでは、次のことが提供されます。
    
    *   LiveLabのテナンシの1つへのアクセス
    *   Oracle Cloud Infrastructureのサインイン・ページに移動するリンク(**「OCIの起動」**ボタン)
    *   LiveLabsテナンシにサインインするためのユーザー名とパスワード。初めてサインインすると、パスワードを変更するように求められます。
    *   自分のコンパートメント。このコンパートメントをワークショップ全体で「自分のコンパートメント」と呼びます。コンパートメントの名前は、ワークショップ全体で頻繁に選択する必要があるため、ノートにとります。
    *   コンパートメント内のAutonomous Database。データベースの`ADMIN`アカウントのパスワードが提供されます。
3.  ユーザー名を書き留め、Oracle Cloud Infrastructureの**「パスワードのコピー」**ボタンをクリックします。
    
4.  **「予約情報」**パネルで、**「OCIの起動」**ボタンをクリックします。
    
    新しいブラウザ・タブが開き、LiveLabsテナンシのサインイン・ページが表示されます。
    
5.  ユーザー名(必要な場合)を入力し、パスワードを**「パスワード」**ボックスに貼り付けて、**「サインイン」**をクリックします。
    
    **「パスワードの変更」**ページが表示されます。
    
6.  **「現在のパスワード」**ボックスに、パスワードを貼り付けます。**「New Password」**および**「Confirm New Password」**ボックスに、新しいパスワードを入力します。ページに表示されるパスワード要件に注意してください。**「Save New Password」**をクリックします。
    
    これで、Oracle Cloud InfrastructureのLiveLabsサンドボックスにサインインしました。
    
7.  ターゲット・データベースへのアクセス: ナビゲーション・メニュー(左上隅のハンバーガー・メニュー)から、**「Oracle Database」**、**「Autonomous Transaction Processing」**の順に選択します。**「リスト範囲」**で、**LiveLabs**フォルダの下のコンパートメントを選択します。右側の表で、ターゲット・データベースの名前をクリックします。
    

## タスク2: Oracle Database Actionsへのアクセス

データベース・アクションは、ターゲット・データベースでSQLコマンドを実行する方法を提供します。ここでは、データベース・アクションにアクセスするためのステップごとの手順について説明します。後続の演習では、「データベース・アクション」でSQLワークシートにアクセスするだけです。必要に応じて、これらのステップをいつでも参照してヘルプを参照できます。

1.  「**Autonomous Databaseの詳細**」ページの上部で、「**データベース・アクション**」をクリックします。
    
    **サインイン**・ページが表示されます。
    
2.  必要に応じて、`ADMIN`ユーザーとしてサインインします。LiveLabsから提供されたデータベース・パスワードを使用してください。
    
    **Oracle Database Actions**という名前のブラウザ・タブが開きます。
    
3.  **「Development」**セクションで、**「SQL」**をクリックします。
    
    ブラウザのタブ名が**「SQL | Oracle Database Actions」**に変更されます。
    
4.  警告およびヘルプ・ダイアログ・ボックスを閉じます。
    
5.  インタフェースを確認します。ワークショップでデータベース・アクションを使用する方法を次に示します:
    
    *   左側の**「ナビゲータ」**ペインで、ターゲット・データベースの**HCM1**スキーマから表を選択します。
    *   右側の**「ワークシート」**で、SQLコマンドおよびスクリプトを実行します。
    *   ページ下部の**「問合せ結果」**タブおよび**「スクリプト出力」**タブで、実行中のスクリプトから生成された問合せ結果および出力を確認します。
    
    ![Oracle DatabaseアクションのSQLワークシート](images/database-actions.png "Oracle DatabaseアクションのSQLワークシート")
    

## タスク3: サンプル・データのデータベースへのロード

データベースの`ADMIN`ユーザーとして、`load-data-safe-sample-data_admin.SQL` SQLスクリプトを実行してサンプル・データをデータベースにロードします。このスクリプトは、Oracle Data Safe機能の演習に使用できるサンプル・データを含む複数の表を作成します。また、`ADMIN`ユーザーのデータベース・アクティビティも生成されます。

1.  [**load-data-safe-sample-data\_admin.sql**](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/load-data-safe-sample-data_admin.sql)スクリプトをダウンロードし、NotePadなどのテキスト・エディタで開きます。
    
2.  スクリプト全体をクリップボードにコピーし、データベース・アクションのワークシートに貼り付けます。スクリプトの最後の行は次のとおりです。
    
    `select null as "End of script" from dual;`
    
3.  ツールバーで、**「スクリプトの実行」**ボタンをクリックし、スクリプトの実行が終了するまで待ちます。**「スクリプト出力」**タブにエラー・メッセージが表示されている場合は心配しないでください。これらは、スクリプトを初めて実行したときに想定されます。
    
    *   スクリプトの実行には数分かかります。
    *   左下隅には、歯車の残りが約1分間ある場合があり、その後、スクリプトの処理時に回転します。スクリプトの実行が終了すると、スクリプトの出力が表示されます。
    *   スクリプトは、**END OF SCRIPT**というメッセージで終了します。
    
    ![「スクリプトの実行」ボタン](images/run-script.png "「スクリプトの実行」ボタン")
    
4.  サンプル・データが正常にロードされたことを確認するには、スクリプト出力の最後に、`HCM1`スキーマの各表の行数を確認します。カウントは次のとおりです。
    
    *   `COUNTRIES` - 25行
    *   `DEPARTMENTS` - 27行
    *   `EMPLOYEES` - 107行
    *   `EMP_EXTENDED` - 107行
    *   `JOBS` - 19行
    *   `JOB_HISTORY` - 10行
    *   `LOCATIONS` - 23行
    *   `REGIONS` - 4行
    *   `SUPPLEMENTAL_DATA` - 149行
    
    結果が上記のものと異なる場合は、[load-data-safe-sample-data\_admin.sql](https://objectstorage.us-ashburn-1.oraclecloud.com/p/VEKec7t0mGwBkJX92Jn0nMptuXIlEpJ5XJA-A6C9PymRgY2LhKbjWqHeB5rVBbaV/n/c4u04/b/livelabsfiles/o/data-management-library-files/load-data-safe-sample-data_admin.sql)スクリプトを再実行します。
    
5.  _ブラウザ_・ページをリフレッシュしてデータベース・アクションをリフレッシュします。プロンプトが表示されたら、**「Leave page」**をクリックします。
    
6.  **「ナビゲータ」**ペインの最初のドロップダウン・リストに`HCM1`スキーマがリストされていることを確認します。
    
7.  _**「SQL」→「Oracle Database Actions」**タブは開いたままにしておきます。このワークショップを通してこのタブに戻ります。_セッションが期限切れになると、いつでも再度サインインできます。「**Autonomous Database | Oracle Cloud Infrastructure**」タブに戻ります。
    

**次の演習に進む**ことができます。

## さらに学ぶ

*   [Oracle Cloud Infrastructureドキュメント](https://docs.oracle.com/iaas/Content/home.htm)
*   [OCI Cloud Free Tier](https://www.oracle.com/cloud/free/)
*   [Autonomous Databaseのプロビジョニング](https://docs.oracle.com/en/cloud/paas/autonomous-database/adbsa/autonomous-provision.html)
*   [Autonomous Databaseへのデータのロード](https://docs.oracle.com/en/cloud/paas/autonomous-database/adbsa/load-data.html)

## 謝辞

*   **著者** - データベース開発、コンサルティング・ユーザー・アシスタンス開発者、Jody Glover
*   **最終更新者/日付** - Jody Glover、2023年6月8日