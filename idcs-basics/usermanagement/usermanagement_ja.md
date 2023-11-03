# インスタンスのプロビジョニング

## 概要

多くの場合、組織は従業員や請負業者、またはIDCSのような一元的なアイデンティティ・システムに手動でアイデンティティを組み込む必要があります。このユースケースでは、IDCSユーザー管理者がIDCSサービスに新規ユーザーを手動で追加します。通常、これは自動プロビジョニング、一括フラットファイル・インポートまたはオンプレミスのActive Directoryとの同期によって発生します。

IDCS機能は、IDCS REST APIサービスを使用して100%構築されています。したがって、Webインタフェースで対話的に実行するタスクも、これらの同じREST APIコールを使用してカスタム・アプリケーションを介して実行できます。これは、IDCSを使用したOracleのAPIファースト・アーキテクチャの明確な利点です。

IDCSでは、オンプレミスActive Directoryからのユーザー(グループ)のオンボーディング、ファイル・アップロード、REST API、オンプレミスのOracle Identity Managementソリューションの使用、またはIDCS管理コンソールからの手動によるオンボーディングがサポートされています。ワークショップでは、ファイル・アップロード・オプションとAPIコールのユーザー管理を使用します。

推定ラボ時間: 90分

### 目標

この演習では、次のことを行います。

*   UIでのユーザーの作成
*   CSVでユーザーをインポート
*   REST APIを使用したユーザーの作成

### 前提条件

*   Oracle Free Tierまたは有料アカウント
*   ローカルにインストールされたPostmanネイティブ・アプリケーション

## タスク1: UIでのユーザーの作成

*   _ペルソナ_:
    *   管理者

1.  管理者アカウントの資格証明を使用してIDCS管理コンソールに移動します。左側の_「ユーザー」_メニューを選択し、_+Add_をクリックするか、ダッシュボードから_「ユーザーの追加」_アイコンを選択します。
    
    ![イメージ](images/L1001.png)
    
2.  すべての必須フィールドに入力し、「終了」をクリックします。演習全体で一貫性のある命名規則を使用することをお薦めします。次に例を示します。
    
    *   名: Firstname
    *   姓: Lastname-UI
    *   ユーザー名/電子メール: firstname.lastname+ui@domain.tld
    
    ![イメージ](images/L1002.png)
    
3.  ユーザーの作成を確認します。これを行うには、管理コンソールの_「ユーザー」_タブに移動します。新しいユーザーがコンソールに表示されることを確認します。
    
    ![イメージ](images/L1003.png)
    
        Once a new user is created in the IDCS service, the user receives an email invitation to finish first time login formalities such as setting a password to logging into IDCS.
        

## タスク2: CSVを使用したユーザーのインポート

*   _ペルソナ_:
    *   管理者

アイデンティティ・ドメイン管理者またはユーザー管理者である場合は、カンマ区切り値(CSV)ファイルを使用してユーザー・アカウントをバッチ・インポートできます。

1.  アップロードCSVファイルを取得します。これを行うには、左側の_「ユーザー」_メニューを選択し、_「インポート」_をクリックします。右側の「ヘルプ」セクションで、_「サンプル・ファイルのダウンロード」_をクリックします。
    
    ![イメージ](images/L1004.png)
    
    **または**、既存のユーザーのリストをエクスポートできます。UIで作成したユーザーをフィルタしてから、「エクスポート」->「すべてエクスポート」をクリックします。エクスポートを確認してからジョブに移動し、ファイルをダウンロードします
    
    ![イメージ](images/L1005.png)
    
    zipファイルを展開して _Users.CSV_を開くか、_UserExport... .CSV_ファイルを開きます。お気に入りのエディタからファイルの内容を調べます。CSVファイルで確認したデフォルトの例を使用するか、必要に応じていくつか入力できますが、例と同様です。たとえば、エクスポートした既存のユーザーを使用する場合は、接尾辞(UI)を別のユーザー(CSVなど)に変更します。
    
