# SecureOracle環境の初期化

## 概要

この演習では、このワークショップを正常に実行するために必要なすべてのコンポーネントを確認して起動します。

_推定ラボ時間_: 30分

### 目標

*   SecureOracleワークショップ環境を初期化します。

### 前提条件

この演習では、次を想定しています。

*   Free Tier、有料またはLiveLabs Oracle Cloudアカウント
*   次を完了しました:
    *   演習: 設定の準備(_無料層_および_有料テナント_のみ)
    *   演習: 環境設定

## タスク1: 必要なプロセスが稼働していることの検証

1.  リモート・デスクトップ・セッションにアクセスできるようになったら、次に示すように続行して環境を検証してから、後続の演習の実行を開始します。次のプロセスが稼働している必要があります。
    
    *   データベース・リスナー
        *   リスナー(1521)
    *   データベース・サーバー・インスタンス
        *   IAMDB
    *   Hedwigメール・サーバー
    
    ![](./images/landing.png " ")
    
2.  _ターミナル_・セッションから次を実行して、予想されるプロセスが稼働していることを確認します。
    
        <copy>
        systemctl status hedwig.service
        systemctl status oracle-database
        </copy>
        
    
    ![](./images/hedwig.png " ") ![](./images/sql.png " ")
    
    前述のように、期待されるすべてのプロセスが出力に表示される場合、環境は次のタスクに対応できます。
    
3.  疑わしい出力、障害または停止コンポーネントが表示された場合は、それに応じてサービスを再起動します。
    
        <copy>
        sudo systemctl restart oracle-database
        </copy>
        
    
    _Hedwig Mailサーバー_を再起動するには:
    
        <copy>
        export JAVA_HOME=/home/oracle/products/jdk
        sudo -E /home/oracle/demo/hedwig-0.7/bin/run.sh stop
        sudo -E /home/oracle/demo/hedwig-0.7/bin/run.sh start
        </copy>
        

## タスク2: OIGコンポーネントの起動

1.  _SSHクライアントを使用してリモートで_新しいターミナル・セッションを開き、次に示すように続行して、すべてのコンポーネントを_oracle_ユーザーとして起動します
    
        <copy>sc start all</copy>
        
    
    **ノート:** OIGコンポーネントを起動する時間は、15分から20分によって異なります。![OIGコンポーネントの起動中](./images/startall.png " ")
    
    参照用に、様々なコンポーネントおよびアプリケーションを起動または停止するために、次のコマンドを使用できます。
    
        sc <start|stop|status> oim          // start, stop or status OIG, SOA Server and OUD
        sc <start|stop|status> oim_bip      // start, stop or status OIG, SOA Server, OUD and BIP Server
        sc <start|stop|status> oam          // start, stop or status OAM, OUD and both OHS Servers
        sc <start|stop|status> oam_ohs1     // start, stop or status OAM, OUD and OHS Server 1
        sc <start|stop|status> oam_ohs2     // start, stop or status OAM, OUD and OHS Server 2
        sc <start|stop|status> all          // start, stop or status all OIG and OAM components
        

## タスク3: 開発ツールの実行

SecureOracleの開発ツールは、OIGワークフロー承認用のSOAコンポジットの編集などのユースケースをサポートすることを目的としていますが、必要に応じて様々なコンポーネントのカスタマイズおよび構成にも役立ちます。

1.  リモート・デスクトップで開いたターミナル・セッションから、次のコマンドを実行して、**SOA拡張を使用したOracle JDeveloper**を起動します。
    
        <copy>
        cd ~
        ./startJDEVSOA.sh
        </copy>
        
    
    **ノート**: **`/home/oracle/jdevhome`**フォルダで使用可能なOIGワークフローを含むSOAコンポジットを示すサンプル・アプリケーションがあります。
    
        NAME                                 DESCRIPTION
        RoleOwnerApproval.jws                Role single approval
        MultiRoleOwnerApproval.jws           Role multi-approval
        CustomDisconnectedProvisioning.jws   Disconnected app approval based on Sales Role
        
    
    また、OIMの即時利用可能なSOAコンポジットは次のフォルダにあります。
    
        /home/oracle/products/oam/idm/server/workflows/composites
        
