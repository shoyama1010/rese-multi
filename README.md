# RESE

飲食店予約サービスアプリケーションです。ユーザー登録無しでも店舗情報の閲覧は可能ですが、登録していただくと来店予約やお気に入り店舗のチェック等の機能をご利用可能になります。

![トップページ](TopPage.png)

## 作成した目的

クライアントより自社で予約サービスを持ちたいとの要望があった為、要望に添った機能を持つ予約サービスを構築するため作成しました。

## アプリケーションURL

### ローカル環境

http://localhost

### AWSを使用した本番環境

http://IPアドレス

## 機能一覧

・アカウント作成機能

・アカウント作成時のメール認証機能

・ログイン及びログアウト機能

・飲食店一覧表示機能

・飲食店詳細表示機能

・店舗一覧ソート機能

・店舗検索機能

・飲食店お気に入り情報追加機能

・飲食店お気に入り情報削除機能

・飲食店予約情報追加機能

・飲食店予約情報更新機能

・飲食店予約情報削除機能

・飲食店レビュー作成機能

・飲食店レビュー閲覧機能

・飲食店口コミ投稿機能

・飲食店口コミ編集機能

・飲食店口コミ削除機能

・飲食店口コミ閲覧機能

・ユーザー情報表示機能

・ユーザーお気に入り情報一覧表示機能

・ユーザー飲食店予約情報一覧表示機能

・決済機能

・管理者及び店舗代表者用管理画面ログイン・ログアウト機能

・店舗代表者アカウント作成機能

・csvファイルインポート機能

・店舗別予約情報表示機能

・予約者に対するお知らせメール送信機能

・予約者に対するリマインダーメール送信機能

・QRコード生成機能

・店舗情報新規作成機能

・店舗情報更新機能

・画像アップロード機能

## 使用技術

・Laravel 8

・nginx 1.21.1

・php 7.4.9

・html

・css

・mysql 8.0.26

## テーブル設計

![テーブル設計書1](ReseTable1.png)

![テーブル設計書2](ReseTable2.png)

![テーブル設計書3](ReseTable3.png)

## ER図

![ER図](rese.drawio.png)

# 環境構築

### 1 Gitファイルをクローンする

git clone git@github.com:basstuba/rese.git

### 2 Dockerコンテナを作成する

docker-compose up -d --build

### 3 Laravelパッケージをインストールする

docker-compose exec php bash

でPHPコンテナにログインし

composer install

### 4 .envファイルを作成する

PHPコンテナにログインした状態で

cp .env.example .env

作成した.envファイルの該当欄を下記のように変更

DB_HOST=mysql

DB_DATABASE=laravel_db

DB_USERNAME=laravel_user

DB_PASSWORD=laravel_pass

MAIL_MAILER=smtp

MAIL_HOST=mail

MAIL_PORT=1025

MAIL_FROM_ADDRESS=tubatest@gmail.com

MAIL_FROM_NAME="${APP_NAME}"

**.envファイルの最後に追加**

STRIPE_KEY=stripeで取得した公開キー

STRIPE_SECRET=stripeで取得したシークレットキー

### 5 テーブルの作成

docker-compose exec php bash

でPHPコンテナにログインし(ログインしたままであれば上記コマンドは実行しなくて良いです。)

php artisan migrate

### 6 ダミーデータ作成

PHPコンテナにログインした状態で

php artisan db:seed

### 7 アプリケーション起動キーの作成

PHPコンテナにログインした状態で

php artisan key:generate

### 8 シンボリックリンクの作成

PHPコンテナにログインした状態で

php artisan storage:link

### 9 cronの設定

phpコンテナに**ログインしていない**状態で

crontab -e

エディタが開いたら

&ast; &ast; &ast; &ast; &ast; cd アプリケーションまでの絶対パス && docker-compose exec php php artisan schedule:run >> /dev/null 2>&1

を登録（例）&ast; &ast; &ast; &ast; &ast; cd /home/username/coachtech/laravel/rese/src && docker-compose exec php php artisan schedule:run >> /dev/null 2>&1

## 各種機能について

### 飲食店お気に入り情報追加機能と飲食店予約情報追加機能

・ログインしていないユーザーが使用しようとするとログインページに遷移する仕様になっています。これはもしログインしないと機能しない仕様にするとユーザーにエラーと勘違いされるかもしれないと思ったのと登録していないユーザーへ登録を促す意味があります。

### ソート機能

・評価が一件も無い店舗が最後尾になっているのを明確にする為'仙人'だけ口コミのダミーデータを用意していません。評価が高い順も低い順も最後尾は'仙人'になります。