2.  管理者アカウントの資格証明を使用してIDCS管理コンソールに移動します。左側の_「ユーザー」_メニューを選択するか、ダッシュボードから_「ユーザー」_アイコンを選択します。
    
    ![イメージ](images/L1006.png)
    
3.  _「インポート」_ボタンをクリックします。
    
    ![イメージ](images/L1007.png)
    
4.  「ユーザーのインポート」画面がポップアップします。_「参照」_をクリックします。CSVファイルを選択します。_「インポート」_をクリックします。
    
    ![イメージ](images/L1008.png)
    
        By uploading the sample file, sample users will be uploaded into IDCS. In order to upload different users, the csv sample file needs to be altered.
        
5.  _「ジョブ」_メニュー項目に移動し、インポート・ジョブが正常に終了したことを確認します。_「詳細の表示」_をクリックして、すべてのユーザーがインポートされたかどうかを確認します。また、csvのすべてのユーザーのリストが、すべての属性の詳細および作成のステータスとともに表示されます。
    
    ![イメージ](images/L1009.png)
    
6.  管理コンソールの_「ユーザー」_タブに移動して、ユーザーの作成を確認します。新しいユーザーがコンソールに表示されることを確認します。
    
    ![イメージ](images/L1010.png)
    
7.  ターゲットのエンドユーザー(Firstname Lastname-CSVなど)をクリックし、ユーザーの詳細な属性情報を確認します。
    
8.  簡単にするために、ユーザーの電子メールを制御する電子メールに変更し、_「ユーザーの更新」_を選択します。次に、「_Reset Password_」ボタンをクリックします。
    
    ![イメージ](images/L1011.png)
    
9.  電子メールでリセット・メッセージを確認し、パスワードを変更します。
    
    ![イメージ](images/L1012.png)
    
10.  次に、別のブラウザを開き(Chromeを使用している場合は、「Incognito Window」を選択します)、管理者としてIDCS管理コンソールにログインします。
    
        https://<your tenant>/ui/v1/adminconsole
        
    
    ![イメージ](images/L1013.png)
    
    ユーザーにはまだアプリケーションへのアクセス権がないことがわかります。演習2では、ユーザーがアプリケーションをリクエストする方法を説明します。
    
    ![イメージ](images/L1014.png)
    

## タスク3: REST APIを使用したAPIユーザーの作成

*   _ペルソナ_:
    *   管理者

このユースケースでは、RESTクライアント(この場合はPostman)を使用してIDCSへのAPIコールを実行します。

関連するREST APIコールのPostmanコレクションが各参加者に提供されます。

    This use case will demonstrate using a common utility, Postman, for connecting to Identity Cloud Service via the out-of-the-box REST API.
    

### IDCSへのクライアントPOSTMANアプリケーションの登録

1.  管理者としてIDCS管理コンソールにログインします:
    
        https://<your tenant>/ui/v1/adminconsole
        
2.  ログイン後に表示されるIDCSダッシュボードから_「アプリケーション」_タブを選択します
    
    ![イメージ](images/L1015.png)
    
3.  _「追加」_ボタンをクリックして、Postmanで使用する新規アプリケーションを作成します。PostmanがIDCS REST APIをコールできるようにするには、最初にPostmanにそれを許可するCLIENT\_IDおよびCLIENT\_SECRETが必要です。これを実現するには、次のステップに示すように、特定の認可付与タイプを使用してIDCSに機密アプリケーション・タイプを作成します。
    
    ![イメージ](images/L1016.png)
    
4.  アプリケーション・タイプのポップアップ・メニューから_「機密アプリケーション」_を選択します
    
    ![イメージ](images/L1017.png)
    
5.  _「名前」_をPostman-<\>に設定し、_「次へ」_をクリックします。
    
    ![イメージ](images/L1018.png)
    
