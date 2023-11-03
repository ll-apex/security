# 環境の準備

## 概要

このラボでは、ワークショップのためにOracle Cloud Infrastructureで環境を準備します。

_次の手順をよく読んでください。:_

*   **「テナンシで実行」**オプションの場合: テナンシ管理者の場合は、2、3および5を除くすべてのタスクを完了します。テナンシ管理者でない場合は、組織内の1人のヘルプを登録して、タスク5を除くすべてのタスクを完了します。
    
*   **「LiveLabsサンドボックスで実行」**オプションの場合: タスク5、6および7のみを完了します。Oracleでは、テナンシ、コンパートメント、LiveLabsテナンシのOracle Cloudアカウント、および事前にプロビジョニングされたAutonomous Databaseが提供されます。
    

推定ラボ時間: 15分(テナンシで実行)、5分(LiveLabsサンドボックスで実行)

### 目標

この演習では、次のことを行います。

*   コンパートメントの作成
*   ユーザー・グループの作成およびグループへのOracle Cloudアカウントの追加
*   ユーザー・グループのIAMポリシーの作成
*   Autonomous Transaction Processingデータベースのプロビジョニング
*   (LiveLabsサンドボックス予約のみ) LiveLabs予約情報を表示してサインインします
*   Oracle Database Actionsへのアクセス
*   サンプル・データをデータベースにロードします。

### 前提条件

この演習では、次を想定しています。

*   Oracle Cloudアカウントを取得し、Oracle Cloud Infrastructure Console (`https://cloud.oracle.com`)にサインインします。

## タスク1: コンパートメントの作成

Oracle Cloud Infrastructure Identity and Access Management (IAM)で自分用のコンパートメントを作成します。ここから、このコンパートメントを「コンパートメント」と呼びます。使用可能な既存のコンパートメントがテナンシにある場合は、このタスクをスキップできます。

1.  ナビゲーション・メニューから、**「アイデンティティとセキュリティ」**、**「コンパートメント」**の順に選択します。
    
    IAMの**「コンパートメント」**ページが表示されます。
    
2.  **コンパートメントの作成**をクリックします。
    
    **「コンパートメントの作成」**ダイアログ・ボックスが表示されます。
    
3.  コンパートメントの名前を入力します。
    
4.  コンパートメントの説明を入力します。
    
5.  親コンパートメントを選択します。
    
6.  **コンパートメントの作成**をクリックします。
    

## タスク2: ユーザー・グループの作成およびグループへのOracle Cloudアカウントの追加

ユーザー・グループを作成し、Oracle Cloudアカウントをグループに追加します。

1.  ナビゲーション・メニューから、**「アイデンティティとセキュリティ」**、**「グループ」**の順に選択します。
    
    IAMの**「グループ」**ページが表示されます。
    
2.  「**グループの作成**」をクリックします。
    
    **「グループの作成」**ダイアログ・ボックスが表示されます。
    
3.  グループの名前を入力します。たとえば、`dsg01` (データ・セーフ・グループ1の短縮形)と入力します。
    
4.  **データ・セーフ・ユーザー1のユーザー・グループ**など、グループの説明を入力します。説明は必須です。
    
5.  (オプション)**「拡張オプションの表示」**をクリックし、タグを作成します。
    
6.  **「作成」**をクリックします
    
    **グループ詳細**ページが表示されます。
    
7.  **「グループ・メンバー」**で、**「ユーザーをグループに追加」**をクリックします。
    
    **「ユーザーをグループに追加」**ダイアログ・ボックスが表示されます。
    
8.  ドロップダウン・リストから、このワークショップのユーザーを選択し、**「追加」**をクリックします。
    
    ユーザーはグループ・メンバーとしてリストされます。
    

## タスク3: ユーザー・グループのIAMポリシーの作成

ワークショップに必要な権限を付与するIAMポリシーを作成します。

1.  ナビゲーション・メニューから、**「アイデンティティとセキュリティ」**、**「ポリシー」**の順に選択します。
    
    IAMの**ポリシー**・ページが表示されます。
    
2.  左側の**「COMPARTMENT」**で、**ルート**・コンパートメントを選択したままにします。
    
3.  **ポリシーの作成**をクリックします。
    
    **「ポリシーの作成」**ページが表示されます。
    
4.  ポリシーの名前を入力します。`dsg01` など、グループ名の後にポリシーに名前を付けると便利です。
    
5.  ポリシーの説明(**データ・セーフ・グループ1のポリシー**など)を入力します。
    
6.  **「COMPARTMENT」**ドロップダウン・リストから、**ルート**・コンパートメントを選択したままにします。
    
7.  **「ポリシー・ビルダー」**セクションで、**「手動エディタの表示」**スライダを右に移動して、ポリシー・フィールドを表示します。
    
8.  「ポリシー」フィールドに、次のポリシー・ステートメントを入力します。`{group name}`および`{compartment name}`を適切な値に置き換えます。
    
    *   **Oracle Data Safeの基礎の開始**ワークショップでは、次の権限が必要です:
    
        <copy>
        Allow group {group name} to manage data-safe-family in compartment {compartment name}
        Allow group {group name} to manage autonomous-database in compartment {compartment name}
        </copy>
        
    
    *   **Oracle Data Safeとアプリケーションおよびサービスとの統合**ワークショップでは、次の権限が必要です。テナンシ管理者のみが、**Oracle Data Safeイベントを設定してターゲット・データベースのセキュリティ・ドリフトについて通知を受ける**というラボを実行するために必要な権限を持っていることに注意してください。
    
        <copy>
        Allow group {group name} to read compartments in compartment {compartment name}
        Allow group {group name} to manage data-safe-family in compartment {compartment name}
        Allow group {group name} to manage autonomous-database in compartment {compartment name}
        Allow group {group name} to use cloud-shell in tenancy
        Allow group {group name} to manage buckets in compartment {compartment name}
        Allow group {group name} to manage objects in compartment {compartment name}
        Allow group {group name} to manage instance-family in compartment {compartment name}
        Allow group {group name} to read app-catalog-listing in tenancy
        Allow group {group name} to manage virtual-network-family in compartment {compartment name}
        </copy>
        
