# 設定

## 概要

前の演習で、ADBインスタンスを作成しました。この演習では、Oracle Cloud ShellからADBインスタンスに接続します。

見積時間: 20分

### 目標

*   バケット、認証トークンおよびOracle Walletの作成
*   ADBインスタンスのロード
*   ユーザーへのロールおよび権限の付与
*   ユーザーのデータベース資格証明の作成

### 前提条件

*   演習: ADBのプロビジョニング

## タスク1: バケットの作成

1.  まだログインしていない場合は、Oracle Cloudにログインします。
    
2.  ハンバーガー・メニューをクリックし、**「ストレージ」**に移動して**「バケット」**をクリックします。
    
    ![](./images/object_storage.png " ")
    
3.  ATPがプロビジョニングされているコンパートメントを選択し、**「バケットの作成」**をクリックします。
    
    ![](./images/step1-3.png " ")
    
4.  バケットに**adb1**という名前を付け、**「作成」**をクリックします。
    
    ![](./images/step1-4.png " ")
    
5.  バケットが作成されたら、バケットをクリックし、`bucket name`および`namespace`をノートにとります。
    
    ![](./images/step1-5.png " ")
    

## タスク2: クラウド・シェルでのOracle Walletの作成

ADB用のOracle Walletを作成するには、複数の方法があります。これはこのワークショップの焦点ではないため、Oracle Cloud Shellを使用します。Oracleウォレットの詳細とインタフェースを使用したウォレットの作成については、このワークショップのラボを参照してください: [ADBを使用したデータの分析- 演習6](https://apexapps.oracle.com/pls/apex/dbpm/r/livelabs/view-workshop?p180_id=553)

1.  まだログインしていない場合は、Oracle Cloudにログインします。
    
2.  クラウド・シェル・アイコンをクリックして、クラウド・シェルを起動します ![](./images/cloud-shell.png " ")
    
3.  Cloud Shellの起動中に、「ハンバーガー」メニュー→**「Autonomous Transaction Processing」**をクリックします ![](https://oracle-livelabs.github.io/common/images/console/database-atp.png " ")
    
4.  **「表示名」**をクリックして、ADBメイン・ページに移動します。
    
    ![](./images/step2-4.png " ")
    
5.  数分で必要になる**OCID** (Oracle Cloud ID)を見つけてコピーします。
    
    ![](./images/locate-ocid.png " ")
    
6.  autonomous\_database\_ocidを使用して、Oracle Walletを作成します。使いやすくするために、ウォレット・パスワードをADB管理パスワードと同じ値に設定します: _WElcome123##_ノート: これは推奨される方法ではなく、この演習の目的で使用します。
    
7.  次のコマンドをコピーして、クラウド・シェルに貼り付けます。まだ入らないでください。
    
        <copy>
        cd ~
        oci db autonomous-database generate-wallet --password WElcome123## --file 21c-wallet.zip --autonomous-database-id  </copy> ocid1.autonomousdatabase.oc1.iad.xxxxxxxxxxxxxxxxxxxxxx
        
    
    ![](./images/wallet.png " ")
    
8.  「コピー」を押してステップ5のOCIDをコピーし、terraformの出力セクションにリストされている自律型データベースOCIDを入力します。--autonomous-database-id句とOCIDの間に空白があることを確認します。**「Enter」**をクリックします。辛抱強く、約20秒かかります。
    
9.  ウォレット・ファイルは、/home/yourtenancynameのクラウド・シェル・ファイル・システムにダウンロードされます
    
10.  次のクラウド・シェルにlistコマンドを入力して、_21c-wallet.zip_が作成されたことを確認します
    
        ls
        
    
    ![](./images/21cwallet.png " ")
    

## タスク3: 認証トークンの作成

1.  右上隅の個人アイコンをクリックします。
    
2.  **「ユーザー設定」**を選択します。
    
    ![](./images/select-user.png " ")
    
3.  **ユーザー名**をコピーします。
    
    ![](./images/copy-username.png " ")
    
4.  **「ユーザー情報」**タブで、**「コピー」**ボタンをクリックしてユーザー**OCID**をコピーします。
    
    ![](./images/copy-user-ocid.png " ")
    
5.  次のコマンドを使用して、実際の_ユーザーOCID_を次のユーザーIDに置き換えて、説明`adb1`の認証トークンを作成します。_ノート: すでに認証トークンがある場合、ユーザー当たり2を超える作成を試みるとエラーが発生する可能性があります_
    
        <copy>
         oci iam auth-token create --description adb1 --user-id </copy> ocid1.user.oc1..axxxxxxxxxxxxxxxxxxxxxx
        
    
    ![](./images/token.png " ")
    
6.  **"token"**で始まる出力の行を特定します。
    
7.  **token**の値を安全な場所にコピーすると、次の手順で必要になります。
    

## タスク4: アプリケーション・スキーマを使用したADBインスタンスのロード

1.  クラウド・シェルに戻り、クラウド・シェルがまだ実行されていない場合は起動します。
    
2.  wgetコマンドを実行して、オブジェクト・ストレージからload\_21c.shスクリプトをダウンロードします。
    
        <copy>
        cd $HOME
        pwd
        wget https://objectstorage.us-ashburn-1.oraclecloud.com/p/VEKec7t0mGwBkJX92Jn0nMptuXIlEpJ5XJA-A6C9PymRgY2LhKbjWqHeB5rVBbaV/n/c4u04/b/livelabsfiles/o/data-management-library-files/load-21c.sh
        chmod +x load-21c.sh
        export PATH=$PATH:/usr/lib/oracle/19.10/client64/bin
        </copy>
        
3.  ノートパッドから2つの引数、管理パスワードおよびATPインスタンスの名前を渡すロード・スクリプトを実行します。このスクリプトは、すべてのデータをアプリケーションのATPインスタンスにインポートし、スキーマごとにSQL Developer Webを設定します。このスクリプトはopcユーザーとして実行されます。ATP名は、ADBインスタンスの名前である必要があります。次の例では、_adb1_を使用しました。このロード・スクリプトの実行には約3分かかります。_ノート: 別のADB名を使用する場合は、adb1をADBインスタンス名に置き換えます_
    
        <copy> 
        ./load-21c.sh WElcome123## adb1 2>&1 > load-21c.out</copy>
        
    
    ![](./images/load21c-1.png " ")
    

## タスク5: ユーザーへのロールおよび権限の付与

1.  「Autonomous Databaseホームページ」に戻ります。
    
    ![](./images/step4-0.png " ")
    
    ![](./images/step4-1.png " ")
    
2.  **「ツール」**タブをクリックします。
    
    ![](./images/step4-tools.png " ")
    
3.  **「データベース・アクション」**をクリックします。
    
    ![](./images/step4-database.png " ")
    
4.  ユーザー名として**「admin」**を選択します。
    
    ![](./images/step4-admin.png " ")
    
5.  パスワード: **WElcome123##**。
    
    ![](./images/step4-password.png " ")
    
6.  「管理」で**「データベース・ユーザー」**を選択します。
    
    ![](./images/step4-databaseuser.png " ")
    
7.  **HR**ユーザーの場合は、**3つのドット**をクリックしてメニューを展開し、**「編集」**を選択します。
    
    ![](./images/step4-edit.png " ")
    
8.  **「REST有効化」**および**「認可が必要」**スライダを有効にします。
    
    ![](./images/step4-enable-rest.png " ")
    
9.  上部にある**「付与されたロール」**タブをクリックします。
    
    ![](./images/step4-roles.png " ")
    
10.  **「DWROLE」**を下にスクロールし、**「1番目」**および**「3番目」**チェック・ボックスが有効になっていることを確認します。
    
    ![](./images/step4-dwrole.png " ")
    
11.  最下部までスクロールし、**「変更の適用」**をクリックします。
    
    ![](./images/step4-apply.png " ")
    
12.  検索バーの**「X」**をクリックして、すべてのユーザーを再度表示します。
    
    ![](./images/step4-cancel-search.png " ")
    
13.  **OE**ユーザーに対してステップ7から12を繰り返します。
    
    ![](./images/step5-13a.png " ")
    
    ![](./images/step5-13b.png " ")
    
    ![](./images/step5-13c.png " ")
    
    ![](./images/step5-13d.png " ")
    
    ![](./images/step5-13e.png " ")
    
14.  **REPORT**ユーザーに対して、ステップ7から12を繰り返します。
    
    ![](./images/step5-14a.png " ")
    
    ![](./images/step5-14b.png " ")
    
    ![](./images/step5-14c.png " ")
    
    ![](./images/step5-14d.png " ")
    
    ![](./images/step5-14e.png " ")
    

## タスク6: SQL Developer Webへのログイン

1.  SQL Developer Webにログインして、データがロードされていることを確認します。
    
2.  左上にある**「ハンバーガー・ボタン」**を選択し、**「開発」**タブを展開します。**「SQL」**を選択します。
    
    ![](./images/step4-sql.png " ")
    
3.  **「X」**をクリックしてポップアップを閉じます。
    
    ![](./images/step4-sql-x.png " ")
    
4.  次のコード・スニペットを実行し、665アイテムがあることを確認します。
    
        <copy>
        select count(*) from oe.order_items;
        </copy>
        
    
    ![](./images/step4-run.png " ")
    

## タスク7: ユーザーのデータベース資格証明の作成

オブジェクト・ストアのデータにアクセスするには、データベース・ユーザーがOCIオブジェクト・ストア・アカウントおよび認証トークンを使用してオブジェクト・ストアで自身を認証できるようにする必要があります。これを行うには、Autonomous Transaction Processingで暗号化されたこの情報を格納するユーザーのプライベートCREDENTIALオブジェクトを作成します。この情報は、ユーザー・スキーマでのみ使用できます。

1.  コード・スニペットをコピーしてSQL Developerワークシートに貼り付けます。`<username>`および`<token>`を次のユーザー名とパスワードに置き換えて、Oracle Cloud Infrastructure Object Storageサービスの資格証明を指定します:
    
    *   資格証明名: 認証トークンの説明。この例では、ステップ1の説明(`adb1`)を使用して認証トークンが作成されます。
    *   ユーザー名: ユーザー名は、ステップ3で書き留めた**OCIユーザー名**になります
    *   パスワード: パスワードは、ステップ3で生成したOCIオブジェクト・ストア認証**トークン**になります。
    
        	<copy>
        	BEGIN
        		DBMS_CLOUD.CREATE_CREDENTIAL(
        		credential_name => 'adb1',
        		username => '<username>',
        		password => '<token>'
        		);
        	END;
        	/
        	</copy>
        
    
    ![](./images/step7-1.png " ")
    
    これで、オブジェクト・ストアからデータをロードする準備ができました。
    
2.  「**ADMIN**」と「**Sign Out**」という単語の横にある下矢印をクリックします。
    
    ![](./images/step4-signout.png " ")
    

**次の演習に進む**ことができます。

## 謝辞

*   **著者** - データベース製品管理担当シニア・ディレクター、Kay Malcolm
*   **貢献者** - Anoosha Pilli、Didi Han、データベース製品管理
*   **最終更新者/日付** - Didi Han、2021年4月