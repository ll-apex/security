# 概要

## このワークショップについて

このワークショップでは、Oracle Transport Layer Security (TLS)ネットワーク暗号化の機能について説明します。これにより、移動中のデータを暗号化および保護するためにこの機能を構成する方法を学習できます。

_推定ワークショップ時間_: 45分

### TLSについて

TLSは、移動中のデータを暗号化するための標準ベースのアプローチです。TLSは一方向認証または相互双方向認証を提供するため、違反の可能性を最小限に抑えます。

### 目標

*   1方向TLSを使用してデータベース通信を正常に保護
*   TLSを構成する前にネットワークトラフィックが暗号化されていないことを確認します
*   ルート・ウォレットおよび自己署名ルートCA証明書の作成
*   データベース・サーバー・ウォレットの作成および証明書リクエストの作成
*   ルートCA証明書によるデータベース証明書の署名
*   データベース・ウォレットへのCAルート証明書およびデータベース・サーバー証明書の追加
*   CAルート証明書をクライアント・トラスト・ストアにインポートします(Linux、Windowsのみ)
*   TLSネットワーク暗号化の設定
*   TLSネットワーク暗号化を使用して接続し、トラフィックが暗号化されていることを確認します
*   新しいOSユーザーを作成し、SQLトラフィックを暗号化します。
*   (オプション)暗号化の無効化

DB Security PMsチーム全体があなたに素晴らしいワークショップを願っています!

### 前提条件

この演習では、次を想定しています。

*   Free Tier、有料またはLiveLabs Oracle Cloudアカウント
*   次を完了しました:
    *   演習: 設定の準備(_無料層_および_有料テナント_のみ)
    *   演習: 環境設定
    *   演習: 環境の初期化

## 確認

*   **著者** - 北米スペシャリスト・ハブ、ソリューション・エンジニア、Stephen Stuart & Alpha Diallo氏
*   **コントリビュータ** - データベース・セキュリティ製品マネージャ、Richard C. Evans氏
*   **最終更新者/日付** - Stephen Stuart & Alpha Diallo、2023年4月