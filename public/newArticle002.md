---
title: DockerをAPIやSDKで操作できるようにする
tags:
  - Docker
private: false
updated_at: '2024-09-12T23:34:12+09:00'
id: 611f8fa6d6f5a29a8436
organization_url_name: null
slide: false
ignorePublish: false
---
# DockerをAPIやSDKで操作できるようにする

※Dockerのアップデートで設定がデフォルトに戻るため注意してください

https://takuya-1st.hatenablog.jp/entry/2021/05/31/142017



```
sudo vim /lib/systemd/system/docker.service
```

該当サービスの ExecStartに -H tcp://0.0.0.0:2375 を追記する。

```
ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock -H tcp://0.0.0.0:2375
```

systemd をリロードして docker を再起動する

```
sudo systemctl daemon-reload
sudo systemctl restart docker
```

これでTCP経由で接続ができる。
