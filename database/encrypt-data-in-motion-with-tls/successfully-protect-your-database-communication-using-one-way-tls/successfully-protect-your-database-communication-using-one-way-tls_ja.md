# 1方向Transport Layer Security (TLS)を使用してデータベース通信を正常に保護

## 概要

このワークショップでは、Oracle Transport Layer Security (TLS)ネットワーク暗号化の機能について説明します。これにより、移動中のデータを暗号化および保護するためにこの機能を構成する方法を学習できます。

説明: TLSは、動いているデータを暗号化するための業界標準です。TLSは一方向認証または相互双方向認証を提供するため、違反の可能性を最小限に抑えます。

_推定ラボ時間:_ 30分

_この演習でテストしたバージョン:_ Oracle DB 19.17

### 目標

*   1方向TLSを使用してデータベース通信を正常に保護
*   TLSを構成する前にネットワークトラフィックが暗号化されていないことを確認します
*   ルート・ウォレットおよび自己署名ルートCA証明書の作成
*   データベース・サーバー・ウォレットの作成および証明書リクエストの作成
*   ルートCA証明書によるデータベース証明書の署名
*   データベース・ウォレットへのCAルート証明書およびデータベース・サーバー証明書の追加
*   CAルート証明書をクライアント・トラスト・ストアにインポートします(Linux、Windowsのみ)
*   TLSネットワーク暗号化の設定
*   TLSネットワーク暗号化を使用して接続し、トラフィックが暗号化されていることを確認します
*   新しいOSユーザーを作成し、SQLトラフィックを暗号化します。
*   (オプション)暗号化の無効化

### 前提条件

この演習では、次を想定しています。

*   Free Tier、有料またはLiveLabs Oracle Cloudアカウント
*   次を完了しました:
    *   演習: 設定の準備(_無料層_および_有料テナント_のみ)
    *   演習: 環境設定
    *   演習: 環境の初期化

## タスク1: tls.zipファイルをローカル・ディレクトリにダウンロードします。

1.  OSユーザー**oracle**として_DBSec-Lab_ VMでターミナル・セッションを開き、`cd`コマンドを使用してlivelabsディレクトリに移動します。
    
        <copy>cd livelabs</copy>
        
    
    **ノート**: リモート・デスクトップ・セッションを使用している場合は、デスクトップの_ターミナル_・アイコンをダブルクリックしてセッションを起動します。
    
2.  Linuxコマンド'wget'を使用して、ラボのコマンドのバンドル(圧縮)ファイルをダウンロードします。
    
        <copy>wget https://objectstorage.us-ashburn-1.oraclecloud.com/p/AjvzsV9DMydnARvKi9Y5j2sFZ2femVdGi5Ciz9r09V--QIRrorH12rLR3zrmKOeH/n/oradbclouducm/b/dbsec_rich/o/tls.zip</copy>
        
3.  ダウンロードしたtls.zipファイルを解凍します。
    
        <copy>unzip tls.zip</copy>
        
4.  tls zipを削除します。
    
        <copy>rm tls.zip</copy>
        
5.  ディレクトリをtlsに変更します。
    
        <copy>cd tls/</copy>
        
6.  次のコマンドを使用して、tslディレクトリのシェル・スクリプトに対する実行権限を有効にします。
    
        <copy>chmod +x *.sh</copy>
        
7.  ファイルを Unixに変換します。これは必要ないかもしれませんが、ファイルを実行しても害はありません。
    
        <copy>dos2unix *</copy>
        

## タスク2: TLSを構成する前に、ネットワーク・トラフィックが暗号化されていないことを確認します。

1.  次のコマンドを使用して、プラガブル・データベースPDB1にpingを実行します。
    
        <copy>tnsping pdb1</copy>
        
2.  まず、既存のPDB1接続を見て、データが移動中に暗号化されていないことを確認します。
    
        <copy>./tls_is_sess_encrypt.sh pdb1</copy>
        
    
    NETWORK\_PROTOCOLを暗号化して'tcp'を読み取ることはできません
    
3.  これにより、ポート1521のトラフィックをインターセプトし、パケット・キャプチャ(pcap)ファイルを生成できます。
    
        <copy>./tls_tcpdump_traffic.sh pdb1</copy>
        
4.  pcapファイルから読み取り可能なデータを抽出します。出力にメールアドレスが表示されます。
    
        <copy>./tls_tcpdump_extract.sh pdb1</copy>
        

## タスク3: ルート・ウォレットおよび自己署名ルートCA証明書の作成。

1.  このステップでは、スクリプトは`orapki`を使用してウォレットを作成し、自己署名証明書を生成し、クライアント・トラスト・ストアで使用するために信頼できる証明書をエクスポートします。
    
        <copy>./tls_create_rootCA_wallet.sh</copy>
        

## タスク4: データベース・サーバーのウォレットを作成し、証明書リクエストを作成します。

