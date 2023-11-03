# Oracle Data Safe REST APIを使用したオブジェクト・ストレージへの監査データのコピーのスケジュール

## 概要

Oracle Data Safeでターゲット・データベースの監査証跡を開始すると、Oracle Data Safeでは、データベースの監査証跡からOracle Data Safeリポジトリへの監査レコードのコピーが開始されます。この演習では、コンピュート・インスタンスでOracle Data Safeアプリケーション・プログラミング・インタフェース(API)およびcrontabを使用して、ターゲット・データベースのOracle Data Safeの監査データのオブジェクト・ストレージへのコピーをスケジュールします。

Oracle Cloud Infrastructureにバケットがすでにあり、Oracle Data Safeでターゲット・データベースの監査証跡を開始している場合は、タスク1および2をスキップできます。

推定ラボ時間: 30分

### 目標

この演習では、次のことを行います。

*   コンパートメントにバケットを作成します
*   Oracle Data Safeでターゲット・データベースの監査証跡を開始します
*   Oracle Data Safeによって収集された監査データの量の表示
*   クラウド・シェルでのSSHキーの作成
*   仮想クラウド・ネットワーク(VCN)の作成
*   Oracle Linux Cloud Developer 8イメージを使用したコンピュート・インスタンスの作成
*   Cloud Shellからコンピュート・インスタンスに接続します
*   APIキーの作成
*   秘密キー(PEMファイル)をコンピュート・インスタンスにアップロードします
*   構成ファイルの作成
*   Javaプログラムのコンパイル
*   ターゲット・データベースのコンパートメントOCIDを取得します
*   コンパイル済Javaクラス・ファイルの実行
*   監査データがバケット内にあることを確認します。
*   cronjobのSHスクリプトの作成
*   crontabを使用したSHスクリプトのスケジュール
*   crontabのスケジュール済アクティビティの削除

### 前提条件

この演習では、次を想定しています。

*   Oracle Cloudアカウントを取得し、Oracle Cloud Infrastructure Consoleにサインインします。
*   このワークショップのための環境の準備([環境の準備](?lab=prepare-environment)を参照)
*   ターゲット・データベースをOracle Data Safeに登録しました([Autonomous DatabaseのOracle Data Safeへの登録](?lab=register-autonomous-database)を参照)

## タスク1: コンパートメントへのバケットの作成

監査データを格納するバケットを作成します。また、バケットを使用して、後のタスクでPEMファイルをコンピュート・インスタンスに転送します。

1.  Oracle Cloud Infrastructureのナビゲーション・メニューから、**「ストレージ」**、**「バケット」**の順に選択します。
    
2.  コンパートメントを選択します。
    
3.  **「バケットの作成」**をクリックします。
    
    **「バケットの作成」**ダイアログ・ボックスが表示されます。
    
4.  「バケット名」に、**DataSafeAuditData**と入力します。
    
5.  デフォルト設定のままにし、**「作成」**をクリックします。
    

## タスク2: Oracle Data Safeでのターゲット・データベースの監査証跡の開始

1.  Oracle Cloud Infrastructureのナビゲーション・メニューから**「Oracle Database」**、**「データ・セーフ- データベース・セキュリティ」**の順に選択します。
    
2.  左側の**「セキュリティ・センター」**で、**「アクティビティ監査」**をクリックします。
    
3.  左側の**「関連リソース」**で、**「監査証跡」**をクリックします。
    
4.  左側の**「コンパートメント」**ドロップダウン・リストから、コンパートメントが選択されていることを確認します。
    
5.  右側で、**UNIFIED\_AUDIT\_TRAIL**のターゲット・データベースの名前をクリックします。
    
    **「監査証跡の詳細」**ページが表示されます。
    
6.  **「開始」**をクリックします。
    
    **「監査証跡の開始: UNIFIED\_AUDIT\_TRAIL」**ダイアログ・ボックスが表示されます。
    
7.  開始日を今月の初日に設定します。
    
8.  **「開始」**をクリックします。**収集状態**が**STARTING**から**COLLECTING**、**IDLE**に変更されるまで待ちます。約1分かかります。
    

