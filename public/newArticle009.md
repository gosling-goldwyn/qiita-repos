---
title: 映像出力なしのマシンにUbuntuをインストールする
tags:
  - Ubuntu
  - RS232C
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: false
---
## 結論

GRUBのTry or Install Ubuntuで実行されるコマンドに

```bash
console=tty0 console=ttyS0,115200n8
```

することで、シリアルポートに出力をリダイレクトできました！

参考) <https://www.thomas-krenn.com/en/wiki/Installing_Ubuntu_20.04_via_a_serial_console>

## 経緯

- メルカリで購入したミニPCに映像出力端子が付いていなかった
  - Lanner社のNCB-1011というマシンでした

## 試したこと

### 普通にシリアル側でインストールを試してみる

最初に試したのはシリアルからのインストールでした。  
RJ45のシリアルポートだったので、変換器をamazonで購入しteratermで覗いてみると、BIOSを開けることが判明。  
BIOSが開けたのでUSBメモリにUbuntuのインストーラを入れてブートしたところ、GRUBの画面が出てきました！  
が、喜びもつかの間、GRUBでTry or Install Ubuntuを選択すると、何も表示されなくなりました……  
ネットワークに`UBUNTU-SERVER`というマシンが存在したため、インストーラの起動はしているみたいでした。

#### SSHしてみる

ネットワークに存在するのでSSHを試してみましたが、パスワードがランダム生成なので、当たり前ですがつなぐことはできませんでした。

### VGAを後付けする

調べたところ、Lanner社のNCB-1011にはオプションでVGAの映像出力をつけることができるようでした。

![[alt text](https://www.lannerinc.com/phocadownload/user-manuals/network-appliances/NCA-1011_manual_v1.2.pdf)](<images/2025-01-18 183637.png>)
(引用元 <https://www.lannerinc.com/phocadownload/user-manuals/network-appliances/NCA-1011_manual_v1.2.pdf>)

これで行けると思ったら、ピン配列が6x2という配列で、国内のECサイトで見つけることができませんでした。  
そこでamazonに12x1のケーブル(<https://www.amazon.co.jp/dp/B01332457U>)があったため購入してみました。  
圧着端子をむき出しにしてピンをつけてみましたが、結局映像は出なかったため諦めることにしました・・・

### ネットワークブートする

ネットワークブートに対応していないマザーボードだったためできませんでした・・・

### ディスプレイなしでインストールを進めてみる

Vmware workstationで仮想マシンのubuntuインストールと並行してインストールしてみました。  
が、これも上手くいかず。  
おそらくストレージのパーティションなどの設定が仮想マシンと実機で異なるため、キーボードの操作も異なっていたのだと思われます。

### シリアルポートにリダイレクトする方法を考える

いろいろ調べていたら、Debianのインストール時にRS232にリダイレクトする方法がヒットしました。そのページの情報をもとにUbuntuでのやり方を調べたら見事欲しい情報にヒットしました！

```bash
console=tty0 console=ttyS0,115200n8
```

## まとめ

映像端子がなくてもシリアルポートさえあれば、Linuxのインストールは可能であることが分かりました！  
フリマサイトで買ったPCに映像端子が無かった場合は、この方法を試してみてください！
