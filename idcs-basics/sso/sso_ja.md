# 環境の設定

## 概要

Oracle Identity Cloud Service (IDCS)は、_SAML_ (Security Access Markup Language)プロトコルを介して統合できるすべてのサービスとの統合を提供します。管理は、単一のコントロールパネルを介して様々なアプリケーションにユーザーを管理することができ、エンドユーザーはワンクリックでアプリケーションにアクセスできるようになります。

IDCSは、標準のSAML 2.0ブラウザPOSTログインおよびログアウト・プロファイルのサポートを提供します。

IDCSはSAMLおよびOpenID Connect/OAuthをサポートしていますが、SAML対応でないシステムでSSOが必要な場合、何度もあることに注意してください。IDCS App Gateは、ヘッダーベース、Cookieベースまたはフォーム入力SSOを使用する非SAMLシステムをサポートします。App Gateは、プロキシ構成にデプロイされたNginxベースのソフトウェア・アプライアンスで、VMWareまたはOracle VirtualboxのAWS、OCI、OCI-Cまたはオンプレミスにデプロイできます。Identity Cloud Serviceを介して提供され、IDCS Standardのお客様が利用できます。

このラボは、サード・パーティのSaaSアプリケーションとのフェデレーテッド・シングル・サインオン(SSO)を示すことを目的としています。目的は、IDCSをOracleおよびOracle以外の製品およびサービスの増大するポートフォリオの中心的なアイデンティティ・プロバイダとして持つというビジネス価値を示すことです。

推定ラボ時間: 90分

### 目標

この演習では、次のことを行います。

*   Salesforceアプリケーションの構成
*   グループへのアプリケーションの割当て
*   グループ・アクセスのリクエスト
*   SSO構成の検証
*   プロビジョニングおよび同期の構成

### 前提条件

*   Oracle Free Tierまたは有料アカウント
*   Googleアカウント
*   Salesforce開発者アカウント

## タスク1: Salesforceアプリケーションの構成

*   _ペルソナ_:
    *   管理者

この実習では、SAMLを使用した_Salesforce_との統合を設定します。IDCSは_IdP_ (アイデンティティ・プロバイダ)として機能し、Salesforce組織は_SP_ (リライイング・パーティとも呼ばれるサービス・プロバイダ)として機能します

1.  ローカルXMLファイルにIDCS Metadataをダウンロードします。Metadataは、次の場所から入手できます。
    
        https://<your tenant>/fed/v1/metadata
        
    
    使用しているブラウザに応じて、最も簡単な方法は、ページ上の任意の場所を右クリックして「名前を付けて保存」オプションを選択し、名前を指定してXMLファイルとして保存することです。XMLファイルが変更される可能性があるため、コピー/貼付けオプションを使用しないでください。
    
    ![イメージ](images/L2001.png)
    

**次のステップ2から8は、ワークショップの前提条件の項で説明されています。**

