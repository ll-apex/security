# IDCSでの条件付きMFAの有効化

## 概要

Multi-Factor Authentication (MFA)は、ユーザーのアイデンティティを検証するために複数の要素を使用する必要がある認証方法です。

Oracle Identity Cloud ServiceでMFAが有効になっている場合、ユーザーがアプリケーションにサインインすると、ユーザー名とパスワードの入力を求められます。これは、ユーザーが知っている最初の要素です。次に、ユーザーは2番目のタイプの検証を指定する必要があります。これは2ステップ検証と呼ばれます。2つの要素は連携して、追加情報または2つ目のデバイスを使用してユーザーのアイデンティティを検証し、ログイン・プロセスを完了することで、セキュリティの追加レイヤーを追加します。

ユーザーはますます接続され、どこからでもアカウントやアプリケーションにアクセスできます。管理者として、従来のユーザー名とパスワードの上にMFAを追加すると、データおよびアプリケーションへのアクセスを保護するのに役立ちます。これにより、オンラインでのアイデンティティ盗難や不正行為の可能性も減少し、アカウント・パスワードが侵害された場合でもビジネス・アプリケーションが保護されます。

推定ラボ時間: 20分

### 目標

この演習では、次のことを行います。

*   有効化するMFAファクタの選択
*   Salesforceのサインオン・ポリシーの定義
*   MFAを使用したエンド・ユーザーとしてのサインイン
*   ポリシーを介したアクセスのブロック
*   更新されたポリシーをテストします

### 前提条件

*   Oracle Free Tierまたは有料アカウント
*   Googleアカウント
*   Salesforce開発者アカウント
*   演習2が完了しました

## タスク1: MFAファクタの有効化

*   _ペルソナ_:
    *   管理者

1.  IDCS管理コンソールに管理者としてログインします。
    
        https://<your tenant>/ui/v1/adminconsole
        
2.  _「セキュリティ」_に移動し、_「MFA」_を選択します。
    
3.  有効化する_MFAファクタ_を選択し、_「保存」_をクリックします。
    
    ![イメージ](images/L5001.png)
    

## タスク2: Salesforceのサインオン・ポリシーの定義

*   _ペルソナ_:
    *   管理者

サインオン・ポリシーを使用すると、アイデンティティ・ドメイン管理者、セキュリティ管理者およびアプリケーション管理者は、ユーザーがOracle Identity Cloud Serviceにサインインできるかどうか、またはユーザーがOracle Identity Cloud Serviceにアクセスできないようにするかどうかを決定するためにOracle Identity Cloud Serviceで使用される基準を定義できます。

Oracle Identity Cloud Serviceには、デフォルトのサインオン・ルールを含むデフォルトのサインオン・ポリシーが用意されています。Oracle Identity Cloud Serviceは、Oracle Identity Cloud Serviceにサインインしようとするユーザーに対してルールの基準を評価します。デフォルトでは、このルールにより、すべてのユーザーがOracle Identity Cloud Serviceにサインインできます。これは、ユーザーが使用する認証(ユーザー名とパスワードを指定したローカル認証または外部アイデンティティ・プロバイダを使用した認証)で十分であることを意味します。

ただし、他のサインオン・ルールを追加することで、このポリシーに基づいて構築できます。これらのルールを追加することで、一部のユーザーがOracle Identity Cloud Serviceにサインインできないようにすることができます。または、サインインを許可するが、マイ・プロファイル・コンソールや「Identity Cloud Service」コンソールなど、Oracle Identity Cloud Serviceによって保護されているリソースにアクセスするための追加ファクタを要求するプロンプトを表示することもできます。

1.  _「セキュリティ」_にナビゲートし、_「ネットワーク・ペリメータ」_を選択します。
    
    ![イメージ](images/L5002.png)
    
        A network perimeter contains a list of IP addresses.
        
        After creating a network perimeter, you can prevent users from signing in to Oracle Identity Cloud Service if they use one of the IP addresses in the network perimeter. This is known as blacklisting. A blacklist contains IP addresses or domains that are suspicious. As an example, a user may be trying to sign in to Oracle Identity Cloud Service with an IP address that comes from a country where hacking is rampant.
        
        You can also configure Oracle Identity Cloud Service so that users can log in, using only IP addresses contained in the network perimeter. This is known as whitelisting, where users who attempt to sign in to Oracle Identity Cloud Service with these IP addresses will be accepted. Whitelisting is the reverse of blacklisting, the practice of identifying IP addresses that are suspicious, and as a result, will be denied access to Oracle Identity Cloud Service.
        
2.  _「追加」_をクリックして、新しいネットワーク・ペリメータを追加します。次に示すようにIP範囲を入力します。ここでは、認証が拒否されないようにダミー範囲またはダミー・アドレス(10.0.0.0など)を配置し、_「保存」_をクリックします。
    
    ![イメージ](images/L5003.png)
    
