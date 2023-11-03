# Oracle Unified Directory(OUD)のデプロイ

## 概要

このラボでは、Kubernetes環境でのOracle Unified Directory 12c PS4 (12.2.1.4.0)のデプロイおよび実行に関連するステップについて説明します。

_見積時間:_ 20分

### <製品・技術>について

Oracle Unified Directoryは、堅牢なアイデンティティ管理のための包括的なディレクトリ・ソリューションを提供します。Oracle Unified Directoryは、ストレージ、プロキシ、同期および仮想化機能を備えたオールインワンのディレクトリ・ソリューションです。アプローチを統合しながら、高パフォーマンスのエンタープライズおよびキャリアグレード環境に必要なすべてのサービスを提供します。Oracle Unified Directoryは、数十億のエントリに対するスケーラビリティ、インストールの容易さ、柔軟なデプロイメント、エンタープライズ管理性および効果的な監視を保証します。

### 目標

この演習では、次のことを行います。

*   kubernetes環境でのOUDインスタンスの作成

### 前提条件

この演習では、次を想定しています。

*   Free Tier、有料またはLiveLabs Oracle Cloudアカウント
*   次を完了しました:
    *   演習: 設定の準備(_無料層_および_有料テナント_のみ)
    *   演習: 環境設定
    *   演習: 環境の初期化
    *   演習: Oracle Identity Governance(OIG)ドメインのデプロイ
    *   演習: Oracle Access Manager(OAM)ドメインのデプロイ

## タスク1: コンテナ作成のための環境の準備

1.  Kubernetesクラスタの準備完了の確認
    
        	<copy>kubectl get nodes,pods -n kube-system</copy>
        
    
    ![Kubernetesクラスタ](images/1-kube.png)
    
2.  Oracle Unified Directoryイメージの確認
    
        	<copy>docker images | grep oud</copy>
        
3.  Kubernetesネームスペースの作成
    
        	<copy>cd /u01/k8siam/fmw-kubernetes/OracleUnifiedDirectory/kubernetes/samples/</copy>
        
    
        	<copy>cp oudns.yaml oudns.yaml_orig</copy>
        
    
        	<copy>cp /u01/sampleFilesOUD/oudns.yaml .</copy>
        
    
        	<copy>kubectl apply -f oudns.yaml</copy>
        
4.  ネームスペースが作成されていることを確認します。
    
        	<copy>kubectl get namespaces </copy>
        
5.  ユーザーIDとパスワードのシークレットの作成
    
        	<copy>cp secrets.yaml secrets.yaml_orig</copy>
        
    
        	<copy>cp /u01/sampleFilesOUD/secrets.yaml .</copy>
        
    
    このファイルには、更新されたシークレット、ネームスペース、および指定された値を含むその他のパラメータがBase64形式で含まれます
    
    秘密- 秘密
    
    namespace - myoudns
    
    %rootUserDN% - rootUserDNパラメータの'cn=Directory Manager'のBase64エンコード値
    
    %rootUserPassword% - 'Welcom@123' rootUserPasswordパラメータのBase64エンコード値
    
    %adminUID% - adminUIDパラメータの'admin'のBase64エンコード値
    
    %adminPassword% - adminPasswordパラメータの'Welcom@123'のBase64エンコード値
    
    %bindDN1% - 'cn=Directory Manager bindDN1パラメータのBase64エンコード値
    
    %bindPassword1% - bindPassword1パラメータの'Welcom@123'のBase64エンコード値
    
    %bindDN2% - bindDN2パラメータの'cn=Directory Manager'のBase64エンコード値
    
    %bindPassword2% - bindPassword2パラメータの'Welcom@123'のBase64エンコード値
    
6.  ファイルを適用します。
    
        	<copy>kubectl apply -f secrets.yaml</copy>
        
7.  シークレットが作成されたことを確認します。
    
        	<copy>kubectl --namespace myoudns get secret</copy>
        
8.  ファイルシステムベースの PersistentVolumeに使用するホストディレクトリを準備します
    
        	<copy>mkdir -p /u01/domains/oud_user_projects</copy>
        
    
        	<copy>chmod 777 /u01/domains/oud_user_projects</copy>
        
    
        	<copy>cp persistent-volume.yaml persistent-volume.yaml_orig</copy>
        
    
        	<copy>cp /u01/sampleFilesOUD/persistent-volume.yaml .</copy>
        
9.  ファイルを適用します。
    
        	<copy>kubectl apply -f persistent-volume.yaml</copy>
        
10.  PersistentVolumeおよびPersistentVolumeClaimを確認します。
    
        <copy>kubectl --namespace myoudns describe persistentvolume oudpv</copy>
        
    
        <copy>kubectl --namespace myoudns describe pvc oudpvc</copy>
        
    
    ![永続ボリュームの検証](images/2-pv.png)
    

## タスク2: ディレクトリ・サーバー(instanceType= ディレクトリ)

この演習の一部として、Oracle Unified Directory 12c PS4 (12.2.1.4.0)イメージに基づく単一のコンテナで構成されるPOD (oudpod1)を作成します。

1.  PODを作成するには、適切な値でoud-dir-POD.yamlファイルのコピーを更新します。
    
        	<copy>cd /u01/k8siam/fmw-kubernetes/OracleUnifiedDirectory/kubernetes/samples/</copy>
        
    
        	<copy>cp /u01/sampleFilesOUD/oud-dir-pod.yaml .</copy>
        
    
    このサンプル・ファイルには、次の更新済パラメータが含まれています。
    
    ネームスペース- myoudns
    
    Oracleイメージ・タグ- oracle/oud:12.2.1.4.0
    
    秘密の名前- oudsecret
    
    PV名- oudpv
    
    PVC名- oudpvc
    
    サンプルエントリ数- 15
    
2.  ファイルを適用します。
    
        	<copy>kubectl apply -f oud-dir-pod.yaml</copy>
        
3.  作成されたポッドのステータスを確認します:
    
        	<copy>kubectl get pods -n myoudns</copy>
        
    
    ![OUDポッド](images/3-pods.png)
    
    コンテナの初期化中にコンテナ・ログを修復するには、次のコマンドを使用します。
    
        	<copy>kubectl --namespace myoudns logs -f -c oudds1 oudpod1</copy>
        
    
    Ctrl+Cを押してコンテナ・ログを終了します
    
4.  Oracle Unified Directoryディレクトリ・サーバー・インスタンスが実行されていることを検証するには、コンテナに接続します。
    
        	<copy>kubectl --namespace myoudns exec -it -c oudds1 oudpod1 /bin/bash</copy>
        
5.  コンテナでldapsearchを実行して、ディレクトリ・サーバーからエントリを返します。
    
        	<copy>cd /u01/oracle/user_projects/oudpod1/OUD/bin</copy>
        
    
        	<copy>./ldapsearch -h localhost -p 1389 -D "cn=Directory Manager" -w Welcom@123 -b "" -s sub "(objectclass=*)" dn</copy>
        
    
    ![ユーザー・エントリ](images/4-oud.png)
    
        	<copy>exit</copy>
        

## さらに学ぶ

*   [DockerおよびKubernetesでのOracle Unified Directoryリファレンス](https://docs.oracle.com/en/middleware/idm/access-manager/12.2.1.4/oamkd/overview.html#GUID-38F207C8-E648-4A79-8205-942DAD5F674A)

## 確認

*   **著者** - NATDソリューション・エンジニアリング、Anuj Tripathi、Keerti R氏
*   **貢献者** - Keerti R、Anuj Tripathi
*   **最終更新者/日付** - NATDソリューション・エンジニアリング、Keerti R、2022年1月