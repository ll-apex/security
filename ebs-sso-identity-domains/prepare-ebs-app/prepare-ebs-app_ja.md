# 設定の準備

## 概要

このラボでは、EBSアプリケーションでシングル・サインオン機能を有効にするために必要な変更を行う方法について説明します。また、SSOフローのテストに使用されるEBSアプリケーションとOCIアイデンティティ・ドメインで管理ユーザーを作成します。

_推定ラボ時間:_ 20分

### 目標

*   OCI IAMアイデンティティ・ドメインでのOracle E-Business Suiteのシステム管理者の作成
*   Oracle E-Business Suiteのシステム管理者の電子メール・アドレスの更新
*   Oracle E-Business Suiteプロファイルの更新
*   EBSアプリケーションの再起動

### 前提条件

この演習では、次を想定しています。

*   前の演習を正常に実行しました
*   ローカル・システムにインストールされたJDK 8以上

## タスク1: OCI IAMアイデンティティ・ドメインでのOracle E-Business Suiteのシステム管理者の作成

Oracle E-Business Suite (EBS)のシステム管理者に対応するユーザーをOracle IAMアイデンティティ・ドメインに作成してください。そうしないと、認証にOCI IAMアイデンティティ・ドメインを使用するようにEBSが構成された後、システム管理者がEBSコンソールにログインできなくなります。

1.  OCI IAMアイデンティティ・ドメインにサインインして、**OCIコンソール**にアクセスします。ログインしたら、**「アイデンティティとセキュリティ」**の下の**「ドメイン」**に**ナビゲート**します。ここで、前にプロビジョニングした**アイデンティティ・ドメイン**を選択します。
    
    ![イメージ9](./images/image9.png "イメージ9")
    
2.  **「ユーザー」**の**「ユーザーの追加」ウィンドウ**で、次の値を指定し、**「作成」**をクリックします。
    
    1.  **名**: EBS
    2.  **姓**: システム管理者
    3.  「ユーザー名として電子メール・アドレスを使用」の選択を解除します。
    4.  **ユーザー名**: sysadmin
    5.  **電子メール**: Oracle E-Business SuiteのSYSADMINアカウントに設定された電子メール・アドレスを指定します。
    
    ![イメージ10](./images/image10.png "イメージ10")
    

## タスク2: Oracle E-Business Suiteのシステム管理者の電子メール・アドレスの更新

指定した電子メール・アドレスとOracle Identity Cloud Serviceの対応するユーザーが一致するように、Oracle E-Business SuiteのSYSADMINユーザーの電子メール・アドレスを更新します。

1.  管理者(sysadminなど)としてOracle E-Business Suiteアプリケーションにログインします。
    
2.  Oracle E-Business Suiteホームページで、**「ナビゲータ」**をスクロール・ダウンし、**「ユーザー管理」**を展開して**「ユーザー」**をクリックします。
    
3.  **ユーザー・メンテナンス**・ページで、ユーザー名**SYSADMIN**で検索し、**SYSADMIN**ユーザーの**更新アイコン**をクリックします。
    
    ![イメージ11](./images/image11.png "イメージ11")
    
4.  **「電子メール」**フィールド値を、OCI IAMアイデンティティ・ドメインでのシステム管理者ユーザーの作成中に指定したものと同じ電子メール・アドレスで更新し、**「適用」**をクリックします。
    
    ![イメージ12](./images/image12.png "イメージ12")
    
5.  Oracle E-Business Suiteアプリケーションを閉じます。
    

## タスク3: Oracle E-Business Suiteプロファイルの更新

_Oracle E-Business Suiteローカル・ログイン・ページを使用するかわりに、E-Business-Suite認証されていないユーザーをE-Business Suite AsserterにリダイレクトするようにOracle E-Business Suiteを構成するには、次のステップに従います。_

1.  Oracle E-Business Suiteホームページで、ナビゲータをスクロール・ダウンし、**「機能管理者」**を展開して**「コア・サービス」**タブをクリックし、**「プロファイル」タブ**をクリックします。
    
2.  「検索」、「プロファイル値」、「コード」フィールドに**App%Agent%**と入力し、**「検索」**をクリックします。
    
    ![イメージ18](./images/image18.png "イメージ18")
    
3.  「プロファイル値の定義: **アプリケーション認証エージェント**」ページで、「サイト値」フィールドに**E-Business Suite AsserterのURL - https://ebsasserter.example.com:7004/ebs**と入力し、**保存**します。
    
4.  **「プロファイル」タブ**に戻り、「検索」に**%SSO%Type%**と入力し、**SSWAからSSWAw/SSO**に**APPS\_SSO**コード・エントリを更新して、プロファイルを**保存**します。
    
    ![イメージ19](./images/image19.png "イメージ19")
    

5 **「プロファイル」タブ**に戻り、「検索」に**%Oracle Applications Session%**と入力し、値を**HOST**から**DOMAIN**に更新して、プロファイルを**保存**します。

![イメージ20](./images/image20.png "イメージ20")

**ノート** EBSアプリケーションを実行してこれらのプロファイル・ルールを更新するには、ローカル・システムにJAVAがインストールされている必要があります。

## タスク4: Oracle E-Business Suiteの再起動

プロファイルの変更が完了したら、EBSサーバーにSSH接続し、次のコマンドを実行します。

    $ sudo hostname demoebs.example.com
    # sudo su - oracle
    $ /u01/install/APPS/scripts/stopapps.sh
    $ /u01/install/APPS/scripts/startapps.sh
    
    

**ノート**必要に応じて、前述のホスト名を使用してください。

**次の演習に進みます。**

## 謝辞

*   **著者** - Gautam Mishra、Aqib Bhat、Samratha S P
*   **貢献者** - Chetan Soni氏、Sagar Takkar氏
*   **サポート担当者** - Deepak Rao Narasimha Gajendragad
*   **リード元** - Deepthi Shetty
*   **最終更新者/日付** - Gautam Mishra 2023年5月