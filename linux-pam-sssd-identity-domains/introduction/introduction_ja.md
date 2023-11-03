# 概要

## このワークショップについて

OCI IAMアイデンティティ・ドメインは、様々なIAMユースケースおよびシナリオに対処するために使用できる包括的なIdentity-as-a-Service (IDaaS)ソリューションです。OCI IAMを使用して、多数のクラウドおよびオンプレミス・アプリケーションにわたるユーザーのアクセスを管理し、セキュアな認証、権限の簡単な管理、エンド・ユーザーのシームレスなSSOを実現できます。このワークショップでは、OCI IAM Linux Pluggable Authentication Module (PAM)を使用してLinux環境をOCI IAMアイデンティティ・ドメインと統合し、第1および第2ファクタ認証でエンド・ユーザー認証を実行する方法を示します。

次の図は、OCI IAMを使用したLinux PAM設定の高レベル・アーキテクチャを示しています。

![Architecture](./images/architecture-diagram.png "Architecture")

このラボでは、**OCI IAMのLinux Pluggable Authentication Module (PAM)モジュールを使用したLinux環境への認証**というユースケースで**OCI IAMアイデンティティ・ドメイン**の使用を開始するステップについて説明します。このワークショップでは、**リソース・マネージャ**を介して_Terraform_を使用して**Linuxサーバー**をOCIにデプロイするステップに従います。同じスタックで、**OCI IAMアイデンティティ・ドメイン**もデプロイされます。デプロイ後は、Terraformを使用して_Linuxサーバー_および_アイデンティティ・ドメイン_で必要な構成変更も行います。フロー全体の検証に進む前に、アイデンティティ・ドメインの構成を完了するための_手動タスク_をいくつか実行します。検証が完了したら、_リソース・マネージャ_を使用してクリーンアップ・アクティビティ/ステップを実行します。

_推定ワークショップ時間:_ 1時間

### 目標

このワークショップでは、次のことを学習します。

*   Terraformスタックをデプロイし、リソース・マネージャを介してOCIにLinuxサーバーおよびOCI IAMアイデンティティ・ドメインを作成します。
*   OCI IAMアイデンティティ・ドメインに機密アプリケーションを作成します。
*   Terraformスタックをデプロイして、LinuxインスタンスおよびOCI IAMアイデンティティ・ドメインを構成します。
*   _MFA_を使用して認証フローをテストします。
*   デプロイ済リソースをクリーン・アップします。

### 前提条件

この演習では、次を想定しています。

*   管理アクセス権があるPay Goテナンシ(無料ではない)

## さらに学ぶ

*   [OCI IAMアイデンティティ・ドメイン](https://docs.oracle.com/en-us/iaas/Content/Identity/home.htm)
*   [OCI IAM Linux PAMモジュール](https://docs.oracle.com/en/cloud/paas/identity-cloud/uaids/manage-linux-authentication-using-linux-pam-module.html#GUID-8FE587F4-D44C-47C1-BBE2-3D32886D0553)

## 謝辞

*   **著者** - Gautam Mishra、Aqib Bhat
*   **貢献者** - Deepthi Shetty
*   **最終更新者/日付** - Gautam Mishra 2023年7月