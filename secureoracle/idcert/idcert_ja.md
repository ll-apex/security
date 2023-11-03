# アイデンティティの証明

## 概要

アイデンティティの証明は、企業内のユーザー権限とアクセス権限をレビューして、ユーザーが所有を承認されていない権限を取得していないことを確認するプロセスです。また、各アクセス権限の承認(認証)または拒否(取消し)も含まれます。このラボでは、カスタム・レビューアによるユーザー証明をレビューします。

次のユースケースを使用できます。

*   カスタム・レビューアによるユーザー認証

_推定ラボ時間_: 60分

### 目標

*   アイデンティティ認証の理解

### 前提条件

この演習では、次を想定しています。

*   Free Tier、有料またはLiveLabs Oracle Cloudアカウント
*   SSH経由でホストにアクセスするためのSSH秘密キー
*   次を完了しました:
    *   演習: SSHキーの生成(_フリー層_および_有料テナント_のみ)
    *   演習: 設定の準備(_無料層_および_有料テナント_のみ)
    *   演習: 環境設定
    *   演習: 環境の初期化

## タスク1: 必要なコンポーネントおよびアプリケーションへのアクセスの検証

1.  OIM管理およびセルフサービス・コンソール、Roundcube Eメール・クライアントおよびMy HRアプリケーションにアクセスできることを確認してください。次のリンクは、リモートデスクトップで実行されている Firefoxでもブックマークされています。詳細は、_ラボ: 環境の初期化_を参照してください
    
    Oracle Identity Manager管理コンソール:
    
        URL         http://secureoracle.oracledemo.com:14000/sysadmin
        User        xelsysadm
        Password    Oracle123
        
    
    Oracle Identity Managerセルフサービス:
    
        URL         http://secureoracle.oracledemo.com:14000/identity
        User        xelsysadm
        Password    Oracle123
        
    
    電子メールWebクライアント(Roundcube):
    
        URL         http://secureoracle.oracledemo.com/roundcubemail-1.4.1/
        User        admin
        Password    Oracle123
        Server      secureoracle.oracledemo.com
        
    
    IGAアプリケーション
    
        	URL         http://secureoracle.oracledemo.com:7001/ords/f?p=102
        	User        xelsysadm
        	Password    Oracle123
        

## タスク2: カスタム・レビューアによるユーザー証明

ユーザー証明により、マネージャは、ロール、アカウントおよび権限への従業員アクセスを証明できます。通常、組織内の各マネージャは、そのマネージャの直属の部下である個人のアクセス権限をレビューします。または、Oracle Identity Managerデータベースの**`CERT_CUSTOM_ACCESS_REVIEWERS`**表に証明ルールを定義することで、ユーザー証明のカスタム・レビューアを指定できます。

SecureOracle環境では、My IGA Applicationに**`CERT_CUSTOM_ACCESS_REVIEWERS`**表を保守するカスタムUIが含まれているため、管理者はカスタム・レビューアを簡単に定義できます。

1.  My IGA Applicationにログインして顧客レビュー担当者を定義します。
    
    例次のリンクおよび資格証明を使用します。
    
        	URL         http://secureoracle.oracledemo.com:7001/ords/f?p=102
        	User        xelsysadm
        	Password    Oracle123
        