1.  次に、rootCAによって署名されるDBウォレットおよび証明書リクエストを作成します。このステップでは、rootCA信頼できる証明書もDBウォレットにインポートします。
    
        <copy>./tls_create_DB_wallet.sh</copy>
        

## タスク5: ルートCA証明書を使用してデータベース証明書に署名します。

1.  次に、rootCAがDBサーバー・ユーザー証明書に署名します。これにより、証明書の有効性が得られます。パブリック・ルートまたは中間認証局(CA)、組織のルートまたは中間CAによって署名されていない場合、証明書は信頼できない可能性があります。
    
        <copy>./tls_sign_DB_cert.sh</copy>
        

## タスク6: CAルート証明書およびデータベース・サーバー証明書をデータベース・ウォレットに追加します。

1.  署名付きDBユーザー証明書を生成した後、それをDBウォレットにインポートします。このステップでは、DBサーバーのユーザー証明書がリクエストされた証明書からユーザー証明書に変更されることがわかります。
    
        <copy>./tls_import_signed_cert.sh</copy>
        

ノート: 署名付きユーザー証明書をインポートする前に、DBウォレットの出力は次のようになります。

    Requested Certificates: 
    Subject:        CN=dbsec-lab,OU=dbsecdemo,O=LiveLabs,L=Austin,ST=Texas,C=US
    User Certificates:
    Trusted Certificates: 
    Subject:        C=US,CN=ROOT
    

署名付き証明書をインポートすると、DBウォレットの出力は次のようになります。

    Requested Certificates: 
    User Certificates:
    Subject:        CN=dbsec-lab,OU=dbsecdemo,O=LiveLabs,L=Austin,ST=Texas,C=US
    Trusted Certificates: 
    Subject:        C=US,CN=ROOT
    

## タスク7: クライアント・トラスト・ストアへのCAルート証明書のインポート(Linux、Windowsのみ)

1.  署名付きDBサーバー・ユーザー証明書を取得したら、それをDBウォレットのルートの場所にデプロイします。DBは、`WALLET_ROOT`パラメータを使用して、tdeやtlsなどのウォレット関連情報を検索します。このステップでは、署名付き証明書を含むDBウォレットが、PDBの`WALLET_ROOT` tlsディレクトリと、ORACLEソフトウェア・クライアントがウォレット`/etc/ORACLE/WALLETS/<user>`を検索するデフォルト・ディレクトリの両方にコピーされます。この場合、`ORACLE`ユーザーとして`sqlplus`を使用しているため、`/etc/ORACLE/WALLETS/ORACLE`になります。
    
        <copy>./tls_deploy_db_wallet.sh</copy>
        

ノート: WALLET\_ROOT初期化パラメータを設定するには、DBを再起動する必要があります。スクリプトによってDBが自動的に再起動されます。

## タスク8: TLSネットワーク暗号化の構成

1.  pdb1\_tls接続文字列の新しいtnsnames.oraエントリを追加します。これにより、既存のpdb1接続文字列がコピーされ、TCPおよび1521のかわりにTCPSプロトコルおよびポート1522を使用するように変更されます。
    
        <copy>./tls_update_tnsnames_ora.sh</copy>
        
2.  このステップでは、データベースのsqlnet.oraを更新して、SSL\_CLIENT\_AUTHENTICATIONパラメータをfalseとして含めます。falseに設定すると、一方向TLSをクライアントで使用できます。trueに設定する場合は、相互TLS (mTLS)を使用する必要があります。
    
        <copy>./tls_update_sqlnet_ora.sh</copy>
        
3.  このステップでは、Oracle Listenerを停止し、ポート1522のTCPS (TLS)接続で使用できるようにlistener.oraを更新します。Oracle Listenerを起動すると、既存のCDBおよびPDBが動的に登録されます。
    
        <copy>./tls_update_listener_ora.sh</copy>
        
4.  tnspingを使用して、PDB1の暗号化されていないリスナー別名に接続できることを確認します。
    
        <copy>tnsping pdb1</copy>
        
5.  現在、リスナーはポート1522のTCPS (TLS)接続に使用できるため、tnspingを使用して接続を検証できます。
    
        <copy>tnsping pdb1_tls</copy>
        

## タスク9: TLSネットワーク暗号化を使用して接続し、トラフィックが暗号化されていることを確認します。

1.  PDB1への暗号化されていない接続の場合と同様に、pdb1\_tlsのtnsnames別名を使用してステップを再実行します。この接続は、ポート1521ではなくポート1522のPDB1へのTLS暗号化接続になります。
    
        <copy>./tls_is_sess_encrypt.sh pdb1_tls</copy>
        
    
    出力は、かわりに'tcps'と表示されます。
    
2.  ここでも、tcpdumpを使用してsqlplusからSQL問合せを取得します。問合せは画面に表示されますが、パケット・キャプチャ(pcap)ファイルには暗号化されていない電子メールは含まれません。
    
        <copy>./tls_tcpdump_traffic.sh pdb1_tls</copy>
        