## タスク3: Oracle Data Safeによって収集された監査データの量の表示

1.  ページ上部のブレッドクラムで、**「アクティビティ監査」**をクリックします。
    
2.  **「セキュリティ・センター」**で、**「監査プロファイル」**をクリックします。
    
3.  右側で、ターゲット・データベースの名前をクリックします。
    
    ターゲット・データベースの**「監査プロファイル詳細」**ページが表示されます。
    
4.  ページを**「監査ボリュームの計算」**セクションまで下にスクロールします。
    
5.  **「Data Safeによって収集済」**をクリックします。
    
    「**Compute Collected Volume**」ダイアログボックスが表示されます。
    
6.  **「開始月」**および**「終了月」**フィールドをそれぞれ現在の月の最初の日と最後の日に設定し、**「コンピュート」**をクリックします。Oracle Data Safeによって収集された監査レコードの数を記録します。
    
7.  なんらかの理由で監査レコード数がゼロに等しい場合は、データベース・アクションで[**load-data-safe-sample-data\_admin.SQL**](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/load-data-safe-sample-data_admin.sql) SQLスクリプトを実行して、サンプル・データをデータベースにロードします。このスクリプトは、`ADMIN`ユーザーの監査可能なデータベース・アクティビティを生成します。次に、ステップ5と6を繰り返して、収集された監査データの量を表示します。
    

## タスク4: クラウド・シェルでのSSHキーの作成

Cloud Shellで、コンピュート・インスタンスへの接続に使用できるSSHキー・ペアを作成します。LiveLabsワークショップの標準名は`cloudshellkey`です。

1.  Oracle Cloud Infrastructureのツールバーで、**「開発者ツール」**をクリックし、**「クラウド・シェル」**を選択します。クラウド・シェルを初めて開くと、ホーム・ディレクトリ(`/home/jody_glove`など)に移動します。
    
2.  (オプション)クラウド・シェル環境をリセットします。次のコマンドは、クラウド・シェル・マシンの`$HOME`ディレクトリ内のすべてのデータを消去し、`$HOME/.bashrc`、`$HOME/.bash_profile`、`$HOME/.bash_logout`および`$HOME/.emacs`ファイルをデフォルト値に戻します。確認のためにプロンプトで **y**と入力します。
    
        $ <copy>csreset --all</copy>
        
3.  クラウド・シェル・マシンのホーム・ディレクトリに`.ssh`ディレクトリを作成し、それに変更してから、作業しているディレクトリを確認します。
    
        $ <copy>mkdir ~/.ssh</copy>
        $ <copy>cd ~/.ssh</copy>
        $ <copy>pwd</copy>
        
        /home/your-user-name/.ssh
        
4.  `.SSH`ディレクトリにいる間に、SSHキー・ペアを生成します。次のコマンドは、秘密キー`cloudshellkey`と公開キー`cloudshellkey.pub`の2つのキーを生成します。LiveLabs標準であるため、`cloudshellkey`命名規則を使用してください。パスフレーズの入力を求められたら、**\[Enter\]**を2回クリックしてパスフレーズを入力しません。
    
        $ <copy>ssh-keygen -b 2048 -t rsa -f cloudshellkey</copy>
        
5.  秘密キーおよび公開キー・ファイルが`.ssh`ディレクトリに存在することを確認します。
    
        $ <copy>ls</copy>
        
        cloudshellkey cloudshellkey.pub
        
6.  公開鍵の内容を表示します。後で、これをクリップボードにコピーし、コンピュート・インスタンスの作成時にSSHキー・ボックスに貼り付けます。
    
        $ <copy>cat cloudshellkey.pub</copy>
        
7.  Cloud Shellを開いたままにします。
    

## タスク5: 仮想クラウド・ネットワーク(VCN)の作成

1.  Oracle Cloud Infrastructureのナビゲーション・メニューから、**「ネットワーキング」**、**「仮想クラウド・ネットワーク」**の順に選択します。
    
2.  コンパートメントを選択します。
    
3.  **「VCNの作成」**をクリックします。
    
4.  **「名前」**に**labVCN**と入力します。
    
5.  コンパートメントを選択します。
    
