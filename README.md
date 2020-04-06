# Rails 6 + mysql 標準テンプレート

Rails 6で新規アプリを作れるテンプレートです。データベースを mysql に変更していること以外は、特に細かいことはやっていません。

## 必要なもの

mysqlをインストールしておいてください。サーバーとクライアント両方いります。

rbenv、  nodenv、 direnvの３点セット。必須ではないですが、塩としょうゆと砂糖みたいなもんなので、入れておいて損はないです。

## mysqlのユーザーを作る

$ sudo mysql -u root

でmysqlに入ります。

以下のコマンドを打って、ローカルのテストユーザーを作ります。railsがDBにアクセスするときに、このユーザー情報を使います。
```
create user 適当なユーザー名@'localhost' identified by '適当なパスワード';
grant all on *.* to さっきのユーザー名@'localhost';
```

mysqlの初期設定では、パスワードに大文字小文字数字記号を全部入れる必要があります。あとで環境変数に設定してしまえば打ち直さなくていいものなので、適当にいきましょう。

## 環境変数に情報を設定する

$ direnv edit .

とすると、エディタが立ち上がります。ここに、以下のようにして環境変数の宣言を追加します。direnvを使わない人は、適当に環境変数に突っ込んでください。

export RAILS_DATABASE_USERNAME=さっきのユーザー名

export RAILS_DATABASE_PASSWORD=さっきのパスワード

保存して、閉じます。

config/database.yml を、すでによしなに設定済みなので、RailsがDBを見に行くときに、上記の環境変数名を自動で読みに行くようになっています。

## アプリ名を変える

変えないと、複数のアプリを作ったりしたときにDBが競合してしまうので、必ず変えましょう。

module Basic
module Basic
module Basic
module Basic
config/application.rb の、 module Basic と書いてあるところを、 module 作りたいアプリの名前 に書き換えればいいらしいです。
## rubyとnodeを入れる

このリポジトリのフォルダに入った状態で、

$ rbenv install

$ nodenv install

をします。 ruby 2.6.2 と node 10.16.0 が入ります。違うバージョンでもいいのですが、たまたまこれで動作確認したので、そのまま覚えさせました。

ruby はビルドが行われるので、 gcc や依存ライブラリがないと途中でこけます。足りないものはエラーメッセージに出るはずなので、その都度インストールしましょう。ライブラリを入れながら、こけなくなるまで rbenv install を連発してれば、だいたいなんとかなります（なんとかならないときもあります）。

## 最後の仕上げ

$ gem install bundler

と

$ npm install yarn

をします。

そのご、

bundle install --path=vendor/bundle

で、アプリに必要なgemを拾ってきます。

これも、 native extensions のビルドが挟まるので、 gcc がエラーを出すことがあります。特に、 nokogiri とか、なんとかracerとかね。mysql2でこけるときもあります。ここは頑張るしかないです。

すべてのgemをインストール出来たら、

$ bundle ex rails webpacker:install

として、 webpacker なるものを入れます。(rails6になって追加されたらしいけど、なんで必要なのかよくわかってません。webpackのなにかだと思うけど。フロント？)

最後に、

$ bundle ex rake db:create

をすれば、準備が整います。

## 起動

$ bundle ex rails s -b 0.0.0.0

-bオプションは、仮想マシンの人にとっては必要です。

## You're on rails!

あとは、がんがん作ればいいだけですね！

ちなみに、このテンプレートから複数のアプリを作る場合、同一マシン上であれば、かなりの作業をスキップできます。アプリ名を設定し、 bundle install と rails webpacker:install と rake db:create だけやればよいです。