2.  [Salesforce開発者アカウント](https://developer.salesforce.com)に登録します。
    
    ![イメージ](images/L2002.png)
    
    登録したら、Eメールを確認してアカウント登録を確認し、Salesforceにログインします。「Salesforce Classicに切替え」を選択する必要がある場合があります。
    
3.  登録後に提供されたURLを使用してSalesforceアプリケーションにアクセスし、次の印刷画面に従ってプロファイル・メニューをクリックし、オプション_「Salesforce Classicに切替え」_を選択します。
    
    ![イメージ](images/L2003.png)
    
4.  プロファイルの近くにある上部タブ・メニューの_「設定」_に移動します。
    
    ![イメージ](images/L2004.png)
    
5.  カスタム・ドメインを登録します。_「ドメイン管理」_に移動して_「自分のドメイン」_を選択し、選択したドメインを登録します。これは、カスタム・アプリケーションURLをユースケース専用にするためです。
    
6.  新しいドメインの_可用性を確認_します。
    
7.  新しいドメインを_登録_します。
    
    ![イメージ](images/L2005.png)
    
8.  Salesforceは、ドメイン・レジストリを新しいレジストリで更新します。完了すると、確認のメールが届きます。ドメインが使用可能になるまでに数分かかる場合があります。
    
9.  「ドメイン管理」で、「ドメイン」をクリックし、新しく作成したドメインをクリックします。
    
    ![イメージ](images/L2006.png)
    
10.  新しいドメインを登録したら、ドメインURLを書き留めます。
    
    ![イメージ](images/L2007.png)
    
11.  上のURLを使用して新しいドメインでログインし、_「ドメイン管理」_に戻り、_「ドメイン」_をクリックしてから、最近作成したドメインをクリックします。
    
12.  _「ユーザーへのデプロイ」_をクリックします。ポップアップ・ウィンドウが表示されたら、_「OK」_をクリックします。
    
    ![イメージ](images/L2008.png)
    
13.  これで、ドメインが設定され、ユーザーにデプロイされました。「自分のドメイン」でステップ4が完了し、_「ユーザーにデプロイされたドメイン」_が表示されていることを確認します。
    
14.  サイド・メニュー・バーから、_「セキュリティ制御」_に移動して_「シングル・サインオン設定」_を選択します。
    
15.  _「編集」_をクリックし、_「SAMLを使用したフェデレーテッド・シングル・サインオン」_オプションを有効にします。_「保存」_をクリックします。
    
    ![イメージ](images/L2010.png)
    
        We need to activate the SAML option on Salesforce in order to be able to integrate with IDCS via these open standards.  Also, the SAML metadata needs to be exchanged between IDCS and Salesforce
        
16.  IDCSメタデータをインポートするには、_「New from Metadata File」_ボタンをクリックします。_「ファイルの選択」_ボタンを使用して、ステップ1からダウンロードしたメタデータ・ファイルを選択します。_「作成」_をクリックします。
    
    ![イメージ](images/L2011.png)
    
        IDCS metadata file contains information about IDCS endpoints for SSO, single logout, Entity ID, Issuer details, IDCS signing certificate etc which are needed by Salesforce in order to establish a federated trust with IDCS.
        
17.  すべてのデフォルト情報を保持し、「_Save_」をクリックします。
    
    ![イメージ](images/L2012.png)
    
    この例の詳細は次のとおりです。URLとドメイン情報は異なることに注意してください。
    
    ![イメージ](images/L2013.png)
    
18.  次のことに注意してください。
    

*   ドメイン名の値(prodsys-dev-edなど)
    
*   (オプション)組織ID値。例: 00D4J000000DND9
    
    ![イメージ](images/L2014.png)
    

ノート: 「ログインURL」フィールドに組織ID値が表示されない場合は、次のステップを参照してください。

19.  組織IDがログインURLに表示されない場合は、_「会社プロファイル」_をクリックし、_「会社情報」_をクリックして_「組織ID」_を取得します。
    
    ![イメージ](images/L2015.png)
    

Salesforceは正常に構成されました。次のステップでは、IDCSを構成します。

## タスク2: Salesforce IDCSアプリケーションの作成

*   _ペルソナ_:
    *   管理者

1.  管理者としてIDCS管理コンソールにログインします:
    
        https://<your tenant>/ui/v1/adminconsole
        
    
    ![イメージ](images/L2016.png)
    
2.  ログイン後に表示されるIDCSダッシュボードから「アプリケーション」タブを選択します。
    
    ![イメージ](images/L2017.png)
    
3.  新規アプリケーションを作成するには、_「追加」_ボタンをクリックします。
    
    ![イメージ](images/L2018.png)
    
4.  _「アプリケーション・カタログ」_を選択します。
    
    ![イメージ](images/L2019.png)
    
        Note that IDCS provides an application template for Salesforce.
        The App Catalog is a collection of partially configured application templates that Oracle creates and maintains for you. You can use the templates to define the application, configure Single Sign-On, and enable and configure provisioning and synchronization. The App Catalog templates allow you to onboard applications quickly and securely.
        
        App templates simplify adding a new application by prepopulating all common attributes and values so that you just have to enter your tenant specific details.
        
5.  _アプリケーション・カタログ_内で、_Salesforce_を検索します。_「追加」_をクリックします。
    
    ![イメージ](images/L2020.png)
    
6.  構成画面の最初のページで、Salesforce内からノートにとった_「ドメイン名」_の値を指定します。オプションで、_「組織ID」_を追加できます。_「次へ」_をクリックします。
    
7.  「_SSO Configuration_」画面が表示されたら、「_Next_」を再度クリックします。
    
    ![イメージ](images/L2021.png)
    
8.  _「完了」_をクリックします
    
    ![イメージ](images/L2022.png)
    
9.  アプリケーションのアクティブ化
    

    ![Image](images/L2023.png)
    

## タスク3: グループへのアプリケーションの割当て

*   _ペルソナ_:
    *   管理者

IDCSでは、アクセス権をユーザーに直接割り当てるか、特定のアプリケーションに直接割り当てるか、または専用グループを使用して間接的に割り当てることができます。ユース・ケースでは、グループを使用してSalesforceアプリケーションへのアクセス権をユーザーに付与します。

1.  ナビゲーション・ドロワーをクリックし、_「グループ」_を選択します。
    
2.  _「従業員」_というラベルのグループを追加します。「_User can request access_」ボックスにチェックマークを付けます。
    
    ![イメージ](images/L2024.png)
    
3.  _「完了」_をクリックします
    
4.  _アクセス_・タブに移動します。_「割当て」_をクリックします。
    
5.  _Salesforce_アプリケーションに沿って_「割当て」_ボタンを選択し、_「OK」_をクリックします。
    
    ![イメージ](images/L2025.png)
    
        Note: you need to add a corresponding account in Salesforce for Federation to work correctly.  How accounts may be created in production are out of scope for this workshop but can be accomplished in a number of methods.  
        
6.  IDCSのアカウントに対応するアカウントを作成するには、[Salesforce開発者アカウント](https://developer.salesforce.com/)にログインします。
    
    ![イメージ](images/L2026.png)
    
7.  Salesforce Classicでない場合は、次の印刷画面に従ってプロファイル・メニューをクリックし、オプション_「Salesforce Classicに切替え」_を選択します。
    
    ![イメージ](images/L2027.png)
    
8.  プロファイルの近くにある上部タブ・メニューの_「設定」_に移動します。
    
    ![イメージ](images/L2028.png)
    
9.  左側のパネルで、_「ユーザーの管理」_、_「ユーザー」_、_「新規ユーザー」_の順にクリックします。
    
    ![イメージ](images/L2029.png)
    
10.  次に示すようにユーザーを作成します。電子メール・アドレスは必ずログインとして使用し、IDCSのアカウントと一致するものを使用してください。次のパラメータを設定します。
    
    | パラメータ | 値 |
    | --- | --- |
    | ロール | 例: マーケティング・チーム |
    | ユーザー・ライセンス | Salesforceプラットフォーム |
    | プロファイル | Standardプラットフォーム・ユーザー |
    
    ![イメージ](images/L2030.png)
    

これで、アカウントがSalesforceで使用可能になり、IDCSにはテストする準備ができた認証済アカウントがあります。

## タスク4: グループ・アクセスの要求

*   _ペルソナ_:
    *   エンド・ユーザー

_Employee_グループをIDCSに作成しました。このIDCSにはSalesforceアプリケーションへのアクセス権がありますが、まだユーザーが割り当てられていません。グループの作成時にオプション_「User can request access」_を選択したため、グループへのアクセスをリクエストし、Salesforceアプリケーションに暗黙的にアクセスできるようになるはずです。

1.  ブラウザを閉じてキャッシュをクリアし、IDCSコンソールにログインします。
    
        https://<yourtenant>/ui/v1/myconsole
        
    
        Please use the same user credentials that you have entered in the previous section in order to create a user account in Salesforce. If you have followed the conventions recommended by this guide this should be in the form of:     <username>+ui@<email_provider.com>
        
2.  _「自分のアプリケーション」_ページで、_「+追加」_アクセス・リクエスト・ボタンをクリックします。
    
    ![イメージ](images/L2031.png)
    
3.  _「グループ」_タブで、_「従業員」_グループを選択し、_+_記号を選択します。
    
    ![イメージ](images/L2032.png)
    
4.  結果のポップアップ・ページで_理由_を指定します。「_OK_」をクリックします。これは自動承認されたリクエストであり、管理者の介入を必要とせずにアクセス権をすぐに付与する必要があります。
    
    ![イメージ](images/L2033.png)
    
5.  右上にあるメニューから_「マイ・アクセス」_セクションに移動します。
    
    ![イメージ](images/L2034.png)
    
6.  Salesforceアプリケーションが_「自分のアプリケーション」_ページに表示されるようになったことを確認します。
    
    ![イメージ](images/L2035.png)
    

## タスク5: SSO構成の検証

*   _ペルソナ_:
    *   エンド・ユーザー

1.  IDCSコンソールで、_「自分のアプリケーション」_ページから_「Salesforce Chatter」_アプリケーションをクリックします。
    
    ノート: ユーザーがSalesforceへのアクセス権を付与するグループに割り当てられている間にSalesforceアプリケーションが表示されない場合は、最初にIDCSおよびSalesforceのユーザーとの同期を構成する必要があります。ステップ6では、IDCSとSalesforceの間でユーザーを同期する方法について説明します。
    
    ![イメージ](images/L2036.png)
    
2.  ユーザーがSalesforce Chatter (SSO)に自動的にログインしていることを確認します。
    
    ![イメージ](images/L2037.png)
    

Salesforceにログインしなくても、IDCS内で開始したのと同じユーザー・プロファイル情報がSalesforce内で表示されるようになりました。

    When you submit your IDCS user login credentials, in the background, IDCS prepares a SAML assertion and redirects the user’s browser to the Salesforce SaaS application.
    
    Salesforce consumes the SAML assertion and maps the user in its local identity store, based upon the federation agreements which had previously been configured.
    

## タスク6: プロビジョニングおよび同期の構成

*   _ペルソナ_:
    *   管理者

### Salesforceからのホスト名、組織IDおよびドメイン名の取得

Oracle Identity Cloud ServiceでSalesforceアプリケーションを構成するには、ホスト名、組織IDおよびドメイン名が必要です。これらの値は、Salesforceから取得します。

1.  ホームページの左側のナビゲーション・メニューで、_「シングル・サインオン設定」_を検索してクリックします。「シングル・サインオン設定」ページが唯一の有効なリンクとして表示されます。
    
    ![イメージ](images/L2038.png)
    
2.  _「シングル・サインオン設定」_セクションで、_「シングル・サインオン設定」_ページでアイデンティティ・プロバイダに指定した名前をクリックします。
    
3.  「Entity ID」フィールドに指定された値のホスト名をメモし、対応するインスタンス名をクリックします。
    
    ![イメージ](images/L2039.png)
    
        Note: Use this host name value while enabling user provisioning for the Salesforce app in Oracle Identity Cloud Service in the "Enabling Provisioning" section, and while verifying the SSO initiated from Salesforce in the "Verifying Service Provider Initiated SSO from Salesforce" section.
        
4.  _「Endpoints」_セクションで、SalesforceログインURLのドメイン名および組織IDをノートにとります。
    
        https://<Domain_Name>.my.salesforce.com?so=<Organization_ID>
        or
        https://<Domain_Name>.my.salesforce.com
        
    
    ドメイン名はURLの先頭、組織IDは末尾です。
    
    ![イメージ](images/L2040.png)
    

### Salesforceからのコンシューマ・キーおよびコンシューマ・シークレットの取得

1.  Salesforceで_「Lightning Experience」_に切り替えます。
    
    ![イメージ](images/L2041.png)
    
2.  ページの右上隅の歯車をクリックし、_「Setup」_を選択します。
    
    ![イメージ](images/L2042.png)
    
3.  ホームページの左側のナビゲーションメニューで、「App Manager」を検索してクリックします。Lightning Experienceの_「App Manager」_ページが表示されます。
    
    ![イメージ](images/L2043.png)
    
4.  ページ右隅で、_「New Connected App」_をクリックします。「New Connected App」ページが表示されます。
    
    ![イメージ](images/L2044.png)
    
5.  「Basic Information」セクションで、接続するアプリケーションの_「Connected App Name」_を入力します。
    
6.  管理者の_「Contact Email」_を入力します。
    
7.  「API (有効化OAuth設定)」セクションで、_「有効化OAuth設定」_チェック・ボックスを選択します。
    
8.  _「Callback URL」_フィールドに、認可サーバーからアクセス・トークンを受信するパブリック・ドメインURLを入力します。たとえば、https://login.salesforce.com.です。
    
9.  _「選択したOAuthスコープ」_フィールドで、_「使用可能なOAuthスコープ」_リストの下の_「フル・アクセス(フル)」_を選択し、_「追加」_をクリックしてOAuthを変更するためのフル・アクセス権を付与します。
    
    ![イメージ](images/L2045.png)
    
10.  下へスクロールし、_保存_をクリックします。
    
11.  接続アプリケーションを使用する前にサーバーで変更が有効になるまで2から10分待機し、_「Continue」_をクリックします。
    
12.  ほとんどの場合、新しく作成されたアプリケーション・ページが自動的に表示されます。表示されない場合は、次の手順に従ってください。ホーム・ページの左側のナビゲーション・メニューで、_「アプリケーション・マネージャ」_を検索してクリックします。新しく作成したアプリケーションを検索し、右側にある小さな矢印をクリックして_「表示」_を選択します。
    
    ![イメージ](images/L2046.png)
    
13.  「API (有効化OAuth設定)」セクションで、_「コンシューマ・シークレット」_フィールドの横にある_「クリックして公開」_をクリックします。
    
    ![イメージ](images/L2047.png)
    
14.  _「コンシューマ・キー」_および_「コンシューマ・シークレット」_の値をメモします。
    

### Salesforceからの管理者パスワードの導出

Salesforceアプリケーションのプロビジョニングおよび同期を有効にする前に、**セキュリティ・トークンを追加する必要があります**。トークン値はSalesforceから取得します。

最終値は次のようになります。

    <yourSalesforceAdminPassword + securityToken(ex: LKdMzECdjFKYSj028WJhU1GG)>
    

1.  Salesforceホームページの右上隅にあるユーザー・アイコンをクリックして、ドロップダウン・リストから「Settings」をクリックします。
    
    ![イメージ](images/L2048.png)  
     
    
2.  左側のナビゲーション・メニューで、_「自分のセキュリティ・トークンのリセット」_を検索してクリックします。
    
    ![イメージ](images/L2049.png)
    
3.  _「セキュリティ・トークンのリセット」_ページで、_「セキュリティ・トークンのリセット」_をクリックします。セキュリティ・トークンが管理者の電子メール・アドレスに送信されます。
    
4.  セキュリティ・トークンをメモし、セキュリティ・トークンを管理者パスワードに追加します。
    
        <yourSalesforceAdminPassword + securityToken(ex: LKdMzECdWJhU1GG)>
        
    
        NOTE: the <>, + and example must not be included
        

### SalesforceプロファイルIDの取得

Oracle Identity Cloud Serviceで作成したユーザーは、Salesforceプロファイルに割り当てる必要があります。Salesforceアプリケーションのユーザー・プロビジョニングおよび同期を有効にするには、このSalesforceプロファイルのプロファイルID値をSalesforceから取得する必要があります。

1.  _「Salesforce Classicに切替え」_をクリックします
    
    ![イメージ](images/L2050.png)
    
2.  _「設定」_をクリックします。
    
    ![イメージ](images/L2051.png)
    
3.  _「管理」_に移動し、_「ユーザーおよびプロファイルの管理」_を選択します。
    
    ![イメージ](images/L2052.png)
    
4.  2番目のページで、_「Standard Platform User」_をクリックします。
    
    ![イメージ](images/L2053.png)
    

  5. プロファイルのURLを書き留めます。_「プロファイルID」_は、URLの最後にあるIDです。

    ![Image](images/L2054.png)
    

6.  後で使用するため、値をノートにとっておきます。

### Salesforceのプロビジョニングおよび同期の有効化

プロビジョニングでは、次の使用可能な操作が提供されます。

*   _アカウントの作成_: Oracle Identity Cloud Serviceの対応するユーザーにSalesforceアクセス権が付与されると、Salesforceアカウントが自動的に作成されます。
*   _アカウントの非アクティブ化_: Oracle Identity Cloud Serviceの対応するユーザーに対してSalesforceアクセス権が非アクティブ化またはアクティブ化されると、Salesforceアカウントが自動的に非アクティブ化またはアクティブ化されます。
*   _アカウントの削除_: Oracle Identity Cloud Serviceの対応するユーザーからSalesforceアクセス権が取り消されると、Salesforceからアカウントが自動的に削除されます。

この他に、Salesforceで定義されたユーザー・アカウント・フィールドと、Oracle Identity Cloud Serviceで定義された対応するフィールド間で属性をマップできます。

1.  IDCS管理コンソールに移動し、前に作成したSalesforceアプリケーションを選択します。
    
2.  _「プロビジョニング」_ページで、_「プロビジョニングの有効化」_を選択します。
    
3.  ウィンドウがポップアップします。_「承諾の付与」_をクリックします
    
4.  次のパラメータに、Salesforceから前のステップで導出した値を入力します。
    
    ![イメージ](images/L2055.png)
    
    | パラメータ | 値 |
    | --- | --- |
    | ホスト名 | 「Obtaining Host Name、 Organization ID、 and Domain Name from Salesforce」の項のステップを実行して取得したSalesforceホスト名を入力します。 |
    | 管理者ユーザー名 | 管理者アカウント・ユーザー名を入力します。 |
    | 管理者パスワード | 「Salesforceからの管理者パスワードの導出」の項のステップを実行して導出した更新済のパスワードを入力します。 |
    | クライアントID | 「Salesforceからのコンシューマ・キーおよびコンシューマ・シークレットの取得」のステップを実行して取得したSalesforceコンシューマ・キーを入力します。 |
    | クライアント・シークレット | 「Salesforceからのコンシューマ・キーおよびコンシューマ・シークレットの取得」のステップを実行して取得したSalesforceコンシューマ・シークレットを入力します。 |
    
5.  _「接続のテスト」_をクリックして、Salesforceとの接続を検証します。Oracle Identity Cloud Serviceに確認メッセージが表示されます。
    
6.  _「属性マッピング」_をクリックします。
    
    ![イメージ](images/L2056.png)
    
7.  _「プロファイルID」_を前述の値に設定します。
    
    ![イメージ](images/L2057.png)
    

  8. _「プロビジョニング」_ページを下にスクロールし、_「同期の有効化」_を選択します。

    ![Image](images/L2058.png)
    

このオプションは、Salesforceの既存のアカウント詳細を同期し、対応するOracle Identity Cloud Serviceユーザーにリンクします。  

### IDCSでの同期のテスト

1.  アプリケーション・ページで、_「インポート」_を選択します。
    
2.  _「インポート」_をクリックし、しばらく待ちます。
    
    ![イメージ](images/L2059.png)
    
3.  _「リフレッシュ」_をクリックします
    
    ![イメージ](images/L2060.png)
    
4.  IDCSからインポートされたユーザーが表示されます。
    
    ![イメージ](images/L2061.png)
    

  5. 最後のステップは、IDCSとSalesforceの間でリンクするユーザーを確認することです。「同期化」ステータスで、_「確認」_をクリックします。IDCSとSalesforceのユーザー間のリンクが成功すると、同期ステータスが_「確認済」_になります。

    ![Image](images/L2062.png)
    

    Every user that has status Confirmed in the Synchronization Status has access to Salesforce in My Apps. Also, the application Salesforce will be visible to all users in My Apps that have status Confirmed.
    

次の演習に進むことができます。

 

## 謝辞

*   **作成者** - SEHubセキュリティおよび管理チーム
*   **最終更新者/日付** - プリンシパル・ソリューション・エンジニア、Lucian Ionescu、15.09.2020