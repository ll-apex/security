# アプリケーション・オンボード

## 概要

このラボでは、フラット・ファイル・コネクタを使用してアプリケーションをOracle Identity Governance(OIG)にオンボードするステップについて説明します。

_見積時間_: 25分

### 目標

この演習では、次のことを行います。

*   フラット・ファイル・コネクタを使用したOIGへのアプリケーションのオンボード

### 前提条件

この演習では、次を想定しています。

*   Free Tier、有料またはLiveLabs Oracle Cloudアカウント
*   次を完了しました:
    *   演習: 設定の準備(_無料層_および_有料テナント_のみ)
    *   演習: 環境設定
    *   演習: 環境の初期化

## タスク1: OIGサーバーの起動

1.  _Weblogic管理コンソール_が事前ロードされた右側のWebブラウザ・ウィンドウで、まだログインしていない場合は、_「ユーザー名」_フィールドをクリックし、保存された資格証明を選択するか、次を使用してログインします。
    
    *   ユーザー名
    
        <copy>weblogic</copy>
        
    
    *   パスワード
    
        <copy>Welcome1</copy>
        
    
    ![](images/3-weblogic.png)
    
2.  _「環境」_の下の_「サーバー」_をクリックします。
    
    ![](images/4-weblogic.png)
    
3.  「Control」タブをクリックします。oim\_server1およびsoa\_server1を選択し、_「起動」_をクリックしてサーバーを起動します。これには約5分から8分かかる場合があります。
    
    ![](images/5-weblogic.png)
    
    ![](images/6-weblogic.png)
    
4.  サーバーが_「実行中」_状態であることに注意してください。
    
    ![](images/7-weblogic.png)
    

## タスク2: フラット・ファイルのディレクトリへのコピー

1.  oracleユーザーとしてターミナル・セッションを開き、権限ファイルをコピーします。
    
        <copy>
        cd ~
        unzip /u01/files/target/access/archived/documents_16-06-2021_09-51-31.zip -d /u01/files/target/access/
        ls -latr /u01/files/target/access
        </copy>
        
    
    ![](images/8-files.png)
    
2.  ユーザー・アカウント・ファイルのコピー
    
        <copy>
        unzip /u01/files/target/accounts/archived/accounts_16-06-2021_09-53-03.zip -d /u01/files/target/accounts/
        ls -latr /u01/files/target/accounts
        </copy>
        
    
    ![](images/9-files.png)
    

## タスク3: 「アプリケーションの作成」画面

1.  _Weblogic管理コンソール_が事前にロードされた右側のブラウザ・ウィンドウで、次のURLを新しいタブで開き、_「ユーザー名」_フィールドをクリックして保存された資格証明を選択するか、次を使用して_OIGアイデンティティ・コンソール_にログインします
    
    *   URL
    
        <copy>http://oiri.livelabs.oraclevcn.com:14000/identity/faces/signin</copy>
        
    
    *   ユーザー名
    
        <copy>xelsysadm</copy>
        
    
    *   パスワード
    
        <copy>Welcome1</copy>
        
    
    ![](images/10-oig.png)
    
2.  「管理」タブの「アプリケーション」ボックスを選択します。
    
    ![](images/11-application.png) ![](images/11a-application.png)
    
3.  「アプリケーション」ページで、ツールバーの「作成」メニューをクリックし、_「ターゲット」_オプションを選択してターゲット・アプリケーションを作成します。
    
    ![](images/12-application.png)
    

## タスク4: 基本情報の指定

1.  「基本情報」ページで、_「コネクタ・パッケージ」_オプションが選択されていることを確認します。
    
2.  「バンドルの選択」ドロップダウン・リストから、_「フラット・ファイル・コネクタ12.2.1.3.0」_を選択します。
    
3.  アプリケーションのアプリケーション名、表示名および説明を入力します。
    
        Application Name : <copy>DMS</copy>
        
    
        Display Name : <copy>Document Management System</copy>
        
    
    ![](images/1-app.png)
    
4.  「詳細設定」セクションを展開し、パラメータ_flatFileLocation_の値を入力します。
    
        flatFileLocation : <copy>/u01/files/target/accounts/accounts.csv</copy>
        
    
    ![](images/3-app.png)
    
5.  _「ヘッダーの解析」_をクリックすると、フラット・ファイルのヘッダーが解析されます。
    
6.  「フラット・ファイル・スキーマ・プロパティ」表
    
    *   MVA列で対応するチェック・ボックスを選択して、`document_access`を複数値としてマークします。
    *   ユーザー名属性の「名前」列を選択します。
    *   「データ型」列から「日付」データ型を選択して、`start_date`属性のデータ型を変更します。
    *   ID属性のUID列を選択します。

![](images/4-app.png)

7.  「次」をクリックして、「スキーマ」ページに進みます。

## タスク5: スキーマ情報の更新

