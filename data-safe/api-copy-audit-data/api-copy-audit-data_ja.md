# Oracle Data Safe REST APIを使用したオブジェクト・ストレージへの監査データのコピー

## 概要

Oracle Data Safeでターゲット・データベースの監査証跡を起動すると、Oracle Data Safeでは、データベースの監査証跡からOracle Data Safeリポジトリへの監査レコードのコピーが開始されます。この演習では、Oracle Data Safeアプリケーション・プログラミング・インタフェース(API)を使用して、ターゲット・データベースのOracle Data Safeの監査データをオブジェクト・ストレージにコピーします。

推定ラボ時間: 30分

### 目標

この演習では、次のことを行います。

*   監査データを格納するバケットの作成
*   Oracle Data Safeでターゲット・データベースの監査証跡を開始します
*   Oracle Data Safeによって収集された監査データの量の表示
*   Oracle Cloud Infrastructureでクラウド・シェルにアクセスし、SDK for Javaインストールを確認します
*   SDKの構成
*   Javaファイルのコンパイル
*   ターゲット・データベースのコンパートメントOCIDを取得します
*   コンパイルしたJavaファイルの実行
*   監査データがバケットにコピーされていることを確認します。

### 前提条件

この演習では、次を想定しています。

*   Oracle Cloudアカウントを取得し、Oracle Cloud Infrastructure Consoleにサインインします。
*   このワークショップのための環境の準備([環境の準備](?lab=prepare-environment)を参照)
*   ターゲット・データベースをOracle Data Safeに登録しました([Autonomous DatabaseのOracle Data Safeへの登録](?lab=register-autonomous-database)を参照)

### 前提

Cloud Shellでは、次のアプリケーション・バージョンが実行されています:

*   Javaバージョン11.0.17
*   Java(TM) SEランタイム環境18.9 (ビルド11.0.17+10-LTS-269)
*   Linux 7.9
*   Oracle Cloud Infrastructure Java SDK 3.2.2

## タスク1: 監査データを格納するバケットの作成

1.  Oracle Cloud Infrastructureのナビゲーション・メニューから、**「ストレージ」**、**「バケット」**の順に選択します。
    
2.  コンパートメントが選択されていることを確認します。
    
3.  **「バケットの作成」**をクリックします。
    
    **「バケットの作成」**パネルが表示されます。
    
4.  「バケット名」に、**DataSafeAuditData**と入力します。
    
5.  デフォルト設定のままにし、**「作成」**をクリックします。
    

## タスク2: Oracle Data Safeでのターゲット・データベースの監査証跡の開始

1.  ナビゲーション・メニューから**「Oracle Database」**、**「データ・セーフ- データベース・セキュリティ」**の順に選択します。
    
2.  左側の**「セキュリティ・センター」**で、**「アクティビティ監査」**をクリックします。
    
3.  左側の**「関連リソース」**で、**「監査証跡」**をクリックします。
    
4.  左側の**「コンパートメント」**ドロップダウン・リストから、コンパートメントが選択されていることを確認します。
    
5.  右側で、**UNIFIED\_AUDIT\_TRAIL**のターゲット・データベースの名前をクリックします。
    
    **「監査証跡の詳細」**ページが表示されます。
    
6.  **「開始」**をクリックします。
    
    **「監査証跡の開始: UNIFIED\_AUDIT\_TRAIL」**ダイアログ・ボックスが表示されます。
    
7.  開始日を今月の初日に設定します。
    
    *   現在月の初日である場合は、すべてのデータを確実に収集するために前日を選択できます。
    *   **「自動パージ」**オプションは選択しないでください。
8.  **「開始」**をクリックします。**収集状態**が**STARTING**から**COLLECTING**、**IDLE**に変更されるまで待ちます。約1分かかります。
    

## タスク3: Oracle Data Safeによって収集された監査データの量の表示

