# Oracle Transparent Data Encryption (TDE)

## 概要

このワークショップでは、Oracle Transparent Data Encryption (TDE)の様々な機能について説明します。これにより、ユーザーは、機密データを暗号化するためにこれらの機能を構成する方法を学習できます。

_推定ラボ時間:_ 45分

_この演習でテストしたバージョン:_ Oracle DB 19.19

### ビデオ・プレビュー

「_Livelabs - Oracle ASO (Transparent Data Encryption & Data Redaction) (2022年5月)_」のプレビューを見る[](youtube:JflshZKgxYs)

### 目標

*   データベースのコールド・バックアップを取得し、必要に応じてデータベースのリストアを有効にします。
*   データベースでのTransparent Data Encryptionの有効化
*   Transparent Data Encryptionを使用したデータの暗号化

### 前提条件

この演習では、次を想定しています。

*   Free Tier、有料またはLiveLabs Oracle Cloudアカウント
*   次を完了しました:
    *   演習: 設定の準備(_無料層_および_有料テナント_のみ)
    *   演習: 環境設定
    *   演習: 環境の初期化

### ラボのタイミング(推定)

| ステップ番号 | 機能 | 近似時間 |
| --- | --- | --- |
| 1 | DBのリストアの許可 | 5分 |
| 2 | キーストアの作成 | 5分未満 |
| 3 | ローカル自動ログイン・キーストアの作成 | 5分未満 |
| 4 | マスター・キーの作成 | 5分未満 |
| 5 | CDB$ROOTの既存のUSERS表領域の暗号化 | 5分 |
| 6 | CDB$ROOTの暗号化資格証明 | 5分 |
| 7 | PDBのSYSTEM、SYSAUXおよびUSERS表領域の暗号化 | 5分 |
| 8 | すべての新規表領域の暗号化 | 5分 |
| 9 | マスター・キーの更新 | 5分 |
| 10 | キーストアの詳細の表示 | 5分 |
| 11 | TDE前のリストア | 5分 |

## タスク1: DBのリストアの許可

1.  OSユーザー**oracle**として、_DBSec-Lab_ VMでターミナル・セッションを開きます
    
        <copy>sudo su - oracle</copy>
        
    
    **ノート**: リモート・デスクトップ・セッションを使用している場合は、デスクトップの_ターミナル_・アイコンをダブルクリックしてセッションを起動します。
    
2.  scriptsディレクトリへ移動します
    
        <copy>cd $DBSEC_LABS/tde</copy>
        
3.  backupコマンドを実行します:
    
        <copy>./tde_backup_db.sh</copy>
        
    
    ![TDE](./images/tde-001.png "DBのバックアップ")
    
4.  完了すると、コンテナおよびプラガブル・データベースが自動的に再起動されます。
    
    **ノート:**
    
    *   このスクリプトを以前に実行していて、既存のバックアップ・ファイルがある場合、スクリプトは完了しません
    *   このスクリプトを再度実行する前に、既存のバックアップを手動で管理する必要があります(削除または移動)。

## タスク2: キーストアの作成

1.  このスクリプトを実行して、オペレーティング・システムにキーストア・ディレクトリを作成します
    
        <copy>./tde_create_os_directory.sh</copy>
        
    
    ![TDE](./images/tde-002.png "キーストア・ディレクトリの作成")
    
2.  データベース・パラメータを使用してTDEを管理します。そのためには、いずれかのパラメータを有効にするためにデータベースを再起動する必要があります。スクリプトによって再起動が実行されます。
    
        <copy>./tde_set_tde_parameters.sh</copy>
        
    
    ![TDE](./images/tde-003.png "TDEパラメータの設定")
    
3.  コンテナ・データベースのソフトウェア・キーストア(**Oracle Wallet**)を作成します。`NOT_AVAILABLE`から`OPEN_NO_MASTER_KEY`へのステータス結果が表示されます。
    
        <copy>./tde_create_wallet.sh</copy>
        
    
    ![TDE](./images/tde-004.png "ソフトウェア・キーストアの作成")
    
    **ノート:**次のコマンドでパスワードを非表示にするために、管理パスワードのシークレットを作成します。
    
4.  これで、Oracle Walletが作成されました。
    

## タスク3: マスター・キーの作成

1.  コンテナ・データベースのTDEマスター・キー(**MEK**)を作成するには、次のコマンドを実行します
    
        <copy>./tde_create_mek_cdb.sh</copy>
        
    
    ![TDE](./images/tde-005.png "コンテナ・データベースTDEマスター・キーの作成")
    
