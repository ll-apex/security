# 概要

## このワークショップについて

ワークショップの演習では、Oracle Identity Cloud Service (IDCS)の最新バージョンをデモンストレーションします。

Identity Cloud Serviceは、Oracleの次世代の包括的なセキュリティおよびアイデンティティ・プラットフォームで、クラウド・プラットフォーム「as a service」を介してすべてのコアなアイデンティティ管理およびアクセス管理機能を提供する、完全に統合された革新的なサービスです。Identity Cloud Service (IDCS)の設計は、スケーラビリティ、柔軟性、自己回復性、デプロイメントの容易性、機能アジリティ、技術採用および組織連携のクラウドの原則に自然に合せたマイクロ・サービス・アーキテクチャに基づいています。

Oracle Identity Cloud Serviceには、大まかに次の機能があります。

*   アイデンティティおよびアクセス管理
*   オンプレミスのActive Directoryまたはサード・パーティのアイデンティティ・システムとの統合
*   シングル・サインオン(SSO)
*   ユーザー認証サービス
*   Identity Federationサービス(SAML)
*   OAuthサービス
*   監査およびレポート・サービス

推定ラボ時間: 5時間

## IDCSアーキテクチャ

Oracle Identity Cloud Serviceは、OracleのPaaSサービスの幅広いポートフォリオの一部として利用可能な新しいプラットフォーム・サービスです。IDCSは一連のマイクロサービスとして実装されており、設計方針は、SCIM、OAUTH、SAML、OpenID Connectなどの最新標準をサポートするAPIファーストです。

次の図は、一連のサービス・レイヤーとして編成されたIDCSアーキテクチャを示しています。

![イメージ](images/W001.png)

## ラボ構成および詳細

このワークショップは、Oracle Public Cloud (OPC)で主催されます。これには、クラウド・サービスの組合せ、ホストされたオンプレミス・ソフトウェアおよびサード・パーティ・ソフトウェアが含まれます。Identity Cloud Service以外に、アイデンティティ同期、フェデレーション、認証、SSOなど、統合のデモンストレーションを可能にする残りのコンポーネントが含まれています。

次に、ワークショップのコンポーネントの概要を示します。

*   Oracle Cloud Infrastructure(OCI)
*   Oracle Identity Cloud Service (IDCS)

サードパーティのSaaSアプリケーション

*   Salesforce - IDCSアプリケーション統合に使用
*   Google - IDCSフェデレーションに使用
*   Okta - IDCSフェデレーションに使用

サードパーティ・コンポーネント

*   Postman - IDCS REST APIユーザー管理に使用

## 概念と用語

この項では、ワークショップ全体で使用されるいくつかの概念および用語について簡単に説明します。

*   Oracle Public Cloud (OPC) - OPCは、Oracleのクラウド・ポートフォリオ全体を示します。これは、業界で最も幅広く完全なSaaS、PaaSおよびIaaSクラウド・サービスのポートフォリオで構成されています。OPCサービスは、数十のグローバル・データ・センターを介して世界中に存在します。
    
*   SaaS / PaaS / IaaS - Oracleクラウド・サービスの3つのサービス・カテゴリです。SaaSはSoftware-as-a-Serviceの略で、一般的にはOracle Customer Experience (CX)、Salesforce.com、WorkDay、Office365などのクラウド・サービスを表す場合があります。PaaSはPlatform-as-a-Serviceの略で、Java、データベース、統合、アイデンティティなどのミドルウェア・サービスを指します。IaaSはInfrastructure-as-a-Serviceの略で、コンピュート、ネットワーク、ストレージなどのサービスを表します。IaaSのトップ・プロバイダには、Microsoft (Azure)、Amazon (AWS)およびOracleがあります。
    
*   テナント- テナントはクラウド・サービスのサブスクリプションを表します。Oracleのクラウド・サービスの多く(Identity Cloud Serviceを含む)はマルチテナントです。つまり、複数の顧客(部門、会社、組織、代理店など)がOPCで稼働する共通のクラウド・サービスに登録され、サービスを受けることになります。マルチテナントのシナリオでは、各テナントに独自のデータ、構成設定、ユーザーおよびその他のサービス関連のアーティファクトがあります。
    