6.  **「IPv4 CIDR Blocks」**に、**10.0.0.0/24**と入力します。
    
7.  **「このVCNでDNSホスト名を使用」**チェック・ボックスを選択したままにします。
    
8.  **「DNSラベル」**に、**labVCN**と入力します。
    
9.  **「VCNの作成」**をクリックします。
    
10.  **「サブネットの作成」**をクリックします。
    
11.  名前には、**lab-public-subnet1**と入力します。
    
12.  コンパートメントを選択します。
    
13.  サブネット・タイプの場合は、**「リージョナル」**を選択したままにします。
    
14.  **「IPv4 CIDR Block」**に、**10.0.0.0/24**と入力します。
    
15.  サブネット・アクセスの場合、**「パブリック・サブネット」**は選択したままにします。
    
16.  「**このSUBNETでDNSホスト名を使用**」チェックボックスは選択したままにします。
    
17.  **「DNSラベル」**に、**subnet1**と入力します。
    
18.  **「サブネットの作成」**をクリックします。
    
19.  左側の**「インターネット・ゲートウェイ」**をクリックします。
    
20.  **「インターネット・ゲートウェイの作成」**をクリックします。**「インターネット・ゲートウェイの作成」**パネルが表示されます。
    
21.  名前には、**livelabs-igw**と入力します。
    
22.  コンパートメントを選択します。
    
23.  **「インターネット・ゲートウェイの作成」**をクリックします。
    
24.  左側の**「ルート表」**をクリックします。
    
25.  ルート表のリストで、**「labVCNのデフォルト・ルート表」**をクリックします。
    
26.  **「ルート・ルールの追加」**をクリックします。
    
    **「ルート・ルールの追加」**パネルが表示されます。
    
27.  ターゲット・タイプの場合は、**「インターネット・ゲートウェイ」**を選択します。
    
28.  宛先CIDRブロックには、**0.0.0.0/0**と入力します。
    
29.  ターゲット・インターネット・ゲートウェイの場合は、**「livelabs-igw」**を選択します。
    
30.  **「ルート・ルールの追加」**をクリックします。
    

## タスク6: Oracle Linux Cloud Developer 8イメージを使用したコンピュート・インスタンスの作成

Oracle Linux Cloud Developer Imageは、GPUシェイプを除くすべてのコンピュート・シェイプでサポートされています。すべての標準シェイプおよびフレキシブル・シェイプに対して、このイメージには8 GB以上のメモリーが必要です。唯一の例外はVMです。Standard.E2.1。マイクロ・シェイプ。1 GBのメモリーしか割り当てられません。VMのメモリー・サイズが小さいため。Standard.E2.1。マイクロシェイプ、一部のグラフィカル集中型プログラムはイメージにインストールされません。

1.  Oracle Cloud Infrastructureのナビゲーション・メニューから、**「コンピュート」**、**「インスタンス」**の順に選択します。
    
2.  コンパートメントを選択します。
    
3.  **「インスタンスの作成」**をクリックします。
    
4.  コンピュート・インスタンスのわかりやすい名前を入力します。
    
5.  コンパートメントを選択したままにします。
    
6.  配置はそのままにします。
    
7.  **「イメージとシェイプ」**セクションで、**「編集」**をクリックします。
    
8.  **「イメージの変更」**をクリックします。**「Oracle Linux」**は選択したままにします。下にスクロールして、**「Oracle Linux Cloud Developer 8」**を選択します。**「次のドキュメントを確認しました: Oracle LInux Cloud Developer Image使用条件」**チェック・ボックスを選択します。**「イメージの選択」**をクリックします。
    
9.  **「シェイプの変更」**をクリックします。**「仮想マシン」**および**「アンペア」**シェイプ・シリーズは選択したままにします。**VM.Standard.A1のままにします。フレックス**イメージが選択されました。下にスクロールして**8** GBのメモリーを入力します。**「シェイプの選択」**をクリックします。
    
