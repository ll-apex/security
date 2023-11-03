# Oracle DatabaseおよびOracle Identity Governanceへの接続を確立するには

## 概要

Oracle DatabaseおよびOracle Identity Governanceへの接続の確立

*   ペルソナ: Identity Domain Administrator

_予想時間_: 15分

ラボのクイック・ウォークスルーについては、次のビデオをご覧ください。[サイジングのないOracle Video Hubのビデオ](videohub:1_61qwyi4k)

### 目標

この演習では、次のことを行います。

*   Oracle DatabaseおよびOracle Identity Governanceへの接続を確立するには

## タスク1: Dockerが稼働していることの確認

1.  端末セッションをオープンします。
    
2.  dockerのバージョンを確認します。
    
        <copy>docker -v</copy>
        
    
        Expected output: Docker version 23.0.0, build e92dd87
        
3.  ステータスを検証して、dockerサービスが稼働中かどうかを検証します。
    
        <copy>systemctl status docker</copy>
        
    
    コマンド・プロンプトに戻るには、**Ctrl+C**と入力します
    

## タスク2: Oracle Identity Governance (OIG) DBサービスの起動

1.  スクリプト・ファイルがあるディレクトリに移動します。
    
        <copy>cd /scratch/idmqa/scripts</copy>
        
2.  ディレクトリ内のファイルを表示します。
    
        <copy>ls</copy>
        
3.  次のスクリプトを使用して、DBおよびすべてのサーバーを手動で起動します。
    
        <copy>./start_db.sh</copy>
        
    
    DBが起動するまで待機します。
    
4.  次のコマンドを使用して、OIGサービスを起動します。
    
        <copy>./start_all_servers.sh</copy>
        

## タスク3: Oracle Identity Governanceとの統合

1.  Oracle Access Governanceサービスのホームページ_ラボ6: タスク1を参照_し、ナビゲーション・メニュー・アイコンをクリックして、**「サービス管理」**、**「接続済システム」**の順に選択します。
    
    ![アクセス・ガバナンス・コンソール- 接続済システム](images/connected-systems.png)
    
2.  **「接続システムの追加」**をクリックします
    
    ![追加- 接続済システム](images/add-connected-system.png)
    
3.  **「Would you like to connect to an Identity Governance System」**というラベルのタイルで、**「Add」**ボタンを選択します。 ![Access Governanceコンソール- 接続済システム- 追加](images/connected-system-page.png)
    
4.  情報ポップアップの**「Close」**をクリックして、**「Add an Identity Governance System」**ページに移動し、構成を開始します。
    
    ![ポップアップウィンドウを閉じる](images/pop-up.png)
    
5.  **「システムの選択」**ステップで、**Oracle Identity Governance**のタイルを選択してターゲットOracle Identity Governance接続済システムのエージェントを構成し、**「次」**をクリックします。
    
    ![Access Governanceコンソール- 接続されたシステム- 次へ](images/select-oig.png)
    
6.  **「詳細の入力」**ステップで、次の詳細を入力します:
    
    *   **名前:** oag
    *   **説明:** oag
    *   **「次へ」をクリックします。**
    
    ![Access Governanceコンソール- Connected Systems-OIG](images/oag-select-system.png)
    
7.  **「構成」**ステップで、ターゲット・システムの接続詳細を入力します。
    
    **JDBC URL:**次のURLのプレースホルダをコンピュート・インスタンスのプライベートIPに置き換えます。コンピュート・インスタンスのプライベートIPについては、_ラボ5: タスク3_を参照してください。
    
        <copy>jdbc:oracle:thin:@//<--privateipofyourcomputeinstance-->:1521/ORCL.NETWORKSPEOSUBN.IDMOCICLOU02PHX.ORACLEVCN.COM</copy>
        
    
    **OIGデータベース・ユーザー名:**
    
        <copy>DEV_OIM</copy>
        
    
    **パスワード:**
    
        <copy>Welcome1</copy>
        
    
    **パスワードの確認:**
    
        <copy>Welcome1</copy>
        
    
    **OIGサーバーURL:**次のURLのプレースホルダをコンピュート・インスタンスのプライベートIPに置き換えます。コンピュート・インスタンスのプライベートIPについては、_ラボ3: タスク3_を参照してください。
    
        <copy>http://<--privateipofyourcomputeinstance-->:14000</copy>
        
    
    **OIGサーバーのユーザー名:**
    
        <copy>xelsysadm</copy>
        
    
    **OIGサーバーのユーザー・パスワード:**
    
        <copy>Welcome1</copy>
        
    
    **OIGサーバー確認パスワード:**
    
        <copy>Welcome1</copy>
        
    
    ![詳細の構成](images/oag-connection-details.png)
    
8.  「エージェントのダウンロード」ステップで、_「ダウンロード」リンク_を選択し、エージェントzipファイルをダウンロードします。zipファイルは、/home/opc/Downloadsにあります。
    
    ![エージェントのダウンロード](images/oag-download-link.png)
    
9.  ダウンロードしたエージェントのzipファイルを確認できます。
    
    ![ファイル・システムにナビゲートします。](images/locate-zip.png)
    
    ![zipファイルを確認します。](images/verify-zip.png)
    

## タスク4: コンピュート・インスタンスへのOAGエージェントのインストールと構成

1.  端末セッションをオープンします。
    
    ![ターミナル・セッションを開く](images/open-terminal-window.png)
    
