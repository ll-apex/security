# 概要

このワークショップのこのセクションでは、Oracle AuditポリシーのためのOracle Database 21cの機能拡張について説明します。このリリース以降、SQL文を実行する現在のユーザーに統合監査ポリシーが適用されます。

以前のリリースでは、統合監査ポリシーは、SQL文が実行されるトップレベル・ユーザー・セッション(ログイン・ユーザー・セッション)を所有するユーザーに強制されていました。

現行ユーザーがログイン・ユーザーと異なるシナリオは次のとおりです。

*   トリガーの実行
*   定義者権限プロシージャの実行
*   ビューの評価時に実行されるファンクションおよびプロシージャ

推定ワークショップ時間:60分

### ラボ

*   統合監査ポリシー
*   STIGの統合監査ポリシー
*   監査ポリシーのためのSYSLOG宛先

### 前提条件

*   Oracle Cloudアカウント- このワークショップのLiveLabsランディング・ページを参照して、サポートされている環境を確認してください
*   viの作業知識

_ノート: **無料トライアル**・アカウントがある場合、無料トライアルの期限が切れると、アカウントは**Always Free**アカウントに変換されます。Always Free環境が使用可能でないかぎり、Free Tierワークショップは実行できません。**[Free TierのFAQページはここをクリックしてください。](https://www.oracle.com/cloud/free/faq.html)**_

[次の演習に進む](#next)ことができます。

## Oracle Database 21cについて

21c世代のOracleのコンバージド・データベースは、すべてのデータ型(リレーショナル、JSON、XML、空間、グラフ、OLAPなど)と業界をリードするパフォーマンス、スケーラビリティ、可用性、セキュリティを、すべての業務、分析、その他の混合ワークロードに提供します。

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