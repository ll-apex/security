# ポリシー・レビュー・キャンペーンの作成およびレビュー

## 概要

Access Governance管理者(Pamela Green)は、ポリシー・レビュー・キャンペーンを作成およびレビューできます。

*   Persona: アクセス・ガバナンス管理者

_予想時間_: 15分

ラボのクイック・ウォークスルーについては、次のビデオをご覧ください。[サイジングのないOracle Video Hubのビデオ](videohub:1_yivv30ww)

### 目標

この演習では、次のことを行います。

*   OCI IAMポリシーのポリシー・レビュー・キャンペーンの作成
*   キャンペーンで発生したポリシー・レビュー・タスクの確認
*   キャンペーン・レビューアとして割り当てられたポリシー・レビュー・タスクの評価

## タスク1: ポリシー・レビュー・キャンペーンの作成

1.  ブラウザから、Oracle Access Governanceコンソールに移動します。
    
2.  **Oracle Access Governance管理者**のユーザー名とパスワードを入力します(Pamela Green)
    
    **ユーザー名:**
    
        <copy>pamela.green</copy>
        
    
    **パスワード:**
    
        <copy>Oracl@123456</copy>
        

Oracle Access Governanceコンソールのホーム・ページに移動します。

3.  Oracle Access Governanceのホーム・ページから、**「アクセス・レビュー」**→**「キャンペーン」**にナビゲートします。

![Access Governanceホームページ](images/navigate-campaign.png)

4.  **「キャンペーンの作成」**をクリックします

![Access Governanceホームページ](images/create-a-campaign.png)

5.  **「どのタイプのアクセス・レビュー・キャンペーンを実行しますか。」**で、**Oracle Cloud Infrastructure**へのレビュー・アクセスを選択します
    
    ![Access Governanceホームページ](images/select-oci-campaign.png)
    
6.  「選択基準」ステップで、**「どのテナンシ?」**タイルを選択します。使用可能なクラウド・テナンシのリストが表示されます。
    
    ![クラウド・プロバイダを選択します](images/select-tenancies.png)
    
7.  適切なクラウド・テナンシを選択します。このチュートリアルでは、クラウド・テナンシを選択します。選択に対して緑のティックがマークされます。
    
    ![クラウド・プロバイダを選択します](images/which-tenancy.png)
    
8.  「**Refine further**」をクリックします。特定のコンパートメントとドメインを選択して選択をさらに絞り込み、ドメイン固有のポリシー・レビューを実行できます。
    
    ![クラウド・プロバイダを選択します](images/click-refine.png)
    
9.  次に示す**コンパートメント**の詳細を入力し、**「適用」**をクリックします
    
    コンパートメント: ag-compartment
    
    ![クラウド・プロバイダを選択します](images/ag-compartment.png)
    
10.  次のステップに進み、確認するポリシーを選択します。**「ポリシー」**タイルを選択します。選択したドメインで使用可能なポリシーのリストが表示されます。
    
    ![Access Governanceホームページ](images/select-which-policies.png)
    
11.  確認するポリシーを選択します。このチュートリアルでは、次のポリシーを選択し、**「選択の適用」**をクリックします。
    
          - auditors-policy
          - network-admins-policy
          - security-admins-policy
        
    
    ![Access Governanceホームページ](images/select-the-policies.png)
    
12.  **「ワークフローの割当て」**ステップに進みます。これを行うには、**「I'm good、 go to workflows」をクリックします。**ここで、レビュー・タスクの承認ワークフローを定義し、**「次へ」**をクリックします。
    
    ![Access Governanceホームページ](images/choose-workflow.png)
    
    ![Access Governanceホームページ](images/click-next-workflow.png)
    
13.  **「詳細の追加」**ステップでは、アクセス・レビュー・キャンペーンを実行する頻度(1回または定期)を定義し、キャンペーンに意味のある名前を付け、補足説明を追加し、その所有者やキャンペーンを開始または終了する時期などの追加属性に値を割り当てることができます。
    
14.  このチュートリアルでは、**「詳細の追加」**ステップで次の変更を行います:
    
          **How often do you want this to run?** : One time
        
          **What do you want to call this campaign?**: Policy-Review-OCI-IAM
        
          **How do you want to describe this campaign?**: Policy-Review-OCI-IAM
        
          **Who owns this campaign?**: Me
        
          **How would you like to schedule your campaign?** : Run now (will start 10 minutes from creation)
        
15.  **「次へ」**をクリックします。
    
    ![Access Governanceホームページ](images/campaign-information.png)
    
16.  **「確認および送信」**ステップには、前のステップで追加した情報が表示されます。**「作成」**を選択してキャンペーンを作成します。キャンペーンがスケジュールされ、**「キャンペーン」**ページに表示されます。作成から10分後に実行されます。
    
    ![OCI詳細の入力](images/click-create-new-campaign.png)
    
    ![OCI詳細の入力](images/campaign-scheduled.png)
    

## タスク2: ポリシー・レビュー・タスクの実行

ここでは、前のタスクで作成されたキャンペーンによって発生したOCI IAMレビュー・タスクをレビューおよび証明します。

1.  ブラウザから、Oracle Access Governanceコンソールに移動します。
    
