# Oracle Key Vault(OKV)

## 概要

このワークショップでは、Oracle Key Vault (OKV)の様々な機能について説明します。ユーザーは、このアプライアンスを構成して鍵を管理する方法を学ぶことができます。

_推定ラボ時間:_ 55分

_この演習でテストしたバージョン:_ Oracle OKV 21.7

### ビデオ・プレビュー

_LiveLabs - Oracle Key Vault (2022年5月)_のプレビューを見る[](youtube:4VR1bbDpUIA)

### 目標

*   Oracle DB (TDEによって暗号化)のOKVへの接続
*   既存のDB WalletをOKVで管理
*   DB Walletの移行およびOKVによるオンライン・キーの管理

### 前提条件

この演習では、次を想定しています。

*   Free Tier、有料またはLiveLabs Oracle Cloudアカウント
*   次を完了しました:
    *   演習: 設定の準備(_無料層_および_有料テナント_のみ)
    *   演習: 環境設定
    *   演習: 環境の初期化

### ラボのタイミング(推定)

| ステップ番号 | 機能 | 近似時間 | 詳細 |
| --- | --- | --- | --- |
| 1 | (必須) TDEの前提条件 | <10分 |  |
| 2 | エンドポイントの追加 | <10分 |  |
| 3 | OKV仮想Walletの内容の表示 | 5分未満 |  |
| 4 | TDE Walletのアップロード | 5分 | Oracle WalletをOracle Key Vaultにバックアップするには |
| 5 | オンライン・マスター・キーへの移行 | 5分 | Oracle Key Vaultと直接通信するようにデータベースを再構成するには |
| 6 | OKV SEPS Walletの作成 | 5分未満 |  |
| 7 | ReKey操作の実行 | 5分 |  |
| 8 | OKVを使用したSSHキー管理とリモート・サーバー・アクセス制御 | 10分 |  |
| 9 | OKVによるシークレット管理 | 10分間 |  |
| 10 | OKVラボ構成のリセット | <10分 |  |

## タスク1: (必須) TDEの前提条件

**この演習を開始する前に**、Transparent Data Encryption (TDE) Livelabsのステップ1から4を実行していることを確認してください。

まだ実行していない場合は、次の手順に従ってすぐに実行します。

1.  OSユーザー**oracle**として、_DBSec-Lab_ VMでターミナル・セッションを開きます
    
        <copy>sudo su - oracle</copy>
        
    
    **ノート**: リモート・デスクトップ・セッションを使用している場合は、デスクトップの_ターミナル_・アイコンをダブルクリックしてセッションを起動します。
    
2.  TDEスクリプト・ディレクトリに移動
    
        <copy>cd $DBSEC_LABS/tde</copy>
        
3.  データベースのコールド・バックアップがあることを確認します(**DBが再起動します**)。
    
        <copy>./tde_backup_db.sh</copy>
        
    
    ![Key Vault](../advanced-security/tde/images/tde-001.png "DBのバックアップ")
    
4.  オペレーティング・システムでのキーストア・ディレクトリの作成
    
        <copy>./tde_create_os_directory.sh</copy>
        
    
    ![Key Vault](../advanced-security/tde/images/tde-002.png "キーストア・ディレクトリの作成")
    
5.  データベース・パラメータを使用してTDEを管理します(**DBが再起動します。**)
    
        <copy>./tde_set_tde_parameters.sh</copy>
        
    
    ![Key Vault](../advanced-security/tde/images/tde-003.png "TDEパラメータの設定")
    
6.  コンテナ・データベースの**Oracle Wallet**の作成
    
        <copy>./tde_create_wallet.sh</copy>
        
    
    ![Key Vault](../advanced-security/tde/images/tde-004.png "ソフトウェア・キーストアの作成")
    
7.  コンテナ・データベースのTDEマスター・キー(**MEK**)の作成
    
        <copy>./tde_create_mek_cdb.sh</copy>
        
    
    ![Key Vault](../advanced-security/tde/images/tde-005.png "コンテナ・データベースTDEマスター・キーの作成")
    
8.  プラガブル・データベースの**pdb1**マスター・キー(MEK)の作成
    
        <copy>./tde_create_mek_pdb.sh pdb1</copy>
        
    
    ![Key Vault](../advanced-security/tde/images/tde-006.png "プラガブル・データベースのTDEマスター・キーの作成")
    