2.  プラガブル・データベース**pdb1**のマスター・キー(MEK)を作成するには、次のコマンドを実行します
    
        <copy>./tde_create_mek_pdb.sh pdb1</copy>
        
    
    ![TDE](./images/tde-006.png "プラガブル・データベースのTDEマスター・キーの作成")
    
3.  必要に応じて、**pdb2**で同じ操作を実行できます。これは要件ではなく、TDEのあるデータベースとTDEがないデータベースを表示すると役立つ場合があります。
    
        <copy>./tde_create_mek_pdb.sh pdb2</copy>
        
    
    ![TDE](./images/tde-007.png "プラガブル・データベースのTDEマスター・キーの作成")
    
4.  これで、マスター・キーがあり、表領域または列の暗号化を開始できます。
    

## タスク4: ローカル自動ログインWalletの作成

1.  スクリプトを実行して、オペレーティング・システム上のOracle Walletコンテンツを表示します
    
        <copy>./tde_view_wallet_on_os.sh</copy>
        
    
    ![TDE](./images/tde-010.png "OSでのOracle Walletコンテンツの表示")
    
2.  データベース内のOracle Walletの外観を表示できます。
    
        <copy>./tde_view_wallet_in_db.sh</copy>
        
    
    ![TDE](./images/tde-011.png "データベースでのOracle Walletコンテンツの表示")
    
3.  次に、**Oracle Walletの自動ログイン**を作成します。
    
        <copy>./tde_create_autologin_wallet.sh</copy>
        
    
    ![TDE](./images/tde-012.png "Oracle Walletの自動ログインの作成")
    
4.  同じ問合せを実行して、オペレーティング・システム上のOracle Walletコンテンツを表示します。
    
        <copy>./tde_view_wallet_on_os.sh</copy>
        
    
    ![TDE](./images/tde-013.png "OSでのOracle Walletコンテンツの表示")
    
    **ノート**: **cwallet.sso**ファイルが表示されます。
    
5.  データベース内のOracle Walletに対する変更はありません。
    
        <copy>./tde_view_wallet_in_db.sh</copy>
        
    
    ![TDE](./images/tde-014.png "データベースでのOracle Walletコンテンツの表示")
    
6.  これで自動ログインが作成されました。
    

## タスク5: 既存の表領域の暗号化

1.  Linuxコマンド文字列を使用して、`EMPDATA_PROD`表領域に関連付けられているデータ・ファイル`empdata_prod.dbf`のデータを表示します。
    
        <copy>./tde_strings_data_empdataprod.sh</copy>
        
    
    ![TDE](./images/tde-015.png "データファイルのデータの表示")
    
    **ノート:**
    
    *   データが表示され、データベースに接続されていません。
    *   これは、データを表示するためにデータベースをバイパスするオペレーティング・システム・コマンドです
    *   これは、データベースが認識していないため、サイドチャネル攻撃と呼ばれます。
2.  次に、表領域全体を暗号化してデータを**明示的に暗号化**します。
    
        <copy>./tde_encrypt_tbs.sh</copy>
        
    
    ![TDE](./images/tde-016.png "明示的にデータを暗号化する")
    
    **ノート:**デフォルトでは、構文はAES256暗号化アルゴリズムを使用しています。
    
3.  さて、サイドチャネル攻撃を再試行してください
    
        <copy>./tde_strings_data_empdataprod.sh</copy>
        
    
    ![TDE](./images/tde-017.png "サイドチャネル攻撃を再試行")
    
4.  すべてのデータが暗号化され、表示されなくなったことがわかります。
    

## タスク6: すべての新規表領域の暗号化

1.  最初に、既存の初期化パラメータを確認します。
    
        <copy>./tde_check_init_params.sh</copy>
        
    
    ![TDE](./images/tde-018.png "既存の初期化パラメータの確認")
    
2.  次に、初期化パラメータ`TABLESPACE_ENCRYPTION`を`AUTO_ENABLE`に変更して常に**暗黙的にすべての新しい表領域を暗号化**し、非表示の初期化パラメータ`_tablespace_encryption_default_algorithm`をデフォルトの暗号化アルゴリズムとして`AES256`を使用するように変更します。
    
        <copy>./tde_set_encrypt_all_new_tbs.sh</copy>
        
    
    ![TDE](./images/tde-019.png "初期化パラメータの変更")
    
    **ノート:**
    
    *   `TABLESPACE_ENCRYPTION`パラメータは変更できないため、**データベースを再起動する必要があります**。
    *   このパラメータは、`ENCRYPT_NEW_TABLESPACES`パラメータの代替として、**Oracle Databaseバージョン19.16で導入**されます。
    *   `ENCRYPT_NEW_TABLESPACES`と同様に、このパラメータでは、新しく作成されたユーザー表領域を暗号化するかどうかを指定できます。
    *   `ENCRYPT_NEW_TABLESPACES`設定で指定された動作が`TABLESPACE_ENCRYPTION`設定で指定された動作と競合する場合は、`TABLESPACE_ENCRYPTION`の動作が優先されます
    *   そのため、`TABLESPACE_ENCRYPTION`が`AUTO_ENABLE`に設定されている場合、`ENCRYPT_NEW_TABLESPACES`は自動的に`ALWAYS`に設定されます。