3.  _「セキュリティ」_に移動し、_「サインオン・ポリシー」_を選択します。Salesforceアプリケーションのサインオン・ポリシーを定義するには、_「追加」_をクリックします。
    
    ![イメージ](images/L5004.png)
    
        Note that a Default Sign-On Policy exists and is enabled by default. The default policy applies to all application that have not been explicitly mapped to a sign on policy.
        
4.  「詳細」セクションに_「ポリシー名」_および_「説明」_を入力し、_「>」_ボタンをクリックして次のセクションに移動します。
    
    ![イメージ](images/L5005.png)
    
5.  _「サインオン・ルール」_セクションで_「ルールの追加」_をクリックして新しいルールを追加します。
    
    ![イメージ](images/L5006.png)
    
6.  ユーザーがEmployeesグループに属している場合にMFAのプロンプトを表示するルールを作成します。次のパラメータが設定されていることを確認します。
    
    | パラメータ | 値 |
    | --- | --- |
    | ルール名 | 従業員のMFA |
    | これらのグループのメンバーである | 従業員 |
    | 追加ファクタの要求 | チェック済 |
    | 登録 | オプションです |
    | 信頼できるデバイスを使用する際の追加ファクタの頻度 | セッションごとに1回 |
    
7.  ルールを保存するには、_「保存」_をクリックします
    
    ![イメージ](images/L5007.png)
    
8.  2つ目のルールを追加するには、_「追加」_をクリックします。
    
    ![イメージ](images/L5008.png)
    
9.  次のパラメータが設定されていることを確認します。
    
    | パラメータ | 値 |
    | --- | --- |
    | ルール名 | ブラックリストに登録されたIP |
    | ユーザーのクライアントIPアドレス | これらの1つ以上のネットワークでブラックリストに登録されたIP |
    | アクセス | 拒否済 |
    
10.  ルールを保存するには、_「保存」_をクリックします
    
    ![イメージ](images/L5009.png)
    
        Note the Sign-On Rules are ordered. The actions (allow/deny/prompt for MFA) are determined by the first rule where the conditions match. If none of the rule conditions match the default action is deny.
        
11.  _ブラックリストに登録されたIP_ルールを上部に移動するには、順序変更アイコンをクリックし、ルールを上部にドラッグします。_「終了」_をクリックします。
    
    ![イメージ](images/L5010.png)
    
        The above rules are evaluated as below
        
        a. if user access is from a blacklisted IP the deny access.
        b. if user is a member of Salesforce Employee group, then prompt for MFA once per session.
        c. for all other users deny access.
        
12.  _「アプリケーション」_タブをクリックし、_「割当て」_ボタンをクリックします。
    
    ![イメージ](images/L5011.png)
    
13.  _「Salesforce」_アプリケーションを選択し、_「OK」_をクリックします。
    
    ![イメージ](images/L5012.png)
    
14.  Salesforceがサインオン・ポリシーに正常に追加されました。
    
    ![イメージ](images/L5013.png)
    
15.  _「サインオン・ポリシー」_に戻り、_「アクティブ化」_をクリックして_Salesforce_ポリシーをアクティブ化します。
    
    ![イメージ](images/L5014.png)
    

## タスク3: MFAを使用したエンド・ユーザーとしてのサインイン

*   _ペルソナ_:
    *   エンド・ユーザー

1.  Salesforceアプリケーションに割り当てられているユーザー・アカウントでIDCSコンソールにサインインします。
    
2.  _「Salesforce Chatter」_をクリックします。
    
    ![イメージ](images/L5015.png)
    
3.  アカウントの2ステップ検証を有効にするように求められます。_「有効化」_をクリックして、アカウントの2ステップ検証を有効にします。サインオン・ポリシーで「登録」が「オプション」に設定されているため、エンド・ユーザーにはこのステップをスキップするオプションが表示されます。
    
    ![イメージ](images/L5016.png)
    

### Oracle Identity Cloud ServiceのMFAオプション: 電子メール

    Email:
    Users receive an email message that contains a temporary passcode that must be used during log in.
    

A. Oracleは、メインの電子メール・アカウントをMFAオプションとして自動的に設定します。ただし、_「デフォルトとして設定」_オプションをクリックすることをお薦めします。

![イメージ](images/L5017.png)

B. 電子メール・アドレスに送信されたコードを入力し、_「検証」_をクリックします。

![イメージ](images/L5018.png)

C. これで、電子メール・アドレスが正常に登録されました。追加の方法を設定することをお薦めします。MFA構成を続行するには、別の方法(_モバイル・アプリケーション_)を選択します。_「完了」_をクリックしてMFA構成を終了することもできます。

![イメージ](images/L5019.png)

    Note: Under *My Profile* and under the tab *Security*, further MFA options can be configured at a later point of time.
    

### Oracle Identity Cloud ServiceのMFAオプション: モバイル・オーセンティケータ・アプリケーション

    Mobile Authenticator Application:
    Users generate an OTP on the OMA App on their device that must be used during log in.
    

