# アクセス・レビュー・タスクの実行

## 概要

アクセス・レビューは、Oracle Access Governanceコンソールからユーザー(Mark Hernandez、Harlan Bullard、Jerry Poland)によって実行でき、Access Governance管理者(Pamela Green)によって確認されます。

*   ペルソナ: キャンペーン・レビューアおよびキャンペーン管理者

_予想時間_: 15分

ラボのクイック・ウォークスルーについては、次のビデオをご覧ください。[サイジングのないOracle Video Hubのビデオ](videohub:1_0sz90jrj)

### 目標

この演習では、次のことを行います。

*   アクセス・ガバナンス・ユーザーとしてアクセス・レビュー要求を作成
*   キャンペーン管理者としてキャンペーンを作成
*   アクセス・ガバナンス・キャンペーン管理者としてアクセス・レビュー要求を承認

## タスク1: Oracle Access Governanceを従業員ユーザーとしてログインしてアクセス・リクエストを作成

1.  従業員ユーザーとしてOracle Access Governanceにログインします- 次のユーザー名とパスワードでMark Hernandez。
    
    **ユーザー名:**
    
        <copy>mhernandez</copy>
        
    
    **パスワード:**
    
        <copy>Oracl@123456</copy>
        

Oracle Access Governanceコンソールのホーム・ページに移動します。

3.  Oracle Access Governanceコンソールのホームページで、ナビゲーション・メニューから**「自分の作業」→「新規アクセスのリクエスト」**を選択します。
    
    ![アクセス・レビュー](images/navigate-access.png)
    
4.  **「どのアクセスを要求していますか。」**で、**「どのアクセスが必要ですか。」**を選択します。
    
    ![アクセス・レビュー](images/which-access.png)
    
5.  新しいアクセスを要求する-> \[はい\]を選択します。**「次へ」**をクリックします
    
    ![アクセス・レビュー](images/click-yes.png)
    
6.  「アクセスする対象」で、**「DB-Manage-Access」**を選択し、**「次へ」**をクリックします。
    

    ![Access Review](images/select-db-manage-access.png)
    

7.  「このリクエストについて追加情報が必要です」->「理由を指定してください」
    
    ![アクセス・レビュー](images/provide-justification.png)
    
8.  **「リクエストの発行」**をクリックします
    
9.  従業員ユーザーとしてOracle Access Governanceにログインします。Harlan Bullardは、次に説明するユーザー名とパスワードを使用してログインします。
    

    **Username:**
    ```
    <copy>harlan.bullard</copy>
    ```
    
    **Password:**
    ```
    <copy>Oracl@123456</copy>
    ```
    

Oracle Access Governanceコンソールのホーム・ページに移動します。

10.  Oracle Access Governanceコンソールのホームページで、ナビゲーション・メニューから**「自分の作業」→「新規アクセスのリクエスト」**を選択します。
    
11.  **「どのアクセスを要求していますか。」**で、**「どのアクセスが必要ですか。」**を選択します。
    
12.  新しいアクセスを要求する-> \[はい\]を選択します。**「次へ」**をクリックします
    
13.  「アクセスする対象」で、**「DB-Manage-Access」**を選択し、**「次へ」**をクリックします。
    
14.  「このリクエストについて追加情報が必要です」->「理由を指定してください」
    
15.  **「リクエストの発行」**をクリックします
    
16.  従業員ユーザーとしてOracle Access Governanceにログインします。Jerry Polandは、次に説明するユーザー名とパスワードでログインします。
    

    **Username:**
    ```
    <copy>jerry.poland</copy>
    ```
    
    **Password:**
    ```
    <copy>Oracl@123456</copy>
    ```
    

Oracle Access Governanceコンソールのホーム・ページに移動します。

17.  Oracle Access Governanceコンソールのホームページで、ナビゲーション・メニューから**「自分の作業」→「新規アクセスのリクエスト」**を選択します。
    
18.  **「どのアクセスを要求していますか。」**で、**「どのアクセスが必要ですか。」**を選択します。
    
19.  新しいアクセスを要求する-> \[はい\]を選択します。**「次へ」**をクリックします
    
20.  「アクセスする対象」で、**「DB-Manage-Access」**を選択し、**「次へ」**をクリックします。
    
21.  「このリクエストについて追加情報が必要です」->「理由を指定してください」
    
22.  **「リクエストの発行」**をクリックします
    

## タスク2: アクセス・リクエストを承認するためのアクセス・ガバナンス管理者としてのOracle Access Governanceへのログイン

1.  従業員ユーザーとしてOracle Access Governanceにログインします。Pamela Greenは、次に説明するユーザー名とパスワードを使用します。
    
    **ユーザー名:**
    
        <copy>pamela.green</copy>
        
    
    **パスワード:**
    
        <copy>Oracl@123456</copy>
        

