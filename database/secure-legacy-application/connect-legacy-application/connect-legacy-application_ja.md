# 従来のGlassfish HRアプリケーションへの接続

## 概要

このラボでは、GlassfishのレガシーHRアプリケーションに接続します。これには、SSHキーの生成とそれを使用したアプリケーション仮想マシン(VM)への接続が含まれます。

### 目標

この演習では、次のタスクを実行します。

*   フォルダを設定し、SSHキー・ペアを生成します。
*   SSHを使用してGlassfish HRアプリケーションVMに接続します。
*   Glassfishアプリケーション・サーバーのプロパティ・ファイルにATP TLS接続文字列を保存します。

### 前提条件

この演習では、次を想定しています。

*   Oracle Cloud Infrastructure (OCI)テナンシ・アカウント
*   前の演習の完了: Autonomous Databaseインスタンスの構成

## タスク1: フォルダの設定とSSHキー・ペアの生成

1.  OCIコンソールの右上に移動し、クラウド・シェルのラベルが付いたターミナル・アイコンを選択し、それがロードおよび設定されるまで待ちます。クラウド・シェルは、事前認証済のOracle Cloud Infrastructure CLIやその他の便利な開発者ツールとともにLinuxシェルへのアクセスを提供する、Oracle Cloudコンソールからアクセスできる無料のブラウザベース・ターミナルです。
    
    ![クラウド・シェルのオープン](images/open-cloud-shell.png)
    
2.  次のコマンドを入力します。
    
    *   アプリケーション・フォルダを作成します。
        
            <copy>mkdir myhrapp</copy> 
            
    *   作成したフォルダに移動します。
        
            <copy>cd myhrapp</copy>
            
    *   SSH鍵ペアを生成します。「Return」を2回押して確認し、パスフレーズオプションを省略します。
        
            <copy>ssh-keygen -b 2048 -t rsa -f myhrappkey</copy>
            
    *   所有者のみが読取り機能と書込み機能の両方を持つように、SSHキー・ペアの権限を設定します。
        
            <copy>chmod 600 myhrappkey</copy>
            
    *   パスコードを作成しないようにするには、もう一度\[Enter\]を押します。
        
    *   今作成したキー・ペアを表示します。
        
            <copy>ls</copy>
            
    *   公開キーを取得し、選択したクリップボードにコピー/貼り付けます。これは、次のステップで必要になります。
        
            <copy>cat myhrappkey.pub</copy>
            
    *   秘密キーを取得し、選択したクリップボードにコピー/貼り付けます。これは、次のステップで必要になります。
        
            <copy>cat myhrappkey</copy>
            
3.  クラウド・シェルを最小化します。将来のタスクにもう一度必要になります。
    

## タスク2: Glassfishアプリケーション・インスタンスの作成

1.  左上のハンバーガー・メニューを使用して、**「コンピュート」**に移動し、**「カスタム・イメージ」**を選択します。
    
    ![カスタム・イメージにナビゲートします。](images/navigate-custom-image.png)
    
2.  **「イメージのインポート」**を選択します。
    
    ![インポート・イメージの選択](images/select-import-image.png)
    
3.  次のイメージに示すように、対応するフィールドに入力し(選択したコンパートメントを使用)、**「イメージのインポート」**を選択します。**オブジェクト・ストレージURL**には、次のリンクを使用します: https://objectstorage.us-ashburn-1.oraclecloud.com/p/ba8MlTYIT2N2X\_HXjVX9WltQA3-5ZhP6J1CWOKRtWS3UKAPTyfs-RY3eXChPE-SP/n/c4u04/b/livelabsfiles/o/data-management-library-files/MyHRApp-20221201-final
    
    ![カスタム・イメージの作成](images/create-custom-image.png)
    
4.  カスタム・イメージが使用可能になったら、右上のハンバーガー・メニューを使用して**「コンピュート」**に移動し、**「インスタンス」**を選択します。
    
    ![インスタンスに移動](images/navigate-instances.png)
    
