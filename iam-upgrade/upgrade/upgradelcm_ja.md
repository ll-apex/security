# Oracle IAM 11.1.2.3から12.2.1.4インプレース・アップグレード

## 概要

この項では、11.1.2.3 LCMベースのOIM-OAM統合環境をバックエンド・ディレクトリ・サーバーとしてOUD、WebサーバーとしてOHSとともに12.2.1.4にアップグレードするステップについて説明します。

_推定ラボ時間_: 48-72時間

### Oracle IAMのアップグレード戦略について

Oracle Identity and Access Management (IAM)デプロイメントは、複数の異なるコンポーネントで構成されています:

*   データベース
*   ユーザー情報を格納するLDAPディレクトリ
*   認証用のOracle Access Manager
*   プロビジョニング用のOracle Identity Governance (正式名称Oracle Identity Manager)
*   Optionally, Oracle HTTP Server and Webgate securing access to Oracle Access Manager and Oracle Identity Governance

Oracle Internet Directory、Oracle Unified DirectoryおよびOracle Identity and Access Managementのアップグレードには、様々なアップグレード戦略を採用できます。選択する戦略は、主にビジネス・ニーズによって異なります。このラボでは、アップグレード戦略ドキュメント([Oracle IAMアップグレード戦略](https://docs.oracle.com/en/middleware/fusion-middleware/iamus/place-upgrade-strategies.html#GUID-9F906AE2-5BDF-426D-A97C-AC546ABFBD28))で概説されているマルチホップ・インプレース・アップグレードを使用します

インプレース・アップグレードでは、既存のデプロイメントを取得し、それを所定の場所でアップグレードできます。

*   アップグレードを実行する場合、アップグレードが成功するように、各ステージでできるだけ少ない変更を行う必要があります。
*   たとえば、Oracle Identity and Access Managementのアップグレード、ディレクトリの変更、オペレーティング・システムの更新など、複数のアップグレード・アクティビティを同時に実行することはできません。
*   このようなアップグレードを実行する場合は、段階的に実行する必要があります。次のステージに進む前に、各ステージを検証する必要があります。この方法の利点は、問題が発生した場所を正確に識別し、問題を修正するか元に戻してから実行を続行できることです。

### 目標

この演習では、次のことを行います。

*   Oracle IAM 11.1.2.3からOracle IAM 12.2.1.4へのインプレース・アップグレード・プロセスについて理解します

### 前提条件

この演習では、次を想定しています。

*   有料またはLiveLabs Oracle Cloudアカウント
*   次を完了しました:
    *   演習: 設定の準備(_無料層_および_有料テナント_のみ)
    *   演習: 環境設定
    *   演習: 環境の初期化
*   アップグレード前のステップの一部として、顧客はoamconsoleの10gエージェントを削除する必要があります。このUA準備状況チェックに失敗すると、10gエージェントが存在することを示す失敗します。OAMのアップグレード前のステップの一部として、oamconsoleの10gエージェントを削除します。10gエージェントが存在する場合、UA準備状況チェックは失敗します
*   「OHS\_managed\_template.jar」というエラーがあるシステム・コンポーネント・インフラストラクチャで、UA準備状況チェックが失敗した場合。OHSをアップグレードしていないため、これは無視できます

## **タスク1**: IAMコンポーネントの11.1.2.3から12.2.1.3へのアップグレード

1.  OUD 11.1.2.3からOUD 12.2.1.3へのアップグレード

*   Oracle Unified Directoryのアップグレード・ガイドの_第6.6項_で説明されているすべてのステップの実行- [6.6既存のOracle Unified Directoryサーバー・インスタンスのアップグレード](https://docs.oracle.com/en/middleware/idm/unified-directory/12.2.1.3/oudig/updating-oracle-unified-directory-software.html#GUID-506B9DAC-2FDB-47C9-8E00-CC1F99215E81)
    
*   次のステップを使用してキーストア暗号化強度を更新し、12cアップグレードのパスワードを変更し、現在のキーストアの内容をリストしてキーストア・パスワード`<copy>keytool -list -v -keystore /u01/app/oracle/config/domains/IAMGovernanceDomain/config/fmwconfig/default-keystore.jks -storepass IAMUpgrade12c##</copy>`を入力します
    
*   • /tmp/keystoreという一時フォルダを作成し、keytoolコマンド`<copy> keytool -genkeypair -keystore /tmp/keystore/default-keystore.jks -keyalg RSA -validity 3600 -alias xell -dname "CN=wsidmhost.idm.oracle.com, OU=Identity, O=Oracle, C=US" -keysize 2048 -storepass IAMUpgrade12c## -keypass IAMUpgrade12c## </copy>`を使用して新しいキーを生成します。
    
*   署名証明書の生成
    
        <copy>
        keytool -certreq -alias xell -file /tmp/keystore/xell.csr -keypass IAMUpgrade12c## -keystore /tmp/keystore/default-keystore.jks -storepass IAMUpgrade12c## -storetype jks
        </copy>
        
*   証明書のエクスポート
    
        <copy>
        keytool -export -alias xell -file /tmp/keystore/xlserver.cert -keypass IAMUpgrade12c## -keystore /tmp/keystore/default-keystore.jks -storepass IAMUpgrade12c## -storetype jks
        </copy>
        
*   証明書を信頼する
    
        <copy>
        keytool -import -trustcacerts -alias xeltrusted -noprompt -file /tmp/keystore/xlserver.cert -keystore /tmp/keystore/default-keystore.jks -storepass IAMUpgrade12c##
        </copy>
        
*   証明書のインポート
    
        <copy>
        keytool -importkeystore -srckeystore /tmp/keystore/default-keystore.jks -destkeystore /u01/app/oracle/config/domains/IAMGovernanceDomain/config/fmwconfig/default-keystore.jks -srcstorepass IAMUpgrade12c## -deststorepass IAMUpgrade12c## -noprompt
        </copy>
        
*   .certと.csrを移動
    
        <copy>
        cp /tmp/keystore/x*.* /u01/app/oracle/config/domains/IAMGovernanceDomain/config/fmwconfig/
        </copy>
        
*   キーストアの確認
    
        <copy>
        keytool -list -v -keystore /u01/app/oracle/config/domains/IAMGovernanceDomain/config/fmwconfig/default-keystore.jks -storepass IAMUpgrade12c##
        </copy>
        
    
    EMのxellのパスワードを"IAMUpgrade12c##"に更新
    
    *   EMコンソールを開く
    *   「Weblogic Domain」→「IAMGovernanceDomain」にナビゲートします。
    *   右クリック IAMGovernanceDomain
    *   「セキュリティ」→「資格証明」を選択します。
    *   oimエントリの展開
    *   xell行のハイライト
    *   「編集」をクリックします
    *   パスワードの更新
    *   「OK」をクリックします。

2.  OIM/OAMを11.1.2.3からOIG/OAM 12.2.1.3にアップグレードします。
    
    アップグレード・アドバイザを使用して、OAMおよびOIM統合環境をアップグレードします。Oracleサポートのログイン資格証明でログインする必要があります。MOSドキュメントへのリンクを次に示します。
    
    *   [統合OAM/OIM環境の12c (12.2.1.3)アドバイザへのアップグレード](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=318814815527407&id=2342931.2&_adf.ctrl-state=13r3ivrcxc_57)
    *   _「ステップ4: 構成」_に移動します。
    *   _「ライフ・サイクル管理ツール設定環境のアップグレード」_にナビゲートします
    *   この項にリストされているすべてのステップを実行してOAMをアップグレードします。
    *   _ステップ4_は、OIMに対して1回、OAMに対して1回実行する必要があることに注意してください。

![Oracle Supportアップグレード・アドバイザ](./images/step2.png " ")

3.  次に示すMOSドキュメント・リンクを使用して、「スタック・パッチ・バンドル: Oracle Identity Management製品のスタック・パッチ・バンドルの適用」に記載されている12.2.1.3パッチを適用します。
    *   [OIGのスタック・パッチ・バンドル・ページ](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=320313382903924&id=2657920.1&_adf.ctrl-state=13r3ivrcxc_110)
    *   [12.2.1.3のSPBをダウンロードして適用します](https://support.oracle.com/epmos/faces/PatchSearchResults?_adf.ctrl-state=r390fd14k_135&_afrLoop=321341144687003)

## **タスク2**: IAMコンポーネントの12.2.1.3から12.2.1.4へのアップグレード

1.  次のドキュメントの_第6.4項_の手順を使用して、OUDを12.2.1.3から12.2.1.4にアップグレードします。
    
    *   [OUD 12.2.1.3を12.2.1.4にアップグレードします。](https://docs.oracle.com/en/middleware/idm/unified-directory/12.2.1.4/oudig/updating-oracle-unified-directory-software.html#GUID-506B9DAC-2FDB-47C9-8E00-CC1F99215E81)
2.  OAM 12.2.1.3を12.2.1.4にアップグレード: OAM 12cR2 PS4のアップグレード・アドバイザ(OAM 12.2.1.4.0)を使用してOAMをアップグレードします。
    
    *   [Oracle Access Manager 12cR2 PS4へのアップグレード](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=320632596387945&id=2564763.2&_adf.ctrl-state=13r3ivrcxc_167)
    *   _ステップ4: 構成_にナビゲートし、_「アップグレード」_を選択します。
    
    ![OAM 12cR2 PS4のアップグレード・アドバイザ(OAM 12.2.1.4.0)](./images/step6.png " ")
    
3.  OIG 12cR2 PS4のアップグレード・アドバイザ(OAM 12.2.1.4.0)を使用したOIG 12.2.1.3から12.2.1.4へのアップグレードOIGのアップグレード
    
    *   [Oracle Identity Governance/Oracle Identity Managerのアップグレード・アドバイザ 12cR2 PS4](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=320673956019924&id=2667893.2&_adf.ctrl-state=13r3ivrcxc_220)
    *   _ステップ4: 構成/アップグレード_にナビゲートします。
    
    ![OIG 12cR2 PS4のアップグレード・アドバイザ(OAM 12.2.1.4.0)](./images/step7.png " ")
    
4.  次に示すMOSドキュメント・リンクを使用して、スタック・パッチ・バンドル: Oracle Identity Management製品のスタック・パッチ・バンドルの適用に記載されている12.2.1.4パッチを適用します。
    
    *   [Oracle Identity Management製品のスタック・パッチ・バンドルの適用](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=320313382903924&id=2657920.1&_adf.ctrl-state=13r3ivrcxc_110)
    *   [ダウンロードして適用 12.2.1.4 SPB](https://support.oracle.com/epmos/faces/ui/patch/PatchDetail.jspx?parent=DOCUMENT&sourceId=2657920.1&patchId=32395452)

## **タスク3**: LDAPコネクタを使用したOIGおよびOAMの統合

次のドキュメントの_第2.3項_のステップ・バイ・ステップの手順を使用して、OIGとOAMの統合を構成します。

*   [Oracle Identity GovernanceとOracle Access Managerの統合の構成](https://docs.oracle.com/en/middleware/idm/suite/12.2.1.4/idmig/integrating-oracle-identity-governance-and-oracle-access-manager-using-ldap-connectors.html#GUID-9FD153DD-1497-4846-8D39-813B20E29B40)

## **タスク4**: OHSを12.2.1.4に遷移します。

1.  次のステップに従って、OHSをインストールおよび構成します。
    
    *   [OHSの準備とインストール](https://docs.oracle.com/en/middleware/fusion-middleware/12.2.1.4/wtins/preparing-install-and-configure-product.html#GUID-16F78BFD-4095-45EE-9C3B-DB49AD5CBAAD)
2.  OAM WebGateの構成: 次のドキュメントの_「Oracle HTTP Server WebGateの構成」_の6つのステップをすべて実行します。
    
    *   [WebGateの構成](https://docs.oracle.com/en/middleware/fusion-middleware/12.2.1.4/wgins/configuring-oracle-http-server-webgate-oracle-access-manager.html#GUID-79326DB8-CCB1-47F6-8CC2-80B6402C13FC)
    *   11.1.2.3 - Webgate\_IDM\_11gで使用されているものと同じエージェント・プロファイルを使用します
    *   _OAM\_DOMAIN\_HOME/output/OAM\_Webgate\_IDM11g_ディレクトリにあるすべてのアーティファクトを_OHSDOMAIN\_HOME/config/fmwconfig/components/OHS/ohs1/webgate/config_ディレクトリにコピーします。
    *   copyコマンドの後、_OHSDOMAIN\_HOME/config/fmwconfig/components/OHS/ohs1/webgate/config_ディレクトリには次のファイルとディレクトリが必要です
        *   aaa\_cert.pem
        *   aaa\_key.pem
        *   cwallet.sso
        *   ObAccessClient.xml
        *   oblog\_config\_wg.xml
        *   password.xml
        *   cwallet.ssoファイルを含むウォレット・ディレクトリ
        *   aaa\_cert.pemおよびaaa\_key.pemを使用した単純なディレクトリ
    *   単純ディレクトリが存在しない場合は、1つを作成して\*.pemファイルをコピーします。
    *   oamconsoleにナビゲートします: 構成/設定/Access Manager設定/Webgate Traffic Load Balancer/OAMサーバー・ポート
        *   ポートをOAMサーバー・ポート14100に変更します。
    *   「Oamconsole: Application Security/Agents/Webgate\_IDM\_11g」にナビゲートします。
        *   既存のユーザー定義パラメータの内容を次のリストに置き換えます。
            *   maxSessionTimeUnits= 分
            *   OAMRestEndPointHostName=wsidmhost.idm.oracle.com
            *   client\_request\_retry\_attempts=1
            *   proxySSLHeaderVar=IS\_SSL
            *   inactiveReconfigPeriod=10
            *   OAMRestEndPointPort=14100
            *   URLInUTF8Format=true
            *   OAMServerCommunicationMode=HTTP
3.  このMOSドキュメントを使用したOHSのヘッダー・サイズの増加
    
    *   [HTTPヘッダー・サイズを増やしてサーバー制限エラーを防止する方法](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=321479834906018&id=819301.1&_adf.ctrl-state=13r3ivrcxc_273)
    *   グローバル・サーバー・ディレクティブ"KeepAlive"の後に、LimitRequestFieldSize 16380をhttpd.confファイルに追加します
4.  OHSサーバーの起動
    

## **タスク5**: IAM 12.2.1.4統合環境の検証

1.  次のドキュメントの_第2.4.7項_のステップを使用してOAMおよびOIGをテストします。
    
    *   [Access ManagerとOracle Identity Governanceの統合の機能的なテスト](https://docs.oracle.com/en/middleware/idm/suite/12.2.1.4/idmig/integrating-oracle-identity-governance-and-oracle-access-manager-using-ldap-connectors.html#GUID-3803AA41-A882-41C9-B1E8-0BBCBD581CE9)
2.  OAMを検証します。
    
    *   [OAM管理サーバー・コンソール](http://wsidmhost.idm.oracle.com:7001/oamconsole)にアクセスし、_oamadmin_としてログインします。
        *   OAM管理サーバーが正しく機能していることを検証します。
    *   [OAM Serverコンソール](http://wsidmhost.idm.oracle.com:14100/oam/server/logout)にアクセスします。
        *   OAMサーバーが実行中であり、HTTPポートで正常にリスニングしていることを検証します
    *   [OAM Serverコンソール](http://wsidmhost.idm.oracle.com:7777/oam/server/logout)にアクセスします。
        *   OAMサーバーに対するOHSプロキシ構成が機能しており、Webgateも機能していることを検証します。
    *   `Netstat -a -n | grep 5575`を実行します
        *   LISTENが返されます。
        *   OAMサーバーがOAPポートでリスニングしていることを検証します
3.  OIGの検証:
    
    *   次のURLを使用して、Oracle Identity Governanceページにアクセスします。
        *   [Oracle Identityセルフ・サービス](http://wsidmhost.idm.oracle.com:7778/identity)
        *   [Oracle Identity System Administration](http://wsidmhost.idm.oracle.com:7778/sysadmin)
    *   「パスワードを忘れた場合」、「新規アカウントの登録」および「ユーザー登録のトラッキング」機能のリンクがログイン・ページに表示され、機能することを確認します。
    *   [Oracle Identityセルフ・サービス](http://wsidmhost.idm.oracle.com:7778/identity)に_xelsysadm_としてログインします。
    *   新規ユーザーの作成
    *   _xelsysadm_としてログアウトし、_手順4_で作成したユーザーとしてログインします。- ログインのプロンプトが表示されたら、新しく作成したユーザーの有効な資格情報を指定します。
    *   新しいパスワードを入力し、3つのチャレンジ質問と回答を選択します。- 新しいユーザーがログインします。
    *   新しいプライベートブラウザを開き、_xelsysadm_としてログインすることで、ロック/無効化機能が動作することを確認します。手順4で作成したユーザーアカウントをロックまたは無効にします。
    *   新しいユーザーがログに記録されているブローワに戻り、新しいリンクをクリックして、ログイン・ページに戻す必要があります。
    *   SSOログアウト機能が動作することを確認するには、_xelsysadm_がまだログアウトされているOracle Identity Self Serviceページをログアウトします。- ページからログアウトすると、SSOログアウト・ページにリダイレクトされます。

**おめでとうございます!このワークショップを終えました!**

## さらに学ぶ

*   Oracle IAMの最新バージョンの詳細は、[こちら](https://docs.oracle.com/en/middleware/idm/suite/12.2.1.4/index.html)を参照してください
*   アップグレード計画の詳細は、[ここ](https://docs.oracle.com/en/middleware/fusion-middleware/iamus/place-upgrade-strategies.html#GUID-9F906AE2-5BDF-426D-A97C-AC546ABFBD28)を参照してください

## 謝辞

*   **著者** - クラウド・プラットフォームCOE、ディレクタ、Anbu Anbarasu氏
*   **コントリビュータ** - Eric Pollard - 継続的なエンジニアリング、Ajith Puthan - IAMサポート、Rene Fontcha
*   **最終更新者/日付** - Sahaana Manavalan氏、LiveLabs Developer、NA Technology、2022年5月