# 概要

**SecureOracle 8.0**ワークショップの演習では、**Oracle IAM Suite 12c R2 PS4 (12.2.1.4.0)**の使用を開始するための機能の一部について説明します。従業員のライフサイクル管理、アイデンティティ認証、アイデンティティ・ガバナンスのためのRESTful API、OAuthクライアントおよびトークン管理を実践します。

Oracle IAM Suiteは、ユーザー・ライフサイクルを管理し、ファイアウォール内およびファイアウォールを越えて、およびクラウドに、エンタープライズ・リソース間でセキュアなアクセスを提供するために設計された、統一された統合セキュリティ・プラットフォームを提供します。

_推定ワークショップ時間_: 4時間

### 目標

このワークショップの終わりに、次のことをよく理解します。

*   SecureOracleデモンストレーション環境
*   従業員ライフサイクル管理
*   アイデンティティの証明
*   アイデンティティ・ガバナンス、OAuthクライアントおよびトークン管理用のRESTful API

### 前提条件

このワークショップに参加するには、次が必要です:

*   Oracle Free Tier、Always Free、有料またはLiveLabsクラウド・アカウント

## SecureOracle 8.0について

SecureOracle 8.0は、次のOracleコンポーネントを含むOracle IAM Suite 12c R2 PS4 (12.2.1.4.0)のデモ環境です。

*   アイデンティティ・ガバナンス:
    *   OIG 12c、SOA Suite 12c、BI Publisher 12c、OUD 12c、DB 19cおよび12cコネクタ
*   アクセス管理:
    *   OAM 12c、OHS/WebGate 12c、OUD 12cおよびDB 19c
