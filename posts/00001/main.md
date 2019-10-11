---
Keywords: nmap, cheatsheet
Copyright: (C) 2019 しろくまの雑記帳
---

# Nmapチートシート  

---

* [ターゲッティング](#target)
* [ポートの指定](#port)
* [スキャンモード](#scan)
* [OS、バージョン検知](#os)
* [スキャンタイミングオプション](#timing)


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
