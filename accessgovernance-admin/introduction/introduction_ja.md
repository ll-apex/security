# 概要

## このワークショップについて

Oracle Access Governanceは、高度なセキュリティの脅威と規制の増加に対処するために、セキュリティ所有者が直面する課題の増大に対処します。このクラウド・ネイティブ・ソリューションは、多くのアプリケーション、ワークロード、インフラストラクチャ、およびアイデンティティ・プラットフォームにわたるガバナンスとコンプライアンスの要件を満たすのに役立ちます。組織全体の可視性と機能を提供し、クラウド環境とオンプレミス環境全体で異常を特定し、セキュリティ・リスクを軽減します。Oracle Access Governanceは、高度な分析を使用して、直感的なユーザー・エクスペリエンスを提供し、アクセス権限、行動、リスクに関する推奨事項とインサイトを提供します。

次の図は、Oracle Access Governanceの高レベル・アーキテクチャを示しています。

![キャンペーンのリストを表示](images/oracle-access-governance-overview.png)

このラボでは、**アクセス制御、アクセス・レビューおよびポリシー・レビュー**のいくつかのユースケースで**Oracle Access Governance**の使用を開始するステップについて説明します。このワークショップでは、Oracle Access Governanceを使用して、従業員と請負業者のアプリケーション・アクセスを管理し、管理しています。このラボでは、データベースがターゲット・システムとしてAGに接続され、アイデンティティ・コレクション、承認ワークフロー、ロール、アクセス・バンドルおよび一元化されたポリシーを作成してアクセス制御を実装する方法を示します。また、アクセス・レビュー・キャンペーンとポリシー・レビュー・キャンペーン、および関連するレビュー・タスクの実行方法も示します。

**Oracle Access Governance**では次のことが可能です:

*   **管理者**: 様々なシステム、サービスおよびアクセス制御の管理タスクを実行します。
*   アクセス・ガバナンスとコンプライアンスのためにインテリジェントなアクセス・レビュー・キャンペーンを実行する**キャンペーン管理者**
*   **Cloud Access Reviewers**: ポリシー・アクセスのインサイトを確認し、**規範的な分析に基づいて情報に基づいた意思決定を行う**
*   **「ユーザー」**と**「ユーザー・マネージャ」**は、それぞれ自分と直属の部下に割り当てられたアクセス権を検証します。
*   ロール、アイデンティティ・コレクション、ポリシーおよび承認ワークフローを管理するには、**アクセス制御管理者**。

_推定ワークショップ時間:_ 3時間

### 目標

このワークショップでは、次のことを学習します。

*   Oracle Access Governanceサービス・インスタンスの設定および構成
*   OIGおよびデータベース用のOracle Access Governanceエージェントのインストールおよび構成
*   AGデータ・ロードの実行およびIAMユーザーの作成
*   クラウド・アイデンティティ・プロバイダとしてOCIと統合
*   アイデンティティ収集、アクセス・バンドル、ポリシーおよび承認ワークフローの管理
*   アクセス・レビュー・キャンペーンを作成し、ターゲット・データベース・システムのアクセス・レビュー・タスクを実行します
*   ポリシー・レビュー・キャンペーンを作成し、OCI IAMポリシーのポリシー・レビュー・タスクを実行します

### 前提条件

この演習では、次を想定しています。

*   管理アクセス権があるテナンシ

## さらに学ぶ

*   [Oracle Access Governance製品ページ](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governanceのドキュメント](https://docs.oracle.com/en/cloud/paas/access-governance/index.html)
*   [Oracle Access Governanceの製品ツアー](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access GovernanceのFAQ](https://www.oracle.com/security/cloud-security/access-governance/faq/)
*   [Oracle Access Governanceのお知らせブログ](https://blogs.oracle.com/cloudsecurity/post/intelligent-cloud-delivered-access-governance-with-prescriptive-analytics)

## 確認

*   **著者** - Anuj Tripathi、Indira Balasundaram、Anbu Anbarasu