3.  pcapファイルからデータを抽出すると、取得したトラフィックが暗号化されているため、電子メール・アドレスが表示されなくなります。
    
        <copy>./tls_tcpdump_extract.sh pdb1_tls</copy>
        
    
    Oracle DatabaseとOracle SQL\*Plusクライアント間の移動中のデータの暗号化に成功しました。
    

## タスク10: 新しいOSユーザーを作成し、SQLトラフィックを暗号化します。

1.  次のステップでは、別のオペレーティング・システム・ユーザーを作成し、新しいユーザーとOracle Database間の接続も暗号化されるようにします。OSユーザー'dba\_dan'を作成します。
    
        <copy>./tls_useradd_dba_dan.sh</copy>
        
2.  Oracle Instant ClientおよびSQL\*Plus RPMをインストールします。ノート: このステップでは、2つのRPMをダウンロードするためにVMにインターネット・アクセスが必要です。
    
        <copy>./tls_install_oracle_ic.sh</copy>
        
3.  Danのホーム・ディレクトリにtns\_adminディレクトリと'dba\_dan'のsqlnet.oraを作成します。
    
        <copy>./tls_dba_dan_sqlnet_ora.sh</copy>
        
4.  Danのホーム・ディレクトリに、tns\_adminディレクトリと'dba\_dan'のtnsnames.oraファイルを作成します。tnsnames.oraファイルには、pdb1\_tls別名のエントリのみが含まれます。
    
        <copy>./tls_dba_dan_tnsnames_ora.sh</copy>
        
5.  Oracle Databaseは自己署名証明書を使用しているため、rootCAから、dba\_danはrootCA信頼できる証明書でウォレットを維持するか、rootCA信頼できる証明書をLinuxの信頼できるルート証明書リストに追加する必要があります。このステップでは、Linuxトラステッド・ルート証明書リストにrootCAトラステッド証明書を追加し、OS上のすべてのユーザーがそれを使用してポート1522のTCPSを介してPDB1に接続できるようにします。エンタープライズ環境では、これは、TLSを使用するOracle Databasesに接続するための内部自己署名証明書を管理するための最も効率的な方法です。Windowsでは、Microsoft管理コンソール(MMC)を使用してこの証明書をインストールします。
    
        <copy>./tls_install_linux_cert.sh</copy>
        

ノート: rootCA.crtがLinuxディレクトリ'/etc/pki/ca-trust/source/anchor'にコピーされ、信頼できる証明書のリストにロードされたことがわかります。この場所は、ご使用の環境で使用するLinuxディストリビューションによって異なる場合があります。

6.  Linuxユーザー'dba\_dan'として接続をテストします。
    
        <copy>sudo su - dba_dan</copy>
        
7.  Oracle Instant Clientは、tns関連のパラメータの場所を知る必要があります。これは、mTLSのかわりにTLSを指定する別名、pdb1\_tlsおよびSSL\_CLIENT\_AUTHENTICATION接続になります。
    
        <copy>export TNS_ADMIN=$HOME/tns_admin</copy>
        
8.  SQL\*Plusを使用して、TCPS暗号化リスナーを使用してDBに接続します。
    
        <copy>sqlplus system/Oracle123@pdb1_tls</copy>
        
9.  ネットワークプロトコルが「tcps」であることを示します。
    
        <copy>SELECT sys_context('USERENV', 'NETWORK_PROTOCOL') as network_protocol FROM dual;</copy>
        

## タスク11: (オプション)暗号化の無効化

1.  SQL\*Plusを終了します。
    
        <copy>exit</copy>
        
2.  DBA Danを終了し、Oracleユーザーに戻ります。
    
        <copy>exit</copy>
        
3.  このステップでは、リスナーのTLS暗号化を無効にし、sqlnet.oraおよびtnsnames.oraファイルからパラメータを削除し、TLSウォレット・ファイルを削除します。
    
        <copy>./tls_disable.sh</copy>
        

## **付録**: 製品について

### **概要**

Oracle Databaseは、ネットワーク間を移動するデータが保護されるように、ネイティブ・データ・ネットワーク暗号化とTLSベースの暗号化の両方を提供します。

![ネットワーク暗号化](./images/nne-concept.png "ネットワーク暗号化")

## さらに学ぶ

技術文書:

*   [Transport Layer Security認証の構成](https://docs.oracle.com/en/database/oracle/oracle-database/19/dbseg/configuring-secure-sockets-layer-authentication.html)

## 確認

*   **著者** - 北米スペシャリスト・ハブ、ソリューション・エンジニア、Stephen Stuart & Alpha Diallo氏
*   **コントリビュータ** - データベース・セキュリティ製品マネージャ、Richard C. Evans氏
*   **最終更新者/日付** - Stephen Stuart & Alpha Diallo、2023年4月