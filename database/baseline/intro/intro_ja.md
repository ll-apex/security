# 概要

## このワークショップについて

### 概要

_ワークショップ完了までの推定時間_: 310分

このワークショップは、Oracle Database Securityの機能に特化したハンズオン・ラボの最初の部分です。2つ目のワークショップは、_DB Security Advanced_を参照してください。

シンプルなインターネット接続で数分でデプロイされたOCIアーキテクチャに基づいて、Oracle Databaseセキュリティ製品マネージャ・チームが事前構成した完全な環境でDBセキュリティのユースケースをテストできます。

これで、PC上の重要なリソース(ストレージ、CPUまたはメモリー)やマスターする複雑なツールが不要になりました。これにより、すべての新しいDBセキュリティ機能のリズムで完全に自律的に検出できます。

_Livelabs - データベース・セキュリティの基本(2022年5月)_のプレビューを見る[](youtube:tyyZmW4YyPk)

### コンポーネント

**DBセキュリティ・ハンズオン・ラボ(v5.3 - 2023年9月)**の完全なアーキテクチャは次のとおりです。

![DBSec LiveLabsアーカイブ](./images/dbseclab-archi.png "DBSec LiveLabsアーカイブ")

5つのVMで構成されます。

*   **DBSec- ラボVM**(すべてのワークショップで必須: ベースラインおよびアドバンスト・ワークショップ)
*   **Audit Vault Server VM** (高度なワークショップのみ)
*   **DB Firewall Server VM** (高度なワークショップのみ)
*   **Key Vault Server VM**(Advancedワークショップのみ)
*   **DB23c VM** (SQL Firewallワークショップのみ)

この第1部では、さまざまなリソースを使用してこれらの仮想マシンと対話します。

*   SSHターミナル・クライアント
*   Glassfish HRアプリケーション
*   Oracle Enterprise Manager 13c
*   (オプション) SQL Developer

このワークショップの経験が可能なかぎり最適になるように、「ラボ: _環境の初期化_」を実行して、これらのリソースがすべて正しく設定されていることを確認しないでください。

### 目標

このハンズオン・ラボでは、データベースをベースラインから最大セキュリティ・アーキテクチャ(MSA)まで保護および保護するようにDBセキュリティ機能を構成する方法を学習できます。

この最初の部分では、Oracle Database Securityのベースライン製品およびソリューションが次のように表示されます。

*   **Oracle Database Security Assessment Tool** (DBSAT)
*   **Oracle Native Network Encryption**(NNE)
*   **Oracle権限分析**
*   **Oracle統合監査**
*   **Oracle透過的機密データ保護(TSDP)**
*   **Oracle SQL Firewall**
*   **オンプレミス・データベース用のOracle Data Safe**

DB Security PMsチーム全体があなたに素晴らしいワークショップを願っています!

[次の演習に進む](#next)ことができます。

## 確認

*   **著者** - Database Security PM、Hakim Loumi氏
*   **貢献者** - Pedro Lopes、Richard Evans、Angeline Dhanarani、Bettina Schaeumer、Jody Glover
*   **最終更新者/日付** - Hakim Loumi、データベース・セキュリティPM - 2023年9月