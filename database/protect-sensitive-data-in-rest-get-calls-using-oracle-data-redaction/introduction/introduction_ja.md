# Oracle Data Redactionを使用したREST GETコールの機密データの保護

## 概要

Oracle Data Redactionは、機密データをリアルタイムでマスクし、不正な開示から保護できる高度なセキュリティ機能です。この機能は、Autonomous Databaseサブスクリプションに含まれており、レポートに機密情報を表示したり、GET APIを介して他のアプリケーションに送信するなどの読取り専用シナリオに特に役立ちます。

`DMBS_REDACT PL/SQL`パッケージは、リダクション・ポリシーを管理し、特定の列およびリダクション形式を構成するために使用されます。

このワークショップでは、Oracle REST Data Services (ORDS)でOracle Data Redactionを使用してGETレスポンスでデータをリダクションし、機密データのプライバシを確保する方法を学習します。このプロセスには、ORDSを介して使用可能にする表を有効にするREST、特定の列および表に対するリダクション・ポリシーの作成、使用するリダクション機能の指定が含まれます。明確なデータを含むレスポンスと、機密データがリダクションされたレスポンスを対比できます。

![ラボ・アーキテクチャ](images/lab-architecture.png)

推定ワークショップ時間:42分

### 目標

この演習では、次のタスクを実行します。

*   Autonomous Database環境を構成します。
*   リダクションを使用して、REST GETコールを匿名化します。
*   環境をリセットします。

### 前提条件

このワークショップでは、次を想定しています。

*   Oracle Cloud Infrastructureテナンシ・アカウント。
*   データベースに関する知識が必要
*   クラウドとデータベースの用語を理解しておくと役立ちます
*   Oracle Cloud Infrastructure(OCI)に精通しておくと便利です。
*   RESTFULサービスの基本的な理解が役立ちます

_ノート: このワークショップを通じて、Oracle Cloudでリソースを見つけることに苦労した場合、コンパートメントとリージョンの両方がリソースを作成した場所に対応していることを確認してください。_

## Oracle Data Redactionの詳細を参照してください。

*   [Oracle Data Redactionの概要](https://docs.oracle.com/en/database/oracle/oracle-database/21/asoag/introduction-to-oracle-data-redaction.html#GUID-82EA9712-387C-4D3A-BB72-F64A707C67CA)
*   [Oracle Data RedactionのFAQ](https://www.oracle.com/technetwork/database/options/data-masking-subsetting/learnmore/faq-security-asdr-external-3215961.pdf)

## 確認

*   **著者** - Alpha Diallo & Ethan Shmargad、北米スペシャリスト・ハブ
*   **作成者** - Pedro Lopes、データベース・セキュリティ製品マネージャー
*   **最終更新者/日付** - Alpha Diallo & Ethan Shmargad、2023年2月