2.  サイドバー・メニューで、**「カスタム・レビューア」**オプションをクリックして「カスタム・レビューア」ページを開きます。必要に応じて、カスタム・レビューアおよびユーザーの定義に進みます。
    
    例便宜上、カスタム・レビューアは、認定される2人のユーザーとともにすでに定義されています。
    
        	MAP NAME         REVIEWER LOGIN   USER LOGIN   ACCESS TYPE
        	2019 Q4 Review   MGRAFF           AHUTTON      User
        	2019 Q4 Review   MGRAFF           AHUTTON      User
        
    
    **ノート**: レビュー担当者およびユーザーを追加できます。OIMは、**「マップ名」**ごとに証明タスクを生成します。ルールおよび定義の詳細は、[ユーザー認定のカスタム・レビューア](https://docs.oracle.com/en/middleware/idm/identity-governance/12.2.1.4/omusg/managing-identity-certification.html#GUID-941F44D2-1B30-4B0A-AF25-3BE0430C7F8A)の公式ドキュメントを参照してください。
    
    ![IGAアプリケーション・カスタム・レビューア(個人)](./images/img-custom-reviewer.png " ")
    
    図1.カスタム審査担当者
    
3.  My IGA Applicationからサインアウトします。
    
4.  OIMセルフ・サービスに管理者としてログインして、証明オプションを構成します。
    
    例次のリンクおよび資格証明を使用します。
    
        	URL         http://secureoracle.oracledemo.com:14000/identity
        	User        xelsysadm
        	Password    Oracle123
        
5.  **`Compliance -> Identity Certification -> Certification Configuration`**に移動し、次のように構成を検証します。
    
    例: 次の構成を参照として使用します。
    
        Password required on sign-off                : [checked]
        Allow comments on certify operations         : [unchecked]
        Allow comments on all non-certify operations : [checked]
        Verify employee access                       : [checked]
        Prevent self certification                   : [checked]
        	                                               Alternate Reviewer : User Manager
        
        User and Account Selections         : Include any user with active accounts
        Allow advanced delegation           : [unchecked]
        Allow multi-phased review           : [unchecked]
        Allow reassignment                  : [unchecked]
        Allow auto-claim                    : [checked]
        Perform closed loop remediation     : [checked]
        
        Enable Interactive Excel            : [unchecked]
        Enable Certification Reports        : [checked]
        
6.  **「接続のテスト」**ボタンをクリックしてBI Publisherコンポーネントとの通信をテストします。変更を加えた場合は変更を保存し、それ以外の場合は**「取消」**をクリックしてページを閉じます。
    
7.  証明定義の作成に進みます。便宜上、証明定義はすでに作成されています。参照として、次の表には定義で使用されるパラメータが含まれています。
    
    例証明定義のパラメータ:
    
        Name              : 2019 Q4 Review
        Type              : User
        Description       : Custom Reviewers Certification
        
        In Base Selection
        Only Users from Select Organization : Oracle Users
        	                                      Finance
        	                                      Sales
        Constrains                          : Users with Any Level of Risk
        
        In Content Selection
        Include users with no accounts      : [checked]
        
        Limit the role-assignments to certify for each user : All Roles
        Include accounts with no certifiable attributes     : [checked]
        
        Limit the application-instance-assignments to certify for each user : All Application Instances
        Limit the entitlement-assignments to certify for each user          : All Entitlements
        
        In Configuration
        Accept the defaults settings
        
        In Reviewers
        Reviewer                        : Custom Access Reviewer
        Custom Access Reviewer Map Name : 2019 Q4 Review
        
        In Incremental
        Accept the defaults settings
        
        In Summary, review results
        Name              : 2019 Q4 Review
        Description       : Custom Reviewers Certification
        Type              : User
        Reviewer          : Custom Access Reviewer
        Incremental       : No
        Base Selection    : 3 Organizations Selected
        
        	Users with Any Level of Risk
        Content Selection : All Roles
        	                    All Application Instances
        	                    All Entitlements
        
    
    **ノート**: **`CERT_CUSTOM_ACCESS_REVIEWERS`**表に定義されているマップ名**`2019 Q4 Review`**を指定した**「レビューア」**セクションに注意してください。証明定義の作成後、OIMでは、新しい証明のジョブを自動的に作成するオプションが提供されます。前述の例で、OIMはジョブ**`Cert_2019 Q4 Review`**を作成しました。
    
8.  証明の実行を続行するには、ユーザー**XELSYSADM**としてOIMセルフ・サービスにログインします。**`Compliance -> Identity Certification -> Definitions`**にアクセスします。
    
9.  動作保証**`2019 Q4 Review`**を選択し、**「今すぐ実行」**ボタンをクリックします。
    
10.  OIMは、証明タスクを開始し、レビューア(この場合はユーザー**MGRAFF**)に通知します。電子メールWebクライアント(Roundcube)にログインして、電子メール通知を確認できます。
    
    例次のリンクおよび資格証明を使用します
    
        URL         http://secureoracle.oracledemo.com/roundcubemail-1.4.1/
        User        mgraff
        Password    Oracle123
        Server      secureoracle.oracledemo.com
        
    
    ![RoundcubeレビューEメール通知](./images/img-cert-email.png " ")
    
    図2.証明Eメール
    
11.  ユーザー**MGRAFF**としてOIMセルフ・サービスにログインします。**`Self Service -> Certifications`**にアクセスします。**「保留中の証明」**ページに新しい証明がリストされ、リンク名**`2019 Q4 Review [Molly Graff]`**をクリックしてユーザーを確認および証明します。
    
    ![OIMセルフ・サービス保留証明ウィンドウ](./images/user-certifications.png " ")
    
    図3.ユーザー認証
    
12.  すべてのユーザーがデフォルトで認証されると、OIMは確認者**MGRAFF**に認証タスクをサインオフするための資格証明の入力を求めます。
    
13.  OIMセルフ・サービスからサインアウトします。
    

## **付録**: カスタム・レビューアについて

特定のレビューアとともに、特定のユーザー・アカウント、ロール、権限またはアプリケーション・インスタンスに対して証明ルールを指定するか、これらのエンティティの組合せに対して証明ルールを指定することで、ユーザー証明に対して独自のカスタム・アクセス・レビューアを定義できます。また、証明ルールをグループ化し、証明ルールにマップ名を割り当てて、そのマップ名のレビューアを指定できます。

これらのルールは、Oracle Identity Managerデータベースの**`CERT_CUSTOM_ACCESS_REVIEWERS`**表で定義できます。

ユーザー証明のカスタム・アクセス・レビューア機能を使用するには、次の条件が満たされている必要があります。

*   レビューア表は、フィールド/列のいずれについてもワイルドカードをサポートしていません。
*   レビューア表には、証明に含めるすべてのユーザーごとに定義したマッピングを保持します。
*   アプリケーション・インスタンス情報は、すべてのアカウントと権限のマッピングで必要になります。
*   マップ名ごとに許可されるデフォルト・レビューアのインスタンスと代替レビューアのマッピングは1つのみです。

SecureOracle環境では、My IGA Applicationに**`CERT_CUSTOM_ACCESS_REVIEWERS`**表を保守するカスタムUIが含まれているため、管理者はカスタム・レビューアを簡単に定義できます。このオプションは、「My IGA Application」メニューの**「Custom Reviewers」**オプションで使用できます。

詳細は、公式ドキュメントの[ユーザー認証のカスタム・レビューア](https://docs.oracle.com/en/middleware/idm/identity-governance/12.2.1.4/omusg/managing-identity-certification.html#GUID-941F44D2-1B30-4B0A-AF25-3BE0430C7F8A)を参照してください。

_次の演習に進む_ことができます。

## さらに学ぶ

Oracle Identity and Access Managementの詳細は、次のリンクを参照してください。

*   [Oracle Identity Management Webサイト](https://docs.oracle.com/en/middleware/idm/suite/12.2.1.4/index.html)
*   [Oracle Identity Governanceのドキュメント](https://docs.oracle.com/en/middleware/idm/identity-governance/12.2.1.4/index.html)
*   [Oracle Access Managementドキュメント](https://docs.oracle.com/en/middleware/idm/access-manager/12.2.1.4/books.html)

## 謝辞

*   **著者** - Ricardo Gutierrez氏、ソリューション・エンジニアリング- セキュリティおよび管理
*   **貢献者** - Rene Fontcha
*   **最終更新者/日付** - Sahaana Manavalan氏、LiveLabs開発者、NA Technology、2022年3月