---
Keywords: ctf,cpawctf
Copyright: (C) 2020 しろくまの雑記帳
---

# CpawCTFにチャレンジしてみた（Level.2編）

---

### 一問目 隠されたフラグ ジャンル・Stego

---

#### 問題

以下の画像には、実はフラグが隠されています。
目を凝らして画像を見てみると、すみっこに何かが…!!!!
フラグの形式はcpaw{xxx}です。フラグは小文字にしてください。


#### 解法

問題文が示唆する通り画像の左上と右上にモールス信号が並んでいる。
モールス信号を復号するWebサービスがあるので、それを使ってフラグゲット。

  
### 二問目 Redirect ジャンル・Web

---

#### 問題

このURLにアクセスすると、他のページにリダイレクトされてしまうらしい。
果たしてリダイレクトはどのようにされているのだろうか…

#### 解法

ページにアクセスしてデベロッパーツールのネットワークタブを開き、リロードする。
q9の上にq15が出てくるので、こいつのレスポンスヘッダーを確認する。
フラグゲット。

  
### 三問目 HTTP Traffic ジャンル・Network＋Forensics

---

#### 問題

HTTPはWebページを閲覧する時に使われるネットワークプロトコルである。
ここに、とあるWebページを見た時のパケットキャプチャファイルがある。
このファイルから、見ていたページを復元して欲しい。
http_traffic.pcap

#### 解法

Wiresharkには通信内容からファイルを復元するきのうがあるので、それを使う。

Wiresharkでpcapファイルを開く。
ファイル -> オブジェクトのエクスポート -> HTTPを選択して、すべて保存。
network100(1)がhtmlのソースになっているので開いてみる。
  
「ボタンを押せ」と出ているが、押せないのでとりあえずデベロッパーツールを開く。（view-sourceでも良い）
cssやjs、イメージタグのパスが狂っているので、ソースの指定通りにフォルダーを作ってからページを開くとボタンが出現するので押してフラグゲット。

  
### 四問目 Who am I ? ジャンル・Ricon

---

#### 問題

僕（twitter:@porisuteru）はスペシャルフォース2をプレイしています。
とてもおもしろいゲームです。
このゲームでは、僕は何と言う名前でプレイしているでしょう！
フラグはcpaw{僕のゲームアカウント名}です。

#### 解法

問題文でヒントを提示してくれているので、それをもとに調べてみる。

  
### 五問目 leaf ins forest ジャンル・Forensics

---

#### 問題

このファイルの中にはフラグがあります。探してください。
フラグはすべて小文字です！

file

#### 解法

ファイルを開いて流し読みすると、ところどころ大文字やら記号やらが混じっている。
目grepしても良いが、

        cat misc100 | sed -e 's/[a-z]//g' | sed -e 's/!//g'

などで見やすくしたいところ。

  
### 六問目 Image ジャンル・Misc

### 問題

Find the flag in this zip file.

file

#### 解法

ダウンロードしたファイルを`file`コマンドで調べると`misc100.zip: OpenDocumen`と出るので、OpenofficeかLibreOfficeで開けばOK。


### 七問目 Block Cipher ジャンル・Crypto

---

#### 問題

与えられたC言語のソースコードを読み解いて復号してフラグを手にれましょう。

暗号文：cpaw{ruoYced_ehpigniriks_i_llrg_stae}

crypto100.c

#### 解法

ソースコードを見ると、プログラムの実行には2つの引数が必要である。
1つ目の引数は解読前のフラグ、2つ目の引数はキーとなる任意の数字。
2つ目の引数を1から順に試してもいいが、ソースコードを書き換えたほうがCTFっぽいかなと。

      #include <stdio.h>
      #include <stdlib.h>
      #include <string.h>
      int main(int argc, char* argv[]){
        int i;
        int j;
        int key;
        const char* flag = argv[1];
        for(key = 0; key < 30; ++key) {
        printf("cpaw{");
        for(i = key - 1; i <= strlen(flag); i+=key) for(j = i; j>= i - key + 1; j--) printf("%c", flag[j]);
        printf("}\n");
        }
        return 0;
      }

変数keyの文字列型から数値型への変換を削除。
3つのprintfをfor文で包む。(反復回数は暗号文の文字数分くらいで。)
最後のprintfに改行を入れておくと出力結果が見やすい。

### 八問目 reversing easy! ジャンル・Reversing

---

#### 問題

フラグを出す実行ファイルがあるのだが、プログラム（elfファイル）作成者が出力する関数を書き忘れてしまったらしい…
reverse100

### 解法

objdumpやstringコマンドでファイルを覗く。
or
radareでファイルを開く
`aa`コマンドで解析モードにして、`afl`コマンドでバイナリファイルの関数を表示させる。
main関数が見つかるので`s main`でジャンプ。
`pdc`デコンパイルした結果を表示させるとフラグ読める。

### 九問目 Baby's SQLi −Stage 1− ジャンル・Web

---

### 問題

困ったな……どうしよう……．
ぱろっく先生がキャッシュカードをなくしてしまったショックからデータベースに逃げ込んでしまったんだ．
うーん，君SQL書くのうまそうだね．ちょっと僕もWeb問作らなきゃいけないから，連れ戻すのを任せてもいいかな？
多分，ぱろっく先生はそこまで深いところまで逃げ込んで居ないと思うんだけども……．

とりあえず，逃げ込んだ先は以下のURLだよ．
一応，報酬としてフラグを用意してるからよろしくね！

https://ctf.spica.bz/baby_sql/

Caution
・sandbox.spica.bzの80,443番ポート以外への攻撃は絶対にしないようにお願いします．
・あなたが利用しているネットワークへの配慮のためhttpsでの通信を推奨します．

### 解法

```
    select * from palloc_home
```

### 十問目 Can you login? ジャンル・Network

---

### 問題

古くから存在するネットワークプロトコルを使った通信では、セキュリティを意識していなかったこともあり、さまざまな情報が暗号化されていないことが多い。そのため、パケットキャプチャをすることでその情報が簡単に見られてしまう可能性がある。
次のパケットを読んで、FLAGを探せ！
network100.pcap

### 解法

Wiresharkでパケットを見る。
TCPでフィルターを掛けてもコレといった情報は見つからず。
次に、FTPでフィルターを掛ける。
するとユーザー名とパスワードが見つかったので、これを使ってログインしてみる。
ログインして`ls -la`してみるとフラグ感のあるファイルが見つかる。
接続先では`cat`や`vim`といったコマンドが使えなかったので、`get`コマンドで
落として確認。
