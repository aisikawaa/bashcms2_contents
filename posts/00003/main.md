---
Keywords: docker, ハッキングラボ
Copyright: (C) 2020 しろくまの雑記帳
---

# Dockerを使ってWebアプリのハッキングラボをセットアップしてみた。

---

### イントロ

* 環境構築のバリエーションとして。
* 環境構築にかかる時間を節約できそう。
* Dockerに慣れる。
  

今回はホストに攻撃役を演じてもらい、やられ役をDockerで構築する方向でやってみる。

というわけで、中古のラップトップ(でも何でも良いので)を用意して、そこにKaliとかParrotとか
BlackArchとか好きなディストリビューションをいれておく。



---

### やったときの環境

* ホストOS
    * ParrotOS 4.7

* マシン
    * Levono Thinkpad T430
    * CPU:i7-3632QM
    * RAM:16GB

### 手順

Dockerはインストール済みとして。

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

こんな調子でいろんな環境が試せる。
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

適宜`"docker search"`で検索。

---

### ペンテスト環境を管理するスクリプト

```
    git clone https://github.com/eystsen/pentestlab.git
    cd pentestlab
    ./pentestlab.sh --help

```
作者に感謝!!

### 参照元

* [Web Application Pentest Lab setup Using Docker](https://www.hackingarticles.in/web-application-pentest-lab-setup-using-docker/)@hackingarticles.in