10.  **「ネットワーキング」**セクションで、**「編集」**をクリックし、**「既存の仮想クラウド・ネットワークの選択」**を選択したままにします。**「コンパートメント内の仮想クラウド・ネットワーク」**で、**labVCN**を選択します。**「コンパートメント内のサブネット」**で、**「lab-public-subnet1」**を選択します。**「パブリックIPv4アドレス」**では、**「パブリックIPv4アドレスの割当て」**を選択したままにします。
    
11.  **「SSHキーの追加」**セクションで、**「公開キーの貼付け」**を選択します。Cloud Shellに戻り、SSH公開キー全体をクリップボードにコピーします。`SSH-rsa`で始まり、`jody_glove@1e3ebc618797`のようなもので終わります。「SSH keys」ボックスに公開鍵を貼り付けます。ハードリターンがないことを確認してください。必要に応じて、`cat .SSH/cloudshellkey.pub`を実行して公開キーを再度表示できます。
    
12.  **「ブート・ボリューム」**セクションで、デフォルト設定をそのままにします。
    
13.  **「作成」**をクリックし、コンピュート・インスタンスがプロビジョニングされるまで2分待ちます。**「作業リクエスト」**ページが表示され、コンピュート・インスタンスに関する情報を表示できます。
    

## タスク7: クラウド・シェルからのコンピュート・インスタンスへの接続

1.  コンピュート・インスタンス・ページから移動した場合は、次のようにして再度検索できます: Oracle Cloud Infrastructureのナビゲーション・メニューから、**「コンピュート」**、**「インスタンス」**の順に選択します。コンパートメントを選択します。コンピュート・インスタンスの名前をクリックします。
    
2.  **「インスタンス情報」**タブの**「インスタンス・アクセス」**で、パブリックIPアドレスをクリップボードにコピーします。
    
3.  クラウド・シェルで、次の`SSH`コマンドを入力してコンピュート・インスタンスに接続し、`public-ip-address`をクリップボードにコピーしたものに置き換えます。
    
        $ <copy>ssh -i ~/.ssh/cloudshellkey opc@public-ip-address</copy>
        
    
    コンピュート・インスタンスの信頼性を確立できないことを示すメッセージが表示されます。接続を続行しますか?
    
4.  **yes**と入力して続行します。
    
    コンピュート・インスタンスのパブリックIPアドレスが、クラウド・シェル・マシンの既知のホストのリストに追加されます。端末プロンプトは`[opc@<your-compute-name> ~]$`になります。ここで、`opc`はコンピュート・インスタンス上のユーザー・アカウントです。新しいコンピュート・インスタンスに接続しました。
    

## タスク8: APIキーの作成

SDKの構成には、APIキーの作成、構成ファイルの作成、コンピュート・インスタンスへのPEMファイルのアップロードの3つの部分があります。SDK for Javaを使用するには、公開キーをOracleにアップロードして、APIリクエストの署名に使用するキー・ペアが必要です。APIをコールしているユーザーのみが秘密キーを所有している必要があります。

1.  まず、APIキーを作成します。これを行うには、Oracle Cloud Infrastructure Consoleの右上隅で**「プロファイル」**アイコンをクリックし、ユーザー名をクリックします。
    
2.  左側の**「APIキー」**をクリックします。
    
3.  **「APIキーの追加」**をクリックします。
    
    **「APIキーの追加」**ダイアログ・ボックスが表示されます。
    
4.  **「APIキー・ペアの生成」**を選択したままにし、**「秘密キーのダウンロード」**をクリックして、秘密キー(PEMファイル)をコンピュータのローカル・ディレクトリに保存します。
    
5.  **「追加」**をクリックします
    
    **構成ファイルのプレビュー**・ダイアログ・ボックスが表示されます。このダイアログには、構成ファイルのプレビューが表示されます。
    
6.  構成ファイルの内容は、後のタスクで必要になるため、一時テキスト・ファイルにコピーします。コンテンツは次のようになります。
    
        <copy>[DEFAULT]
        user=ocid1.user.oc1...
        fingerprint=your-fingerprint
        tenancy=ocid1.tenancy.oc1...
        region=eu-frankfurt-1
        key_file=<path to your private keyfile> # TODO</copy>
        
7.  「**Close**」をクリックします。
    
