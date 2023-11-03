# アップグレード前の要件

## 概要

この演習では、Oracle Identity Manager 12cへのアップグレード前に実行するアップグレード前のタスク(バックアップ、現在の環境のクローニング、アップグレード前レポートの分析、システムが動作保証された要件を満たしていることの確認など)について説明します。

_推定ラボ時間_: 30分

### 目標

この演習では、次のことを行います。

*   アップグレード・アシスタントを実行する非SYSDBAユーザーの作成
*   OPSS暗号化キーのエクスポート
*   Upgrade Assistantを実行して、アップグレード前の準備状況チェックを実行します。

### 前提条件

この演習では、次を想定しています。

*   Free Tier、有料またはLiveLabs Oracle Cloudアカウント
*   次を完了しました:
    *   演習: 設定の準備(_無料層_および_有料テナント_のみ)
    *   演習: 環境設定
    *   演習: 環境の初期化

## タスク1: SYSDBA以外のユーザーの作成

Oracleでは、_FMW_というSYSDBA以外のユーザーを作成してアップグレード・アシスタントを実行することをお薦めします。このユーザーにはスキーマの変更に必要な権限がありますが、完全な管理者権限がありません。

1.  データベースにログインし、_fmw.sql_スクリプトを実行します
    
        <copy>sqlplus / as sysdba</copy>
        
    
        SQL> <copy>@/u01/scripts/FMW.sql</copy>
        
    
        SQL> <copy>exit</copy>
        

## タスク2: OPSS暗号化キーのエクスポートおよびコピー

Oracle Identity Manager 11g (11.1.2.3) setup.TheからOPSS暗号化キーをエクスポートします。次のステップを実行して、12c (12.2.1.4) OIGへのアップグレード後に11g (11.1.2.3) OIGから暗号化されたデータが正しく読み取られるようにします。エクスポートされたキーは、アップグレード・プロセスを完了するためにoneHopUpgradeツールで必要になります。

1.  _<11g\_(11.1.2.3\_ORACLE\_HOME>/oracle\_common/common/bin_の場所に移動します。
    
        <copy>cd /u01/oracle/middleware11g/oracle_common/common/bin</copy>
        
2.  _wlst.sh_スクリプトを起動します
    
        <copy>./wlst.sh</copy>
        
3.  オフライン・モードでの_exportEncryptionKey_ WLSTコマンドの実行
    
        <copy>exportEncryptionKey('/u01/oracle/middleware11g/user_projects/domains/iam11g_domain/config/fmwconfig/jps-config.xml', '/u01/OPSS_EncryptKey', 'Welcom@123')</copy>
        
4.  WLSTを終了します。
    
        <copy>exit ()</copy>      
        
    
    ![](images/1-wlst.png)
    
5.  _/u01/OPSS\_EncryptKey_ディレクトリに移動し、エクスポートされた暗号化鍵ファイルが作成されていることを確認します
    
        <copy>cd /u01/OPSS_EncryptKey</copy>
        
    
        <copy>ls -latr</copy>
        
6.  .xldatabasekeyを11g (11.1.2.3)設定場所から_/u01/OPSS\_EncryptKey_ディレクトリにコピーします。
    
        <copy>cp /u01/oracle/middleware11g/user_projects/domains/iam11g_domain/config/fmwconfig/.xldatabasekey /u01/OPSS_EncryptKey</copy>
        

## タスク3: アップグレード前の準備状況チェック

1.  Upgrade Assistantをレディネス・モードで実行して、アップグレード前の準備状況チェックを実行します
    
        <copy>cd /u01/oracle/middleware12c/oracle_common/upgrade/bin</copy>
        
    
        <copy>./ua -readiness</copy>
        
    
    アップグレード・アシスタントは準備モードで起動されます。
    
2.  ようこそ- _「次へ」_をクリックします
    
    ![](images/2-ua.png)
    
3.  準備状況チェック・タイプ- _ドメイン・ベース_。11g OIMホーム(_`/u01/oracle/middleware11g/user_projects/domains/iam11g_domain/`_)を参照します。
    
    ![](images/3-ua.png)
    
4.  コンポーネント・リスト- _「次へ」_をクリックします
    
    ![](images/4-ua.png)
    
5.  OPSSスキーマ- 適切な資格証明を入力し、_「接続」_をクリックしてデータベースへの接続をテストします
    
        DBA Username: <copy>FMW</copy>
        
    
        DBA Password: <copy>Welcom#123</copy>
        
    
    ![](images/5-ua.png)
    
6.  MDSスキーマ- 同じユーザー名とパスワードが自動的に更新されます- _「次へ」_をクリックします
    
    ![](images/6-ua.png)
    
7.  UMSスキーマ- 同じユーザー名とパスワードが自動的に更新されます- _「次へ」_をクリックします
    
    ![](images/7-ua.png)
    
8.  SOAINFRAスキーマ- 同じユーザー名とパスワードが自動的に更新されます- _「次へ」_をクリックします
    
    ![](images/8-ua.png)
    
9.  OIMスキーマ- 同じユーザー名とパスワードが自動的に更新されます- _「次へ」_をクリックします
    
    ![](images/9-ua.png)
    
10.  準備状況サマリー- _「次へ」_をクリックします
    
11.  準備状況チェックが完了したら、_「終了」_をクリックし、UAを_クローズ_します
    
    ![](images/10-ua.png)
    

## タスク4: Oracle Identity Managerのアップグレード前レポートの分析(オプション)

1.  アップグレード前レポート・ユーティリティは、既存のOracle Identity Manager環境を分析し、アップグレードを開始する前に完了する必要がある必須前提条件に関する情報を提供します。問題が解決されない場合はアップグレードが失敗する可能性があるため、アップグレード前レポートにリストされているすべての問題に対処してから、アップグレードを続行することが重要です。サンプル・アップグレード前レポートは、この演習の一部としてすでに生成されています。これらは、_`/u01/Upgrade_Utils/OIM_preupgrade_reports`_ディレクトリで表示および分析できます。
    
        <copy>cd /u01/Upgrade_Utils/OIM_preupgrade_reports</copy>
        
2.  _index.html_ページを開き、様々なレポートに移動して分析します。
    
        <copy>firefox index.html</copy>
        
    
    ![](images/Reports.png)
    

## タスク5: 11gのサーバーおよびプロセスの停止

Upgrade Assistantを実行してスキーマをアップグレードする前に、11g OIGドメインのすべてのプロセスとサーバー(管理サーバー、ノード・マネージャ(ノード・マネージャを構成した場合)およびすべての管理対象サーバーを含む)を停止する必要があります。

1.  _stopDomain11g.sh_スクリプトを実行して、すべての11gサーバーを停止します。
    
        <copy>cd /u01/scripts</copy>
        
    
        <copy>./stopDomain11g.sh</copy>
        

これで、実行するすべてのアップグレード前タスクが完了します。

[次の演習に進む](#next)ことができます。

## 確認

*   **著者** - NATDソリューション・エンジニアリング、Anuj Tripathi、Brijith TG、Keerti R氏
*   **貢献者** - Keerti R、Brijith TG、Anuj Tripathi
*   **最終更新者/日付** - 2021年6月、NATDソリューション・エンジニアリング、Keerti R氏