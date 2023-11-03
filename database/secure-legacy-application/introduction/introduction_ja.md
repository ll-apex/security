# Oracle Autonomous Database上のOracle Database Vaultを使用したレガシー・アプリケーションの保護

## 概要

Oracle Autonomous Databaseは、自己管理機能に加えて、Oracle Database Vaultとも統合されます。Oracle Database Vaultは、機密データへのアクセスを制限し、権限のないユーザーがアクセスまたは操作できないようにすることで、セキュリティの追加レイヤーを提供するセキュリティ拡張ツールです。Oracle Autonomous Databaseの自己管理機能をOracle Database Vaultのセキュリティ機能と組み合せることで、組織はパフォーマンスの向上とセキュリティの向上の両方を利用できます。

Oracle Autonomous Databaseの主な機能の1つは、最適なパフォーマンスのために自動的にチューニングする機能です。これは、データベース・ワークロードを継続的に分析し、データベース構成をリアルタイムで調整する機械学習アルゴリズムを使用することで実現されます。これにより、ワークロードやデータ量が時間の経過とともに変化する場合でも、Oracle Autonomous Databaseは一貫して高いパフォーマンスを実現できます。

全体として、Oracle Autonomous DatabaseとOracle Database Vaultは、データベース・パフォーマンスの最適化やセキュリティの向上を目指す組織にとって貴重なツールです。これら2つのテクノロジを組み合せることで、組織はOracle Autonomous Databaseの自己管理機能を利用しながら、Oracle Database Vaultのセキュリティ機能を使用して機密データを保護できます。

![ラボ・アーキテクチャ](images/intro-architecture.png)

所要時間: 90分

### 目標

この演習では、次のタスクを実行します。

*   GlassfishレガシーHRアプリケーションへの接続
*   Autonomous Databaseインスタンスの構成
*   Glassfishアプリケーションのデータのロードおよび検証
*   Database Vaultを有効にし、HRアプリケーションを確認します。
*   EMPLOYEESEARCH\_PRODスキーマへの接続の識別
*   Database Vaultを有効にしたGlassfish HRアプリケーション機能の確認

### 前提条件

このワークショップでは、次を想定しています。

*   Oracle Cloud Infratructureタンナンシ・アカウント
*   データベースに関する知識が必要
*   クラウドとデータベースの用語を理解しておくと役立ちます
*   Oracle Cloud Infrastructure(OCI)に精通しておくと便利です。
*   データ保護とセキュリティに関する基本的な理解は、
*   Linux/Bashコマンドに精通していることが役立ちます。

_ノート: このワークショップを通じて、Oracle Cloudでリソースを見つけることに苦労した場合、コンパートメントとリージョンの両方がリソースを作成した場所に対応していることを確認してください。_

## Oracle Database Vaultについてさらに学習しますか。

*   [Oracle Database Vaultランディング・ページ](https://www.oracle.com/security/database-security/database-vault/)
*   [Oracle Database Vaultの概要](https://docs.oracle.com/database/121/DVADM/dvintro.htm#DVADM001)
*   [追加のDatabase Vault LiveLab](https://apexapps.oracle.com/pls/apex/r/dbpm/livelabs/view-workshop?wid=682&clear=RR,180&session=100352880546347)

## 確認

*   **著者** - Ethan Shmargad氏、北米スペシャリスト・ハブ
*   **作成者** - シニア・プリンシパル・プロダクト・マネージャー、Richard Evans
*   **最終更新者/日付** - Ethan Shmargad、2022年9月