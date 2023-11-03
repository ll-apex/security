# 概要

このワークショップのこのセクションでは、AutoUpgradeユーティリティを使用したOracle Databasesのアップグレードについて説明します。AutoUpgradeユーティリティは、アップグレード前に問題を識別し、アップグレード前とアップグレード後のアクションを実行し、アップグレードをデプロイし、アップグレード後のアクションを実行して、ユーザーの介入なしにアップグレードされたOracleを起動します。

AutoUpgradeユーティリティは、アップグレードの開始前、アップグレード・デプロイメント中、およびアップグレード後のチェックおよび構成の移行中に、アップグレード・プロセスを自動化するように設計されています。AutoUpgradeは、新しいOracle Databaseリリースのバイナリをダウンロードし、新しいリリースのOracleホームを設定した後に使用します。AutoUpgradeを使用すると、データベース・デプロイメントごとに必要に応じてカスタマイズされた単一の構成ファイルを使用して、複数のOracle Databaseデプロイメントを同時にアップグレードできます。AutoUpgradeを使用して、非CDBデータベースをPDBデータベースにアップグレードすることもできます。

推定ワークショップ時間:60分

### ラボ

*   AutoUpgrade
*   AutoUpgrade - 非CDBからCDB

### 前提条件

*   Oracle Cloudアカウント- このワークショップのLiveLabsランディング・ページを参照して、サポートされている環境を確認してください
*   viの作業知識

_ノート: **無料トライアル**・アカウントがある場合、無料トライアルの期限が切れると、アカウントは**Always Free**アカウントに変換されます。Always Free環境が使用可能でないかぎり、Free Tierワークショップは実行できません。**[Free TierのFAQページはここをクリックしてください。](https://www.oracle.com/cloud/free/faq.html)**_

[次の演習に進む](#next)ことができます。

## Oracle Database 21cについて

21c世代のOracleのコンバージド・データベースは、すべてのデータ型(リレーショナル、JSON、XML、空間、グラフ、OLAPなど)と業界をリードするパフォーマンス、スケーラビリティ、可用性、セキュリティを、すべての運用、分析、その他の混合ワークロードに対して提供します。

![Oracle DB 21cの利点](images/21c-support.png "Oracle DB 21cの利点") Database 21cでの主な更新内容は次のとおりです。

*   JSONバイナリ・データ型
*   ブロックチェーン表
*   Pythonによる自動機械学習
*   シャーディング、データベース・インメモリーおよびグラフ分析の拡張。

21cによって、顧客はできます

*   ITコストと複雑性の削減
*   イノベーションを解放
*   強力なデータ駆動型アプリケーションの開発

## さらに学ぶ

*   [Oracle Databaseブログ](http://blogs.oracle.com/database)
*   [Oracle Database 21cの概要](https://blogs.oracle.com/database/introducing-oracle-database-21c)

## 謝辞

*   **作成者** - Donna Keesling、データベースUAチーム
*   **貢献者** - Kay Malcolm、David Start、Kamryn Vinson、Anoosha Pilli
*   **最終更新者/日付** - Kay Malcolm、2020年11月