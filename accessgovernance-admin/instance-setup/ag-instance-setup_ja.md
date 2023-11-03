# Oracle Access Governanceサービス・インスタンスの設定および構成

## 概要

この演習では、OAGサービス・インスタンスを設定し、このワークショップを正常に実行するために必要な構成を行います。

ペルソナ: Identity Domain Administrator

_見積時間_: 30分

ラボのクイック・ウォークスルーについては、次のビデオをご覧ください。[サイジングのないOracle Video Hubのビデオ](videohub:1_21nk0xhx)

### 目標

この演習では、次のことを行います。

*   AGサービス・インスタンスの作成
*   AGコンソールのURLへのアクセス
*   OCI IAMのユーザーへのAGロールの割当て

### 前提条件

この演習では、次を想定しています。

*   OCI管理者権限を持つ有効なOracle OCIテナンシ。
*   Access Governanceを利用できるリージョンを選択してください。

## タスク1: AGサービス・インスタンスの作成

現在アイデンティティ・ドメインにログインしていない場合は、アイデンティティ・ドメイン: ag-domainを**Identity Domain Administrator**として使用してOCIコンソールにログインします。

1.  OCIコンソールで、左上隅のナビゲーション・メニュー・アイコンをクリックして、_ナビゲーション・メニューを表示します。__「ナビゲーション」メニュー_で_「アイデンティティとセキュリティ」_をクリックします。製品のリストから_「Access Governance」_を選択します。メニュー・オプションが表示されない場合は、選択したリージョンを確認し、そのリージョンでAccess Governanceが使用可能であることを確認してください。
    
    ![サービス・インスタンスの作成](images/oci-console.png)
    
2.  「アクセス・ガバナンス」ページで、_「サービス・インスタンス」_を選択します。
    
        Name: ag-service-instance
        Description: Oracle Access Governance service instance
        Compartment: Ensure your ag-compartment is selected
        
    
    ![サービス・インスタンスの作成](images/create-service-instance.png)
    
3.  ライセンス・タイプ「Access Governance for Oracle Workloads」を選択します。_「サービス・インスタンスの作成」_をクリックします
    
    ![ライセンス・タイプの選択](images/license-type.png)
    
4.  サービス・インスタンスが_「アクティブ」_ステータスになるまで待ちます。以降の演習で使用するため、このURLを書き留めます。
    
    ![サービス・インスタンスがアクティブです](images/ag-url.png)
    
5.  URLにアクセスするには、サービス・インスタンスをクリックします。
    
    ![Access Governanceコンソール](images/ag-console.png)
    

## タスク2: ユーザーへのAGアプリケーション・ロールの割当て

1.  OCIコンソールのアイデンティティ・ドメイン: ag-domainにIdentity Domain Administratorとしてログインします。
    
    *   OCIコンソールで、「アイデンティティ」->「ドメイン」->「ag-domain」->「Oracle Cloud Services」->「AG-service-instance」->「アプリケーション・ロール」に移動します。
        
    *   _「AG管理者」_ロール、_「AGキャンペーン管理者」_ロールおよび_「AGアクセス制御管理」_ロールがリストされていることを確認します。それぞれの右隅にある下向き矢印をクリックします。
        
    
    ![OIGアイデンティティ・ロールとアクセス・ポリシー](images/user-approle.png)
    
    *   Click on _Assigned Users -> Manage_. Select _Pamela Green_ in _Available Users._ Click on _Assign_
    
    ![OIGアイデンティティ・ロールとアクセス・ポリシー](images/user-approle-list.png)
    
    *   ユーザーPamela Greenが_「Assigned Users」_の下に表示されます。
    
    ![OIGアイデンティティ・ロールとアクセス・ポリシー](images/user-approle-assign.png)
    
    *   Pamela Greenには、_AG管理者_アプリケーション・ロール_AGキャンペーン管理者_および_AGアクセス制御管理_ロールが割り当てられています。これでウィンドウを閉じることができます。
        
    *   次に、_AG User_ロールと_AG CloudAccessReviewer_ロールがリストされていることを確認します。それぞれの右隅にある下向き矢印をクリックします。
        
        ![OIGアイデンティティ・ロールとアクセス・ポリシー](images/aguser.png)
        
        ![OIGアイデンティティ・ロールとアクセス・ポリシー](images/agreviewer.png)
        
    *   Click on _Assigned Users -> Manage_. Select _Mark Hernandez_ and _Harlan Bullard_ in _Available Users._ Click on _Assign_
        
    
    ![OIGアイデンティティ・ロールとアクセス・ポリシー](images/ag-userassign.png)
    
    ![OIGアイデンティティ・ロールとアクセス・ポリシー](images/ag-reviewerassign.png)
    
    *   Mark HernandezとHarlan Bullardには、_AG User_アプリケーション・ロールおよび_AG CloudAccessReviewer_が割り当てられました。これでウィンドウを閉じることができます。
    
    **次の演習に進みます。**
    

## さらに学ぶ

*   [Oracle Access Governanceアクセス・レビューの作成キャンペーン](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance製品ページ](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governanceの製品ツアー](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access GovernanceのFAQ](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## 謝辞

*   **著者** - Anuj Tripathi、Indira Balasundaram、Anbu Anbarasu