1.  ページ上部のブレッドクラムで、**「アクティビティ監査」**をクリックします。
    
2.  **「セキュリティ・センター」**で、**「監査プロファイル」**をクリックします。
    
3.  右側で、ターゲット・データベースの名前をクリックします。
    
    ターゲット・データベースの**「監査プロファイル詳細」**ページが表示されます。
    
4.  ページを**「監査ボリュームの計算」**セクションまで下にスクロールします。
    
5.  **「Data Safeによって収集済」**をクリックします。
    
    「**Compute Collected Volume**」ダイアログボックスが表示されます。
    
6.  **「開始月」**および**「終了月」**フィールドをそれぞれ現在の月の最初の日と最後の日に設定し、**「コンピュート」**をクリックします。
    
7.  **「データ・セーフに収集済(オンライン)」**列で、Oracle Data Safeによって収集された監査レコードの数を記録します。
    

## タスク4: Oracle Cloud Infrastructureでのクラウド・シェルへのアクセスおよびSDK for Javaのインストールの確認

Oracle Cloud Infrastructure SDK for Java (oci-java-SDK)には、Oracle Cloud Infrastructureのリソースの管理に使用できるSDK for Javaが用意されています。

1.  クラウド・シェルを開くには、Oracle Cloud Infrastructure Consoleの右上隅にある**「開発者ツール」**アイコンをクリックし、**「クラウド・シェル」**を選択します。
    
    クラウド・シェルを初めて開くと、現在のディレクトリがホーム・ディレクトリになります(たとえば、`/home/jody_glove`)。
    
2.  (オプション)クラウド・シェル環境をリセットします。次のコマンドは、クラウド・シェル・マシンの`$HOME`ディレクトリ内のすべてのデータを消去し、`$HOME/.bashrc`、`$HOME/.bash_profile`、`$HOME/.bash_logout`および`$HOME/.emacs`ファイルをデフォルト値に戻します。確認のためにプロンプトで **y**と入力します。
    
        $ <copy>csreset --all</copy>
        
3.  `/usr/lib64/java-OCI-SDK`ディレクトリを確認します。これはOCI Java SDKの場所です。
    
        $ <copy>ls /usr/lib64/java-oci-sdk</copy>
        
        addons  apidocs  buildTools  CHANGELOG.md  CONTRIBUTING.md  examples  lib  LICENSE.txt  NOTICE.txt  README.md  shaded  third-party  THIRD_PARTY_LICENSES.txt
        
4.  `/usr/lib64/java-oci-sdk/lib`ディレクトリの内容をリストします。`oci-java-sdk-full-version.jar`ファイルのバージョンに注意してください。次の例では、バージョンは3.3.0です。
    
        $ <copy>ls /usr/lib64/java-oci-sdk/lib</copy>
        
        jersey  jersey3  oci-java-sdk-full-3.3.0.jar  oci-java-sdk-full-3.3.0-javadoc.jar  oci-java-sdk-full-3.3.0-sources.jar
        
5.  `/usr/lib64/java-oci-sdk/third-party/lib`ディレクトリ内のサード・パーティ・ライブラリをリストします。
    
        $ <copy>ls /usr/lib64/java-oci-sdk/third-party/lib</copy>
        
6.  `/usr/lib64/java-oci-sdk/examples`ディレクトリの例をリストします。`DataSafeRestAPIClientExample.java`ファイルがあることに注意してください。このJavaプログラムには、指定したコンパートメントからオブジェクト・ストレージ内の指定されたバケットに監査データをコピーするOracle Data Safe REST APIコマンドが含まれています。
    
        $ <copy>ls /usr/lib64/java-oci-sdk/examples</copy>
        
        ...
        DataSafeRestAPIClientExample.java
        ...
        
7.  `DataSafeRestAPIClientExample.java`ファイルを確認します。
    
        $ <copy>cat /usr/lib64/java-oci-sdk/examples/DataSafeRestAPIClientExample.java</copy>
        