*   OAUTH 2.0 - OAUTHは認可を委任するための標準プロトコルです。これは、エンティティがリモート・プロバイダに格納されているリソース(サービス、API、データ)にアクセスする権限を持つ方法です。たとえば、オンプレミス・アプリケーションでは、IDCS REST APIを使用して、アプリケーション内で使用するためにIDCSからユーザーまたはグループに関する情報を取得できます。REST APIは、クライアント・アプリケーションがREST APIの特定のスコープに登録および認可されるように、OAUTHサービスによって保護されます。したがって、OAUTHは、インターネットから見えるサービス(IDCS REST APIなど)が認可されていないユーザーまたはアプリケーションによって消費されないようにします。
    
*   SAML 2.0 - SAMLはセキュリティ・アサーション・マークアップ言語を表します。これは、ユーザー認証をフェデレートするための標準です。SAMLは、サービス・プロバイダとアイデンティティ・プロバイダの2つの関連エンティティを定義します。ユーザーがSAML用に構成されたアプリケーションまたはサービス(サービス・プロバイダ)にアクセスしようとすると、ローカルID/PWを介してユーザーをログインするのではなく、SPによってユーザー認証が実行されます。したがって、SPとIDPの間に信頼関係が確立されます。Oracle IDCSは、サービス・プロバイダまたはアイデンティティ・プロバイダのロール(あるいはその両方)を取るように構成できます。サービス・プロバイダとして、IDCSはセルフサービス・プロファイル管理やパスワードのリセットなどを可能にします。
    
*   IDプロバイダ- このタイプのプロバイダはIDアサーション・プロバイダとも呼ばれ、Oracle Identity Cloud Serviceの外部にあるWebサイトを使用してOracle Identity Cloud Serviceと対話するユーザーの識別子を提供します。
    

## Oracleソリューションの利点

この項では、Oracle Identity Cloud Serviceソリューションの経済的、ビジネス的および技術的なメリットについて簡単に説明します。

*   **オープンかつ標準ベース** - 100%オープンで標準ベースのソリューションを使用して、クラウドとオンプレミスのアプリケーションを迅速に統合します。サポートされている標準の例には、System for Cross-domain Identity Management (SCIM)、Open Authorization (OAUTH 2.0)、Security Assertion Markup Language (SAML 2.0)、Representational State Transfer (REST)、OpenID Connectなどがあります。
    
*   **安全な多層防御** - Oracle Public Cloud (OPC)サービスとしてホスティングされ、オンプレミスのエンタープライズ機能と統合されたOracleのIdentity Cloud Serviceによって、防御レイヤーを利用します。
    
*   **ハイブリッド・アイデンティティ** - エンタープライズグレードのハイブリッド・デプロイメントを使用して、クラウドとオンプレミスの両方のアプリケーションのユーザー・アイデンティティを管理します。オンプレミス環境とIDCS環境間でデータを統合および交換するには、いくつかのオプションがあります。
    
*   **クラウド・アイデンティティにおけるOracleのマーケット・リーダーシップ** - Oracle Identity Cloud Serviceは、Oracleのクラウド・アイデンティティ市場への第一歩ではありません。過去4年間、様々なOracle Public Cloud (OPC)サービスに対して、クラウド・アイデンティティ・サービスを提供してきました。1日3,000万件を超えるログインで、35,000件の顧客のニーズを大規模に処理するアイデンティティ・サービスを検討すると、IDCSを導入する前に、Oracleがクラウド・アイデンティティ・マーケットのリーダーであることがわかります。
    
*   **最新のアーキテクチャ** - 一部のベンダーのクラウド・サービスに関する考え方は、単にオンプレミス・アプリケーションを「クラウド」データ・センターにデプロイすることですが、Oracleはソリューションを完全に一から書き直しました。このアーキテクチャはAPIファーストであり、オープン・スタンダードを活用したマイクロサービス・アーキテクチャ上に構築されています。アイデンティティ・サービスに真のプラットフォームを提供します。
    