1.  _`document_access`_属性を展開し、表示名を変更します。
    
        Display Name : <copy>DMS Access</copy>
        
    
    ![](images/5-app.png)
    
2.  _`document_access`_属性の「詳細設定」アイコンをクリックします。
    
    *   「Lookup and Entitlement(参照および資格)」チェックボックスを選択します。
        
    *   これらの詳細を入力します
        
            List of values : <copy>lookup.dms.access</copy>
            
        
            Length : <copy>15</copy>
            

![](images/5a-app.png)

![](images/6-app.png)

3.  ユーザー名、IDおよび`document_access`属性の「大/小文字を区別しない」列を選択します。
    
    ![](images/7-app.png)
    
4.  「次」をクリックして、「設定」ページに進みます。
    

## タスク6: 設定情報の指定

1.  設定ページで、プレビュー設定をクリックして設定をプレビューします。
    
2.  「プロビジョニング」タブで、ドロップダウンから「アカウント名」に_「ユーザー名」_を選択します。
    
    ![](images/8-app.png)
    
3.  「リコンシリエーション」タブで、「リコンシリエーション・ジョブ」を展開します。
    
    ![](images/9-app.png)
    
    ![](images/10-app.png)
    
4.  このワークショップではこれらのジョブは必要ないため、「フラット・ファイル差分同期」、「フラット・ファイル削除同期」および「フラット・ファイル削除」でジョブを削除します。
    
    ![](images/11-app.png)
    
5.  フラット・ファイル資格/権利ジョブの下のDMSフラット・ファイル資格/権利ローダーを展開し、これらの詳細を入力します。
    
        Flat File directory : <copy>/u01/files/target/access/</copy>
        
    
        Lookup Name : <copy>lookup.dms.access</copy>
        
    
        Code Key Attribute : <copy>ID</copy>
        
    
        Decode Attribute : <copy>Access</copy>
        
    
    ![](images/13-app.png)
    
6.  「フラット・ファイル・フル」ジョブの下の「DMSフラット・ファイル・アカウント・ローダー」を展開し、これらの詳細を入力します。
    
        Flat File directory : <copy>/u01/files/target/accounts/</copy>
        
    
    ![](images/14-app.png)
    
7.  「次へ」をクリックして、「終了」ページに進みます。
    
    ![](images/15-app.png)
    

## タスク7: 応募詳細のレビューおよび発行

1.  「終了」ページでは、アプリケーション・サマリーを確認し、「終了」をクリックしてアプリケーションを送信します。
    
    ![](images/16-app.png)
    
2.  「はい」をクリックして、デフォルトの要求フォームを作成します。
    
    ![](images/17-app.png)
    
3.  「アプリケーション」ページで、「検索」アイコンをクリックします。作成したDMSアプリケーションがリストされていることを確認します。
    
    ![](images/18-app.png)
    
4.  ログアウトして、アイデンティティ・セルフ・サービスに再度ログインします。
    

## タスク8: リコンシリエーションの実行

1.  「管理」タブの「アプリケーション」ボックスを選択します。
    
2.  「検索」アイコンをクリックし、DMSアプリケーション行を選択します。
    
3.  「Manage Jobs(ジョブの管理)」を選択します。
    
    ![](images/19-app.png)
    
4.  サービス内容調整の実行
    
    *   「Flat File Entitlement(フラット・ファイル権限)」を展開し、「DMS Flat File Entitlements Loader(DMSフラット・ファイル権限ローダー)」を展開します。
    *   「Run now」をクリックし、「Refresh」アイコンを複数回クリックして、「Stopped::Success」結果がジョブ履歴の下に表示されることを確認します。
    
    ![](images/20-app.png)
    
    ![](images/21-app.png)
    
5.  完全リコンシリエーションの実行
    
    *   「Flat File Full」を展開し、「DMS Flat File Accounts Loader」を展開します。
    *   「Run now」をクリックし、「Refresh」アイコンを複数回クリックして、「Stopped::Success」結果が「Job history」の下に表示されることを確認します。
    
    ![](images/22-app.png)
    
    ![](images/23-app.png)
    
6.  「管理」タブの「ユーザー」ボックスに移動し、任意のユーザーをクリックします。
    
    ![](images/24-app.png)
    
    ![](images/25-app.png)
    
7.  「Entitlements」タブおよび「Accounts」タブをクリックし、ユーザーが適切な資格を使用して「DMS」アプリケーションにプロビジョニングされていることを確認します。
    
    ![](images/26-app.png)
    
    ![](images/27-app.png)
    

## 確認

*   **著者** - NATDソリューション・エンジニアリング、Brijith TG、Vineeth Boopathy、Keerti R氏
*   **貢献者** - Keerti R、Brijith TG、Vineeth Boopathy
*   **最終更新者/日付** - Rene Fontcha、LiveLabs Platform Lead、NA Technology、2021年11月