9.  **Oracle Walletの自動ログイン**を停止します
    
        <copy>./tde_create_autologin_wallet.sh</copy>
        
    
    ![Key Vault](../advanced-security/tde/images/tde-012.png "Oracle Walletの自動ログインの作成")
    
10.  これで、**cwallet.sso**ファイルを含むこれらすべてのファイルが表示されます。
    
        <copy>./tde_view_wallet_on_os.sh</copy>
        
    
    ![Key Vault](./images/okv-201.png "OSでのOracle Walletコンテンツの表示")
    
11.  また、次のように設定して使用可能なデータベース内のウォレット
    
        <copy>./tde_view_wallet_in_db.sh</copy>
        
    
    ![Key Vault](./images/okv-202.png "データベースでのOracle Walletコンテンツの表示")
    
12.  これで、データベースがOKVラボの準備ができました。
    

## タスク2: エンドポイントの追加

まず、データベース・サーバーについて知るにはOracle Key Vaultが必要です。これを行うには、OKVでエンドポイントとして作成します

1.  _`https://kv`_へのWebブラウザを開き、Oracle Key Vaultコンソールにアクセスします
    
    **ノート:**リモート・デスクトップを使用していない場合は、_`https://<OKV-VM_@IP-Public>`_に移動してこのページにアクセスすることもできます
    
2.  次の資格証明を使用してOracle Key Vault Webコンソールにログインします
    
        <copy>KVRESTADMIN</copy>
        
    
        <copy>T06tron.</copy>
        
    
    ![Key Vault](./images/okv-001.png "Key Vault - ログイン")
    
3.  **「Endpoints」**タブに移動します。
    
    ![Key Vault](./images/okv-002.png "Key Vault - エンドポイント")
    
4.  使用可能なエンドポイントがないことがわかります
    
    ![Key Vault](./images/okv-003.png "Key Vault - エンドポイント")
    
5.  **OKVdeploy.tgz**ファイルを使用してユーティリティーを配備し、プロセスを自動化します
    
    *   OSユーザー**oracle**として、_DBSec-Lab_ VMでターミナル・セッションを開きます
        
            <copy>sudo su - oracle</copy>
            
        
        **ノート**: リモート・デスクトップ・セッションを使用している場合は、デスクトップの_ターミナル_・アイコンをダブルクリックしてセッションを起動します。
        
    *   scriptsディレクトリへ移動します
        
            <copy>cd $DBSEC_LABS/okv</copy>
            
    *   バイナリを解凍します(DBSecLab VMにファイルをすでにダウンロードしています)。
        
            <copy>./okv_unpack_restservice.sh</copy>
            
        
        ![Key Vault](./images/okv-004.png "Key Vaultバイナリの解凍")
        
    *   OKVユーティリティ構成の作成
        
        *   現在のOKV構成ファイル **okvrestcli.ini**を確認します
            
        *   ダウンロード **okvrestcli.jar**
            
        *   エンドポイントを追加する自動スクリプト**okv-ep.sh**を作成します
            
                <copy>./okv_crea_config_script.sh</copy>
                
            
            ![Key Vault](./images/okv-005a.png "OKV構成スクリプトの作成") ![Key Vault](./images/okv-005b.png "OKV構成スクリプトの作成")
            
            **ノート:**
            
            *   スクリプト_`OKV-ep.sh`_は、エンドポイント、Oracle Walletを作成し、OKVソフトウェアをデプロイするプロセスを自動化します
            *   また、OKVサーバーからRESTfulサービス・ユーティリティの最新バージョンもダウンロードします。
    *   エンドポイントとしてDBSec-Lab VMに**cdb1**データベースを追加します
        
            <copy>./okv_add_endpoint.sh</copy>
            
        
        ![Key Vault](./images/okv-006.png "エンドポイントの追加")
        
    *   終了する前に、エンドポイントのパスワードを変更する必要があります
        
        *   これは、OKVエンドポイント・クライアント・ソフトウェアがKey Vault Serverとの通信に使用するパスワードです
            
        *   デフォルトのWalletパスワード_`change-on-install`_を新しいパスワード_`Oracle123`_で変更します
            
                <copy>./okv_change_endpoint_pwd.sh</copy>
                
            
                <copy>change-on-install</copy>
                
            
                <copy>Oracle123</copy>
                
            
            ![Key Vault](./images/okv-007.png "パスワードを変更")
            
