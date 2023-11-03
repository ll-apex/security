# 21C環境の設定

## 概要

この演習では、スクリプトを実行して、Oracle Database 21cワークショップの環境を設定します。

見積時間: 15分

### 目標

この演習では、次のことを行います。

*   接続の定義およびテスト
*   21cラボの実行に必要なスクリプトをダウンロード
*   選択したパスワードでスクリプトを更新します

### 前提条件

*   Oracle Account
*   viまたはnanoの作業知識
*   SSHキー
*   DBCS VMデータベースの作成
*   DBCSパブリック・アドレス
*   一意のデータベース名

## タスク1: サーバー設定

1.  まだログインしていない場合は、Oracle Cloudにログインし、Oracle Cloudシェルを再起動するか、ステップ3にスキップします。
    
2.  クラウド・シェルまたはターミナル・ウィンドウで、SSHキーを作成したフォルダに移動し、IPアドレスを使用して次のコマンドを入力します:
    
        $ <copy>ssh -i <<sshkeyname>> opc@</copy>< your_IP_address >
        Enter passphrase for key './myOracleCloudKey':
        Last login: Tue Feb  4 15:21:57 2020 from < your_IP_address >
        [opc@tmdb1 ~]$
        
3.  コンテナ・データベースおよびプラガブル・データベースのディレクトリを作成する必要があります。
    
        <copy>
        sudo mkdir /u02/app/oracle/oradata/CDB21
        sudo chown oracle:oinstall /u02/app/oracle/oradata/CDB21
        sudo chmod 755 /u02/app/oracle/oradata/CDB21
        sudo mkdir /u02/app/oracle/oradata/pdb21
        sudo chown oracle:oinstall /u02/app/oracle/oradata/pdb21
        sudo chmod 755 /u02/app/oracle/oradata/pdb21
        sudo mkdir /u02/app/oracle/oradata/PDB21
        sudo chown oracle:oinstall /u02/app/oracle/oradata/PDB21
        sudo chmod 755 /u02/app/oracle/oradata/PDB21
        sudo mkdir /u02/app/oracle/oradata/toys_root
        sudo chown oracle:oinstall /u02/app/oracle/oradata/toys_root
        sudo chmod 755 /u02/app/oracle/oradata/toys_root
        sudo mkdir /u03/app/oracle/fast_recovery_area
        sudo chown oracle:oinstall /u03/app/oracle/fast_recovery_area
        sudo chmod 755 /u03/app/oracle/fast_recovery_area
        </copy>
        
4.  接続後、「oracle」OSユーザーに切り替えることができます。
    
        [opc@tmdb1 ~]$ <copy>sudo su - oracle</copy>
        
5.  最初のステップは、この演習に必要なすべてのスクリプトを取得することです。
    
        <copy>
        cd /home/oracle
        wget https://objectstorage.us-ashburn-1.oraclecloud.com/p/VEKec7t0mGwBkJX92Jn0nMptuXIlEpJ5XJA-A6C9PymRgY2LhKbjWqHeB5rVBbaV/n/c4u04/b/livelabsfiles/o/data-management-library-files/Cloud_21c_Labs.zip
        </copy>
        
6.  Cloud\_21c\_Labs.zipの解凍
    
        <copy>
        unzip Cloud_21c_Labs.zip
        </copy>
        
7.  スクリプトをすべてのユーザーが読取り、書込みおよび実行できるようにします。
    
        <copy>
        chmod 777 /home/oracle/labs/update_pass.sh
        </copy>
        
8.  /home/oracle/labs/update\_pass.shシェル・スクリプトを実行します。シェル・スクリプトにより、password\_defined\_during\_DBSystem\_creationの入力を求められ、演習で使用されるすべてのシェル・スクリプトおよびSQLスクリプトに設定されます。
    
        [oracle@da3 ~]$ <copy>/home/oracle/labs/update_pass.sh</copy>
        Enter the password you set during the DBSystem creation: WElcome123##
        

## タスク2: データベースの作成

