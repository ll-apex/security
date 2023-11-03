# アクセス制御

## 概要

この演習では、アクセス・ガバナンスのアクセス制御を確認します

_予想時間_: 15分

ラボのクイック・ウォークスルーについては、次のビデオをご覧ください。[サイジングのないOracle Video Hubのビデオ](videohub:1_ruy3z0dh)

### 目標

この演習では、次のことを行います。

*   アイデンティティ・コレクションの作成
*   パラレル・エスカレーション・ルールによる承認ワークフローの作成
*   アクセス・バンドルの作成
*   アクセス権限をプロビジョニングするための一元化されたポリシーの作成

## タスク1: アイデンティティ・コレクションの作成- IT管理

1.  Access Governanceコンソールのホームページで、「アクセス制御」タブをクリックします。次に、「アイデンティティ・コレクション」タイルで「選択」をクリックします。
    
    ![アイデンティティ・コレクションの作成](images/ag-homepage.png)
    
    ![アイデンティティ・コレクションの作成](images/identity-collections.png)
    
2.  「アイデンティティ・コレクション」ページに、作成したアイデンティティ・コレクションがここにリストされます。「アイデンティティ・コレクションの作成」をクリックして、アイデンティティ・コレクションを作成します。
    
    ![アイデンティティ・コレクションの作成](images/create-identity-collection.png)
    
3.  「詳細の追加」タブで、次の詳細を入力します。
    
    このアイデンティティ・コレクションにどのような名前を付けますか。: IT管理
    
    このコレクションをどのように説明しますか: IT管理
    
    誰がこのアイデンティティ・コレクションを管理できますか: pamela.green
    
    タグを追加しますか? : 承認者
    
    _「次へ」_をクリックします。
    
    ![アイデンティティ・コレクションの作成](images/create-workflow.png)
    
4.  「Select Identities」→「Included named identities」で、次のオプションを選択します。
    
    _「Pamela Green」_を選択します。
    
    _「次へ」_をクリックします。
    
    ![アイデンティティ・コレクションの作成](images/include-identities.png)
    
5.  _「作成」_をクリックします
    

## タスク2: アイデンティティ・コレクションの作成- 品質保証

1.  Access Governanceコンソールのホームページで、「アクセス制御」タブをクリックします。次に、「アイデンティティ・コレクション」タイルで「選択」をクリックします。
    
2.  「アイデンティティ・コレクション」ページに、作成したアイデンティティ・コレクションがここにリストされます。「アイデンティティ・コレクションの作成」をクリックして、アイデンティティ・コレクションを作成します。
    
    ![アイデンティティ・コレクションの作成](images/create-identity-collection.png)
    
3.  「詳細の追加」タブで、次の詳細を入力します。
    
    このアイデンティティ・コレクションにどのような名前を付けますか。: QAチーム
    
    このコレクションをどのように説明しますか? : QAチーム
    
    誰がこのアイデンティティ・コレクションを管理できますか: pamela.green
    
    タグを追加しますか。: DB、データベース、操作
    
    _「次へ」_をクリックします。
    
    ![アイデンティティ・コレクションの作成](images/qa-collection.png)
    
4.  「アイデンティティの選択」→「メンバーシップ・ルール」で、次のオプションを選択します。
    
    _「すべて」_チェックボックスを選択します。
    
    属性の選択: 組織
    
    条件: 等しい
    
    属性フィールド: 品質保証
    
    ![アイデンティティ・コレクションの作成](images/qa-rule.png)
    
5.  _「作成」_をクリックします
    
    ![アイデンティティ・コレクションの作成](images/qa-collection-create.png)
    

## タスク3: 承認ワークフローの作成

1.  Access Governanceコンソールのホームページで、「アクセス制御」タブをクリックします。次に、「Manage Approval Workflows(承認ワークフローの管理)」タイルの「Select(選択)」をクリックします。
    
    ![承認ワークフロー](images/ag-homepage.png)
    
