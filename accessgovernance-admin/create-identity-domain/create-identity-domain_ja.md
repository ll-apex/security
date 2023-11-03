# コンパートメントおよびアイデンティティ・ドメインの作成

## 概要

コンパートメントの作成

*   Persona: デフォルト・ドメイン管理者

_予想時間_: 15分

ラボのクイック・ウォークスルーについては、次のビデオをご覧ください。[サイジングのないOracle Video Hubのビデオ](videohub:1_8rvbi8pv)

### 目標

この演習では、次のことを行います。

*   **コンパートメント**の作成
*   **アイデンティティ・ドメイン**の作成
*   アカウントのアクティブ化

### 前提条件

この演習では、次を想定しています。

*   OCI管理者権限を持つ有効なOracle OCIテナンシ。

## タスク1: コンパートメントの作成

1.  **デフォルト・ドメイン管理者**としてOCIにログインします。左上隅のハンバーガーメニューを開きます。「アイデンティティとセキュリティ」をクリックし、「アイデンティティ」→「コンパートメント」を選択します。
    
    ![コンパートメントに移動](images/navigate-comp.png)
    
2.  _コンパートメントの作成_をクリックします
    
    ![コンパートメントの作成の選択](images/create-compartment.png)
    
3.  作成するアイデンティティ・ドメインの詳細を入力します。_「コンパートメントの作成」_をクリックします
    
        Name: ag-compartment
        Description: Oracle Access Governance Compartment
        Parent Compartment: Ensure your root compartment is selected
        
    
    ![新規コンパートメントの作成](images/new-compartment.png)
    
4.  これで、コンパートメント**ag-compartment**が作成されました
    

## タスク2: アイデンティティ・ドメインの作成

1.  **デフォルト・ドメイン管理者**としてOCIコンソールにログインします。左上隅のハンバーガーメニューを開きます。「アイデンティティとセキュリティ」をクリックし、「アイデンティティ」→「ドメイン」を選択します。
    
    ![ドメインに移動](images/navigate-to-domains.png)
    
2.  アイデンティティ・ドメインを作成する_ag-compartment_を選択します。_「ドメインの作成」_をクリックします
    
    ![「Create Identity Domain」を選択します。](images/create-domains.png)
    
3.  作成するアイデンティティ・ドメインの詳細を入力します。_「作成」_をクリックします
    
        Display Name: ag-domain
        Description: Oracle Access Governance Identity Domain
        Domaintype: Free
        Domain Administrator: Select the checkbox for Create an administrative user for this domain 
        Administrator first name: Enter administrator  first name 
        Administrator last name: Enter administrator last name 
        Administrator username/email: Enter the administrator email id
        Compartment: Ensure ag-compartment compartment is selected
        
    
    ![「Create Identity Domain」をクリックします。](images/click-create-domain.png)
    
    ![アイデンティティ・ドメインの作成の完了](images/complete-creation-domain.png)
    
4.  これで、アイデンティティ・ドメインが作成されました。
    
    ![アクティブ・アイデンティティ・ドメイン](images/active-identity-domain.png)
    

## タスク3: アカウントのアクティブ化

1.  管理者の電子メールに移動し、**「アカウントのアクティブ化」**をクリックします
    
    ![電子メールのアクティブ化](images/activate-email.png)
    
2.  次の画面にパスワードを入力して送信します
    
    **次の演習に進みます。**
    

## さらに学ぶ

*   [Oracle Access Governanceアクセス・レビューの作成キャンペーン](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance製品ページ](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governanceの製品ツアー](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access GovernanceのFAQ](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## 謝辞

*   **著者** - Anuj Tripathi、Indira Balasundaram、Anbu Anbarasu