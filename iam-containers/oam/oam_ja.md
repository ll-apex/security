# Oracle Access Manager (OAM)ドメインのデプロイ

## 概要

このラボでは、Oracle WebLogic Kubernetes Operator (3.1.0)を利用して、KubernetesクラスタでOAMドメインをデプロイおよび実行するためのステップについて説明します。

_見積時間:_ 30分

### 製品・技術について

Oracle Access Managementは、従来のアクセス管理機能を補完する革新的な新しいサービスを提供します。MFA、粗粒度の高い認可およびセッション管理を備えたWeb SSOを提供するだけでなく、標準のSAMLフェデレーションおよびOAuth機能も提供して、外部のクラウドおよびモバイル・アプリケーションに安全にアクセスできます。Oracle Identity Cloud Serviceと簡単に統合して、ハイブリッド・アクセス管理機能をサポートできるため、お客様はオンプレミスおよびクラウドのアプリケーションとワークロードをシームレスに保護できます。Oracle WebLogic Server Kubernetes Operatorは、Oracle Access Management (OAM)のデプロイメントをサポートします。

OAMドメインは、ドメイン・ホームが永続ボリューム(PV)に配置されている「永続ボリューム上のドメイン」モデルを使用してサポートされます。

### 目標

この演習では、次のことを行います。

*   kubernetes環境でのOAMのデプロイ

### 前提条件

この演習では、次を想定しています。

*   Free Tier、有料またはLiveLabs Oracle Cloudアカウント
*   次を完了しました:
    *   演習: 設定の準備(_無料層_および_有料テナント_のみ)
    *   演習: 環境設定
    *   演習: 環境の初期化
    *   演習: Oracle Identity Governance(OIG)ドメインのデプロイ

## タスク1: OAMドメインをデプロイするためのコード・リポジトリの設定

次に示すコンポーネントは、OIGデプロイメント中に(ディレクトリ/u01/k8siamの下に)すでにインストールされているため、再構成する必要はありません。OAMデプロイメント・スクリプトをKubernetes Operatorサンプルの場所に直接コピーできます。

*   Oracle WebLogic Server Kubernetes Operator Dockerイメージ
*   WebLogic Kubernetesオペレータのソース・コード
*   FMW KubernetesリポジトリからのOAMデプロイメント・スクリプト

1.  OAMデプロイメント・スクリプトをOracle WebLogic Server Kubernetes Operatorのサンプルの場所にコピーします。
    
        	<copy>cd /u01/k8siam</copy>
        
    
        	<copy>cp -rf /u01/k8siam/fmw-kubernetes/OracleAccessManagement/kubernetes/create-access-domain  /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/</copy>
        

## タスク2: Oracle WebLogic Server Kubernetes Operatorのインストール

1.  次のコマンドを実行して、演算子のネームスペースを作成します:
    
        	<copy>kubectl create namespace opns</copy>
        
2.  次のコマンドを実行して、オペレータの名前空間にオペレータのサービスアカウントを作成します。
    
        	<copy>kubectl create serviceaccount -n opns op-sa</copy>
        
3.  次のhelmコマンドを実行して、オペレータをインストールおよび起動します。
    
        	<copy>cd /u01/k8siam/weblogic-kubernetes-operator</copy>
        
    
        	<copy>helm install weblogic-kubernetes-operator kubernetes/charts/weblogic-operator \
        	--namespace opns --set image=weblogic-kubernetes-operator:3.1.0 \
        	--set serviceAccount=op-sa --set "domainNamespaces={}" --set "javaLoggingLevel=FINE" --wait</copy>
        
    
    ![オペレータ・ポッドのインストール](images/1-helm.png)
    
4.  オペレータのポッドとサービスが実行中であることを確認します
    
        	<copy>kubectl get all -n opns</copy>
        
    
    ![オペレータ・ポッド](images/2-kube.png)
    

## タスク3: RCUスキーマの作成

1.  ドメインのネームスペースを作成します。
    
        	<copy>kubectl create namespace accessns</copy>
        
2.  RCUを実行するヘルパー・ポッドを作成します:
    
        	<copy>kubectl run helperoam --image oracle/oam:12.2.1.4.0 -n accessns -- sleep infinity</copy>
        
3.  ポッドが実行されていることを確認します:
    
        	<copy>kubectl get pods -n accessns</copy>
        
4.  ヘルパー・ポッドでbashシェルを起動します:
    
        	<copy>kubectl exec -it helperoam -n accessns -- /bin/bash</copy>
        
5.  _`<PRIVATE_IP>`_をインスタンスのプライベートIP(演習1に記載)に置き換え、次のコマンドを実行して環境を設定します。
    
        	<copy>export CONNECTION_STRING=<PRIVATE_IP>:1521/orcl.livelabs.oraclevcn.com
        	export RCUPREFIX=OAMK8S
        	echo -e Welcom#123"\n"Welcom#123 > /tmp/pwd.txt
        	cat /tmp/pwd.txt</copy>
        
    
    たとえば、次のとおりです。
    
        	export CONNECTION_STRING=11.0.2.64:1521/orcl.livelabs.oraclevcn.com
        	export RCUPREFIX=OAMK8S
        	echo -e Welcom#123"\n"Welcom#123 > /tmp/pwd.txt
        	cat /tmp/pwd.txt
        
