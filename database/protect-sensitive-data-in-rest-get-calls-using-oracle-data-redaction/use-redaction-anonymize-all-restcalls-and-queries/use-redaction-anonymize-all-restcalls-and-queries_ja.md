# リダクションを使用したすべてのRESTコールおよび問合せの匿名化

## 概要

この演習では、リダクション・ポリシーを適用する前後に、REST Get Callsを使用してEmployee表データを表示するOracle Data Redactionのアプリケーションを示します。

見積時間: 15分

### 目標

この演習では、次のタスクを実行します。

*   表をRESTで有効にします。
*   データ・リダクション・ポリシーを表に適用します。
*   REST Getコールからリダクションされたデータを参照してください。

### 前提条件

この演習では、次を想定しています。

*   Oracle Cloud Infrastructure (OCI)テナント・アカウント
*   **ORDSを使用したリストア・コールのリダクション**LiveLabワークショップでこれまでのすべてのラボを完了

_警告: リソースの終了には数分かかる場合があります_

## タスク1: RESTによる表の有効化

1.  `EMPLOYEESEARCH_PROD`のデータベース・アクション起動パッドから、**「開発」セクション**の下の**「SQL」**に移動します。
    
    ![起動パッドからSQLを選択](images/launchpad-sql.png)
    
2.  **表のRESTの有効化**は簡単です。これを行うには、**SQLワークシート**の左側にある**ナビゲータ**で作成した`DEMO_HR_EMPLOYEES`という名前の表を見つけます。表名を右クリックして、ポップアップ・メニューで**「REST」**を選択し、「有効化」を選択します。
    
    ![RESTの有効化](images/enable-rest.png)
    
3.  ページの右側から**「RESTオブジェクトの有効化」スライダ**が表示されます。このページのデフォルトを使用しますが、プレビューURLを任意のクリップボードに**コピー**します。これは、**REST対応表へのアクセス**に使用するURLです。このプロセスでは、表に対してORDSも有効化され、「コードの表示」オプションを選択してこの処理のコードを表示します。
    
    ![ORDSの有効化](images/rest-ords.png)
    

準備ができたら、スライダの右下にある**「有効化」ボタン**をクリックします。_警告: 「認証が必要」を有効にしないでください。そのためには、ユーザーが追加の認証プロセスを実行する必要があります。_

3.  これで終わりです。表は**REST対応**です。新しいブラウザ・ウィンドウまたはタブを開き、前のステップでコピーした**「URL」**を入力します。
    
    ![リダクション前RESTコール](images/pre-redaction-rest.png)
    

## タスク2: 表へのデータ・リダクション・ポリシーの適用

1.  `ADMIN`の**「データベース・アクション」**SQL開発ページに戻ります。次のテキストをワークシートに貼り付けて、`EMPLOYEESEARCH_PROD`へのアクセス権を`DBMS_REDACT`パッケージに付与します。
    
        <copy>grant execute on sys.dbms_redact to EMPLOYEESEARCH_PROD</copy>   
        
    
    ![リダクション前RESTコール](images/grant-red.png)
    
2.  `EMPLOYEESEARCH_PROD`の**「データベース・アクション」**SQL開発ページに戻り、**最初の問合せ**を実行します。**ページの下部**にある問合せ結果の下で、リダクションされていない結果を表示します。
    
        <copy>SELECT
            USERID,
            FIRSTNAME,   
            LASTNAME,  --redact
            EMAIL, --redact
            PHONEMOBILE,
            STARTDATE, --redact
            SALARY, --redact
            MANAGERID,
            DEPARTMENT
        FROM
            DEMO_HR_EMPLOYEES;</copy>   
        
    
    ![問合せ](images/qry.png)
    
    前のタスクのブラウザ・ウィンドウで表データも確認します。
    
    これは、リダクション・ポリシーが適用される前のデータの外観です。
    
3.  **リダクション・ポリシー**を追加して、姓をランダムな文字で実行します。
    
        <copy>begin
                dbms_redact.add_policy(
                    object_schema => 'EMPLOYEESEARCH_PROD',
                    object_name   => 'demo_hr_employees',
                    column_name   => 'lastname',
                    policy_name   => 'redact_emp_info',
                    function_type => dbms_redact.random,
                    expression    => '1=1'
                );
        end;
        /</copy>   
        
    
    ![姓](images/last-name.png)
    
4.  **電子メール列**をリダクション・ポリシーに追加し、`X`で匿名化するデフォルトの**正規表現パターン**を使用してリダクションします
    
        <copy>begin
                dbms_redact.alter_policy (
                object_schema => 'EMPLOYEESEARCH_PROD',
                object_name   => 'DEMO_HR_EMPLOYEES',
                column_name   => 'email',
                policy_name   => 'redact_emp_info',
                action              => dbms_redact.add_column,
                function_type          => DBMS_REDACT.REGEXP,
                function_parameters    => NULL,
                expression             => '1=1',
                regexp_pattern         => DBMS_REDACT.RE_PATTERN_EMAIL_ADDRESS,
                regexp_replace_string  => DBMS_REDACT.RE_REDACT_EMAIL_NAME,
                regexp_position        => DBMS_REDACT.RE_BEGINNING,
                regexp_occurrence      => DBMS_REDACT.RE_FIRST,
                regexp_match_parameter => DBMS_REDACT.RE_CASE_INSENSITIVE,
                policy_description     => 'Regular expressions to redact email address names',
                column_description     => 'email column contains customer emails');
            END;
        /</copy>   
        
    
    ![電子メール](images/email.png)
    
5.  **開始日列**をリダクション・ポリシーに追加し、日および月をリダクションします。
    
        <copy>BEGIN
                DBMS_REDACT.alter_policy (
                object_schema       => 'EMPLOYEESEARCH_PROD',
                object_name         => 'DEMO_HR_EMPLOYEES',
                policy_name         => 'redact_emp_info',
                action              => DBMS_REDACT.add_column,
                column_name         => 'startdate',
                function_type       => DBMS_REDACT.partial,
                function_parameters => 'm1d1Y'
                );
            END;
        /</copy>   
        
    
    ![開始日](images/start-date.png)
    
6.  **「給与」列**をリダクション・ポリシーに追加し、最初の2桁をリダクションして99にします。
    
        <copy>BEGIN
                DBMS_REDACT.alter_policy (
                object_schema          => 'EMPLOYEESEARCH_PROD', 
                object_name            => 'DEMO_HR_EMPLOYEES', 
                column_name            => 'salary',
                column_description     => 'holds employee salaries',
                policy_name            => 'redact_emp_info', 
                policy_description     => 'Partially redacts the salary column',
                function_type          => DBMS_REDACT.PARTIAL,
                function_parameters    => '9,1,2',
                expression             => '1=1');
            END;
        /</copy>   
        
    
    ![給与](images/salary.png)
    

## タスク3: REST Getコールからリダクションされたデータの表示

1.  **ブラウザのリロード**・ウィンドウを使用して、表に対する変更を表示します。
    
    ![リダクション済REST](images/redacted-call.png)
    
2.  **前のタスクからの最初の問合せ**を実行し、リダクションされたデータを**ページの下部**に表示します。
    
    ![リダクション済REST](images/redacted-qry.png)
    

ORDSを使用してRESTコールを正常にリダクションしました。

## 謝辞

*   **著者** - Alpha Diallo & Ethan Shmargad、北米スペシャリスト・ハブ
*   **作成者** - Pedro Lopes、データベース・セキュリティ製品マネージャー
*   **最終更新者/日付** - Alpha Diallo & Ethan Shmargad、2023年2月