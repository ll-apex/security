# 概要

## このワークショップについて

Oracle Identity Managementを使用すると、組織は、ファイアウォール内とクラウド内を問わず、すべてのエンタープライズ・リソースにわたってユーザー・アイデンティティのエンドツーエンドのライフサイクルを効果的に管理できます。Oracle Identity Managementプラットフォームは、アイデンティティ・ガバナンス、アクセス管理およびディレクトリ・サービスのためのスケーラブルなソリューションを提供します。この最新のプラットフォームは、組織がセキュリティを強化し、コンプライアンスを簡素化し、モバイルおよびソーシャル・アクセスに関するビジネス・チャンスを獲得するのに役立ちます。

Oracleには、DockerおよびKubernetes用のコンテナを利用してOracle Identity and Access Management製品のライフサイクル管理を最新化することで、DevOpsデリバリ・モデルが含まれています。このアプローチにより、物理クラウド、プライベート・クラウドまたはパブリック・クラウド上の様々なデプロイメントにわたるOracle Identity and Access Management製品のデプロイメントとメンテナンスが簡略化されます。

Kubernetesは、クラスタ間でコンテナ化されたアプリケーションを実行および調整するためのシステムです。コンテナ化されたアプリケーションおよびサービスのライフサイクルを管理するため、予測性、スケーラビリティおよび高可用性が提供されます。

Oracleにはオープン・ソースのWebLogic Server Kubernetes Operatorが用意されています。このOperatorには、Oracle Identity Governance (OIG)やOracle Access Management (OAM)など、様々なFusion Middleware製品のデプロイメントおよび管理を支援するいくつかの主要な機能があります。KubernetesでのOracle Unified Directoryデプロイメントでは、Oracleが提供するデプロイメント・スクリプトを利用して、提供されたサンプルまたはHelmチャートを使用してOracle Unified Directoryコンテナを作成します。

Kubernetesオペレータと即時利用可能なデプロイメント・スクリプトを使用すると、次のことができます:-

1.  Kubernetes永続ボリュームにOIG/OAM/OUDインスタンスを作成します。この永続ボリュームは、NFSファイル・システムまたはその他のKubernetesボリューム・タイプに存在できます
2.  宣言的な起動パラメータおよび目的の状態に基づいてサーバーを起動します。
3.  外部アクセス用のOIG/OAM/OUDサービスの公開
4.  オンデマンドでのサーバーの起動と停止によるOIG/OAM/OUDのスケーリング
5.  オペレータおよびWebLogicサーバーのログを公開してElasticsearchにログインし、Kibanaで操作します
6.  PrometheusおよびGrafanaを使用したOIGインスタンスの監視

_推定時間:_ 90分

### 目標

このワークショップでは、次のことを学習します。

*   単一ノードのローカルkubernetesクラスタの設定
*   kubernetes環境でのOIGのデプロイ
*   kubernetes環境でのOAMのデプロイ
*   kubernetes環境でのOUDインスタンスの作成

### 前提条件

この演習では、次を想定しています。

*   Free Tier、有料またはLiveLabs Oracle Cloudアカウント

_ノート: **無料トライアル**・アカウントがある場合、無料トライアルの期限が切れると、アカウントは**Always Free**アカウントに変換されます。Always Free環境が使用可能でないかぎり、Free Tierワークショップは実行できません。**[Free TierのFAQページはここをクリックしてください。](https://www.oracle.com/cloud/free/faq.html)**_

## さらに学ぶ

*   [クベネット環境でのOIG](https://oracle.github.io/fmw-kubernetes/oig/)
*   [クベネット環境でのOAM](https://oracle.github.io/fmw-kubernetes/oam/)
*   [クベネット環境でのOUD](https://oracle.github.io/fmw-kubernetes/oud/)

## 確認

*   **著者** - NATDソリューション・エンジニアリング、Anuj Tripathi、Keerti R氏
*   **貢献者** - Keerti R、Anuj Tripathi
*   **最終更新者/日付** - NATDソリューション・エンジニアリング、Keerti R、2022年1月