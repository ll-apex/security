# サンプル・スキーマの設定

## 概要

この演習では、後続の演習のデータベース・スキーマの設定方法について説明します。

見積時間: 10分

### 目標

この演習では、サンプル・スキーマを設定します。

*   環境変数の設定
*   データベース・サンプル・スキーマを取得し、解凍します。
*   サンプル・スキーマのインストール

### 前提条件

この演習では、次を想定しています。

*   LiveLabsクラウド・アカウントおよび割り当てられたコンパートメント
*   DB19cコンピュート・インスタンスのIPアドレスおよびインスタンス名
*   LiveLabsアカウントに正常にログインしました
*   有効なSSHキー・ペア

## タスク1: サンプル・データのインストール

このステップでは、Oracle Databaseサンプル・スキーマの選択をインストールします。これらのスキーマの詳細は、この演習の最後にあるスキーマ合意を確認してください。

次の手順を完了すると、サンプル・スキーマ**SH**、**OE**および**HR**がインストールされます。これらのスキーマは、SQL言語の概念およびその他のデータベース機能を示すために、Oracleドキュメントで使用されます。スキーマ自体については、『Oracle Databaseサンプル・スキーマ』の[『Oracle Databaseサンプル・スキーマ』](https://www.oracle.com/pls/topic/lookup?ctx=dblatest&id=COMSC)を参照してください。

1.  _whoami_を実行して、値 _oracle_が返されることを確認します。)
    
    ノート: puttyを使用してWindowsで実行している場合は、セッション・タイムアウトが0より大きいことを確認してください。
    
        <copy>whoami</copy>
        
2.  oracleユーザーでない場合は、再度ログインします。
    
        <copy>
        sudo su - oracle
        </copy>
        
    
    ![ユーザーoracleの置換](./images/sudo-oracle.png " ")
    
3.  Oracleバイナリを指すように環境変数を設定します。SID (Oracle Databaseシステム識別子)の入力を求められたら、**cdb1**と入力します。
    
        <copy>
        . oraenv
        </copy>
        
    
    ![環境変数](./images/oraenv.png " ")
    
4.  Databaseサンプル・スキーマを取得し、解凍します。次に、スクリプトのパスを設定します。
    
        <copy>
        wget https://github.com/oracle/db-sample-schemas/archive/v19c.zip
        unzip v19c.zip
        cd db-sample-schemas-19c
        perl -p -i.bak -e 's#__SUB__CWD__#'$(pwd)'#g' *.sql */*.sql */*.dat
        sed -i 's#COMPRESS,#,#g' sales_history/csh_v3.sql
        </copy>
        
    
    ![スキーマのインストール](./images/install-schema-zip1.png " ") ![スキーマのインストール](./images/install-schema-zip2.png " ")
    
5.  このgrepコマンドを実行して、パスが適切に設定されていることを確認します。このコマンドを実行すると、何も返されません。
    
        <copy>
        grep 'CWD' */*.sql
        </copy>
        
6.  **oracle**ユーザーとしてSQL\*Plusを使用してログインし、TEST\_DATA表領域を作成して、サンプル・スキーマのインストールを準備します。
    
        <copy>
        sqlplus system/Oracle123@localhost:1521/pdb1
        create tablespace TEST_DATA datafile '/u01/oradata/cdb1/pdb1/test_data.dbf' size 300m autoextend on extent management local uniform size 512K;
        </copy>
        
    
    ![sqlplusを起動します。](./images/start-sqlplus-create-tbs-01.png " ")
    
7.  次のスクリプトを実行して、サンプル・スキーマをインストールします。
    
        <copy>
        @/home/oracle/DBSecLab/db-sample-schemas-19c/sales_history/sh_main Oracle123 test_data temp Oracle123 /home/oracle/DBSecLab/db-sample-schemas-19c/sales_history/ /home/oracle/ v3 localhost:1521/pdb1
        create index sh.sales_cust_channel_promo_idx on sh.sales(cust_id, channel_id, promo_id);
        </copy>
        
    
    ![スキーマ・インストール](./images/sales_history_creation.png " ")
    
        <copy>
        col table_name for a30
        set line 2000 pages 2000
        select table_name, num_rows from user_tables order by table_name;
        </copy>
        
    
    ![スキーマがインストールされました](./images/tables-created.png " ")
    
8.  oracleユーザーにSQL Plusを終了します。
    
        <copy>
        exit
        </copy>
        
    
    ![opcに戻る](images/return-to-opc.png)
    
9.  サンプル・データ・データファイルのgzipバックアップの作成(ストレージ圧縮シミュレーション)
    
    **ノート**: 実際の数値は、スクリーンショットに示されている数と若干異なる場合がありますが、これは圧縮が発生したことを示しています。それがあなたのために示されている限り、あなたは研究室を通り抜けるのが良いです。
    
        <copy>
        du -hs /u01/oradata/cdb1/pdb1/test_data.dbf
        cp /u01/oradata/cdb1/pdb1/test_data.dbf /u01/oradata/test_data.dbf
        </copy>
        
    
        <copy>
        gzip /u01/oradata/test_data.dbf
        du -hs /u01/oradata/test_data.dbf.gz
        </copy>
        
    
    ![ストレージ圧縮](images/storage-compress-simulation.png)
    
    ご覧のとおり、test\_data.dbfデータファイルをバックアップし、317MBから17MBに圧縮しました。
    

**おめでとうございます!**これで、演習を実行する環境が整いました。

**次の演習に進む**ことができます。

## Oracle Databaseサンプル・スキーマ契約

Copyright (c) 2019 Oracle

このソフトウェアおよび関連ドキュメント・ファイル(以下「本ソフトウェア」といいます)のコピーを取得するすべての人に無償で、制限なくソフトウェアを扱うことを許可します。本ソフトウェアのコピーを使用、コピー、変更、マージ、公開、配布、サブライセンスおよび/または販売する権利、および本ソフトウェアを提供する相手にそれを許可する権利は、次の条件に従います。

上記の著作権表示およびこの許可表示は、このソフトウェアのすべてのコピーまたは大部分に含まれるものとします。

_ソフトウェアは、「現状のまま」提供され、あらゆる種類の保証なし、エクスプレスまたは暗示され、商品性の保証、特定の目的および非侵害のためのフィットネスを含むがこれらに限定されない。IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM、 DAMAGES OR OTHER LIABILITY、 WHETHER IN AN ACTION OF CONTRACT、 TORT OR OTHER、 ARISING FROM、 OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.いかなる場合でも、著者または著作権者は、いかなる請求、損害、その他の責任に対しても責任を負うものとします。_

## 確認

*   **作成者**
    *   主席データベースおよびO&Mソリューション・アーキテクト、Royce Fu氏
    *   北米スペシャリストハブ、ソリューションエンジニア、Noah Galloso氏
*   **協力者**
    *   Richard Evans、Database Security製品管理
    *   Database Advanced Compression製品管理、Gregg Christman氏
*   **最終更新者/日付**
    *   Royce Fu、2023年6月