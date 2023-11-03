# Oracle Data Safe CLIを使用した最新のセキュリティ評価のダウンロード

## 概要

クラウド・シェルでコマンドライン・インタフェース(CLI)を使用して、Oracle Data Safeでタスクを実行できます。Cloud Shellは、Linuxシェルを実行する小さな仮想マシンで、Oracle Cloud Infrastructure Consoleのテナンシからアクセスできます。テナンシ内(月次テナンシ制限内)で無料で利用できます。CLIを使用して複雑なタスクを実行する場合は、すべての CLIコマンドを含むSHスクリプトを作成することをお勧めします。

この演習では、CLIを使用して、ターゲット・データベースの最新のセキュリティ評価レポートをリフレッシュし、クラウド・シェル・マシンのディレクトリにダウンロードします。まず、Oracle Data Safeのコマンドライン・インタフェース(CLI)のドキュメントを理解します。

推定ラボ時間: 20分

### 目標

この演習では、次のことを行います。

*   Oracle Data Safe CLIのドキュメントを確認します
*   Cloud Shellへのアクセス
*   CLIコマンドを一度に1つずつ構築およびテストします。
*   すべてのCLIコマンドを使用してSHファイルを作成します。
*   SHファイルを実行し、セキュリティ評価レポートを表示します。

### 前提条件

この演習では、次を想定しています。

*   Oracle Cloudアカウントを取得し、Oracle Cloud Infrastructure Consoleにサインインします。
*   このワークショップのための環境の準備([環境の準備](?lab=prepare-environment)を参照)
*   ターゲット・データベースをOracle Data Safeに登録しました([Autonomous DatabaseのOracle Data Safeへの登録](?lab=register-autonomous-database)を参照)

## タスク1: Oracle Data Safe CLIのドキュメントの確認

