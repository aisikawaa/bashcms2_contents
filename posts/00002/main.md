---
Keywords: nmap
Copyright: (C) 2020 Atsushi Ishikawa
---

# Nmap オプションの意味

---

公式ドキュメントより
書きかけ。

### スキャンモードオプション

* -sS: SYNスキャン

Stealth scanと呼ばれる。SYNに対してACKが帰ってきたら、RSTを送って3WAY Handshakeを確立させない。
そのため、ターゲットにログが残らないという仕組み。
最近のIDS/IPSはだいたいSYNスキャンに対応しているので隠密性に期待するというよりは、
スキャンが高速であるという点から多用される。
管理者権限でないと使用できないのと、状況によっては誤検知してしまう点に注意。

---

* -sT: Connectスキャン

3WAY Handshakeで接続してスキャンする。
ログが残るかどうかは相手次第。(ログレベルの設定とか設計による。)

---

* -sU: UDPスキャン

NmapはデフォルトでTCPスキャンを行うため、UDPプロトコルを用いたスキャンはこのオプションを指定しなければ使えない。
UDPスキャンは時間がかかる上に、コネクションを確立しないのでopenなのかcloseなのか、そもそもターゲットホストの死活も判別がつかない。
UDPを使うサービスは主にDNS(53)、SNMP(161)、DHCP(53、161/162、67/68)などなど。
全ポートスキャンするのに18時間かかるので-Fオプションとか、--top-ports、--host-timeoutなどでスキャンタスクをコントロールしたほうがよさそうだ。