8.  クラウド・シェルで、`root`ユーザーに切り替えます。
    
        $ <copy>sudo su -</copy>
        
9.  `.oci`ディレクトリを作成します。
    
        # <copy>mkdir ~/.oci</copy>
        

## タスク9: コンピュート・インスタンスへの秘密キー(PEMファイル)のアップロード

秘密キー(PEMファイル)をオブジェクト・ストレージにアップロードし、それをコンピュート・インスタンスにコピーします。

1.  Oracle Cloud Infrastructureのナビゲーション・メニューから、**「ストレージ」**、**「バケット」**の順に選択します。コンパートメントを選択します。バケットの名前をクリックします。
    
    **「バケットの詳細」**ページが表示されます。
    
2.  **「オブジェクト」**セクションにスクロール・ダウンし、**「アップロード」**をクリックします。
    
    **「オブジェクトのアップロード」**パネルが表示されます。
    
3.  秘密キー・ファイルを**「コンピュータからファイルを選択」**領域にドラッグし、**「アップロード」**をクリックします。
    
4.  「**Close**」をクリックします。
    
5.  秘密キー・ファイル・リストの行の最後にある3つのドットをクリックし、**「事前認証済リクエストの作成」**を選択します。
    
    **「事前認証済リクエストの作成」**パネルが表示されます。
    
6.  **「事前認証済リクエストの作成」**をクリックします。
    
    **「事前認証済リクエストの詳細」**ダイアログ・ボックスが表示されます。
    
7.  **事前認証済リクエストURL**をクリップボードにコピーし、一時ローカル・テキスト・ファイルに貼り付けます。_重要:_ダイアログ・ボックスを閉じると、このURLを再度表示できなくなります。
    
8.  「**Close**」をクリックします。
    
9.  クラウド・シェルで、`.oci`ディレクトリに変更します。
    
        # <copy>cd ~/.oci</copy>
        
10.  `WGET`コマンドを使用して、秘密キー・ファイルをオブジェクト・ストレージから`.oci`ディレクトリにコピーします。`pre-authenticated-request-url`を独自のURLに置き換えます。
    
        # <copy>wget pre-authenticated-request-url</copy>
        
11.  ディレクトリの内容を一覧表示して、秘密鍵ファイルが存在することを確認します。
    
        # <copy>ls</copy>
        

## タスク10: 構成ファイルの作成

このタスクでは、SDKの`.oci`ディレクトリに`config`という名前の構成ファイルを作成し、(前のタスクで作成した) APIキーから取得したAPIコンテンツを追加します。構成ファイルで、コンピュート・インスタンスの秘密キー・ファイルに実際のパスを追加して、最後の行を修正します。後続のタスクでコンパイルするjavaファイルは、`DEFAULT`という名前のプロファイルを持つ`~/.oci/config`内の構成ファイルを検索します。

1.  viエディタを使用して、まだ`.oci`ディレクトリにいる間に構成ファイルを作成します。
    
        # <copy>vi config</copy>
        
2.  構成ファイルの内容を`config`ファイルに貼り付けます。ノート: 以前にこのコンテンツを一時テキスト・ファイルに貼り付けました。コンテンツは次のコードのようになります。必ず上部に`[DEFAULT]`を含めてください。
    
         <copy>[DEFAULT]
         user=ocid1.user.oc1...
         fingerprint=your-fingerprint
         tenancy=ocid1.tenancy.oc1...
         region=eu-frankfurt-1
         key_file=<path to your private keyfile> # TODO</copy>
        
3.  最後の行を、コンピュート・インスタンス上のPEMファイルへのパスに変更します。次の例では、`your-private-key-file-name`を独自の秘密キー・ファイル名に置き換えます。
    
        <copy>key_file=~/.oci/your-private-key-file-name</copy>
        
4.  ファイルを保存して閉じます(**\[Esc\]**を押し、**:wq**と入力して**\[Enter\]**を押します)。
    
5.  現在のディレクトリの内容をリストし、`config`ファイルが存在することを確認します。
    
        # <copy>ls</copy>
        
        config  your-private-key-file-name
        

## タスク11: Javaプログラムのコンパイル

