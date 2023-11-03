# Oracle Identity Governance (OIG)ドメインのデプロイ

## 概要

このラボでは、Oracle WebLogic Kubernetes Operator (3.1.0)を利用して、kubernetesクラスタでのOIGドメインのデプロイおよび実行に関連するステップについて説明します

_見積時間:_ 30分

ラボのクイック・ウォークスルーについては、次のビデオをご覧ください。[Oracle Identity Governance (OIG)ドメインのデプロイ](videohub:1_furulwif)

### 製品・技術について

Oracle Identity Governance(OIG)は、エンタープライズITリソース内のユーザーのアクセス権限を自動的に管理する強力で柔軟なエンタープライズ・アイデンティティ管理システムです。Oracle WebLogic Kubernetes Operatorは、Oracle Identity Governance (OIG)のデプロイメントをサポートします。OIGドメインは、ドメイン・ホームが永続ボリューム(PV)に配置されている「永続ボリューム上のドメイン」モデルを使用してサポートされます。

### 目標

この演習では、次のことを行います。

*   kubernetes環境でのOIGのデプロイ

### 前提条件

この演習では、次を想定しています。

*   Free Tier、有料またはLiveLabs Oracle Cloudアカウント
*   次を完了しました:
    *   演習: 設定の準備(_無料層_および_有料テナント_のみ)
    *   演習: 環境設定
    *   演習: 環境の初期化

## タスク1: Oracle WebLogic Server Kubernetes Operator Docker Imageのインストールと実行

Oracle WebLogic Server Kubernetes Operator Dockerイメージは、マスター・ノードおよびKubernetesクラスタ内の各ワーカー・ノードにインストールする必要があります。または、クラスタがアクセスできるDockerレジストリにイメージを配置することもできます。

1.  マスター・ノードで次のコマンドを実行して、Oracle WebLogic Server Kubernetes Operatorイメージをプルします。
    
        	<copy>docker pull ghcr.io/oracle/weblogic-kubernetes-operator:3.1.0</copy>
        
2.  docker tagコマンドを次のように実行します。
    
        	<copy>docker tag ghcr.io/oracle/weblogic-kubernetes-operator:3.1.0 weblogic-kubernetes-operator:3.1.0</copy>
        
3.  weblogic kubernetesオペレータのイメージを確認します。
    
        	<copy>docker images | grep operator</copy>
        

## タスク2: Oracle Identity Governanceドメインをデプロイするためのコード・リポジトリの設定

1.  作業ディレクトリを作成し、オペレータgithubプロジェクトからOracle WebLogic Kubernetes Operator 3.1.0ソース・コードをダウンロードします
    

mkdir -p /u01/k8siam cd /u01/k8siam git clone https://github.com/oracle/weblogic-kubernetes-operator.git --branch release/3.1.0 \`\`\`

2.  OIGリポジトリからOracle Identity Governanceデプロイメント・スクリプトをクローニングし、WebLogicオペレータ・サンプルの場所にコピーします
    
        	<copy>git clone https://github.com/oracle/fmw-kubernetes.git</copy>
        
    
        	<copy>cp -rf /u01/k8siam/fmw-kubernetes/OracleIdentityGovernance/kubernetes/create-oim-domain  /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/</copy>
        

## タスク3: Oracle WebLogic Kubernetes Operatorのインストール

1.  次のコマンドを実行して、演算子のネームスペースを作成します
    
        	<copy>kubectl create namespace operator</copy>
        
2.  オペレータのネームスペースにオペレータのサービス・アカウントを作成します。
    
        	<copy>kubectl create serviceaccount -n operator operator-serviceaccount</copy>
        
3.  次のhelmコマンドを実行して、オペレータをインストールおよび起動します。
    
        	<copy>
        

