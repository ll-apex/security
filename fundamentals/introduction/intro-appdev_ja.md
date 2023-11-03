# 概要

このワークショップのこのセクションでは、Oracle Database 21cの最新リリースで使用可能な新しい演算子、パラメータ、式およびSQLマクロについて説明します。Oracle 21cでは、EXCEPT、EXCEPT ALLおよびINTEREST ALLという3つの新しい演算子が導入されています。SQL集合演算子は、ANSI SQLで定義されているすべてのキーワードをサポートするようになりました。新しい演算子EXCEPT \[ALL\]は機能的にはMINUS \[ALL\]と同等です。MINUSおよびINTERSECT演算子は、キーワードALLをサポートするようになりました。

データベースの起動時に評価される初期化パラメータに式を使用できます。現在のシステム構成および環境を考慮した式を指定できるようになりました。これは、Oracle Autonomous Database環境で特に役立ちます。

SQLマクロ(SQM)を作成して、共通のSQL式および文を、他のSQL文で使用できる再利用可能なパラメータ化された構造体に分解できます。SQLマクロは、計算およびビジネス・ロジックをカプセル化するスカラー式(通常はSELECTリスト、WHERE、GROUP BYおよびHAVING句で使用)とすることも、表式(通常はFROM句で使用)とすることもできます。

推定ワークショップ時間:60分

### ラボ

*   新しいset演算子
*   初期化パラメータの式
*   SQMスカラー式および表式
*   インメモリーでのテキスト/JSONの検索

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

Autonomous Databaseで21cを使用すると、次のことができます:

*   ITコストと複雑性の削減
*   イノベーションを解放
*   強力なデータ駆動型アプリケーションの開発

## さらに学ぶ

*   [Oracle Databaseブログ](http://blogs.oracle.com/database)
*   [Oracle Database 21cの概要](https://blogs.oracle.com/database/introducing-oracle-database-21c)

## 確認

*   **作成者** - Donna Keesling、データベースUAチーム
*   **貢献者** - Kay Malcolm、David Start、Kamryn Vinson、Anoosha Pilli
*   **最終更新者/日付** - Kay Malcolm、2020年3月