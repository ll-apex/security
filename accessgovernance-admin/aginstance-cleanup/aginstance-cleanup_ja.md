# アクセス・ガバナンス・サービス・インスタンスのクリーンアップ

## 概要

演習を完了したら、クリーンアップを実行してAccess Governanceサービス・インスタンスを処分することをお薦めします。この演習では、リソースを適切に破棄します。

_予想時間_: 10分

ラボのクイック・ウォークスルーについては、次のビデオをご覧ください。[サイジングのないOracle Video Hubのビデオ](videohub:1_ixwlapkq)

### 目標

この演習では、次のことを行います。

*   アクセス・ガバナンス・サービス・インスタンスの破棄

### 前提条件

この演習では、次を想定しています。

*   Oracle Cloudアカウント
*   次を完了しました:
    *   演習: 設定の準備
    *   演習: 環境設定

## タスク1: Access Governanceサービス・インスタンスの破棄

1.  Oracle Cloudへのログイン
    
    ![OCIコンソールを開く](images/open-oci-console.png)
    
2.  左上隅の「Navigation Menu」アイコンをクリックして、「Navigation」メニューを表示します。ナビゲーション・メニューで「Identity and Security」をクリックします。製品のリストから「Access Governance」を選択します。
    
    ![アクセス・ガバナンスに移動](images/access-governance.png)
    
3.  「Access Governance」ページで、「Service Instances」を選択します。_ラボ4: タスク1_で作成したサービス・インスタンス**service-instance**をクリックします
    
    ![dockerのステータスを検証します。](images/service-instance.png)
    
4.  _「削除」_をクリックします。プロンプトが表示され、_「削除」_オプションを確認します。
    
    ![dockerのステータスを検証します。](images/delete-service-instance.png)
    
5.  これで、Access Governanceサービス・インスタンスが正常に削除されました。
    
    **次の演習に進みます。**
    

## さらに学ぶ

*   [Oracle Access Governanceアクセス・レビューの作成キャンペーン](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance製品ページ](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governanceの製品ツアー](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access GovernanceのFAQ](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## 謝辞

*   **著者** - Anuj Tripathi、Indira Balasundaram、Anbu Anbarasu