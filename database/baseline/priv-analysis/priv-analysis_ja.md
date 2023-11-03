# Oracle権限分析

## 概要

このワークショップでは、Oracle Privilege Analysisの機能を紹介します。これにより、ユーザーはこの機能を使用して、すべてのデータベース・ライフ中にすべてのユーザーがアクセスした権限の使用状況を常に把握する方法を学習できます。

_推定ラボ時間:_ 15分

_この演習でテストしたバージョン:_ Oracle DB 19.19

### ビデオ・プレビュー

「_LiveLabs - Oracle Privilege Analysis_」のプレビューを見る[](youtube:OsvxpBIKoOQ)

### 目標

*   データベースのワークロードを取得します
*   権限分析レポートを生成して、この取得時に使用されたすべてのユーザー/システム権限を把握します。

### 前提条件

この演習では、次を想定しています。

*   Free Tier、有料またはLiveLabs Oracle Cloudアカウント
*   次を完了しました:
    *   演習: 設定の準備(_無料層_および_有料テナント_のみ)
    *   演習: 環境設定
    *   演習: 環境の初期化

### ラボのタイミング(推定)

| ステップ番号 | 機能 | 近似時間 |
| --- | --- | --- |
| 1 | 分析のための作業量の取得 | 5分 |
| 2 | 占有ワークロードの分析 | 5分 |
| 3 | 取得を削除します | 5分未満 |

## タスク1: 分析するワークロードの取得

1.  OSユーザー**oracle**として、_DBSec-Lab_ VMでターミナル・セッションを開きます
    
        <copy>sudo su - oracle</copy>
        
    
    **ノート**: リモート・デスクトップ・セッションを使用している場合は、デスクトップの_ターミナル_・アイコンをダブルクリックしてセッションを起動します。
    
2.  scriptsディレクトリへ移動します
    
        <copy>cd $DBSEC_LABS/priv-analysis</copy>
        
3.  まず、ユーザーに`CAPTURE_ADMIN`ロールがあることを確認し、権限分析取得を作成します。
    
        <copy>./pa_create_capture.sh</copy>
        
    
    ![権限分析](./images/pa-001.png "権限分析取得の作成")
    
4.  次に、取得を開始します。
    
        <copy>./pa_enable_capture.sh</copy>
        
    
    ![権限分析](./images/pa-002.png "取得の開始")
    
    **ノート**: これにより、使用中のすべての権限またはロール(あるいはその両方)の収集が開始されます
    
5.  使用済および未使用のロールと権限を使用できるように、一部のワークロードを生成します。
    
        <copy>./pa_generate_workload.sh</copy>
        
    
    ![権限分析](./images/pa-003.png "一部のワークロードの生成")
    
6.  十分なデータがあると感じた場合は、取得を無効にできます
    
        <copy>./pa_disable_capture.sh</copy>
        
    
    ![権限分析](./images/pa-004.png "取得の無効化")
    

## タスク2: 取得されたワークロードの分析

1.  レポートの生成
    
        <copy>./pa_generate_report.sh</copy>
        
    
    ![権限分析](./images/pa-005.png "レポートの生成")
    
    **ノート:**
    
    *   取得時に使用されたと識別されたすべての権限およびロールを取得し、各ユーザーに付与されたロールおよび権限と比較します。
    *   処理するボリュームによっては、生成に数分かかる場合があります
2.  次に、取得出力に関連付けられたビューを問い合せて、レポート結果を表示します。
    
        <copy>./pa_review_report.sh</copy>
        
    
    ![権限分析](./images/pa-006.png "レポート結果")
    
    **ノート:**
    
    *   取得時にすべてのアクティブ・ユーザーによって使用および未使用のすべての権限(システムおよびオブジェクト)を表示できます。
    *   このステップは、ユーザーが自分の権限を正しく使用しているかどうかを判断するため、または、特にアイデンティティ盗難の危険を避けるために必要でない権限を取り消す必要があるかどうかを判断するために、この期間中にデータベースで何が起こったかをよりよく理解するために不可欠です。
    *   この権限分析タスクは必要な回数実行できることに注意してください。実際には、ユーザーのアクティビティ権限を常に制御し、潜在的な攻撃者による権限昇格の試行を回避するために、**できるだけ頻繁に実行することを強くお薦めします**
