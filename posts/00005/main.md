---
Keywords:
Copyright: (C) 2019 しろくまの雑記帳
---

# CpawCTFにチャレンジしてみた(Level.1編)

---

### 一問目 Test Probrem ジャンル・Misc

---

問題の指示に従って答えを入力すればOK。

### 二問目 Classical Cipher ジャンル・Crypto

---

問題

暗号には大きく分けて、古典暗号と現代暗号の2種類があります。特に古典暗号では、古代ローマの軍事的指導者ガイウス・ユリウス・カエサル（英語読みでシーザー）が初めて使ったことから、名称がついたシーザー暗号が有名です。これは3文字分アルファベットをずらすという単一換字式暗号の一つです。次の暗号文は、このシーザー暗号を用いて暗号化しました。暗号文を解読してフラグを手にいれましょう。

暗号文: fsdz{Fdhvdu_flskhu_lv_fodvvlfdo_flskhu}

解法

問題文より暗号文を三文字シフトすれば平文が得られる。

### 三問目 Can you execute? ジャンル・Reversing

---

問題

拡張子がないファイルを貰ってこのファイルを実行しろと言われたが、どうしたら実行出来るのだろうか。
この場合、UnixやLinuxのとあるコマンドを使ってファイルの種類を調べて、適切なOSで実行するのが一般的らしいが…

解法

まずは、ファイルをダウンロードして実行する。
MacOSでは動かなかった。
(こういうファイルは実機上ではなく、仮想マシン上で動かしたほうが良い。今回はちょっと横着した。)

fileコマンドで調べると

        exec_me: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, 
        for GNU/Linux 2.6.24, BuildID[sha1]=663a3e0e5a079fddd0de92474688cd6812d3b550, not stripped

と出た。
というわけで、ParrotOSがインストールされた別のマシンで実行してフラグゲット。

