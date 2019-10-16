---
Keywords: docker, ハッキングラボ
Copyright: (C) 2019 しろくまの雑記帳
---

# Dockerを使ってWebアプリのハッキングラボをセットアップしてみた。

---

### イントロ
ipusiron氏の「ハッキング・ラボのつくりかた」の環境構築について、自分なりにアレンジしてみるかってことでねー
(「ハッキング・ラボのつくりかた」は超良書です。まだの方は急いでラボメンになりましょう。)

---

### なぜアレンジするのか？

* 環境構築のバリエーションとして。
* 環境構築にかかる時間を節約できそう。
* Dockerに慣れる。
  

今回はホストに攻撃役を演じてもらい、やられ役をDockerで構築する方向でやってみます。

というわけで、中古のラップトップ(でも何でも良いので)を用意して、そこにKaliとかParrotとか
BlackArchとか好きなディストリビューションをいれておきましょう。
(OSインストールは別ページにて書こうと思います。)

マシンスペックが推奨ぎりぎりでお困りの方も、もしかしたらサクサク実験できるようになるかも。
（このあたりは、ハッキング・ラボシリーズに記載の「ラボの心得」を参考に。)



---

### 環境

* ホストOS
    * ParrotOS 4.7(おすすめです！！)

* マシン
    * Levono Thinkpad T430
    * CPU:i7-3632QM
    * RAM:16GB

### 手順

Dockerはインストール済みとします。

1. DVWAを構築する。

* イメージをプル

        docker pull vulnerbles/web-dvwa

* イメージを起動

        docker run -p 80:80 vulnerbles/web-dvwa

イメージが起動したら、ブラウザを立ち上げ`"localhost/setup.php"`にアクセス。  
データベースの作成/リセットボタンを押す。
データベースが作成されたら、ログインして使用開始。

2. Mutillidaeの構築

DVWAを構築したときと同じく、pullしてrun

* イメージをプル

        docker pull szsecurity/mutillidae

* イメージを起動

        docker run -p 80:80 szsecurity/mutillidae

`"localhost/<port>"`でアクセス。
データベースをリセットして準備完了。

---

こんな調子でいろんな環境が試せます。
以下、イメージリスト

* raeseba/bwapp
* webgoat/webgoat-7.1
* webgoat/webgoat-8.0
* citizentig/nowasp
* bkimminich/juice-shop
* ismisepaul/securityshepherd
* ~~wpscanteam/vulnerbleworpress~~
  l505/vulnerbleworpressで検索すべし。
* opendns/security-ninjas
* eystsen/altoro

適宜`docker search`で検索。
気に入ったイメージはGithubで検索してリポジトリをフォークしておいたほうがいいかも。