2.  **Oracle SQLDeveloper**を起動するには、次のコマンドを実行します:
    
        <copy>
        ./startSQLDEV.sh
        </copy>
        
    
    **ノート**: `HR`、`HEDWIG`および`IAMDB`という3つの接続は、My HR、HedwigおよびIAMデータベース・スキーマにアクセスするためにすでに定義されています。または、ローカル・ホスト・コンピュータにSQL Developerをインストールして、異なるデータベース・スキーマにアクセスすることもできます。
    
3.  **Apache Studio**を起動し、OUD/LDAPインスタンスを管理するには、次のコマンドを実行します:
    
        <copy>
        	./startAStudio.sh
        </copy>
        
    
    **ノート**: OUDにアクセスするために接続`IAM-OUD`がすでに定義されています。
    
4.  IAMデータベースを指す**Oracle PL/SQL**コマンドライン・ツールを起動するには、次のコマンドを実行します:
    
        <copy>
        cd ~
        . ./setDBenv.sh
        ./startPLSQL.sh
        </copy>
        

## タスク4: 管理コンソール、アプリケーションおよびユーザー資格証明

ローカル・コンピュータからこれらにアクセスする場合は、次のいずれかのオプションを選択します。

*   ローカルMac/Linuxホストで次のホスト・エントリを**`/etc/hosts`**に追加した後、またはMicrosoft Windowsホストで**`C:\Windows\System32\drivers\etc\hosts`**に追加した後、この項に示されているURLを使用します
    
        <copy><public_ip> secureoracle.oracledemo.com  secureoracle</copy>
        
*   次の各URLの**`secureoracle.oracledemo.com`**をインスタンスの`Public_IP_Address`に置き換えます
    

1.  次のURLおよび資格証明を使用して、すべての異なるWebコンソールにアクセスします。
    
    Oracle Identity Manager Web管理コンソール:
    
        URL         http://secureoracle.oracledemo.com:14000/sysadmin
        User        xelsysadm
        Password    Oracle123
        
    
    Oracle Identity Managerセルフサービス:
    
        URL         http://secureoracle.oracledemo.com:14000/identity
        User        xelsysadm
        Password    Oracle123
        
    
    Oracle Identity Manager Design Console。最初に次のコマンドを実行して、設計コンソールを起動します。
    
        <copy>
        cd /home/oracle/products/oim/idm/designconsole
        ./xlclient.sh
        </copy>
        
    
    要求されたら、次のURLおよび資格証明を入力します。
    
        URL         t3://secureoracle.oracledemo.com:14000
        User        xelsysadm
        Password    Oracle123
        
    
    Oracle BI Publisher管理コンソール:
    
        URL         http://secureoracle.oracledemo.com:9502/xmlpserver
        User        xelsysadm
        Password    Oracle123
        
    
    Oracle Identity Manager EMコンソール:
    
        URL         http://secureoracle.oracledemo.com:7001/em
        User        weblogic
        Password    Oracle123
        
    
    Oracle Access Manager管理コンソール:
    
        URL         http://secureoracle.oracledemo.com:8001/oamconsole
        User        oamadmin
        Password    Oracle123
        
    
    Oracle Access Manager EMコンソール:
    
        URL         http://secureoracle.oracledemo.com:8001/em
        User        weblogic
        Password    Oracle123
        
    
    Email Server管理コンソール(Hedwig Mail Server):
    
        URL         http://secureoracle.oracledemo.com:7001/hedwig-web-0.6/
        User        admin
        Password    Oracle123
        
    
    **ノート**: Roundcube電子メール・クライアントとの接続時に問題が発生した場合は、**Hedwigメール・サーバー**を再起動できます。たとえば、次のコマンドを実行して、ユーザー_opc_としてHedwig Mail Serverを再起動します。
    
        <copy>
        export JAVA_HOME=/home/oracle/products/jdk
        sudo -E /home/oracle/demo/hedwig-0.7/bin/run.sh stop
        sudo -E /home/oracle/demo/hedwig-0.7/bin/run.sh start
        </copy>
        
    
    電子メールWebクライアント(Roundcube):
    
        URL         http://secureoracle.oracledemo.com/roundcubemail-1.4.1/
        User        admin
        Password    Oracle123
        Server      secureoracle.oracledemo.com
        
    
    自分のHRアプリケーション:
    
        URL         http://secureoracle.oracledemo.com:7001/ords/f?p=100
        User        hradmin
        Password    Oracle123
        
    
    IGAアプリケーション:
    
        URL         http://secureoracle.oracledemo.com:7001/ords/f?p=102
        User        mgraff
        Password    Oracle123
        
    
    APEX管理コンソール
    
        URL         http://secureoracle.oracledemo.com:7001/ords/apex_admin
        User        admin
        Pass        #Oracle123
        
    
    APEX HRワークスペース
    
        URL         http://secureoracle.oracledemo.com:7001/ords/
        Workspace   HRSPACE
        User        hradmin
        Pass        Oracle123
        
    
    **ノート**: My HRおよびMy IGAアプリケーションは、これらのアプリケーションをサポートするサービス(Oracle REST Data Services)としてOIGドメインが起動された後、OIG管理サーバーにデプロイされます。
    
