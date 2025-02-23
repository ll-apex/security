# 設定の準備

## 概要

このラボでは、このワークショップの実行に必要なリソースの設定に必要なOracle Resource Manager (ORM)スタックのzipファイルをダウンロードする方法について説明します。このワークショップでは、Database Securityマーケットプレイス・イメージおよびVirtual Cloud Network (VCN)を実行するコンピュート・インスタンスが必要です。

見積時間: 20分

### 目標

*   ORMスタックのダウンロード
*   既存のVirtual Cloud Network (VCN)の構成

### 前提条件

この演習では、次を想定しています。

*   Oracle Cloudアカウント

## タスク1: Oracle Resource Manager (ORM)スタックのzipファイルのダウンロード

1.  環境を構築するために必要なリソース・マネージャのzipファイルをダウンロードするには、次のリンクをクリックします:

\- \[dbsec-mkplc-basics.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-basics.zip) \- \[dbsec-mkplc-advanced.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/https:/objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-advanced.zip) \- \[dbsec-mkplc-basics.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-basics.zip) \- \[dbsec-mkplc-avdf.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-avdf.zip) \- \[dbsec-mkplc-basics.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-basics.zip) \- \[dbsec-mkplc-basics.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-basics.zip) \- \[dbsec-mkplc-basics.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-basics.zip) \- \[dbsec-mkplc-basics.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-basics.zip) \- \[dbsec-mkplc-okv.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-okv.zip) \- \[dbsec-mkplc-basics.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-basics.zip) \- \[dbsec-mkplc-basics.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-basics.zip) \- \[dbsec-mkplc-basics.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-basics.zip) \- \[dbsec-mkplc-sqlfw.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-sqlfw.zip) \- \[dbsec-mkplc-storyhack.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-storyhack.zip) \- \[dbsec-mkplc-basics.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-basics.zip) \- \[dbsec-mkplc-basics.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-basics.zip)

2.  ダウンロード・フォルダに保存

このスタックを使用して、インスタンスで自己完結型/専用VCNを作成することを強くお薦めします。_Step 3_に進み、推奨事項に従います。既存のVCNを使用する場合は、次に示すように次のステップに進み、必要なエグレス・ルールで既存のVCNを更新します。

## タスク2: 既存のVCNへのセキュリティ・ルールの追加

このワークショップでは、特定の数のポートが使用可能である必要があります。専用のVCNを作成するデフォルトのORMスタック実行を使用して満たすことができる要件です。既存のVCNを使用するには、次のポートをエグレス・ルールに追加する必要があります

| ポート | 説明 |
| :-- | :-- |
| 22 | SSH |
| 80 | アプリケーション(http) |
| 6080 | noVNCリモートデスクトップ |
| {: title="必要なポート"} |  |

| ポート | 説明 |
| :-- | :-- |
| 443 | アプリケーション(https) |
| 7803 | Oracle Enterprise Manager |
| 8080 | Glassfishアプリケーション |
| 50002 | Glassfishアプリケーション |
| {: title="オプション・ポート- noVNCリモート・デスクトップ外のアプリケーション・アクセス用"} |  |

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

**次の演習に進む**ことができます。

## 確認

*   **著者** - NA Technology、マスター・プリンシパル・ソリューション・アーキテクト、Rene Fontcha氏
*   **コントリビュータ** - Kay Malcolm、製品マネージャ、データベース製品管理
*   **最終更新者/日付** - 2022年4月、テクニカル・プログラム・マネージャ、Marion Smith