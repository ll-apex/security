# ローカルKubernetesノードへのOIRIのデプロイ

## 概要

このラボでは、構成ファイルの設定、ウォレットの作成およびHelmチャートのインストールを含む、Oracle Identity Role Intelligenceの構成に必要なステップについて説明します。

_推定ラボ時間_: 40分

### 目標

この演習では、次のことを行います。

*   OIRI dockerイメージのロード
*   Kubernetes構成ファイルを設定します
*   OIRIおよびdingウォレットの作成
*   データベース・スキーマの作成およびシード
*   OIRI Helmチャートのインストール

### 前提条件

この演習では、次を想定しています。

*   Free Tier、有料またはLiveLabs Oracle Cloudアカウント
*   次を完了しました:
    *   演習: 設定の準備(_無料層_および_有料テナント_のみ)
    *   演習: 環境設定
    *   演習: 環境の初期化
    *   演習: KubernetesクラスタのデプロイとOIGサーバーの起動

## タスク1: OIRI dockerイメージのロード

OIRIサービスは、次の4つのイメージで構成されます:

*   OIRI: OIRIサービス
*   OIRI-cli: OIRIコマンドライン・インタフェース
*   oiri-ding: データ・インポート用
*   oiri-ui: Identity Role Intelligenceユーザー・インタフェース

これらのdockerイメージをロードするには、次のステップに従います。