## タスク5: SDKの構成

Oracle Cloud Infrastructure SDKsには、ユーザー資格証明やテナンシOCIDなどの基本的な構成情報が必要です。このタスクでは、構成ファイルを作成してこの情報を指定します。

1.  `.oci`という名前のディレクトリを作成し、それに対する`read/write/execute`権限を付与してから、そのディレクトリに切り替えます。
    
        $ <copy>mkdir ~/.oci</copy>
        $ <copy>chmod 777 ~/.oci</copy>
        $ <copy>cd ~/.oci</copy>
        
2.  Oracle Cloud Infrastructure Consoleの右上隅にある**「プロファイル」**アイコンをクリックし、ユーザー名を選択します。
    
3.  左側の**「APIキー」**をクリックします。
    
4.  **「APIキーの追加」**をクリックします。
    
    **「APIキーの追加」**ダイアログ・ボックスが表示されます。
    
5.  **「APIキー・ペアの生成」**を選択したままにして、**「秘密キーのダウンロード」**をクリックします。秘密キー(PEMファイル)がブラウザにダウンロードされます。秘密鍵ファイルをコンピュータ上の任意のローカルディレクトリに保存します。
    
6.  **「追加」**をクリックします
    
    **構成ファイルのプレビュー**・ダイアログ・ボックスが表示され、構成ファイルのプレビューが表示されます。
    
7.  構成ファイルのプレビューの内容を一時ローカル・テキスト・ファイルにコピーします。必ず`[DEFAULT]`を含めてください。次のようになります。
    
        <copy>[DEFAULT]
        user=ocid1.user.oc1...
        fingerprint=ff:35...
        tenancy=ocid1.tenancy.oc1...
        region=eu-frankfurt-1
        key_file=<path to your private keyfile> # TODO</copy>
        
8.  「**Close**」をクリックします。
    
    新しいAPIキーは、**「APIキー」**の下にリストされます。
    
9.  クラウド・シェルの右上隅にある**「クラウド・シェル・メニュー」**アイコンをクリックし、**「アップロード」**を選択します。
    
    **「ホーム・ディレクトリへのファイルのアップロード」**ダイアログ・ボックスが表示されます。
    
10.  秘密キー・ファイルをダイアログ・ボックスにドラッグし、**「アップロード」**をクリックします。
    
    秘密キー・ファイルがホーム・ディレクトリにアップロードされます。
    
11.  **「ファイル転送」**ダイアログ・ボックスを閉じるには、**「非表示」**をクリックします。
    
12.  秘密キー・ファイルを`~/.oci`ディレクトリに移動します。次の例では、`your-private-key-file.pem`を独自の秘密キー・ファイル名に置き換えます。
    
        $ <copy>mv ~/your-private-key-file.pem ~/.oci/your-private-key-file.pem</copy>
        
13.  viエディタを使用して、`~/.oci`ディレクトリに構成ファイルを作成します。
    
        $ <copy>vi config</copy>
        
14.  構成ファイルの内容を`config`ファイルに貼り付けます。コンテンツは次のコードのようになります。`[DEFAULT]`を含めることを忘れないでください。
    
        <copy>[DEFAULT]
        user=ocid1.user.oc1...
        fingerprint=ff:35...
        tenancy=ocid1.tenancy.oc1...
        region=eu-frankfurt-1
        key_file=<path to your private keyfile> # TODO</copy>
        
15.  最後の行を秘密キー・ファイルへのパスに変更します。次の例では、`your-private-key-file.pem`を独自の秘密キー・ファイル名に置き換えます。**\# TODO**テキストを削除します。
    
        <copy>key_file=~/.oci/your-private-key-file.pem</copy>
        
16.  ファイルを保存して閉じます(**\[Esc\]**を押し、**:wq**と入力して**\[Enter\]**を押します)。
    

## タスク6: Javaファイルのコンパイル