6.  OKVコンソールに戻り、画面をリフレッシュすると、追加されたエンドポイントが表示されます
    
    ![Key Vault](./images/okv-008.png "Key Vault - エンドポイント")
    
7.  エンドポイント名をクリックします(_`CDB1_ON_DBSECLAB`_を参照)
    
8.  **「Default Wallet」**セクションで、OKVで作成されたWalletがこのエンドポイントのデフォルトのWalletであることを確認します
    
    ![Key Vault](./images/okv-009.png "「Default Wallet」セクション")
    
9.  エンドポイントが追加されました。
    

## タスク3: OKV Virtual Walletのコンテンツの表示

このホストにエンドポイントを追加した後はいつでも、このスクリプトを実行してOracle Key Vaultの仮想Walletのコンテンツを表示できます

1.  ターミナル・セッションに戻り、**オペレーティング・システム**でWalletの内容を表示します
    
        <copy>./okv_view_wallet_on_os.sh</copy>
        
    
    ![Key Vault](./images/okv-010.png "OSでのOKV Walletコンテンツの表示")
    
2.  **データベース**内(`V$ENCRYPTION_WALLET`内)
    
        <copy>./okv_view_wallet_in_db.sh</copy>
        
    
    ![Key Vault](./images/okv-011.png "データベース上のOKV Walletコンテンツの表示")
    
3.  そして最後に、**Key Vault**で
    
        <copy>./okv_view_wallet_in_kv.sh</copy>
        
    
    ![Key Vault](./images/okv-012.png "Key VaultでのOKV Walletコンテンツの表示")
    

## タスク4: TDE Walletのアップロード

通常、ユーザーが最初に行うのは、既存のOracleウォレット(**ewallet.p12**ファイル)をOracle Key Vaultにアップロードすることです。

1.  Oracle WalletをOracle Key Vaultにアップロードします(リマインダとして、パスワードは_`Oracle123`_です)。
    
        <copy>./okv_upload_wallet.sh</copy>
        
    
        <copy>Oracle123</copy>
        
    
    ![Key Vault](./images/okv-013.png "OKVへのOracle Walletのアップロード")
    
2.  ここで、データベース内の仮想Walletの新しいコンテンツを表示します
    
        <copy>./okv_view_wallet_in_db.sh</copy>
        
    
    ![Key Vault](./images/okv-014.png "データベース上のOKV Walletコンテンツの表示")
    
3.  Key Vaultで
    
        <copy>./okv_view_wallet_in_kv.sh</copy>
        
    
    ![Key Vault](./images/okv-015.png "Key VaultでのOKV Walletコンテンツの表示")
    
4.  これらの情報を表示するには、_`KVRESTADMIN`_としてOKV Webコンソールに戻ります
    
    ![Key Vault](./images/okv-001.png "KVRESTADMINユーザー")
    
5.  **「キーとウォレット」**タブに移動し、_`CDB1`_をクリックします。
    
    ![Key Vault](./images/okv-016.png "「Keys &amp; Wallets」セクション")
    
6.  **「Wallet Contents」**セクションで、**アップロードされたすべてのWalletコンテンツを参照**できます
    
    ![Key Vault](./images/okv-017.png "「Wallet Contents」セクション")
    
    **ノート:**スクリプト`okv_view_wallet_in_kv.sh`に表示されるものとまったく同じです。
    

## タスク5: オンライン・マスター・キーへの移行

Oracle WalletファイルをOKVサーバーにアップロードすると、マスター・キーをWalletファイルに格納することから、Oracle Key Vaultから問い合せることに移行できます。

1.  ターミナル・セッションに戻り、仮想Walletをオンライン・マスター・キーに移行します。このステップでは、`TDE_CONFIGURATION`初期化パラメータを`KEYSTORE_CONFIGURATION=FILE`から`KEYSTORE_CONFIGURATION=OKV|FILE`に設定します。これは動的パラメータであるため、データベースを再起動する必要はありません。
    
        <copy>./okv_migrate_wallet_to_kv.sh</copy>
        
    
    ![Key Vault](./images/okv-018.png "オンライン・マスター・キーへのOracle Walletの移行")
    