### 飲食店レビュー作成機能

・ユーザーがログインしている時のみ飲食店詳細ページにボタンが表示されます。

![飲食店詳細ページ](ShopPage.png)

### 決済機能

・マイページに決済ボタンがあります。

![マイページ](MyPage.png)

### 管理者及び店舗代表者用管理画面ログイン機能

・専用のアカウントでログインすることによってメニュー画面にログイン画面へのリンクが表示されます。

![メニュー画面](Menu.png)

**専用アカウント**

email -> coachtech@coachtech.com

password -> coachtech

### 管理者及び店舗代表者のアカウント

・管理者アカウント1件と店舗代表者アカウント20件を用意してあります。

**管理者アカウント**

email -> admin@admin.com

password -> administrator

**店舗代表者アカウント**

email -> manager-1@manager.com ~ manager-20@manager.com

password -> manager1 ~ manager20

・数字は案件シートの店舗データ一覧の上から順番に振り分けました。

（例）manager-1~のemailとpasswordは仙人の店舗代表者アカウント

**ログイン時はemailとpasswordの入力と役職の選択をしてください。**

### 管理者用管理画面

・店舗別口コミ一覧より各店舗の口コミ一覧を閲覧でき、口コミ一覧ページにて管理者による口コミの削除が可能です。

![管理者用管理画面](AdminPage.png)

・csvファイルのインポートもこちらの画面にて実行可能です。

・csvファイルの作成方法は後述の「その他」の欄に記載しています。

### 店舗代表者用管理画面

・もし担当店舗が設定されていない店舗代表者アカウントでログインした場合は「店舗別予約情報表示機能」「予約者に対するお知らせメール送信機能」「店舗情報更新機能」は使用できません。

![店舗代表者用管理画面1](ManagerPage.png)

・あらかじめ用意したアカウント20件や管理者用管理画面で作成したアカウントでも担当店舗が設定されている場合は使用可能です。

![店舗代表者用管理画面2](ManagerPage2.png)

### 予約者に対するお知らせメール送信機能

・担当店舗予約情報確認ページから使用できます。送信するお客様の氏名を選択し送信してください。

![担当店舗予約情報確認ページ](ManagerReservation.png)

### QRコード

・「お知らせメール」と「リマインダーメール」にQRコードを送付しています。

![お知らせメール](ThanksMail.png)

![リマインダーメール](ReminderMail.png)

## その他

### 1 新規登録時の認証メール、お知らせメール、リマインダーメールは**MailHog**へ送信されるように設定しています。

#### ローカル環境

http://localhost:8025

#### AWSを使用した本番環境

http://IPアドレス:8025

### 2 データベースのテーブルを確認できるphpMyAdminのURLは下記の通りです。

#### ローカル環境

http://localhost:8080

#### AWSを使用した本番環境

http://IPアドレス:8080

### 3 csvファイルの作成方法は下記の通りです。

#### Excelで作成する場合

![Excelシート](CsvSample.png)

1行目は画像の通り各項目の名称を記述し、2行目から各項目の店舗の情報を記述する

メニューバーにある'ファイル'をクリック

'名前を付けて保存'をクリック

保存場所を選択

ファイル名を記入

ファイルの種類は'CSV UTF-8 （コンマ区切り）'を選択

保存をクリックして完了

#### Googleスプレッドシートで作成する場合

作成用の雛形シートをご用意いたしましたので下記URLからご利用ください。

https://docs.google.com/spreadsheets/d/1VevaLxk7BTwrX2AjbK3tuQFct3rcF87Oy6akv0I4BlE/edit?hl=ja&gid=0#gid=0

1行目は各項目の名称となりますので、2行目から各項目の店舗の情報を記述してください。

店舗情報の記述後メニューバーにある'ファイル'をクリック

ダウンロードにカーソルを移動

カンマ区切り形式（.csv）をクリックし、ダウンロードすると完了

#### 店舗の画像について

**既存の店舗と同じ画像を使用する場合はcsvファイルのインポートだけで反映されますが、新規の画像を使用する場合は申し訳ございませんが事前に店舗代表者用管理画面にて画像のアップロードをお願い致します。**

画像URLは既存の店舗と同じ画像であればphpMyAdminからshopsテーブルのimg_url又はimagesテーブルのimage_urlをご確認ください。

新規の画像であればimagesテーブルのimage_urlをご確認ください。

### 4 docker-compose.ymlの設定はlocalhostでの接続設定になっています。

### 5 本番環境は現在稼働していません。