# OCIポリシー、VCN、グループおよびコンパートメントの作成

## 概要

アイデンティティ・ドメインで**Identity Domain Administrator**ロールを持つユーザーは、OCIポリシー、グループおよびコンパートメントを作成できます。**OCI** console.ThisラボのOCI iamユーザーは、このOCI-IAMポリシー・レビューの実行に必要なOCIポリシー、VCN、グループおよびコンパートメントの設定方法を示します。

*   ペルソナ: Identity Domain Administrator

_予想時間_: 15分

ラボのクイック・ウォークスルーについては、次のビデオをご覧ください。[サイジングのないOracle Video Hubのビデオ](videohub:1_wabc1y93)

### 目標

この演習では、次のことを行います。

*   OCIポリシー、VCN、グループおよびコンパートメント、OCI IAMユーザーの手動作成
*   ノート: この演習で作成するすべてのリソースは、**ag-compartment**に作成されるはずです
*   この演習では、次のリソースを作成します。

| リソースの種類 | リソース | 説明 |
| :-- | :-: | :-: |
| コンパートメント | 開発環境 | 開発環境 |
|  | 品質保証 | 品質保証 |
|  | テスト | テスト |
| ユーザー数 | demouser1 | demouser1はグループに属します- SecurityAdmins |
|  | demouser2 | demouser2はグループに属します- SecurityAdminsおよびNetworkAdmins |
|  | demouser3 | demouser3はグループに属します- SecurityAdminsおよび監査者 |
| グループ | SecurityAdmins | SecurityAdmins |
|  | NetworkAdmins | NetworkAdmins |
|  | 監査者 | 監査者 |
| ポリシー | 監査者ポリシー | 監査者のアクセス・ポリシー |
|  | network-admins-policy | ネットワーク管理者のアクセス・ポリシー |
|  | セキュリティ管理者ポリシー | セキュリティ管理者のアクセス・ポリシー |
| Virtual Cloud Network | ag-vcn | AGテストVirtual Cloudネットワーク |

## タスク1: コンパートメントの作成

1.  OCIコンソール・アイデンティティ・ドメイン: ag-domainに**Identity Domain Administrator**としてログインします。

![OCIコンソールにログイン](images/oci-console.png)

2.  OCIコンソールで、左上のナビゲーション・メニュー・アイコンをクリックして、ナビゲーション・メニューを表示します。ナビゲーション・メニューで「アイデンティティとセキュリティ」をクリックします。製品のリストから「コンパートメント」を選択します。

![Naviagteからコンパートメント](images/navigate-compartment.png)

3.  _「コンパートメントの作成」をクリックします。_ 3つのコンパートメントを作成するには、次の詳細を指定します: **開発、品質保証およびテスト**
    
    ![区分](images/create-compartment.png)
    

**名前:**開発

**説明:**開発

**親コンパートメント:** **ag-compartment**コンパートメントを選択します

_コンパートメントの作成_をクリックします

![コンパートメントの作成](images/create-compartment-tab.png)

**名前:** Quality-Assurance

**説明:** Quality-Assurance

**親コンパートメント:** **ag-compartment**コンパートメントを選択します

_コンパートメントの作成_をクリックします

![コンパートメントの作成](images/qa-compartment.png)

**名前:**テスト

**説明:**テスト

**親コンパートメント:** **ag-compartment**コンパートメントを選択します

_コンパートメントの作成_をクリックします

![コンパートメントの作成](images/testing-compartment.png)

_コンパートメント_が正常に作成されました。

## タスク2: グループの作成

1.  「アイデンティティ」->「ドメイン」->「ag-domain」->「グループ」に移動します。
    
    ![グループの作成](images/create-group.png)
    
2.  _「グループの作成」_をクリックします。次の詳細を入力して、3つのグループを作成します: **SecurityAdmins、NetworkAdminsおよび監査者**
    

**名前:** SecurityAdmins

**説明:** SecurityAdmins

_「作成」_をクリックします

![グループの作成](images/securityadmins-group.png)

**名前:** NetworkAdmins

**説明:** NetworkAdmins

_「作成」_をクリックします

![グループの作成](images/networkadmins-group.png)

**名前:**監査者

**説明:**監査者

_「作成」_をクリックします

![グループの作成](images/auditors-group.png)