2.  次に、Walletの内容を表示します。
    
    *   ...データベース内
        
            <copy>./okv_view_wallet_in_db.sh</copy>
            
        
        ![Key Vault](./images/okv-019.png "データベース上のOKV Walletコンテンツの表示")
        
        **ノート:** OKVの行が表示されます。
        
    *   Key Vaultで
        
            <copy>./okv_view_wallet_in_kv.sh</copy>
            
        
        ![Key Vault](./images/okv-020.png "Key VaultでのOKV Walletコンテンツの表示")
        
        **ノート:** TDE MEK移行の行(MKIDを含む行)が表示されます。
        
3.  慣れたら、`$TDE_HOME`の既存のWalletファイルを削除できます。
    
        <copy>./okv_delete_wallet_files.sh</copy>
        
    
    ![Key Vault](./images/okv-021.png "Oracle Walletの削除")
    
    **ノート:**
    
    *   安全のため、一時バックアップ・ディレクトリを`$TDE_HOME/backup`にし、ウォレット関連ファイルをそのディレクトリに移動します。
    *   すべてが成功したことを確認した後に実際に削除する場合は、
4.  これらの情報を表示するには、_`KVRESTADMIN`_としてOKV Webコンソールに戻ります
    
    ![Key Vault](./images/okv-001.png "KVRESTADMINユーザー")
    
5.  **「キーとウォレット」**タブに移動し、_`CDB1`_をクリックします。
    
    ![Key Vault](./images/okv-016.png "「Keys &amp; Wallets」セクション")
    
6.  **「Wallet Contents」**セクションで、**移行されたすべてのWalletコンテンツを参照**できます。
    
    ![Key Vault](./images/okv-022.png "「Wallet Contents」セクション")
    
    **ノート:**
    
    *   これは、スクリプト`okv_view_wallet_in_kv.sh`から表示できるものとまったく同じです。
    *   右下隅には、これらの2つの新しい行が9つの既存の行に追加されていることがわかります。

## タスク6: OKV SEPS Walletの作成

多くの場合、ファイルシステム上に保持されているシェルスクリプトからデータベースに接続する必要があります。これらのスクリプトにデータベース接続の詳細が含まれている場合、これはセキュリティ上の大きな問題になる可能性があります。1つの解決策はOS認証を使用することです。Oracleでは、Oracleログイン資格証明がクライアント側のOracle Walletに格納される**セキュアな外部パスワード・ストア(SEPS)**を使用するオプションが提供されます。ここでは、OKVパスワードを知らなくなったDBAとOKV管理者間で職務を分離できます。

1.  OKVエンドポイント・パスワードをSEPS Walletに配置します
    
        <copy>./okv_add_kv_pwd_to_seps.sh</copy>
        
    
    ![Key Vault](./images/okv-023.png "OKVエンドポイント・パスワードをSEPS Walletに配置します")
    
    **ノート:**これで、SEPS Wallet (`${SEPS_WALLET_DIR}/cwallet.sso`)にOKVパスワードが格納されました
    
2.  OKV自動ログイン・キーストアのシークレットを追加して、データベースでのSEPS Walletの設定を終了します
    
        <copy>./okv_setup_external_store.sh</copy>
        
    
    ![Key Vault](./images/okv-024.png "データベースでのSEPS Walletの設定")
    
    **ノート:** OKVパスワードが格納されたTDE Wallet auto\_login (`${TDE_HOME}/cwallet.sso`)の日付を参照してください。
    
3.  これで、外部ストアを介してログインし、パスワードを公開せずにキーストアを管理できるようになりました。
    

## タスク7: 再入力操作の実行

続行する前に、コンテナ・データベースのマスター・キーを作成する必要があります。各プラガブル・データベースには、独自のマスター・キーも必要です(`PDB$SEED`を除く)。

1.  ターミナル・セッションに戻り、**コンテナ・データベース**TDEマスター・キーを再入力します
    
        <copy>./okv_online_cdb_rekey.sh</copy>
        
    
    ![Key Vault](./images/okv-025.png "コンテナ・データベースTDEマスター・キーのキー更新")
    
    **ノート:**
    
    *   前の演習でSEPS Walletを作成した後、External Storeコマンドを使用してログインできるようになりました。
    *   再入力をより簡単に見つけるために明示的なタグを付けることを忘れないでください
