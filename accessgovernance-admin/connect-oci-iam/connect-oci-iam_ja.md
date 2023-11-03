# Oracle Access GovernanceとOCI IAMの統合

## 概要

**アクセス・ガバナンス管理者**は、Oracle Access GovernanceとOCI IAMの統合を学ぶことができます。

*   Persona: アクセス・ガバナンス管理者

_予想時間_: 15分

ラボのクイック・ウォークスルーについては、次のビデオをご覧ください。[サイジングのないOracle Video Hubのビデオ](videohub:1_cupvwe5w)

### 目標

この演習では、次のことを行います。

*   Oracle Access Governanceコンソールでの新しいOCI IAM Cloud Service接続の構成

## タスク1: Oracle Access Governanceコンソールでの新しいOCI IAM Cloud Service接続の構成

1.  ブラウザで、_ラボ3: タスク1_に記載されているURLを使用してOracle Access Governanceサービスのホーム・ページに移動し、**アクセス・ガバナンス管理者**アプリケーション・ロールを持つユーザーとしてログインします。

Oracle Access Governanceキャンペーン管理者のユーザー名とパスワードの入力(Pamela Green)

    **Username:**
    ```
    <copy>pamela.green</copy>
    ```
    
    **Password:**
    ```
    <copy>Oracl@123456</copy>
    ```
    

2.  Oracle Access Governanceサービスのホームページで、ナビゲーション・メニュー・アイコンをクリックし、**「サービス管理」→「接続済システム」**を選択します。
    
3.  「接続されたシステム」ページから**「接続されたシステムの追加」**ボタンを選択します。
    
    ![クラウド・サービス・プロバイダを選択します](images/add-system.png)
    
4.  「追加」ボタンをクリックして、**「クラウド・サービス・プロバイダに接続しますか。」**タイルを選択します。
    

![クラウド・サービス・プロバイダを選択します](images/select-cloud-provider.png)

5.  **「システムの選択」**ステップで、**「Oracle Cloud Infrastructure」**タイルを選択し、**「次へ」**をクリックします。

![クラウド・サービス・プロバイダを選択します](images/select-oci.png)

6.  接続されているシステムの名前と説明を入力し、**「次」**をクリックします。

名前: OCI-IAM

説明: OCI-IAM

![OCI詳細の入力](images/enter-oci-system-name.png)

![OCI詳細の入力](images/enter-data.png)

7.  OCIユーザー(agcs-user)のフィンガープリントを取得します。**新しいプライベート・ブラウザ・ウィンドウ**を開き、**ドメイン管理者**としてOCIコンソールの**デフォルト・ドメイン**にログインします。
    
8.  OCIコンソールで、左上のナビゲーション・メニュー・アイコンをクリックして、_「ナビゲーション」メニューを表示します。_ _「ナビゲーション」メニュー_の_「アイデンティティとセキュリティ」_をクリックします。製品のリストから_「ドメイン」_を選択します。
    
    ![ドメインに移動](images/navigate-domains.png)
    
9.  「ドメイン」ページで、_「デフォルト・ドメイン」_をクリックします。**「ルート・コンパートメント」**が選択されていることを確認します。
    
    ![ドメインに移動](images/default-domain.png)
    
10.  _「ユーザー」_を選択します。_agcs-user_をクリックします。
    

![ユーザーの作成](images/select-users.png)

11.  スクロールして、**APIキー**をクリックします

![OCI詳細の入力](images/api.png)

12.  **「APIキーの追加」**をクリックします。**「APIキー・ペアの生成」**をクリックします。
    
    ![OCI詳細の入力](images/add-api-key.png)
    
13.  「**Download private key**」および「**Download public key**」をクリックします。
    

![OCI詳細の入力](images/click-add.png)

14.  **「追加」**をクリックします。
    
15.  テキスト・エディタで**ダウンロードした秘密キー**を書き留めます。これは次のステップで必要です。
    
16.  **「構成ファイルのプレビュー」**で、次のステップに必要な次の詳細を書き留めます。
    
    *   ユーザーOCIDID
    *   指紋
    *   テナントOCID
    *   領域
    
    ![OCI詳細の入力](images/config-file.png)
    
17.  Oracle Access Governanceを使用してブラウザに戻り、次に説明する次の詳細を入力します。
    
    **OCIユーザーのOCIDとは何ですか。**: 前のステップで書き留めたOCIユーザー(agcs-user)のOracle Cloud Identifier (OCID)を入力します。
    
    **OCIユーザーのフィンガープリントとは**: 前のステップで書き留めたAPI署名キーの公開キーのフィンガープリントを入力します。
    
    **OCIユーザーの秘密SSHキーとは何ですか。**: API署名キーの前のステップからダウンロードした秘密SSHキー(.pemファイル)を入力します。
    
    **OCIテナンシOCIDとは何ですか。**: 前のステップで書き留めたターゲット・テナンシのOCIDを入力します。
    
    **OCIテナンシのホーム・リージョンとは何ですか。**: 前のステップで示したリージョン識別子を使用して、ターゲットOCIテナンシのホーム・リージョンを入力します。
    
    ![OCI詳細の入力](images/details-entered.png)
    
18.  **「追加」をクリックします。**「管理」をクリックしてステータスを表示します。接続の詳細が正常に検証されると、**「検証」**操作の**「成功」**ステータスが表示されます。OCIテナンシで使用可能なデータによっては、フル・データ・ロード操作に数分かかる場合があります。増分データ・ロードは、この接続されたシステムがデータを同期するために4時間ごとに実行されます。
    

![OCI接続ステータス](images/oci-connection-status.png)

**次の演習に進む**ことができます。

## さらに学ぶ

*   [Oracle Access Governanceアクセス・レビューの作成キャンペーン](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance製品ページ](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governanceの製品ツアー](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access GovernanceのFAQ](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## 確認

*   **著者** - Anuj Tripathi、Indira Balasundaram、Anbu Anbarasu