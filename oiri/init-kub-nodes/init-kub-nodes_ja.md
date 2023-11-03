# KubernetesクラスタのデプロイおよびOIGサーバーの起動

## 概要

このラボでは、すべてを確認して開始し、このワークショップを正常に実行するために必要なKubernetesノードをデプロイします。

_推定ラボ時間_: 20分

### 目標

この演習では、次のことを行います。

*   Kubernetesクラスタの作成および初期化
*   OIG 12cドメインを起動します。OIGで作成された様々なロールおよび権限を分析します

### 前提条件

この演習では、次を想定しています。

*   Free Tier、有料またはLiveLabs Oracle Cloudアカウント
*   次を完了しました:
    *   演習: 設定の準備(_無料層_および_有料テナント_のみ)
    *   演習: 環境設定
    *   演習: 環境の初期化

## タスク1: Kubernetesクラスタおよびポッド・ネットワーク・アドオンの初期化

1.  端末セッションをオープンします。
    
        <copy>sudo swapoff -a</copy>
        
    
        <copy>sudo setenforce 0</copy>
        
    
        <copy>sudo sed -i --follow-symlinks 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux</copy>
        
2.  ポッド・ネットワークをデプロイおよび初期化し、ポッド・ネットワークがホスト・ネットワークと重複していないことを確認します。
    
        <copy>sudo kubeadm init --pod-network-cidr=10.244.0.0/16</copy>
        
3.  kubectlがroot以外のユーザーと連携できるようにします。
    
        <copy>sudo cp -i /etc/kubernetes/admin.conf /home/oracle/.kube/config</copy>
        
    
        <copy>sudo chown oracle:oinstall /home/oracle/.kube/config</copy>
        
4.  別のターミナル・セッションを起動し、コントロールプレーン・ノードでポッドをスケジュールします。
    
        <copy>kubectl taint nodes --all node-role.kubernetes.io/master-</copy>
        
5.  すべてのネームスペースのすべてのポッドをリストします。
    
        <copy>kubectl get pods --all-namespaces</copy>
        
6.  クラスタ内のリソースを更新し、すべてのポッドが「実行中」状態であることを確認します。
    
        <copy>kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml</copy>
        
    
    1-2分間待機し、すべてのポッドをリストして、それらが_「実行中」_および_「READY 1/1」_状態であることを確認します。
    
        <copy>kubectl get pods --all-namespaces</copy>
        
    
    ![Kubernetesクラスタおよびポッド・ネットワーク・アドオンの初期化](images/pods.png)
    

## タスク2: Oracle Identity Governance (OIG)サーバーの起動とOIGでのロールの分析

1.  管理サーバーが実行されていることを確認します。ブラウザ・ウィンドウを開き、次のURLを使用してWeblogicコンソールにアクセスします。
    
        URL  http://oiri.livelabs.oraclevcn.com:7001/console/login/
        
    
    ![Weblogicコンソール・ページ](images/weblogic-console.png)
    
2.  weblogic資格証明を使用してコンソールにログインします。
    
        Username  weblogic
        Password  Welcome1
        
    
    ![Weblogicコンソール資格証明ログイン](images/weblogic-credentials.png)
    
3.  Weblogicコンソールで、_「環境」_の下の_「サーバー」_をクリックします。「サーバーのサマリー」で、_「制御」_をクリックします。
    
    ![「環境」の下の「サーバー」をクリックします。](images/weblogic-server.png)
    
    SOAおよびOIMサーバーを選択し、_「起動」_をクリックします。
    
    ![「Control」をクリックして起動します。](images/control-server.png) ![「実行中」ステータスのサーバー](images/running-server.png)
    
4.  別のブラウザ・タブを開き、次のURLを使用して_OIGアイデンティティ・コンソール_にアクセスします。次の資格証明を使用してアイデンティティ・コンソールにログインします。
    
        URL       http://oiri.livelabs.oraclevcn.com:14000/identity
        Username  xelsysadm
        Password  Welcome1
        
    
    ![OIGアイデンティティ・コンソール・ホームページ](images/oig.png)
    
    ![OIGアイデンティティ・コンソール資格証明ログイン](images/oig-credentials.png)
    
5.  右上隅にある_「管理」_をクリックします。次に、_「ユーザー」_をクリックし、約1000人のテスト・ユーザーがそれぞれのロールおよび権限で作成されていることを確認します。任意のユーザーをクリックし、_「アカウント」_タブをクリックして、ユーザーが_「ドキュメント・アクセス」_アプリケーションにプロビジョニングされていることを確認します。
    
    ![「OIGアイデンティティ管理」タブ](images/users-oig.png)
    
    ![「OIG IDユーザー」タブ](images/display-users-oig.png)
    
    ![OIGアイデンティティ・ユーザーの「アカウント詳細」タブ](images/user-details-oig.png)
    
6.  次に、_「ホーム」_をクリックします。次に、_「ロールおよびアクセス・ポリシー」_をクリックし、_「ロール」_を選択します。ユーザーがOIRIアプリケーションにログインできるように、_OrclOIRIRoleEngineer_ロールが作成され、アプリケーション・ユーザーに割り当てられていることに注意してください。_OrclOIRIRoleEngineer_ロールをクリックします。「_Members_」タブをクリックし、この役割が _xelsysadm_ユーザーに割り当てられていることを確認します。
    
    ![OIGアイデンティティ・ロールとアクセス・ポリシー](images/roles-oig.png)
    
    ![「OIGアイデンティティ・ロール」タブ](images/display-roles-oig.png)
    
    ![OIGアイデンティティ・ロールの「メンバー詳細」タブ](images/members-role-oig.png)
    

[次の演習に進む](#next)ことができます。

## 謝辞

*   **著者** - NATDソリューション・エンジニアリング、Anuj Tripathi、Brijith TG、Keerti R氏
*   **貢献者** - Keerti R、Brijith TG、Anuj Tripathi
*   **最終更新者/日付** - Indiradarshni B、NATDソリューション・エンジニアリング、2022年12月