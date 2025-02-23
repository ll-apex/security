# Oracle透過的機密データ保護(TSDP)

## 概要

このワークショップでは、Oracle Transparent Sensitive Data Protection (TSDP)の機能を紹介します。ユーザーは、これらの機能をオンザフライでリダクションすることで機密データへのアクセスを保護するために構成する方法を学ぶことができます。

_推定ラボ時間:_ 15分

_この演習でテストしたバージョン:_ Oracle DB 19.19

### ビデオ・プレビュー

今のところ動画はありません

### 目標

*   機密データに対するTSDPポリシーの作成
*   動的機密データ・リダクションをチェックして、アプリケーション外に露出しないようにします

### 前提条件

この演習では、次を想定しています。

*   Free Tier、有料またはLiveLabs Oracle Cloudアカウント
*   次を完了しました:
    *   演習: 設定の準備(_無料層_および_有料テナント_のみ)
    *   演習: 環境設定
    *   演習: 環境の初期化

### ラボのタイミング(推定)

| ステップ番号 | 機能 | 近似時間 |
| --- | --- | --- |
| 1 | 演習用のTSDP環境の準備 | 5分未満 |
| 2 | TSDPポリシーの作成 | 5分 |
| 3 | TSDP Labs環境のリセット | 5分未満 |

## タスク1: 演習用のTSDP環境の準備

1.  OSユーザー**oracle**として、_DBSec-Lab_ VMでターミナル・セッションを開きます
    
        <copy>sudo su - oracle</copy>
        
    
    **ノート**: リモート・デスクトップ・セッションを使用している場合は、デスクトップの_ターミナル_・アイコンをダブルクリックしてセッションを起動します。
    
2.  scriptsディレクトリへ移動します
    
        <copy>cd $DBSEC_LABS/tsdp</copy>
        
3.  TSDP**管理ユーザー**、TSDP**データ所有者**を作成し、**TSDP演習表**を作成します
    
        <copy>./tsdp_prepare_env.sh</copy>
        
    
    ![TSDP](./images/tsdp-001.png "TSDP管理ユーザーの作成")
    

## タスク2: TSDPポリシーの作成

1.  機密タイプ`CREDIT_CARD_TYPE`の作成
    
        <copy>./tsdp_create_sensitive_type.sh</copy>
        
    
    ![TSDP](./images/tsdp-002.png "機密タイプCREDIT_CARD_TYPEの作成")
    
    **ノート:**
    
    *   機密タイプは、機密として指定するデータのクラスです
    *   ここでは、**すべてのクレジット・カード番号**の`credit_card_type`機密タイプを作成します
2.  保護する機密列を識別します(ここでは、列`CORPORATE_CARD`を使用します)。
    
        <copy>./tsdp_add_sensitive_col.sh</copy>
        
    
    ![TSDP](./images/tsdp-003.png "保護する機密列の識別")
    
    **ノート:**保護する列を識別するには、定義した機密タイプに基づいて、OEM Cloud Controlアプリケーション・データ・モデル(ADM)を使用してこれらの列を識別するか、`DBMS_TSDP_MANAGE.ADD_SENSITIVE_COLUMN`プロシージャを使用できます
    
3.  最初の8文字を"\*"で置換する**部分リダクション**に基づいて、TSDPポリシー"`REDACT_PARTIAL_CC`"を作成します
    
        <copy>./tsdp_create_policy.sh</copy>
        
    
    ![TSDP](./images/tsdp-004.png "TSDPポリシーREDACT_PARTIAL_CCの作成")
    
    **ノート:**次の構成要素を持つ無名ブロックを定義して、ポリシーを作成できます。
    
    *   ポリシーにOracle Data Redactionを使用している場合、部分的なデータ・リダクションなど、使用するデータ・リダクションのタイプの指定
    *   ポリシーにOracle Virtual Private Databaseを使用している場合、使用するVPD設定の指定
    *   ポリシーが有効なときにテストする条件。たとえば、ポリシーを有効にする前に満たす必要がある列のデータ型など
    *   `DBMS_TSDP_PROTECT.ADD_POLICY`プロシージャを使用して、これらの構成要素を結合する名前付きの透過的機密データ保護ポリシー
4.  TSDPポリシー`REDACT_PARTIAL_CC`を、以前に作成した機密タイプ`CREDIT_CARD_TYPE`に関連付けます。
    
        <copy>./tsdp_associate_policy.sh</copy>
        
    
    ![TSDP](./images/tsdp-005.png "TSDPポリシーの関連付け")
    
5.  TSDPポリシーを有効にする**前に**機密データを選択します
    
        <copy>./tsdp_select_data.sh</copy>
        
    
    ![TSDP](./images/tsdp-006.png "TSDPポリシーを有効にする前に機密データを選択してください")
    
    **ノート:**列「`CORPORATE_CARD`」のクレジット・カード番号はクリア・テキストです
    
6.  TSDPポリシー"`REDACT_PARTIAL_CC`"を有効化します
    
        <copy>./tsdp_enable_policy.sh</copy>
        
    
    ![TSDP](./images/tsdp-007.png "TSDPポリシーREDACT_PARTIAL_CCを有効化します")
    
