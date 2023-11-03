# 環境の初期化

## 概要

この演習では、このワークショップを正常に実行するために必要なすべてのコンポーネントを確認して開始します。

_予想時間:_ 10分

### 目標

この演習では、次のことを行います。

*   ワークショップ・インスタンスを起動します
*   Oracle Databaseの検証
*   Kubernetesノードの作成および初期化

### 前提条件

この演習では、次を想定しています。

*   Free Tier、有料またはLiveLabs Oracle Cloudアカウント
*   次を完了しました:
    *   演習: 設定の準備(_無料層_および_有料テナント_のみ)
    *   演習: 環境設定

## タスク1: 環境検証

1.  ターミナル・インスタンスを開き、OIGデータベースが実行されていることを確認します。
    
        	<copy>systemctl status oracle-database.service</copy>
        
    
    ![データベース・システム・サービス](images/2-db.png)
    
    Ctrl+Cを押して端末に戻ります。
    
2.  dockerとhelmのバージョンを確認します。
    
        	<copy>docker version</copy>
        
    
        	<copy>helm version</copy>
        
    
    ![Dockerおよびhelmバージョン](images/1-versions.png)
    
3.  OIG、OAMおよびOUDのdockerイメージを確認します。
    
        	<copy>docker images | grep oracle</copy>
        
    
    ![Dockerイメージ](images/3-dockerimages.png)
    

## タスク2: 環境初期化

1.  環境を初期化するには、次の前提条件の設定ステップを実行します。
    
        	<copy>
        

sudo setenforce 0 sudo sed -i --follow-symlinks 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux sudo swapoff -a \`\`\`

2.  インスタンスのプライベートIPに注意してください。
    
        	<copy>cat /etc/hosts</copy>
        
    
    たとえば:
    
    ![/etc/hostsファイル](images/4-ip.png)
    
3.  ポッド・ネットワークをデプロイおよび初期化し、ポッド・ネットワークがホスト・ネットワークと重複していないことを確認します。
    
        	<copy>sudo kubeadm init --pod-network-cidr=10.244.0.0/16</copy>
        

_ノート: ポッド・ネットワーク初期化のために前述のコマンドで使用されるネットワークCIDR範囲は10.244.0.0/16で、これは10.0.0.0/24のホスト・サブネットCIDRに対する非OVERLAPPINGです。ホスト・サブネットCIDRは、デプロイメントごとに同じままになります。_

4.  kubectlがroot以外のユーザーと連携できるようにします。
    
        	<copy>sudo mkdir -p /home/oracle/.kube
        	sudo cp -i /etc/kubernetes/admin.conf /home/oracle/.kube/config
        

sudo chown -R oracle:oinstall /home/oracle/.kube \`\`\`

5.  Kubernetesのバージョンを確認し、コントロール・ペイン・ノードでポッドをスケジュールします。
    
        	<copy>kubectl version --short</copy>
        
    
    ![Kubectlバージョン](images/5-kube.png)
    
        	<copy>kubectl taint nodes --all node-role.kubernetes.io/control-plane-</copy>
        
6.  すべてのネームスペースのすべてのポッドをリストします。
    
        	<copy>kubectl get pods --all-namespaces</copy>
        
7.  クラスタ内のリソースを更新し、すべてのポッドが「実行中」状態であることを確認します。
    
        	<copy>kubectl apply -f https://raw.githubusercontent.com/flannel-io/flannel/master/Documentation/kube-flannel.yml</copy>
        
    
    1-2分待って、すべてのポッドをリストし、それらが「実行中」状態であることを確認します。
    
        	<copy>kubectl get pods --all-namespaces</copy>
        
    
    ![Kube-systemポッド](images/6-pod.png)
    

次の演習に進むことができます。

## 確認

*   **著者** - NATDソリューション・エンジニアリング、Anuj Tripathi、Keerti R氏
*   **貢献者** - Keerti R、Anuj Tripathi
*   **最終更新者/日付** - NATDソリューション・エンジニアリング、Keerti R、2022年1月