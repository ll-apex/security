# 設定の準備

## 概要

この演習では、このワークショップの実行に必要なリソースの設定に必要なOracle Resource Manager (ORM)スタックのzipファイルをダウンロードする方法を示します。このワークショップでは、Oracle Identity Governance MarketplaceイメージおよびVirtual Cloud Network (VCN)を実行するコンピュート・インスタンスが必要です。

_推定ラボ時間:_ 15分

### 目標

*   ORMスタックのダウンロード
*   既存のVirtual Cloud Network (VCN)の構成

### 前提条件

この演習では、次を想定しています。

*   Oracle Free Tierまたは有料クラウド・アカウント

## タスク1: Oracle Resource Manager (ORM)スタックのzipファイルのダウンロード

1.  環境を構築するために必要なリソース・マネージャのzipファイルをダウンロードするには、次のリンクをクリックします: [oig-mkplc-freetier.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/p/Ksdi-p916TjexGrvk5NhKAyTkVVdJbaiDedy6Y-Uhwv6EfwDGX8FsT5W4mUP0sb5/n/natdsecurity/b/stack/o/oig-mkplc-freetier.zip)
    
2.  ダウンロード・フォルダに保存します。
    

このスタックを使用して、インスタンスで自己完結型/専用VCNを作成することを強くお薦めします。_Step 3_に進み、推奨事項に従います。既存のVCNを使用する場合は、次に示すように次のステップに進み、必要なイングレス・ルールで既存のVCNを更新します。

## タスク2: 既存のVCNへのセキュリティ・ルールの追加

このワークショップでは、特定の数のポートが使用可能である必要があります。専用のVCNを作成するデフォルトのORMスタック実行を使用して満たすことができる要件です。既存のVCNを使用するには、次のポートをイングレス・ルールに追加する必要があります

| ポート | 説明 |
| :-- | :-- |
| 22 | SSH |
| 6080 | noVNCリモートデスクトップ |

1.  _ネットワーキング>> Virtual Cloud Networks_にアクセスします
2.  ネットワークの選択
3.  「リソース」で、「セキュリティ・リスト」を選択します。
4.  「Create Security List」ボタンの下にある「Default Security Lists」をクリックします。
5.  「Add Ingress Rule」ボタンをクリックします。
6.  次を入力します:
    *   ソースCIDR: 0.0.0.0/0
    *   宛先ポート範囲: _前述の表を参照_
7.  「イングレス・ルールの追加」ボタンをクリックします。

## タスク3: コンピュートの設定

前述の2つのステップの詳細を使用して、演習_「環境設定」_に進み、Oracle Resource Manager (ORM)および次のいずれかのオプションを使用してワークショップ環境を設定します。

*   スタックの作成: _コンピュートとネットワーキング_
*   スタックの作成: 前述の_ステップ2_に従ってセキュリティ・リストが更新された既存のVCNを使用した_コンピュートのみ_

## 確認

*   **著者** - NATDソリューション・エンジニアリング、Anuj Tripathi、Brijith TG、Keerti R氏
*   **貢献者** - Keerti R、Brijith TG、Anuj Tripathi
*   **最終更新者/日付** - 2021年6月、NATDソリューション・エンジニアリング、Keerti R氏