# 認証フローの検証

## 概要

このラボでは、OCI IAMを使用してLinuxサーバーへの認証をテストする方法について説明します。

_見積時間:_ 5分

### 目標

*   _default_パスワードを変更します。
*   _2番目の_ファクタを使用して、OCI IAMを使用してLinuxサーバーへの認証を検証します。

## タスク1: OCI IAM POSIXユーザーのデフォルト・パスワードの変更

1.  次に、サンプルSSOテスト・ユーザーとして作成されたユーザーを示します。
    
    ![ユーザー](./images/users.png "ユーザー")
    
    **デフォルト・パスワード** - "Welcome@1234567890"
    
2.  新しく作成したドメインを使用してOCIコンソールにログインし、_POSIX_ユーザーの資格証明を入力します。
    
    ![ログイン](./images/sign-in.png "ログイン")
    
3.  デフォルトのパスワードをリセットします。
    
    ![パスワード・リセット](./images/password-reset.png "パスワード・リセット")
    
4.  _セキュアな検証_を有効にし、モバイル・デバイスを登録します。
    
    ![enable-mfa](./images/enable-mfa.png "enable-mfa")
    
    ![スキャン-QR](./images/scan-qr-code.png "スキャン-QR")
    
5.  **「Done」**をクリックし、_タスク2_に進みます。
    
    ![登録する](./images/enrolled.png "登録する")
    

## タスク2: MFAによる認証の検証

**Stack 2- Configure**が正常に配備されたら、次に示す手順を実行してください。

*   OCI IAM Linuxプラガブル認証モジュール(PAM)がインストールされているLinux環境にSSHで接続します。
    
*   プロンプトが表示されたら、OCI IAM _POSIX_ユーザーのパスワードを入力します。_PUSH_通知が登録されたモバイル デバイスに送信されます。通知の**「許可」**をタップし、画面の**\[Enter\]**を押します。
    
    ![検証](./images/validate.png "検証")
    

## まとめ

このラボでは、アイデンティティ・ドメインを使用して、テスト・ユーザーのパスワードおよび検証済ユーザー認証を_2番目の_ファクタとともにLinuxサーバーに正常に変更できました。

**次の演習に進みます。**

## 確認

*   **著者** - Gautam Mishra、Aqib Bhat
*   **貢献者** - Deepthi Shetty
*   **最終更新者/日付** - Gautam Mishra 2023年7月