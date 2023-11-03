# 概要

## このワークショップについて

Oracle Identity Governance(OIG)は、エンタープライズITリソース内のユーザーのアクセス権限を自動的に管理する強力で柔軟なエンタープライズ・アイデンティティ管理システムです。Oracle Identity Role Intelligence(OIRI)は、OIGスイートの一部として利用可能な新しいマイクロサービスであり、ロールベースのアクセス制御(RBAC)を最適化するインテリジェントで自動化された柔軟な方法です。このワークショップでは、OIRIのデプロイ、OIGからのデータのインポート、ロール・マイニングの実行およびOIRIからのOIGへのロールの公開に関する実務経験を提供します。

_推定ラボ時間_: 2時間

### 製品・技術について

Oracle Identity Role Intelligenceは、ロールベースのアクセス制御(RBAC)を最適化するためのインテリジェントで自動化された柔軟な方法です。OIRIはコンテナ化されたマイクロサービスであり、Oracle Identity Governance (OIG)の拡張機能です。マイクロサービスは、オンプレミスまたはクラウドにデプロイできます。オンプレミスの環境でKubernetesコンテナにデプロイできます。OIRIの主な機能は次のとおりです。

*   ピア・グループ間での権限パターンの検出
*   ユーザー属性に基づいてロール・マイニングを行うトップダウン・アプローチ、アプリケーションと権限に基づいてデータをフィルタするボトムアップ・アプローチ、またはハイブリッド・アプローチのサポート
*   候補者ロールと既存のロールを比較して、ロールの急増を回避
*   ユーザー・アフィニティおよびロール・アフィニティに基づいて候補ロールをチューニングする機能
*   ロール採用のワークフローをトリガーするためのOIGへのロールの自動発行
*   OIGデータベースやフラット・ファイルなど、様々なソースからデータをマージし、候補ロールを本番に移行する前にWhat-If分析を提供する機能

### 目標

このワークショップでは、次のことを行います。

*   ローカルKubernetesノードへのOIRIのデプロイ
*   OIGからOIRIへのデータのインポート
*   ロール・マイニングの実行および候補者ロールの分析
*   ロールをOIGに発行

### 前提条件

この演習では、次を想定しています。

*   Free Tier、有料またはLiveLabs Oracle Cloudアカウント

_ノート: **無料トライアル**・アカウントがある場合、無料トライアルの期限が切れると、アカウントは**Always Free**アカウントに変換されます。Always Free環境が使用可能でないかぎり、Free Tierワークショップは実行できません。**[Free TierのFAQページはここをクリックしてください。](https://www.oracle.com/cloud/free/faq.html)**_

## さらに学ぶ

*   [Oracle Identity Management 12.2.1.1.4.0](https://docs.oracle.com/en/middleware/idm/suite/12.2.1.4/index.html)
*   [Oracle Identity Role Intelligence](https://docs.oracle.com/en/middleware/idm/identity-role-intelligence/amiri/overview-oracle-identity-role-intelligence.html)

## 謝辞

*   **著者** - NATDソリューション・エンジニアリング、Anuj Tripathi、Brijith TG、Keerti R氏
*   **貢献者** - Keerti R、Brijith TG、Anuj Tripathi
*   **最終更新者/日付** - 2021年6月、NATDソリューション・エンジニアリング、Keerti R氏