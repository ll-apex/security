# コンピュート・インスタンス設定の検証

## 概要

このラボでは、Oracle Cloudで実行されている事前作成されたコンピュート・インスタンスにログインする方法について説明します。

_推定ラボ時間_: 10分

ラボのクイック・ウォークスルーについては、次のビデオをご覧ください。[コンピュート・インスタンス設定の検証](videohub:1_x7kr6rj0)

### 目標

この演習では、次のことを行います。

*   インスタンスへの接続に必要な詳細の収集(パブリックIPアドレス)
*   リモート・デスクトップまたはSSHプロトコルを使用してコンピュート・インスタンスに接続する方法を学習します

### 前提条件

この演習では、次を想定しています。

*   **「LiveLabsで実行」**オプションを使用して予約が正常に発行されました
*   予約が正常に実行され、出力に有効なリモート・デスクトップURLが指定されました
*   LiveLabsクラウド・アカウントおよび割り当てられたコンパートメント
*   コンピュート・インスタンスのIPアドレスおよびインスタンス名
*   LiveLabsアカウントに正常にログインしました
*   有効なSSHキー(_SSHターミナル接続のみ_)

## タスク1: グラフィカルリモートデスクトップへのアクセス

このワークショップを簡単に実行できるように、VMインスタンスは、ラップトップまたはワークステーション上の最新のブラウザを使用してアクセス可能なリモートのグラフィカル・デスクトップで事前構成されています。次の手順に従ってログインしてください。

1.  インスタンスがプロビジョニングされたら、**「自分の予約」**にナビゲートし、表示されたリストから送信したリクエストを検索します(これが最初のリクエストの場合、1つのアイテムのみが表示されます)。
    
    ![予約する](images/my-reservations.png "予約する")
    
2.  予約プロビジョニングが完了したら、**「ワークショップの起動」**をクリックします。
    
    ![予約完了](images/my-reservation-completed.png "予約完了")
    
3.  「**ログイン情報の表示**」をクリックし、「**リモートデスクトップ**」リンクをクリックします。
    
    ![ログイン情報](images/login-info.png "ログイン情報")
    
    これにより、1回のクリックでリモート・デスクトップに直接移動できます。
    
    ![noVNCリモートデスクトップの起動](images/novnc-launch-get-started-2.png "noVNCリモートデスクトップの起動 ")
    
    > **ノート:**まれに、次に示すように、ブラウザ・タイプに応じて**「Deceptive Site Ahead」**などのエラーが表示されることがあります。
    
    LiveLabsプロビジョニングに使用されるパブリックIPアドレスは、再利用可能なアドレスのプールから取得され、このエラーは、アドレスが長い間終了したコンピュート・インスタンスによって以前に使用されていたが、適切に保護されておらず、危険にさらされ、フラグが設定されたことが原因です。
    
    **詳細**をクリックし、最後に **この安全でないサイトへ**をクリックすると、無視して続行できます。
    
    ![サイト・エラー・メッセージ](images/novnc-deceptive-site-error.png "サイト・エラー・メッセージ ")
    

## 付録1: SSHキー・ベース認証を使用したホストへのログイン(オプション) - パスの選択

SSH端末によるアクセスはオプションで、必要に応じて次の3つの方法からパスを選択します。ターミナル内で完全に実行できるLiveLabを実行する場合は、Oracle Cloud Shell (付録1A)を選択することをお薦めします。

オプションは次のとおりです。

1.  付録1A: クラウド・シェルを使用した接続_(推奨)_
2.  付録1B: MACまたはWindows CYGWINエミュレータを使用した接続
3.  付録1C: Puttyを使用した接続_(マシンにアプリケーションをインストールする必要があります)_

## 付録1A: クラウド・シェルおよび接続へのキーのアップロード

1.  _**「コンピュート」→「インスタンス」**_に移動して、作成したインスタンスを選択します(正しいコンパートメントを選択していることを確認してください)。
    