2.  ここで、プラガブル・データベース**pdb1**のマスター・キーを再入力します
    
        <copy>./okv_online_pdb_rekey.sh pdb1</copy>
        
    
    ![Key Vault](./images/okv-026.png "プラガブル・データベースのTDEマスター・キーのキー更新")
    
3.  必要に応じて、**pdb2**でも同じ操作を実行できます。これは要件ではなく、TDEを含むデータベースとTDEがないデータベースを表示すると役立つ場合があります。
    
        <copy>./okv_online_pdb_rekey.sh pdb2</copy>
        
4.  ここで、Key Vaultの仮想Walletの新しいコンテンツを表示します
    
        <copy>./okv_view_wallet_in_kv.sh</copy>
        
    
    ![Key Vault](./images/okv-027.png "Key VaultでのOKV Walletコンテンツの表示")
    
5.  これらの情報を表示するには、_`KVRESTADMIN`_としてOKV Webコンソールに戻ります
    
    ![Key Vault](./images/okv-001.png "KVRESTADMINユーザー")
    
6.  **「キーとウォレット」**タブに移動し、_`CDB1`_をクリックします。
    
    ![Key Vault](./images/okv-016.png "「Keys &amp; Wallets」セクション")
    
7.  **「Wallet Contents」**セクションで、**cdb1**および**pdb1** (およびpdb2 (その場合はpdb2)のキー更新済マスター・キーを確認できます)
    
    ![Key Vault](./images/okv-028.png "「Wallet Contents」セクション")
    
    **ノート:**
    
    *   これは、スクリプト`okv_view_wallet_in_kv.sh`から表示できるものとまったく同じです。
    *   右下隅には、これらの2つの新しい行が11個の既存の行に追加されていることがわかります。
8.  結果の2ページ目を表示するには、「**次へ**」ボタンをクリックします
    
    ![Key Vault](./images/okv-029.png "「Wallet Contents」セクション")
    
9.  これで、コンテナおよびプラガブル・データベースのマスター・キーを再キー設定しました。
    

## タスク8: SSHキー管理およびリモート・サーバー・アクセス制御

この演習では、ユーザーの公開キーを一元的に管理することによって、リモート・サーバー・アクセス制御を紹介します。2つ目の部分では、OKVでユーザーの秘密キーを管理し、それらの秘密キーを抽出不可にします。

1.  ...

## タスク9: OKVを使用したシークレット管理

この演習では、OKV On-Demandからデータベース・アカウントのパスワードをフェッチします。

1.  シークレット管理の新規エンドポイントの作成
    
        <copy>./okv_add_endpoint_secret.sh</copy>
        
    
    ![Key Vault](./images/okv-030.png "シークレット管理の新規エンドポイントの作成")
    
    **ノート:**
    
    *   DB以外のEndPointのディレクトリを作成します。ここでは、DBアカウントのエンドポイントです
    *   パスワードなしでEndPointをプロビジョニングし、`$OKV_RESTHOME/conf/okvrestcli.ini`のClient ConfigをシークレットEndPointウォレット・ディレクトリを指すように変更します
2.  シークレット・パスワードを作成し、OKVにアップロードします
    
        <copy>./okv_crea_secret_pwd.sh</copy>
        
    
    ![Key Vault](./images/okv-031.png "OKVへのシークレット・パスワードの作成")
    
    **ノート:**
    
    *   このスクリプトは、シークレットを登録するためのJSONファイル(`$OKV_RESTHOME/sec-reg.JSON`)を生成します
    *   生成すると、シークレット・パスワードがOKVにアップロードされます
    *   OKVはシークレット・パスワードの一意のIDで応答します... **後で使用するためにコピーしてください**。
    *   パスワードは現在OKVにあるため、シークレット・パスワードを含む一時ファイルは不要になったため、スクリプトによって削除されます
3.  ここで、カスタム属性をシークレット・パスワードに定義します(前にコピーしたシークレットの一意のIDを**パラメータとして貼り付け**てください)
    
        <copy>./okv_add_secret_attributes.sh <SECRET_UNIQUE_ID></copy>
        
    
    ![Key Vault](./images/okv-032.png "シークレット・パスワードへのカスタム属性の定義")
    
    **ノート:**
    
    *   DBユーザーのユーザー名(`REFRESH_DWH)`および接続文字列)をデータベースに追加します(`dbsec-lab:1521/pdb1`を参照)。
    *   最終チェックでは、すべてのカスタム属性が正しく設定されていることを確認します。
