---
title: Firebase Setup 
date: "2019-04-01T00:00:00.000Z"
description: Firebase Setup Guide
order: 200
---

# [Setup](https://firebase.google.com/docs/web/setup)

<https://firebase.google.com/docs/web/setup>

## Point

firebaseの初期化の方法が記載されています。databaseやmessagingなど、使用しないfirebaseの機能は読み込む必要はないです。

手始めに、[Firebase Web Codelab](https://codelabs.developers.google.com/codelabs/firebase-web/#0) を使用して、実際に体験するのがよいと思います。

ホスティングするだけだったり、極めて小規模のサービスの場合は、上記サンプルでも十分かもしれませんが、実際の開発では、[webpack](https://webpack.js.org/), [dotenv](https://www.npmjs.com/package/dotenv)などを使用して、ビルドやローカルウェブサーバーの起動などをするのが一般的だと思います。

プロジェクトは最低限、開発環境・本番環境・テスト環境で分けた方がいいです。

料金プランはとりあえず無料プラン（Spark プラン）で大丈夫です。後から変更できます。ただ、使用できる機能がいくつか制限されるので注意が必要です。

例えば、Cloud FunctionsでGoogle以外のサードパーティAPIを使用するなどして、外部への通信が発生する場合は、アカウントの課金設定と[有料プラン](https://firebase.google.com/pricing/)への変更が必要です。

## FAQ

### Q.1

ドキュメントに従って初期化を進めると、configとして設定する値をHTMLファイルにコピー＆ペーストするようですが、configに設定している設定値が外部公開されてしまうということでしょうか？

#### A.1
はい。外部からアクセスできる環境にデプロイすれば、当然、configに設定している設定値が外部公開されます。

プロジェクトは最低限、開発環境・本番環境・テスト環境で分けた方が良いので、私の場合は、[dotenv](https://www.npmjs.com/package/dotenv)で環境変数からconfigの設定値を読み込んでfirebaseの初期化を行い、[webpack](https://webpack.js.org/)などでビルドやローカルウェブサーバーの起動を行っています。

### Q.2

configに設定している設定値が外部公開されてもセキュリティーとしては問題ないのでしょうか？

#### A.2

[Firebase リリース チェックリスト](https://firebase.google.com/support/guides/launch-checklist)に下記の記載があります。

> 不正使用を防ぐためのドメインのホワイトリストを追加します。
>   
> Google Developer Console のブラウザ API キーとクライアント ID の本番環境ドメインをホワイトリストに登録
> Firebase コンソール パネルの [Authentication] タブにある本番環境ドメインをホワイトリストに登録

デプロイして本番公開する前に、API KEYのアクセス制限を設定してください。

コンソールにログインし、API KEYを公開予定のホストからのアクセスのみに設定（HTTP referrersに公開予定のホストをホワイトリストに登録）するだけです。

[SDK のインポートと暗黙の初期化](https://firebase.google.com/docs/web/setup#sdk_imports_and_implicit_initialization)に記載されている通り、Firebase Hosting を使用してウェブアプリをホストすると、/__/firebase/init.jsにconfigとして設定する値が自動で設定され、外部公開されます。

設定することを忘れないうちに、API KEYのアクセス制限は、プロジェクト作成後すぐに設定しておいた方がよいです。

開発用で使用するプロジェクトの場合は、localhostなどの実際にローカル環境でアクセスするホスト名を追加すれば問題ないです。

併せて、stackoverflowの[How to restrict Firebase data modification?](https://stackoverflow.com/questions/35418143/how-to-restrict-firebase-data-modification)を参照すると理解が深まります。

/__/firebase/init.jsの例については、firebaseの各種デモサイトで確認できます。

例えば、[https://github.com/firebase/friendlypix](https://github.com/firebase/friendlypix)のデモサイトの[/__/firebase/init.jsを確認](https://friendly-pix.com/__/firebase/init.js)してみてください。


### Q.3

請求額を定期的に通知できますか？

#### A.3

[プログラムによる予算通知の例](https://cloud.google.com/billing/docs/how-to/notify)を参照してください。