6.  データベースにRCUスキーマを作成します。これには約3分から4分かかる場合があります。
    
        	<copy>/u01/oracle/oracle_common/bin/rcu -silent -createRepository -databaseType ORACLE -connectString \
        	$CONNECTION_STRING -dbUser sys -dbRole sysdba -useSamePasswordForAllSchemaUsers true \
        	-selectDependentsForComponents true -schemaPrefix $RCUPREFIX -component MDS -component IAU \
        	-component IAU_APPEND -component IAU_VIEWER -component OPSS -component WLS -component STB -component OAM -f < /tmp/pwd.txt</copy>
        
    
    ![RCUスキーマの作成](images/3-rcu.png)
    
7.  ヘルパーbashシェルを終了します
    
        	<copy>exit</copy>
        

## タスク4: ドメイン作成のための環境の準備

1.  ドメイン・ネームスペースの演算子の構成
    
        	<copy>cd /u01/k8siam/weblogic-kubernetes-operator</copy>
        
    
        	<copy>helm upgrade --reuse-values --namespace opns --set "domainNamespaces={accessns}" --wait weblogic-kubernetes-operator kubernetes/charts/weblogic-operator</copy>
        
2.  ドメインのKubernetesシークレットの作成
    
        	<copy>cd /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/create-weblogic-domain-credentials</copy>
        
    
        	<copy>./create-weblogic-credentials.sh -u weblogic -p Welcom@123 -n accessns -d accessinfra -s accessinfra-domain-credentials</copy>
        
3.  ドメイン・シークレットが作成されたことを確認します
    
        	<copy>kubectl get secret accessinfra-domain-credentials -o yaml -n accessns</copy>
        
4.  RCUのKubernetesシークレットの作成
    
        	<copy>cd /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/create-rcu-credentials</copy>
        
    
        	<copy>./create-rcu-credentials.sh -u OAMK8S -p Welcom#123 -a sys -q Welcom#123 -d accessinfra -n accessns -s accessinfra-rcu-credentials</copy>
        
5.  RCUシークレットが作成されたことの確認
    
        	<copy>kubectl get secret accessinfra-rcu-credentials -o yaml -n accessns</copy>
        

## タスク5: Kubernetes永続ボリュームおよび永続ボリューム要求の作成

1.  永続ボリュームに必要なディレクトリを作成します。
    
        	<copy>mkdir -p /u01/domains/accessdomainpv</copy>
        
    
        	<copy>chmod 777 /u01/domains/accessdomainpv</copy>
        
2.  create-PV-PVC-inputs.yamlファイルのバックアップ・コピーを作成し、スクリプトを実行してPVおよびPVC構成ファイルを作成します。
    
        	<copy>cd /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/create-weblogic-domain-pv-pvc</copy>
        
    
        	<copy>mkdir output_access</copy>
        
    
        	<copy>cp create-pv-pvc-inputs.yaml create-pv-pvc-inputs.yaml_OIG</copy>
        
    
        	<copy>cp /u01/sampleFilesOAM/create-pv-pvc-inputs.yaml .</copy>
        
    
        	<copy>./create-pv-pvc.sh -i create-pv-pvc-inputs.yaml -o output_access</copy>
        
3.  ファイルが作成されたことを示すには、次を実行します。
    
        	<copy>ls output_access/pv-pvcs</copy>
        
4.  次のkubectlコマンドを実行して、ドメイン・ネームスペースにPVおよびPVCを作成します:
    
        	<copy>kubectl create -f output_access/pv-pvcs/accessinfra-domain-pv.yaml -n accessns</copy>
        
    
        	<copy>kubectl create -f output_access/pv-pvcs/accessinfra-domain-pvc.yaml -n accessns</copy>
        
5.  作成されたPVおよびPVCの検証
    
        	<copy>kubectl describe pv accessinfra-domain-pv</copy>
        
    
        	<copy>kubectl describe pvc accessinfra-domain-pvc -n accessns</copy>
        
    
    ![永続ボリュームの検証](images/4-pv.png)
    

## タスク6: OAMドメイン・ネームスペースの作成

1.  ドメイン作成スクリプトの準備
    
        	<copy>cd /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/create-access-domain/domain-home-on-pv</copy>
        
    
        	<copy>cp create-domain-inputs.yaml create-domain-inputs.yaml.orig</copy>
        
    
        	<copy>mkdir output_access</copy>
        
    
        	<copy>cp /u01/sampleFilesOAM/create-domain-inputs.yaml .</copy>
        
2.  _create-domain-inputs.yaml_ファイルを編集して、_rcuDatabaseURL_パラメータのインスタンスのプライベートIPを更新します。_rcuDatabaseURL_パラメータは、_create-domain-inputs.yaml_ファイルの最後までです。ファイルの末尾に移動し、インスタンスのプライベートIPにパラメータを更新します。
    
        	<copy>vi create-domain-inputs.yaml</copy>
        
    
    ![rcuDatabaseURLパラメータの更新](images/13-rcu.png)
    