2.  次の資格証明を使用して、他のミドルウェア・コンポーネントにアクセスできます。
    
    Oracle IAMデータベース:
    
        Host/port       secureoracle.oracledemo.com:1521
        Service name    iamdb.oracledemo.com
        User            sys as SYSDBA
        Password        Oracle123
        
    
    My HR Applicationデータベース:
    
        Host/port       secureoracle.oracledemo.com:1521
        Service name    iamdb.oracledemo.com
        User            hr
        Password        Oracle123
        
    
    Oracle Unified Directory(OUD):
    
        Host/port       secureoracle.oracledemo.com:1389
        User            cn=Directory Manager
        Password        Oracle123
        

## タスク5: ブランドSecureOracle (オプション)

OIGセルフ・サービス・インタフェースでロゴをカスタマイズするには、次の手順を使用します。図については、インスタンスにステージングされたサンプル・ロゴ・イメージを使用します。ご希望の方はお気軽にご使用ください。独自のロゴを使用する場合は、145 x 38ピクセルの推奨サイズに従ってください。

1.  イメージファイルを _`/home/oracle/products/oim/idm/server/apps/oim.ear/iam-consoles-faces.war/images`_にコピーします
    
        <copy>
        cp /home/oracle/demo/sample-data/mylogo.png /home/oracle/products/oim/idm/server/apps/oim.ear/iam-consoles-faces.war/images
        </copy>
        
2.  **xelsysadm**ユーザーとしてOIGセルフ・サービス・コンソールにログインします。セルフサービス ページの右上隅にある **\[サンドボックス\]**リンクをクリックします。
    
3.  **「Manage Sandboxes」**タブで**「Create Sandbox」**をクリックし、名前を入力して**「Save and Close」**、**「OK」**の順にクリックします。
    
4.  現在のページの右上隅にある**「カスタマイズ」**リンクをクリックします。
    
5.  カスタマイズ・パネルがページの上部に表示されます。**「構造」**をクリックし、Oracleロゴ・イメージをクリックします。ポップアップ・ウィンドウの**「共有コンポーネント編集の確認」**で、**「編集」**ボタンをクリックします。
    
6.  次に、右パネルの上部にある**歯車と鉛筆**アイコンをクリックします。
    
7.  **「コンポーネントのプロパティ: commandImageLink」**ウィンドウで、「アイコン」プロパティの横の**下矢印**アイコンをクリックして、**「式ビルダー」**を選択します。
    