9.  **「作成」**をクリックします
    

## タスク4: Autonomous Transaction Processingデータベースのプロビジョニング

コンパートメントにAutonomous Transaction Processing (ATP)データベースを作成します。続行する前に、(Always Free) Autonomous Databaseを作成するのに十分な割当てがテナンシにあることを確認してください。

> **ノート**: テナンシで既存のATPデータベースを使用する予定がある場合、またはOracle提供の環境を使用している場合は、このタスクをスキップできます。

1.  ナビゲーション・メニューから、**「Oracle Database」**、**「Autonomous Transaction Processing」**の順に選択します。
    
2.  左側の**「フィルタ」**セクションで、データベースの作成後にデータベース・リストを表示できるように、ワークロード・タイプが**「トランザクション処理」**または**「すべて」**であることを確認します。
    
3.  **「コンパートメント」**ドロップダウン・リストからコンパートメントを選択します。
    
4.  **「Autonomous Databaseの作成」**をクリックします。
    
5.  **「Autonomous Databaseの作成」**ページで、データベースの基本情報を入力します:
    
    *   **コンパートメント** - 必要に応じて、別のコンパートメントを選択します。
    *   **表示名** - 表示用に、データベースの記憶可能な名前を入力します。
    *   **データベース名** - データベース名を入力します。文字と数字のみを使用し、文字で始まることが重要です。最大長は14文字です。アンダースコアはサポートされていません。
    *   **ワークロード・タイプ** - **「トランザクション処理」**を選択します。
    *   **デプロイメント・タイプ** - **「サーバーレス」**を選択したままにします。
    *   **Always Free** - スライダを右に移動してこのオプションを選択します。
    *   **データベース・バージョン** - **21c**は選択したままにします。
    *   **OCPU数** - **1** OCPUになります。
    *   **ストレージ** - ストレージの0.02TBを取得します。
    *   **パスワード**および**パスワードの確認** - `ADMIN`データベース・ユーザーのパスワードを指定し、それを停止します。パスワードは12文字から30文字の長さとし、大文字、小文字および数字を少なくとも1つ含める必要があります。ユーザー名または二重引用符(")文字を含めることはできません。
    *   **Access Type** - **「Secure access from everywhere」**は選択したままにします。
    *   **ライセンス・タイプ** - **「ライセンス込み」**を選択したままにします。
6.  **「Autonomous Databaseの作成」**をクリックします。
    
    **「Autonomous Databaseの詳細」**ページが表示されます。
    
7.  データベース・インスタンスがプロビジョニングされるまで数分待ちます。
    
    **「使用可能」**は、大きいATPアイコンの下に表示されます。
    
    ![「Autonomous Database詳細」ページ](images/autonomous-database-details-page.png "「Autonomous Database詳細」ページ")
    

## タスク5 (LiveLabsサンドボックス予約のみ): LiveLabsサンドボックス予約情報を表示し、サインインします。

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
    

## タスク6: Oracle Database Actionsへのアクセス

データベース・アクションは、ターゲット・データベースでSQLコマンドを実行する方法を提供します。ここでは、データベース・アクションにアクセスするためのステップごとの手順について説明します。演習では、「データベース・アクション」でSQLワークシートにアクセスするだけです。必要に応じて、これらのステップをいつでも参照してヘルプを参照できます。

1.  「**Autonomous Databaseの詳細**」ページの上部で、「**データベース・アクション**」をクリックします。
    
    **サインイン**・ページが表示されます。
    
2.  必要に応じて、`ADMIN`ユーザーとしてサインインします。
    
    **Oracle Database Actions**という名前のブラウザ・タブが開きます。_このタブはワークショップ全体を通して開いたままにします。_セッションが期限切れになると、いつでも再度サインインできます。
    
    *   テナンシ管理者がAutonomous Databaseを指定した場合は、そのユーザーからパスワードを取得します。
    *   Oracle提供の環境を使用している場合は、提供されたデータベース・パスワードを入力します。
3.  **「Development」**セクションで、**「SQL」**をクリックします。
    
    ブラウザのタブ名が**「SQL | Oracle Database Actions」**に変更されます。
    
4.  警告およびヘルプ・ダイアログ・ボックスを閉じます。
    
5.  インタフェースを確認します。ワークショップでデータベース・アクションを使用する方法を次に示します:
    
    *   左側の**「ナビゲータ」**ペインで、ターゲット・データベースの**HCM1**スキーマから表を選択します。
    *   右側の**「ワークシート」**で、SQLコマンドおよびスクリプトを実行します。
    *   ページ下部の**「問合せ結果」**タブおよび**「スクリプト出力」**タブで、実行中のスクリプトから生成された問合せ結果および出力を確認します。
    
    ![Oracle DatabaseアクションのSQLワークシート](images/database-actions.png "Oracle DatabaseアクションのSQLワークシート")
    

## タスク7: サンプル・データのデータベースへのロード

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

## 確認

*   **著者** - データベース開発、コンサルティング・ユーザー・アシスタンス開発者、Jody Glover
*   **最終更新者/日付** - Jody Glover、2023年6月8日