Oracle Linux Cloud Developerイメージには、SDKおよびJavaソフトウェアがすでにインストールされています。OCI jarファイルは`/usr/lib64/java-oci-SDK/lib/oci-java-SDK-full-<version>.jar`、サードパーティ・ライブラリは`/usr/lib64/java-oci-SDK/third-party/lib`にあります。Javaファイルをコンパイルするには、`javac`コマンドを使用します。

このタスクでは、SDKインストールに付属する`DataSafeRestAPIClientExample.java`という名前のJavaプログラムをコンパイルします。このプログラムの目的は、監査データをOracle Data Safeリポジトリから指定したオブジェクト・ストレージ・バケットにコピーすることです。必要に応じて、`wget https://raw.githubusercontent.com/oracle/oci-java-SDK/master/bmc-examples/src/main/java/DataSafeRestAPIClientExample.java`コマンドを実行して、Githubから直接プログラムをダウンロードすることもできます。

1.  `oci-java-sdk-full-version.jar`ファイルのバージョンを確認します。この例では、バージョンは2.27.0です。
    
        # <copy>ls /usr/lib64/java-oci-sdk/lib</copy>
        
        oci-java-sdk-full-2.27.0.jar  oci-java-sdk-full-2.27.0-javadoc.jar  oci-java-sdk-full-2.27.0-sources.jar
        
2.  `/usr/lib64/java-oci-sdk`ディレクトリに切り替えます。
    
        # <copy>cd /usr/lib64/java-oci-sdk</copy>
        
3.  `DataSafeRestAPIClientExample.java`ファイルをコンパイルします。`oci-java-sdk-full-<version>.jar`ファイル名の正しいバージョンを使用してください。次の例では、バージョン2.27.0を使用します。
    
    Javaプログラムが1つ以上の外部ライブラリ(JARファイル)に依存することは非常に一般的です。フラグ`-classpath` (または`-cp`)を使用して、外部ライブラリを検索する場所をコンパイラに指示します。デフォルトでは、コンパイラはブートストラップ・クラスパスおよび`CLASSPATH`環境変数を検索します。
    
        # <copy>javac -cp lib/oci-java-sdk-full-2.27.0.jar:third-party/lib/* examples/DataSafeRestAPIClientExample.java</copy>
        
    
    ノート: ファイルのコンパイル後に出力はありません。単にプロンプトに戻ります。
    
4.  `examples`ディレクトリに移動し、ファイルをリストします。`DataSafeRestAPIClientExample.class`という名前のクラス・ファイルがあることを確認します。
    
        # <copy>cd examples</copy>
        # <copy>ls</copy>
        

## タスク12: ターゲット・データベースのコンパートメントOCIDの取得

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
    

## タスク13: コンパイル済Javaクラス・ファイルの実行

`DataSafeRestAPIClientExample.class`ファイルを実行して、スケジュールする前にエラーなしで実行することをテストします。プログラムには2つの入力が必要であり、事前に定義できます。

*   コピーされた監査データを格納するバケットの名前
*   ターゲット・データベースのコンパートメントOCID

1.  **BUCKET**と **COMPARTMENT**の2つの変数を設定するには、次のコマンドを実行します。`compartment-OCID-for-target-database`を独自のOCIDに置き換えます。
    
        # <copy>export BUCKET=DataSafeAuditData</copy>
        # <copy>export COMPARTMENT=compartment-ocid-for-target-database</copy>
        
2.  `/usr/lib64/java-oci-sdk/lib/examples`ディレクトリで作業していることを確認します。
    