2.  Oracle Cloudシェルを起動するには、クラウド・コンソールに移動し、ページの右上にある「クラウド・シェル」アイコンをクリックします。
    
    ![](https://oracle-livelabs.github.io/common/labs/generate-ssh-key-cloud-shell/images/cloudshellopen.png " ")
    
    ![](https://oracle-livelabs.github.io/common/labs/generate-ssh-key-cloud-shell/images/cloudshellsetup.png " ")
    
    ![](https://oracle-livelabs.github.io/common/labs/generate-ssh-key-cloud-shell/images/cloudshell.png " ")
    
3.  クラウド・シェルのハンバーガー・アイコンをクリックし、**「アップロード」**を選択して秘密キーをアップロードします。秘密キーに`.pub`拡張子がないことに注意してください。
    
    ![](https://oracle-livelabs.github.io/common/labs/generate-ssh-key-cloud-shell/images/upload-key.png " ")
    
4.  作成したコンピュート・インスタンスに接続するには、秘密キーをロードする必要があります。これは、`.pub`拡張子を持た_ない_キー・ペアの半分です。マシン上でそのファイルを見つけ、**「アップロード」**をクリックして処理します。
    
    ![](https://oracle-livelabs.github.io/common/labs/generate-ssh-key-cloud-shell/images/upload-key-select.png " ")
    
5.  キー・ファイルがCloud Shellディレクトリにアップロードされるまでお待ちください。 ![](https://oracle-livelabs.github.io/common/labs/generate-ssh-key-cloud-shell/images/upload-key-select-2.png " ")
    
    ![](https://oracle-livelabs.github.io/common/labs/generate-ssh-key-cloud-shell/images/upload-key-select-3.png " ")
    
6.  終了したら、次のコマンドを実行して、SSHキーがアップロードされたかどうかを確認します。.sshディレクトリに移動し、権限を変更します。
    
        <copy>
        ls
        </copy>
        
    
        mkdir ~/.ssh
        mv <<keyname>> ~/.ssh
        chmod 600 ~/.ssh/<privatekeyname>
        ls ~/.ssh
        
    
    ![](https://oracle-livelabs.github.io/common/labs/generate-ssh-key-cloud-shell/images/upload-key-finished.png " ")
    
7.  アップロードしたキー名(秘密キー)を使用してコンピュート・インスタンスへのSecure Shell。
    
        ssh -i ~/.ssh/<sshkeyname> opc@<Your Compute Instance Public IP Address>
        
    
    ![](./images/em-mac-linux-ssh-login.png " ")
    

sshインできない場合は、次のトラブルシューティングのヒントを確認してください。

次の演習に進むことができます。

## 付録1B: MACまたはWindows CYGWINエミュレータによる接続

ワークショップによっては、セキュア・シェル・クライアント(SSH)を介してインスタンスに接続する必要がある場合があります。次の演習でSSH端末を介してタスクを実行するように指示されている場合は、次のオプションを確認し、ニーズに最も適したオプションを選択します。

1.  _**「コンピュート」→「インスタンス」**_に移動し、作成したインスタンスを選択します(正しいコンパートメントを必ず選択してください)。
    
2.  インスタンスのホームページで、インスタンスのパブリックIPアドレスを見つけます。
    
3.  opcユーザーとして端末(MAC)またはcygwinエミュレータを開きます。プロンプトが表示されたら、yesと入力します。
    
        ssh -i ~/.ssh/<sshkeyname> opc@<Your Compute Instance Public IP Address>
        
    
    ![](./images/em-mac-linux-ssh-login.png " ")
    

次の演習に進むことができます。

## 付録1C: Puttyを使用したWindows経由の接続

Windowsでは、SSHクライアントとしてPuTTYを使用できます。PuTTYを使用すると、WindowsユーザーはSSHおよびTelnetを使用してインターネット経由でリモート・システムに接続できます。SSHはPuTTYでサポートされ、セキュア・シェルを提供し、転送前に情報を暗号化します。

1.  PuTTYをダウンロードしてインストールします。[http://www.putty.org](http://www.putty.org)
    
2.  PuTTYプログラムを実行します。コンピュータで、**「すべてのプログラム」→「PuTTY」→「PuTTY」**に移動します。
    
3.  次の情報を選択または入力します。
    
    *   カテゴリ: _セッション_
    *   IPアドレス: _サービス・インスタンスのパブリックIPアドレス_
    *   ポート: _22_
    *   接続タイプ: _SSH_
    
    ![](images/7c9e4d803ae849daa227b6684705964c.jpg " ")
    

### **自動ログインの構成**

1.  カテゴリ・セクションで、**「接続」**、**「データの選択」**の順にクリックします。
    
2.  自動ログイン・ユーザー名を入力します。**「opc」**を入力します。
    
    ![](images/36164be0029033be6d65f883bbf31713.jpg " ")
    

### **秘密キーの追加**

1.  「カテゴリ」セクションで、「認証」を**クリックします**。
    
2.  **クリックして**、VMの公開キーに一致する秘密キー・ファイルを参照して検索します。この秘密キーには、PuTTyが機能するための.ppk拡張子が必要です。
    
3.  .ppk拡張子がない場合は、PuttyGenを使用して秘密キーを.ppk形式に変換する手順については、[付録](#Appendix:TroubleshootingTips)を参照してください。
    
    ![](images/df56bc989ad85f9bfad17ddb6ed6038e.jpg " ")
    

### **すべての設定を保存するには:**

1.  「Category」セクションで、「**Click**」セッションをクリックします。
2.  「保存済セッション」セクションで、セッションに名前を付けます(たとえば、EM13C-ABC)、**「保存」**をクリックします)。

次の演習に進むことができます。

## 付録: トラブルシューティングのヒント

演習中に問題が発生した場合は、次のステップに従って問題を解決します。解決できない場合は、**ヘルプが必要**セクションをスキップして、サポートフォーラムを通じて問題を投稿してください。

### 問題1: インスタンスにログインできません

参加者はインスタンスにログインできません

#### 問題#1を修正するためのヒント

インスタンスにログインできない理由はいくつかあります。ここでは、ワークショップ参加者から見た一般的なものをいくつか紹介します

*   秘密キーの権限がオープンしすぎます- 必ず`chmod 600 ~/.ssh/<yourprivatekeyname>`を使用してファイルをchmodしてください
*   正しくフォーマットされていないSSHキー(修正については上記を参照)
*   ユーザーがMACターミナル、Puttyなどからログインすることを選択し、インスタンスが会社VPNによってブロックされています(VPNをシャットダウンし、Cloud Shellへのアクセスまたは使用を試みます)
*   sshキーに指定された名前が正しくありません(sshkeynameを使用しないでください。指定したキー名を使用してください)
*   opcユーザーの前に@を配置(上の形式を使用して@記号とログインを削除)
*   oracleユーザーであることを確認します(確認するコマンド_whoami_を入力します。入力しない場合は、_sudo su - oracle_と入力してoracleユーザーに切り替えます)。
*   インスタンスが実行中であることを確認します(コマンド_ps -ef | grep oracle_を入力して、oracleプロセスが実行中であるかどうかを確認します)。

### 問題2: ppkキーが必要

参加者はWindowsでPuTTYを実行しており、_ppk_形式のSSH秘密キーが必要です

#### 問題#2を修正するためのヒント

Puttyを使用してサーバーに接続する場合は、SSHキーをPuttyと互換性のある形式に変換する必要があります。キーを必要な.ppk形式に変換するには、PuTTYgenを使用します。

[ダウンロード PuTTYgen](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwigtZLx47DwAhUYKFkFHf99BmAQFjAAegQIAxAD&url=https%3A%2F%2Fwww.puttygen.com%2F&usg=AOvVaw1fagG6hM51oZWfQB_rqn2t)

PuTTYgenを使用してキーを.ppk形式に変換するには、次のステップを実行します。

1.  PuTTYgenを開き、**「変換」**に移動して、**「キーのインポート」**をクリックします。PuTTYgenは、キーをロードするウィンドウを表示します。
2.  **SSH秘密キー**を参照し、ファイルを選択して**「開く」**をクリックします。SSH秘密キーはUsers\[user\_name\].SSHディレクトリにある場合があります。
3.  秘密キーに関連付けられたパスフレーズを入力するか、空白のままにして**「OK」**をクリックします。_キー・フィンガープリントは、ビット数が4096であることを確認します。_
4.  **「File」**に移動し、**「Save private key」**をクリックしてキーを.ppk形式で保存します。

## 謝辞

*   **著者** - NA Technology、LiveLabs Platform Lead、Rene Fontcha氏
*   **最終更新者/日付** - Rene Fontcha、LiveLabs Platform Lead、NA Technology、2021年6月