3.  最後に、表領域TESTを作成および削除して効果を確認します。
    
        <copy>./tde_create_new_tbs.sh</copy>
        
    
    ![TDE](./images/tde-020.png "表領域TESTを作成および削除して効果を確認します。")
    
    **ノート**: 表領域**TEST**は暗号化パラメータを指定せずに作成されたという事実にもかかわらず、デフォルトでAES256暗号化アルゴリズムで暗号化されます。
    
4.  これで、すべての新規表領域がデフォルトで暗号化されます。
    

## タスク7: マスター・キーの更新

1.  コンテナ・データベースのTDEマスター・キー(MEK)を再キーするには、次のコマンドを実行します
    
        <copy>./tde_rekey_mek_cdb.sh</copy>
        
    *   キー更新の前にCDBキーを確認してください...

![TDE](./images/tde-021.png "コンテナ・データベースTDEマスター・キー(MEK)のキー更新前")

    - ...and after
    
    ![TDE](./images/tde-022.png "After rekeying the container database TDE Master Key (MEK)")
    
    - You can see the new key generated for the container
    

2.  プラガブル・データベース**pdb1**のマスター・キー(MEK)を再キーするには、次のコマンドを実行します
    
        <copy>./tde_rekey_mek_pdb.sh pdb1</copy>
        
    
    *   キーを再入力する前に、pdb1キーを確認してください...
    
    ![TDE](./images/tde-023.png "プラガブル・データベースのTDEマスター・キー(MEK)のキー更新前")
    
    *   ...以降
    
    ![TDE](./images/tde-024a.png "プラガブル・データベースのTDEマスター・キー(MEK)のキー更新後")
    
    *   プラガブル・データベースに生成された新しいキーを確認できます
3.  必要に応じて、**pdb2**でも同じ操作を実行できます。
    
        <copy>./tde_rekey_mek_pdb.sh pdb2</copy>
        
    
    **ノート:**
    
    *   ただし、これは必須ではありません。
    *   TDEのあるデータベースとTDEがないデータベースを表示すると役立つ場合があります。
4.  これでマスター・キーができたので、表領域または列の暗号化を開始できます。
    

## タスク8: キーストアの詳細の表示

1.  キーストアを作成したら、これらのスクリプトのいずれかを実行できます。**ewallet.p12**ファイルのコピーが複数あることがわかります。作成やキー更新などの変更を行うたびに、ewallet.p12ファイルがバックアップされます。**orapki**を使用して、Oracle Walletファイルの内容も確認できます。
    
    *   キーストアに関連するOSファイルの表示
    
        <copy>./tde_view_wallet_on_os.sh</copy>
        
    
    ![TDE](./images/tde-024b.png "キーストアに関連するOSファイルの表示")
    
    *   データベース内のキーストア・データの表示
    
        <copy>./tde_view_wallet_in_db.sh</copy>
        
    
    ![TDE](./images/tde-024c.png "データベース内のキーストア・データの表示")
    

## タスク9: TDE前のリストア

1.  まず、このスクリプトを実行してpfileをリストアします
    
        <copy>./tde_restore_init_parameters.sh</copy>
        
    
    ![TDE](./images/tde-025.png "PFILEのリストア")
    
2.  次に、データベースのリストア(これには時間がかかる場合があります)
    
        <copy>./tde_restore_db.sh</copy>
        
    
    ![TDE](./images/tde-026.png "データベースのリストア")
    
3.  第3に、関連するOracle Walletファイルを削除します。
    
        <copy>./tde_delete_wallet_files.sh</copy>
        
    
    ![TDE](./images/tde-027.png "関連するOracle Walletファイルの削除")
    
4.  第4に、コンテナおよびプラガブル・データベースを起動します。
    
        <copy>./tde_start_db.sh</copy>
        
    
    ![TDE](./images/tde-028.png "データベースの起動")
    
    **ノート**: これで、データベースがTDEより前の状態にリストアされました。
    
