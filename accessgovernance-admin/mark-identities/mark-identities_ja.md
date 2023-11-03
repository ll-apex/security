# アイデンティティのマーク

## 概要

Access Governance管理者(Pamela Green)は、アイデンティティをアクティブ化します。

*   Persona: アクセス・ガバナンス管理者

_予想時間_: 15分

ラボのクイック・ウォークスルーについては、次のビデオをご覧ください。[サイジングのないOracle Video Hubのビデオ](videohub:1_ml4wxlqu)

### 目標

この演習では、次のことを行います。

*   アイデンティティのアクティブ化

## タスク1: Oracle Access Governanceコンソールへのサインイン

1.  ブラウザから、_ラボ3: タスク1_に記載されているURLを使用してOracle Access Governanceコンソールに移動します
    
2.  **Oracle Access Governance管理者**のユーザー名とパスワードを入力します(Pamela Green)
    
    **ユーザー名:**
    
        <copy>pamela.green</copy>
        
    
    **パスワード:**
    
        <copy>Oracl@123456</copy>
        

Oracle Access Governanceコンソールのホーム・ページに移動します。

![Access Governanceホームページ](images/ag-homepage.png)

## タスク2: アイデンティティのアクティブ化

このタスクでは、サービスに含めるアイデンティティを選択します。

1.  Oracle Access Governanceコンソールで、「サービス管理」→「アイデンティティの管理」に移動します

![「アイデンティティの管理」のナビゲート](images/navigate-manage-identities.png)

2.  **「任意」**条件一致オプションを選択します。
    
    ![「Manage Identities」ページ](images/select-any.png)
    
3.  含めるアイデンティティと一致する条件について、次のオプションを選択します。
    
    *   属性の選択: ステータス
    *   演算子の選択: 次を含む
    *   属性値: アクティブ
    
    **\[Enter\]**を押します
    
4.  **前述のルールに基づくサマリーのプレビュー**をクリックします。ルールに一致するアイデンティティが表示されます。
    
5.  ポップアップを閉じ、**「保存」**をクリックします
    

![「Manage Identities」ページ](images/identities-user.png)

**次の演習に進む**ことができます。

## さらに学ぶ

*   [Oracle Access Governanceアクセス・レビューの作成キャンペーン](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance製品ページ](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governanceの製品ツアー](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access GovernanceのFAQ](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## 確認

*   **著者** - Anuj Tripathi、Indira Balasundaram、Anbu Anbarasu
*   **最終更新者/日付** - Anbu Anbarasu、2023年5月