3.  次に、DB管理コンソール(OEM Cloud Control)を開き、同じレポートを表示しますが、より適切な方法で表示します。
    
    *   URL _`https://dbsec-lab:7803/em`_でWebブラウザを開く
        
        **ノート:**リモート・デスクトップを使用していない場合は、_`https://<YOUR_DBSEC-LAB_VM_PUBLIC_IP>:7803/em`_に移動してこのページにアクセスすることもできます
        
    *   _`SYSMAN`_として、パスワード_`Oracle123`_を使用してOracle Enterprise Manager 13cコンソールにログインします。
        
            <copy>SYSMAN</copy>
            
        
            <copy>Oracle123</copy>
            
        
        ![権限分析](./images/pa-007.png "OEM - ログイン画面")
        
    *   すべてのデータベースを展開し、**cdb\_PDB1**をクリックします
        
        ![権限分析](./images/pa-008.png "OEM - ターゲットの概要")
        
    *   メニューで、**「セキュリティ」**および**「権限分析」**を選択します。
        
        ![権限分析](./images/pa-009.png "OEM - 権限分析メニュー")
        
    *   「**Named**」を選択し、「Credential Name」として「**PA\_ADMIN**」を選択します。
        
        ![権限分析](./images/pa-010.png "OEM - 権限分析ログイン")
        
    *   \[**ログイン**\]をクリックします
        
    *   取得中に生成されたレポート(**「すべてのデータベース取得」**を参照)を選択し、\[**レポートの表示**\]をクリックします
        
        ![権限分析](./images/pa-011.png "OEM - 権限分析取得")
        
    *   取得期間中にデータベースに接続しているすべてのユーザーが、使用された権限とともに表示されます。
        
        ![権限分析](./images/pa-012.png "OEM - 権限分析グローバル・レポート")
        
    *   **「使用済」**タブをクリックして、取得中に使用されたすべての権限と使用者を表示します。
        
        ![権限分析](./images/pa-013.png "OEM - 権限分析使用済レポート")
        
        **ノート**: 「スプレッドシートにエクスポート」ボタンを使用すると、権限の付与または取消しに関する決定を行うことができる個人にこの情報を提供できます。
        
    *   未使用の権限(アカウントが不要な権限)を表示することもできます。その場合、これらの不要な特権を削除することは、これらのアカウントが侵害された場合に発生する可能性のある損害の量を縮小する優れた方法です。**「未使用」**タブをクリックして、取得時に使用されなかったすべての権限、および取得時に使用されなかったすべての権限を表示します。
        
        ![権限分析](./images/pa-014.png "OEM - 権限分析未使用レポート")
        
        **ノート**: ユーザーに権限が多すぎるか、タスクを実行する必要がない権限が明確に表示されます。
        

## タスク3: 取得の削除

1.  レポートをレビューし、権限分析に満足したら、作成した取得を削除できます
    
        <copy>./pa_drop_capture.sh</copy>
        
    
    ![権限分析](./images/pa-015.png "取得を削除します")
    

次の演習に進むことができます。

## **付録**: 製品について

### **概要**

権限分析により、データベース・ロールおよび権限に対する最小権限のベスト・プラクティスを実装することで、アプリケーションおよびデータベース操作のセキュリティが向上します。

権限分析は、Oracle Databaseカーネルの内部で実行することで、最小権限モデルを実装するための使用済および未使用の権限を識別することで、ユーザー、ツールおよびアプリケーション・アカウントの攻撃面を縮小するのに役立ちます。

![権限分析](./images/pa-concept.png "権限分析の概念")