2.  「承認ワークフロー」ページに、作成した承認ワークフローがここにリストされます。「承認ワークフローの作成」をクリックして、最初の承認ワークフローを作成します。
    
    ![承認ワークフロー](images/create-approval-workflow.png)
    
3.  今すぐ承認ワークフローを構築しましょう。「+」ボタンをクリックし、次に基づいて承認ワークフローを構成します。
    
    • どのタイプの承認?: アイデンティティ収集
    
    • 承認アイデンティティ収集: IT管理
    
    • すべてのメンバーが承認する必要がありますか? : いいえ
    
    •他のすべてのフィールドについては、既定のまま
    
    • 「追加」をクリックします
    
    ![承認ワークフロー](images/approval-workflow-id.png)
    
    ![承認ワークフロー](images/approval-workflow-edit.png)
    
    構成が次と一致することを確認したら、「次」をクリックします
    
4.  「詳細の追加」ページで、「承認ワークフロー: 承認- ワークフロー-IT- 管理」という名前を付けます。次に、説明を入力します。「次へ」をクリックしてこれまでに構成を確認してから、「公開」をクリックします。
    
    ![承認ワークフロー](images/approval-workflow-name.png)
    
    ![承認ワークフロー](images/approval-workflow-publish.png)
    

## タスク4: アクセス・バンドルの作成

1.  Access Governanceコンソールのホームページで、「アクセス制御」タブをクリックします。次に、「Access Bundles」タイルの「Select」をクリックします。
    
    ![アクセス・バンドルの作成](images/navigate-access-bundle.png)
    
2.  「Create a access bundle - DB Read Access」をクリックします。
    
    ![アクセス・バンドルの作成](images/create-access-bundle.png)
    
3.  バンドル設定の場合は、次と一致するようにバンドルを構成します。
    
    • このバンドルのターゲットは何ですか?: OAG-DB
    
    • 誰がこのバンドルをリクエストできますか?: 誰か
    
    • どの承認ワークフローを使用しますか?: 承認- ワークフロー-IT- 管理
    
    「次へ」をクリックします。
    
    ![アクセス・バンドルの作成](images/click-next.png)
    
4.  アクセス・バンドルに含める権限を選択します。
    
    どの権限をこのバンドルに含まれますか? : リストからアクセス・バンドルに含める次のものを選択してください。
    
    *   任意のファイル・グループの読取り
        
    *   任意の表の読取り
        
    
    ![アクセス・バンドルの作成](images/read-permissions.png)
    
    「次へ」をクリックします。
    
5.  「Add Details」ステップで、次を構成します。
    
    • このバンドルの名前は何ですか?: DB読取りアクセス
    
    • このバンドルをどのように説明しますか?: DB読取りアクセス
    
    •認証タイプ: パスワード
    
    • デフォルト表領域: USERS
    
    • TemporaryTablespace: 一時
    
    • プロファイル名: DEFAULT
    
    •他のすべてのオプションはデフォルトのまま
    
    ![アクセス・バンドルの作成](images/db-read.png)
    
    その後、「次へ」をクリックします。
    
6.  この時点までに作成した構成を確認します。名前以外は、次に示す構成のようになります。次に、「作成」をクリックします。
    
    ![アクセス・バンドルの作成](images/db-read-create.png)
    
7.  「Create a access bundle - DB Manage Access」をクリックします。
    
    ![アクセス・バンドルの作成](images/create-access-bundle.png)
    
8.  バンドル設定の場合は、次と一致するようにバンドルを構成します。
    
    • このバンドルのターゲットは何ですか?: OAG-DB
    
    • 誰がこのバンドルをリクエストできますか?: 誰か
    
    • どの承認ワークフローを使用しますか?: 承認- ワークフロー-IT- 管理
    
    「次へ」をクリックします。
    