1.  ブラウザで、[Oracle Data Safeコマンドライン・リファレンス](https://docs.oracle.com/en-us/iaas/tools/oci-cli/3.22.2/oci_cli_docs/cmdref/data-safe.html)を開きます。
    
2.  ページを下にスクロールし、使用可能なコマンドを確認します。リストはリソースに基づいてソートされます。
    
    *   眠らずに
    *   アラート・ポリシー
    *   アラート- サマリー
    *   監査- アーカイブ- 取得
    *   監査- イベント- 要約
    *   監査ポリシー
    *   ...
3.  リソースの説明および使用可能なコマンドを表示するには、リソースの名前をクリックします。
    
4.  コマンドの詳細を表示するには、コマンドの名前をクリックします。説明を確認します。**「使用状況」**セクションには、開始点として使用できるサンプル・コードが用意されています。構造に注意してください。`oci data-safe`、リソース名、リソースのコマンド、および存在する場合は`[OPTIONS]`で始まります。オプションは必須またはオプションです。**「必須パラメータ」**セクションには、コマンドで指定する必要があるパラメータが通知されます。**「オプション・パラメータ」**セクションでは、必要に応じてコマンドに追加できるパラメータについて説明します。
    
    たとえば、`alerts-update`コマンドは次のように構造化されています。
    
        <copy>oci data-safe alert alerts-update [OPTIONS]</copy>
        
5.  必須パラメータまたはオプション・パラメータを指定する場合は、パラメータの名前を含め、その値に従う必要があることに注意してください。たとえば、ターゲット・データベースのすべてのアラートを閉じるには、次のコマンドを発行します(`your-compartment-ocid`および`your-target-database-OCID`を独自の値に置き換えます)。
    
        <copy>oci data-safe alert alerts-update --compartment-id your-compartment-ocid --status CLOSED --target-id your-target-database-OCID</copy>
        
6.  ページの最後までスクロールして、例を表示します。
    

## タスク2: Cloud Shellへのアクセス

1.  クラウド・シェルを開くには、Oracle Cloud Infrastructure Consoleの右上隅にある**「開発者ツール」**アイコンをクリックし、**「クラウド・シェル」**を選択します。
    
    クラウド・シェルを初めて開くと、現在のディレクトリがホーム・ディレクトリになります(たとえば、`/home/jody_glove`)。この演習では、ホーム・ディレクトリ(`~/`)で作業できます。
    
2.  (オプション)クラウド・シェルを頻繁に使用し、新規に開始する場合は、リセットできます。次のコマンドは、クラウド・シェル・マシンの`$HOME`ディレクトリのすべてのデータを消去し、`$HOME/.bashrc`、`$HOME/.bash_profile`、`$HOME/.bash_logout`および`$HOME/.emacs`ファイルをデフォルト値にリセットします。確認のためにプロンプトで**y**と入力します。
    
        $ <copy>csreset --all</copy>
        

## タスク3: CLIコマンドを一度に1つずつ構築およびテストする

SHスクリプトに必要なコマンドと値を特定し、それぞれをクラウド・シェルでテストします。

1.  コンパートメントOCIDを定義する変数を作成します。
    
    これを行うには、まずコンパートメントOCIDを見つけます: Oracle Cloud Infrastructureのナビゲーション・メニューから**「アイデンティティとセキュリティ」**を選択し、右側の**「アイデンティティ」**で**「コンパートメント」**をクリックします。コンパートメントの名前をクリックします。**「コンパートメント情報」**タブで、**「OCID」**の横にある**「コピー」**をクリックします。クラウド・シェルで、次のコマンドを入力し、`your-compartment-OCID`を独自のOCIDに置き換えます。
    
        $ <copy>export compartment_id=your-compartment-ocid</copy>
        
2.  (Autonomous Database OCIDではなく) Oracle Data Safeターゲット・データベースのOCIDを定義する変数を作成します。
    
    これを行うには、まずターゲット・データベースのOCIDを検索します: ナビゲーション・メニューから、**「Oracle Database」**、**「データ・セーフ- データベース・セキュリティ」**の順に選択します。左側の**「データ・セーフ」**で、**「ターゲット・データベース」**をクリックします。左側の**「リスト範囲」**で、コンパートメントを選択します。右側で、ターゲット・データベースの名前をクリックします。**「ターゲット・データベースの詳細」**タブで、**「OCID」**の横にある**「コピー」**をクリックします。クラウド・シェルで、次のコマンドを入力し、`your-target-database-OCID`を独自のOCIDに置き換えます。
    
        $ <copy>export target_id=your-target-database-ocid</copy>
        
3.  ダウンロードしたセキュリティ評価レポートのファイル名を定義する変数を作成します。
    
    これを行うには、クラウド・シェルで次のコマンドを入力し、`your-download-file-name`を任意の名前に置き換えます。必要なファイル形式に応じて、.pdfまたは.xlsを含めます。ノート: パス(`~/examples/myreport.pdf`など)を指定できます。ただし、簡単にホーム・ディレクトリを使用しましょう(そのためパスは必要ありません)。
    
        $ <copy>export file_name=your-download-file-name</copy>
        
4.  ダウンロードしたセキュリティ評価のレポート形式(PDFまたはXLS)を定義する変数を作成します。
    
    これを行うには、クラウド・シェルで次のコマンドを入力し、`pdf-or-xls`を**PDF**または**XLS**に置き換えます。
    
        $ <copy>export format=pdf-or-xls</copy>
        
5.  セキュリティ評価を作成し、そのOCIDを取得します。
    
    これを行うには、クラウド・シェルで次のコマンドをそのまま入力します。セキュリティ評価が作成されるまで待ちます(約1分)。
    
        $ <copy>security_assessment_id=$(oci data-safe security-assessment create --compartment-id $compartment_id --target-id $target_id --query data.id --raw-output)</copy>
        
    
    `security-assessment create` CLIコマンドを`--query data.id`および`--raw-output`パラメータとともに使用する方法に注意してください。リソースに関するメタデータを取得する必要がある場合は、`--query data`および`--raw-output`パラメータを含めることで、使用可能なメタデータ値を学習できます。たとえば、前述の文で`--query data.id`のかわりに`--query data`を使用した場合、出力値には使用可能なすべてのキーと値のペアが含まれます。
    
        <copy>{"compartment-id": "ocid1.compartment.oc1...", "defined-tags": { "Oracle-Tags": { "CreatedBy": "jody.glove..", "CreatedOn": "2023-01-30T20:40:53.671Z" } }, "description": null, "display-name": "SA_1675111253797", "freeform-tags": {}, "id": "ocid1.datasafesecurityassessment.oc1...", "ignored-assessment-ids": null, "ignored-targets": null, "is-baseline": false, "is-deviated-from-baseline": null, "last-compared-baseline-id": null, "lifecycle-details": null, "lifecycle-state": "CREATING", "link": null, "schedule": null, "schedule-security-assessment-id": null, "statistics": null, "system-tags": {}, "target-ids": [ "ocid1.datasafetargetdatabase.oc1..." ], "target-version": null, "time-created": "2023-01-30T20:40:53.797000+00:00", "time-updated": "2023-01-30T20:40:53.797000+00:00", "triggered-by": "USER", "type": "SAVED" }</copy>
        
6.  Oracle Data Safeでセキュリティ評価が作成されていることを確認します。
    
    これを行うには、ナビゲーション・メニューから**「Oracle Database」**、**「データ・セーフ- データベース・セキュリティ」**の順に選択します。左側の**「データ・セーフ」**で、**「セキュリティ評価」**をクリックします。**「ターゲットのサマリー」**タブをクリックし、ターゲット・データベースを含む行を見つけて、**「レポートの表示」**をクリックします。最新のセキュリティ評価ページの上部で、**「履歴の表示」**をクリックします。コンパートメントが選択されていることを確認します。前のステップでCLIコマンドによって生成されたターゲット・データベースの新しいセキュリティ評価を見つけます。
    
    少なくとも2つのセキュリティ評価が必要です。最初のデータベースは、ターゲット・データベースを登録したときにOracle Data Safeによって自動的に作成されました。2つ目は、コマンドラインで作成したものです。2番目のものがリストされていない場合は、もう少し待つ必要がある場合があります。
    
7.  セキュリティ評価PDFを生成します。
    
    セキュリティ評価をダウンロードする前に、最初に生成する必要があります。これを行うには、クラウド・シェルに戻り、次のコマンドをそのまま入力します。前のステップで定義した変数(`$format`および`$security_assessment_id`)の使用方法に注意してください。レポートが生成されるまで待機します(約1分)。
    
        $ <copy>oci data-safe security-assessment generate-security-assessment-report --format $format --security-assessment-id $security_assessment_id</copy>
        
8.  Cloud Shellのホーム・ディレクトリにセキュリティ評価PDFをダウンロードします。
    
    これを行うには、クラウド・シェルで次のコマンドをそのまま入力します。前のステップで定義した変数(`$file_name`、`$format`および`$security_assessment_id`)の使用方法に注意してください。レポートがダウンロードされるまで待ちます(約1分)。
    
        $ <copy>oci data-safe security-assessment download-security-assessment-report --file $file_name --format $format --security-assessment-id $security_assessment_id</copy>
        
9.  PDFがCloud Shellマシンにダウンロードされていることを確認します。
    
        $ <copy>ls</copy>
        
        your-download-file-name
        
10.  次のコマンドを入力して、PDFファイルを削除します。`your-download-file-name`を独自のPDFファイル名に置き換えます。
    
        $ <copy>rm your-download-file-name</copy>
        

## タスク4: すべてのCLIコマンドを使用したSHファイルの作成

SHファイルを作成し、前のタスクでテストしたすべてのコマンドを追加します。変数には独自の値を使用してください。

1.  クラウド・シェルでviエディタを開き、`example.sh`というファイルを作成します。
    
        $ <copy>vi example.sh</copy>
        
2.  すべてのコマンドをファイルに貼り付けます。2つの`sleep`コマンドを挿入します。1つはPDFレポートの作成後、もう1つは生成後に挿入するため、プログラムで操作が完了するまでの時間が許可されます。それ以外の場合はエラーが発生します。`sleep`コマンドの前に(`echo`を使用して)コンソールに書き込むことで、ユーザーに待機時間を通知することもできます。
    
        <copy>export compartment_id=ocid1.compartment.oc1...
        export target_id=ocid1.datasafetargetdatabase.oc1...
        export file_name=MyLatestSecurityAssessment.pdf
        export format=PDF
        security_assessment_id=$(oci data-safe security-assessment create --compartment-id $compartment_id --target-id $target_id --query data.id --raw-output)
        echo "Waiting for 2 minutes to allow time for the latest security assessment to be refreshed."
        sleep 120
        oci data-safe security-assessment generate-security-assessment-report --format $format --security-assessment-id $security_assessment_id
        echo "Waiting for 1 minute to allow time for a PDF report to be generated."
        sleep 60
        oci data-safe security-assessment download-security-assessment-report --file $file_name --format $format --security-assessment-id $security_assessment_id</copy>
        
3.  ファイルを保存して終了します(**\[Esc\]**を押し、**:wq**と入力して**\[Enter\]**を押します)。
    
4.  ファイルに対する権限を付与します。
    
        $ <copy>chmod 777 example.sh</copy>
        

## タスク5: SHファイルの実行およびセキュリティ評価レポートの表示

SHファイルを実行すると、最新のセキュリティ評価がCloud Shellマシンにダウンロードされます。そこから、ローカル・コンピュータにダウンロードして表示できます。

1.  次のコマンドを入力して、SHファイルを実行します。スクリプトが実行されるまで3分間待ちます。
    
        $ <copy>./example.sh</copy>
        
2.  ステップにさらに時間がかかることを示すサービス・エラー・メッセージが表示された場合は、SHスクリプトで適切な`sleep`値を増やします。
    
3.  ホームディレクトリ内のファイルを一覧表示します。
    
        $ <copy>ls</copy>
        
        example.sh  your-download-file-name
        
4.  クラウド・シェルの右上隅にある**「クラウド・シェル・メニュー」**アイコン(歯車輪)をクリックし、**「ダウンロード」**を選択します。
    
    **ファイルのダウンロード**ダイアログ・ボックスが表示されます。
    
5.  PDFファイルの名前を入力し、**「ダウンロード」**をクリックします。
    
    ファイルがブラウザにダウンロードされます。
    
6.  PDFファイルを開き、評価レポートを確認します。終了したらブラウザ・タブを閉じます。
    
7.  クラウド・シェルを閉じ、**「終了」**をクリックして確認します。
    

**次の演習に進む**ことができます。

## 関連リソース

*   [クラウド・シェル](https://docs.oracle.com/en-us/iaas/Content/API/Concepts/cloudshellintro.htm)
*   [Oracle Data Safe CLI](https://docs.oracle.com/en-us/iaas/tools/oci-cli/3.22.2/oci_cli_docs/cmdref/data-safe.html)
*   [CLIの使用](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/cliusing.htm#Managing_CLI_Input_and_Output)

## 謝辞

*   **著者** - データベース、コンサルティング・ユーザー・アシスタンス開発者、Jody Glover
*   **コンサルタント** - Bettina Schaeumer
*   **最終更新者/日付** - Jody Glover、2023年4月11日