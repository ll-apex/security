# ORMスタックをデプロイして、Linux PAMモジュールおよびアイデンティティ・ドメインを構成します

## 概要

このスタックを使用すると、**Linuxサーバーおよびアイデンティティ・ドメイン**を構成できます。このスタックの一部として、機密アプリケーションが**アイデンティティ・ドメイン**の下に作成され、サンプル・ユーザーが作成されます。

_見積時間:_ 20分

### 目標

*   **SSSDを使用したLinux-PAM**のインストールおよび構成
*   **「アイデンティティ・ドメイン」**の下に**「機密アプリケーション」**を作成し、サンプル_POSIX_ユーザーおよびPOSIXグループを作成します。

### 前提条件

*   **Stack2- Configure.zip**がダウンロードされたら、**Stack1- Deploy.zip**のデプロイ中に作成された更新された**SSH.key**ファイルを使用します。

## タスク1: リソース・マネージャを介した構成スタックのデプロイ

1.  OCIコンソールにログインしたら、**「開発者サービス」**に移動し、**「リソース・マネージャ」**の下の**「スタック」**を選択します。**「スタックの作成」**をクリックします
    
    ![スタック](./images/stacks.png "スタック")
    
    ![作成スタック](./images/create-stacks.png "作成スタック")
    
2.  スタックの作成ウィザードで、**「スタック2 - Configure.zip」**オプションを選択し、前の演習でダウンロードした**「デプロイ」**スタックを参照してアップロードします。**「次へ」**をクリックします。
    
    ![アップロード](./images/upload.png "アップロード")
    
3.  次に、**「変数の構成」**セクションで、次の値を入力し、**「次へ」**をクリックします。
    
    ![configure-variables](./images/configure-variables.png "configure-variables")
    
    1.  前に作成した_Linuxコンピュート・インスタンスのパブリックIPアドレス_
    2.  _アイデンティティ・ドメインURL_ - デプロイ済ドメインのドメインURL。**ノート**ドメインURLの最後から**:443**を削除します。
    3.  _クライアントID_ - OCI IAM機密アプリケーションのクライアントIDを入力してください
    4.  _クライアント・シークレット_ - OCI IAM機密アプリケーションのクライアント・シークレットを入力してください
4.  **「詳細の確認」**で、構成を確認し、**「作成」**をクリックします。**「適用の実行」**が選択されていることを確認します。
    
    ![レビュー](./images/review.png "レビュー")
    

**ノート**スタックが完了するまでに約15分かかる場合があります。**job**が成功するまでお待ちください。

## まとめ

この演習では、Linuxサーバー上でLinux Pluggable Authentication Module (PAM)を正常にデプロイおよび構成し、機密アプリケーション、_POSIX_ユーザーおよび_POSIX_グループを作成するようにアイデンティティ・ドメインを構成できました。

**次の演習に進みます。**

## 確認

*   **著者** - Gautam Mishra、Aqib Bhat
*   **貢献者** - Deepthi Shetty
*   **最終更新者/日付** - Gautam Mishra 2023年7月