2.  **Oracle Access Governance Campaign Reviewer**のユーザー名とパスワード(Pamela Green)を入力します
    
    **ユーザー名:**
    
        <copy>pamela.green</copy>
        
    
    **パスワード:**
    
        <copy>Oracl@123456</copy>
        

Oracle Access Governanceコンソールのホーム・ページに移動します。

3.  Oracle Access Governanceコンソールのホームページで、ナビゲーション・メニューから**「アクセス・レビュー」→「自分のアクセス・レビュー」**を選択します。
    
4.  ポリシー・レビュー・キャンペーンによって作成されたレビュー・タスクを表示するには、**「アクセス制御レビュー・タスク」**タブをクリックします。レビューアとして割り当てられたすべてのポリシー・アクセス・レビュー・タスクが表示されます。Oracle Access Governanceは、社内の分析ベースのインテリジェンス・システムを使用して、承認/レビューの推奨事項を提供します。
    

![Access Governanceホームページ](images/access-control-review-tasks.png)

5.  このチュートリアルでは、Oracle Access Governanceが提供する推奨事項を確認します。

*   network-admins-policyはレビュー用にマークされています
*   security-admins-policyはレビュー用にマークされています
*   auditors-policyはAcceptとしてマークされています

6.  Oracle Access Governanceによって生成されたインサイトを確認します。**auditors-policy**の場合、**「インサイト」**列の下の対応する**「アクション」**リンクをクリックします。
    
7.  **「インサイト」**ページで、ポリシー・レビュー・タスクの推奨事項を表示できます。左側のパネルで、ポリシー情報を表示できます。右側には、アクション可能なポリシー・ステートメントと非アクション可能なポリシー・ステートメントの完全なリストを表示し、ポリシー詳細を表示して、ポリシー・ステートメントがアクセス権を付与しているユーザーとその内容を確認し、各ステートメントで適切な意思決定を行うことができます。
    

    ![Access Governance Homepage](images/view-policy.png)
    

8.  ポリシー・ステートメントの横にある「表示」ボタンをクリックして、ポリシーに関連付けられているリソースを表示します。「要約」および「詳細」をクリックして情報を表示します。「閉じる」をクリックします。
    
    ![Access Governanceホームページ](images/summary.png)
    
    ![Access Governanceホームページ](images/details.png)
    
9.  レビューを決定するには、そのポリシー内のすべてのアクション可能な文を一度に取り消すか、または各ポリシー・ステートメントについて個別に決定します。このチュートリアルでは、2つのユースケースを検証します。
    

    **Usecase 1:**  Revoke policy statement from a policy - **auditors-policy**
    
      - Let’s revoke the policy statement **Allow group ag-domain/Auditors to read audit-events in compartment ag-compartment** from the policy  **auditors-policy**. 
    
      ![Access Governance Homepage](images/auditor-policy.png)
    
      - Click on the cross button under Actions column for the policy statement **Allow group ag-domain/Auditors to read audit-events in compartment ag-compartment**
    
      ![Access Governance Homepage](images/click-revoke-auditor-policy.png)
    
      - Click on **Apply**
    
      ![Access Governance Homepage](images/click-apply-auditor-policy.png)
    
      -  Enter justification for why you revoke the access review items and accept the remaining items then click on **Submit** This will trigger the auto-remediation process in the Oracle Access Governance system.
    
      ![Access Governance Homepage](images/policy-review-list-new.png)
    
    
    **Usecase 2:**  Revoke an entire policy - **security-admins-policy** 
    
      - Let's revoke the entire policy **security-admins-policy** 
    
       ![Access Governance Homepage](images/security-policy.png)
    
      - Click on the Revoke All button. 
    
       ![Access Governance Homepage](images/revoke-all-security-policy.png)
    
      - Click **Apply.** The **Confirmation** dialog box is displayed.
    
        ![Access Governance Homepage](images/apply-security-policy.png)
    
     - Provide justification and then click **Submit.** The closed loop access remediation will take place automatically.
    

10.  (オプション)アイデンティティ・ドメイン: ag-domain OCIコンソールにIdentity Domain Administratorとしてログインします。「アイデンティティとセキュリティ」->「アイデンティティ」->「ポリシー」に移動します。

*   ポリシー全体を確認します- ポリシーのリストから **security-admins-policy**が正常に取り消されました。

ポリシー・レビューを実行する前に:

![Access Governanceホームページ](images/before-security-policy.png)

ポリシー・レビューの実行後:

![Access Governanceホームページ](images/after-security-policy.png)

*   ポリシー・ステートメント- ポリシーの**コンパートメントag-compartment内の監査イベントを読み取ることをグループag-domain/Auditorsに許可** - **auditors-policy**が正常に取り消されたことを確認します。

ポリシー・レビューを実行する前に:

![Access Governanceホームページ](images/before-auditor-policy.png)

ポリシー・レビューの実行後:

![Access Governanceホームページ](images/after-auditor-policy.png)

これで、OCI IAMポリシー・レビューの作成および実行に関するチュートリアルは終了です。

**次の演習に進む**ことができます。

## さらに学ぶ

*   [Oracle Access Governanceアクセス・レビューの作成キャンペーン](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance製品ページ](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governanceの製品ツアー](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access GovernanceのFAQ](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## 謝辞

*   **著者** - Anuj Tripathi、Indira Balasundaram、Anbu Anbarasu
*   **最終更新者/日付** - Anbu Anbarasu、2023年5月