1.  ターミナル・セッションを開き、OIRI dockerイメージをロードします。
    
        <copy>cd /u01/setup/oiri/oiri-12.2.1.4.210423</copy>
        
    
        <copy>docker load --input oiri-12.2.1.4.210423.tar</copy>
        
    
        <copy>docker load --input oiri-ui-12.2.1.4.210423.tar</copy>
        
    
        <copy>docker load --input oiri-cli-12.2.1.4.210423.tar</copy>
        
    
        <copy>docker load --input oiri-ding-12.2.1.4.210423.tar</copy>
        
    
    ![OIRI dockerイメージをロードするターミナル・コマンド](images/load-docker-images.png)
    
    _ノート:_
    
    Dockerイメージは、次のいずれかの方法でロードできます:
    
    *   [共有ZipファイルからのOIRI Dockerイメージの使用](https://docs.oracle.com/en/middleware/idm/identity-role-intelligence/amiri/installing-oracle-identity-role-intelligence.html#GUID-174D3A93-752E-4E5A-AF3F-0648E87BC0F0)
    *   \[コンテナ・レジストリからのDockerイメージの使用\] (https://docs.oracle.com/en/middleware/idm/identity-role-intelligence/amiri/installing-oracle-identity-role-intelligence.html#GUID-B23F0B59-AF19-4C7F-A268-8B6F3B5FC6B0))
2.  イメージを確認します。
    
        <copy>docker images | grep 12.2.1.4.210423</copy>
        
    
    ![イメージを確認します](images/verify-docker-images.png)
    

## タスク2: 構成ファイルの設定

データ・インポート(またはデータ取込み)およびHelmチャートの構成に必要なファイルを設定します。

1.  NFSにディレクトリを作成します。NFSは前提条件であり、ノード間で使用する永続ボリュームを作成するために使用されます。
    
        <copy>mkdir /nfs/ding</copy>
        
    
        <copy>mkdir /nfs/oiri</copy>
        
    
    Helmチャートで使用されるvalues.yamlファイルを生成するために、次のディレクトリを作成します。このディレクトリは、クラスタで共有する必要がないvalues.yamlファイルの格納にのみ使用されるため、NFS上に存在する必要はありません。
    
        <copy>mkdir /u01/k8s/</copy>
        
    
    作成されたディレクトリに対する書込み権限を確認します。
    
        <copy>chmod -R 775 /nfs/ding/</copy>
        
    
        <copy>chmod -R 775 /nfs/oiri/</copy>
        
    
        <copy>chmod -R 775 /u01/k8s/</copy>
        
2.  hostsファイルに示されているように、インスタンスのプライベートIPを書き留めます。
    
        <copy>vi /etc/hosts</copy>
        

たとえば、次のとおりです。

    ![Private IP of your instance](images/ip.png)
    

3.  _oiri-cli_コンテナを実行します。
    
        <copy>docker run -d --name oiri-cli \
        

\-v /nfs/ding/:/app/  
\-v /nfs/oiri/:/app/oiri  
\-v /u01/k8s/:/app/k8s  
\--group-add 54321  
oiri-cli:12.2.1.4.210423  
tail -f /dev/null \`\`

    Notice *--group-add 54321* where, 54321 is the `GROUP_ID`.
    `GROUP_ID` is the ID of the group having access to the volumes.
    

4.  KubernetesクラスタからKube構成コンテンツをコピーします。_/home/oracle/.kube/config_ファイルの内容をノートパッドまたはクリップボードにコピーします。必ずズーム・アウトし、ファイル内のすべての行をコピーしてください。
    
        <copy>vi /home/oracle/.kube/config</copy>
        
    
    ![KubernetesクラスタからのKube構成コンテンツのコピー](images/config.png)
    
5.  _oiri-cli_コンテナに移動します。
    
        <copy>docker exec -it oiri-cli /bin/bash</copy>
        
6.  Kube構成ファイルを作成します。_/home/oracle/.kube/config_ファイルからコピーされた内容を挿入し、ファイルを保存します。
    
        <copy>vi /app/k8s/config</copy>
        
    
        <copy>chmod 400 /app/k8s/config</copy>
        
    
    ![Kube構成ファイルを作成し、コピーしたコンテンツを貼り付けます](images/kube-config.png)
    
7.  kubectlとhelmのバージョンを確認します。_helm version_および_kubectl version_コマンドが警告やエラーなしで正常に実行されていることを確認し、バージョンを表示します。
    
        <copy>kubectl version</copy>
        
    
        <copy>helm version</copy>
        
    
    ![kubectlおよびhelmバージョンの確認](images/kube-version.png)
    

_ノート: エラーがある場合、Kube構成ファイルの内容が正しくコピーされない可能性があります。前のステップ4-6に従ってkube構成ファイルの内容をコピーして貼り付け、再試行してください_

8.  構成ファイルを設定します。`<VM IP>`パラメータを、ステップ2の_/etc/hosts_ファイルから指定されたインスタンス・プライベートIPアドレスに置き換えます。
    
        <copy>./setupConfFiles.sh -m prod \
        

\--oigdbhost \--oigdbport 1521  
\--oigdbsname oiri.livelabs.oraclevcn.com  
\--oiridbhost \--oiridbport 1521  
\--oiridbsname oiri.livelabs.oraclevcn.com  
\--sparkmode k8s  
\--dingnamespace ding  
\--dingimage oiri-ding:12.2.1.4.210423  
\--k8scertificatefilename ca.crt  
\--sparkk8smasterurl k8s://https://:6443  
\--oigserverurl http://:14000 \`\`

たとえば、次のとおりです。

    ```
    

./setupConfFiles.sh -m prod  
\--oigdbhost 10.0.0.231  
\--oigdbport 1521  
\--oigdbsname oiri.livelabs.oraclevcn.com  
\--oiridbhost 10.0.0.231  
\--oiridbport 1521  
\--oiridbsname oiri.livelabs.oraclevcn.com  
\--sparkmode k8s  
\--dingnamespace ding  
\--dingimage oiri-ding:12.2.1.4.210423  
\--k8scertificatefilename ca.crt  
\--sparkk8smasterurl k8s://https://10.0.0.231:6443  
\--oigserverurl http://10.0.0.231:14000 \`\`

    ![Terminal commands to set up configuration files](images/setup.png)
    

9.  構成ファイルが生成されたことを確認します。
    
        <copy>ls /app/data/conf/</copy>
        
    
        <copy>ls /app/oiri/data/conf</copy>
        
10.  Helmチャートに使用するvalues.yamlファイルを設定します。`<VM IP>`パラメータを、ステップ2の_/etc/hosts_ファイルから指定されたインスタンス・プライベートIPアドレスに置き換えます。
    
        <copy>./setupValuesYaml.sh \
        

\--oiriapiimage oiri:12.2.1.4.210423  
\--oirinfsserver \--oirinfsstoragepath /nfs/oiri  
\--oirinfsstoragecapacity 10Gi  
\--oiriuiimage oiri-ui:12.2.1.4.210423  
\--dingimage oiri-ding:12.2.1.4.210423  
\--dingnfsserver \--dingnfsstoragepath /nfs/ding  
\--dingnfsstoragecapacity 10Gi  
\--ingresshostname oiri  
\--sslsecretname "oiri-tls-cert" \`\`\`

たとえば、次のとおりです。

    ```
    

./setupValuesYaml.sh  
\--oiriapiimage oiri:12.2.1.4.210423  
\--oirinfsserver 10.0.0.231  
\--oirinfsstoragepath /nfs/oiri  
\--oirinfsstoragecapacity 10Gi  
\--oiriuiimage oiri-ui:12.2.1.4.210423  
\--dingimage oiri-ding:12.2.1.4.210423  
\--dingnfsserver 10.0.0.231  
\--dingnfsstoragepath /nfs/ding  
\--dingnfsstoragecapacity 10Gi  
\--ingresshostname oiriliri  

11.  values.yamlが生成されていることを確認し、コンテナを終了します。
    
        <copy>ls /app/k8s/</copy>
        
    
        <copy>exit</copy>
        

## タスク3: データ・インポートのエンティティ・パラメータの更新

1.  データ・インポートのエンティティ・パラメータを更新できるようにするには、次のコマンドを実行します。
    
        <copy>docker run -d --name ding-cli \
        

\-v /nfs/ding/:/app/  
\-v /nfs/oiri/:/app/oiri  
\-v /u01/k8s/:/app/k8s  
oiri-ding:12.2.1.4.210423  
tail -f /dev/null \`\`\`

## タスク4: Walletの作成

1.  キーストアにOIG証明書をインポートします。そのためには、署名検証のためにOIG証明書をエクスポートします。
    
        <copy>cd /u01/oracle/config/domains/oig_domain/config/fmwconfig/</copy>
        
    
        <copy>keytool -export -rfc -alias xell -file xell.pem -keystore default-keystore.jks</copy>
        

プロンプトが表示されたら、キーストアのパスワードを入力します。`Password: <copy>Welcome1</copy>`

![署名検証のためにOIG証明書をエクスポートするターミナル・コマンド](images/keystore.png)

_default-keystore.jks_は、_DOMAIN\_HOME/config/fmwconfig_にあります。ここでエクスポートする証明書は、OIG REST APIを保護します。OIGサーバー証明書と同じではありません。

2.  OIGキーストアからエクスポートされた_xell.pem_ファイルを_/nfs/oiri/data/keystore/_ディレクトリにコピーします。
    
        <copy>sudo cp /u01/oracle/config/domains/oig_domain/config/fmwconfig/xell.pem /nfs/oiri/data/keystore/</copy>
        
    
        <copy>sudo chown opc:users /nfs/oiri/data/keystore/xell.pem</copy>
        
    
        <copy>sudo ls -latr /nfs/oiri/data/keystore</copy>
        
    
    ![OIGキーストアからエクスポートされたxell.pemファイルを/nfs/oiri/data/keystore/にコピーします。](images/opc.png)
    
3.  _oiri-cli_コンテナ内にキーストアを生成します。
    
        <copy>docker exec -it oiri-cli /bin/bash</copy>
        
    
        <copy>keytool -genkeypair \
        

\-alias oirikey  
\-keypass Welcome1  
\-keyalg RSA  
\-keystore /app/oiri/data/keystore/keystore.jks  
\-storetype pkcs12  
\-storepass Welcome1 \`\`

出力:

    ```
    What is your first and last name?
    [Unknown]:
    What is the name of your organizational unit?
    [Unknown]:
    What is the name of your organization?
    [Unknown]:
    What is the name of your City or Locality?
    [Unknown]:
    What is the name of your State or Province?
    [Unknown]:
    What is the two-letter country code for this unit?
    [Unknown]:
    Is CN=Unknown, OU=Unknown, O=Unknown, L=Unknown, ST=Unknown, C=Unknown correct?
    [no]:  yes
    ```
    
    ![Terminal commands to Generate a keystore inside the oiri-cli container](images/keytool.png)
    

4.  OIRIキーストアに証明書をインポートします。
    
        <copy>keytool -import \
        -alias xell \
        -file /app/oiri/data/keystore/xell.pem \
        -keystore /app/oiri/data/keystore/keystore.jks</copy>
        
    
    要求に応じて、キーストア・パスワードを入力します。
    
        Password: <copy>Welcome1</copy>
        
    
    出力:
    
        Trust this certificate? [no]: yes
        
    
    ![OIRIキーストアへの証明書のインポート](images/import-keytool.png)
    
5.  ウォレットを作成します。
    
        <copy>oiri-cli --config=/app/data/conf/config.yaml wallet create</copy>
        
    
    要求された場合、次の情報を入力します。
    
    *   OIRI DB UserName接頭辞。例: dev、test、prodスキーマ名は \_OIRI: **DEV**になります
    *   OIRI DBパスワード: **Welcome1**
    *   OIG DB UserName: **DEV\_OIM**
    *   OIG DBパスワード: **Welcome1**
    *   OIGサービス・アカウントUserName: **xelsysadm**
    *   OIGサービス・アカウントのパスワード: **Welcome1**
    *   OIRI KeyStoreパスワード: **Welcome1**
    *   OIRI JWTキーの別名: **oirikey**
    *   OIRI JWTキー・パスワード: **Welcome1**
    
    ![ウォレットの作成](images/wallet.png)
    
6.  OIRIおよびDingウォレットが作成されたことを確認します。
    
        <copy>ls /app/data/wallet</copy>
        
    
        <copy>ls /app/oiri/data/wallet</copy>
        
    
    ![作成されたOIRIおよびDingウォレットの確認](images/createdb-wallet.png)
    

## タスク5: OIRIデータベース・スキーマの作成およびシード

1.  データベース・ユーザー・スキーマを作成します。
    
        <copy>oiri-cli --config=/app/data/conf/config.yaml schema create /app/data/conf/dbconfig.yaml</copy>
        

要求されたら、SYSユーザー・パスワードを入力します。

    ```
    Password: <copy>Welcome1</copy>
    ```
    
    ![Terminal command to Create the database user schema](images/createdb-wallet.png)
    

2.  データベース・スキーマをシードします。
    
        <copy>oiri-cli --config=/app/data/conf/config.yaml schema migrate /app/data/conf/dbconfig.yaml</copy>
        
3.  ウォレットを確認します。
    
        <copy>./verifyWallet.sh</copy>
        
    
    ![ウォレットを検証するターミナル・コマンド](images/verifywallet.png)
    
    ノート: ウォレットの検証に失敗した場合は、_oiri-cli --config=/app/data/conf/config.yaml wallet update_コマンドを使用して、問題があると報告されたエントリを修正します
    

## タスク6: OIRI Helmチャートのインストール

1.  次のネームスペースを作成します。
    
        <copy>kubectl create namespace oiri</copy>
        
    
        <copy>kubectl create namespace ding</copy>
        
    
        <copy>exit</copy>
        
2.  oiri-cliコンテナの外部にあるDockerコンテナ・ホスト・マシンからSSLを有効にします。
    
        <copy>cd ~</copy>
        
    
        <copy>openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout tls.key -out tls.crt -subj "/CN=oiri"</copy>
        
    
        <copy>kubectl create secret tls oiri-tls-cert --key="tls.key" --cert="tls.crt"</copy>
        
3.  ヘルム・チャートをインストールします。
    
        <copy>docker exec -it oiri-cli /bin/bash</copy>
        
    
        <copy>helm install oiri /helm/oiri -f /app/k8s/values.yaml</copy>
        
    
    ![ヘルムチャートをインストールするためのターミナルコマンド](images/helm.png)
    
4.  ポッドをリストし、すべてのポッドが実行されていることを確認します。さらに、ネームスペースdingおよびoiriの下のポッドがRUNNINGおよびREADY 1/1状態であるかどうかを確認します。
    
        <copy>kubectl get pods --all-namespaces</copy>
        
    
        <copy>kubectl get pods -n ding</copy>
        
    
        <copy>kubectl get pods -n oiri</copy>
        
    
        <copy>exit</copy>
        
    
    ![ポッドの一覧表示](images/running-pods.png)
    
    ![名前空間dingおよびoiriの下のポッドがRUNNINGでREADY 1/1状態であることを確認します](images/ready-pods.png)
    

[次の演習に進む](#next)ことができます。

## 確認

*   **著者** - NATDソリューション・エンジニアリング、Anuj Tripathi、Brijith TG、Keerti R氏
*   **貢献者** - Keerti R、Brijith TG、Anuj Tripathi
*   **最終更新者/日付** - Indiradarshni B、NATDソリューション・エンジニアリング、2022年12月