*   開発ツールおよびアセット:
    *   [JDeveloper 12cとSOA拡張](http://www.oracle.com/technetwork/middleware/soasuite/downloads/index.html)
    *   [SQL Developer 19.2.1](https://www.oracle.com/database/technologies/appdev/sql-developer.html)
    *   [Apache Studio 2](https://directory.apache.org/studio/)
    *   [Oracle APEX 19.2](https://apex.oracle.com/en/)
    *   私のHRおよび私のIGAアプリケーションおよびデモ・シナリオのサンプル

**ノート:** OIMとOIGは交換可能な用語であり、同じ製品であるOracle Identity ManagerまたはOracle Identity Governanceを参照します。

SecureOracleをOracle Identity Cloud Service (IDCS)とともに使用して、ハイブリッドなアイデンティティ管理環境を紹介できます。IDCSと統合されている場合、OIGコンポーネントはプロビジョニングおよびガバナンス・サービスを提供し、IDCSはクラウドおよびオンプレミス・アプリケーションへのアクセス管理サービスを提供します。

Oracle IAM Suite 12c R2 PS4は、柔軟性があり、本番環境の出発点として使用できるOracle IAM標準インストール・トポロジを使用してデプロイできます。_図1_は、管理サーバーと1つ以上の管理対象サーバーを含む1つ以上のクラスタを含む標準のWebLogicサーバー・ドメインを示しています。

![](./images/idm12cps4-standard-topology2.png " ")

_図1_。Oracle Identity and Access ManagementのStandardトポロジ

_図2_は、SecureOracle環境を構成するドメインを示しています。OIGコンポーネントとOAMコンポーネントは個別に起動することも、すべて同時に起動することもできます。さらに構成すると、オンラインで使用可能な公式ドキュメント[OIGとOAMの統合](https://docs.oracle.com/en/middleware/idm/suite/12.2.1.4/integrate.html)に従って統合できます。

![](./images/img-sodomains.png " ")

_図2_。SecureOracle - IAM Suite 12c R2 PS4のデモ・プラットフォーム

デフォルトでは、すべての非SSLポートが様々な演習の実行に使用されますが、特定のデモ要件を満たすために必要に応じてSSLポートを有効にできます。SSLを有効にする方法の詳細については、公式製品のドキュメントを参照してください。

### 演習の概要

*   **ラボ: SecureOracle 8.0スタート・ガイド** - このラボでは、SecureOracle 8.0デモンストレーション・プラットフォームの新機能を確認し、様々なコンポーネントの起動、開発ツールの実行、様々な管理コンソールおよびデモ・アプリケーションへのアクセス方法を学習します。
    
*   **ラボ: 従業員ライフサイクル管理** - このラボでは、Oracle Identity ManagerでMy HR Applicationを認可ソースとして使用して、従業員のオンボーディング、ユーザー・ライフサイクル、異動、マネージャ承認および従業員の退職に関連するいくつかのユースケースを実行します。
    
*   **ラボ: アイデンティティの証明** - アイデンティティの証明は、企業内のユーザー権限とアクセス権限をレビューして、ユーザーが所有を許可されていない資格を取得していないことを確認するプロセスです。また、各アクセス権限の承認(認証)または拒否(取消し)も含まれます。このラボでは、カスタム・レビューアによるユーザー証明をレビューします。
    
*   **ラボ: RESTful OIMおよびOAM API** - この演習では、OIMセルフ・サービス、ユーザー・プロファイル更新、リクエスト、承認およびOAM OAuthサービスのREST APIの起動に関連するいくつかのユースケースを確認します。
    

## バージョン8.0の新機能

SecureOracleのバージョン8.0には、次の機能が含まれています。

*   Oracle IAM Suite 12c R2 PS4 (12.2.1.4.0)の新規インストール
    
*   Oracle Access Managementには、次のサービスが含まれます。
    
    *   Adaptive Authentication Service
    *   OAuthおよびOpenIDConnectサービス
    *   アイデンティティ・フェデレーション
    *   アクセス・ポータル・サービス
*   デフォルトでは、OAMとWebGate間の通信はRESTを介したOAPです。
    
*   [HTTPie](https://httpie.org/) RESTful API、WebサービスおよびHTTPサーバーと対話するオープン・ソースのコマンドラインHTTPクライアント。
    
*   Oracle Identity Governanceには、次の12cコネクタが含まれます:
    
    *   Box、Concur、DBアプリケーション表、Oracle/MySQL DBユーザー管理、フラット・ファイル、Google Apps、GoToMeeting、IDCS、Microsoft ADユーザー管理、Office365、EBS従業員リコンシリエーション、EBSユーザー管理、OID/ODSEE/OUD/LDAP、Salesforce、SAP成功要因、SAPユーザー管理、SAPユーザー管理エンジン、ServiceNow、Unix、WebEx
*   [Cockpit](https://cockpit-project.org/)は、使いやすいWebコンソールを使用してサーバーを管理するためのオープン・ソース・ツールです。CockpitにはSecureOracleがプリインストールされており、SSHクライアントを使用せずにすべてのコンポーネントを起動できます。
    
    ![](./images/img-cockpit.png " ")
    
    図1.Cockpit Webコンソール
    
*   Oracle Unified Directoryには、次のサービスが含まれています。
    
    *   アイデンティティ情報へのアクセスおよびディレクトリ・データの管理のためのREST API
*   事前構成済コネクタ
    
    *   IDCSのプロビジョニングおよびガバナンスと統合するための12c IDCSコネクタ
    *   OUDとの統合のための12c ODSEE/OUD/LDAPコネクタ
    *   My HRアプリケーションと統合するための12c DBATコネクタ
*   次のようなSecureOracleドキュメントが更新されました。
    
    *   スタート・ガイド
    *   OCIへのSecureOracleのデプロイ
    *   VMware-VirtualBoxへのSecureOracleのデプロイ
    *   従業員ライフサイクル管理
    *   RESTful OIMおよびOAM API
    *   OAMハイブリッド・シナリオ
    *   アイデンティティの証明
    *   Androidエミュレータ NoxPlayerガイド
*   デモのユース・ケースをサポートするオープン・ソースの電子メール・サーバーとクライアント
    
    *   Webメール・クライアント: HTMLコンテンツをサポートする[Roundcube 1.4.1](https://roundcube.net/ "ラウンドキューブ")
    *   電子メール・サーバー: 電子メール・アカウントを維持するための単純なWebコンソールを使用した[Hedwigメール・サーバー](http://hwmail.sourceforge.net/ "ヘドウィグ")
*   SecureOracleイメージは、次の場所にデプロイできます。
    
    *   OCIコンピュート
    *   VirtualBoxまたはVMware
*   APEX 19.2へのデモンストレーション・アセットの更新
    
    *   私のHRアプリケーションは、完全にAPEXで開発され、Oracle Databaseで実行されているため、HR従業員のライフサイクル管理とOIGとの統合のデモに役立ちます。最新のブラウザをサポートする完全なHTML5インタフェース。
    *   私のIGAアプリケーション- また、APEXで開発され、OIG REST APIやカスタム・レビューアなどの認定を使用してガバナンス機能を紹介することを目的としています。最新のブラウザをサポートする完全なHTML5インタフェース。
    
    ![](./images/img-myhr-app-menu.png " ")
    
    図2.自分のHRアプリケーション
    
    ![](./images/img-iga-app-dash.png " ")
    
    図3.IGAアプリケーション
    
*   IDCSとの統合に関するハイブリッド・ユース・ケースのサポート:
    
    *   IDCSによるプロビジョニング用のOIG 12cコネクタ
    *   IDCSとの統合のためにクラウド・モードで構成されたOAM Webgate

## さらに学ぶ

Oracle Identity and Access Managementの詳細は、次のリンクを参照してください。

*   [Oracle Identity Management Webサイト](https://docs.oracle.com/en/middleware/idm/suite/12.2.1.4/index.html)
*   [Oracle Identity Governanceのドキュメント](https://docs.oracle.com/en/middleware/idm/identity-governance/12.2.1.4/index.html)
*   [Oracle Access Managementドキュメント](https://docs.oracle.com/en/middleware/idm/access-manager/12.2.1.4/books.html)

## 確認

*   **著者** - Ricardo Gutierrez氏、ソリューション・エンジニアリング- セキュリティおよび管理
*   **貢献者** - Meghana Banka、Rene Fontcha
*   **最終更新者/日付** - Rene Fontcha、LiveLabs Platform Lead、NA Technology、2020年11月