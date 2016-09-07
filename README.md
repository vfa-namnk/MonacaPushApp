# 【Monaca】アプリにプッシュ通知を組み込もう！

![画像1](/readme-img/001.png)

## 概要
* [ニフティクラウドmobile backend](http://mb.cloud.nifty.com/)の『プッシュ通知』機能を実装したサンプルプロジェクトです
* 簡単な操作ですぐに [ニフティクラウドmobile backend](http://mb.cloud.nifty.com/)の機能を体験いただけます★☆

## ニフティクラウドmobile backendって何？？
スマートフォンアプリのバックエンド機能（プッシュ通知・データストア・会員管理・ファイルストア・SNS連携・位置情報検索・スクリプト）が**開発不要**、しかも基本**無料**(注1)で使えるクラウドサービス！

注1：詳しくは[こちら](http://mb.cloud.nifty.com/price.htm)をご覧ください

![画像2](/readme-img/002.png)

## 動作環境の準備
### 共通
* Monaca会員登録
 * 右記リンクより登録（無料）をお願いします　https://ja.monaca.io/
* ブラウザ環境：ChromeまたはMozilla Firefox

### Android端末で動作確認をする場合
* PC
* Googleアカウント
* Android端末

### iOS端末で動作確認をする場合
* Mac
 * キーチェーンアクセスを利用します
* Apple Developer Program(有償)アカウント
 * 別のMacで使用しているアカウントの場合、発行する証明書に秘密鍵を紐付けることができません。ただし、アカウントを使用しているMacから秘密鍵を書き出して、今回使用するMacに送ることで作業は可能です
* iOS端末
 * Lightningケーブル（端末のUDIDを調べるために必要です）

※このサンプルアプリは、実機ビルドが必要です

## プッシュ通知の仕組み
* ニフティクラウドmobile backendのプッシュ通知は、各プラットフォームが提供している通知サービスを利用しています
 * Androidの通知サービス __FCM（Firebase Cloud Messaging）__

 ![画像a1](/readme-img/a001.png)

 * iOSの通知サービス　__APNs（Apple Push Notification Service）__

 ![画像i1](/readme-img/i001.png)

* 上図のように、アプリ（Monaca）・サーバー（ニフティクラウドmobile backend）・通知サービス（FCMあるいはAPNs）の間で認証が必要になります
 * 認証に必要な鍵や証明書の作成は作業手順の「0.プッシュ通知機能使うための準備」で行います

## 作業の手順
### 0.プッシュ通知機能使うための準備
* 動作確認を行う端末に応じて該当する内容を準備してください

#### Android端末で動作確認をする場合
* ニフティクラウド mobile backendと連携させるためのAPIキー(サーバーキー)と端末情報の登録処理時に必要なSender ID(送信者ID)を取得する必要があります
* 下記リンク先のドキュメントを参考に、FCMプロジェクトの作成とAPIキー・Sender IDの取得を行ってください
 * __[mobile backendとFCMの連携に必要な設定](https://github.com/NIFTYCloud-mbaas/ncmb_doc/blob/hotfix/fcm_changes/doc/current/tutorial/push_setup_android.html)__

#### iOS端末で動作確認をする場合
* Android端末と違い、作業が少し複雑です
 * 作り方を誤ると動作しない可能性があります
 * 丁寧に作業していきましょう！
* 下図の内容を作成していきます

![画像i2](/readme-img/i002.png)

* 作成したファイルはダウンロードし、同じフォルダにまとめておきましょう

* ①CSRファイルを作成します　※初回利用時のみ
 * __注意__：CSRファイルは既に作成したものがあれば、新しく作成しないでください！必ず既存のものを使用します。複数作成してしまうとファイル名が同じであるため区別できなくなり失敗につながる恐れがあります。
* 「キーチェーンアクセス」を開いて、メニューバーの「キーチェーンアクセス」＞「証明書アシスタント」＞をクリックします

![画像i3](/readme-img/i003.png)

* 「鍵ペア情報」を確認して「続ける」をクリックし、「設定結果」が出るので「完了」をクリックします
* 保存場所を選択して「保存」をクリックします

![画像i4](/readme-img/i004.png)

* ここから[Apple Developer Programのメンバーセンター](https://developer.apple.com/account/)にログインして作業を行います

![画像i5](/readme-img/i005.png)

* ②開発用証明書(.cer)を作成します　※初回利用時のみ
 * __注意__：開発用証明書(.cer)ファイルは既に作成したものがあれば、新しく作成しないでください！必ず既存のものを使用します。複数作成してしまうとファイル名が同じであるため区別できなくなり失敗につながる恐れがあります。
* 「Certificates」＞「All」＞右上の「＋」をクリックして、「iOS App Development」にチェックをいれます

![画像i6](/readme-img/i006.png)
![画像i7](/readme-img/i007.png)
![画像i8](/readme-img/i008.png)

* ③AppIDを作成します
* 「Identifiers」＞「App IDs」＞右上の「＋」をクリックします

![画像i9](/readme-img/i009.png)
![画像i10](/readme-img/i010.png)
![画像i11](/readme-img/i011.png)
![画像i12](/readme-img/i012.png)

* ④動作確認で使用する端末の登録をします
 * 既に登録済みの場合、この作業は不要です
* 「Devices」＞「All」＞右上の「＋」をクリックします

![画像i13](/readme-img/i013.png)

* UDIDは下記のいずれかの方法で調べることができます

![画像i14](/readme-img/i014.png)
![画像i15](/readme-img/i015.png)

* 先ほどの入力欄に調べたUDIDをコピーして貼り付け、「Continue」をクリックします

![画像i16](/readme-img/i016.png)

* ⑤プロビジョニングプロファイルを作成します
* 「Provisioning Profiles」＞「All」＞右上の「＋」をクリックします

![画像i17](/readme-img/i017.png)
![画像i18](/readme-img/i018.png)
![画像i19](/readme-img/i019.png)
![画像i20](/readme-img/i020.png)

* ⑥APNs用証明書(.cer)の作成をします
* 「Certificates」＞「All」＞右上の「＋」をクリックします

![画像i21](/readme-img/i021.png)
![画像i22](/readme-img/i022.png)

* CSRファイルは①開発用証明書(.cer)作成時と同じものを使用します

![画像i23](/readme-img/i023.png)
![画像i24](/readme-img/i024.png)

* 証明書などの作成は以上です
* 次に、作成した証明書から必要なp12形式で証明書を書します
 * ⑦開発用証明書(秘密鍵.p12)
 * ⑧APNs用証明書(.p12)

* ⑦開発用証明書(秘密鍵.p12)を書き出します
* ②で作成した（あるいは既存の）「開発用証明書(.cer)」をダブルクリックしてキーチェーンアクセスを開きます

![画像i25](/readme-img/i025.png)

* 選択した証明書を右クリックします

![画像i26](/readme-img/i026.png)

* ⑧APNs用証明書(.p12)を書き出します
* キーチェーンアクセスを開きます
* ⑥で作成した「APNs用証明書(.cer)」をダブルクリックしてキーチェーンアクセスを開きます
* 三角のアイコンをクリックして、開きます
 * 重要：証明書と秘密鍵を別々にする必要があります！

![画像i27](/readme-img/i027.png)
![画像i28](/readme-img/i028.png)

* iOS端末で動作確認をする場合の準備は以上です

* この後使用する証明書やファイルを確認しておきましょう
 * ②開発用証明書(.cer)・⑤プロビジョニングプロファイル・⑦開発用証明書(秘密鍵.p12)・⑧APNs用証明書(.p12)


### 1. [ニフティクラウドmobile backend](http://mb.cloud.nifty.com/)の会員登録とログイン→アプリ作成と設定
* 上記リンクから会員登録（無料）をします。登録ができたらログインをすると下図のように「アプリの新規作成」画面が出るのでアプリを作成します

![画像3](/readme-img/003.png)

* アプリ作成されると下図のような画面になります
* この２種類のAPIキー（アプリケーションキーとクライアントキー）はMonacaで作成するiOSアプリに[ニフティクラウドmobile backend](http://mb.cloud.nifty.com/)を紐付けるために使用します

![画像4](/readme-img/004.png)

* 続けてプッシュ通知の設定を行います

![画像5](/readme-img/005.png)

### 2. [Monaca](https://ja.monaca.io/)の会員登録とログイン→プロジェクトの作成
* 上記リンクから会員登録（無料）をします。登録ができたらログインします

![画像8](/readme-img/008.png)

* プロジェクト名を入力します
* 説明は空欄でOKです
* インポート方法は「URLを指定してインポート」を選択し、この画面([GitHub](https://github.com/natsumo/MonacaPushApp.git))の![画像10](/readme-img/010.png)ボタンをクリックして、さらに![画像16](/readme-img/016.png)ボタンを__右クリック__してURLをコピーしたものを貼り付けてください

![画像9](/readme-img/009.png)

* 作成されたプロジェクトを「開く」をクリックして開きます

![画像6](/readme-img/006.png)

### 3. APIキーの設定

* `index.html`を編集します
* `YOUR_NCMB_APPLICATION_KEY`と`YOUR_NCMB_CLIENT_KEY`の部分を、先程[ニフティクラウドmobile backend](http://mb.cloud.nifty.com/)のダッシュボード上で確認したAPIキーに書き換えます
* また、Android端末で動作確認をする場合は、`YOUR_FCM_SENDER_ID`をFCMでプロジェクト作成時に発行されたSender ID(送信者ID)に書き換えます

![画像07](/readme-img/007.png)

* このとき、ダブルクォーテーション（`"`）を消さないように注意してください！
* 書き換え終わったら「保存」をクリックして保存をします

![画像11](/readme-img/011.png)

### 4. 実機ビルド
* 動作確認を行う端末に応じて該当する作業を行ってくださ

#### Android端末で動作確認をする場合
* 「ビルド」＞「Androidアプリのビルド」をクリックして、Androidアプリケーションのビルドを開きます
* ｢デバッグビルド｣を選択して｢次へ｣をクリックします
* 少し待つとビルドが完了します
* 任意の方法で端末にインストールをしてください

#### iOS端末で動作確認をする場合
* 「設定」＞「iOSビルド設定...」をクリックして、iOSビルド設定を開きます
* ⑦開発用証明書(秘密鍵.p12)と②開発用み証明書(.cer)を設定します

![画像i30](/readme-img/i030.png)
![画像i31](/readme-img/i031.png)

* 「ビルド」＞「iOSアプリのビルド」をクリックして、iOSアプリケーションのビルドを開きます
* ｢デバッグビルド｣を選択して｢次へ｣をクリックします
* 「参照...」をクリックして⑤で作成したプロビジョニングプロファイルを設定し、「次へ」をクリックします

![画像i29](/readme-img/i029.png)

* 少し待つとビルドが完了します
* 任意の方法で端末にインストールをしてください
 * インストール方法は、下記の参考欄に「iOSアプリのインストール方法」として記載していますのでご参照ください

### 5.動作確認
* インストールしたアプリを起動します
 * プッシュ通知の許可を求めるアラートが出たら、必ず許可してください！
* 起動されたらこの時点でAndroid端末はレジスタレーションID、iOS端末はデバイストークンが取得されます
* [ニフティクラウドmobile backend](http://mb.cloud.nifty.com/)のダッシュボードで「データストア」＞「installation」クラスを確認してみましょう！

![画像12](/readme-img/012.png)

* 端末側で起動したアプリは一度閉じておきます

### 6.__プッシュ通知を送りましょう！__
* いよいよです！実際にプッシュ通知を送ってみましょう！
* [ニフティクラウドmobile backend](http://mb.cloud.nifty.com/)のダッシュボードで「プッシュ通知」＞「＋新しいプッシュ通知」をクリックします
* プッシュ通知のフォームが開かれます
* 必要な項目を入力してプッシュ通知を作成します

![画像13](/readme-img/013.png)

* 端末を確認しましょう！
* 少し待つとプッシュ通知が届きます！！！

## 解説
サンプルプロジェクトに実装済みの内容のご紹介

#### SDKのインポートとCordvaプラグインの設定
* SDK・プラグインの更新も同様の作業で行えます

##### SDKのインポート
* Monacaで「設定」＞「JS/CSSコンポーネントの追加と削除...」を開きます
* 下図のようにSDKをインポートできます

![画像14](/readme-img/014.png)

##### Cordvaプラグインの設定
* Monacaで「設定」＞「Cordvaプラグインの管理...」を開きます
* プッシュ通知をアプリに実装する場合は以下のプラグインを有効にします

![画像15](/readme-img/015.png)

#### ロジック
 * `index.html`の`<script></script>`タグ内にデバイストークンを取得し、[ニフティクラウドmobile backend](http://mb.cloud.nifty.com/)に保存するロジックを書いています

```js
document.addEventListener("deviceready", function(){
            // デバイストークンを取得してinstallationに登録する
            window.NCMB.monaca.setDeviceToken(
                "YOUR_NCMB_APPLICATIONKEY",
                "YOUR_NCMB_CLIENTKEY",
                "YOUR_FCM_SENDER_ID"
            );
        },false);
```

* 「`YOUR_NCMB_APPLICATIONKEY`」、「`YOUR_NCMB_CLIENTKEY`」はmobile backendのダッシュボードのアプリケーションキー、クライアントキーにそれぞれ書き換えてください
* Android端末で動作確認を行う場合は、「`YOUR_FCM_SENDER_ID`」をFCMでプロジェクト作成時に発行されたSenderID(送信者ID)に書き換えてください


## 参考
* ニフティクラウドmobile backend のドキュメントもご活用ください
 * [クイックスタート](http://mb.cloud.nifty.com/doc/current/introduction/quickstart_monaca.html)
 * [プッシュ通知](http://mb.cloud.nifty.com/doc/current/push/basic_usage_ios.html)

### iOSアプリのインストール方法
1. iTunesを使う方法
ダウンロードしたプロジェクト.ipaをドラッグ＆ドロップ

1. Xcodeを使う方法
http://docs.monaca.mobi/cur/ja/manual/deploy/non_market_deploy/

1. DeployGateを使う方法
アカウント（無料）を取得し、ダウンロードしたプロジェクト.ipaをドラッグ＆ドロップ
https://deploygate.com/