cd weblogic-kubernetes-operator/helm install weblogic-kubernetes-operator kubernetes/charts/weblogic-operator  
\--namespace operator  
\--set image=weblogic-kubernetes-operator:3.1.0  
\--set serviceAccount=operator-serviceaccount  
\--set "domainNamespaces={}" \`\`\`\`\`

    ![Install operator pod](images/7-helm.png)
    

4.  オペレータのhelmインストールを確認します。
    
        	<copy>helm ls -n operator</copy>
        
5.  オペレータのポッドが1/1 READY状態で動作していることを確認します。
    
        	<copy>kubectl get all -n operator</copy>
        
    
    ![オペレータ・ポッドの状態](images/8-operator.png)
    

## タスク4: RCUスキーマの作成

1.  次のコマンドを使用して、ドメインのネームスペースを作成します:
    
        	<copy>kubectl create namespace oimcluster</copy>
        
2.  次のコマンドを実行してヘルパー・ポッドを作成します:
    
        	<copy>kubectl run helper --image oracle/oig:12.2.1.4.0 -n oimcluster -- sleep infinity</copy>
        
3.  ヘルパー・ポッドの確認
    
        	<copy>kubectl get pods -n oimcluster</copy>
        
4.  ヘルパー・ポッドでbashシェルを起動します。これにより、実行中のrcuポッドのbashシェルに移動します。
    
        	<copy>kubectl exec -it helper -n oimcluster -- /bin/bash</copy>
        
5.  ヘルパーbashシェルで、次のコマンドを実行して環境を設定します。前の演習の「環境の初期化」、タスク2、ステップ2(インスタンス・プライベートIP)からコピーした_`<PRIVATE_IP>`_パラメータを置き換えます。
    
        	<copy>
        	export DB_HOST=<PRIVATE_IP>
        	</copy>
        
    
        	<copy>
        	export DB_PORT=1521
        	export DB_SERVICE=orcl.livelabs.oraclevcn.com
        	export RCUPREFIX=OIGK8S
        	export RCU_SCHEMA_PWD=Welcom#123
        	echo -e Welcom#123"\n"Welcom#123 > /tmp/pwd.txt
        	cat /tmp/pwd.txt
        	export RCU_SCHEMA_PWD=Welcom#123
        	</copy>
        
    
    たとえば、次のとおりです。
    
        	export DB_HOST=11.0.2.64
        

    export DB_PORT=1521
    export DB_SERVICE=orcl.livelabs.oraclevcn.com
    export RCUPREFIX=OIGK8S
    export RCU_SCHEMA_PWD=Welcom#123
    echo -e Welcom#123"\n"Welcom#123 > /tmp/pwd.txt
    cat /tmp/pwd.txt
    export RCU_SCHEMA_PWD=Welcom#123
    ```
    

6.  ヘルパーbashシェルで次のコマンドを実行して、データベースにRCUスキーマを作成します。
    
        	<copy>/u01/oracle/oracle_common/bin/rcu -silent -createRepository -databaseType ORACLE -connectString \
        	$DB_HOST:$DB_PORT/$DB_SERVICE -dbUser sys -dbRole sysdba -useSamePasswordForAllSchemaUsers true \
        	-selectDependentsForComponents true -schemaPrefix $RCUPREFIX -component OIM -component MDS -component SOAINFRA -component OPSS \
        	-f < /tmp/pwd.txt</copy>
        
    
    _**ノート: RCUスキーマの作成には約5分から8分かかる場合があります。**_
    
    ![RCUスキーマの作成](images/9-rcu.png)
    
7.  ヘルパーbashシェルを終了します
    
        	<copy>exit</copy>
        

## タスク5: ドメイン作成のための環境の準備

1.  ドメイン・ネームスペース内のドメインを管理するためのOracle WebLogic Kubernetes Operatorの構成
    
        	<copy>helm upgrade --reuse-values --namespace operator --set "domainNamespaces={oimcluster}" --wait weblogic-kubernetes-operator kubernetes/charts/weblogic-operator</copy>
        
2.  ドメインと同じKubernetesネームスペースでcreate-weblogic-credentialsスクリプトを使用して、ドメインのKubernetesシークレットを作成します:
    
        	<copy>
        