6.  認可付与タイプおよびAPIロールを提供するために、_「このアプリケーションをクライアントとして今すぐ構成」_をクリックします。
    
    ![イメージ](images/L1019.png)
    
7.  すべての_「許可される付与タイプ」_チェック・ボックスを選択し、_「リダイレクトURL」_をhttps://localhost.に設定します。_「Identity Cloud Service管理APIへのクライアント・アクセス権の付与」_セクション(ページの下部)に移動し、次のAPIロール_「Identity Domain Administrator」_を追加します。このようにして、基本的にこのIDCSアプリケーションに対するアクセス権をIDCS APIのフル・セットに付与します。次に、_「次へ」_をクリックします。
    
    ![イメージ](images/L1020.png)
    
8.  _「リソース」_で変更を行わず、_「次へ」_をクリックします
    
    ![イメージ](images/L1021.png)
    
9.  _「Web層ポリシー」_で変更を行わず、_「次へ」_をクリックします。
    
    ![イメージ](images/L1022.png)
    
10.  Auth2.0 標準を介してPostmanにAPIロールを付与するIDCSアプリケーションをファイナライズするには、_「終了」_をクリックします。
    
    ![イメージ](images/L1023.png)
    
11.  アプリケーションが作成されたら、_「クライアントID」_および_「クライアント・シークレット」_を書き留め、_「閉じる」_をクリックします。これらは、Auth2.0標準プロトコルを使用してIDCS APIをコールするためにPostmanデスクトップ・アプリケーションで使用されます。
    
    ![イメージ](images/L1024.png)
    
12.  _「アクティブ化」_ボタンをクリックします。
    
    ![イメージ](images/L1025.png)
    
13.  アプリケーションのアクティブ化の確認
    
    ![イメージ](images/L1026.png)
    
14.  アプリケーションがアクティブになり、すぐに使用できます。
    
    ![イメージ](images/L1027.png)
    
15.  IDCSからのサイン・アウト
    

### Postmanの構成

1.  Postmanを開きます。起動メッセージがある場合はすべて無視します。
    
    ![イメージ](images/L1028.png)
    
2.  まず、IDCS Postman環境変数、グローバル変数およびIDCS APIコレクションをインポートする必要があります。左上隅にある_「インポート」_ボタンをクリックします。
    
    ![イメージ](images/L1029.png)
    
