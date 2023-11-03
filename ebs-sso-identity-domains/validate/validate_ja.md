# 検証

## 概要

このラボでは、**OCI IAMアイデンティティ・ドメイン**を介してEBSアプリケーションへのSSOフローをテストする方法を示します

### 目標

*   EBS Asserter構成の検証
*   Oracle E-Business Suiteによるシングル・サインオンのテスト

## タスク1: EBS Asserter構成の検証

**スタック2 - 構成**が正常にデプロイされ、EBSプロファイルの変更が実行されたら、次に示すEBSアサータURLにアクセスしてください。

*   **https://ebsasserter.example.com:7004/ebs/validate**

ブラウザには、EBS Asserter構成の結果が表示されます。

![取得1](./images/capture1.png "取得1")

## タスク2: SSOテスト・ユーザーのデフォルト・パスワードの変更

次に、サンプルSSOテスト・ユーザーとして作成されたユーザーを示します。

![キャプチャ3](./images/capture3.png "キャプチャ3")

    **Default Password** - "Welcome@1234567890"
    

## タスク3: Oracle E-Business Suiteを使用したシングル・サインオンのテスト

1.  EBS AsserterダイレクトURLリンクを使用したSSOのテスト
    
    1.  ブラウザ・ウィンドウを開き、EBS AsserterのURLを入力します- **https://ebsasserter.example.com:7004/ebs**
    2.  **OCI IAMアイデンティティ・ドメインのサインイン**・イン・ページが表示されます。前に作成したユーザーの**ユーザー名とパスワード**を使用してサインインします。
    3.  認証に成功すると、ユーザーは**EBS資格証明**を入力せずに**Oracle E-Business Suite**ホーム・ページにリダイレクトされます。
    4.  **Oracle EBS**ホームページが表示された場合は、**ログイン・ユーザー名**を確認します。
    5.  Oracle EBSからログアウトします。ブラウザは、**OCI IAMアイデンティティ・ドメインのサインイン**・イン・ページにリダイレクトされます。
    
    ![取得2](./images/capture2.png "取得2")
    

**次の演習に進みます。**

## 謝辞

*   **著者** - Gautam Mishra、Aqib Bhat、Samratha S P
*   **貢献者** - Chetan Soni氏、Sagar Takkar氏
*   **サポート担当者** - Deepak Rao Narasimha Gajendragad
*   **リード元** - Deepthi Shetty
*   **最終更新者/日付** - Gautam Mishra 2023年5月