2.  /home/opc/Downloadsフォルダにあるダウンロードしたzipファイル(oag.zip)を/home/opc/zip\_oagフォルダに移動します。
    
        <copy>mv /home/opc/Downloads/oag.zip /home/opc/zip_oag</copy>
        
    
    ![OAGエージェントをzip_oagに移動します](images/move-oag-agent.png)
    
    エージェントzip (oag.zip)がフォルダzip\_oag内に存在することを確認します。
    
        <copy>cd /home/opc/zip_oag</copy>
        <copy>ls</copy>
        
    
    ![エージェントのzip oag.zipを確認します](images/env_setup.png)
    
3.  次のコマンドを使用して環境変数を設定します。
    
        <copy>cd ~</copy>
        <copy>source oag_agent.env</copy>
        
    
    ![環境の初期化](images/terminal-oag.png)
    
4.  エージェントのインストール
    
        <copy>sh agentManagement.sh --volume /home/opc/vol_oag --agentpackage /home/opc/zip_oag/oag.zip --install</copy>
        
    
    ![エージェントのインストール](images/agent-install.png)
    
5.  エージェントの開始
    
        <copy>sh agentManagement.sh --volume /home/opc/vol_oag --start</copy>
        
    
    ![エージェントの開始](images/agent-start.png)
    
6.  エージェントの検証
    
        <copy>sh agentManagement.sh --volume /home/opc/vol_oag --status</copy>
        
    
    ![エージェントの検証](images/agent-status.png)
    

## タスク5: Oracle Databaseへの接続およびDBエージェントのダウンロード

1.  次のステップに従って、Oracle Access Governanceコンソールの「接続済システム」ページにナビゲートします: Oracle Access Governanceのナビゲーション・メニュー・アイコンの「ナビゲーション」メニューから、「サービス管理」→「接続済システム」を選択します。「Add a connected system」ボタンをクリックして、ワークフローを開始します。
    
2.  「接続システムの追加」ページから、「データベース管理システムに接続しますか?」タイルの「追加」ボタンを選択します。
    
3.  ワークフローのシステム・ステップの選択で、「データベース・ユーザー管理」(Oracle DB)を選択し、「次へ」をクリックします。
    
4.  ワークフローの「Enter Details」ステップで、接続されたシステムの詳細を入力します。
    
         * What do you want to call your database : OAG-DB
         * How do you want to describe this database: OAG-DB
        
    
    「次へ」をクリックします。
    
5.  ワークフローの「構成」ステップで、Oracle Access Governanceがターゲット・データベースに接続するために必要な構成の詳細を入力します。
    
         * Easy Connect URL for Database: jdbc:oracle:thin:@//<—privateipaddressofcomputeinstance-->/ORCL.NETWORKSPEOSUBN.IDMOCICLOU02PHX.ORACLEVCN.COM
        
         * User Name: sys as sysdba
        
         * Password: Welcome1
        
         * Confirm password: Welcome1
        
6.  右側のペインをチェックして、選択した内容を表示します。入力した詳細に問題がない場合は、「追加」を選択して接続システムを作成します。
    
7.  ワークフローの「終了」ステップでは、Oracle Access GovernanceとOracle Database間のインタフェースに使用するエージェントをダウンロードするように求められます。「ダウンロード」リンクを選択して、エージェントを実行する環境にエージェントzipファイルをダウンロードします。
    

## タスク6: ターゲット・システムへのDBエージェントのインストール

1.  ターミナルを開きます。
    
2.  ボリュームを作成します。
    
        <copy>mkdir  /home/opc/vol_oag_db</copy>
        
3.  「ダウンロード」フォルダ内にエージェントzip (OAG-DB.zip)が存在することを確認します。
    
        <copy> cd /home/opc/Downloads
        ls</copy>
        
4.  次のコマンドを使用して環境変数を設定します。
    
        <copy> cd ~
        source oag_agent.env</copy>
        
5.  次のコマンドを使用して、エージェントをインストールします。
    
        <copy>curl https://raw.githubusercontent.com/oracle/docker-images/main/OracleIdentityGovernance/samples/scripts/agentManagement.sh -o agentManagement.sh; 
        

\`\`\`\`\` sh agentManagement.sh --volume /home/opc/vol\_oag\_db --agentpackage /home/opc/Downloads/OAG-DB.zip --install \`\`\` 3.次のコマンドを使用してエージェントを起動します。

      ```
      <copy>sh agentManagement.sh --volume /home/opc/vol_oag_db --start</copy>
      ``` 
    

## タスク7: エージェント・インストールの検証

1.  Oracle Access Governanceコンソールにログインし、ナビゲーション・メニュー・アイコンを選択してナビゲーション・メニューを表示します。
2.  Oracle Access Governanceコンソールで、ナビゲーション・メニューから「サービス管理」→「接続済システム」を選択します。
3.  「Connected Systems」画面で、接続されたシステムの名前を示すタイルに、「Waiting for initial connection」のステータスが表示されます。「接続の管理」をクリックします。
4.  ページ下部の「アクティビティ・ログ」に、検証操作のステータスが表示されます。エージェントが起動している間は「保留中」です。エージェントが起動しない場合は、エージェントのインストール・ログおよび操作ログで問題を確認してください。
5.  エージェントが起動すると、検証操作のステータスが「成功」と表示されます。

**次の演習に進む**ことができます。

## さらに学ぶ

*   [Oracle Access Governanceアクセス・レビューの作成キャンペーン](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance製品ページ](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governanceの製品ツアー](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access GovernanceのFAQ](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## 謝辞

*   **著者** - Anuj Tripathi、Indira Balasundaram、Anbu Anbarasu