A. 携帯電話で_Oracle Mobile Authenticator_アプリケーションをダウンロードする。

B. モバイルにインターネット接続がある場合は、QRコードをスキャンします。

![イメージ](images/L5020.png)

C. 携帯電話にインターネット接続がない場合、またはサードパーティのオーセンティケータ(Google Authenticatorなど)を使用している場合。_「オフライン・モードまたは別のオーセンティケータ・アプリケーションの使用」_ボックスにチェックマークを入れ、生成された新しいオフラインQRコードをスキャンします。QRコードをスキャンすると、モバイル・アプリは30秒ごとにOTPを生成します。パスコード・フィールドにOTPを入力し、_「検証」_をクリックします。

![イメージ](images/L5021.png)

D. 別の方法をクリックして、追加の2ステップ検証方法を登録できる。このチュートリアルでは、_モバイル番号_に進みます。

### Oracle Identity Cloud ServiceのMFAオプション: テキスト・メッセージ(SMS)

    Text Message (SMS):
    Users receive a temporary passcode as a text message (SMS) on their device that must be used during log in.
    

A. 「_Mobile Number_」をクリックします。

B. モバイル番号を入力し、_「送信」_をクリックする。

![イメージ](images/L5022.png)

C. OTPを含む_SMS_ (テキスト・メッセージ)がモバイル番号に送信される。

D. 「パスコード」フィールドにOTPを入力し、_「検証」_をクリックします。

### Oracle Identity Cloud ServiceのMFAオプション: セキュリティの質問

    Security Questions:
    Users are prompted during the sign-in process to correctly answer a defined number of security questions to verify their identity.
    

A. _「セキュリティ質問」_をクリックします。

B. 使用する質問を選択し、記憶する回答を入力し、オプションで各回答にヒットを入力して_「保存」_をクリックします。

![イメージ](images/L5023.png)

C. _「完了」_をクリックしてプロセスを完了する。これで、Salesforceにリダイレクトされます。

![イメージ](images/L5024.png)

### Oracle Identity Cloud ServiceのMFAオプション: バックアップ検証方法

    Backup verification method:
    Oracle makes it possible to use a backup verification method in case the current MFA option cannot be entered, i.e. when the configured MFA device is not reachable.
    

A. IDCSにアクセスし、「My Apps」で「Salesforce」を選択します。事前構成済のMFAオプションが表示されます。この場合、_モバイル・オーセンティケータ・アプリケーション_です。

B. _「バックアップ検証方法の使用」_をクリックします。登録されたバックアップ2ステップ検証方法のリストが表示されます。

![イメージ](images/L5025.png)

C. この例では、_「セキュリティ質問」_を選択します。

D. セキュリティ質問に回答し、_「検証」_をクリックします。

![イメージ](images/L5026.png)

## タスク4: ポリシーを介したアクセスのブロック

*   _ペルソナ_:
    
    *   管理者
    
        Block blacklisted IPs
        After creating a network perimeter, you can prevent users from signing in to Oracle Identity Cloud Service if they use one of the IP addresses in the network perimeter. This is known as blacklisting. A blacklist contains IP addresses or domains that are suspicious. As an example, a user may be trying to sign in to Oracle Identity Cloud Service with an IP address that comes from a country where hacking is rampant.
        

1.  _「セキュリティ」_に移動し、_「ネットワーク・ペリメータ」_を選択します。前に作成したブラックリストに登録されたIPネットワーク・ペリメータの右側にあるメニューをクリックし、「編集」をクリックします。以前に構成されたダミーのIPアドレスが表示されます。
    
    ![イメージ](images/L5027.png)
    
2.  ダミーIPをIP範囲に変更します。
    

*   Webブラウザを開き、_パブリックIPアドレス_を示すWebサイトにアクセスします(例: https://www.whatismyip.com/)。
    
*   パブリックIPアドレスをコピーします。
    
*   IDCSに戻り、ダミーのIPアドレスをパブリックIPアドレスに置き換えて、_「保存」_をクリックします。
    
    ![イメージ](images/L5028.png)
    

## タスク5: 更新されたポリシーのテスト

*   _ペルソナ_:
    *   エンド・ユーザー

1.  Salesforceアプリケーションに割り当てられているユーザー・アカウントでIDCSコンソールにサインインします。
    
2.  _「Salesforce Chatter」_タイルをクリックします。
    
    ![イメージ](images/L5029.png)
    
3.  リクエストがブラックリストに登録されたIPで発生した場合、アクセスは拒否されます。
    
    ![イメージ](images/L5030.png)
    

次の演習に進むことができます。

## 謝辞

*   **作成者** - SEHubセキュリティおよび管理チーム
*   **最終更新者/日付** - プリンシパル・ソリューション・エンジニア、Lucian Ionescu、15.09.2020