9.  アクセス・バンドルに含める権限を選択します。
    
    どの権限をこのバンドルに含まれますか? : リストからアクセス・バンドルに含める次のものを選択してください。
    
    *   任意のSQLチューニング・セットの管理
        
    *   データベース・トリガーの管理
        
    *   KEY MANAGEMENTの管理
        
    *   リソース・マネージャの管理
        
    *   SQL管理オブジェクトの管理
        
    *   SQLチューニング・セットの管理
        
    
    ![アクセス・バンドルの作成](images/administer-permission.png)
    
    「次へ」をクリックします。
    
10.  「Add Details」ステップで、次を構成します。
    
    • このバンドルの名前は何ですか?: DBアクセスの管理
    
    • このバンドルをどのように説明しますか?: DBアクセスの管理
    
    •認証タイプ: パスワード
    
    • デフォルト表領域: USERS
    
    • TemporaryTablespace: 一時
    
    • プロファイル名: DEFAULT
    
    •他のすべてのオプションはデフォルトのまま
    
    ![アクセス・バンドルの作成](images/db-manage-access.png)
    
    その後、「次へ」をクリックします。
    
11.  この時点までに作成した構成を確認します。名前以外は、次に示す構成のようになります。次に、「作成」をクリックします。
    
    ![アクセス・バンドルの作成](images/create-db-manage-access.png)
    

## タスク5: アクセス権限をプロビジョニングするための集中ポリシーの作成

1.  Access Governanceコンソールのホームページで、「アクセス制御」タブをクリックします。次に、「ポリシー」タイルの「選択」をクリックします。
    
    ![ポリシーの作成](images/ag-homepage.png)
    
    ![ポリシーの作成](images/navigate-policies.png)
    
2.  「ポリシー」ページに、作成したポリシーのリストが表示されます。「Create a policy」をクリックします。
    
    ![ポリシーの作成](images/create-policy.png)
    
3.  ポリシーに次のような名前および説明を指定します。
    
    • このポリシーにどのような名前を付けますか?:DBポリシー
    
    • このポリシーをどのように説明しますか: DBポリシー
    
    ![ポリシーの作成](images/build-policy.png)
    
4.  この同じページで、アクセス・バンドル・アソシエーションを追加します。ページの下部で「+」ボタンをクリックし、「Access Bundle Association」を選択します。
    
    ![ポリシーの作成](images/policy-access-bundle-association.png)
    
5.  アクセスを許可するアイデンティティ・コレクションを検索します: QAチーム。選択には緑色のチェックマークが付きます。「次へ」をクリックします。
    
    ![ポリシーの作成](images/select-qa.png)
    
6.  割り当てるアクセス・バンドルを検索します: DB読取りアクセス。選択には緑色のチェックマークが付きます。
    
    ![ポリシーの作成](images/selet-db-read-access.png)
    
    その後、「次へ」をクリックします。
    
7.  「Review and submit」ページで、作成前に右下隅にある「Preview policy association」をクリックできます。その後、そのサイドバーを閉じ、「アソシエーションの追加」をクリックします。
    
    ![ポリシーの作成](images/click-add-association.png)
    
8.  最後に、「Create」をクリックします。
    

## タスク6: 手動データロードの実行

1.  Access Governanceコンソールのホームページで、「サービス管理」→「接続済システム」に移動します。
    
    ![ポリシーの作成](images/connected-system.png)
    
2.  「接続システム」ページで、**OAG-DB**接続システムを選択します。
    
    ![ポリシーの作成](images/select-system.png)
    
3.  「アクション」→「データを今すぐロード」をクリックします。これにより、手動のデータ・ロードが実行されます。
    
    ![ポリシーの作成](images/load-date.png)
    
4.  データ・ロードが完了すると、ステータスは「成功」として表示されます。
    
    **次の演習に進みます。**
    

## さらに学ぶ

*   [Oracle Access Governanceアクセス・レビューの作成キャンペーン](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance製品ページ](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governanceの製品ツアー](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access GovernanceのFAQ](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## 謝辞

*   **著者** - Anuj Tripathi、Indira Balasundaram、Anbu Anbarasu