## タスク7: ドメイン作成スクリプトを実行して、ドメイン関連のkubernetesアーティファクトを生成します

1.  ドメインの作成スクリプトを実行し、生成されたアーティファクトを格納する入力ファイルと出力ディレクトリを指定します。
    
        	<copy>./create-domain.sh -i create-domain-inputs.yaml -o output_access</copy>
        

## タスク8: Kubernetesリソースの作成

1.  次のコマンドを使用して、Kubernetesリソースを作成します:
    
        	<copy>cd  /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/create-access-domain/domain-home-on-pv/output_access/weblogic-domains/accessinfra</copy>
        
    
        	<copy>kubectl apply -f domain.yaml</copy>
        

## タスク9: ドメインの確認

1.  OAMポッドのステータスを表示するには、次のコマンドを実行します。
    
        	<copy>kubectl get pods -n accessns</copy>
        
    
    ![OAMポッド](images/6-pods.png)
    
2.  上記のすべてのサービスが表示されるまで約10~15分かかります。ポッドのステータスが0/1の場合、ポッドは起動されますが、ポッドに関連付けられているOAMサーバーが現在起動しています。ポッドの起動中に、次のコマンドを実行して、ポッド・ログの起動ステータスを確認できます:
    
        	<copy>kubectl logs accessinfra-adminserver -n accessns</copy>
        
    
        	<copy>kubectl logs accessinfra-oam-server1 -n accessns</copy>
        
    
    スクリプトによって作成されるデフォルトのドメインには、次の特性があります。
    
    ポート9001でリスニングするAdminServerという名前の管理サーバー。
    
    サイズ3のoam\_clusterという名前の構成済OAMクラスタ。
    
    サイズ3のpolicy\_clusterという名前の構成済ポリシー・マネージャ・クラスタ。
    
    ポート14100でリスニングするoam\_server1という名前のOAM管理対象サーバーを起動しました。
    
    oam-policy-mgr1という名前のポリシー・マネージャ管理対象サーバーを起動し、ポート15100でリスニングしています。
    
    ログ・ファイルは/u01/domains/accessdomainpv/logs/accessinfraにあります。
    
3.  ドメインの説明:
    
        	<copy>kubectl describe domain accessinfra -n accessns</copy>
        
4.  ドメイン、サーバー・ポッドおよびサービスが作成され、ステータスが1/1のREADY状態であることを確認します
    
        	<copy>kubectl get all,domains -n accessns</copy>
        
    
    ![OAMポッド](images/7-pods.png)
    
5.  ポッドを確認します。管理サーバーのポッドIPアドレスをコピーします
    
        	<copy>kubectl get pods -n accessns -owide</copy>
        
    
    たとえば、次のとおりです。
    
    ![OAMポッド](images/12-pods.png)
    
6.  ブラウザ・ウィンドウを開き、次のURLを使用して管理サーバー・ポッドIPを使用してweblogicコンソールにアクセスします。
    
        	<copy><ADMIN_IP>:9001/console</copy>
        
    
    ここで、_`<ADMIN_IP>`_は管理サーバー・ポッドIPです
    
    ![管理コンソール](images/8-vnc.png)
    
    次の資格証明を使用してコンソールにログインします。
    
    ユーザー名:
    
        	<copy>weblogic</copy>
        
    
    パスワード:
    
        	<copy>Welcom@123</copy>
        
    
    _「環境」_の下の_「サーバー」_をクリックして、OAMサーバーが実行中であることを確認します。
    
    ![管理コンソール](images/11-vnc.png)
    

_ノート: ヒープ・サイズが制限されているため、OAM管理対象サーバーは警告状態で表示される場合があります。ヒープ・サイズは、生成されたドメインyamlファイルを更新することで拡張できます。_

8.  ブラウザ・タブを開き、次のURLを使用して管理サーバー・ポッドIPを使用してOAMコンソールにアクセスします。
    
        	<copy><ADMIN_IP>:9001/oamconsole</copy>
        
    
    ここで、_`<ADMIN_IP>`_は管理サーバー・ポッドIPです
    
    ![OAMコンソール](images/9-vnc.png)
    
    次の資格証明を使用してコンソールにログインします。
    
    ユーザー名:
    
        	<copy>weblogic</copy>
        
    
    パスワード:
    
        	<copy>Welcom@123</copy>
        
    
    ![OAMコンソール](images/10-vnc.png)
    

次の演習に進むことができます。

## さらに学ぶ

*   [DockerおよびKubernetesでのOracle Access Managementリファレンス](https://docs.oracle.com/en/middleware/idm/access-manager/12.2.1.4/oamkd/overview.html#GUID-38F207C8-E648-4A79-8205-942DAD5F674A)

## 確認

*   **著者** - NATDソリューション・エンジニアリング、Anuj Tripathi、Brijith TG、Keerti R氏
*   **貢献者** - Keerti R、Brijith TG、Anuj Tripathi
*   **最終更新者/日付** - NATDソリューション・エンジニアリング、Keerti R、2022年1月