3.  次のコマンドを実行して、クラス・ファイルを実行します。次の例では、`oci-java-sdk-full-2.27.0.jar`を使用していますが、システム上のバージョンを使用してください。`org.slf4j.impl.StaticLoggerBinder`クラスのロード失敗に関するエラーは無視できます。
    
        # <copy>java -cp /usr/lib64/java-oci-sdk/lib/oci-java-sdk-full-2.27.0.jar:/usr/lib64/java-oci-sdk/third-party/lib/*:/usr/lib64/java-oci-sdk/examples DataSafeRestAPIClientExample $BUCKET $COMPARTMENT</copy>
        
        SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
        SLF4J: Defaulting to no-operation (NOP) logger implementation
        SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
        Getting the namespace
        
        
        Namespace: frmwj0cqbupb
        
        
        Getting content for object cursor  from bucket: DataSafeAuditData
        
        
        ignore
        Finished reading content for object cursor, last upload's last auditEvent record's timecollected FAILED
        
        
        2023-02-15T19:03:33.225Z
        Querying for auditEvents with timeCollected Start = 2023-02-15T19:03:33.225Z, End = 2023-02-16T19:03:33.223Z
        
        
        Count43
        
        
        Upload complete at  Thu Feb 16 19:03:34 GMT 2023 of auditeventjson2023-02-16T19:03:34.290935835Z _noofrecords_ 43 Start =2023-02-15T19:03:33.225Z, End=2023-02-16T19:03:33.223Z  OpcRequestId: fra-1:RMvrVLBJGMQYyKnOATHwJs6Ywthox3dK9BGYWIaZv3LD2lFq5oRaUuZKzWsJkwZf
        
        
        Upload complete at  Thu Feb 16 19:03:34 GMT 2023 of cursor  OpcRequestId: fra-1:v5KE-S9VbuBMsqnof2qx5dkabTsHgbZv50wSAJJLk-TD-b3e4cqwRmIDG6Bdwa1y
        
4.  出力を確認します。最後の3行目は、オブジェクト・ストレージにコピーされた監査レコードの数を示します。実際の値は異なる場合があります。カウントがゼロの場合は、バケット内のカーソルを削除し、ステップ3を繰り返します。
    

## タスク14: 監査データがバケット内にあることの確認

1.  Oracle Cloud Infrastructureのナビゲーション・メニューから、**「ストレージ」**、**「バケット」**の順に選択します。
    
2.  コンパートメントを選択します。
    
3.  バケットの名前をクリックします。
    
    バケットの**「バケットの詳細」**ページが表示されます。
    
4.  **「オブジェクト」**セクションにスクロール・ダウンします。
    
5.  これで、テキスト`noofrecords_<some-number>`を含む`auditeventjson`という名前の明細項目があることに注意します。これは、Oracle Data Safeリポジトリからコピーされた監査データです。`<some-number>`は、コピーされた監査レコードの数です。
    
    ![初期監査レコード・オブジェクト](images/initial-audit-records-object.png "初期監査レコード・オブジェクト")
    

## タスク15: cronjobのSHスクリプトの作成

コンパイルされたJavaプログラムが正常に機能することを確認したので、コンピュート・インスタンスでcronjobを使用してスケジュールできます。Cronデーモンは、システム上でスケジュールされた時間にプロセスを実行する組込みのLinuxユーティリティです。Cronは、事前定義済のコマンドおよびスクリプトについてcrontab (cron表)を読み取ります。cronジョブを作成するには、`root`ユーザーまたは`sudo`権限を持つユーザーである必要があります。

このタスクでは、Javaプログラムを実行する変数およびJavaコマンドを含むSHスクリプトを作成します。次のタスクでは、SHスクリプトをスケジュールします。

1.  `/usr/local/bin`ディレクトリに移動します。
    
        # <copy>cd /usr/local/bin</copy>
        
2.  viエディタを使用して、`datasafejob.SH`という名前のSHファイルを作成します。
    
        # <copy>vi datasafejob.sh</copy>
        