4.  最後に、シークレット・パスワード(DBユーザー"_`REFRESH_DWH`_"および接続文字列"_`dbsec-lab:1521/pdb1`_"をパラメータとして使用)を使用してデータベースにログインして、シークレット構成をテストします
    
        <copy>./okv_login_with_secret.sh REFRESH_DWH dbsec-lab:1521/pdb1</copy>
        
    
    ![Key Vault](./images/okv-033.png "シークレット構成のテスト")
    
    **ノート:**
    
    *   ご覧のとおり、このシークレットは現在OKVにあるため、パスワードを知らずにターゲットDBにログインしたり、パスワードを入力せずにログインできます。
    *   3秒後、スクリプトはSQLセッションを中断し、自動的に終了します。
5.  この概念に問題がない場合は、シークレット構成をリセットします
    
        <copy>./okv_clean_endpoint_secret.sh</copy>
        
    
    ![Key Vault](./images/okv-034.png "シークレット構成のリセット")
    
6.  OKVでシークレットを使用および管理する方法がわかりました。
    

\-->

## タスク10: OKVラボ構成のリセット

1.  この演習中にOKVで作成されたエンドポイントおよびWalletを削除します
    
        <copy>./okv_reset_config.sh</copy>
        
    
    ![Key Vault](./images/okv-050.png "OKV構成のリセット")
    
2.  OKVバイナリのリセット
    
        <copy>
        rm -Rf $OKV_HOME
        rm -Rf $OKV_RESTHOME/!(*.tgz)
        ll $OKV_RESTHOME
        </copy>
        
    
    ![Key Vault](./images/okv-051.png "OKVバイナリのリセット")
    
3.  アップロードしたキーをKey Vaultにドロップします
    
    *   _`KVRESTADMIN`_としてOKV Webコンソールに戻ります
        
        ![Key Vault](./images/okv-001.png "アップロードしたキーをKey Vaultにドロップします")
        
    *   **「Keys & Wallets」**タブに移動し、サブメニューの**「Keys & Secrets」**を選択します。
        
        ![Key Vault](./images/okv-052.png "「キーとシークレット」セクション")
        
    *   すべてのアイテムを選択し、\[**削除**\]をクリックします
        
        ![Key Vault](./images/okv-053.png "すべての項目の削除")
        
    *   「**OK**」をクリックし、削除を確定します。
        
        ![Key Vault](./images/okv-054.png "すべての項目の削除")
        
    *   アップロードされたキーが削除されました
        
        ![Key Vault](./images/okv-055.png "すべての項目の削除")
        
4.  before-TDEのようにDBをリストア
    
    *   TDEスクリプト・ディレクトリに移動
        
            <copy>cd $DBSEC_LABS/tde</copy>
            
    *   まず、このスクリプトを実行してpfileをリストアします
        
            <copy>./tde_restore_init_parameters.sh</copy>
            
        
        ![Key Vault](../advanced-security/tde/images/tde-025.png "PFILEのリストア")
        
    *   次に、データベースのリストア(これには時間がかかる場合があります)
        
            <copy>./tde_restore_db.sh</copy>
            
        
        ![Key Vault](../advanced-security/tde/images/tde-026.png "データベースのリストア")
        
    *   第3に、関連するOracle Walletファイルを削除します。
        
            <copy>./tde_delete_wallet_files.sh</copy>
            
        
        ![Key Vault](../advanced-security/tde/images/tde-027.png "関連するOracle Walletファイルの削除")
        
    *   第4に、コンテナおよびプラガブル・データベースを起動します。
        
            <copy>./tde_start_db.sh</copy>
            
        
        ![Key Vault](../advanced-security/tde/images/tde-028.png "データベースの起動")
        
        **ノート**: これで、データベースがTDEより前の状態にリストアされました。
        
    *   最後に、TDEについて初期化パラメータが何も言わないことを確認します。
        
            <copy>./tde_check_init_params.sh</copy>
            
        
        ![Key Vault](../advanced-security/tde/images/tde-029.png "初期化パラメータの確認")
        
    *   OKVスクリプト・ディレクトリに戻り、**データベース**でOracle Walletの内容を表示します
        
            <copy>$DBSEC_LABS/okv/okv_view_wallet_in_db.sh</copy>
            
        
        ![Key Vault](./images/okv-056.png "データベースでのOracle Walletの内容の表示")
        