*   **サービス範囲** - 一部の第1世代のベンダーは、アイデンティティ・クラウド・サービス(SaaSアプリケーションへのSSOなど)の提供に関するニッチな問題を解決しました。しかし、現実には、あらゆる規模の企業がクラウドに最高の統合ソリューションを導入したくないということになります。大多数は、エンタープライズ・グレードのニーズを解決する適切なソリューションを備えたサービス・パートナを選択します。オンプレミスとクラウドの両方のアイデンティティ・インフラストラクチャをサポートし、ペースと時間枠でクラウドへの移行を可能にするビジネス・パートナーを見つけることも同様に不可欠です。このベンダーはOracleです。
    

## 目標

このワークショップの終わりには、

*   ユーザーとグループの管理
*   カタログの使用とSSO構成
*   SAMLベースおよびソーシャル・フェデレーション
*   条件付き適応型MFA

## 前提条件

次に、ワークショップの外部コンポーネントの概要を示します。

1.  Googleアカウント
2.  Salesforce開発者アカウント
3.  Oktaインテグレータ・アカウント
4.  Postmanネイティブ・アプリケーション

### Gmailアカウントの作成

Gmailにサインアップするには、Googleアカウントを作成します

1.  [Googleアカウント作成ページ](https://accounts.google.com/SignUp)に移動します
2.  画面のステップに従ってアカウントを設定します。
3.  作成したアカウントを使用して、Gmailにサインインします。

### Salesforce開発者アカウントの作成

Salesforceは無料の開発者版を提供しています。salesforce.comに無料の開発者アカウントを作成するステップ

1.  ブラウザを開き、[Salesforce開発者アカウント](https://developer.salesforce.com/signup)のサインアップ・ページにアクセスします
    
2.  サインアップ・フォームに入力します。
    
    *   _Eメール_の名と姓を入力し、Eメール・アドレスを入力します(アクティブ化Eメールを開く必要があります)。ノート: ステップ1で作成したGmailアカウントを使用します。
    *   _「ユーザー名」_には、電子メール・アドレスの形式で一意のユーザー名を指定します
    *   「基本契約」チェック・ボックスを選択し、_「サイン・アップ」_ボタンをクリックします ![イメージ](images/PR01.png)
3.  メールを確認してください。Developer Editionアカウントのアクティブ化電子メールが届きます。
    
4.  アクティブ化電子メールのリンクをクリックします。新しいパスワード情報を入力し、「_Save_」をクリックします。
    

#### Salesforceへのカスタム・ドメインの登録

カスタム・ドメインまたはマイ・ドメインは、共通のSalesforce URLではなく独自のSalesforce組織でカスタマイズされたURLを持つ方法です。

Salesforceの一部の機能を使用するには、My Domainが必要です。このような機能には、次のようなものがあります。

*   Lightningコンポーネント・タブ、Lightningページ、Lightningアプリケーション・ビルダーまたはLightningスタンドアロン・アプリケーションに_Lightningコンポーネント_を表示します。
*   FacebookやGoogleなどの_ソーシャル・サインオンと認証プロバイダ_を統合します。
*   外部アイデンティティ・プロバイダで_シングル・サインオン(SSO)_を使用します。ワークショップのシナリオでは、これは_Oracle Identity Cloud Service_になります。ノート: 「Salesforce Classicに切替え」を選択する必要があります。

1.  Salesforceの_ホームページ_から、プロファイル(右上隅にある「プロファイルの表示」アイコン)に移動します。
    
2.  _「オプション」_メニューで、「Salesforceクラシックに切り替える」を選択します。
    
    ![イメージ](images/PR02.png)
    
3.  _「設定(クラシック)」_→_「管理」_→_「ドメイン管理」_→_「自分のドメイン」_を選択します。
    
    ![イメージ](images/PR03.png)
    
    ![イメージ](images/PR04.png)
    
4.  Salesforce汎用URL内で使用するサブドメイン名を入力します。これは、Oracle Cloudアカウントのサインアップで使用されるアカウント名(クラウド・アカウントの識別に使用)にできます
    
    ![イメージ](images/PR05.png)
    
5.  「有効数量のチェック」ボタンをクリックします(自分の名前がすでに使用されている場合は、別の名前を選択します)。「ドメインの登録」ボタンをクリックします。
    
    ![イメージ](images/PR06.png)
    
6.  Salesforceは、新しいドメイン・レジストリを更新します。完了すると、確認のメールが届きます。ドメインが使用可能になるまでに数分かかる場合があります。
    
7.  アクティブ化電子メールを受信したら、電子メールからリンクをクリックして、カスタム・ドメインURLを含む「ログイン」画面に戻ります。
    

### Oktaインテグレータ・アカウントの作成

1.  ブラウザを開き、[Okta Integratorページ](https://www.okta.com/integrate/signup/)にアクセスします
    
2.  サインアップ・フォームに入力します。
    
    *   _電子メール_・ノートを入力します: ステップ2で作成したGmailアカウントを使用します。
    *   _「名」_および_「姓」_を入力します。
    *   _会社_名を入力します。これは、Oracle Cloudアカウントのサインアップ(クラウド・アカウントの識別に使用)で使用されるアカウント名にできます

![イメージ](images/PR07.png)

3.  メールアドレスを確認してください。Oktaから確認メールが送信されます。確認電子メールの_アカウントをアクティブ化_して、受信ボックスに移動し、continue.Clickのアカウントを確認してください。
    
    ![イメージ](images/PR08.png)
    
4.  新しいパスワードを設定し、アカウントをアクティブ化します。
    

### Postmanネイティブ・アプリケーションのインストール

1.  Postmanは、macOS、WindowsおよびLinuxオペレーティング・システムのネイティブ・アプリケーションとして使用できます。

Postmanをインストールするには、[アプリケーション・ページ](https://learning.getpostman.com/docs/postman/launching_postman/installation_and_updates/)に移動し、プラットフォームに応じてmacOS / Windows / Linuxの_「ダウンロード」_をクリックします。

_macOSインストール_

アプリをダウンロードして解凍したら、Postmanをダブルクリックします。ファイルを「アプリケーション」フォルダに移動するように求められます。「アプリケーション・フォルダに移動」をクリックして、今後の更新を正しくインストールできることを確認します。プロンプトの後にアプリが開きます。

![イメージ](images/PR09.png)

_Windowsインストール_

インストーラ・ファイルをダウンロードしたら、それを実行し、画面の指示に従います。

_Linuxインストール_

Linuxでのインストールの場合は、次のステップを実行します。

1.  最初にファイルをダウンロードして解凍します
2.  次に、Postman.desktopという名前でデスクトップ・ファイルを作成します。次の場所にPostman.desktopファイルを作成します。

    ~/.local/share/applications/Postman.desktop
    

上のファイルの次の内容を使用します。

      [Desktop Entry]
      Encoding=UTF-8
      Name=Postman
      Exec=YOUR_INSTALL_DIR/Postman/app/Postman %U
      Icon=YOUR_INSTALL_DIR/Postman/app/resources/app/assets/icon.png
      Terminal=false
      Type=Application
      Categories=Development;
    

Postman.desktopファイルが作成されると、アプリケーション・ランチャを使用してPostmanアプリケーションを開くことができます。デスクトップを確認し、「Postman」アイコンをダブルクリックします。

最初の演習に進むことができます。

## 追加情報

このワークブックは主に、Oracle Identity Cloud Serviceワークショップで演習を完了するために必要な手順およびコンテキストを提供するように設計されています。Oracleソリューションに関する追加情報が必要な場合は、地域のOracleアカウント・チームに連絡するか、ソリューションに関する次の公開情報を確認できます。

*   [Identity Cloud ServiceのWebサイト](https://cloud.oracle.com/en_US/identity)
*   [ソリューション・データ・シート](http://www.oracle.com/technetwork/middleware/id-mgmt/overview/idcs-datasheet-3097388.pdf)
*   [製品ドキュメント](http://docs.oracle.com/cloud/latest/identity-cloud/index.html)
*   [ブログ](https://blogs.oracle.com/imc/)

## 確認

*   **作成者** - SEHubセキュリティおよび管理チーム
*   **最終更新者/日付** - プリンシパル・ソリューション・エンジニア、Lucian Ionescu、15.09.2020