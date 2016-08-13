# API Keyの取得

## 証明書のフィンガプリントの取得

まず、Googke Maps API Keyを発行するために、開発で使用しているMacのフィンガプリントの取得をおこないます。

    $  keytool -list -v -keystore ~/.android/debug.keystore

![map0101](img-map02/map0201.png)

![map0102](img-map02/map0202.png)

## Google API Consoleでプロジェクトを作成

https://code.google.com/apis/console/?pli=1

にログインします。

プロジェクト作成をクリックします。

![map0103](img-map02/map0203.png)

プロジェクト名を入力し、Nearby Message APIの利用規約に同意し、[作成]をクリックします。

![map0104](img-map02/map0204.png)

## Google Map APIを有効にする

[APIと認証]-[API]を選択します。項目の中からGoogle Maps Android APIをクリックします。

![map0105](img-map02/map0205.png)

[APIを有効にする]ボタンをクリックします。

![map0106](img-map02/map0206.png)

## APIキーの作成

[APIと認証]-[認証情報]を選択し、認証情報を追加をクリックします。

![map0107](img-map02/map0207.png)

APIキーをクリックします。

![map0108](img-map02/map0208.png)

APIキーの作成に必要なものは、SHA-1 認証フィンガプリントとアプリのパッケージ名です。

アプリのパッケージ名は、ソースコードの一番上に書かれています。
![map0109](img-map02/map0209.png)

パッケージ名と、SHA-1 認証フィンガプリントを入力し、[作成]をクリックします。
![map0110](img-map02/map0210.png)

これでAPIキーの生成が完了です。APIキーをコピーしておきます。
![map0111](img-map02/map0211.png)