_グループ_が正常に作成されました。

## タスク3: サンプル・ユーザーの作成

1.  「アイデンティティ」->「ドメイン」->「ag-domain」->「ユーザー」に移動します
    
    ![ユーザーの作成](images/create-user.png)
    
2.  _「ユーザーの作成」_をクリックします。次の詳細を入力して、3人のサンプル・ユーザーを作成します: **demouser1、demouser2およびdemouser3**
    

**名:**デモ

**姓:** user1

**ユーザー名:** demouser1

**電子メール:** demouser1@example.com

**グループ:**チェック・ボックス**SecurityAdminsを選択します。**

_「作成」_をクリックします

![ユーザーの作成](images/demo-user1.png)

![ユーザーの作成](images/createuser-securityadmin.png)

**名:**デモ

**姓:** user2

**ユーザー名:** demouser2

**電子メール:** demouser2@example.com

**グループ:**チェック・ボックス**SecurityAdmins**および**NetworkAdminsを選択します。**

_「作成」_をクリックします

![ユーザーの作成](images/demo-user2.png)

![ユーザーの作成](images/user-group-assign.png)

**名:**デモ

**姓:** user3

**ユーザー名:** demouser3

**電子メール:** demouser3@example.com

**グループ:**チェック・ボックス**SecurityAdmins**および**「監査者」を選択します。**

_「作成」_をクリックします

![ユーザーの作成](images/demo-user3.png)

![ユーザーの作成](images/user-group.png)

_ユーザー_が正常に作成されました。

## タスク4: ポリシーの作成

1.  OCIコンソールで、左上のナビゲーション・メニュー・アイコンをクリックして、_「ナビゲーション」メニューを表示します。_ _「ナビゲーション」メニュー_の_「アイデンティティとセキュリティ」_をクリックします。製品のリストから_「ポリシー」_を選択します。

![ポリシーの選択](images/policy-page.png)

2.  「ポリシー」ページで、_「ポリシーの作成」_を毎回クリックして、**auditors-policy、network-admins-policyおよびsecurity-admins-policy**という3つのポリシーを作成します。
    
        Name: auditors_policy
        Description: Access Policy for Auditors
        Compartment: Ensure ag-compartment is selected
        Policy Builder: Select the show manual editor checkbox
        
        
    
        <copy>Allow group ag-domain/Auditors to read instances in compartment ag-compartment
        Allow group ag-domain/Auditors to read audit-events in compartment ag-compartment
        Allow group ag-domain/Auditors to inspect vaults in compartment ag-compartment
        Allow group ag-domain/Auditors to inspect virtual-network-family in compartment ag-compartment
        Allow group ag-domain/Auditors to inspect keys in compartment ag-compartment
        Allow group ag-domain/Auditors to inspect vaults in compartment ag-compartment
        Allow group ag-domain/Auditors to inspect virtual-network-family in compartment ag-compartment
        Allow group ag-domain/Auditors to inspect keys in compartment Quality-Assurance
        Allow group ag-domain/Auditors to inspect vaults in compartment Development
        Allow group ag-domain/Auditors to inspect virtual-network-family in compartment Testing
        </copy>
        
    
    _「作成」_をクリックします
    
        Name: network_admins_policy
        
        Description: Access Policy for Network Administrators
        
        Compartment: Ensure ag-compartment is selected
        
        Policy Builder: Select the show manual editor checkbox
        
        
    
        <copy>Allow group ag-domain/NetworkAdmins to manage all-resources in compartment ag-compartment</copy>
        
    
    _「作成」_をクリックします
    
        Name: security_admins_policy
        Description: Access Policy for Security Admins
        Compartment: Ensure ag-compartment is selected
        Policy Builder: Select the show manual editor checkbox
        
        
    
        <copy>Allow group ag-domain/SecurityAdmins to manage virtual-network-family in compartment ag-compartment
        Allow group ag-domain/SecurityAdmins to manage vaults in compartment ag-compartment
        Allow group ag-domain/SecurityAdmins to manage secret-family in compartment ag-compartment
        Allow group ag-domain/SecurityAdmins to manage keys in compartment ag-compartment
        Allow group ag-domain/SecurityAdmins to inspect work-requests in compartment Quality-Assurance
        Allow group ag-domain/SecurityAdmins to manage keys in compartment Development
        Allow group ag-domain/SecurityAdmins to manage bastion in compartment Testing	
        </copy>
        
    
    _「作成」_をクリックします
    

