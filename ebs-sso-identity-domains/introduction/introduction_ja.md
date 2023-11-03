# 概要

## このワークショップについて

OCI IAMアイデンティティ・ドメインは、様々なIAMユースケースおよびシナリオに対処するために使用できる包括的なIdentity-as-a-Service (IDaaS)ソリューションです。OCI IAMを使用して、多数のクラウドおよびオンプレミス・アプリケーションにわたるユーザーのアクセスを管理し、セキュアな認証、権限の簡単な管理、エンド・ユーザーのシームレスなSSOを実現できます。このワークショップでは、Terraformを使用してEBSアプリケーション、EBS AsserterサーバーおよびOCI IAMアイデンティティ・ドメインをOCIにデプロイし、Terraformを使用してデプロイされた設定を構成し、アイデンティティ・ドメインのEBS Asserter機能を活用してEBSアプリケーションへのSSOを実現します。

次の図は、OCI IAMを使用したE-Business Suite Asserter設定の概要を示しています。

![ebs-oci-iam-identity-domains](./images/ebs-oci-iam-identity-domains.png "イメージ1")

次の図は、E-Business Suite Asserterを使用してOracle E-Business SuiteをOCI IAMと統合する場合のログイン・フローを示しています。

![ワークフロー](./images/workflow.png)

*   ユーザーは、Oracle E-Business Suiteで保護されたリソースへのアクセスをリクエストします。
*   Oracle E-Business Suiteは、ユーザー・ブラウザをE-Business Suite Asserterアプリケーションにリダイレクトします。
*   E-Business Suite Asserterは、OCI IAM SDKを使用して認可URLを生成し、ブラウザをOCI IAMにリダイレクトします。
*   OCI IAMは、そのサインイン・ページをユーザーに提供します。
*   ユーザーは、OCI IAMに資格証明を送信します。
*   OCI IAMは、認可コードを発行し、ユーザーのブラウザをE-Business Suite Asserterにリダイレクトします。
*   E-Business Suite Asserterは、OCI IAM SDKを使用してOCI IAMと通信し、認可コードをアクセス・トークンに交換します。
*   OCI IAMは、アクセス・トークンとIDトークンをE-Business Suite Asserterに発行します。
*   E-Business Suite Asserterは、Oracle E-Business Suite Cookieを作成し、ユーザーのブラウザをOracle E-Business Suiteにリダイレクトします。
*   Oracle E-Business Suiteは、ユーザーがリクエストした保護されたリソースを表示します。

このラボでは、一般的なユース・ケースである**EBS Asserterを使用したEBS SSOの達成**を使用して**OCI IAMアイデンティティ・ドメイン**の使用を開始するステップについて説明します。このワークショップでは、**リソース・マネージャ**を介して_Terraform_を使用して**EBS**アプリケーションをOCIにデプロイするステップに従います。同じスタックによって、**EBS Asserter Server**および**OCI IAMアイデンティティ・ドメイン**もデプロイされます。デプロイ後、Terraformを使用して_EBS Asserter Server_および_アイデンティティ・ドメイン_で必要な構成変更も行います。フロー全体の検証に向かう前に、いくつかの_手動タスク_を実行して、EBSアプリケーションおよびアイデンティティ・ドメインで構成を完了します。検証が完了したら、_リソース・マネージャ_を使用してクリーンアップ・アクティビティ/ステップを実行します。

_見積時間:_ 1時間

### 目標

このワークショップでは、次のことを学習します。

*   リソース・マネージャを介してTerraformスタックをデプロイし、OCI上にEBSアプリケーション、EBS AsserterインスタンスおよびOCI IAMアイデンティティ・ドメインを作成します。
*   OCI IAMアイデンティティ・ドメインでの機密アプリケーションの作成
*   Terraformスタックをデプロイして、デプロイされたEBS AsserterインスタンスおよびOCI IAMアイデンティティ・ドメインを構成します
*   SSOを有効にするには、EBSアプリケーションでサイト・プロファイルを手動で更新します。
*   設定の検証とSSOフローのテスト
*   デプロイ済リソースをクリーン・アップします。

### 前提条件

この演習では、次を想定しています。

*   管理アクセス権があるPay Goテナンシ(無料ではない)

## さらに学ぶ

*   [OCI IAMアイデンティティ・ドメイン](https://docs.oracle.com/en-us/iaas/Content/Identity/home.htm)
*   [E-Business Suite Asserter](https://docs.oracle.com/en/solutions/sso-oci-iam-ebs-asserter/index.html#GUID-9E74257D-396D-49E5-A2FD-6E59ACC48946)

## 謝辞

*   **著者** - Gautam Mishra、Aqib Bhat、Samratha S P
*   **貢献者** - Chetan Soni氏、Sagar Takkar氏
*   **サポート担当者** - Deepak Rao Narasimha Gajendragad
*   **リード元** - Deepthi Shetty
*   **最終更新者/日付** - Gautam Mishra 2023年5月