5.  **インスタンスの作成**を選択します。
    
6.  対応するフィールドに次の情報を入力します。
    
    *   **名前**: myhrapp
    *   **コンパートメントに作成**: 選択したコンパートメントを選択します。
    *   **配置**: 選択した配置を選択します。
    *   **イメージ**: **「イメージの変更」**を選択します。イメージ・ソースを**「カスタム・イメージ」**に変更します。**コンパートメントが、カスタム・イメージをインポートしたコンパートメントと同じであることを確認します**。前のステップで作成したカスタムGlassfishイメージを選択します(ロードに少し時間がかかる場合があります)。
    *   **シェイプ**: 提供されているデフォルトのシェイプを使用します。
    *   **ネットワーキング**: **「新規仮想クラウド・ネットワークの作成」**および**「新規パブリック・サブネットの作成」**を選択します。名前`myhrapp-vcn`および任意のコンパートメントを使用します。CIDRブロックの場合は、`10.0.0.0/24`を使用します。パブリックIPアドレスの場合は、**「パブリックIPv4アドレスの割当て」**を選択します。
    *   **SSHキーの追加**: **「公開キーの貼付け」**を選択します。クリップボードから作成した公開SSHキーをコピーして貼り付けます(公開キーを確認します)。
    *   **ブート・ボリューム**: すべてのオプションが選択されて**いない**ことを確認します。
7.  **「作成」**を選択します。
    
    ![実行中のインスタンス](images/instance-running.png)
    

## タスク3: ATP TLS接続文字列をGlassfishアプリケーション・サーバーのプロパティ・ファイルに保存します。

1.  Cloud Shellターミナルを開きます。`myhrapp`ディレクトリにいることを確認します。
    
        <copy>cd myhrappkey</copy>
        
2.  インスタンスのパブリックIPアドレスを使用してGlassfishアプリケーション・インスタンスにSSH接続します。接続を続行するかどうかを確認するプロンプトが表示されたら、端末 **yes**を入力します。
    
        <copy>ssh -i myhrappkey opc@<PASTE INSTANCE PUBLIC IP ADDRESS HERE></copy>
        
    
    ![インスタンスへのSSH](images/ssh-into-instance.png)
    
    _ノート:_キーに問題がある場合は、**myhrapp**ディレクトリで所有者の読取りおよび書込み権限を維持する必要があることに注意してください。この問題を実行する場合は、Cloud Shellの**myhrapp**ディレクトリに次のコマンドをコピーして貼り付け、読取りおよび書込み権限を更新します。
    
        <copy>chmod 600 myhrappkey</copy>
        
3.  `secure-legacy-app-db-vault`ディレクトリに移動します。
    
        <copy>cd secure-legacy-app-db-vault</copy>
        
4.  `save_connection_string.sh`スクリプトを実行して、ATPインスタンスからTLS接続文字列を保存します。プロンプトが表示されたら、接続文字列を貼り付けます。
    
        <copy>./save_connection_string.sh</copy>
        
    
    ![接続文字列の保存](images/connect-string.png)
    
5.  `test_connection_string.sh`スクリプトを実行して、ATPインスタンスへのアプリケーション・インスタンス接続をテストします。プロンプトが表示されたら、前に作成したATP ADMINパスワードを入力します。接続が成功したことを示す正しい出力が表示されることを確認します。
    
        <copy>./test_connection_string.sh</copy>
        
    
        ...
        
        SQL> SQL> Current User is...
        SQL> 
        USER
        --------------------------------------------------------------------------------
        ADMIN
        
        SQL> 
        SQL> Success!
        
    
    ![接続のテスト](images/test-connection.png)
    

**次の演習に進みます。**

## 謝辞

*   **著者** - Ethan Shmargad氏、北米スペシャリスト・ハブ
*   **作成者** - シニア・プリンシパル・プロダクト・マネージャー、Richard Evans
*   **最終更新者/日付** - Ethan Shmargad、2022年9月