Oracle Access Governanceコンソールのホーム・ページに移動します。

2.  MyStuff→Approvals.Youにナビゲートすると、**DB-Manage-Access**のユーザーHarlan Bulllard、Mark HernandezおよびJerry Polandからのリクエストが表示されます。
    
3.  「処理」で、ユーザーHarlan Bullard、Mark HernandezおよびJerry Polandの要求を承認および承認します。
    

## タスク3: 手動データロードの実行

1.  Access Governanceコンソールのホームページで、「サービス管理」→「接続済システム」に移動します。
    
    ![ポリシーの作成](images/connected-system.png)
    
2.  「接続システム」ページで、**OAG-DB**接続システムを選択します。
    
    ![ポリシーの作成](images/select-system.png)
    
3.  「アクション」→「データを今すぐロード」をクリックします。これにより、手動のデータ・ロードが実行されます。
    
    ![ポリシーの作成](images/load-date.png)
    
4.  データ・ロードが完了すると、ステータスは「成功」として表示されます。
    

## タスク4: Oracle Access Governanceをアクセス・ガバナンス管理者としてログインしてキャンペーンを作成

1.  従業員ユーザーとしてOracle Access Governanceにログインします。Pamela Greenは、次に説明するユーザー名とパスワードを使用します。
    
    **ユーザー名:**
    
        <copy>pamela.green</copy>
        
    
    **パスワード:**
    
        <copy>Oracl@123456</copy>
        
2.  「アクセス・レビュー」→「キャンペーン」にナビゲートします。**「キャンペーンの作成」**をクリックします
    

![アクセス・レビュー](images/navigate-campaigns.png)

![アクセス・レビュー](images/create-campaign.png)

3.  どのタイプのアクセス・レビュー・キャンペーンを実行しますか。-> **「Access Governanceによって管理されるシステムの確認」**を選択します。

![アクセス・レビュー](images/access-review-ag.png)

4.  **「アクセス権があるユーザー」**を選択します。組織**品質保証**を検索します。**「選択の適用」**をクリックします。

![アクセス・レビュー](images/who-has-access.png)

![アクセス・レビュー](images/quality-assurance.png)

5.  **「I'm good to go with the workflow」**をクリックします。

![アクセス・レビュー](images/select-workflow.png)

6.  **「独自のワークフローを選択」**を選択します。**「次へ」**をクリックします。
    
7.  **「いくつのレベルの承認が必要ですか。」**→1レベルの承認ワークフローを選択します。
    
8.  **「最初のレビューアは誰ですか。」** ->「所有者」を選択します。「保存」をクリックして「次へ」をクリックします。
    

![アクセス・レビュー](images/edit-workflow.png)

9.  **「キャンペーンのスケジュール方法」**で、**「今すぐ実行」**を選択します。**「このキャンペーンをどのように記述しますか。」**を指定し、**「次へ」**をクリックします。

![アクセス・レビュー](images/create-workflow.png)

10.  「作成」をクリックします。キャンペーンが作成されました。
    
11.  作成されたキャンペーンを表示するには、「アクセス・レビュー」→「キャンペーン」にナビゲートします。
    

![アクセス・レビュー](images/campaign-name.png)

12.  詳細を表示するには、キャンペーンをクリックします。

![アクセス・レビュー](images/view-campaign.png)

13.  「追加詳細」をクリックして、作成されたキャンペーンの詳細情報を表示します。

![アクセス・レビュー](images/campaign-details.png)

14.  「アクセス・レビュー」→「自分のアクセス・レビュー」にナビゲートします。

![アクセス・レビュー](images/view-access-review-request.png)

15.  ユーザーHarlan Bulllard、Mark HernandezおよびJerry Poland for DB-Manage-Accessからのリクエストが表示されます。ビューをクリックして、要求のインサイトをレビューします。

![アクセス・レビュー](images/harlan-user.png)

![アクセス・レビュー](images/jerry-user.png)

![アクセス・レビュー](images/mark-user.png)

16.  「Actions」で、「Accept」をクリックし、ユーザーHarlan Bullard、Mark HernandezおよびJerry Polandの要求を受け入れます。

**次の演習に進む**ことができます。

## さらに学ぶ

*   [Oracle Access Governanceアクセス・レビューの作成キャンペーン](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance製品ページ](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governanceの製品ツアー](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access GovernanceのFAQ](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## 確認

*   **著者** - Anuj Tripathi、Indira Balasundaram、Anbu Anbarasu
*   **最終更新者/日付** - Anbu Anbarasu、2023年7月