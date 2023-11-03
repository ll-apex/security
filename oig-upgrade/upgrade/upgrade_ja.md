# ワンホップ・アップグレード

## 概要

このラボでは、Oracle Identity Managerのワンホップ・アップグレードに関連するステップについて説明します。

_推定ラボ時間_: 50分

### 目標

この演習では、次のことを行います。

*   12cへのスキーマ・アップグレードの実行
*   OIM 11g設定のアップグレードされたスキーマをOIM 12c設定でリワイヤリングします。

### 前提条件

この演習では、次を想定しています。

*   Free Tier、有料またはLiveLabs Oracle Cloudアカウント
*   次を完了しました:
    *   演習: 設定の準備(_無料層_および_有料テナント_のみ)
    *   演習: 環境設定
    *   演習: 環境の初期化
    *   演習: アップグレード前の要件

## タスク1: アップグレードに使用できる既存のスキーマの識別

1.  _sys_ユーザーとしてデータベースに接続し、次のSQL問合せを実行して既存のドメインのバージョンを確認します。
    
        <copy>sqlplus / as sysdba</copy>
        
    

SQL> SET LINE 120 COLUMN MRC\_NAME FORMAT A14 COLUMN COMP\_ID FORMAT A20 COLUMN VERSION FORMAT A12 COLUMN STATUS FORMAT A9 COLUMN UPGRADED FORMAT A8 SELECT MRC\_NAME, COMP\_ID, OWNER, VERSION, STATUS, UPGRADED FROM SCHEMA\_VERSION\_REGISTRY where OWNER like '%DEV11G%' ORDER BY MRC\_NAME, COMP\_ID; \`\`

バージョン11gが表示されることがわかります。

    ![](images/1-sql.png)
    
    ```
    <copy>exit</copy>
    ```
    

## タスク2: アップグレード・アシスタントの実行によるスキーマのアップグレード

1.  _oracle\_common/upgrade/bin_ディレクトリに移動します。
    
        <copy>cd /u01/oracle/middleware12c/oracle_common/upgrade/bin/</copy>
        
2.  アップグレード・アシスタントにJVMエンコーディング要件を含むようにパラメータを設定します
    
        <copy>export UA_PROPERTIES="-Dfile.encoding=UTF-8"</copy>
        
3.  Upgrade Assistantの起動
    
        <copy>./ua</copy>
        

アップグレード・アシスタントが起動します。

4.  アップグレード・タイプ- _ドメインで使用されるすべてのスキーマ_。11gドメイン・ホームの参照- _`/u01/oracle/middleware11g/user_projects/domains/iam11g_domain/`_
    
    ![](images/2-upgrade.png)
    
5.  「コンポーネント」リスト- _「次へ」_をクリックします
    
6.  前提条件チェック- すべての前提条件が満たされていることを確認してください。
    
    ![](images/3-upgrade.png)
    
7.  OPSSスキーマ
    
        DBA Username: <copy>FMW</copy>
        
    
        DBA Password: <copy>Welcom#123</copy>
        
    
    ![](images/3a-upgrade.png)
    
8.  MDSスキーマ- 同じユーザー名とパスワードが自動的に更新されます- _「次へ」_をクリックします
    
9.  UMSスキーマ- 同じユーザー名とパスワードが自動的に更新されます- _「次へ」_をクリックします
    
10.  SOAINFRAスキーマ- 同じユーザー名とパスワードが自動的に更新されます- _「次へ」_をクリックします
    
11.  OIMスキーマ- 同じユーザー名とパスワードが自動的に更新されます- _「次へ」_をクリックします
    
12.  スキーマの作成- _「指定したドメインの欠落スキーマの作成」_が選択されていることを確認します。_「すべてのスキーマに同じパスワードを使用」_をクリックし、スキーマ・パスワードを_Welcom#123_として入力します
    
        Schema Password: <copy>Welcom#123</copy>
        
    
    ![](images/4-upgrade.png)
    
13.  確認- _「次へ」_をクリックします
    
    ![](images/5-upgrade.png)
    
14.  スキーマの作成の進行状況- スキーマの作成が完了したら、_「アップグレード」_をクリックします
    
    ![](images/6-upgrade.png)
    
15.  アップグレードが正常に完了したら、アップグレード・アシスタントを閉じます。
    
    ![](images/7-upgrade.png)
    

## タスク3スキーマ・アップグレードの検証

1.  _sys_ユーザーとしてデータベースに接続し、次のSQL問合せを実行してバージョンを確認します
    
        <copy>sqlplus / as sysdba</copy>
        
    

SQL> SET LINE 120 COLUMN MRC\_NAME FORMAT A14 COLUMN COMP\_ID FORMAT A20 COLUMN VERSION FORMAT A12 COLUMN STATUS FORMAT A9 COLUMN UPGRADED FORMAT A8 SELECT MRC\_NAME、 COMP\_ID、 OWNER、 VERSION、 STATUS、 UPGRADED FROM SCHEMA\_VERSION\_REGISTRY where OWNER like '%DEV11G%' ORDER BY MRC\_NAME、 COMP\_ID; \`\`スキーマのバージョンが12cに正しく更新されていることを確認することで、スキーマのアップグレードが成功したことを確認できます。

    ![](images/8-sql.png)
    

## タスク4: 一時フォルダのクリーニング

1.  _/tmp_ディレクトリはJVM _java.io.tmpdir_プロパティーに対して設定されているため、_/tmp_フォルダ内の不要なファイルがOIGのアップグレードプロセスに干渉し、結果としてMDSが破損する可能性があります。したがって、アップグレードプロセスを開始する前に、_/tmp_フォルダをクリーニングします。
    
        <copy>rm -rf /tmp/*</copy>
        

## タスク5: 12c管理対象サーバーの停止

1.  ドメインをリワイヤリングする前に、SOA、OIM管理対象サーバーを停止します。管理サーバーとデータベースが稼働していることを確認します。まず、OIMサーバーを停止します。
    
        <copy>cd /u01/oracle/middleware12c/user_projects/domains/iam12c_domain/bin</copy>
        
    
        <copy>./stopManagedWebLogic.sh oim_server1</copy>
        
2.  OIMサーバーが停止したら、SOAサーバーを停止します。
    
        <copy>./stopManagedWebLogic.sh soa_server1</copy>
        
    
    Weblogic 12cコンソールを確認して、すべての管理対象サーバーがSHUTDOWN状態であることを確認します。
    
    ![](images/9-server12c.png)
    

## タスク6: ドメインのワイヤリング

1.  _oneHop.properties_ファイルのさまざまなプロパティー値を確認します
    
        <copy>vi /u01/Upgrade_Utils/oneHop.properties</copy>
        
2.  _oneHop.properties_ファイルを _<12c\_ORACLE\_HOME>/idm/server/upgrade/oneHopUpgrade_の場所にコピーします
    
        <copy>cp /u01/Upgrade_Utils/oneHop.properties /u01/oracle/middleware12c/idm/server/upgrade/oneHopUpgrade/</copy>
        
        
3.  _<12c\_ORACLE\_HOME>/idm/server/upgrade/oneHopUpgrade_ディレクトリに移動して、_oneHopUpgrade.sh_スクリプトを起動します。
    
        <copy>cd /u01/oracle/middleware12c/idm/server/upgrade/oneHopUpgrade/</copy>
        
    
        <copy>sh oneHopUpgrade.sh -p Welcom@123</copy>
        

実行時に、次のようにパスワードを指定します。

*   管理サーバー: **Welcom@123**
*   DEV11G\_OIM: **Welcom#123**
*   DEV11G\_SOAINFRA: **Welcom#123**
*   DEV11G\_STB: **Welcom#123**
*   DEV11G\_ORASDPM: **Welcom#123**
*   DEV11G\_WLS: **Welcom#123**
*   DEV11G\_MDS: **Welcom#123**
*   DEV11G\_IAU\_APPEND: **Welcom#123**
*   DEV11G\_IAU\_VIEWER: **Welcom#123**
*   DEV11G\_OPSS: **Welcom#123**
*   11gからのOPSS暗号化キーのエクスポートに使用されたパスワード: **Welcom@123**
*   ".xldatabasekey"キーストアのOIG12csp4設定のパスワード: **Welcom@123**
*   ".xldatabasekey"キーストアのOIG12csp4設定のキー・パスワード: **Welcom@123**
*   ".xldatabasekey"キーストアのOIG11gR2PS3設定のパスワード: **Welcom@123**
*   ".xldatabasekey"キーストアのOIG11gR2PS3設定のキー・パスワード: **Welcom@123**

リワイヤリングが完了するまでに約10~15分かかります。次のメッセージが表示されるまで待機します- 「Rewiring done successfully」

      ![](images/10-rewire.png)
    

これで、12cドメインがアップグレードされた11gスキーマに接続されました。この時点で、12cドメインのすべてのサーバーが停止されます。次の演習に進んで、すべてのサーバーを再起動し、アップグレード・プロセスを完了します。

[次の演習に進む](#next)ことができます。

## 確認

*   **著者** - NATDソリューション・エンジニアリング、Anuj Tripathi、Brijith TG、Keerti R氏
*   **貢献者** - Keerti R、Brijith TG、Anuj Tripathi
*   **最終更新者/日付** - 2021年6月、NATDソリューション・エンジニアリング、Keerti R氏