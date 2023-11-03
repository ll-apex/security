# 概要

## このワークショップについて

### 概要

_ワークショップ完了までの推定時間_: 425分

このワークショップは、Oracle Database Securityの機能に特化したハンズオン・ラボの2番目の部分です。最初のワークショップは、_DBセキュリティの基本_を参照してください。

シンプルなインターネット接続で数分でデプロイされたOCIアーキテクチャに基づいて、Oracle Databaseセキュリティ製品マネージャ・チームが事前構成した完全な環境でDBセキュリティのユースケースをテストできます。

これで、PC上の重要なリソース(ストレージ、CPUまたはメモリー)やマスターする複雑なツールが不要になりました。これにより、すべての新しいDBセキュリティ機能のリズムで完全に自律的に検出できます。

_Livelabs - Database Security Advanced(2022年5月)_のプレビューを見る[](youtube:h4gXFpOxWZU)

### コンポーネント

**DBセキュリティ・ハンズオン・ラボ(v5.3 - 2023年9月)**の完全なアーキテクチャは次のとおりです。

![DBSec LiveLabsアーカイブ](./images/dbseclab-archi.png "DBSec LiveLabsアーカイブ")

5つのVMで構成されます。

*   **DBSec- ラボVM**(すべてのワークショップで必須: ベースラインおよびアドバンスト・ワークショップ)
*   **Audit Vault Server VM** (高度なワークショップのみ)
*   **DB Firewall Server VM** (高度なワークショップのみ)
*   **Key Vault Server VM**(Advancedワークショップのみ)
*   **DB23c VM** (SQL Firewallワークショップのみ)

この2回目のワークショップでは、様々なリソースを使用してこれらのVMを操作します。

*   SSHターミナル・クライアント
*   Glassfish HRアプリケーション
*   Oracle Enterprise Manager 13c
*   Oracle SQL Developer
*   Oracle Golden Gate Webコンソール(AVDFラボの場合のみ)
*   Oracle AVDF Webコンソール(AVDFラボの場合のみ)
*   Oracle Key Vault Webコンソール(OKVラボの場合のみ)

このワークショップの経験が可能なかぎり最適になるように、「ラボ: _環境の初期化_」を実行して、これらのリソースがすべて正しく設定されていることを確認しないでください。

### 目標

このハンズオン・ラボでは、データベースをベースラインから最大セキュリティ・アーキテクチャ(MSA)まで保護および保護するようにDBセキュリティ機能を構成する方法を学習できます。

この2番目の部分では、Oracle Database Securityの高度な製品とソリューションが次のように表示されます。

*   Oracle Advanced Securityオプション
    *   **Oracle Transparent Data Encryption** (TDE)
    *   **Oracleデータ・リダクション**
*   **Oracle Database Vault**(DV)
*   **Oracle Label Security**(OLS)
*   **Oracle Data Masking and Subsetting** (ミリ秒)
*   **Oracle Audit Vault and Database Firewall**(AVDF)
*   **Oracle Key Vault** (OKV)

DB Security PMsチーム全体があなたに素晴らしいワークショップを願っています!

[次の演習に進む](#next)ことができます。

## 謝辞

*   **著者** - Database Security PM、Hakim Loumi氏
*   **貢献者** - Rene Fontcha
*   **最終更新者/日付** - Hakim Loumi、データベース・セキュリティPM - 2023年9月