権限分析は、データベース・ユーザーおよびアプリケーションで使用される権限を動的に取得します。権限分析を使用すると、最小権限のガイドラインを迅速かつ効率的に実施できます。最小権限モデルでは、ユーザーにはジョブの実行に必要な権限とアクセス権のみが与えられます。多くの場合、ユーザーは異なるタスクを実行しますが、すべてのユーザーに同じ強力な権限のセットが付与されます。権限分析を行わないと、各ユーザーが持つ必要のある権限を把握することは困難な作業になる可能性があり、多くの場合、ユーザーはタスクが異なる場合でも、共通の権限セットを使用できます。権限を管理する組織でも、ユーザーは時間の経過とともに権限を蓄積し、権限を失うことはほとんどありません。職務を分離すると、1つのプロセスがユーザーごとに別々のタスクに分割されます。最小限の権限によって分離が強制されるため、ユーザーは必要なタスクのみを実行できます。職務の分離の実施は、内部統制に有益ですが、特権資格証明を盗んだ悪意のあるユーザーからのリスクも軽減します。

権限分析は、実行時にデータベース・ユーザーおよびアプリケーションによって使用される権限を取得し、その結果を問合せ可能なデータ・ディクショナリ・ビューに書き込みます。アプリケーションに定義者の権利および実行者の権利プロシージャが含まれている場合、権限分析では、権限取得が作成され有効化される前にプロシージャがコンパイルされていた場合でも、プロシージャのコンパイルと実行に必要な権限が取得されます。

様々なタイプの権限分析ポリシーを作成して、特定の目的を実現できます。

*   **ロールベースの権限使用の取得**
    
        You must provide a list of roles. If the roles in the list are enabled in the database session, then the used privileges for the session will be captured. You can capture privilege use for the following types of roles: Oracle default roles, user-created roles, Code Based Access Control (CBAC) roles, and secure application roles.
        
*   **コンテキストベースの権限使用の取得**
    
        You must specify a Boolean expression only with the `SYS_CONTEXT` function. The used privileges will be captured if the condition evaluates to `TRUE`. This method can be used to capture privileges and roles used by a database user by specifying the user in `SYS_CONTEXT`.
        
*   **ロールおよびコンテキストベースの権限使用の取得**
    
        You must provide both a list of roles that are enabled and a `SYS_CONTEXT` Boolean expression for the condition. When any of these roles is enabled in a session and the given context condition is satisfied, then privilege analysis starts capturing the privilege use.
        
*   **データベース全体の権限の取得**
    
        If you do not specify any type in your privilege analysis policy, then the used privileges in the database will be captured, except those for the user `SYS`. (This is also referred to as unconditional analysis, because it is turned on without any conditions.)
        

### **権限分析を使用する利点**

*   不必要に付与された権限の確認
*   最小権限のベスト・プラクティスの実装: データベースにアクセスするアカウントの権限は、アプリケーションまたはユーザーが厳密に必要とする権限に制限する必要があります。
*   セキュアなアプリケーションの開発: アプリケーションの開発フェーズで、多くの強力なシステム権限およびロールをアプリケーション開発者に付与する管理者もいます。
*   マルチテナント環境で権限分析ポリシーを作成および使用できます。
*   Can be used to capture the privileges that have been exercised on pre-compiled database objects (PL/SQL packages, procedures, functions, views, triggers, and Java classes and data)

## 詳細

技術文書:

*   [Oracle Privilege分析19c](https://docs.oracle.com/en/database/oracle/oracle-database/19/dbseg/performing-privilege-analysis-find-privilege-use.html#GUID-44CB644B-7B59-4B3B-B375-9F9B96F60186)

ビデオ:

*   _権限分析の理解(2019年1月)_[](youtube:3oRODVtWwbg)

## 謝辞

*   **著者** - Database Security PM、Hakim Loumi氏
*   **貢献者** - Richard Evans
*   **最終更新者/日付** - Hakim Loumi、データベース・セキュリティPM - 2023年11月