# リダクションを使用したRESTコールのみの匿名化

## 概要

演習2では、Oracle Data Redactionを使用して、`DEMO_HR_EMPLOYEES`表から問い合せたすべてのデータを式'1=1'でリダクションしました。この演習では、RESTコールで使用されるモジュールを識別し、RESTコールの結果のみをリダクションします。これにより、Database Actions SQLおよびその他のすべての問合せは、リダクションされていないデータを返すことができます。

見積時間: 15分

### 目標

この演習では、次のタスクを実行します。

*   RESTコールのセッション情報の識別および統合監査ポリシーの作成
*   リダクション・ポリシーを変更する前に従業員の問合せおよびRESTコール・データを表示し、監査レコードを確認します
*   リダクション・ポリシーを更新し、従業員問合せデータ、RESTコール・データおよび監査レコードをレビューします
*   監査ポリシーおよびリダクション・ポリシーを削除します。

### 前提条件

この演習では、次を想定しています。

*   Oracle Cloud Infrastructure (OCI)テナント・アカウント
*   **Oracle Data Redactionを使用したREST GETコールの機密データの保護**のLiveLabワークショップでこれまでのすべてのラボを完了

_警告: リソースの終了には数分かかる場合があります_

## タスク1: RESTコールのセッション情報を識別し、統合監査ポリシーを作成します。

1.  まず、RESTコールのセッション情報を識別する必要があります。これを行うには、`DEMO_HR_EMPLOYEES`に対してSELECTを実行するすべてのセッションを監査します。このための統合監査ポリシーを作成して有効にします。`ADMIN` SQLウィンドウにナビゲートし、次のスクリプトを貼り付けて、文を実行します。このスクリプトは監査ポリシーを作成します。
    
        <copy>CREATE AUDIT POLICY audit_hr_select ACTIONS SELECT on employeesearch_prod.demo_hr_employees;</copy>   
        
    
    ![監査ポリシーの作成](images/create-audit-policy.png)
    
2.  次のスクリプトを実行して、統合監査ポリシーを有効にします。
    
        <copy>audit policy audit_hr_select;</copy>   
        
    
    ![監査ポリシーの有効化](images/enable-audit-policy.png)
    
3.  ここでは、問合せで使用されているモジュールの情報も含める必要があります。これにより、RESTコールを絞り込むための追加情報が提供されます。
    
        <copy>audit context namespace userenv attributes module;</copy>   
        
    
    ![監査コンテキスト](images/audit-context.png)
    

## タスク2: リダクション・ポリシーを変更する前に従業員問合せおよびRESTコール・データを表示し、監査レコードを確認します

1.  `EMPLOYEESEARCH_PROD`に戻り、問合せを再度実行します。
    
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
        
    
    リダクション・ポリシーをまだ変更していないため、データはリダクション済として表示されます。
    
    ![リダクション済問合せ](./images/redacted-qry.png)
    
2.  `ADMIN`として、統合監査証跡に新しく作成された統合監査ポリシーの新しい監査レコードがあることを確認します。
    
        <copy>SELECT dbusername, dbproxy_username, client_program_name, sql_text, APPLICATION_CONTEXTS
            FROM unified_audit_trail
        WHERE application_contexts is not null
            AND regexp_like(unified_audit_policies,'AUDIT_HR_SELECT');</copy>  
        
    
    ![新規監査レコード](images/new-audit-rec.png)
    
3.  ブラウザ・タブをリフレッシュして、ブラウザからRESTコールを実行します。データはリダクションされます。 ![リダクション済RESTコール](./images/redacted-call.png)
    
4.  ここでも、`ADMIN`として、統合監査証跡に追加の監査レコードが表示されます。これらの新しい監査レコードは、application\_contexts列の値の違いを示す必要があります。たとえば、Database Actions SQLでは値'/\_/SQL/'が表示され、Oracle Rest Data Services CALLではapplication\_contexts列の値が'/demo\_hr\_employees/'と表示されます。 ![追加監査レコード](images/add-audit-rec.png)
    

## タスク3: リダクション・ポリシーの更新、従業員問合せデータ、RESTコール・データおよび監査レコードのレビュー

1.  次に、`EMPLOYEESEARCH_PROD`として、Oracle Data Redactionポリシーのパラメータ式を'1=1'からRESTコールで使用されることがわかっているものに更新します。
    
        <copy>BEGIN
        DBMS_REDACT.ALTER_POLICY  (
            OBJECT_SCHEMA => 'EMPLOYEESEARCH_PROD'
            ,object_name => 'DEMO_HR_EMPLOYEES'
            ,policy_name => 'redact_emp_info'
            ,action => DBMS_REDACT.MODIFY_EXPRESSION
            ,expression => 'sys_context(''userenv'',''module'') = ''/demo_hr_employees/''');
        END;
        /</copy> 
        
    
    ![変更リダクション・ポリシー](images/change-red-pol.png)
    
2.  次に、Database Actions SQLで`EMPLOYEESEARCH_PROD`として問合せを再実行します。データはリダクションされません。
    
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
        
    
    ![問合せの再実行](images/re-run-qry.png)
    
3.  RESTコールも再実行します。データはリダクションされます。 ![問合せの再実行](./images/redacted-call.png)
    

## タスク4: 監査ポリシーの削除、およびリダクション・ポリシー。

1.  統合監査ポリシーはその目的を果たしているため、すべてのSELECT文を監査する必要がないため、削除できます。`ADMIN`として、次のスクリプトを実行します。
    
        <copy>noaudit policy audit_hr_select;
        drop AUDIT POLICY audit_hr_select;</copy>  
        
    
    ![監査ポリシーの削除](images/drop-aud-pol.png)
    
2.  `EMPLOYEESEARCH_PROD`の**「SQL」ウィンドウ**に戻り、**リダクション・ポリシーを削除します**。
    
        <copy>BEGIN
                dbms_redact.drop_policy (
                object_schema => 'EMPLOYEESEARCH_PROD',
                object_name   => 'DEMO_HR_EMPLOYEES',
                policy_name   => 'redact_emp_info'
                );
            end;
        /</copy>   
        
    
    ![ドロップ](images/drop.png)
    

**次の演習に進みます。**

## 謝辞

*   **著者** - Alpha Diallo & Ethan Shmargad、北米スペシャリスト・ハブ
*   **作成者** - Pedro Lopes、データベース・セキュリティ製品マネージャー
*   **最終更新者/日付** - Alpha Diallo & Ethan Shmargad、2023年2月