3.  _「リンクからインポート」_を選択し、[「環境の例」](https://github.com/oracle/idm-samples/raw/master/idcs-rest-clients/example_environment.json)URLをボックスに入力して環境変数をインポートし、_「インポート」_をクリックします
    
    ![イメージ](images/L1030.png)
    
4.  Oracle Identity Cloud Service REST API Postmanコレクションをインポートするには、Postmanのメイン・ページで_「インポート」_を再度クリックします。
    
5.  「インポート」ダイアログ・ボックスで、_「リンクからインポート」_を選択し、ボックスに[IDCS Postmanコレクション](https://github.com/oracle/idm-samples/raw/master/idcs-rest-clients/REST_API_for_Oracle_Identity_Cloud_Service.postman_collection.json)URLを指定して、_「インポート」_をクリックします。
    
        The left pane of Postman has a sub-tab called Collections.
        With Collections selected, you will see the various REST statements which have been pre-configured for today’s workshop.
        
6.  グローバル変数ファイルをインポートするには、_「インポート」_をクリックします。
    
7.  「インポート」ダイアログ・ボックスで、_「リンクからインポート」_を選択し、ボックスに[IDCSグローバル変数](https://github.com/oracle/idm-samples/raw/master/idcs-rest-clients/oracle_identity_cloud_service_postman_globals.json)のURLを指定して、_「インポート」_をクリックします。
    
8.  _「変数を含むOracle Identity Cloud Serviceサンプル環境」_環境を選択し、_「設定」_ボタン(歯車のアイコン)をクリックして_「環境の管理」_を選択します
    
    ![イメージ](images/L1032.png)
    
9.  新しく作成した環境(_変数を含むOracle Identity Cloud Serviceの環境例_など)をクリックします。これは、Postmanの新規インストールの場合に使用できる唯一の環境です。
    
    ![イメージ](images/L1033.png)
    
10.  IDCSアクセス・トークンを取得できるように、次のパラメータの初期値と現在の値を設定します。
    
    | パラメータ | 値 |
    | --- | --- |
    | ホスト | IDCSテナントURL(例: https://IDCS-8b000000000000000000000000000000.identity.oraclecloud.com) |
    | CLIENT\_ID | Postman IDCSクライアント・アプリケーションCLIENT\_ID |
    | CLIENT\_SECRET | Postman IDCSクライアント・アプリケーションCLIENT\_SECRET |
    
    ![イメージ](images/L1034.png)
    
11.  完了後、_「更新」_をクリックします。
    

### アクセス・トークンを要求

    The steps performed above are being done to obtain the Access Token, which is shown in the following screen shot.
    
    There are several types of OAUTH requests supported by IDCS, including client credentials.  With a client credentials request, the client ID and client secret are used by the calling application to identify itself and to request an Access Token.  The Access Token is available for use by the application until the time that it expires.
    
    This Access Token establishes trust between your application and IDCS.
    

1.  _「コレクション」_タブで、_OAuth_、_「トークン」_の順に展開します。
    
2.  _「access\_tokenの取得(クライアント資格証明)」_を選択し、_「送信」_をクリックします。アクセス・トークンは、Oracle Identity Cloud Serviceからのレスポンスで返されます。
    
    ![イメージ](images/L1035.png)
    
3.  ステータスが_200 OK_であるかどうかを検証します。 ![イメージ](images/L1036.png)
    
4.  アクセス・トークンの内容を引用符で囲み、\*右クリック()で囲みます。ショートカット・メニューで、_「設定: 変数を含むOracle Identity Cloud Serviceサンプル環境」_を選択します。セカンダリ・メニューで、_access\_token_を選択します。強調表示されたコンテンツがアクセス・トークン値として割り当てられます。
    
    ![イメージ](images/L1037.png)
    

### APIを介したIDCSユーザーの作成

1.  _「コレクション」_タブで、_「ユーザー」_、_「作成」_の順に展開します。
    
2.  _「ユーザーの作成」_を選択します。要求情報は、印刷画面の下に表示されるデフォルト値とともに表示されます。設定に従って変更することも、デフォルトの入力値を使用することもできます。次に例を示します。
    

*   givenName: 名
*   familyName: 姓-REST
*   ユーザー名/電子メール->値(x2): firstname.lastname+REST@domain.tld

3.  _「本文」_をクリックし、_「送信」_をクリックします。
    
    ![イメージ](images/L1038.png)
    
4.  レスポンスで、ステータス_「201 Created」_が表示され、Oracle Identity Cloud Serviceで正常に作成されたユーザーの詳細がレスポンス本文に表示されることを確認します。
    
5.  IDCS UIでは、作成中のユーザーを表示できます。
    
    ![イメージ](images/L1039.png)
    

REST APIを使用して、ユーザーを_変更_および_削除_することもできます。詳細は、次のドキュメントを参照してください。

次の演習に進むことができます。

## 詳細情報

チュートリアル1: [Oracle Identity Cloud Service: 最初のREST APIコール](http://www.oracle.com/webfolder/technetwork/tutorials/obe/cloud/idcs/idcs_rest_1stcall_obe/rest_1stcall.html)

チュートリアル2: [Oracle Identity Cloud Service: REST APIコールを使用したユーザーの管理](http://www.oracle.com/webfolder/technetwork/tutorials/obe/cloud/idcs/idcs_rest_users_obe/rest_users.html)

## 確認

*   **作成者** - SEHubセキュリティおよび管理チーム
*   **最終更新者/日付** - プリンシパル・ソリューション・エンジニア、Lucian Ionescu、15.09.2020