1.  次に、`CDB21`データベースを作成します。このサーバーには既存のデータベースがありますが、`CDB21`はこのワークショップに必要なすべてを使用して作成されました。
    
        <copy>
        cd /home/oracle/labs/M104784GC10
        /home/oracle/labs/M104784GC10/create_CDB21.sh
        </copy>
        
2.  環境を設定します。`CDB21`のプロンプト・タイプ
    
        [oracle@db1 ~]$ <copy>. oraenv</copy>
        ORACLE_SID = [CDB21] ? CDB21
        The Oracle base remains unchanged with value /u01/app/oracle
        [oracle@db1 ~]$
        
3.  次のコマンドを使用して、Oracle Database 21cの`CDB21`および`PDB21`が作成されていることを確認します。
    
        <copy>
        ps -ef|grep smon
        sqlplus / as sysdba
        </copy>
        
    
        <copy>
        show pdbs
        exit;
        </copy>
        
4.  tnsnames.oraファイルの`CDB21`、`PDB21`および`PDB21_2`に対してTNS別名が作成されていることを確認します。存在しない場合は、追加する必要があります。ファイルは`/u01/app/oracle/homes/OraDB21Home1/network/admin/tnsnames.ora`にあります。
    
        	  <copy>
        	  cat /u01/app/oracle/homes/OraDB21Home1/network/admin/tnsnames.ora
        	  </copy>
        
5.  次に、データベースの作成時に`CDB21`のエントリが作成されている必要があります。そのため、`PDB21`の場合は、`CDB21`のエントリをコピーし、両方の場所で値`CDB21`を`PDB21`に変更します。これを`PDB21_2`に対して繰り返します。
    
        	  <copy>
        	  vi /u01/app/oracle/homes/OraDB21Home1/network/admin/tnsnames.ora
        	  </copy>
        
6.  tnsnames.oraにはさらに多くのものがありますが、注意すべき3つのエントリは、次のエントリのようになりますが、かわりにホスト名を使用します。
    
        CDB21 =
        (DESCRIPTION =
         (ADDRESS = (PROTOCOL = TCP)(HOST = db1.subnet11241424.vcn11241424.oraclevcn.com)(PORT = 1521))
         (CONNECT_DATA =
          (SERVER = DEDICATED)
          (SERVICE_NAME = CDB21)
         )
        )
        
        PDB21 =
        (DESCRIPTION =
         (ADDRESS = (PROTOCOL = TCP)(HOST = db1.subnet11241424.vcn11241424.oraclevcn.com)(PORT = 1521))
         (CONNECT_DATA =
          (SERVER = DEDICATED)
          (SERVICE_NAME = PDB21)
         )
        )
        
        PDB21_2 =
        (DESCRIPTION =
         (ADDRESS = (PROTOCOL = TCP)(HOST = db1.subnet11241424.vcn11241424.oraclevcn.com)(PORT = 1521))
         (CONNECT_DATA =
          (SERVER = DEDICATED)
          (SERVICE_NAME = PDB21_2)
         )
        )
        
7.  CDB21への接続をテストします。パスワード**WElcome123##**を使用して、SQL\*PlusでCDB21に接続します。
    
        	  <copy>
        	  sqlplus sys@cdb21 AS SYSDBA
        	  </copy>
        
8.  コンテナ名が**CDB$ROOT**であることを確認します。
    
        <copy>
        	  SHOW CON_NAME;
        	  </copy>
        
9.  前のステップで使用したものと同じパスワードを使用して、PDB21への接続をテストします。
    
        <copy>
        CONNECT sys@PDB21 AS SYSDBA
        </copy>
        
10.  コンテナ名を表示します。これで、**PDB21**が表示されます。
    

    ```
    <copy>
    SHOW CON_NAME;
    </copy>
    ```
    

11.  SQL\*Plusを終了します。
    
        <copy>
        exit
        </copy>
        

## 確認

*   **作成者** - Donna Keesling、データベースUAチーム
*   **コントリビュータ** - David Start、Kay Malcolm、Database Product Management
*   **最終更新者/日付** - 2021年12月、データベース製品管理、製品マネージャー、Arabella Yao氏