5.  最後に、TDEについて初期化パラメータが何も言わないことを確認します。
    
        <copy>./tde_check_init_params.sh</copy>
        
    
    ![TDE](./images/tde-029.png "初期化パラメータの確認")
    
6.  これで、TDEを有効にする前の時点にデータベースがリストアされ、dabaseバックアップを削除できます(オプション)。
    
        <copy>./tde_delete_backup_db.sh</copy>
        
    
    ![TDE](./images/tde-030.png "dabaseバックアップを削除します(オプション)")
    

次の演習に進むことができます。

## **付録**: 製品について

### **概要**

Oracle Databaseコア製品内でハードコードされているこの機能は、_拡張セキュリティ・オプション(ASO)_の一部です。

TDEでは、認可された受信者のみが読み取れるようにデータを暗号化できます。

暗号化を使用すると、保護されていない可能性のある環境内の機密データ(バックアップ・メディアに配置され、外部の記憶域に送信されるデータなど)を保護できます。データベース表の個々の列を暗号化することも、表領域全体を暗号化することもできます。

データが暗号化されると、そのデータにアクセスすると、認可されたユーザーまたはアプリケーションに対してこのデータが透過的に復号化されます。TDEは、ストレージ・メディアまたはデータ・ファイルが盗まれた場合に、メディアに格納されているデータ(保存データとも呼ばれる)を保護するのに役立ちます。

Oracle Databaseでは、認証、認可および監査メカニズムを使用してデータベース内のデータを保護しますが、データが格納されているオペレーティング・システム・データファイルには保護しません。これらのデータファイルを保護するために、Oracle DatabaseにはTransparent Data Encryption (TDE)が用意されています。TDEは、データファイルに格納されている機密データを暗号化します。不正な復号化を防ぐために、TDEは暗号化キーをキーストアと呼ばれるデータベースの外部のセキュリティ・モジュールに格納します。

Oracle Key VaultをTDE実装の一部として構成できます。これにより、企業内でTDEキーストア(Oracle Key VaultではTDEウォレットと呼ばれます)を集中管理できます。たとえば、ソフトウェア・キーストアをOracle Key Vaultにアップロードし、このキーストアの内容を他のTDE対応データベースで利用可能にすることができます。

![TDE](./images/aso-concept-tde.png "TDEの概念")

### **Transparent Data Encryptionを使用する利点**

*   セキュリティ管理者として、機密データが暗号化されるため、ストレージ・メディアまたはデータ・ファイルが盗まれた場合にも安全であることを確信できます
*   TDEを使用すると、セキュリティに関連する規制コンプライアンスの問題への対応に役立ちます。
*   認可されたユーザーまたはアプリケーションのデータを復号化するために、補助表、トリガーまたはビューを作成する必要はありません。表のデータは、データベース・ユーザーおよびアプリケーションに対して透過的に復号化されます。機密データを処理するアプリケーションでTDEを使用すると、アプリケーションをほとんどまたはまったく変更せずに、強力なデータ暗号化を実現できます。
*   データは、このデータにアクセスするデータベース・ユーザーおよびアプリケーションに対して透過的に復号化されます。データベース・ユーザーやアプリケーションは、アクセスするデータが暗号化された形式で格納されていることを認識する必要がありません
*   本番システムでは`Online Table Redefinition`を使用して停止時間ゼロでデータを暗号化することも、メンテナンス期間中にデータをオフラインで暗号化することもできます(`Online Table Redefinition`の詳細は、`Oracle Database Administrator’s Guide`を参照してください)。
*   暗号化データを処理するためにアプリケーションを変更する必要がありません。データベースはデータの暗号化と復号化を管理します。
*   Oracle Databaseによって、TDEマスター暗号化キーおよびキーストア管理操作が自動化されます。ユーザーやアプリケーションがTDEマスター暗号化キーを管理する必要はありません。

## 詳細

技術文書

*   [Transparent Data Encryption (TDE) 19C](https://docs.oracle.com/en/database/oracle/oracle-database/19/asoag/asopart2.html)

ビデオ:

*   _Oracle Transparent Data Encryption (TDE)の理解- Part1 (2020年1月)_[](youtube:avNWykLpic4)
*   _Oracle Transparent Data Encryption (TDE)の理解- Part2 (2020年2月)_[](youtube:aUfwG5MIMNU)
*   _Transparent Data Encryption(TDE)の基本に戻る(2021年3月)_[](youtube:JflshZKgxYs)

## 確認

*   **著者** - Database Security PM、Hakim Loumi氏
*   **貢献者** - Peter Wahl
*   **最終更新者/日付** - Hakim Loumi、データベース・セキュリティPM - 2023年11月