`javac`コマンドを使用して、`DataSafeRestAPIClientExample.java`ファイルをコンパイルします。次の2つの変数は、クラウド・シェルですでに設定されています。これらは、Javaプログラムをコンパイルおよび実行するときに使用できます。

*   `$OCI_JAVA_SDK_LOCATION` = `/usr/lib64/java-oci-sdk`
*   `$OCI_JAVA_SDK_FULL_JAR_LOCATION` = `/usr/lib64/java-oci-sdk/lib/oci-java-sdk-full-version.jar`

1.  `DataSafeRestAPIClientExample.java`を現在のディレクトリ(`~/.oci`)にコピーします。
    
        $ <copy>cp $OCI_JAVA_SDK_LOCATION/examples/DataSafeRestAPIClientExample.java .</copy>
        
2.  `DataSafeRestAPIClientExample.java`をコンパイルします。プログラムのコンパイル後に出力はありません。
    
        $ <copy>javac -cp $OCI_JAVA_SDK_FULL_JAR_LOCATION:$OCI_JAVA_SDK_LOCATION/third-party/lib/* DataSafeRestAPIClientExample.java</copy>
        

## タスク7: ターゲット・データベースのコンパートメントOCIDの取得

1.  Oracle Cloud Infrastructureのナビゲーション・メニューから**「Oracle Database」**、**「データ・セーフ- データベース・セキュリティ」**の順に選択します。
    
2.  左側で、**「ターゲット・データベース」**をクリックします。
    
3.  左側で、コンパートメントを選択します。
    
4.  右側で、ターゲット・データベースの名前をクリックします。
    
    **「ターゲット・データベースの詳細」**ページが表示されます。
    
5.  **「ターゲット・データベースの詳細」**タブで、コンパートメントをノートにとります。
    
6.  ナビゲーション・メニューから**「アイデンティティとセキュリティ」**を選択し、右側の**「アイデンティティ」**で**「コンパートメント」**を選択します。
    
7.  コンパートメントの名前をクリックします。
    
    **「コンパートメントの詳細」**ページが表示されます。
    
8.  **「コンパートメント情報」**タブで、**「OCID」**の横にある**「コピー」**リンクをクリックし、OCIDを一時ローカル・テキスト・ファイルに貼り付けます。次のタスクのOCIDが必要です。
    

## タスク8: コンパイル済Javaファイルの実行

1.  クラウド・シェルに戻り、次のコマンドを実行して2つの変数**BUCKET**および**COMPARTMENT**を定義します。次の例では、`your-compartment-OCID`をターゲット・データベースのコンパートメントOCIDに置き換えます。
    
        $ <copy>export BUCKET=DataSafeAuditData</copy>
        $ <copy>export COMPARTMENT=your-compartment-ocid</copy>
        
2.  `oci-java-sdk-common-httpclient-jersey-version.jar`ファイルのバージョンを検索します。次の例では、バージョンは3.3.0です。
    
        $ <copy> ls $OCI_JAVA_SDK_LOCATION/lib/jersey</copy>
        
        oci-java-sdk-common-httpclient-jersey-3.3.0.jar  oci-java-sdk-common-httpclient-jersey-3.3.0-javadoc.jar  oci-java-sdk-common-httpclient-jersey-3.3.0-sources.jar
        
3.  次のコマンドを実行して、`DataSafeRestAPIClientExample.class`を実行します。`oci-java-sdk-common-httpclient-jersey-version.jar`の`version`を、前のステップで取得したバージョンに置き換えます。`org.slf4j.impl.StaticLoggerBinder`クラスのロード失敗に関するエラーは無視できます。
    
        $ <copy>java -cp $OCI_JAVA_SDK_FULL_JAR_LOCATION:$OCI_JAVA_SDK_LOCATION/third-party/lib/*:$OCI_JAVA_SDK_LOCATION/third-party/jersey/lib/*:$OCI_JAVA_SDK_LOCATION/lib/jersey/oci-java-sdk-common-httpclient-jersey-version.jar:$HOME/.oci DataSafeRestAPIClientExample $BUCKET $COMPARTMENT</copy>
        
        
        SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
        SLF4J: Defaulting to no-operation (NOP) logger implementation
        SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
        Getting the namespace
        
        
        Namespace: frmwj0cqbupb
        
        
        Getting content for object cursor  from bucket: DataSafeAuditData
        
        
        ignore
        Finished reading content for object cursor, last upload's last auditEvent record's timecollected FAILED
        
        
        2023-02-14T22:01:38.325Z
        Querying for auditEvents with timeCollected Start = 2023-02-14T22:01:38.325Z, End = 2023-02-15T22:01:38.324Z
        
        
        Count35
        
        
        Upload complete at  Wed Feb 15 22:01:40 UTC 2023 of auditeventjson2023-02-15T22:01:39.579619Z _noofrecords_ 35 Start =2023-02-14T22:01:38.325Z, End=2023-02-15T22:01:38.324Z  OpcRequestId: fra-1:q_rNFX-2hAnzGEoiurT376...
        
        
        Upload complete at  Wed Feb 15 22:01:40 UTC 2023 of cursor  OpcRequestId: fra-1:YpGeKJmQ7HtfJLXCVGLYKIEGCEPGbsdF...
        
4.  出力を確認します。最後の3行目は、オブジェクト・ストレージにコピーされた監査レコードの数を示します。実際の値は、この例に示されているものとは異なる場合があります。
    

## タスク9: 監査データがバケットにコピーされることの確認

1.  ナビゲーション・メニューから、**「ストレージ」**、**「バケット」**の順に選択します。
    
2.  コンパートメントが選択されていることを確認します。
    
3.  バケットの名前をクリックします。
    
    バケットの**「バケットの詳細」**ページが表示されます。
    
4.  **「オブジェクト」**セクションにスクロール・ダウンします。
    
5.  これで、テキスト`noofrecords_<some-number>`を含む`auditeventjson`という名前の明細項目があることに注意します。これは、Oracle Data Safeリポジトリからコピーされた監査データです。`<some-number>`は、コピーされた監査レコードの数です。
    
        <copy>auditeventjson2023-02-15T22:01:39.579619Z _noofrecords_ 35 Start =2023-02-14T22:01:38.325Z, End=2023-02-15T22:01:38.324Z</copy>
        
6.  オブジェクトおよびカーソルを削除します。一度に1つずつ、行の最後にある3つのドットをクリックし、**「削除」**を選択します。**「オブジェクトの削除の確認」**ダイアログ・ボックスで、**「削除」**をクリックします。
    

**次の演習に進む**ことができます。

## さらに学ぶ

*   [アクティビティ監査の概要](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=UDSCS-GUID-741E8CFE-041E-46C4-9C04-D849573A4DB7)
*   [監査証跡](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=UDSCS-GUID-8E684604-879A-4312-8FF6-519ECD67D179)
*   [はじめに(SDK for Javaを使用)](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/javasdkgettingstarted.htm)
*   [oci-java-sdk (GitHub上)](https://github.com/oracle/oci-java-sdk)
*   [SDK for Java (SDKの構成)](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/javasdk.htm)
*   [Data Safe API (リファレンスとエンドポイント)](https://docs.oracle.com/en-us/iaas/api/#/en/data-safe/20181201/)
*   [Oracle Cloud Infrastructure Java SDK(パッケージおよびクラス)](https://docs.oracle.com/en-us/iaas/tools/java/3.2.2/)

## 確認

*   **著者** - データベース、コンサルティング・ユーザー・アシスタンス開発者、Jody Glover
*   **コンサルタント** - Richard Evans、Bettina Schaeumer、Archana Rao、Anna Haikl
*   **最終更新者/日付** - Jody Glover、2023年4月11日