cd /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/create-weblogic-domain-credentials ./create-weblogic-credentials.sh -u weblogic -p Welcom@123 -n oimcluster -d oimcluster -s oimcluster-domain-credentials \`\`\`

    where:
    
    -u weblogic is the WebLogic username
    
    -p <pwd> is the password for the WebLogic user
    
    -n <domain_namespace> is the domain namespace
    
    -d <domain_uid> is the domain UID to be created. The default is domain1 if not specified
    
    -s <kubernetes_domain_secret> is the name you want to create for the secret for this namespace. The default is to use the domainUID if not specified
    

3.  ドメイン・シークレットが作成されたことを確認します
    
        	<copy>kubectl get secret oimcluster-domain-credentials -o yaml -n oimcluster</copy>
        
    
    ![ドメイン・シークレット](images/10-secret.png)
    
4.  create-weblogic-credentials.shスクリプトを使用して、ドメインと同じKubernetesネームスペースにRCUのKubernetesシークレットを作成します:
    
        	<copy>
        

cd /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/create-rcu-credentials ./create-rcu-credentials.sh -u OIGK8S -p Welcom#123 -a sys -q Welcom#123 -d oimcluster -n oimcluster -s oimcluster-rcu-credentials \`\`

    where:
    
    -u <rcu_prefix> is the name of the RCU schema prefix created previously
    
    -p <rcu_schema_pwd> is the password for the RCU schema prefix
    
    -q <sys_db_pwd> is the sys database password
    
    -d <domain_uid> is the domain_uid that you created earlier
    
    -n <domain_namespace> is the domain namespace
    
    -s <kubernetes_rcu_secret> is the name of the rcu secret to create
    

5.  RCUシークレットが作成されたことの確認
    
        	<copy>kubectl get secret oimcluster-rcu-credentials -o yaml -n oimcluster</copy>
        
    
    ![RCUシークレット](images/11-secret.png)
    

## タスク6: Kubernetes永続ボリュームおよび永続ボリューム要求の作成

1.  永続ボリュームに必要なディレクトリを作成します。
    
        	<copy>
        

mkdir -p /u01/domains/oimclusterdomainpv chmod 777 /u01/domains/oimclusterdomainpv \`\`

2.  create-PV-PVC-inputs.yamlファイルのバックアップ・コピーを作成し、create-PV-PVC.shスクリプトを実行してPVおよびPVC構成ファイルを作成します。
    
        	<copy>
        

cd /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/create-weblogic-domain-pv-pvc cp create-pv-pvc-inputs.yaml create-pv-pvc-inputs.yaml.orig mkdir output\_oimcluster cp /u01/sampleFilesOIG/create-pv-pvc-inputs.yaml . ./create-pv-pvc.sh -i create-pv-pvc-inputs.yaml -o output\_oimcluster \`\`\`

    ![Create persistent volume](images/12-pv.png)
    

3.  ファイルが作成されたことを示すには、次を実行します。
    
        	<copy>ls output_oimcluster/pv-pvcs</copy>
        
4.  次のkubectlコマンドを実行して、ドメイン・ネームスペースにPVおよびPVCを作成します:
    
        	<copy>kubectl create -f output_oimcluster/pv-pvcs/oimcluster-oim-pv.yaml -n oimcluster</copy>
        
    
        	<copy>kubectl create -f output_oimcluster/pv-pvcs/oimcluster-oim-pvc.yaml -n oimcluster</copy>
        
5.  作成されたPVおよびPVCの検証
    
        	<copy>kubectl describe pv oimcluster-oim-pv -n oimcluster</copy>
        
    
    ![永続ボリュームの説明](images/13-pv.png)
    
        	<copy>kubectl describe pvc oimcluster-oim-pvc -n oimcluster</copy>
        
    
    ![永続ボリュームの説明](images/14-pv.png)
    

## タスク7: OIGドメインの作成

1.  create-domain-inputs.yamlファイルのコピーを作成します。
    
        	<copy>
        

cd /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/create-oim-domain/domain-home-on-pv cp create-domain-inputs.yaml create-domain-inputs.yaml.orig mkdir output\_oimcluster cp /u01/sampleFilesOIG/create-domain-inputs.yaml . \`\`

3.  _rcuDatabaseURL_パラメータを編集して、インスタンスのプライベートIPを含めます(前の演習の「環境の初期化」、タスク2、ステップ2を参照)。または、コマンド_cat /etc/hosts_を指定してプライベートIPを取得することもできます。_rcuDatabaseURL_パラメータは、_create-domain-inputs.yaml_ファイルの最後までです。ファイルの末尾に移動し、インスタンスのプライベートIPにパラメータを更新します。

_ノート: ファイルを編集するには、コマンド'vi 'を使用します。その後、矢印キーを使用して、ファイル内で変更を行う必要がある場所に移動できます(ショートカットとして、Shift + Gを使用してファイルの末尾に直接移動できます)。キーボードの「i」を押して挿入モードに切り替えます。必要な変更を入力するか、コピーしたコンテンツを貼り付ける場合は、まず右クリックしてカーソルが正確な場所にあることを確認します。完了したら、\[Esc\]を押して挿入モードを終了します。最後に、「:wq」を押して変更を保存し、viエディタを終了します。_

_ノート: パラメータfrontEndHostをデフォルト値から更新する必要はありません。スクリプトの実行に必要なサンプル値を追加しました。これを使用して、次のステップでweblogic/oimコンソールにアクセスしないでください。_

    ```
    <copy>vi create-domain-inputs.yaml</copy>
    ```
    
    ![Update rcuDatabaseURL parameter](images/15-rcu.png)
    

## タスク8: ドメイン作成スクリプトを実行して、ドメイン関連のkubernetesアーティファクトを生成します

1.  ドメインの作成スクリプトを実行し、入力ファイルと出力ディレクトリを指定して、生成されたアーティファクトを格納します。

_ノート: これには約6-7分かかる場合があります。_

    ```
    <copy>./create-domain.sh -i create-domain-inputs.yaml -o output_oimcluster</copy>
    ```
    

## タスク9: Dockerレジストリ・シークレットおよびKubernetesリソースの作成

1.  oig-dockerという名前のDockerレジストリ・シークレットを作成します。
    
    _ノート: このコマンドはそのまま実行できます。次のコマンドでパラメータ値を更新する必要はありません。_
    
        	<copy>kubectl create secret docker-registry oig-docker -n oimcluster --docker-username='<user_name>' --docker-password='<password>' --docker-server='<docker_registry_url>' --docker-email='<email_address>'</copy>
        
2.  次のコマンドを使用して、Kubernetesリソースを作成します:
    
        	<copy>
        

cd output\_oimcluster/weblogic-domains/oimcluster/ kubectl apply -f domain.yaml \`\`

3.  OIGポッドのステータスを表示するには、次のコマンドを実行します。
    
        	<copy>kubectl get pods -n oimcluster</copy>
        
    
    _**ノート: 最初にintrospect-domain-jobポッドが表示され、管理サーバーおよびsoa-serverポッドの作成を担当します。**_
    
    ![OIGポッド](images/16-pods.png)
    
4.  adminserverおよびsoa-serverのポッドが1/1状態になるまで8~10分かかります。ポッドの起動中に、次のコマンドを実行して、ポッド・ログの起動ステータスを確認できます:
    
        	<copy>kubectl logs oimcluster-adminserver -n oimcluster</copy>
        
    
        	<copy>kubectl logs oimcluster-soa-server1 -n oimcluster</copy>
        

_**ノート: 次のステップに進む必要があるのは、管理サーバーおよびsoa-serverのポッドがREADY 1/1状態になった場合のみです。**_

5.  前のステップの両方のポッドが1/1 READY状態になったら、次のコマンドを使用してOIMサーバーを起動します。
    
        	<copy>kubectl apply -f domain_oim_soa.yaml</copy>
        
    
        	<copy>kubectl get pods -n oimcluster -o wide</copy>
        
    
    OIMポッド(OIM-server)がREADY 1/1状態になるまで、約6分から8分かかる場合があります。ポッドの起動中に、ポッド・ログで起動ステータスを確認できます。
    
    ![OIGポッド](images/17-pods.png)
    
        	<copy>kubectl logs oimcluster-oim-server1 -n oimcluster</copy>
        

## タスク10: ドメイン、ポッドおよびサービスの検証

1.  次のコマンドを実行して、ドメイン、サーバー・ポッドおよびサービスが作成され、READY状態でSTATUSが1/1であることを確認します。
    
        	<copy>kubectl get all,domains -n oimcluster -o wide</copy>
        
    
    スクリプトによって作成されるデフォルトのドメインには、次の特性があります。
    
    ポート7001でリスニングするAdminServerという名前の管理サーバー。
    
    サイズ3のoig\_clusterという名前の構成済OIGクラスタ。
    
    サイズ3のsoa\_clusterという名前の構成済SOAクラスタ。
    
    oim\_server1という名前のOIG管理対象サーバーを起動し、ポート14000でリスニングしています。
    
    soa\_server1という名前のSOA管理対象サーバーを起動し、ポート8001でリスニングしています。
    
    ログ・ファイルは/u01/domains/oimclusterdomainpv/logs/oimclusterにあります。
    
    ![OIGポッド](images/18-pods.png)
    
2.  ドメインの確認
    
        	<copy>kubectl describe domain oimcluster -n oimcluster</copy>
        
    
    出力の「ステータス」セクションに、使用可能なサーバーおよびクラスタがリストされます。
    
3.  ポッドを確認します。管理サーバーのポッドIPアドレスとIP列のoim\_server1に注意してください。
    
        	<copy>kubectl get pods -n oimcluster -o wide</copy>
        
    
    たとえば、次のとおりです。
    
    ![OIGポッド](images/20-pods.png)
    
4.  ブラウザ・ウィンドウを開き、次のURLを使用して管理サーバー・ポッドIPを使用してweblogicコンソールにアクセスします。
    
        	<copy><ADMIN_IP>:7001/console</copy>
        
    
    ここで、_`<ADMIN_IP>`_は、前のステップで書き留めた管理サーバー・ポッドIPです。
    
    ![管理コンソール](images/21-vnc.png)
    
    次の資格証明を使用してコンソールにログインします。
    
    ユーザー名:
    
        	<copy>weblogic</copy>
        
    
    パスワード:
    
        	<copy>Welcom@123</copy>
        
    
    _「環境」_の下の_「サーバー」_をクリックし、SOAおよびOIMサーバーが実行されていることを確認します。
    
    ![管理コンソール](images/22-vnc.png)
    
    ![サーバー・ステータス](images/29-vnc.png)
    
5.  ブラウザ・タブを開き、次のURLを使用してoim\_server1ポッドIPを使用してOIMコンソールにアクセスします。
    
        	<copy><OIM_IP>:14000/identity</copy>
        
    
    ここで、_`<OIM_IP>`_は、前のステップで記録したoim\_server1ポッドIPです。
    
    次の資格証明を使用してコンソールにログインします。
    
    ユーザー名:
    
        	<copy>xelsysadm</copy>
        
    
    パスワード:
    
        	<copy>Welcom@123</copy>
        
    
    ![OIGコンソール](images/23-vnc.png)
    
    ![OIGコンソール](images/24-vnc.png)
    
    ![OIGコンソール](images/25-vnc.png)
    

_ノート: OIMおよびSOAサーバー・コンソールには、それぞれのクラスタIPを使用してアクセスすることもできます。_

次の演習に進むことができます。

## さらに学ぶ

*   [DockerおよびKubernetesでのOracle Identity Governanceリファレンス](https://docs.oracle.com/en/middleware/idm/identity-governance/12.2.1.4/oigdk/overview.html#GUID-1BA2B257-ED5A-4606-B833-C40B2F36F35F)

## 謝辞

*   **著者** - NATDソリューション・エンジニアリング、Anuj Tripathi、Keerti R氏
*   **貢献者** - Keerti R、Anuj Tripathi
*   **最終更新者/日付** - NATDソリューション・エンジニアリング、Keerti R、2022年1月