5.  **タスク1からこの演習を再度実行できます**(TDEを有効にする前に、データベースが特定の時点にリストアされます)。
    

次の演習に進むことができます。

## **付録**: 製品について

### **概要**

Oracle Key Vaultは、企業内のキーおよびセキュリティ・オブジェクトを集中管理するために構築された、フルスタックで強固なセキュリティを備えたソフトウェア・アプライアンスです。

Oracle Key Vaultは強力でセキュアで標準に準拠したキー管理プラットフォームであり、ここでセキュリティ・オブジェクトを格納、管理および共有できます。

![Key Vault](./images/okv-concept.png "Key Vaultの概念")

Oracle Key Vaultで管理できるセキュリティ・オブジェクトには、暗号化キー、Oracleウォレット、Javaキーストア(JKS)、Java Cryptography Extensionキーストア(JCEKS)および資格証明ファイルがあります。

Oracle Key Vaultは、組織全体の暗号化キー・ストレージを迅速かつ効率的に一元化します。Oracle Key Vaultは、Oracle Linux、Oracle Database、Oracle Databaseセキュリティ機能(Oracle Transparent Data Encryption、Oracle Database Vault、Oracle Virtual Private DatabaseおよびOracle GoldenGateなど)テクノロジ上に構築され、一元化され、可用性とスケーラブルなセキュリティ・ソリューションは、組織が今日直面しているキー管理に関する最大の課題を解決するのに役立ちます。Oracle Key Vaultを使用すると、セキュリティ・オブジェクトの保持、バックアップおよびリストア、偶発的な損失の防止、および保護された環境でのライフサイクルの管理を行うことができます。

Oracle Key Vaultは、Oracleスタック(データベース、ミドルウェア、システム)およびAdvanced SecurityのTransparent Data Encryption(TDE)用に最適化されています。さらに、KMIPベースのクライアントとの互換性を確保するために、業界標準のOASIS Key Management Interoperability Protocol (KMIP)に準拠しています。

Oracle Key Vaultを使用して、MySQL TDE暗号化キーなどの様々なエンドポイントを管理できます。

Oracle Key Vaultリリース18.1以上では、新しいマルチマスター・クラスタ・モード操作モードを使用して、可用性の向上および地理的な分散のサポートを実現できます。

マルチマスター・クラスタ・ノードは、高可用性、障害リカバリ、負荷分散および地理的分散をOracle Key Vault環境に提供します。

Oracle Key Vaultマルチマスター・クラスタは、最大の可用性と信頼性を実現するためにOracle Key Vaultノードのペアを作成するメカニズムを提供します。

![Key Vault](./images/okv-cluster-concept.png "Key Vaultマルチマスターコンセプト")

Oracle Key Vaultでは、クラスタ・ノードに対して読取り専用制限モードまたは読取り/書込みモードの2種類のモードがサポートされます。

*   **読取り専用制限モード**
    
    このモードでは、更新またはノードに追加できるのは非クリティカル・データのみです。このモードでは、レプリケーションを介してのみクリティカル・データを更新または追加できます。ノードが読取り専用制限モードになっている状況は2つあります。
    
    *   ノードが読取り専用であり、まだ読取り/書込みピアがありません。
    *   ノードは読取り/書込みペアの一部ですが、その読取り/書込みピアと通信中にブレークダウンがあったか、ノードに障害が発生した場合。2つのノードの1つが非操作の場合、残りのノードは読取り専用制限モードに設定されます。読取り/書込みノードが再びその読取り/書込みピアと通信できるようになると、ノードは読取り専用制限モードから読取り/書込みモードに戻ります。
*   **読取り/書込みモード**
    

このモードでは、重要な情報と重要でない情報の両方をノードに書き込むことができます。読取り/書込みノードは常に読取り/書込みモードで動作します。