7.  TSDPポリシーを有効化**した後**に機密データを選択します
    
        <copy>./tsdp_select_data.sh</copy>
        
    
    ![TSDP](./images/tsdp-008.png "TSDPポリシーの有効化後に機密データを選択します")
    
    **ノート:**
    
    *   これで、クレジット・カード番号が`****-****-9999-9999`の形式でリダクションされていることがわかります。
    *   ご覧のとおり、TSDPは機密データを**すぐに**リダクションし、**SQL問合せのリブートやリライトは不要です**。

## タスク3: TSDP Labs環境のリセット

1.  TSDPの概念に慣れたら、環境をリセットできます。
    
        <copy>./tsdp_reset_env.sh</copy>
        
    
    ![TSDP](./images/tsdp-009.png "TSDP Labs環境のリセット")
    

次の演習に進むことができます。

## **付録**: 製品について

### **概要**

Transparent Sensitive Data Protection (TSPD)は、機密情報を保持するテーブルカラムを検索および分類するための手段です。

この機能を使用すると、機密データを保持するデータベースの表の列をすばやく検索し、このデータを分類して、指定されたクラスのこのデータ全体を保護するポリシーを作成できます。このタイプの機密データの例には、クレジット カード番号または社会保障番号があります。

TSDPポリシーでは、Oracle Data RedactionまたはOracle Virtual Private Databaseの設定を使用して、これらの表の列の機密データを保護します。TSDPポリシーが保護する表の列レベルで適用され、クレジット・カード情報を含むすべてのNUMBERデータ型の列などの特定の列のデータ型を対象とします。分類するすべてのデータに対して統一されたTSDPポリシーを作成し、コンプライアンス規制の変更に応じて必要に応じてこのポリシーを変更できます。オプションで、他のデータベースで使用するためにTSDPポリシーをエクスポートできます。

TSDPポリシーの利点は非常に大きいです。多数のデータベースがある大規模な組織全体でTSDPポリシーを簡単に作成および適用できます。これにより、監査者は、TSDPポリシーがターゲットとするデータの保護を見積もることができるため、非常に役立ちます。TSDPは、同様のセキュリティ制限を持つ多くのデータが存在する場合があり、このすべてのデータに一貫してポリシーを適用する必要がある政府環境で特に役立ちます。ポリシーは、リダクション、暗号化、アクセスの制御、アクセスの監査、監査証跡でのマスクなどです。TSDPを使用しない場合は、すべてのリダクション・ポリシー、列レベルの暗号化構成および仮想プライベート・データベース・ポリシー列を列ごとに構成する必要があります。

### **透過的機密データ保護(TSDP)を使用する利点**

*   **機密データ保護を1回構成してから、必要に応じてこの保護をデプロイします**。透過的機密データ保護ポリシーを構成して、データのクラス(クレジット・カード列など)を実際にターゲット・データを指定せずに保護する方法を指定できます。つまり、透過的機密データ保護ポリシーを作成する場合、保護する実際のターゲット列への参照を含める必要はありません。透過的機密データ保護ポリシーは、データベース内の機密列のリストと、指定した機密タイプとのポリシーの関連付けに基づいて、これらのターゲット列を検索します。これは、透過的機密データ保護ポリシーを作成した後に、より多くの機密データをデータベースに追加する場合に便利です。ポリシーを作成した後、1つのステップで機密データの保護を有効にできます(たとえば、ソース・データベース全体に基づく保護を有効にします)。新しいデータの機密タイプ、および機密タイプとポリシー・アソシエーションによって、機密データの保護方法が決まります。このようにして、新しい機密データを追加する場合、現在の透過的機密データ保護ポリシーの要件を満たしているかぎり、保護を構成する必要はありません。
    
*   **複数の機密列の保護を管理できます**。適切な属性(IDのソース・データベース、機密タイプ自体、特定のスキーマ、表または列など)に基づいて、複数の機密列の保護を有効または無効にできます。この粒度は、データ・セキュリティに対する高度な制御を提供します。この機能の設計により、これらのコンプライアンス規制のプレビューに該当する大規模なデータ・セットの特定のコンプライアンス・ニーズに基づいてデータ・セキュリティを管理できます。データ・セキュリティは、個々の列ではなく特定のカテゴリに基づいて構成できます。たとえば、クレジット・カード番号または社会保障番号の保護を構成できますが、このデータを含むデータベース内の各列およびすべての列に対して保護を構成する必要はありません。
    
*   **Oracle Enterprise Manager Cloud Controlアプリケーション・データ・モデル(ADM)機能を使用して識別された機密列を保護できます。**Cloud Control ADM機能を使用すると、機密タイプを作成したり、機密列のリストを検出できます。次に、この機密列のリストとそれに対応する機密タイプをデータベースにインポートできます。そこから、この情報を使用して透過的機密データ保護ポリシーを作成および管理できます。
    

## 詳細

技術文書:

*   [Oracle透過的機密データ保護19c](https://docs.oracle.com/en/database/oracle/oracle-database/19/dbseg/using-transparent-sensitive-data-protection.html)

## 謝辞

*   **著者** - Database Security PM、Hakim Loumi氏
*   **貢献者** - Pedro Lopes
*   **最終更新者/日付** - Hakim Loumi、データベース・セキュリティPM - 2023年11月