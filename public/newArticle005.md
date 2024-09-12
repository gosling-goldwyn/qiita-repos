---
title: Ubuntuでタイムゾーンを変更する
tags:
  - 'Ubuntu'
  - 'Linux'
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: true
---
# Ubuntuでタイムゾーンを変更する

```sh
$ date
Thu 03 Jun 2021 12:42:13 AM UTC
```
↑ロケールが日本じゃない


```sh
# 一応、バックアップを取ります
$ cp /etc/localtime /etc/localtime.org

# timezoneファイル差し替え
$ ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime

$ date
Thu 03 Jun 2021 09:51:12 AM JST
```
↑コマンド実行後即日本になる

再起動後も反映させたままにするには下記を実施
（`/etc/sysconfig`が無かったのでできなかった）

```sh
# 一応、バックアップを取ります
$ cp /etc/sysconfig/clock /etc/sysconfig/clock.org

# viなどで編集してもよし
$ echo -e 'ZONE="Asia/Tokyo"\nUTC=false' > /etc/sysconfig/clock
```