読取り専用Oracle Key Vaultノードをクラスタに追加して、Oracleウォレット、暗号化キー、Javaキーストア、証明書、資格証明ファイルおよびその他のオブジェクトを必要とするエンドポイントにさらに高い可用性を提供できます。

Oracle Key Vaultマルチマスター・クラスタは、Oracle Key Vaultノードの相互接続されたグループです。クラスタ内の各ノードは、完全に接続されたネットワーク内の他のすべてのノードに接続するように自動的に構成されます。ノードは地理的に分散でき、Oracle Key Vaultエンドポイントはクラスタ内の任意のノードと対話します。

この構成は、他のすべてのノードにデータをレプリケートするため、データ損失のリスクが軽減されます。データ損失を防ぐには、双方向の同期レプリケーションを有効にするために、読取り/書込みペアと呼ばれるノードのペアを構成する必要があります。この構成により、あるノードへの更新をもう一方のノードにレプリケートし、更新が成功したとみなされる前に、もう一方のノードでこれを検証できます。クリティカル・データは、読取り/書込みペア内でのみ追加または更新できます。追加または更新されたすべてのデータは、クラスタの残りの部分に非同期的にレプリケートされます。

アップグレード・プロセスを完了した後、Oracle Key Vaultクラスタ内のすべてのノードはOracle Key Vaultリリース18.1以降であり、他のすべてのノードから1つのリリース更新の範囲内である必要があります。クラスタに追加する新しいOracle Key Vaultサーバーは、クラスタと同じリリース・レベルである必要があります。

クラスタのすべてのノードでクロックを同期する必要があります。したがって、クラスタのすべてのノードでNetwork Time Protocol (NTP)設定を有効にする必要があります。

クラスタ内のすべてのノードは、クラスタ全体の連続レプリケーションを通じて同一のデータセットを維持しながら、アクティブかつ独立してエンドポイントを提供できます。可能な最小の構成は2ノードのクラスタで、最大の構成では、複数のデータ・センターにまたがる複数のペアを備えた最大16のノードを保持できます。

### **Oracle Key Vaultを使用する利点**

*   Oracle Key Vaultは、セキュリティ上の脅威との戦い、キー記憶域の一元化、キー・ライフサイクル管理の一元化に役立ちます。
*   組織にOracle Key Vaultをデプロイすると、次を実現できます。
*   エンドポイント・セキュリティ・オブジェクトおよびキーのライフサイクルを管理します。これには、キーの作成、ローテーション、非アクティブ化および削除が含まれます
*   パスワードを忘れたり、誤って削除したためにキーやウォレットを失うことを防ぎます
*   組織全体で認可されたエンドポイント間でキーを安全に共有します
*   必要なすべてのバイナリ、構成ファイル、およびエンドポイントとOracle Key Vaultの間の相互に認証された接続に必要なエンドポイント証明書が含まれる、単一のソフトウェア・パッケージを使用して、エンドポイントを容易に登録およびプロビジョニングします。
*   Transparent Data Encryption (TDE)、Oracle Real Application Clusters (Oracle RAC)、Oracle Data Guard、プラガブル・データベース、Oracle GoldenGateなどの他のOracle製品および機能と連携します。Oracle Key Vaultでは、Oracle Data Pumpと、Oracle Databaseの主要な機能であるトランスポータブル表領域を使用して、暗号化データを簡単に移動できます。
*   Oracle Key Vaultマルチマスター・クラスタには、次のような追加の利点があります。
*   データを取得できる複数のOracle Key Vaultノードを提供することによる最大限のキーの可用性
*   Oracle Key Vaultマルチマスター・クラスタ・メンテナンス時のゼロ・エンドポイント停止時間

## 詳細

技術文書:

*   [Oracle Key Vault 21](https://docs.oracle.com/en/database/oracle/key-vault/21.3/index.html)
*   [Oracle Key Vault 21 - マルチマスター](https://docs.oracle.com/en/database/oracle/key-vault/21.3/okvag/multimaster_concepts.html)

ビデオ:

*   _Oracle Key Vault 21の概要(2021年1月)_[](youtube:SfXQEwziyw4)

## 確認

*   **著者** - Database Security PM、Hakim Loumi氏
*   **貢献者** - Peter Wahl
*   **最終更新者/日付** - Hakim Loumi、データベース・セキュリティPM - 2023年11月