3.  次の内容をSHファイルに追加します。クラス・ファイルは、タスク13で使用したのとは少し異なるJavaコマンドを使用して実行します。クラス・ファイルをどこからでも実行するには、クラス・パスに`examples`ディレクトリへのパスを含める必要があります。また、`oci-java-sdk-full-2.27.0.jar`を使用しています。システムで正しいバージョンを使用してください。`compartment-OCID-for-target-database`を独自のコンパートメントOCIDに置き換えます。
    
        <copy>#!/bin/bash
        
        export BUCKET=DataSafeAuditData
        
        export COMPARTMENT=compartment-ocid-for-target-database
        
        java -cp /usr/lib64/java-oci-sdk/lib/oci-java-sdk-full-2.27.0.jar:/usr/lib64/java-oci-sdk/third-party/lib/*:/usr/lib64/java-oci-sdk/examples DataSafeRestAPIClientExample $BUCKET $COMPARTMENT</copy>
        
4.  ファイルを保存して閉じます(**\[Esc\]**を押し、**:wq**と入力して**\[Enter\]**を押します)。
    
5.  権限をスクリプトに追加します。
    
        # <copy>chmod 777 datasafejob.sh</copy>
        

## タスク16: crontabを使用したSHスクリプトのスケジュール

SHスクリプトをスケジュールして1分ごとに実行し、スケジュールが機能することをテストできるようにします。確認後、スケジュールを毎日午前2時に変更します。

1.  cronジョブを編集するには、次のコマンドを入力します。
    
        # <copy>crontab -e</copy>
        
2.  最初の行に次を追加して保存します(**\[Esc\]**を押し、**:wq**と入力して**\[Enter\]**を押します)。
    
        <copy>* * * * * /usr/local/bin/datasafejob.sh</copy>
        
3.  Oracle Data Safeで監査するアクティビティを生成します。これを行うには、ターゲット・データベースのデータベース・アクションにアクセスします。[**load-data-safe-sample-data\_admin.sql**](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/load-data-safe-sample-data_admin.sql)スクリプトをダウンロードし、NotePadなどのテキスト・エディタで開きます。スクリプト全体をクリップボードにコピーし、データベース・アクションのワークシートに貼り付けます。ツールバーで、**「スクリプトの実行」**ボタンをクリックし、スクリプトの実行が終了するまで待ちます。
    
4.  バケットに戻り、1分ごとに収集される監査データを表示します。監査データ・オブジェクトが表示されるまで最大10分かかる場合があります。
    
    ![監査レコード・オブジェクト](images/audit-records-objects.png "監査レコード・オブジェクト")
    
5.  Cloud Shellで、crontabにアクセスします。
    
        # <copy>crontab -e</copy>
        
6.  スケジュールを毎日午前2時に変更し、ファイルを保存します(**\[Esc\]**を押し、**:wq**と入力して**\[Enter\]**を押します)。
    
    次の例では、`0 2 * * *`は、システム・クロックが午前2時に表示されるたびにcronジョブが実行されることを示します。
    
        <copy>0 2 * * * /usr/local/bin/datasafejob.sh</copy>
        

## タスク17: crontabでのスケジュール済アクティビティの削除

1.  crontabにアクセスします。
    
        # <copy>crontab -e</copy>
        
2.  コンテンツを削除します。
    
3.  変更を保存します(**\[Escape\]**を押し、**:wq**と入力して**\[Enter\]**を押します)。
    
4.  Cloud Shellを閉じます。
    

**次の演習に進む**ことができます。

## さらに学ぶ

*   [アクティビティ監査の概要](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=UDSCS-GUID-741E8CFE-041E-46C4-9C04-D849573A4DB7)
*   [監査証跡](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=UDSCS-GUID-8E684604-879A-4312-8FF6-519ECD67D179)
*   [Oracle Linux Cloud Developerイメージ](https://docs.oracle.com/en-us/iaas/oracle-linux/developer/index.htm)
*   [はじめに(SDK for Javaを使用)](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/javasdkgettingstarted.htm)
*   [oci-java-sdk (GitHub上)](https://github.com/oracle/oci-java-sdk)
*   [SDK for Java (SDKの構成)](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/javasdk.htm)
*   [Data Safe API (リファレンスとエンドポイント)](https://docs.oracle.com/en-us/iaas/api/#/en/data-safe/20181201/)
*   [Oracle Cloud Infrastructure Java SDK(パッケージおよびクラス)](https://docs.oracle.com/en-us/iaas/tools/java/3.2.2/)

## 謝辞

*   **著者** - データベース開発、コンサルティング・ユーザー・アシスタンス開発者、Jody Glover
*   **貢献者** - Richard Evans、Anna Haikl、Russ Lowenthal、Archana Rao、Bettina Schaeumer
*   **最終更新者/日付** - Jody Glover、2023年4月11日