8.  次のようにパス・イメージを入力します。
    
    たとえば、次の形式でパスを入力します。
    
        <copy>
        /home/oracle/products/oim/idm/server/apps/oim.ear/iam-consoles-faces.war/images/mylogo.png
        </copy>
        
9.  **「OK」**をクリックして式ビルダー・ウィンドウを閉じ、**「OK」**を再度クリックして変更を確認します。カスタム・ロゴ・イメージは、Oracleロゴ・イメージのかわりに表示されます。
    
10.  右上隅にある**「閉じる」**ボタンをクリックして、カスタマイズ・パネルを閉じます。
    
11.  **「サンドボックスの管理」**タブに戻り、作成したサンドボックス名を選択して**「サンドボックスの公開」**をクリックし、**「はい」**をクリックして確認します。
    

## **付録**: サンプル組織について

1.  SecureOracleには、上位OIM組織**Oracleユーザー**のサンプルと、2つの子部門**営業**および**財務**が含まれています。部門ごとに、委任管理を示すために管理者アカウントが定義されています。さらに、管理者承認、エスカレーション、および組織の異動を示すために、サンプル ユーザーが追加されました。
    
    ![サンプルの上位OIM組織](./images/img-orgtree.png " ")
    
    図4.サンプルOIG組織
    
2.  次に、SecureOracleに含まれるデモ・ユーザーのクイック・リファレンスを示します。これらのユーザーと組織をサンプル・デモンストレーションで使用する方法の詳細は、ユースケースのドキュメントを参照してください。
    
        USERNAME        ORGANIZATION     TITLE                        ADMIN ROLE      SCOPE OF CONTROL
        FINANCEADM      Finance          Administration Assistant     FinanceAdmin    Finance
        SALESADM        Sales            Administration Assistant     SaleseAdmin     Sales
        MGRAFF          Sales            Sales Manager
        HDANIELS        Sales            Sales Manager
        JSMITH          Finance          Finance Manager
        
3.  さらに、すべてのデモ用の電子メール・アカウントが作成され、users.Youは、資格証明**USERNAME/Oracle123**を持つ[Roundcube](http://secureoracle.oracledemo.com/roundcubemail-1.4.1/)電子メール・クライアントを使用して受信ボックスにアクセスできます。
    
        USERNAME     EMAIL
        AHUTTON      ahutton@oracledemo.com
        JMALLIN      jmallin@oracledemo.com
        DFAVIET      dfaviet@oracledemo.com
        SMAVRIS      smavris@oracledemo.com
        MCHAN        mchan@oracledemo.com
        HDANIELS     hdaniels@oracledemo.com
        MGRAFF       mgraff@oracledemo.com
        SALESADM     salesadm@oracledemo.com
        ECLARK       eclark@oracledemo.com
        JSMITH       jsmith@oracledemo.com
        PSONG        psong@oracledemo.com
        PCAR         pcar@oracledemo.com
        FINANCEADM   financeadm@oracledemo.com
        DCOBY        dcoby@oracledemo.com
        GMARTON      gmarton@oracledemo.com
        RLAURIA      rlauria@oracledemo.com
        RMAINOR      rmainor@oracledemo.com
        

_次の演習に進む_ことができます。

## さらに学ぶ

Oracle Identity and Access Managementの詳細は、次のリンクを参照してください。

*   [Oracle Identity Management Webサイト](https://docs.oracle.com/en/middleware/idm/suite/12.2.1.4/index.html)
*   [Oracle Identity Governanceのドキュメント](https://docs.oracle.com/en/middleware/idm/identity-governance/12.2.1.4/index.html)
*   [Oracle Access Managementドキュメント](https://docs.oracle.com/en/middleware/idm/access-manager/12.2.1.4/books.html)

## 謝辞

*   **著者** - Ricardo Gutierrez氏、ソリューション・エンジニアリング- セキュリティおよび管理
*   **貢献者** - Rene Fontcha、Sahaana Manavalan
*   **最終更新者/日付** - Sahaana Manavalan氏、LiveLabs開発者、NA Technology、2022年3月