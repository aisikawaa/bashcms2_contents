---
Keywords: nmap, cheatsheet
Copyright: (C) 2019 しろくまの雑記帳
---

# Nmapチートシート  

---

Nmap奥が深いのでメモ。

各オプションの意味は[Nmap オプションの意味](/?post=00002)を参照してください。

* ページ内リンク
    * [ターゲッティング](#target)
    * [ポートの指定](#port)
    * [スキャンモード](#scan)
    * [OS、バージョン検知](#os)
    * [スキャンタイミングオプション](#timing)
    * [アウトプットフォーマット](#format)
    * [スクリプトオプション](#script)

---

### <span id="target">Nmapターゲッティング</span>  

* 単一のIPをスキャンする  

        nmap 192.168.1.1
 
* ホスト名のスキャン

        nmap www.example.com 

* IPアドレス範囲指定

        nmap 192.168.1.1-100

* CIDR表記を使用してスキャン

        nmap 192.168.1.0/24 

* リストからスキャン

        nmap -lR list.txt 

* ランダムスキャン

        nmap -lR "ターゲット数"
  
---
  
### <span id="port">ポートの指定</span>  

* 単一のポート指定

        nmap -p 22 192.168.1.1 

* ポートを範囲選択

        namp -p 1-20 192.168.1.1 

* 複数のポート指定

        nmap -p 22,80 192.168.1.1

* TCPとUDPポートを複数指定してスキャン

        nmap -p U:53, T:21-25,80 192.168.1.1

* 高速モード - スキャン対象ポートを100箇所まで減らす(デフォルトは1000)

        nmap -F 192.168.1.1

* すべてのポートをスキャン

        nmap -p- 192.168.1.1

---

### <span id="scan">スキャンモード</span>

* Connect スキャン

        nmap -sT 192.168.1.1

* SYN スキャン

        nmap -sS 192.168.1.1

* UDP スキャン

        nmap -sU 192.168.1.1

* No ping スキャン

        namp -Pn 192.168.1.1

* Host Discovery

        nmap -sn 192.168.1.1
        (CIDR表記でネットワーク帯スキャン可)

---

### <span id="os">OS、サービスバージョン検出</span>

* バージョンスキャン

        nmap -sV 192.168.1.1

* バージョンスキャンの検出強度を指定

        namp -sV -version-intensity <0-9> 192.168.1.1

* OS検出

        nmap -o 192.168.1.1

* OS検出、バージョン検出

        nmap -A 192.168.1.1

---

### <span id="timing">タイミングオプション(0から５にかけて順に早くなる)</span>

* 非常に遅い(職人)

        nmap -t0 192.168.1.1

* かなり遅い(ナメクジ)

        nmap -t1 192.168.1.1

* ゆっくり(亀)

        nmap -t2 192.168.1.1

* いつもの

        nmap -t3 192.168.1.1

* せっかち

        nmap -t4 192.168.1.1

* 電撃

        nmap -t5 192.168.1.1

---


### <span id="format">アウトプットフォーマット</span>

* ノーマルフォーマット
      
        nmap -oN 192.168.1.1 normal.file

* XMLフォーマット

        nmap -oX 192.168.1.1 xml.file

* Grepableフォーマット

        nmap -oG 192.168.1.1 grep.file

* 全フォーマット出力

        nmap -oA 192.168.1.1 file_name

---

### <span id="script">NSEスクリプトオプション</span>

* デフォルトスクリプト

        nmap -sC 192.168.1.1
        nmap -sV sC 192.168.1.1

* one script

        nmap --script=<script_name> | <script_category> | <script_dir> 192.168.1.1
        (コンマで区切れば複数のスクリプトを選択できる。)

* ワイルドカードを使う

        nmap --script=<script_name.*> 192.168.1.1

* スクリプトヘルプ

        nmap --script-help=<script_name> | <script_category> 


---

|カテゴリー|説明                                                                                                                            |
|----------|---------|
|auth|ターゲットに対して、資格情報の利用するなどして認証をバイパスする。x11-access、ftp-anonなど。|
|broafcast|ローカルネットワークでブロードキャストすることにより、コマンドラインで指定していないホストを検出する。|
|brute|ブルートフォース攻撃を使用して、対象の認証資格情報を推測する。http-brute、oracle-bruteなど。|
|default|デフォルトで使用されるスクリプト。-sCまたは-Aオプション選択時に使用される。|
|discovery|パブリックレジストリ、SNMP対応デバイス、ディレクトリサービスなどを照会することにより、ネットワークに関する詳細情報を収集するスクリプト。html-title(Webサイトのルートパスを取得)、smb-enum-share(smb共有の列挙)など。|
|dos|まんま。DoS攻撃を喰らわすスクリプト。|
|exploit|ターゲットにエクスプロイトを喰らわす。http-shellshockなど。|
|external|サードパーティのデータベースやネットワークリソースにデータを送信。|
|fuzzer|ファジングを試みる。大抵の場合、処理は重く、帯域幅を大量に消費する。|
|intrusive|ターゲットをクラッシュさせたり、ターゲットのリソースを大量消費させる。|
|malware|ターゲットがマルウェアやバックドアに感染しているかどうかチェックする。|
|safe|intrusiveとは逆に対象へのダメージが少ないスクリプト群。悪影響の有無は無保証。|
|version|バージョン検出の拡張機能。-sVオプションが選択されたときに実行される。|
|vuln|既知の脆弱性をチェックし、見つかったときに結果を表示する。|
