# アクセス・ガバナンスのグループとポリシーの作成

## 概要

Access Governanceのポリシーを作成します。

*   Persona: デフォルト・ドメイン管理者

_予想時間_: 15分

ラボのクイック・ウォークスルーについては、次のビデオをご覧ください。[サイジングのないOracle Video Hubのビデオ](videohub:1_x0vzlnhh)

### 目標

この演習では、次のことを行います。

*   Access Governance用の**agcs-user**の作成
*   アクセス・ガバナンスの**グループ**の設定
*   アクセス・ガバナンスの**ポリシー**の設定
*   Oracle Access GovernanceがOCIに接続できるように**ポリシー**を設定します
*   ドメイン管理者アクセス用の**ポリシー**の設定

## タスク1: AGCSユーザーの作成

1.  **デフォルト・ドメイン管理者**としてOCIコンソールの**デフォルト・ドメイン**にログインします
    
2.  OCIコンソールで、左上のナビゲーション・メニュー・アイコンをクリックして、_「ナビゲーション」メニューを表示します。_ _「ナビゲーション」メニュー_の_「アイデンティティとセキュリティ」_をクリックします。製品のリストから_「ドメイン」_を選択します。
    
    ![ドメインに移動](images/navigate-domains.png)
    
3.  「ドメイン」ページで、_「デフォルト・ドメイン」_をクリックします。**ルート・コンパートメント**が選択されていることを確認します。
    
    ![ドメインに移動](images/default-domain.png)
    
4.  _「ユーザー」_を選択します。_「Create User」_をクリックします。
    
    ![ユーザーの作成](images/select-users.png)
    
    ![ユーザーの作成](images/create-user.png)
    
    次の詳細を入力して_agcs-user_を作成します
    
        First name: agcs
        Last name: user
        Use the email address as the username: Uncheck the checkbox 
        Username: agcs-user
        Email: agcsuser@example.com
        
    
    ![ユーザーの作成](images/createuser-tab.png)
    
    _「作成」_をクリックします
    
    _agcs-user_が作成されました。
    

## タスク2: AGグループの作成

1.  **デフォルト・ドメイン管理者**としてOCIコンソールの**デフォルト・ドメイン**にログインします
    
2.  OCIコンソールで、左上のナビゲーション・メニュー・アイコンをクリックして、_「ナビゲーション」メニューを表示します。_ _「ナビゲーション」メニュー_の_「アイデンティティとセキュリティ」_をクリックします。製品のリストから_「ドメイン」_を選択します。
    
    ![ドメインに移動](images/navigate-select-domain.png)
    
3.  「ドメイン」ページで、_「デフォルト・ドメイン」_をクリックします。**ルート・コンパートメント**が選択されていることを確認します。_「グループ」_を選択します。_「グループの作成」_をクリックします
    
    ![アイデンティティ・ドメインの選択](images/default-domain.png)
    
    ![グループの選択](images/select-group.png)
    
    次の詳細を入力して、_agcs-group_を作成し、**agcs-user**ユーザーをグループに割り当てます
    
        Name: agcs-group
        Description: Access governance group to manage users 
        Users: Select the agcs-user 
        
    
    _「作成」_をクリックします
    
    ![AGグループの作成](images/creategroup-tab.png)
    
    _グループ_は正常に作成されました。
    

## タスク3: AGポリシーの作成

1.  OCIコンソールで、左上のナビゲーション・メニュー・アイコンをクリックして、_「ナビゲーション」メニューを表示します。_ _「ナビゲーション」メニュー_の_「アイデンティティとセキュリティ」_をクリックします。製品のリストから_「ポリシー」_を選択します。
    
2.  「ポリシー」ページで、_「ポリシーの作成」_をクリックして、ルート・コンパートメントにポリシー: ag-access-policyを作成します。
    
        Name: ag-access-policy
        Description: IAM policy for granting Domain_Administrators access to manage access governance instances
        Compartment: Ensure your  root compartment is selected
        Policy Builder: Select the show manual editor checkbox
        Statement :
        
    
        <copy>Allow group ag-domain/Domain_Administrators to manage agcs-instance in compartment ag-compartment
        Allow group ag-domain/Domain_Administrators to read objectstorage-namespace in tenancy</copy>
        
    
    _「作成」_をクリックします
    
    「ポリシー」ページのルート・コンパートメントで、「ポリシーの作成」をクリックしてポリシーを作成します: agcs-policy
    
        Name: agcs-policy
        Description: Oracle Access Governance policy 
        Compartment: Ensure your root compartment is selected
        Policy Builder: Select the show manual editor checkbox
        
    
        <copy>ALLOW GROUP agcs-group to read all-resources IN TENANCY
        ALLOW GROUP agcs-group to manage policies IN TENANCY
        ALLOW GROUP agcs-group to manage domains IN TENANCY
        </copy>
        
    
    「作成」をクリックします。
    
    「ポリシー」ページのルート・コンパートメントで、「ポリシーの作成」をクリックしてポリシーを作成します: domain-admin-policy
    
        Name: domain-admin-policy
        Description: IAM policy (domain-admin-policy) in the COMPARTMENT to give access to the Identity Domain admin for the compartment created
        Compartment: Ensure your root compartment is selected
        Policy Builder: Select the show manual editor checkbox
        Statement :
        
    
        <copy>Allow group ag-domain/Domain_Administrators to manage all-resources in compartment ag-compartment</copy>
        
    
    _「作成」_をクリックします
    

**次の演習に進む**ことができます。

## さらに学ぶ

*   [Oracle Access Governanceアクセス・レビューの作成キャンペーン](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance製品ページ](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governanceの製品ツアー](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access GovernanceのFAQ](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## 確認

*   **著者** - Anuj Tripathi、Indira Balasundaram、Anbu Anbarasu
*   **最終更新者/日付** - Anbu Anbarasu、2023年6月