_ポリシー_が正常に作成されました。

## タスク5: VCNの作成

1.  「Networking」→「Virtual Cloud Networks」に移動します
    
    ![VCNにナビゲートします。](images/navigate-vcn.png)
    
2.  **ag-compartment**が選択されていることを確認します。**「VCNウィザードの起動」**をクリックします
    

![VCNにナビゲートします。](images/start-wizard.png)

3.  **「インターネット接続性を持つVCNの作成」**ボックスを選択します。**「VCNウィザードの起動」**をクリックします。

![VCNにナビゲートします。](images/wizard-starts.png)

4.  \[設定\]で、以下の詳細を指定します。

**VCN名:** ag-VCN

**コンパートメント:** ag-compartmentを選択します。

    ![Navigate to VCN](images/enter-vcn-details.png)
    

5.  **「次へ」**をクリックします。
    
    ![VCNにナビゲートします。](images/click-next-wizard.png)
    
6.  すべての詳細を確認します。**「作成」**をクリックします
    
    ![VCNにナビゲートします。](images/click-create-wizard.png)
    
    _VCN_が正常に作成されました。
    

## タスク6: OCI IAMでのユーザーの作成

1.  左上隅の「Navigation Menu」アイコンをクリックして、「Navigation」メニューを表示します。ナビゲーション・メニューで「Identity and Security」をクリックします。製品のリストから「ドメイン」を選択します。
    
    ![ドメインに移動](images/navigate-select-domain.png)
    
2.  「ドメイン」ページで、「アイデンティティ・ドメイン: 作成した_ag-domain_」をクリックします。
    
    ![アイデンティティ・ドメインに移動](images/open-domains.png)
    
    _「ユーザー」_を選択します。_「Create User」_をクリックします。
    
    ![ユーザーにナビゲート](images/navigate-to-users.png)
    
3.  「ユーザー名として電子メール・アドレスを使用」の選択を解除します
    
4.  次の詳細を入力して、3人のユーザー(Pamela Green (キャンペーン管理者)、Harlan Bullard (マネージャ)、Mark Hernandez (従業員ユーザー)をIAMに作成します。ユーザーごとに異なる電子メールIDを使用してください。
    
        First Name: Pamela
        Last Name: Green
        Username: pamela.green
        Email: Specify unique email-id to which you will be receiving activation mail for password reset for the user. 
        
    
    ![ユーザーの作成](images/user-create-pamela.png)
    
    _「作成」_をクリックします
    
        First Name: Harlan
        Last Name: Bullard
        Username: harlan.bullard
        Email: Specify unique email-id to which you will be receiving activation mail for password reset for the user. 
        
    
    ![ユーザーの作成](images/user-create-harlan.png)
    
    _「作成」_をクリックします
    
        First Name: Mark
        Last Name: Hernandez
        Username: mhernandez
        Email: Specify unique email-id to which you will be receiving activation mail for password reset for the user. 
        
    
    ![ユーザーの作成](images/user-create-mark.png)
    
    _「作成」_をクリックします
    
        First Name: Jerry
        Last Name: Poland
        Username: jerry.poland
        Email: Specify unique email-id to which you will be receiving activation mail for password reset for the user. 
        
    
    _「作成」_をクリックします
    
5.  クラウド・コンソールからサインアウトします。
    
6.  作成されるユーザーごとに、_タスク3: ステップ4_で提供されている電子メールIDにアクティブ化メールが送信されます。各ユーザーに受信された_アクティブ化メール_を使用して、3人のユーザーのパスワードをリセットします。次のパスワードにパスワードをリセットします。
    
    **パスワード:**
    
        <copy>Oracl@123456</copy>
        

**次の演習に進む**ことができます。

## さらに学ぶ

*   [Oracle Access Governanceアクセス・レビューの作成キャンペーン](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance製品ページ](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governanceの製品ツアー](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access GovernanceのFAQ](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## 謝辞

*   **著者** - Anuj Tripathi、Indira Balasundaram、Anbu Anbarasu
*   **貢献者** - Abhishek Juneja
*   **最終更新者/日付** - クラウド・プラットフォームCOE、Anbu Anbarasu、2023年1月